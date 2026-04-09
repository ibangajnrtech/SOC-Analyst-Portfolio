# 🚨 Incident Report: Inbound Email Containing Suspicious External Link
## 🧾 Overview
- **Date:** 8th April, 2026
- **Analyst:** Samuel Ibanga
- **Severity:** Medium
- **Status:** Escalated
- **Category:** Phishing

---

## 📌 Executive Summary
Splunk detected an email with a suspicious external URL that was received from a fake Microsoft support email.
No firewall or proxy blocked the outbound request to the suspicious external URL.

---

## 🖥️ Environment / Data Sources
- SIEM logs - Splunk
- Threat Intelligence - TryDetectThis 

---

## 🔍 Detection Details
- **Alert Name:** [Name of alert]
- **Trigger:** Inbound Email containing Suspicious External Link
- **Time Detected:** 8th April, 2026 12:48am
- **Source:** no-reply@m1crosoftsupport.co
- **Destination:** 10.20.2.25 (c.allen@thetrydaily.thm)

---

## 🧪 Investigation Steps

### Step 1: Initial Alert Review
- Reviewed alert details in SIEM (Splunk)
- Identified suspicious activity: User received a potentially malicious email from no-reply@m1crosoftsupport.co about an unusual login from Lagos, Nigeria with the IP address: 102.89.222.143 on 2025-01-24 06:42am. A URL attached to the email was flagged as malicious by the threat intelligence tool TryDetectThis.

### Step 2: Log Analysis
- Examined logs:
  - Email Security Logs

### Step 3: IOC Identification
- Identified Indicators of Compromise:
  - Domain: [URL](https://m1crosoftsupport.co/login)

### Step 4: Threat Intelligence Check
- Checked IOCs using:
  - TryDetectThis
- Result: Malicious

### Step 5: Impact Assessment
- Affected system(s): 10.20.2.25
- User(s): c.allen@thetrydaily.thm
- Scope of impact: Medium

---

## 🧠 Analysis & Findings
c.allen@thetrydaily.thm has received a potentially malicious email from no-reply@m1crosoftsupport.co about an unusual login from Lagos, Nigeria with the IP address: 102.89.222.143 on 2025-01-24 06:42am. A URL (https://m1crosoftsupport.co/login) has been attached which is flagged by TryDetectThis as a malicious URL.

---

## ⚠️ Conclusion
- **Was this malicious?** Yes
- **Confidence Level:** High

---

## 🚑 Response Actions
- [ ] Escalated to Tier 2 SOC

---

## 📤 Escalation Notes (If Applicable)
No sign of a firewall or proxy blocking the outbound request to https://m1crosoftsupport.co/login. Please confirm if user has access the URL.

---

## 💡 Recommendations
- Include the URL in the blacklist to prevent outbound requests from staffs.

---

## 📸 Evidence

