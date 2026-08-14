# The Boogeyman1

Julianne, a finance employee working for Quick Logistics LLC, received a follow-up email regarding an unpaid invoice from their business partner, B Packaging Inc. Unbeknownst to her, the attached document was malicious and compromised her workstation.
The security team was able to flag the suspicious execution of the attachment, in addition to the phishing reports received from the other finance department employees, making it seem to be a targeted attack on the finance team. Upon checking the latest trends, the initial TTP used for the malicious attachment is attributed to the new threat group named Boogeyman, known for targeting the logistics sector.

## Investigation Guide

1) Start by analysing the dump.eml file located in the artefacts directory. There are two ways to analyse the headers and rebuild the attachment:
    - The manual way uses command-line tools such as cat, grep, base64, and sed. Analyse the contents manually and build the attachment by decoding the string located at the bottom of the file.
    - **My preferred approach**: An alternative and easier way to do this is to double-click the EML file to open it via Thunderbird. The attachment can be saved and extracted accordingly.
2) Once the payload from the encrypted archive is extracted, use `lnkparse` to extract the information inside the payload: `lnkparse <LNK FILE>`

# (Endpoint Security) Are you sure that's an invoice?

Based on the initial findings, we discovered how the malicious attachment compromised Julianne's workstation:

- A PowerShell command was executed.
- Decoding the payload reveals the starting point of endpoint activities. 

## Investigation guide

1) Using the initial findings, search the execution of the initial payload in the powershell logs.
2) Since the given data is JSON, we can parse it in CLI using the `jq` command


## JQ Cheatsheet

jq is a lightweight and flexible command-line JSON processor. This tool can be used in conjunction with other text-processing commands. 

- **Parse all JSON into beautified output**: `cat powershell.json | jq `
- **Print all values from a specific field without printing the field**: `cat powershell.json | jq '.Field1'`
- **Print all values from a specific field**: `cat powershell.json | jq '{Field1}'`
- **Print values from multiple fields**: `cat powershell.json | jq '{Field1, Field2}'`
- **Sort logs based on their Timestamp**: `cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'`
- **Sort logs based on their Timestamp and print multiple field values**: `cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[] | {Field}'`

