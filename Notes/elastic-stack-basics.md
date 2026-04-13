# Introduction to Elastic Stack

Elastic Stack (ELK) was originally developed to store, search and visualize large amounts of data. 
Elastic Stack is a collection of different open-soruce components that work together to collect data from any source, store and search it and visualize it in real time.

## Core Components

1. **Elasticsearch**: Elasticsearch is a full-text seach and analytics engine for JSON-formatted documents. It stores, analyzes and correlates data and supports a RESTful API for interacting with it.

2. **Logstash**: Logstash is a data processing engine that takes data from different sources, filters it or normalises it, and then sends it to the destination, which could be kibana or a listening port. A logstash configuration file is divided into three parts, which are:
    - The **input** part is where the user defines the source from which the data is being ingested.
    - The **Filter** part is where the user specified the filter options to normalise the log ingested above.
    - The **Output** part is where the user wants the filtered data to be sent. It can be a listening port, Kibana Interface. Elasticsearch database, or file.
Logstash supports manu Input, Output and Filter plugins.

3. **Beats**: Beats are host-based agents known as data-shippers that ship/transfer data from the endpoints to Elasticsearch. Each beat is a single-purpose agent that sends specific data to Elasticsearch. 

4. **Kibana**: Kibana is a web-based data visualization tool that works with Elasticsearch to analyse, investigate and visualise data streams in real time. It allows users to create multiple visualisations and dashboards for better visibility.

### How they work together:

- **Beats**: collects data from multiple agents. For example, Winlogbeat collects Windows event logs, and Packetbeat collects network traffic flows.
- **Logstash**: collects data from beats, ports or files, parses/normalises it into field value pairs, and stores them into Elasticsearch.
- **Elasticsearch**: acts as a databse used to search and analyse data.
- **Kibana**: is responsible for displaying and visualizing the data stored in Elasticsearch. The data stored in Elasticsearch can easily by shaped into different visualizations, time charts, infographics, etc using Kibana.

# Discover Tab

This is where the SOC analysts spend most of their time. It shows the ingested logs, the search bar, normalized fields andmore. Analysts can search for the logs investigate anomalies and apply filters based on seearch terms and time periods.

1. **Logs**: Each row shows a single log containing information about the event, along with the fields and values found in that log.
2. **Fields Pane**: The left panel of the interface shows the list of fields parsed from the logs. We can click on any field to add it to the filter or remove it from the search.
3. **Index Pattern**: Each type of log is stored in a different index pattern. We can select the index pattern from which we need the logs. For example, for VPN logs, we would need to select the index pattern in which VPN logs are stored.
4. **Search Bar**: It is a place where the user adds search queries and apples filters to narrow down the results.
5. **Time Filter**: We can narrow down results based on any specific time duration.
6. **Time Interval**: This chart shows the event counts over time.
7. **TOP Bar**: This bar contains various options to save the searchm open the saved searches, share or save the search, etc.
8. **Discover Tab**: This is the main workspace in Kibana for exploring, searching and analyzing raw data.
9. **Add Filter**: We can apply filters to specific fields to narrow down results, rather than manually typing entire queries.

## Index Pattern

Kibana requires an index pattern to access the data stored/ingested in Elasticsearch. The index pattern tells Kibana which elasticsearch data we want to explore.

## Fields Pane

The left panel in the Discover tab shows the list of the normalized fields it finds in the available logs.

## Time Filter

This time time filter allows us to apply a log filter based on time.

## Timeline

The timeline pane provides an overview of the number of evets that occurred for the time/date.

## Create Table

By default, the logs are shown in raw form. We can click on any log and select important fields to create a table showing only those fields. This method reduces the noice and makes it more presentable and meaningful.

# KQL Overview

KQL (Kibana Query Language) is a search query language used to search the ingested logs/documents in Elasticsearch.
With KQL, we can search for the logs in two different ways.
- Free text search
- Field-based search

## Free text Search

It allows users to search for logs based on text only. That means a simple search of the term **security** will return all the documents that contain this term, irrespective of the field.
KQL allows the wildcard **`*`** to match parts of the word.

## Logical Operators (AND|OR|NOT)

KQL also allows users to utilize logical operators in the search query.

1. **AND Operator**: Returns all events that have both values search together.
2. **OR Operator**: Returns all events that have either of the values searched.
3. **NOT Operator**: Removes a particular term from the search results. 

## Field-based search

We will provide the field name and the value we are looking for in the logs. This search has a special syntax as `Field: Value`. It uses a colon as a separator between the field and the value.

# Creating Visualizations

The visualization tab allows us to visualize the data in different forms such as tables, pie charts, bar charts, etc. This visualization task willuse the multiple options this tab provides to create some simple presentable visualizations.

## Create Visualization

One way to navigate the visualization tab is to click on any field in the discover tab and click on the visualization. We can create multiple visualizations by selecting options like tables, pie charts, etc.

## Correlation Option

Dragging the required field in the middle will create a correlation tab in the visualization tab.

Steps to take after creating visualizations:
- Create a visualization and click the save button at the top right corner.
- Add the title and description to the visualizatoin.
- Click Save and add to the library when it's done.

# Creating Dashboards

Dashboards provide good visibility into log collection. A user can create multiple dashboards to fulfill a specific need.

## Creating a Custom Dashboard

- Go to the `Dashboard tab` and click on the `Create dashboard`.
- Click on `Add from Library`.
- Click on the visualizations and saved searches. It will be added to the dashboard.
- Once the items are added, adjust them accordingly.
- Don't forget to save the dashboard after completing it.