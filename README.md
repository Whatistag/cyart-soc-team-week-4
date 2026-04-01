# cyart-soc-team-week-4



# 1. Introduction

The purpose of this internship task was to understand advanced Security Operations Center (SOC) operations through theoretical study and hands-on practical implementation. The activities focused on threat hunting, automation, incident response, adversary emulation, evidence analysis, and SOC performance metrics.

The internship involved using multiple security tools including **Elastic Security**, **Wazuh**, **TheHive**, **Velociraptor**, and **MITRE Caldera**. These tools were used to simulate attacks, detect threats, automate responses, and perform post-incident analysis.

The main objective was to develop real-world SOC analyst skills including threat detection, investigation, response automation, and reporting.

---

# 2. Threat Hunting Methodologies

## 2.1 Overview

Threat hunting is a proactive cybersecurity approach where analysts actively search for malicious activities that may evade traditional security controls.

Two major approaches were studied:

**1. Hypothesis-Driven Hunting**

* Analysts create a hypothesis about possible attacker behavior.
* Example: Detect misuse of valid accounts (MITRE T1078).

**2. Data-Driven Hunting**

* Analysts analyze logs and telemetry to discover anomalies.

Threat hunting frameworks studied include:

### SqRR Framework

* Search
* Query
* Retrieve
* Respond

### TaHiTI Framework

Targeted Hunting integrating Threat Intelligence to detect advanced threats.

Threat hunting techniques were mapped to the **MITRE ATT&CK** framework.

---

## 2.2 Threat Hunting Practical

### Hypothesis

Unauthorized privilege escalation may occur through misuse of domain accounts.

### Log Query

Using Elastic Security, logs were queried for **Windows Event ID 4672**, which indicates special privileges assigned to a user.

### Findings

| Timestamp        | User     | Event ID | Notes                       |
| ---------------- | -------- | -------- | --------------------------- |
| 2025-08-18 15:00 | testuser | 4672     | Unexpected admin privileges |

### Threat Intelligence Investigation

Indicators of compromise (IOCs) were checked using:

* **AlienVault OTX**
* Velociraptor process queries

Velociraptor query used:

```
SELECT * FROM processes
```

### Threat Hunting Report (100 words)

A proactive threat hunting activity was conducted to identify potential misuse of privileged accounts. Logs from Elastic Security were analyzed for Windows Event ID 4672, which indicates assignment of special privileges. An anomalous event was detected where the user “testuser” was granted administrative privileges unexpectedly. Threat intelligence checks were performed using AlienVault OTX to identify any associated malicious indicators. Additional endpoint investigation was conducted using Velociraptor to analyze running processes. The activity was mapped to MITRE ATT&CK technique T1078 (Valid Accounts). The investigation highlighted the importance of monitoring privileged access events to detect unauthorized account misuse and potential lateral movement attempts.

---

# 3. Advanced SOAR Automation

## 3.1 Overview

SOAR (Security Orchestration, Automation, and Response) platforms automate repetitive SOC tasks and improve incident response speed.

Key components include:

* **Orchestration** – Integration of multiple security tools
* **Automation** – Automated task execution
* **Response** – Automated containment actions

Tools used included **Splunk Phantom** and TheHive.

---

## 3.2 SOAR Playbook Development

A playbook was developed to automatically respond to phishing alerts.

### Playbook Workflow

1. Receive phishing alert from SIEM
2. Extract suspicious IP
3. Check IP reputation
4. Block IP using CrowdSec
5. Create incident ticket in TheHive

### Playbook Test Results

| Playbook Step | Status  | Notes                          |
| ------------- | ------- | ------------------------------ |
| Check IP      | Success | IP identified as malicious     |
| Block IP      | Success | CrowdSec blocked 192.168.1.102 |
| Create Case   | Success | TheHive case created           |

### Playbook Summary (50 words)

A SOAR playbook was developed to automate phishing alert response. The playbook checks IP reputation, blocks malicious IPs using CrowdSec, and creates incident tickets in TheHive. Automation reduced response time and improved SOC efficiency by eliminating manual investigation steps during phishing incidents.

