# Introduction to Logs

Logs are the digital footprints left behind by any activity. The activity could be a normal one or the one with malicious intent. Tracing down the activity and the individual behind the execution of that activity becomes easier through logs.

## Use Cases of Logs

1) **Security Events Monitoring:** Logs help us detect anomalous behavior when real-time monitoring is used.
2) **Incident Investigation and Forensics:** Logs are the traces of every kind of activity. It offers detailed information on what happened during the incident. The security team utilizes the logs to perform root cause analysis of incidents.
3) **Troubleshooting:** As the logs also record the errors in systems or applications, they can be used to diagnose issues and helpful in fixing them.
4) **Performance Monitoring:** Logs can also provide valuable insights into the performance of applications.
5) **Auditing and Compliance:** Logs play a major role in Auditing and Compliance, making it easier with its capability to establish a trail of different kinds of activities.

# Types of Logs

1) **System Logs:** The system logs can be helpful in troubleshooting running issues in the OS. These logs provide information on various operating system activities.
**Example:** System Startup and shutdown events, Driver Loading events, System Error events, Hardware events

2) **Security Logs:** The security logs help detect and investigate incidents. These logs provide information on the security-related activities in the system.
**Example:** Authentication events, Authorization events, Security Policy changes events, User Account changes events, Abnormal Activity events

3) **Application Logs:** The application logs contain specific events related to the application. Any interactive or non-interactive activity happening inside the application will be logged here.
**Example:**  User Interaction events, Application Changes events, Application Update events, Application Error events

4) **Audit Logs:** The Audit logs provide detailed information on the system changes and user events. These logs are helpful for compliance requirements and can play a vital role in security monitoring as well.
**Example:** Data Access events, System Change events, User Activity events, Policy Enforcement events

5) **Network Logs:** Network logs provide information on the network’s outgoing and incoming traffic. They play crucial roles in troubleshooting network issues and can also be handy during incident investigations.
**Example:** Incoming Network Traffic events, Outgoing Network Traffic events, Network Connection Logs, Network Firewall Logs

6) **Access Logs:** The Access logs provide detailed information about the access to different resources. These resources can be of different types, providing us with information on their access.
**Example:** Webserver Access Logs, Database Access Logs, Application Access Logs, API Access Logs

## Event IDs in Windows Operating System

1) **4624:** A user account successfully logged in
2) **4625:** A user account failed to login
3) **4634:** A user account successfully logged off
4) **4720:** A user account was created
5) **4724:** An attempt was made to reset an account’s password
6) **4722:** A user account was enabled
7) **4725:** A user account was disabled
8) **4726:** A user account was deleted