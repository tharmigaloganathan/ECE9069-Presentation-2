# CVE 2020-5902

CVSS 3.x: 9.8 (Critical)

CVSS 2.0: 10.0 (High)

*This vulnerability was exposed in the BIG-IP program last year. It is a version of buffer overflow that can give the hacker root level privileges. What's unique about this progrgam is that it's used by many companies internally to ...*
Impactful becasue it can attack any application behind an LTM
8,000 exposed devices according to Shodan
  What is shodan?
  2000 actually vulnerable
  

### What is BIG-IP?

BIG-IP is a family of software products from F5 Networks. These products offer an wide variety of different application access and security functions.

Load balancer, virtual IP address on behalf of devices behind it
Traffic management, high availability, acccess control, secuirty, optimization
Both hardward and software solutions
[used to manage high traffic applications, load balancing]
at the very basics: a load balancer, but also...
  firewall, tls inspection, offload, authentication

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