---

# 4. Post-Incident Analysis

## 4.1 Root Cause Analysis

The **5 Whys technique** was used to determine the cause of a phishing incident.

| Question                    | Answer                           |
| --------------------------- | -------------------------------- |
| Why was the email opened?   | User clicked malicious link      |
| Why was link clicked?       | Email appeared legitimate        |
| Why was email not filtered? | Weak email filtering rules       |
| Why were rules weak?        | Security policies outdated       |
| Why were policies outdated? | Lack of regular security reviews |

---

## 4.2 Fishbone Analysis

The Fishbone diagram categorized root causes into:

* Process
* Technology
* Human factors
* Policies

This helped identify weaknesses in email filtering and user awareness training.

---

## 4.3 SOC Metrics

Two key SOC metrics were calculated.

| Metric                      | Value   |
| --------------------------- | ------- |
| Mean Time to Detect (MTTD)  | 2 hours |
| Mean Time to Respond (MTTR) | 4 hours |

### Summary

The phishing incident demonstrated delays in detection and response processes. Improving detection rules and user training can reduce response times.

---

# 5. Alert Triage and Threat Intelligence

Alerts were analyzed using **Wazuh** and validated using **VirusTotal**.

### Alert Investigation

| Alert ID | Description              | Source IP     | Priority | Status |
| -------- | ------------------------ | ------------- | -------- | ------ |
| 005      | Suspicious File Download | 192.168.1.102 | High     | Open   |

### Automated Validation

TheHive was configured to automatically submit file hashes to VirusTotal for malware analysis.

### Investigation Summary (50 words)

An alert for a suspicious file download was analyzed using Wazuh. The file hash was automatically submitted to VirusTotal for reputation analysis. The result indicated malicious behavior. The alert was escalated and an investigation case was created in TheHive for further response.

---

# 6. Evidence Analysis

Evidence was collected and analyzed using Velociraptor and **FTK Imager**.

### Network Connection Analysis

Velociraptor query:

```
SELECT * FROM netstat
```

Suspicious outbound connections were identified from a compromised host.

### Chain of Custody Documentation

| Item        | Description  | Collected By | Date       | Hash Value |
| ----------- | ------------ | ------------ | ---------- | ---------- |
| Network Log | Server-Z Log | SOC Analyst  | 2025-08-18 | SHA256     |

Maintaining chain-of-custody ensures the integrity and admissibility of digital evidence.

---

# 7. Adversary Emulation

Adversary behavior was simulated using **MITRE Caldera**.

### Simulated Attack

Technique:
MITRE ATT&CK **T1566 – Phishing**

### Detection Results

| Timestamp        | TTP   | Detection Status | Notes                  |
| ---------------- | ----- | ---------------- | ---------------------- |
| 2025-08-18 17:00 | T1566 | Detected         | Phishing email blocked |

### Emulation Report (100 words)

Adversary emulation was conducted using MITRE Caldera to simulate a spear-phishing attack mapped to MITRE ATT&CK technique T1566. The simulation tested the detection capability of the Wazuh SIEM platform. The system successfully detected the malicious activity through email monitoring and alert generation. Alerts were triaged and investigated within the SOC workflow. The results confirmed that the existing detection rules were effective in identifying phishing attacks. However, improvements were recommended in automated response and user awareness training to further strengthen organizational defenses against social engineering threats.

---

# 8. Security Metrics and Executive Reporting

SOC performance metrics were analyzed using Elastic Security dashboards.

### Metrics Dashboard

| Metric              | Value   |
| ------------------- | ------- |
| MTTD                | 2 hours |
| MTTR                | 4 hours |
| False Positive Rate | 8%      |

### Executive Summary (150 words)

