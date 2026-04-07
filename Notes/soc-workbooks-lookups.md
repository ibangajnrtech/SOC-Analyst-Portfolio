# Assert & Identities

There are some questions that need to be answered about some alerts that involve a user. This could help to correctly triage an alert and understand if the alert is a False positive or a True Positive.

### Identity Inventory

This is a catalogue of corporate employees (user accounts), services (machine accounts), and their details like privileges, contacts, and roles within the company.

### Sources of Identities

1. **Active Directory**: AD itself is an identity database, and it is commonly used by SOC. Example: On-prem AD, Entra ID.
2. **SSO Providers**: Cloud alternative for AD, an easy way to manage and search the users. Example: Okta, Google Workspace.
3. **HR Systems**: Limited to employees only, but usually provides full employee data. Example: BambooHR, SAP, HiBob.
4. **Custom Solution**: It is common for IT or security teams to maintain their own solutions. Example: CSV or Excel Sheets.

### Asset Inventory

Also called Asset Lookup, is a list of all computing resources within an organisation's IT environment. 

## NetWorking Diagrams

Sometimes an alert will have to be looked at from a network point of view especially in bigger companies.

## Workbooks Theory

With asset inventory and network diagrams, you can get enough context about the user, host or IP address. 
Next thing to do is to make a verdict about whether the activity is safe or not.
Some alert analysis may be very simple while some may require dozens of essential steps that must be followed to avoid missing vital details and confusing attack evidence.

### SOC Workbooks

Also called playbooks, runbooks, or workflow is a structured document that defines the steps required to investigate and remediate specific threats efficiently and consistently
1. **Enrichment**: Use Threat Intelligence and identity inventory to get information about the affected user.
2. **Investigation**: Using the gathered data and SIEM logs, make your verdict if the login is expected.
3. **Escalation**: Escalate the alert to L2 or communicated the login with the user if necessary.