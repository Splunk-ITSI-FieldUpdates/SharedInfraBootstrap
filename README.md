Shared Infrastructure Bootstrap 
![alt text](https://splunk.box.com/v/sharedinfrabootstrap)
https://splunk.box.com/v/sharedinfrabootstrap


Questions? reach out to Martin Wiser on slack or by mwiser@splunk.com

TLDR:   
1.	Go here - https://github.com/Splunk-ITSI-FieldUpdates/SharedInfraBootstrap
2.	Download the Shared Infra bootstrap(s) 
3.	In ITSI 3.0.x, create a restore job and select the bootstrap
4.	If the bootstrap in question provides base searches, edit them to ensure the index= pieces are correct for your environment.
5.	Use the KPIs, services, etc!
 
Background:
Splunk's field organizations are busy bees that help make our products better by fine tuning out-of-the-box content or by building net new capabilities. These "enhancements" are useful to everyone so we decided to open-source them.

The Shared Infrastructure Bootstrap contains common IT/Tech services and service dependencies that we encounter at customer sites. These common services power many business services but rather than building them yourself you can just download and install them and configure them for your environment.

Bootstraps are essentially ITSI backup files, tailored to suit a specific need. Today these include:
•	OS Template (for NIX and Windows)
•	Shared IT Infrastructure
•	VMware (coming soon)
•	ITSI Healthchecks
•	more to come.

Bootstraps are intended for a one-time load into ITSI, then a tailoring to suit the customer’s needs.  Do not try to “upgrade” an environment by uploading a newer version of a bootstrap that’s already in use.  

Shared IT Infrastructure:
Roughly 45% of all P1 issues are caused by issues outside of the application stack.  Instead, they are caused by a failure somewhere in the Shared IT Infrastructure – the large collection of services provided by different groups within the IT organization.   

This bootstrap provides a service dependency tree for modeling the Shared IT Infrastructure as a whole.

Bootstrap Contents: 
Multiple services, in a dependency tree and using examples of entity filtering.  Also includes a CIO-level glass table, and save Service Analyzer view.

Requirements: 
This bootstrap has no specific dependencies, although entity filtering examples use ITSI roles as that are populated automatically by scheduled searches.  For the roles to be populated correctly, one must configure data collection per the ITSI Modules Documentation.

Initial Configuration:
•	Download the bootstrap, create a restore job, restore the bootstrap.
•	Review the installed service dependency tree, disable or remove services that are not necessary within the deployed environment.

The service dependency tree includes 30 discrete areas that are probably already monitored in some way, perhaps by different tools for different departments.  The next step is to prioritize which areas will be instrumented.   

Review critical issues (P0s, P1s and major P2s) from the past 6-9 months, and determine root cause for each.  
•	The functional area where the problem occurred will help you prioritize which of the services you will instrument using metrics, which you will instrument using alerts / Event Analytics, and which can be ignored for now.  
•	The root causes for each case will guide you to specific KPIs that would help with root cause analysis during the next outage.

 
Integrate with existing alerts: 
This tree includes 30 discrete areas that are probably monitored in some way already.   That data may or may not be in Splunk, but the goal here will be to get that data into Splunk and then tie one or more service-specific KPIs to that alert data.  Configuring your alerting system(s) to send alerts to Splunk are covered in detail elsewhere, and not included in this document.

For each of the discrete services in the ITSI “Shared IT Infrastructure” tree corresponding to areas where alerts are being generated:
1.	Configure the alerts to come to Splunk, into a Splunk index not tied to ITSI.
2.	Create a correlation search to normalize the information in the alerts, and to save these as ITSI Notable Events.
3.	Within the associated service, create a KPI counting the number of recent alerts.
4.	In the new KPI’s thresholding, set zero alerts to “normal”, set more than zero to “high”.
5.	Modify the Health Score calculation, setting the importance for the alerting KPI to ‘11’
6.	Optional but recommended: 
a.	Create a correlation search to process and normalize external alerts, storing them back to ITSI as Notable Events.
b.	Create a Notable Event Aggregation Policy for those specific events.

For areas of the ITSI “Shared IT Infrastructure” tree where alerts are not available, remove the Heartbeat KPI.   This will change these services from green to gray, indicating that there is an unmonitored dependency in the environment.

Integrate with OS metric data: 
Some areas of the infrastructure may be monitored at the OS level, including Active Directory, as well as network services such as DNS / DNS / NTP and the systems providing the SMTP backbone.   

For each of these services,
1.	Edit the service in ITSI
2.	Set the Entity Filtering page to match the correct hosts
3.	Use the OS monitoring approach from the OS Bootstrap to create KPIs in this service corresponding to OS metrics.
4.	Edit the Health Score calculation, review / edit the Importance level of critical KPIs.
5.	Save your configuration

Integrate with Service-specific metric data: 
For each of the services related to a recent major outage, 
1.	Identify the root cause, and the data sources where that issue would have been seen.  Ensure this data is in Splunk, or onboard that data.
2.	Edit the service in ITSI
3.	Set the Entity Filtering page to match the correct entities sending the data.
4.	Create KPIs to track the root causes of issues.
5.	Edit the Health Score calculation, review / edit the Importance level of critical KPIs.
6.	Save your configuration



Using the Shared IT Infrastructure Bootstrap:
Once the configuration work above is complete, you can leverage the artifacts created by this bootstrap in the following ways:

Shared IT Infrastructure Health Score
You now have a service that is monitoring across the siloed stacks in the CIO organization.  This health score can be referenced by any other services, allowing business application stacks to tie to the Shared IT Infrastructure as a dependency.

IT Infrastructure Health glass table.  
Think of this as a CIO-level view, an at-a-glance view of current state across all stacks of the IT environment.   
•	Edit this page to remove any services you deleted.  Add services or KPIs as needed.
•	When creating future glass tables, add a widget for “Shared IT Infrastructure” when appropriate, and tie that widget to this glass table.   This will help in troubleshooting, perhaps avoiding the next P1-driven warroom.
 

Service Analyzer View
This bootstrap includes a saved Service Analyzer view for “IT Infrastructure”.  This view may be used in a NOC environment to view health of the Shared IT Infrastructure over time.

Switch to the Tree view, and now you have a real-time, automatically-generated view of the Shared IT Infrastructure and all of its dependencies.  

