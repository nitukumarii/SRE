**How would you patch 500 servers?**



To patch 500 servers, I would use Ansible to automate the process in a controlled and phased manner instead of patching all servers simultaneously. First, I'd verify the inventory and ensure backups or snapshots are available if required. I'd then perform a dry run using --check to validate the playbook. Next, I'd patch a small batch of servers (for example, 10–20) to verify there are no issues. Once validated, I'd perform a rolling deployment using the serial keyword to patch servers in batches, minimizing downtime. After each batch, I'd verify service health before proceeding to the next batch. Finally, I'd review the playbook output, confirm all servers were successfully patched, and generate a deployment report.



**Typical Process**

Verify inventory and target hosts.

Take backups/snapshots (if required).

Perform a dry run (--check).

Patch a small pilot group.

Patch servers in batches using serial.

Validate application/service health after each batch.

Roll back if any issues are detected.

Review logs and confirm successful completion.
