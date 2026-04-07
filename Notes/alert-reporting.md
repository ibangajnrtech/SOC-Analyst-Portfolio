# Alert Funnel

Most of the alerts are closed as False Positives or are handled on L1 Level, but complex and threatening ones are sent to L2 that remediate most breaches.
To send further alerts, three things need to be done which are:
1. Reporting
2. Escalating
3. Communicating

## Alert Reporting
A detailed report to the Level 2 SOC Analyst containing all relevant evidence must be written whenever a complex or threathening alert rises which mostly happens for True Positives that require escalation.

## Alert Escalation
Always escalate every True Positives to the Level 2 SOC Analyst that require additional actions or deeper investigations for further review.
**_A detailed Alert report must be ready before escalating to a Level 2 SOC Analyst._**

## Communication
Always communicate with other IT teams depending on your observations to confirm a True Positive or False Positive.

# Reporting Guide

## Alert Report Purpose

1. **Provide context for escalation**: A well-written report will save lots of time for the L2 Analysts. It can also help them to quickly understand what happened.
2. **Save findings for the records**: Raw SIEM logs are only stored for 3-12 months, but alerts are kept indefinitely. Therefore, it is better to keep all context inside the alert, just in case of necessity.
3. **Improve investigation skills**: If it can't be explained to a 6 year old, then it is not understood well enough. This is a great way to boost L1 skills by summarising alerts.

## Report Format ([The Five W's](https://www.en.wikipedia.org/wiki/Five_Ws))

1. **Who**: Which user logs in, runs the command, downloads the file, etc.
2. **What**: What exact action or event sequence was performed, what file was downloaded, what commands were executed, etc.
3. **When**: When exactly did the suspicious activity start and end, etc.
4. **Where**: Where was the command run, which device, IP, or website was involved in the alert, etc.
5. **Why**: The reason for the verdict.

# Escalation Guide

After a verdict has been made and **_a detailed report_** has been written, you must decide if the alert should be escalated to L2 but this depends on the team.
For a general situation, the best possible times to escalate an alert are:
1. The alert is an indicator of a major cyberattack requiring deeper investigation or DFIR
2. Remediation actions like makware removal, host isolation, or password reset are required
3. Communication with customers, partners, management, or law enforcement agencies is required
4. You just do not fully understand the alert and need some help from more senior analysts.

## Escalation Steps
The steps taken towards escalation varies from team to team. In most cases you might just **reassign the alert to the L2 on the shift and ping them in corporate chat or in person** while some teams may require you to create a formal written escalation requrest wit dozens of required fields.

# SOC Communication

In the best scenario, the SOC team has its own Crisis Communication Procedures which are the guides and processes to help you and your teammates resolve the issues.

## Communication Cases
* **You need to escalate an urgent, critical alert, but L2 is unavailable and does not respond for 30 minutes.**
    Ensure you know where to find emergency contacts, First, try to call L2, then L3 and finally call the manager.
* **The alert about Slack/Teams account compromise requires you to validate the login with the affected user.**
    Do not contact the user through breached chat instea use alternative contact methods like a phone call.
* **You receive an overwhelming number of alerts during a short period of time, some of which are critical.**
    Prioritise the alerts according to the workflow, but inform your L2 on shift about the situation.
* **After a few days, you realise that misclassified the alert and likely missed a malicious action.**
    Immediately reach out to your L2 explaining you concerns. Threat actors can be silent for weeks before impact.
* **You can not complete the alert triage since the SIEM logs are not parsed correctly or are not searchable.**
    Do  not skip the alert but investigate what you can and report the issue to you L2 on shift or SOC engineer.