During the monitoring period, the SOC team analyzed multiple security alerts using Elastic Security and Wazuh. A phishing-related incident was detected and investigated. The mean time to detect the threat was approximately two hours, while the response and containment actions were completed within four hours. Automated tools such as TheHive and VirusTotal improved investigation efficiency by automating threat intelligence checks and case management. Threat hunting activities also identified potential privilege escalation events that required further monitoring. Overall SOC performance was effective, though improvements are recommended in automated response playbooks and proactive threat hunting capabilities. Implementing additional automation and improving security awareness training for users will help reduce phishing risks and improve detection speed. These improvements will enhance the organization's overall cybersecurity posture.

---

# 9. Capstone Project – Comprehensive SOC Incident Response

## Attack Simulation

A vulnerability in **Metasploit Framework** was exploited on **Metasploitable2**.

Exploit used:

```
exploit/multi/samba/usermap_script
```

## Detection

| Timestamp        | Source IP     | Alert Description      | MITRE Technique |
| ---------------- | ------------- | ---------------------- | --------------- |
| 2025-08-18 16:00 | 192.168.1.102 | Samba exploit detected | T1210           |

Detection was performed using Wazuh.

## Response

Actions taken:

* Isolated compromised VM
* Blocked attacker IP using **CrowdSec**
* Created investigation case in TheHive

## Post-Incident Activities

* Root Cause Analysis
* Fishbone diagram
* Metrics calculation
* Executive reporting

---

# 10. Conclusion

This internship provided practical exposure to advanced SOC operations including threat hunting, adversary emulation, automation, and incident response. By integrating tools such as Elastic Security, Wazuh, TheHive, and MITRE Caldera, a complete SOC workflow was simulated from attack detection to post-incident analysis.

The exercises demonstrated the importance of proactive threat hunting, automated response workflows, and continuous improvement through metrics and reporting. Implementing automation and improving detection rules will significantly enhance SOC efficiency and reduce incident response time.

---
## Practical Application

#  1. Threat Hunting Practice


| Machine                                  | Role                    | Tools                                           |
| ---------------------------------------- | ----------------------- | ----------------------------------------------- |
| 🟡 **Windows 10**                        | Log Source + EDR Client | Winlogbeat / Elastic Agent, Velociraptor Client |
| 🔵 **Elastic Server (Ubuntu/Kali)**      | SIEM                    | Elasticsearch + Kibana (Elastic Security)       |
| 🟣 **Velociraptor Server (Ubuntu/Kali)** | EDR Backend             | Velociraptor Server                             |
| 🌐 **Internet**                          | Threat Intel            | AlienVault OTX                                  |

---

# ⚙️ 2. INSTALLATION STEPS (PER MACHINE)

---

##  A. ELASTIC SECURITY SETUP (SIEM)

 Machine: **Ubuntu/Kali (Elastic Server)**

### Step 1: Install Elastic Stack

```bash
curl -fsSL https://elastic.co/start-local | sh
```

OR manual:

```bash
sudo apt install elasticsearch kibana
```

Start:

```bash
systemctl start elasticsearch
systemctl start kibana
```

Open:

```
http://<elastic-ip>:5601
```

---

##  B. WINDOWS 10 (LOG SOURCE)

 Machine: **Windows 10**

### Step 1: Install Elastic Agent (Recommended)

Download from Elastic → install

```powershell
elastic-agent install --url=http://<elastic-ip>:8220 --enrollment-token=<token>
```

 Enable:

* Windows Security Logs
* Event Logs

---

### Step 2: Verify Event Logs

Check for:

* Event ID **4672** (privilege assignment)

---

##  C. VELOCIRAPTOR SETUP (EDR)

---

### Step 1: Install Server

 Machine: **Ubuntu/Kali**

```bash
wget https://github.com/Velocidex/velociraptor/releases/download/v0.7.0/velociraptor-linux-amd64
chmod +x velociraptor-linux-amd64
```

Run:

```bash
./velociraptor-linux-amd64 gui
```

---

### Step 2: Install Client

 Machine: **Windows**

* Download client config from server UI
* Run client:

```powershell
velociraptor.exe --config client.config.yaml
```

---

##  D. ALIENVAULT OTX (Threat Intel)

 No install needed

