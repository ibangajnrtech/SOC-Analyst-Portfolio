# Tool Overview

Wireshark can be used for the following purposes:
- Detecting and troubleshooting network problems, such as network load failure points and congestion.
- Detecting security anomalies, such as rogue hosts, abnormal port usage, and suspicious traffic.
- Investigating and learning protocol details, such as response codes and payload data.

## GUI and Data

At first glance, the following sections stand out in a wireshark GUI:
1) **Toolbar**: The main toolbar contains multiple menus and shortcuts for packet sniffing and processing, including filtering, sorting, summarising, exporting and merging. 

2) **Display Filter Bar**: The main query and filtering section.

3) **Recent Files**: List of the recently investigated files. You can recall listed files with a double-click. 

4) **Capture Filter and Interfaces**: Capture filters and available sniffing points (network interfaces).  The network interface is the connection point between a computer and a network. The software connection (e.g., lo, eth0 and ens33) enables networking hardware.

5) **Status Bar**: Tool status, profile and numeric packet information.

# Packet Dissection

Packet dissection is also known as protocol dissection, which investigates packet details by decoding available protocols and fields. Wireshark supports a long list of protocols for dissection.

## Packet Details

You can click on a packet in the packet list pane to open its details (double-click will open details in a new window). Packets consist of 5 to 7 layers based on the OSI model.

