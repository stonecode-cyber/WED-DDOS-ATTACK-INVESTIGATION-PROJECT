# DOS-DDOS-ATTACK-INVESTIGATION-PROJECT

1. Project Overview
2. Types of DoS/DDoS Attacks
3. Indicators of a DoS/DDoS Attack
4. Web Server Access Log Investigation
5. Splunk SIEM Investigation
6. Tools & Technologies
7. Conclusion

This project documents a hands-on investigation of a Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) attack against a bicycle parts website.

The investigation involved analysing web server access logs and using Splunk Enterprise SIEM to identify suspicious traffic patterns, investigate source IP addresses, identify targeted website resources and assess the impact of the attack on legitimate users.

The practical exercise focused on identifying attack indicators, analysing HTTP request patterns, investigating User-Agent strings, and using Splunk search commands to detect abnormal traffic volumes.

- Slowloris: Sends incomplete HTTP requests to keep server connections occupied.
- HTTP Flood: Sends a large volume of HTTP requests to overwhelm server resources.
- Cache Bypass: Bypasses CDN caching mechanisms and forces requests to reach the origin server.
- Oversized Query: Sends resource-intensive requests that consume server processing resources.
- Login/Form Abuse: Overloads authentication processes through repeated login attempts or password reset requests.
- Faulty Input Validation Abuse: Exploits poorly designed input handling to consume server resources.

The following indicators were considered during the investigation:

- High Request Rate: An unusual increase in requests targeting specific website resources.
- Unusual User-Agents: Suspicious or uncommon User-Agent strings appearing repeatedly.
- Geographic Anomalies: Requests originating from unusual or widely distributed geographical locations.
- Burst Timestamps: Large volumes of requests arriving within very short intervals.
- Server Errors: Increased HTTP 5xx responses, potentially indicating server resource exhaustion.
- Login Abuse: Excessive requests targeting authentication endpoints.

The initial investigation involved examining the website's access.log file to distinguish normal user-generated traffic from suspicious requests.

Task 1: Identify the attacker's IP address
By examining repeated requests originating from the same source within short intervals, I identified suspicious traffic patterns associated with the attack.

Task 2: Identify the repeatedly targeted page
The investigation revealed that the attacker repeatedly targeted the website's /login endpoint, potentially overwhelming the authentication process.

Task 3: Identify the HTTP error code received by legitimate users
Following the excessive requests, legitimate users received HTTP 503 – Service Unavailable, indicating that the website was unable to handle requests.
The second phase involved using Splunk Enterprise and its Search & Reporting application to investigate indexed web traffic.

The investigation focused on analysing request frequency, client IP addresses, User-Agent strings and traffic patterns over time.
I examined the URI field to identify the resource receiving the highest number of requests.

Finding: SEARCH (as identified during the Splunk investigation).
By examining the clientip field, I identified the IP address generating the highest volume of requests to the target URI.

I used Splunk's top command to examine the most frequently appearing client IP addresses.

SPL Query:
```
index="main" | top limit=100 clientip
```
Finding: 60 IP addresses associated with the attacking traffic.
I examined the useragent field to identify the most frequently appearing User-Agent in the traffic.

Finding: Java/1.8.0_181

Observed frequency: 135 requests.
I used Splunk's timechart functionality to visualise request volumes over time and identify the peak traffic period.

Findings:
- Peak request volume: 207 requests per second
- Timestamp: 13:12:22
This traffic spike provided further evidence of automated requests overwhelming the website.
This task involved identifying the first legitimate client IP that received an HTTP 503 response following the attack.

Status: Pending completion.
- Splunk Enterprise: SIEM platform for searching, analysing and visualising web traffic.
- Splunk Search & Reporting: Investigating indexed events and extracting relevant fields.
- SPL (Search Processing Language): Searching and analysing web traffic.
- Web Server Access Logs: Analysing HTTP requests, source IP addresses and response codes.
- HTTP Protocol: Understanding web requests, responses and server status codes.

This investigation provided practical experience in identifying potential DoS/DDoS activity through web server log analysis and SIEM-based investigation.
By correlating request frequency, client IP addresses, User-Agent strings, targeted resources and HTTP response codes, 
I was able to identify suspicious traffic patterns and assess their impact on legitimate website users.

Key Takeaway: Effective DDoS detection goes beyond identifying unusually high traffic volumes. 
It requires correlating multiple indicators, understanding normal traffic behaviour and determining how malicious activity affects service availability.
