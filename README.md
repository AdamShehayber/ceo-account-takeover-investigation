# 🔐 CEO Account Takeover Investigation

## SOC Investigation Using Azure Data Explorer & KQL

This project documents a simulated Security Operations Center (SOC) investigation of an executive account takeover. Using **Azure Data Explorer** and **Kusto Query Language (KQL)**, I analyzed authentication and audit logs to identify the initial compromise, determine the attack method, investigate persistence mechanisms, scope affected accounts, and develop containment and remediation recommendations.

> **Note:** This project was completed in a controlled training environment using synthetic data. All users, organizations, IP addresses, and incident details are fictional.

---

## 🎯 Investigation Objectives

The investigation focused on answering the following questions:

- Was the suspicious foreign sign-in malicious or legitimate travel?
- How did the attacker obtain access?
- What activity occurred after the compromise?
- Did the attacker establish persistence?
- Were additional accounts compromised?
- What indicators of compromise could be identified?
- Which MITRE ATT&CK techniques were represented?
- What containment and remediation actions should be taken?

---

## 🛠️ Technologies & Skills

- Microsoft Azure
- Azure Data Explorer
- Kusto Query Language (KQL)
- Authentication Log Analysis
- Audit Log Analysis
- Threat Hunting
- Incident Investigation
- Indicator of Compromise (IOC) Analysis
- MITRE ATT&CK
- Identity Security
- Business Email Compromise (BEC) Analysis

---

## 🔎 Investigation

### 1. Suspicious Sign-In Triage

I began by reviewing the executive account's authentication activity during the incident window.

The investigation identified multiple failed authentication attempts originating from **Lagos, Nigeria**, followed shortly afterward by a successful authentication from the same infrastructure.

I compared this activity with the user's normal sign-in behavior and identified a significant deviation from the established baseline.

This helped distinguish malicious activity from legitimate travel or normal user behavior.

---

### 2. User Baseline Analysis

I analyzed historical authentication activity to establish the user's normal geographic and IP address patterns.

The account showed consistent legitimate activity from **London, United Kingdom**, while the Lagos activity appeared as an anomaly associated with authentication failures and unfamiliar infrastructure.

Baseline analysis provided additional evidence that the foreign authentication was not normal user activity.

---

### 3. Password Spray Investigation

I then searched authentication failures across the environment to determine whether the activity was isolated to one account.

The investigation revealed multiple Lagos-based IP addresses generating failed authentication attempts across numerous user accounts.

One observed source generated:

- **48 failed authentication attempts**
- **23 targeted accounts**

The distribution of relatively small numbers of attempts across many accounts was consistent with a **password spraying attack**, rather than a traditional brute-force attack against a single account.

---

### 4. Persistence Investigation

After confirming initial access, I analyzed audit logs for actions performed from the suspicious infrastructure.

Two significant persistence and post-compromise activities were identified:

**Unauthorized MFA registration**

An authentication application identified as **"Pixel 6"** was registered to the compromised executive account. This could allow continued account access even after a password reset.

**Malicious inbox rule**

An inbox rule named **"RSS Subscriptions"** was created to redirect or conceal finance and invoice-related messages.

This behavior was consistent with **Business Email Compromise (BEC) staging**, where an attacker attempts to hide financial communications from the legitimate mailbox owner.

---

### 5. Incident Scoping

I searched for successful authentication events associated with the attacker infrastructure to determine whether additional identities had been compromised.

The investigation identified activity affecting more than the original executive account, demonstrating the importance of expanding an investigation beyond the first detected victim.

This step helped establish the broader scope of the incident and identify accounts requiring containment.

---

## 🚨 Key Findings

The investigation determined that:

- The executive account experienced unauthorized access.
- The suspicious authentication was inconsistent with the user's established baseline.
- The attacker infrastructure conducted password spraying across multiple accounts.
- At least one attacker IP generated **48 failures against 23 accounts**.
- Successful authentication occurred after failed login attempts.
- An unauthorized MFA authentication method was registered.
- A malicious inbox rule was created to conceal finance-related email.
- Additional account compromise was identified during incident scoping.
- The observed behavior was consistent with credential-based initial access followed by persistence and BEC staging.

---

## 🧠 MITRE ATT&CK Mapping

| Tactic | Technique | Technique ID |
|---|---|---|
| Credential Access | Brute Force: Password Spraying | T1110.003 |
| Initial Access | Valid Accounts | T1078 |
| Persistence | Account Manipulation: Device Registration | T1098.005 |
| Defense Evasion | Hide Artifacts: Email Hiding Rules | T1564.008 |

---

## 🛡️ Recommended Response Actions

Based on the investigation, recommended containment and remediation actions include:

1. Revoke active sessions and refresh tokens for compromised accounts.
2. Reset credentials for confirmed compromised identities.
3. Remove unauthorized MFA authentication methods.
4. Delete malicious mailbox rules.
5. Block known attacker infrastructure.
6. Review additional targeted accounts for compromise.
7. Require MFA for user accounts.
8. Implement detections for password spraying.
9. Alert on unexpected MFA method registration.
10. Monitor creation of suspicious inbox rules, particularly on executive and finance-related accounts.

---

## 📸 Investigation Evidence

Screenshots included in this repository demonstrate:

- Suspicious sign-in analysis
- Geographic and IP baseline analysis
- Password spray detection
- Audit-log investigation
- MFA persistence discovery
- Malicious inbox-rule discovery
- Compromised-account scoping

---

## 📚 What I Learned

This project strengthened my ability to investigate identity-based attacks using KQL and correlate authentication and audit telemetry across an incident timeline.

A major takeaway was that identifying the initial suspicious login is only the beginning of an investigation. Proper incident response requires determining **how the attacker gained access, what they did after gaining access, whether persistence was established, and whether other identities were affected**.

The project also demonstrated how baseline analysis can help distinguish legitimate anomalous activity from true compromise and how MITRE ATT&CK can be used to document attacker behavior consistently.

---

## ⚠️ Disclaimer

This repository documents a cybersecurity training exercise performed in a controlled environment. The organization, accounts, incident data, and infrastructure used in the scenario are fictional or synthetic. This project is intended solely to demonstrate SOC investigation, KQL, threat hunting, and incident response skills.
