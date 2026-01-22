# 🛡️ SIEM-Based Security Monitoring Lab (SOC L1)

## 📌 Project Overview
This project demonstrates a **SOC Level 1 (L1) Security Monitoring Lab** using **Splunk SIEM** on Linux.  
The lab focuses on **log ingestion, alert creation, and basic incident triage** by monitoring Linux authentication logs for repeated failed SSH login attempts.

This project simulates **real SOC L1 responsibilities**, including:
- Log monitoring
- SIEM alerting
- Incident analysis
- Documentation

---

## 🧰 Tools & Technologies Used
- **Operating System:** CentOS / RHEL (Linux)
- **SIEM Tool:** Splunk Enterprise
- **Log Source:** Linux authentication logs (`/var/log/secure`)
- **Attack Simulation:** Failed SSH login attempts
- **Skills Covered:** SOC monitoring, alerting, triage, documentation

---

## 🏗️ Lab Architecture
```

Linux Server
|
|-- Authentication Logs (/var/log/secure)
|
Splunk SIEM
|
SOC L1 Alert & Triage

````

---

## ⚙️ Step 1: Install Splunk on Linux

Download Splunk:
```bash
cd /opt
wget -O splunk.tgz https://download.splunk.com/products/splunk/releases/9.1.2/linux/splunk-9.1.2-b6b9c8185839-Linux-x86_64.tgz
````

Extract:

```bash
tar -xvzf splunk.tgz
```

Start Splunk and accept license:

```bash
/opt/splunk/bin/splunk start --accept-license
```

Create an **admin username and password** when prompted.

Enable Splunk at boot:

```bash
sudo /opt/splunk/bin/splunk enable boot-start -systemd-managed 1
```

---

## 🌐 Step 2: Access Splunk Web Interface

Open browser and go to:

```
http://localhost:8000
```

Login with:

* **Username:** admin (or your chosen username)
* **Password:** your password

---

## 📥 Step 3: Add Linux Authentication Logs to Splunk

1. Go to **Settings → Add Data**
2. Select **Files & Directories**
3. Add log file:

   ```
   /var/log/secure
   ```
4. Click **Next → Review → Submit**

Splunk will now ingest Linux authentication logs.

---

## 🔍 Step 4: Verify Logs in Splunk

Go to **Search & Reporting** and run:

```spl
failed password
```

This displays failed SSH login attempts from authentication logs.

---

## 🧪 Step 5: Generate Failed Login Attempts

From terminal:

```bash
ssh parthi@localhost
```

Enter **wrong password multiple times (10–12 attempts)**.

Verify logs:

```bash
grep "Failed password" /var/log/secure
```

---

## 🚨 Step 6: Create SIEM Alert (SOC L1)

In Splunk search:

```spl
failed password
```

Click **Save As → Alert**

### Alert Configuration:

* **Title:** SSH Failed Login Alert
* **Alert Type:** Scheduled
* **Cron Schedule:**

  ```
  */1 * * * *
  ```
* **Time Range:** Last 15 minutes
* **Trigger Condition:**

  ```
  Number of Results > 10
  ```
* **Trigger:** Once
* **Permissions:** Private

Click **Save**.

---

## 📊 Step 7: Validate Alert Trigger

1. Generate more failed login attempts
2. Wait 1 minute
3. Go to **Alerts → Triggered Alerts**

You should see:

```
SSH Failed Login Alert – Triggered
```

---

## 🧠 Step 8: SOC L1 Incident Triage

After the alert triggers:

* Review failed login events
* Identify repeated authentication failures
* Assess severity
* Decide escalation requirement

---

## 🧾 Step 9: Incident Documentation

Create an incident report:

```bash
nano incident_report.txt
```

Example content:

```
Incident Type: Multiple Failed SSH Login Attempts
Severity: Medium
Source Log: /var/log/secure
Detected By: Splunk SIEM Alert
Action Taken: Reviewed logs and monitored system
Status: Closed
```
