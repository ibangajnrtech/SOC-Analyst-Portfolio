# Core Metrics

Recall that the main goal of a SOC is to protect the confidentiality, integrity and availability of the organisation's digital assets. 
Metrics in SOC:
1. **Alerts Count**: The ideal metric value depends on the company size but in general, **5 to 30 alerts per day per L1 analyst is a good metric**.
>_AC = Total Count of Alerts Received_
>Measures the overall load of SOC analysts.

2. **False Positive Rate**: With more noise, analysts tend to become less vigilant and more likely to miss the threat and treat all alerts just like "yet another spam". A False positive rate of **0% is an unachievable ideal, but 80% or higher is a serious problem**, usually fixed by a tool and detection rules tuning, often called **_False Positive Remediation_**.
>FPR = False Positives / Total Alerts
> Measures the level of noise in the alerts

3. **Alert Escalation Rate**: L2 analysts rely on L1 to filter out the noise and escalate only the actionable, threathening alerts. As an L1, you don't want to be overconfident and triage alerts you do not fully understand without the help of a senior support.
>AER = Escalated Alerts / Total Alerts
>Measures the experience of L1 Analsyts.

4. **Threat Detection Rate**: The threat detection should always be at 100% since every missed threat can have a devastating consequences, such as ransomware infection and data exfiltration.
>TDR = Detected Threats / Total Threats
>Measures the reliability of the SOC Team

# Triage Metrics

Remember that an alert by iteself will not stop the breach and it is also important to timely receive the alert, triage it and respond to the attack before the attackers achieve their goals. The requirements to ensure a quick detection and remediation of the threat are commonly grouped into a **Service Level Agreement** which is a document signed between the internal SOC team and it's company management, or by the managed SOC provider (MSSP) and it's customers. The agreement usually requires quick threat detection (measured by MTTD), timely alert acknowledgement by L1 analsyts (measured by MTTA) and finally prompt response to the threat, like isolating the device or securing the breached account (measured by MTTR).
![Image showing the typical Detection, Acknowledgement and Response time of a SOC team](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1746642255233.svg)

## Reference Table

The metrics are often used to evaluate your performance, and good results lead to career growth and a raise to more senior positions like L2 Analyst.
Some issues and recommendations to fix the issues:
1. **False Positive Rate over 80%**: Try to exclude trusted activities like system updates from your EDR or SIEM detection rules or consider automating alert triage for most common alerts using SOAR or custom scripts.
2. **Mean Time to Detect over 30 min**: Try to contact SOC engineers to make the detection rules run faster or with a higher rate or check if SIEM logs are collected in real-time, without a 10-minute delay.
3. **Mean Time to Acknowledge over 30 min**: Try to ensure the analysts are notified in real-time when a new alert appears or try to envenly distribute alerts in the queue between the analysts on shift.
4. **Mean Time to Respond over 4 hours**: Try to make everything possible to quickly escalate the threats to L2 or ensure your team has documented what to do during different attack scenarios.