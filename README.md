# SOC Capstone Project – Incident Investigation

This project demonstrates an end-to-end **Security Operations Centre (SOC) incident-handling workflow** using **Wazuh** to monitor an Ubuntu endpoint. The project covers suspicious file detection, investigation, threat-intelligence enrichment, automated remediation, and post-remediation verification using the **EICAR antivirus test file** as a safe security simulation.

## Table of Contents

- Project Overview
- Network Topology
- Tools and Technologies
- Configuration Steps
- Results and Findings
- Author

## Project Overview

The purpose of this project was to demonstrate practical **SOC monitoring and incident response** using Wazuh.

The main objectives were:

- Monitor an **Ubuntu endpoint** using the Wazuh agent
- Configure **File Integrity Monitoring (FIM)**
- Detect suspicious file creation
- Investigate file and cryptographic hash information
- Use **VirusTotal** for threat-intelligence enrichment
- Use **Wazuh Active Response** for automated remediation
- Verify that remediation was successful
- Document the incident and supporting evidence

The overall SOC workflow demonstrated was:

**Detection → Investigation → Threat Intelligence → Response → Remediation → Verification → Closure**

## Network Topology

The SOC lab environment consisted of an **Ubuntu endpoint** and a **Wazuh server**.

- **Ubuntu Endpoint**
  - Wazuh agent installed and running
  - `/root` directory monitored using File Integrity Monitoring
  - EICAR antivirus test file used to simulate suspicious file activity

- **Wazuh Server**
  - Received and analysed security events from the Ubuntu endpoint
  - Hosted custom detection rules
  - Provided **VirusTotal integration**
  - Provided **Wazuh Active Response**
  - Remotely administered from the Ubuntu environment using **SSH**

The monitored file location used during the investigation was:

`/root/eicar.com`

## Tools and Technologies

- **Wazuh** – SIEM/XDR platform used for endpoint monitoring, alerting, FIM, and Active Response
- **Wazuh Agent** – Installed on the Ubuntu endpoint for security monitoring
- **Ubuntu Linux** – Monitored endpoint used during the security simulation
- **File Integrity Monitoring (FIM)** – Used to monitor changes within the `/root` directory
- **VirusTotal** – Used for threat-intelligence enrichment and file-hash analysis
- **EICAR Antivirus Test File** – Safe test file used to simulate suspicious file activity
- **SSH** – Used for remote administration of the Wazuh server
- **curl** – Used to download the EICAR test file
- **Wazuh Active Response** – Used to automate threat remediation
- `remove-threat.sh` – Active Response script used to remove the detected file

## Configuration Steps

1. **Configure the Wazuh Agent**

   The Wazuh agent was installed and running on the Ubuntu endpoint to provide security monitoring and send events to the Wazuh server.

2. **Configure File Integrity Monitoring**

   Wazuh FIM was configured to monitor the `/root` directory in near real-time.

   The main FIM configuration was:

   `<directories realtime="yes">/root</directories>`

   FIM was enabled using:

   `<disabled>no</disabled>`

3. **Configure Custom Detection Rules**

   Custom Wazuh rules were configured to monitor activity within `/root`.

   - **Rule 100200** monitored file modifications
   - **Rule 100201** generated an alert when a new file was added

4. **Emulate Suspicious File Activity**

   The EICAR antivirus test file was downloaded to the Ubuntu endpoint using:

   `sudo curl -Lo /root/eicar.com https://secure.eicar.org/eicar.com`

   EICAR provided a safe method of testing the security controls without introducing real malware.

5. **Detect the File Creation**

   Wazuh FIM detected the newly created file in near real-time.

   The initial detection showed:

   - **Agent:** Ubuntu
   - **File:** `/root/eicar.com`
   - **Event:** File added
   - **Mode:** realtime
   - **Rule ID:** 100201
   - **Rule Level:** 7
   - **Description:** File added to `/root` directory

6. **Investigate the File and Hash**

   The Wazuh alert was investigated to identify the file and its cryptographic hash.

   - **File:** `/root/eicar.com`
   - **MD5:** `44d88612fea8a8f36de82e1278abb02f`
   - **Event:** added
   - **Mode:** realtime

   The cryptographic hash provided an indicator that could be correlated with external threat intelligence.

7. **Perform VirusTotal Threat-Intelligence Enrichment**

   The configured Wazuh VirusTotal integration analysed the file indicator.

   VirusTotal returned:

   - **Positive detections:** 66
   - **Total engines:** 68
   - **File:** `/root/eicar.com`

   Within the controlled EICAR simulation, the result was treated as a confirmed detection requiring remediation.

8. **Configure and Execute Active Response**

   Wazuh Active Response was configured to execute `remove-threat.sh` when the applicable VirusTotal detection rule was triggered.

   Wazuh subsequently reported:

   `active-response/bin/remove-threat.sh removed threat located at /root/eicar.com`

   The remediation event generated:

   - **Rule ID:** 100092
   - **Rule Level:** 12

   This demonstrated automated remediation rather than relying only on manual analyst intervention.

9. **Verify Successful Remediation**

   After Wazuh reported successful removal, the Ubuntu endpoint was checked independently using:

   `sudo ls -l /root/eicar.com`

   Ubuntu returned:

   `ls: cannot access '/root/eicar.com': No such file or directory`

   This confirmed that the EICAR file had been successfully removed from the endpoint.

10. **Close and Classify the Incident**

   The final incident disposition was:

   **TRUE POSITIVE – CONTROLLED SECURITY TEST**

   The EICAR file represented a genuine security detection, but it was deliberately introduced as a safe testing mechanism and was not actual malware. No further remediation was required after successful removal and verification.

## Results and Findings

The project successfully demonstrated a complete **SOC incident-handling workflow** using Wazuh and Ubuntu.

Key results included:

- Wazuh successfully monitored the `/root` directory using **File Integrity Monitoring**
- The creation of `/root/eicar.com` was detected in near real-time
- Custom **Rule ID 100201** successfully generated the initial security alert
- File and cryptographic hash information supported further investigation
- VirusTotal reported **66 detections from 68 engines**
- Wazuh Active Response automatically executed `remove-threat.sh`
- **Rule ID 100092**, Level 12, reported successful remediation
- Independent verification confirmed that `/root/eicar.com` no longer existed
- The incident was classified as a **True Positive – Controlled Security Test**
- The project demonstrated practical experience in endpoint monitoring, alert analysis, threat intelligence, incident response, remediation, verification, and security documentation

The final workflow was:

**Detection → Investigation → Threat Intelligence → Response → Remediation → Verification → Closure**

## Author

**Alex Osale**

- **Field:** Cybersecurity
- **Location:** Dundee, UK
- **Career Goal:** Cybersecurity Analyst
- **LinkedIn:** www.linkedin.com/in/alex-osale-857876bb
- **Email:** lexy8074@gmail.com
