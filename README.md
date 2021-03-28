# CVE 2020-5902

![alt text](https://www.zdnet.com/a/hub/i/2020/07/03/b6c96e0e-7da9-461a-adff-d6009723189a/f5-networks.jpg "F5 Networks Logo")

_Presented by: Tharmiga Loganathan, Manavjot Singh, Mili Choksi_

**Vulnerability Highlights:**

CVSS 3.x: 9.8 (Critical)

CVSS 2.0: 10.0 (High)

_The CVE 2020-5902 vulnerability impacted F5 Network's suite of load-balancing software products called BIG-IP last July. It is a code injection attack that can give hackers root level privileges to vulnerable systems! According to Shodan, there were 8,000 exposed devices at one point with an estimated 2,000 of those devices actually vulnerable to this exploit. This exploit is particularly impactful because it can also impact any device that is behind a BIG-IP load balancer._

> **What is Shodan?**
> At face value, Shodan is the Google of devices connected to the Internet. However, in more negative use-cases, it can be used streamline the process of searching for vulnerable devices.

**Outline:**

* Intro, scale
* What is BIGIP - load balancer
* The exploit
* Create alias explanation
* How to check if you are vulnerable
* Mitigation
* Could still be impacted, use resource integrity!... explain
* So this is a story on why it's important to secure your permissions, and stay up to date on big vulnerabilities that come out!
  

### What is BIG-IP? Impact

BIG-IP is a family of software products from F5 Networks. These products offer an wide variety of different application access and security functions.

Load balancer, virtual IP address on behalf of devices behind it
Traffic management, high availability, acccess control, secuirty, optimization
Both hardward and software solutions
[used to manage high traffic applications, load balancing]
at the very basics: a load balancer, but also...
  firewall, tls inspection, offload, authentication
  
Even if you aren't running an F% BIGIP device, if you have some sort of javascript library that is linking to some website that is behind and F5 device, you could be vulnerable as well
  Solution: subresource integrity, its a hash?

### CVE Timeline

[content]
[How many affected]


### The Exploit

VLANs and Self IP Configuration "self ip"
  Static self-ip - health monitoring ,single machine
  Floating - 
     S
TMUI/Configuration Utility
  "exposed under default configurations"
  Vulnerable URL:
  ```
  https://[hostname]/tmui/login.jsp
  ```
  How to exploit?
  code injection
  * execute commands: tmsh.jsp
  * read files:
  * write files:
  How to get root access from this?
  create alias, delete alias, link list to bash
    Now can use full terminal/bash
    What is alias?
    Create alias command
    
  1. create alias
  2. create file
  3. execute file
  Code to check if you are vulnerable


### Mitigation

Check if you are vulnerable: code 
Check logs to see if you have been exploited
Never expose admin interface to the public
  F5 has said multiple times not to do this
  But if you did anyway, you are vulnerable

• update software
• ensure mgmt port is not publically accessible
• lock down self ip ports "allow none"
• var/log to see if it has been breached

### What now?

[content]

### Resources

https://www.tenable.com/blog/cve-2020-5902-critical-vulnerability-in-f5-big-ip-traffic-management-user-interface-tmui
BIG IP
https://www.youtube.com/watch?v=D6J_j7HdkV8&ab_channel=F5DevCentral 
Self IP
https://www.youtube.com/watch?v=ZeDP6D7fEwM&ab_channel=SteveLyons
The basics of the exploit
https://www.youtube.com/watch?v=hS8ptpsmgJE&ab_channel=F5DevCentral
the exploit - SANS
https://www.youtube.com/watch?v=NmZFwE537Zg&ab_channel=SANSInstitute

