# Introduction to SPL

Splunk is a powerful Security Information and Events Management (SIEM) platform that allows analysts to search, visualize, and investigate log data. Its **Search Processing Language** (SPL) is the key to turning large volumes of data into practical results.

# Search & Reporting

Splunk's **Search & Reporting App** is the default interface used for searching and analyzing data on the Splunk home page.

Key functionalities found when accessing the app:
1. **Search Head**: Where analysts put the queries to filter or aggregate log data.
2. **Time Picker**: Provides multiple options to select the timeframe of your search
3. **Search History**: Saves Splunk's search queries that have previously been used
4. **Data Summary**: Provides a summary of the hosts, sources and sourcetypes available

## Fields Sidebar

This can be found on the left panel of Splunk Search. This sidebar features two sections: one displays selected fields, and the other highlights interesting fields. It also provides quick results, including top values and raw values for each field.
1. **Selected Fields**: The default extracted fields. You can select other fields by clicking them and toggling **_selected_**
2. **Interesting Fields**: Pulls all the interesting fields it finds and displays them in the left panel to further explore
3. **Numeric Fields #**: This symbol represents fields that contain numerical values
4. **Alpha-Numeric Fields α**: The alpha symbol represents fields that contain strings (text values)
5. **Count**: The number of events containing the listed field
6. **More available fields**: If more fields are available, they can be accessed and selected here.

# Search Operators

Splunk **Search Processing Language** (SPL) is behind every search in Splunk. It combines commands, functions and operators that allow you to filter, transform and analyze date from your ingested logs. 

## Free Text Search

The simplest way to use SPL is to use free-text search such as the query below:
`index=windowslogs alice`

Splunk Operators are the building blocks used to construct any search query. These operators are used to filter, remove and refine your search results based on the specified criteria.

### Relational Operators

These operators are used to compare two expressions. They determine the relationship between the expressions, such as whethere one is equal to, not equal to greater than, or less than the other. Examples:
- **Equals`=`**: Search for all events which the field name is equal to the value.
- **Not Equal To `!=`**: Search for all events in which the field name is not equal to the value.
- **Less Than `<`**: Search for all events in which the field is less than the value.
- **Less Than or Equal To `<=`**: Search for all events in which the field is less than or equal to the value.
- **Greater Than `>`**: Search for all events in which the field is greater than the value.
- **Greater Than or Equal To `>=`**: Search for all events in which the field is greater than or equal to the value.

### Logical Operators

Splunk supports the following logical operators, which can be used to connect or modify conditions and operate on Boolean values (true/false):
- **`NOT`**: Returns events where the field and it's specified value does not exist.
- **`AND`**: Returns events in which the two fields and their specified values exist.
- **`OR`**: Returns events in which any of the two fields and their specified values exist.
- **`IN`**: A more convenient alternative to the `OR` keyword, especially for long lists.

### Wildcards and CIDR Search

Splunk supports the use of wildcards and CIDR search for IP addresses to search for a partial or IP subnet match. Examples:
- **`*`**: `status=*fail*` returns all events that have `status` field set to `failed`, `failure`, `appfail`, etc.
- **`*`**: `DestinationIp=172.*` returns events that contain values like `DestinationIp=172.90.0.0.1` or `DestinationIp=172.18.5.22`.
- **`N/A`**: `DestinationIp=172.18.0.0/16` returns events where `DestinationIp` field is within the `172.18.0.0/16` subnet.

## Order of Evaluation

### Quotes

In Splink, quotation marks `""` are used to define exact phrases or strings. You can wrap text in quotes, and Splunk will treat it as a single value. Quotes can also be used to escape search operators. Example:
- `index=windowslogs failed login`: Search for events with **failed** and **login** keywords, in any order.
- `index=windowslogs "failed login"`: Search for the exact phrase **"failed login"**, word order matters.
- `index=windowslogs "TO BE OR NOT TO BE"`: Search for the exact phrase containing **NOT** and **OR**.

### Parentheses

You can utilize parenthese in Splunk to help group conditions together and control how the search is applied. Since **OR** operator takes precendence over **AND**, parentheses can help set the correct order of conditions.

# Filtering Results

In Splunk, commands are linked together using a pipe symbol **|**. Each pipe passes the output of one command into the next, allowing you to refine your results step by step.

## Useful Filtering Commands

### Fields

The fields command is used to include or exclude specific fields from your search results. To exclude a field, use a minus sign before the field name. The plus sign can be used to include a field explicitly, but it isn't required. By default, fields includes any fields listed after the command.

### Dedup

The dedup command removes duplicate values from your search results. For example, if our logs contain seven distinct IP addresses in the SourceIp field, the results will return seven events, one for each unique IP.

### Rename

The rename command allows you to change the name of a field in your search results. This can help improve the readability of your search results, especially if the original fields are too long or not suitable for showing them in the screenshots in formal SOC reports. Also useful to flatten JSON or XML subfields.

### Regex

The regex commands allows you to filter search results using regular expressions, which match specific text patterns in field values. This is useful when you need to find events that follow a specific format rather than an exact keyword. Splunk regular expressions are PCRE (Perl Compatible Regular Expressions) and use the PCRE C library.

# Structuring Results

The Search Processing Language (SPL) provides further commands that help organise and structure your search results.

## Table Command

The table command allows you to select only the fields you are interested in viewing and displays them in a clean, readable format.

# Useful Structuring Commands

Other commands can be used alone or combines with `table` to hone in on the data you're really interested in.

- **`head`**: `index=windowslogs | head 20` returns the first (newest) 20 events. Useful to speed up the search if you don't need complete results.
- **`tail`**: `index=windowslogs | tail 20` returns the last (oldest) 20 events. Useful to speed up the search if you don't need complete results.
- **`sort`**: `index=windowslogs | sort User` will sort the logs in alphabetical order based on the field `User`.
- **`reverse`**: `index=windowslogs | reverse` reverses the order of events (descending order).

# Timelining With Table

The table command can be used to create timelines that help analysts visualise how events unfolded. By organising key fields, we can resconstruct the sequence of actions that occurred on a system.

# Subsearches

With Splunk subsearches and `join` keyword, you can correlate across multiple data sources within one search.