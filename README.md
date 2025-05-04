# Security Incident Investigation

> 📚 *This project was completed as part of the **Google Cybersecurity Certificate – Course 3: Connect and Protect: Networks and Network Security**, Module 4 Activity: Apply OS hardening techniques.*

## Objective
This project aimed to investigate and document a real-world security incident involving a brute force attack and malware injection on the website **yummyrecipesforme.com**. The goal was to analyze network traffic, identify exploited vulnerabilities, and recommend remediations to prevent similar attacks in the future.

### Skills learned

- Interpretation of tcpdump logs and network protocols  
- Incident response documentation and reporting  
- Malware behavior analysis through sandbox testing  
- Detection of web-based attack vectors  
- Understanding of brute force attack mechanisms  
- Application of layered defense strategies  

### Tools used

- `tcpdump` for packet capture and network traffic analysis  
- Sandbox environment for controlled malware execution  
- Text analysis and reporting tools (Word, Markdown)  
- DNS and HTTP protocol interpretation  
- Cybersecurity incident report template  

## Steps

### Section 1: Identify the network protocol involved

The network protocols involved in the incident were:

- **DNS (Domain Name System)**: Used to resolve domain names (`yummyrecipesforme.com` and `greatrecipesforme.com`) to IP addresses.  
- **HTTP (Hypertext Transfer Protocol)**: Used to request and transmit web content, including the malicious file.  
- **TCP (Transmission Control Protocol)**: Transport layer protocol used to establish reliable connections between client and server.  

These protocols appeared in the `tcpdump` logs during the analysis of DNS queries and HTTP GET requests.

---

### Section 2: Document the incident

Customers of **yummyrecipesforme.com** began reporting that the website prompted them to download a suspicious file. Upon executing the file, their browsers were redirected to a different domain (`greatrecipesforme.com`), and their computers showed signs of malware infection (e.g., slow performance).

#### Investigation steps

- A sandboxed test environment was used to safely replicate the incident.  
- Using `tcpdump`, analysts captured network traffic during a visit to the compromised site.  
- A **DNS request** was issued to resolve `yummyrecipesforme.com`, which returned the correct IP.  
- An **HTTP GET** request was made, triggering a download of a malicious executable.  
- Executing the file resulted in a **DNS request** and subsequent **HTTP request** to `greatrecipesforme.com`, redirecting the browser to a clone site hosting malware.

#### Root cause

The investigation revealed that a former employee used a brute force attack to access the website's admin panel. The admin credentials were protected only by a default password, and no controls were in place to block or alert on repeated login attempts. Once inside, the attacker embedded malicious JavaScript into the site’s source code, changed the admin password, and uploaded the malware.

#### Evidence Sources

- tcpdump traffic logs  
- JavaScript code review  
- User helpdesk reports  
- Sandbox behavior analysis  

---

### Section 3: Recommend one remediation for brute force attacks

**Recommendation: Account lockout after failed login attempts**

Enforcing a limit on consecutive failed login attempts is a simple but effective way to mitigate brute force attacks. After a set number of failed logins (e.g., 5), the account is temporarily locked or login attempts are delayed. This increases the time and difficulty for attackers trying to guess credentials and provides an opportunity for alerting and incident response.

---

## References

**Ref 1: DNS request for yummyrecipesforme.com observed in tcpdump**

14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)

**Ref 2: HTTP GET request to download the malicious file**

14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, length 73: HTTP: GET / HTTP/1.1

**Ref 3: Redirection to greatrecipesforme.com after file execution**

14:25:29.576590 IP your.machine.56378 > greatrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, length 73: HTTP: GET / HTTP/1.1