* Go to: (https://otx.alienvault.com)
* Create account

---

#  3. THREAT HUNTING PRACTICAL

---

##  Step 1: HYPOTHESIS DEVELOPMENT

 Hypothesis:

> “Unauthorized privilege escalation using valid accounts (MITRE T1078)”

---

##  Step 2: QUERY IN ELASTIC SECURITY

 Machine: **Elastic Server (Kibana UI)**

Go to:
**Security → Discover**

### Query:

```kql
event.code: "4672"
```

 This shows:

* Special privileges assigned to new logon

---

### 📊 Document Findings

| Timestamp        | User     | Event ID | Notes            |
| ---------------- | -------- | -------- | ---------------- |
| 2025-08-18 15:00 | testuser | 4672     | Unexpected admin |

---

##  Step 3: VALIDATE USING VELOCIRAPTOR

 Machine: **Velociraptor UI**

Go to:

* **Hunt → New Hunt**

Run query:

```sql
SELECT Name, CommandLine, Username FROM processes
```

---

 Look for:

* Suspicious processes
* Privileged users running unusual binaries

---

##  Step 4: THREAT INTEL WITH OTX

 Go to OTX

Search:

* Suspicious IPs from logs
* Known IOCs for **T1078**

---

### Example:

* IP: `192.168.1.100`
* Check if malicious

---

##  Step 5: CROSS-CORRELATION

Match:

| Source       | Finding            |
| ------------ | ------------------ |
| Elastic      | Event 4672         |
| Velociraptor | Suspicious process |
| OTX          | Malicious IP       |

---

#  4. HUNTING REPORT (100 WORDS)

**Threat Hunting Report**

A threat hunting exercise was conducted to detect unauthorized privilege escalation using valid accounts (MITRE ATT&CK T1078). Analysis in Elastic Security identified multiple Event ID 4672 logs indicating privileged logons by user “testuser,” which deviated from normal behavior. Further validation using Velociraptor revealed suspicious processes executed under the same account. Threat intelligence from AlienVault OTX confirmed associated IP activity as potentially malicious. The findings suggest possible misuse of valid credentials for unauthorized access. Immediate actions such as account review, password reset, and privilege auditing are recommended to mitigate further risks and strengthen access controls.

---

**2. SOAR Playbook Development**

#  1. LAB ARCHITECTURE (SOAR FLOW)


## 🔹 Machines Setup

| Machine                         | Role             | Tools               |
| ------------------------------- | ---------------- | ------------------- |
| 🔵 **Wazuh Server**             | Detection (SIEM) | Wazuh               |
| 🟡 **Windows / Ubuntu**         | SOAR             | Splunk Phantom      |
| 🟣 **Windows / Ubuntu**         | Case Mgmt        | TheHive             |
| 🟢 **Linux (same as Wazuh ok)** | Response         | CrowdSec            |
| 🔴 **Kali (optional)**          | Attacker         | Phishing simulation |

---

#  2. INSTALLATION (ONLY REQUIRED PARTS)

---

##  A. SPLUNK PHANTOM (SOAR)

 Machine: **Ubuntu (recommended)**

### Install:

```bash
wget https://download.splunk.com/products/phantom/releases/latest/phantom.tgz
tar -xvzf phantom.tgz
cd phantom
sudo ./install.sh
```

Access:

```
https://<phantom-ip>
```

---

##  B. THEHIVE

 Already installed (from previous lab)

If not:

```bash
docker run -d -p 9000:9000 thehiveproject/thehive
```

---

##  C. CROWDSEC

 Machine: **Wazuh / Ubuntu**

```bash
sudo apt install crowdsec
```

---

##  D. WAZUH (ALREADY SETUP)

We will **simulate phishing alert**

---

#  3. PLAYBOOK CREATION (SPLUNK PHANTOM)

 Machine: **Splunk Phantom UI**

---

##  Playbook Goal

 If phishing alert →

1. Extract IP
2. Check reputation
3. Block IP
4. Create TheHive case

---

##  Step-by-Step Playbook

### Step 1: Create Playbook

* Go → **Playbooks**
* Click → **New Playbook**
* Name: `Phishing_Auto_Response`

---

### Step 2: Add Blocks

---

##  Block 1: Extract IP

* Action: **Parse Artifact**
* Extract:

  * Source IP from Wazuh alert

---

##  Block 2: Check IP Reputation

* App: **VirusTotal / OTX**
* Action: `ip reputation`

Output:

* If malicious → continue

---

##  Block 3: Decision Block

Condition:

```
if reputation == malicious
```

---

##  Block 4: Block IP (CrowdSec)

Add custom action:

```bash
cscli decisions add --ip <ip> --ban
```

(Use Phantom SSH/App integration)

---

##  Block 5: Create TheHive Case

Use TheHive API:

Fields:

* Title: Phishing Incident
* Severity: High
* IP: artifact

---

# 🧪 4. PLAYBOOK TEST (IMPORTANT)

---

##  Step 1: Simulate Phishing Alert in Wazuh

 Machine: **Wazuh Agent (Linux/Windows)**

Generate fake alert:

```bash
logger "Phishing attempt detected from 192.168.1.102"
```

OR custom rule in Wazuh:

```xml
<rule id="100200" level="10">
  <match>phishing</match>
  <description>Phishing Attempt Detected</description>
</rule>
```

---

##  Step 2: Forward Alert to Phantom

* Use Webhook / API
* Configure Wazuh → Phantom integration

---

##  Step 3: Run Playbook

 In Phantom:

* Go → Containers
* Open alert
* Run Playbook

---

#  5. DOCUMENT RESULTS

| Playbook Step | Status  | Notes                          |
| ------------- | ------- | ------------------------------ |
| Check IP      | Success | IP flagged malicious           |
| Block IP      | Success | CrowdSec blocked 192.168.1.102 |
| Create Case   | Success | Case created in TheHive        |

---

#  6. 50-WORD PLAYBOOK SUMMARY


**Playbook Summary**

A SOAR playbook was developed using Splunk Phantom to automate phishing incident response. The playbook extracts suspicious IPs from alerts, checks reputation via threat intelligence, blocks malicious IPs using CrowdSec, and creates incident cases in TheHive. This automation improves response time, reduces manual effort, and enhances SOC efficiency.

---

# Evidence Preservation Practice Report

## 1. Objective

The objective of this exercise is to practice **digital evidence preservation during a security incident investigation**. This includes collecting volatile data, preserving system memory, generating cryptographic hashes for integrity verification, and documenting the chain of custody to maintain evidence authenticity.

<img width="1920" height="922" alt="velociraptor" src="https://github.com/user-attachments/assets/100e08bd-8509-4e52-8476-f6f509d9a148" />

---

# 2. Volatile Data Collection

Volatile data was collected using **Velociraptor** from a Windows virtual machine.

### Steps Performed

1. Open the **Velociraptor server dashboard**.
2. Select the target **Windows client system**.
3. Navigate to:

```
Hunt Manager → New Hunt
```
<img width="1024" height="768" alt="velociraptor new notebook create" src="https://github.com/user-attachments/assets/2507033a-cd38-438b-aef8-70c2d04d2b46" />

4. Run the query:

```
SELECT * FROM netstat()
```
<img width="1024" height="768" alt="velociraptor Add Query Cell Enter Netstat Query" src="https://github.com/user-attachments/assets/6da326f6-286b-4ad9-af7c-723bf11a5693" />

5. This query collects **active network connections** from the system.

<img width="1024" height="768" alt="velociptor after save and run the VQL cell" src="https://github.com/user-attachments/assets/863ce1b0-6209-464a-b508-e9d5428ac62f" />

### Example Output

| Proto | Local Address      | Remote Address   | State       | PID  |
| ----- | ------------------ | ---------------- | ----------- | ---- |
| TCP   | 192.168.1.10:49821 | 104.26.10.78:443 | ESTABLISHED | 3245 |
| TCP   | 192.168.1.10:49711 | 192.168.1.50:22  | TIME_WAIT   | 4123 |
