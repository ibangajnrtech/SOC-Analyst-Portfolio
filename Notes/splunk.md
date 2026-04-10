# Intro to Splunk

Splunk is one of the leading SIEM solutions in the market. It allows users to collect, analyse and correlate network and machine logs in real time.

# Splunk Components

Splunk has three main components: Forwarder, Indexer and Search Head. 

## Splunk Forwarder

Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance. It does not affect the endpoint's performance as it takes a few resources to process.
Some key data sources are:
- Web Server generating web traffic
- Windows machine generating Windows Event Logs, Powershell, and Sysmon data
- Linux host generating host-centric logs
- Database generating DB connection requests, responses and errors

The forwarder collects the data from the log sources and sends it to the Splunk Indexer.

## Splunk Indexer

Splunk Indexer plays the main role in processing the data it receives from forwarders. It parses and normalises the data into field-value pairs, categorises it, and stores the results as events, making the processed data easy to search and analyse.
Now, the data which is normalized and stored by the indexer, can be searched by the Search Head.

## Search Head

Splunk Search Head is the place within the **Search & Reporting App** where users can search the indexed logs. The searches are done using the **SPL** (Search Processing Language), a powerful query Language for searching indexed data. When the user performs a search, the request is sent to the indexer, and the relevant events are returned as field-value pairs.

The Search Head also allows you to transform results into presentable tables and visualizations such as pie, bar and column charts.

# Navigating Splunk

## Splunk Bar

The top panel is the **Splunk Bar**.
The Splunk bar has the following options available:
- **Messages**: View system-level notifications and messages
- **Settings**: Configure Splunk instance settings
- **Activity**: Review the progress if search jobs and processes
- **Help**: View tutorials and documentation
- **Find**: Search across the App

The Splunk Bar, allows users to switch between installed Splunk apps instead of using the Apps panel.

## Apps Panel

This panel shows the apps installed for the Splunk instance. The default app for every Splunk installation is **Search & Reporting**.

## Explore Splunk

This panel contains quick links to add data to the Splunk instance, add new Splunk apps and access the Splunk documentation.

## Splunk Dashboard

By default, no dashboards are displayed. You can choose from a range of dashboards readily available within your Splunk instance.