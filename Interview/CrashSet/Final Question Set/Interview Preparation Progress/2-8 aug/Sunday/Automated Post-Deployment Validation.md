# Python Automation Example (SRE Interview)

## Scenario: Automated Post-Deployment Validation

### Situation

In our deployment process, after every deployment to the Kubernetes environment, engineers manually verified whether the deployment was successful. This involved checking multiple tools such as Kubernetes, application health endpoints, monitoring dashboards, and logs. The process was repetitive, time-consuming, and prone to human error.

---

## Task

I wanted to automate the post-deployment validation process so that engineers could quickly determine whether the deployment was healthy before handing it over to QA or production support.

---

## Solution

I developed a Python automation script that performed multiple validation checks automatically.

### The script performed the following tasks:

- Connected to the Kubernetes cluster using `kubectl`
- Verified that all pods were in the **Running** and **Ready** state
- Checked Kubernetes deployment rollout status
- Validated application health endpoints using HTTP requests
- Queried Dynatrace APIs to verify that there was no sudden increase in response time or error rate after deployment
- Checked recent application logs for deployment failures
- Generated a consolidated health report showing PASS/FAIL for each validation

---

## Python Libraries Used

```python
import subprocess
import requests
import json
import logging
from datetime import datetime
```

---

## Sample Functions

### Check Kubernetes Pods

```python
subprocess.run(["kubectl", "get", "pods", "-n", "production"])
```

### Verify Rollout Status

```python
subprocess.run([
    "kubectl",
    "rollout",
    "status",
    "deployment/payment-service"
])
```

### Validate Health Endpoint

```python
response = requests.get(
    "https://service.company.com/health",
    timeout=5
)

if response.status_code == 200:
    print("Application Healthy")
```

### Logging

```python
logging.info("Deployment validation completed successfully")
```

---

## Validation Flow

```
Deployment Completed
        │
        ▼
Check Kubernetes Pods
        │
        ▼
Verify Rollout Status
        │
        ▼
Check Health Endpoint
        │
        ▼
Review Dynatrace Metrics
        │
        ▼
Review Application Logs
        │
        ▼
Generate Validation Report
```

---

## Result

- Reduced manual deployment validation time by approximately **20–30 minutes**
- Standardized post-deployment verification across environments
- Enabled engineers to identify deployment issues much faster
- Reduced the risk of missing important validation checks
- Improved confidence before releasing the application to users

---

# Possible Interview Questions

## Why did you use Python instead of Bash?

**Answer**

I use Bash for simple Linux administration tasks such as restarting services, checking disk usage, and scheduling cron jobs. For this automation, Python was a better choice because it made it easier to interact with REST APIs, parse JSON responses, generate structured reports, and organize the code into reusable functions. It is also easier to maintain as the automation grows.

---

## Which Python libraries did you use?

- `subprocess` – Execute Kubernetes commands
- `requests` – Call application health endpoints and REST APIs
- `json` – Parse API responses
- `logging` – Generate execution logs
- `datetime` – Timestamp reports

---

## Why automate this process?

Manual deployment validation involved checking multiple tools and dashboards, which was repetitive and time-consuming. Automation reduced manual effort, ensured every deployment followed the same validation process, and helped identify issues much more quickly.

---

## What would you improve in the future?

In the future, I would integrate the script directly into the Jenkins CI/CD pipeline so that deployment validation runs automatically after every deployment. I would also send the validation report to Slack or Microsoft Teams and generate HTML or PDF reports for easier visibility.

---

# Key SRE Skills Demonstrated

- Python Automation
- Kubernetes (EKS)
- CI/CD Validation
- REST API Integration
- Monitoring & Observability
- Deployment Verification
- Incident Prevention
- Operational Efficiency
