1 .  Introduction
    1.1 Background
As the world of digital content grows, trying to read, find, or analyse any information becomes difficult and time-consuming. Need to spend a huge amount of time to process information. To solve this problem, some solutions developed like search engines, designed to help decrease the time required to find information and the amount of information which must be consulted.

    1.2 Formulation of the problem
How the system can monitor the processes are running?
How the system can detect the most errors during the process by using the Elastic Stack?
    1.3 Aim of Document
This document serves as detailed design document for Log Monitoring Framework using Elastic Stack to monitor if something happened to production environment, group the error, and show the daily report

    1.4 Intended Audience
This document is intended for software architects and developers as basis for detailed technical designs of the Elastic Stack

    1.5 Assumptions
Table 1‑1: Assumptions
![image](https://github.com/user-attachments/assets/9640e795-a21a-463d-ae34-73f226f24191)

1.7 Related Document
Table 1‑3: Related Document

Documents

Location

The Complete Guide to The ELK Stack

https://logz.io/learn/complete-guide-elk-stack/

Kibana Guide

https://www.elastic.co/guide/en/kibana/current/index.html

Cross-Cluster Search

https://www.elastic.co/guide/en/kibana/6.7/management-cross-cluster-search.html
Using Cross-Cluster Search	https://www.elastic.co/guide/en/elasticsearch/reference/6.7/modules-cross-cluster-search.html#CO371-1
Space Kibana	 https://www.elastic.co/guide/en/kibana/6.7/spaces-managing.html
Remote Clusters	 https://www.elastic.co/guide/en/kibana/6.7/working-remote-clusters.html
Grok Pattern

https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html

Default Grok Pattern

https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns

Experiment Building Patterns on Logs

http://grokdebug.herokuapp.com/

Explanation Grok Constructor

http://grokconstructor.appspot.com/

Reguler Expression on Grok

https://github.com/kkos/oniguruma/blob/master/doc/RE

=======

2 . Solution Overview
     2.1 Solution Approach

![image](https://github.com/user-attachments/assets/de25564a-5f65-4b18-86e9-f296c303f2ae)


 "ELK" is the acronym for three open source projects: Elasticsearch, Logstash, and Kibana. Elasticsearch is a search and analytics engine. Logstash is a server-side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. Kibana lets users visualize data with charts and graphs in Elasticsearch.

Elastic Stack contains of Beats, ElasticSearch, Logstash, and Kibana. In our case, we will use filebeat for data collection.

1. log file records or activities performed on the system
2. Filebeat comes with internal modules (auditd, Apache, NGINX, System, MySQL, and more) that simplify the collection, parsing, and visualization of common log formats down to a single command. They achieve this by combining automatic default paths based on your operating system, with Elasticsearch Ingest Node pipeline definitions, and with Kibana dashboards. Plus, a few Filebeat modules ship with preconfigured machine learning jobs
3. ElasticSearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores the data.
4. Logstash is an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to ElasticSearch
5. Kibana visualize the ElasticSearch data and navigate the Elastic Stack so we can do anything from tracking query load to understanding the way requests flow through the applications.

=======

  3.2 Kibana

  1. Discover
Find search data by keyword, have access to every document that matches the index pattern inputted, can filter search results. If the time field is configured for the selected index pattern, the distribution of documents from time to time is displayed in a histogram at the top of the page.

![image](https://github.com/user-attachments/assets/faa890c4-fdba-4437-b5fd-ad0e487168cd)

   Picture Explanation

create a new search sheet.
save search results that have been made.
save search results that have been made.
split search results by CSV report and Pemalinks
check file search details.
filter data based on fields entered and operators.
to choose which index pattern to look for.
do a search based on the desired time.
if clicked it can show the table structure and json.
if the cursor is directed to the appropriate part of the image, an add will appear, adding can enter the field into the search.
search by query.
to refresh the page that has been changed.

2. Visualize
a place to process data into visualization with several models, namely graphics, data and maps, the data entered into the visualization is obtained from elasticsearch.

![image](https://github.com/user-attachments/assets/3d1a9549-bc86-4ed4-9c8f-ac8e94fd8e5c)
 Picture Explanation

Area display data based on line graphs.
Control to control interactive and manipulation of the dashboard.
Coordinate Map to display the location based on the coordinates of the destination.
Data Table displays visual data in tabular form.
Gauge to measure the value of the metric associated with the value of the reference.
Goal chart shows how close you are to your final goal.
Heat Map graphical representation of the data in which individual values ​​are contained.
Horizontal Bar displays the graph horizontally.
Line displays graphs based on line curves.
Maps thematic maps where the boundary vector shapes are colored using a color gradient of a higher intensity indicating a greater value, and a color of a lower intensity.
Markdown reduction in the price of the text entry field, Kibana renders the text you entered in this field and displays the results on the dashboard.
Metric Visualization metric displays a single number for each aggregation you choose.
Pie The size of a pie chart slice is determined by metric aggregation. The following aggregations are available for this shaft.
Region Map display metrics on thematic maps. use one of the base maps provided or add your own dark color that represents a higher value.
TSVB time series data visualizer that allows you to use the full power of the Elasticsearch aggregation framework. With TSVB, you can combine an unlimited amount of aggregation to display complex data.
Tag Cloud visual representation of text data shown in font size or color the font size for each word is determined by the metric aggregation.
Timelion time series data visualizer that allows you to combining truly independent data sources in one visualization, driven by expression language you use to retrieve time series data.
Vega You can build graphical visualizations using queries as desired.
Vertical Bar displays the graphic vertically.

   3. Dashboard

Collection of visualizations, searches, and maps, in real time, provides fleeting insights into data and allows you to browse details, for dashboard layout Can be arranged as desired.

![image](https://github.com/user-attachments/assets/771e8419-5a00-4ffe-b00a-0b8bb276625b)
   Picture Explanation

save the dashboard results that have been made.
cancel all activities carried out on the dashboard.
adding data that you want to enter on the dashboard will appear when the panel is clicked and can choose the type of data you want to enter on the dashboard.
adjust the position on the dashboard.
split the results of the dashboard created.
filter data based on fields entered and operators.
to set the layout on the dashboard.
to adjust, edit, visual panel.
do a search based on input query.
choose a search based on the desired time.

 4. Canvas
Is the visualization and presentation of data that is inside Kibana. With Canvas, you can pull data directly from Elasticsearch, and combine data with colors, images, text and imagination as you wish, such as creating dynamic, multi-page, and pixel-perfect displays. With Canvas.

![image](https://github.com/user-attachments/assets/0037aa85-bbd4-4e92-bf7a-c1295d108f4b)
   Picture Explanation

Control settings can choose the interval you want to use.
to refresh the sheet on the canvas.
display the screen to full screen.
zoom in on the canvas screen.
share canvas files in pdf & json format.
to lock the worksheet on the canvas.
caption name on canvas.
Select and insert an image on the canvas.
Add elements to the canvas such as graphics and images.
to arrange the elements inserted on the canvas sheet.

 7. Infrastructure
Application allows you to monitor your infrastructure and identify problems in real time. You start with a visual summary of your infrastructure where you can see basic metrics for servers, containers, and general services. Then you can search to see more detailed metrics or other information for that component.

![image](https://github.com/user-attachments/assets/b516415e-9157-4f5a-8050-51e5e74cdb1c)

    Picture explanation

View an inventory of your infrastructure by hosts, Kubernetes pod or Docker containers. You can group and filter the data in various ways to help you identify the items that interest you.
View current and historic values for metrics such as CPU usage, memory usage, and network traffic for each component. The available metrics depend on the kind of component being inspected.
Use Metrics Explorer to group and visualize multiple customizable metrics for one or more components in a graphical format. You can optionally save these views and add them to dashboards.
Seamlessly switch to view the corresponding logs, application traces or uptime information for a component.

 8. Logs
For servers, containers, and general services the Log has a compact display, such as a console that you can customize, can filter logs by various fields, start and stop live streaming, and highlight interesting text.

![image](https://github.com/user-attachments/assets/b27b88ff-20cf-4fea-a3b9-5a3b9e0e9f40)
      Picture Explanation

Configuration = the data to use for your logsedit Are you using a custom index pattern to store the log entries? Do you want to limit the entries shown or change the fields displayed in the columns? If so, configure the logs source data to change the index pattern and other settings.
Customize = Click Customize to customize the view. Here, you can set the scale to use for the minimap timeline, choose whether to wrap long lines, and choose your preferred text size.
Click the time selector 05/23/20197:58:35PM to choose the timeframe for the logs. Log entries for the time you specify appear in the middle of the page, with the earlier entries above and the later entries below.
Click Stream live to start streaming live log data, or click Stop streaming to focus on historical data. When you are viewing historical data, you can scroll back through the entries as far as there is data available. When you are streaming live data, the most recent log appears at the bottom of the page. In live streaming mode, you are not able to choose a different time in the time selector or use the minimap timeline. To do either of these things, you need to stop live streaming first

9. APM
Elastic Application Performance Monitoring (APM) automatically collects performance metrics and in-depth errors from within the application,APM at Kibana is equipped with a basic license, allowing developers to browse performance data for their applications and quickly find performance bottlenecks

![image](https://github.com/user-attachments/assets/4ad71ec7-82ca-486c-84ee-d9c8c1009e6e)

Picture explanation

Index patterns tell Kibana which Elasticsearch indices you want to explore. An APM index pattern is necessary for certain features in the APM UI, like the query bar. To set up the correct index pattern, simply click Load Kibana objects at the bottom of the Setup Instructions. After you install an Elastic APM agent library in your application, the application automatically appears in the APM UI in Kibana. No further configuration is required.  

10. Uptime
To help quickly identify, diagnose outages, connectivity problems in the network, monitor the status of network endpoints via HTTP / S, TCP, and ICMP. can explore the status of endpoints from time to time, browse to specific monitors, and easily see high-level portraits of the environment at any time.
![image](https://github.com/user-attachments/assets/e15f64f4-7e6c-4f93-83ef-dc62ef6202af)

Picture Explanation

This view is intended to quickly give you a sense of the overall status of the environment you’re monitoring, or a subset of those monitors. Here, you can see the total number of detected monitors within the selected Uptime date range. In addition to the total, the counts for the number of monitors in an up or down state are displayed, based on the last check reported by Heartbeat for each monitor.

12. Dev Tools
Contains development tools that you can use to interact with your data in Kibana, The Console allows you to interact with the REST API of Elasticsearch. Search Profiler The Query Profiler tool can turn this JSON output into easy-to-navigate visualization, allowing you to diagnose and debug requests that perform poorly faster. Grok Debugger grok pattern is supported in the process of ingesting the grok node and the grok Logstash filter.

![image](https://github.com/user-attachments/assets/b8fce8c2-94d8-400a-80c6-00783ea43701)
 Picture explanation

Console = Console enables you to interact with the REST API of Elasticsearch. You can,Send requests to Elasticsearch and view the responses ,View API documentation Get your request history
Search Profiler = Elasticsearch has a powerful Profile API which can be used to inspect and analyze your search queries. The response returns a large JSON blob, which can be difficult to analyze manually.The Query Profiler tool can transform this JSON output into a visualization that is easy to navigate, allowing you to diagnose and debug poorly performing queries much faster.
Grok Pattern = Grok is a pattern matching syntax that you can use to parse arbitrary text and structure it. Grok is good for parsing syslog, apache, and other webserver logs, mysql logs, and in general, any log format that is written for human consumption.

13. Stack Monitoring
The Kibana monitoring feature serves two separate purposes: To visualize monitoring data from all Elastic Stacks. You can see health and performance data for Elasticsearch, Logstash, and Beats in real time, and analyze previous performance. To monitor Kibana itself and to route the data to the monitoring cluster. If you enable monitoring in Elastic Stacks, every Elasticsearch node, Logstash node, Kibana instance.

![image](https://github.com/user-attachments/assets/7c0233cf-6f0f-4a30-94e5-a85058df6274)

=======================================
========================================

     6.2 Server Configuration
         6.2.1 FileBeat Configuration
        How to install FileBeat in each of server instance

Download and unzip file from https://www.elastic.co/downloads/beats/filebeat
Edit filebeat.yml configuration file and set output to Logstash IP + Port
Start the daemon by running sudo ./filebeat -e -c filebeat.yml
         6.2.2 LogStash Configuration
      How to install LogStash

Download and unzip file from https://www.elastic.co/downloads/logstash
Prepare a logstash.conf config file (Input : Beat Port and Output : Elasticsearch IP + Port)
Run bin/logstash
         6.2.3 ElasticSearch Configuration
   How to install ElasticSearch

Download and unzip file from https://www.elastic.co/downloads/elasticsearch
Run bin/elasticSearch
Default URL : localhost:9200
         6.2.4 Kibana Configuration
       How to install Kibana

Download and unzip file from https://www.elastic.co/downloads/kibana
Open config/kibana.yml in an editor
Set elasticSearch.hosts to point at your ElasticSearch instance
Set ElasticSearch Username & Password 
         Example

        elasticSearch.host:["http://<IP ElasticSearch>:<ElasticSearchPort>"]

        elasticSearch.username:"kibana"

        elasticSearch.password:"pass"

Run bin/kibana
Default URL : localhost:5601

6.2.5 UI Interface
            6.2.5.1 How to View Recognized/Unrecognized Log Pattern
Create Query based on specific rule
          Example : Get Total Database or Disk is Full ‘Error’ from attunity task

![image](https://github.com/user-attachments/assets/33702dce-d569-4ee1-bd4c-354a2a5348d0)
Click Visualize Icon on the left panel > Create new visualization >Choose Diagram Type
![image](https://github.com/user-attachments/assets/3546f6fc-841b-4fb3-b40b-72a135fc111c)
Click Visualize Icon on the left panel > Create new visualization >Choose Diagram Type

                  Example : Vertical Bar
![image](https://github.com/user-attachments/assets/9d5418ce-3daa-4c3a-ada7-c119d367e70b)
Choose Index

       Example : attunity-*                  
![image](https://github.com/user-attachments/assets/9537c542-b43f-432b-9750-7a6374ad2d99)
Insert query, X Axis, Y Axis setting
       Example :

       Query : message: *database or disk is full*

       Y-Axis : Count

       X-Axis : Date histogram
 ![image](https://github.com/user-attachments/assets/b29355be-6af7-45dd-a9b2-59df448fcac7)
 Save and give visualization’s name
 ![image](https://github.com/user-attachments/assets/583ecc6c-42de-46d3-836c-da7232321fe9)
Click Dashboard Icon on the left panel > Create New Dashboard > Click Add
![image](https://github.com/user-attachments/assets/5e0188b2-6e51-4f05-9fd3-358ecc1bfb87)
Save and give dashboard’s name
          Example : Attunity Error Dashboard
![image](https://github.com/user-attachments/assets/3e89ad22-3348-43f7-aa37-bbdc93b77d27)
===================

6.2.5.2 Implementation Kibana
                 1. Part How To Login on Kibana

![image](https://github.com/user-attachments/assets/4eb63179-fca2-4065-afac-2a95e0bcdb15)
![image](https://github.com/user-attachments/assets/d9129be7-e846-47f7-8bb7-54eab4e17446)
Picture Explanation

Enter the username that has been created
enter the password that was created
Select the destination management space

![image](https://github.com/user-attachments/assets/8d97affe-6dd5-4d68-89f9-92385914da41)
Picture Explanation

      4. Click the dashboard according to the picture

      5. Select the name of the dashboard you want to display
![image](https://github.com/user-attachments/assets/7d19baba-d5c3-4598-8e9e-23f9e2d031fa)
Spaces

               Spaces enable you to organize your dashboards and other saved objects into meaningful categories. Once inside a space, you see only the dashboards and saved objects that belong to that space. Kibana creates a default space for you. After you create your own spaces, you’re asked to choose a space when you log in to Kibana. You can change your current space at any time by using the menu in the upper left.
 2. Guideline Add Space Kibana             
  hover over the top left corner then choose manage space to make a new room at Kibana 
            Manage spaces → Create a space
![image](https://github.com/user-attachments/assets/dd8ce21a-bf5d-4910-8aa8-730aebd833f6)
![image](https://github.com/user-attachments/assets/d623738f-fb3f-40ad-8be4-6b1c434e931b)
 Picture Explanation
Name = to give a name to the manage space
Avatar = to select the color on the manange space icon and give initials as desired on managing space
Customize = customize the url as desired
scroll down Create space= make managing space
Cancel=cancel managing the space you want to make
![image](https://github.com/user-attachments/assets/ea082623-6293-4f7c-90ab-af1a2985cc4b)
Picture Explanation
to update managing space
delete data managing the space created permanently by entering the confirmation name managing space
if clicking delete space in the display will update the space following the method in number 2


display examples manage the space created
![image](https://github.com/user-attachments/assets/35f4f4a1-7bfd-401d-bdcd-48ce3552eb7d)
 3.  Guideline Add Remote Cluster Kibana 
                Managing remote cluster
                Remote clusters helps you manage remote clusters for use with cross-cluster search and cross-cluster replication. You can add and remove remote clusters and check their connectivity.

                Management > Elasticsearch > Remote clusters
 ![image](https://github.com/user-attachments/assets/a9d3ee58-09d8-45c5-ab2f-d278349ced15)
 ![image](https://github.com/user-attachments/assets/204cf12d-9460-43de-a511-941deba4d76e)
 ![image](https://github.com/user-attachments/assets/f4be95cc-c477-4716-97af-795d928fa65e)
 Picture Explanation
Click the icon in the left corner and bring it to the Kibana interface screen to create a remote cluster
then scroll down select management
select the remote cluster in the elasticsearch menu
Create a new remote cluster
Give the name on the remote cluster that you want to make an example in the picture with the name Payment
fill in the host port that is intended for the remote cluster
Save the results of the remote cluster that was created
remote clusters sign made successfully

         4. Guideline add remote cluster index pattern
create a new index pattern
![image](https://github.com/user-attachments/assets/3beb218e-8478-4afa-a9a2-98a7bb174317)
  2. contains cluster names and index pattern 

         Create your index pattern in Kibana with the convention <cluster-names>:<pattern>

         Example : cluster_payment:attunity-*

         Note : must be adjusted to the name of the remote cluster

     3. to the next process
![image](https://github.com/user-attachments/assets/4887f7ce-e14d-4a83-81ae-6116a7b7a26d)
 4. contains the filter you want to display

      5. make an index pattern

      6. Return to the previous menu
![image](https://github.com/user-attachments/assets/b6cefc43-2753-4475-9b6a-a0191ed1ffde)
 index pattern display successfully created
 ![image](https://github.com/user-attachments/assets/e0e581b2-34df-4a93-a9cb-a1d3d6518f74)
       discover-> search (search content that you want to search)

       show dates -> the time you want to display
![image](https://github.com/user-attachments/assets/877473b6-668d-4657-b28c-62a48b0a7c14)

       ================
       ================
            6.2.5.3 User Metrics For Kibana
                 Will be set based on user ID and role

                 Role will be mapped to several indexes / spaces with certain access level

                 User ID will be mapped to role ID

      6.3 Data Retention Period
         As initial configuration, data retention period will be configured to have 1 month data retention. The rest will be archived via housekeeping process using curator.



7 .  Appendix
    7.1 Beats
     text file Filebeat-attunity.yml



 text file Filebeat-cb1.yml



 text file Filebeat-cb2.yml



 text file Filebeat-cb3.yml



 text file Filebeat-cb4.yml



 text file Filebeat-cb5.yml



 text file Filebeat-cb6.yml



 text file Filebeat-cb7.yml



 text file Filebeat-cb8.yml



 text file Filebeat-flinkjm.yml



 text file Filebeat-flinktm1.yml



 text file Filebeat-flinktm2.yml



 text file Filebeat-kafka.yml


    7.2 Logstash

 text file Logstash.conf

 text file Logstash.yml
        

    7.3 ElasticSearch

 text file elasticsearch.yml



 text file jvm.options


    7.4 Local PV

 text file  local-volume-config-es

         
                   
      
