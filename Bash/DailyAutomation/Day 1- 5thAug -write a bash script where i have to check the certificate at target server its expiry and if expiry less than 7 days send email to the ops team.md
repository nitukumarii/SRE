#!/usr/bin/env bash
#
# check_cert_expiry.sh
#
# Checks the SSL/TLS certificate expiry of a target server/port.
# If the certificate expires in less than a configurable threshold
# (default 7 days), sends an alert email to the ops team.
#
# Usage:
#   ./check_cert_expiry.sh -H example.com [-p 443] [-d 7] [-t ops@example.com] [-f alerts@example.com]
#
# Requirements: openssl, mail (mailutils/bsd-mailx) or sendmail
#
# Exit codes:
#   0 - OK, cert valid beyond threshold (or alert sent successfully)
#   1 - Usage / input error
#   2 - Could not retrieve certificate
#   3 - Cert expiring soon and alert email failed to send

set -euo pipefail

# ---------- Defaults ----------
HOST=""
PORT=443
THRESHOLD_DAYS=7
TO_EMAIL="ops-team@example.com"
FROM_EMAIL="cert-monitor@example.com"
SUBJECT_PREFIX="[Cert Expiry Alert]"
TIMEOUT=10

usage() {
    cat <<EOF
Usage: $0 -H <host> [-p <port>] [-d <days_threshold>] [-t <to_email>] [-f <from_email>]

  -H  Target hostname (required)
  -p  Port (default: 443)
  -d  Alert threshold in days (default: 7)
  -t  Ops team email address (default: ${TO_EMAIL})
  -f  From email address (default: ${FROM_EMAIL})
  -h  Show this help
EOF
    exit 1
}

# ---------- Parse args ----------
while getopts "H:p:d:t:f:h" opt; do
    case "$opt" in
        H) HOST="$OPTARG" ;;
        p) PORT="$OPTARG" ;;
        d) THRESHOLD_DAYS="$OPTARG" ;;
        t) TO_EMAIL="$OPTARG" ;;
        f) FROM_EMAIL="$OPTARG" ;;
        h) usage ;;
        *) usage ;;
    esac
done

if [[ -z "$HOST" ]]; then
    echo "Error: target host is required (-H)" >&2
    usage
fi

if ! command -v openssl >/dev/null 2>&1; then
    echo "Error: openssl is not installed." >&2
    exit 1
fi

# Pick a mail sender: prefer 'mail', fall back to 'sendmail'
MAILER=""
if command -v mail >/dev/null 2>&1; then
    MAILER="mail"
elif command -v sendmail >/dev/null 2>&1; then
    MAILER="sendmail"
else
    echo "Warning: neither 'mail' nor 'sendmail' found. Alerts cannot be emailed." >&2
fi

# ---------- Retrieve certificate ----------
echo "Checking certificate for ${HOST}:${PORT} ..."

CERT_INFO=$(echo | timeout "$TIMEOUT" openssl s_client -servername "$HOST" -connect "${HOST}:${PORT}" 2>/dev/null \
    | openssl x509 -noout -enddate 2>/dev/null || true)

if [[ -z "$CERT_INFO" ]]; then
    echo "Error: Could not retrieve certificate from ${HOST}:${PORT}" >&2
    exit 2
fi

# CERT_INFO looks like: notAfter=Aug  5 12:00:00 2026 GMT
EXPIRY_DATE_STR="${CERT_INFO#notAfter=}"

# ---------- Compute days remaining ----------
EXPIRY_EPOCH=$(date -d "$EXPIRY_DATE_STR" +%s 2>/dev/null || true)
NOW_EPOCH=$(date +%s)

if [[ -z "$EXPIRY_EPOCH" ]]; then
    echo "Error: Could not parse expiry date: $EXPIRY_DATE_STR" >&2
    exit 2
fi

DAYS_LEFT=$(( (EXPIRY_EPOCH - NOW_EPOCH) / 86400 ))

echo "Certificate for ${HOST} expires on: ${EXPIRY_DATE_STR}"
echo "Days remaining: ${DAYS_LEFT}"

# ---------- Alert if needed ----------
if (( DAYS_LEFT < THRESHOLD_DAYS )); then
    echo "ALERT: Certificate expires in ${DAYS_LEFT} day(s), which is below the ${THRESHOLD_DAYS}-day threshold."

    SUBJECT="${SUBJECT_PREFIX} ${HOST} certificate expires in ${DAYS_LEFT} day(s)"
    BODY=$(cat <<EOF
Certificate expiry alert.

Host:            ${HOST}
Port:             ${PORT}
Expiry date:      ${EXPIRY_DATE_STR}
Days remaining:   ${DAYS_LEFT}
Threshold:        ${THRESHOLD_DAYS} day(s)

Please renew this certificate as soon as possible.

-- Automated cert monitor
EOF
)

    if [[ "$MAILER" == "mail" ]]; then
        if echo "$BODY" | mail -s "$SUBJECT" -r "$FROM_EMAIL" "$TO_EMAIL"; then
            echo "Alert email sent to ${TO_EMAIL}"
        else
            echo "Error: failed to send alert email" >&2
            exit 3
        fi
    elif [[ "$MAILER" == "sendmail" ]]; then
        {
            echo "From: ${FROM_EMAIL}"
            echo "To: ${TO_EMAIL}"
            echo "Subject: ${SUBJECT}"
            echo ""
            echo "$BODY"
        } | sendmail -t && echo "Alert email sent to ${TO_EMAIL}" || { echo "Error: failed to send alert email" >&2; exit 3; }
    else
        echo "Error: no mailer available, could not send alert." >&2
        exit 3
    fi
else
    echo "OK: Certificate is valid for more than ${THRESHOLD_DAYS} day(s). No action needed."
fi

exit 0
