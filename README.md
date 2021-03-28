# CVE 2020-5902

![alt text](https://www.zdnet.com/a/hub/i/2020/07/03/b6c96e0e-7da9-461a-adff-d6009723189a/f5-networks.jpg "F5 Networks Logo")

_Presented by: Tharmiga Loganathan, Manavjot Singh, Mili Choksi_

**Vulnerability Highlights:**

CVSS 3.x: 9.8 (Critical)

CVSS 2.0: 10.0 (High)

_The CVE 2020-5902 vulnerability impacted F5 Network's suite of load-balancing software products called BIG-IP last July. It is a code injection attack that can give hackers root level privileges to vulnerable systems! According to Shodan, there were 8,000 exposed devices at one point with an estimated 2,000 of those devices actually vulnerable to this exploit._

> **What is Shodan?**
> At first look, Shodan can be used to search devices connected to the Internet according to different filters. However, in more negative use-cases, it can be used streamline the process of searching for vulnerable devices.

**Outline:**

* Intro, scale
* What is BIGIP - load balancer
* The exploit
* Create alias explanation
* How to check if you are vulnerable
* Mitigation
* Could still be impacted, use resource integrity!... explain
* So this is a story on why it's important to secure your permissions, and stay up to date on big vulnerabilities that come out!
  

## What is BIG-IP? + Why Should I Care?

BIG-IP is a family of software and hardware solutions from F5 Networks. While it originally started out as a load balancer, it has now expanded to offer complimentary services such as traffic management, access control, security and optimization. Load balancers work by providing a virtual IP on behalf of numerous other devices. This is a service that allows large scale applications to seamlessly serve its customers without a bottleneck. 

This exploit is particularly impactful because it can not only impact the load balancing server itself but also any device that is behind the load balancer. 
  
Moreover, even if you aren't running an F5 BIG-IP device, but have some external link to a javascript library included in your application, your application may also be exposed! If the resource happens to be behind a BIG-IP load balancer that is vulnerable, your application can also be exploited. Using subresource integrity can help make you immune to this sort of vulnerability and it is also best practice when it comes to including external libraries in your application, more on this will be discussed later in the reprot. This highlights the importance of staying on top of cybersecurity news and taking all precautions posisble when building your application!

## CVE Timeline

There has been a rise in the number of vulnerabilities among similar software products around the time this vulnerability came out. The very day the exploit got out, F5 Networks has created many articles, videos, and Q&A sessions designed to help its customers keep their systems safe. But it didn't take long for **many** proof of concept code snippets to pop up on GitHub.

![alt text](https://www.tenable.com/sites/drupal.dmz.tenablesecurity.com/files/images/blog/PoC%20scripts%20published%20to%20GitHub.png "GitHub Exploit Code")

Let's dive right into the exploit now!

## The Exploit

Impacts the TMUI (configuration utility for the BIG-IP). 



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


## Mitigation

Check if you are vulnerable: code 
Check logs to see if you have been exploited
Never expose admin interface to the public
  F5 has said multiple times not to do this
  But if you did anyway, you are vulnerable

• update software
• ensure mgmt port is not publically accessible
• lock down self ip ports "allow none"
• var/log to see if it has been breached

## What now?

[content]

## Resources

https://www.tenable.com/blog/cve-2020-5902-critical-vulnerability-in-f5-big-ip-traffic-management-user-interface-tmui
BIG IP
https://www.youtube.com/watch?v=D6J_j7HdkV8&ab_channel=F5DevCentral 
Self IP
https://www.youtube.com/watch?v=ZeDP6D7fEwM&ab_channel=SteveLyons
The basics of the exploit
https://www.youtube.com/watch?v=hS8ptpsmgJE&ab_channel=F5DevCentral
the exploit - SANS
https://www.youtube.com/watch?v=NmZFwE537Zg&ab_channel=SANSInstitute

