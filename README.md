# Phishing-email-analysis-lab(Greenholt Phish) 1

## Objective

The phishing email analysis lab aimed to establish a controlled environment for simulating and detecting cyber attacks. It was carried out in TryHackMe with the primary goal of analyzing email headers and links to identify indicators of phishing and malicious activity. This included examining metadata such as sender domains, URLs, IP addresses, hash values, and other anomalies that may reveal spoofing, unauthorized relays, or additional threats.

### Skills Learned

- Social Engineering Pattern Recognition.
- Malware & Attachment Examination.
- Email Header Analysis.
- Threat Intelligence Correlation.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- ANY.RUN for Sandboxing & Malware Detonation.
- MXToolbox (Header Analyzer) for Email Header & Content Analysis Tools.
- CyberChef for Forensic, defanging & Decoding Utilities.

## Steps
PHISHING ANALYSIS 

#PROJECT1 
As a SOC Level 1 Analyst, I followed a structured investigation to analyze the "Greenholt Phish" incident. Here is the step-by-step process I took to identify the threat and protect the organization on the tryhackme platform: 

1. Initial Triage and Visual Inspection: 
 I started by examining the suspicious email in Thunderbird to avoid accidental execution.

 Observations:

I noted a generic greeting ("Good day") and an unexpected claim of a fund transfer, which the recipient confirmed was unusual.

●Identity Check: I identified the sender as "Mr. James Jackson" at info@mutawamarine.com.

●Reply-To Discrepancy: I discovered that the "Reply-To" address was info.mutawamarine@mail.com.
This is a classic "domain spoofing" red flag where the attacker attempts to divert replies to a public mail server they control.


2. Email Header Analysis:
I moved beyond the visible fields to inspect the raw headers using Mailheader.org to trace the email's true path.

●Locating the Originating IP: I parsed the "Received" headers and identified the originating IP as 192.119.71.157.
●WHOIS & IPinfo.io Investigation: I ran a WHOIS lookup and used IPinfo.io to verify the IP owner as Hostwinds LLC

Findings: The IP belongs to Hostwinds LLC, a VPS provider often used by attackers to host malicious infrastructure.

Security Records: I checked the DNS records using MXToolbox and found an SPF record (v=spf1 include:spf.protection.outlook.com -all) and a DMARC record (p=quarantine), indicating the organization has strict policies to catch unauthorized senders.


3. URL Extraction and Reputation Scanning: 
The email contained several links that I needed to analyze safely.
Extraction: I used URL Extractor and CyberChef (using the "Extract URLs" recipe) to pull all raw links from the body.
Scanning: I submitted these links to URLScan.io to see a safe rendering of the destination pages. This allowed me to verify if they lead to credential harvesting sites without visiting them myself.


4. Attachment Forensics:
   
The email included an attachment named SWT_#09674321_____PDF___.CAB. This is highly suspicious as it uses a compressed file extension (.CAB) but is named to look like a PDF.


Hashing: I used the command sha256sum in my terminal to generate a unique hash of the file: 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f. 

VirusTotal Check: I pasted this hash into VirusTotal.

 Result: The file was flagged by 45 out of 61 security vendors as malicious. It was specifically identified as a Trojan/msil.loki (LokiBot), a common info-stealer.

Sandbox Analysis: I would typically upload a file like this to Any.Run or Hybrid Analysis to observe its behavior in a live environment, such as attempts to contact Command and Control (C2) servers or the dropping of further payloads like the RAR archive seen in the file properties.


5. Final Incident Reporting:
After confirming the email was a malicious phishing attempt, I finalized my response:

Defanging: I used CyberChef to "defang" the malicious IP (e.g., 192[.]119[.]71[.]157) so it could be safely shared in my report.


<img src="https://github.com/user-attachments/assets/96e50fb4-f609-4353-b5da-2e6284aefdbc" />


