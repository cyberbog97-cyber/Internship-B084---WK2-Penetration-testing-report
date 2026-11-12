# Internship-B084---WK2-Penetration-testing-report

This is my Second  project on github for the networkwalks Internship B084 - WK2 program                                                              

**PENETRATION TESTING REPORT:**
**FOOTPRINTING, RECONNAISSANCE AND NETWORK SCANNING**

**W2-PM-FINAL | CYBERSECURITY |  NETWORKWALKS**
                                                                                                                                                
**Report information**:

**Pentester/Author:** Olusoji Samuel

**Programme/Batch:** B084D-Networkwalks

**Report date:**  9 November 2026

**Target website:** networkwalks.com

**Local network assessed:** My local LAN

**Approval for Testing Obtained:** Yes- Consent was obtained before conducting the exercise

**Operating system used:** Kali Linux and Windows

**Phases covered**:

**Phase 1**: Reconnaissance and Footprinting;

**Phase 2**: Network Scanning and Discovery

**Modules**:W2-PM1 (6 Kali Tools);W2-PM5((Zenmap Scanning)

**Assessment status**: Reconnaissance and host discovery completed                                                                                                                                                                                                                                                                                                                                                                           
# 1. Liability Disclaimer
I carried out the activities documented in this report for educational and cybersecurity training purposes. The assessment of the local network was performed within my own lab environment. 
Testing of the external domain, networkwalks.com, must only be conducted within the scope of permission granted by the domain owner or authorized organization. All activities should remain within the approved scope. 
The information in this report is intended for learning, research and security improvement. It should not be used to gain unauthorized access to any system or network. Unauthorized access may have legal consequences, even where no damage is caused.                                         
# 2. Introduction

This report covers the practical activities I carried out during the footprinting, reconnaissance and network scanning phases of my cybersecurity training. 
The first part focused on gathering publicly available information about the domain networkwalks.com using different Kali Linux tools. I used WHOIS, WhatWeb, Nslookup, Curl, WAFW00F and DNSRecon to understand the domain registration details, website technologies, HTTP response headers, DNS records and Web Application Firewall information. 
The second part focused on scanning my own local network. I used Nmap to identify active devices and check which IP addresses were responding on the network. The scan covered 256 IP addresses and identified two active hosts. One of the devices, 192.168.0.100, was identified as my Windows laptop, while 192.168.0.1 was likely the network gateway. 
The main purpose of these exercises was to understand how information can be collected from a website and how devices connected to a network can be discovered before carrying out further security assessments. 

# 3. TOOLS USED
The following tools were used during the practical activities. 

**Tool:**
* Kali linux: Operating system used for the reconnaissance exercises
* Windows 11: Operating system used for the local network environment 
* WHOIS: To obtain domain registration and nameserver information 
* WhatWeb:To identify website technologies, CMS and plugins 
* Nslookup**: To resolve the domain name to its IP address 
* Curl:To inspect HTTP response headers 
* WAFW00F:To identify the Web Application Firewall protecting the website 
* DNSRecon:To enumerate DNS records and identify DNS-related information 
* Nmap:To discover active hosts on the local network 


# 4. Activities Performed

# 4.1 "Footprinting and Reconnaissance**

For this activity, I used six tools in Kali Linux to collect information about networkwalks.com. Each tool provided a different type of information, which helped me build a clearer picture of the website and its publicly visible infrastructure. 

# 4.1.1 WHOIS Lookup
Objective: To obtain publicly available domain registration information and identify the domain's registrar and nameservers. 

**Command used**: 
whois networkwalks.com 
 
**Observations**:
The WHOIS lookup returned the following information: 

* Domain name: NETWORKWALKS.COM 
* Registrar: GoDaddy.com, LLC                                                                                                     
* Creation date: 6 November 2019                                                                                              
* Registry expiry date: 6 November 2027                                                                        
* Last updated: 12 November 2025                                              
* Registrant privacy service: Domains By Proxy, LLC                      
* DNSSEC: Unsigned 

The lookup also returned nameserver information. The results contained HostGator nameservers as well as DomainControl nameservers in different sections of the WHOIS output. 
 
**Security significance**:
WHOIS information helps security professionals understand the registration history and some of the infrastructure associated with a domain. It can also reveal information that may be useful during the reconnaissance stage. 
The use of a domain privacy service is a normal privacy measure and should not be treated as a security weakness. The different nameserver entries should be checked against the current authoritative DNS delegation before drawing any conclusion. 
 
# 4.1.2 WhatWeb Scan 

**Objective:** To identify the technologies and applications used by the website. 

**Command used:**
whatweb networkwalks.com 

**Observations:**

WhatWeb identified the following technologies and information:
* Web server:Apache
* Website platform: WordPress 7.1.4
* WordPress Download Manager: 3.3.76
* jQuery: 3.7.1
* Google Tag Manager
* Bootstrap fingerprint
* HTML5
* Open Graph metadata
* Website IP address: 192.232.216.135           

The scan also identified a cookie named __wpdm_client with the HttpOnly attribute. 

**Security significance:**
Identifying the CMS, plugins and other technologies helps a security professional understand the software running on a website. 
This information may help an attacker identify technologies that could require further investigation. However, the versions detected by WhatWeb do not automatically mean that the website is vulnerable. They would need to be checked against reliable security advisories and confirmed through authorized testing. 


# 4.1.3 Nslookup
**Objective:** To resolve the domain name and identify its associated IP address. 

**Command used:** 
nslookup networkwalks.com 

**Observations:**
The lookup used Google's public DNS resolver:

* DNS server: 8.8.8.8 
* Domain: networkwalks.com 
* Resolved IP address: 192.232.216.135 
* The result was returned as a non-authoritative answer. 

**Security significance:**
Nslookup helped me understand how a domain name is translated into an IP address. This is useful during reconnaissance because it helps identify the public IP address associated with a website. 
The IP address was also consistent with the address identified during the WhatWeb scan. 
 
 
 
# 4.1.4 Curl HTTP Header Inspection 
**Objective:** To inspect the HTTP response headers returned by the website. 

**Command used:** 
curl -I https://networkwalks.com 

**Observations:** 
The website returned an HTTP/2 200 response, which indicates that the request was successful. 
Some of the information returned included: 

* Server: Apache 
* Content type: text/html; charset=UTF-8 
* Referrer-Policy: no-referrer-when-downgrade 
* WordPress REST API endpoint: /wp-json/ 
* WordPress page endpoint: /wp-json/wp/v2/pages/53 
* Cookie: __wpdm_client 
* Cookie attributes: Secure and HttpOnly 
* Cache-related headers: x-endurance-cache-level and x-nginx-cache 

The response did not show an explicitSameSite attribute on the cookie. I also did not observe HSTS, Content-Security-Policy or X-Content-Type-Options in this particular response. 

**Security significance:**
HTTP response headers provide useful information about how a web application communicates with browsers. They can also reveal technical details about the website. 
The WordPress REST API endpoint is a normal WordPress feature, but its publicly available information should still be reviewed to ensure that sensitive information is not exposed. 
The absence of certain security headers in this response suggests areas that may require further review. It does not, by itself, confirm that the website is vulnerable. 


# 4.1.5 WAFW00F Scan 
**Objective:** To identify whether a Web Application Firewall is protecting the website. 

**Command used:** 
wafw00f https://networkwalks.com 

**Observations:**
The scan identified the following:

* Web Application Firewall: ModSecurity 
* Provider/fingerprint: SpiderLabs 
* Number of requests: 2 

**Security significance:**
The result indicates that the website is behind a Web Application Firewall identified as ModSecurity. 
A WAF can help monitor and filter suspicious web requests. Its effectiveness depends on its configuration, rules and maintenance. Identifying the WAF does not confirm how effectively it protects the website or whether all attacks would be blocked. 
 

# 4.1.6 DNSRecon
**Objective:** To enumerate DNS records and collect additional information about the domain's DNS infrastructure. 

**Command used:** 
dnsrecon -d networkwalks.com 

**Observations:**
DNSRecon returned eight records and identified the following information:

* SOA nameserver: ns6135.hostgator.com 
* Nameservers: ns6135.hostgator.com and ns6136.hostgator.com 
* Mail server: mail.networkwalks.com 
* Website IP address: 192.232.216.135 
* BIND version fingerprint: 9.16.23-RH 
* Google site verification TXT record 
* SPF record: v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all 
* cPanel email discovery service record 

DNSRecon also returned an error indicating that it could not obtain an answer to the DNSSEC query. This is consistent with the unsigned DNSSEC status observed in the WHOIS results. 

**Security significance:**
DNS information can reveal details about a domain's hosting environment, mail infrastructure and related services. 
The BIND version fingerprint provides information about the DNS software. This should be checked against the software provider's security updates before determining whether there is any actual vulnerability. 
The SPF record uses ~all, which represents a soft-fail policy. The organization should review whether the record correctly covers all legitimate email-sending services.The DNSSEC result also suggests an opportunity to review whether DNSSEC should be enabled.The nameserver information should be compared with the WHOIS delegation to confirm that the records are consistent. 
 

# 4.2 Network Scanning and Discovery Using Nmap
For the second activity, I used Nmap to identify active devices on my local network. 

The aim was to discover which devices were responding and obtain basic information about the hosts found during the scan. 

* Tool used: Nmap 7.993
* Scan output:The scan reported that 256 IP addresses were scanned and two hosts were up.
* The results identified: 
______________________________________________________________________
* IP address: 192.168.0.1
* Device identification: Likely router/network gateways.
* Status: Host up
_______________________________________________________________________
* IP address: 192.168.0.100
* Device identification: My Windows laptop
* Status: Host up 
______________________________________________________________________

The scan also returned the following information for the gateway: 

* IP address: 192.168.0.1 
* MAC address: E4:7D:EB:26:5E:95
* MAC vendor identification: Shanghai Notion Information Technology 
* Response time: 0.014 seconds 
* The second active host, 192.168.0.100, was identified as my Windows laptop. 
* The scan completed in 5.31 seconds. 

**Security significance:** 
Nmap helped me identify devices that were active on my local network. This is an important part of network discovery because security professionals need to understand which devices are connected before carrying out further assessments. 
Identifying active hosts can also help an administrator notice devices that are not expected to be connected to the network. 
The scan was a host-discovery exercise. It did not provide open-port or service-version results, so I cannot conclude which services are exposed on either device. 
 
 
 

# 5. RISK ANALYSIS / IMPACT 

Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks and observations. 
 
**Risk/Finding**

**1. Web technology information exposed**                                                                                                        
* Observation: WhatWeb identified WordPress and WordPress Download Manage                                                                               
* Impact Attackers may use exposed technology and version information to identify software requiring further security review.                       
* Risk level:🟠Medium 
                                                                                                                                                
**2. Server IP address identifiable.**                                                                                                                  
* Observation:Nslookup resolved the domain to 192.232.216.135.|                                                                                      
* Impact:Provides information about the  network location of the web service.|                                                                    
* Risk level: 🟡 Low.                                                                                                                               
                                                                                                                                               
**3. HTTP technical information exposed.**                                                                                                                   
* Obsevation:Curl returned HTTP response headers and WordPress REST API links.                                                                       
* Impact:May assist technology fingerprinting and further enumeration.                                                                                                                                                 
* Risk level:🟡 Low.  
                                                                                                                                                                                
 **4. WAF technology identifiable.**                                                                                                                  
   * Observation:WAFW00F identified ModSecurity (SpiderLabs).                                                                                        
   * Impact:Reveals information about the web application's security architecture.                                                                  
   * Risk level:🟡 Low                                                                                                                                

   
 **5. DNSSEC not detected.**
                                                                                                                                                                                                                                                                                              
   * Observation:WHOIS and DNSRecon results indicated that DNSSEC was unsigned or returned no answer.                                                 
   * Impact:If DNSSEC is not configured, responses may lack DNSSEC validation protection DNS.                                                       
   * Risk level:🟠 Medium                                                                                                                            
   
 **6. DNS infrastructure information exposed**.    
                                                                                                                                                    
   * Observaation:DNSRecon identified DNS, mail and service-related records.                                                                                         
   * Impact:DNS information can help build a  broader infrastructure profile.                                                                        
   * Risk level:🟠 Medium.    

   
**7. Multiple live hosts visible on local network**. 
                                                                                                                                                       
   * Observation:Zenmap identified two active hosts: 192.168.0.1 and 192.168.0.100.                                                                  
   * Impact:Unknown or unauthorised devices may be present on a network and should be investigated.                                                    
   * Risk level🟡low            

   
   Risk level key: 🔴 Critical · 🟠 Medium · 🟡 Low                                                                                              
   
The findings presented above reflect information gathered during the reconnaissance and network scanning exercises. They highlight possible areas that may require further security review rather than confirmed vulnerabilities. 
Both practical tasks focused on collecting technical information and identifying active devices on the network. No penetration attempts or vulnerability testing were carried out. 
It is important to note that identifying software versions, IP addresses or DNS records does not automatically indicate a security weakness. Additional authorised testing would be needed to determine whether any of these observations present an actual security risk.                                                                                                                                                                                   

# 6. Recommendations 
Based on the findings from the practical activities, I recommend the following security improvements:                                           
1. Review publicly exposed website technologies 
   Organizations should regularly review the technologies, CMS platforms and plugins that are publicly identifiable on their websites.            Unnecessary information should be limited where possible. 
2. Keep website software updated 
   WordPress, plugins and other web components should be maintained with supported versions and checked against current security advisories. 
3. Review HTTP security headers 
   The website's HTTP response headers should be reviewed. The organization should consider appropriate security headers such as HSTS,            Content-Security-Policy and X-Content-Type-Options based on its application requirements. 
4. Review cookie security settings 
   Cookie settings should be reviewed to ensure that appropriate Secure, HttpOnly and SameSite attributes are applied where required. 
5. Review DNS configuration 
   The DNS records should be checked regularly to ensure that only required services and records are publicly available. The nameserver           differences observed during the assessment should also be verified. 
6. Review DNSSEC configuration 
   The organization should consider enabling DNSSEC if supported by its DNS provider and operational requirements. 
7. Review email authentication records 
   The SPF record should be checked against all legitimate email-sending services. DMARC should also be checked and configured appropriately      if it is not already in place. 
8. Review DNS software maintenance 
   The identified BIND version fingerprint should be checked against the provider's current security updates. Software should be patched where    necessary. 
9. Maintain and monitor the Web Application Firewall 
   ModSecurity should be kept updated, properly configured and monitored. Its rules should be reviewed regularly to ensure that it provides       appropriate protection. 
10. Perform regular network discovery 
    Network administrators should periodically scan their own networks to identify active devices and maintain an accurate record of connected     systems. 
11. Investigate unknown devices 
    Any unexpected device discovered during network scanning should be checked and verified before being allowed to remain on the network. 
12. Maintain network documentation 
    IP addresses, MAC addresses, network devices and network topology should be properly documented and updated when changes occur. 
13. Carry out further testing only with authorization 

Any additional port scanning, service enumeration or vulnerability testing should be performed only on systems and networks where permission has been granted. 
 
 
 
 
 

# 7. Conclusion 

During these practical exercises, I completed activities covering footprinting, reconnaissance and network scanning. 

In the first activity, I used six Kali Linux tools to gather information about networkwalks.com. WHOIS provided domain registration details, WhatWeb identified the website technologies, Nslookup resolved the domain name, Curl helped me inspect HTTP response headers, WAFW00F identified ModSecurity, and DNSRecon provided additional information about the domain's DNS infrastructure. 

In the second activity, I used Nmap to scan my local network. The scan covered 256 IP addresses and identified two active hosts. I identified 192.168.0.100 as my Windows laptop, while 192.168.0.1 was likely the network gateway. 

These exercises helped me understand how much information can be collected before attempting any further security assessment. I also learned that identifying a technology or an active device does not automatically mean that a vulnerability exists. Findings need to be properly examined and verified before conclusions can be made. 

Overall, the practical improved my understanding of how reconnaissance and network discovery work, the importance of documenting findings and the need to follow the correct authorization process when carrying out cybersecurity activities.                                                                                                                                                                                                
# 8. Evidence Collected 
Each screenshot show the actual command or tool output.                                                                                         
**WHOIS command and output:Domain registration lookup**
<img width="1920" height="963" alt="Screenshot 2026-11-08 233516" src="https://github.com/user-attachments/assets/9a33c63b-7669-4256-9eab-c4b716234ab6" />
<img width="1920" height="951" alt="Screenshot 2026-11-08 233751" src="https://github.com/user-attachments/assets/c6bb8d9a-5d32-4965-a0df-7f4d92e006e6" />
<img width="1414" height="875" alt="Screenshot 2026-11-08 233821" src="https://github.com/user-attachments/assets/79cf26d9-fe88-4990-8e9e-d7cb4c6732f0" />                                                                                                                                
**WhatWeb command and output: Website technology identification**
<img width="1915" height="909" alt="Screenshot 2026-11-08 233358" src="https://github.com/user-attachments/assets/f6af1ab8-f563-4d91-b0e9-59d309df810c" />                                                                                                                                     
**Nslookup command and output: DNS resolutio**n
<img width="1894" height="953" alt="Screenshot 2026-11-08 234631" src="https://github.com/user-attachments/assets/85c09ef9-11d5-4c24-ad36-845a773ab951" />                                                                                                                                
**Curl command and HTTP headers: HTTP response inspection**
<img width="1917" height="518" alt="Screenshot 2026-11-08 234930" src="https://github.com/user-attachments/assets/56e56333-33d0-4687-9f0c-c7b4616848fc" />                                                                                                                                   
**WAFW00F command and output: WAF identification** 
<img width="1047" height="605" alt="Screenshot 2026-11-08 235418" src="https://github.com/user-attachments/assets/53188fc5-ab70-4a83-85bb-e73499e1ecbe" />


**DNSRecon command and output: DNS enumeration** 
<img width="1788" height="710" alt="Screenshot 2026-11-08 235946" src="https://github.com/user-attachments/assets/b3036284-e098-4283-a85b-a0d40ac52a64" />


Nmap scan output showing two active hosts: Local network discovery 
<img width="1063" height="633" alt="Screenshot 2026-11-09 025646" src="https://github.com/user-attachments/assets/9426a408-6f9a-4864-825b-6a4b8c29f848" />

<img width="1885" height="892" alt="Screenshot 2026-11-10 113241" src="https://github.com/user-attachments/assets/b9048484-9f28-402c-8af9-3ab87175e940" />

End of Report 

Project: Cybersecurity Practical Assessment 

Focus: Footprinting, Reconnaissance and Network Scanning 

Organization / Programme: Networkwalks Internship  B084D 
