---
layout: post
title:  "Adding DataDog Monitoring"
date:   2016-10-12 11:26:48 +0100
categories: DataDog Monitoring
---
This document covers the standard procedure for using DataDog in your project and making
DataDog dashboards

# Adding monitoring

When adding monitoring instrumentation must be added to the services to be monitored. After this these should be added in the agentmanager.exe on the boxes they live on for all environments. For more guidance on this you will have to ask internally.

# Standard Monitoring For API's

#### Query Values

1. Average Response Time (24Hrs) Configurable by Environment
    * Error is set at >500ms, Warning at >350ms
 
    
2. Average Requests (24Hrs) Configurable By Environment
    * Error is set at *TBD*, Warning at *TBD*

#### Graphs

1. Average Response Time (24Hrs) Configurable by Environment Time Series
    * Error is set at >500ms, Warning at >350ms

    ![alt text](/images/timeseries_dd.jpg "Timeseries")    

# Standard Monitoring For Services

Services are currently monitored for if they are up or down.

# Monitoring Full Journeys

![alt text](/images/journey_temp_dd.PNG "DataDog Journey Board")

A journey refers to a route a customer can take through Likely Loans. In order to check on the health of each of these routes we currently make Datadog dashboards which include any alert that may suggest the journey is compromised.

Above is an image of the template in DataDog that can be cloned and adapted to suit a particular journey. A few aspects of it have markers on:

1) An image can be placed here representing the journey being monitored

2) The top row represents services which are essential both for the journey being monitored and the most basic application route. The second row represents services which are only needed for this particular journry. This section currently only shows the health of production, with OK (up) or ERR (down) statuses.

3) The rest of the board is for monitoring anything we should be alerting against. For instance, API's, services and databases would be shown here. Each of these would have monitors representing an aspect that would demonstrate problems in our systems in certain conditions. We only alert on what we can act on and what may pose a danger to our system or inconvenience to our customer. We do not monitor things on these boards as a matter of pure interest. For advise on what to monitor and alert on by our standard practices see the above sections.