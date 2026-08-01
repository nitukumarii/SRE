**What is the difference between an Elastic IP and a Public IP address?**


| Feature         | **Public IP Address**                                                 | **Elastic IP Address (EIP)**                                                                               |
| --------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Definition**  | Public IPv4 address automatically assigned to an EC2 instance         | Static public IPv4 address allocated to your AWS account                                                   |
| **Persistence** | Changes when the instance is stopped and started (unless it's an EIP) | Remains the same until you release it                                                                      |
| **Assignment**  | Automatically assigned by AWS                                         | Manually allocated and associated with an EC2 instance or network interface                                |
| **Flexibility** | Cannot be moved between instances                                     | Can be reassigned to another instance within minutes                                                       |
| **Cost**        | No separate charge while in use                                       | Free when attached to a running instance; charged if allocated but not in use or if multiple EIPs are used |
| **Use Case**    | Temporary internet access                                             | Applications requiring a fixed public IP, DNS mapping, or quick failover                                   |



**Q: When would you use an Elastic IP?**

Answer:

I would use an Elastic IP when an application requires a fixed public IP address, such as a web server, firewall, VPN server, or when DNS records point to a specific IP. It's also useful for disaster recovery because it can be quickly reassigned to another EC2 instance if the original instance fails.

**Q: Why doesn't AWS encourage excessive use of Elastic IPs?**

Answer:

IPv4 addresses are a limited resource. AWS charges for Elastic IPs that are not associated with a running instance or when multiple EIPs are allocated unnecessarily. This encourages customers to use them only when required and to adopt alternatives like Load Balancers or IPv6 where appropriate.
