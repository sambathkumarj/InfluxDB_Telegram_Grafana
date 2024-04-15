# InfluxDB_Telegram_Grafana

# Metrics Collection USING TELEGRAF WITH INFLUX DB

# Telegraf 

Telegraf is a server-based agent for collecting and sending all metrics and events from databases, systems, and IoT sensors. Telegraf is written in Go and compiles into a single binary with no external dependencies, and requires a very minimal memory footprint.

# 4 Types of Telegraf plugins

These are the four basic plugin types: Input, Output, Processor, and Aggregator.

# 1. Input
Telegraf Input Plugins collect metrics from the system, services, and 3rd party APIs.

# 2. Process
Processor Plugins transform, decorate, and filter metrics before they are sent allowing your data to be sanitized as it arrives.

# 3. Aggregate
Aggregator Plugins create aggregate metrics, for example the average mean, minimum, and maximum from the metrics you have collected, and processed.

# 4. Output
Output Plugins write to a variety of datastores, services, & message queues, like InfluxDB, Graphite, OpenTSDB, Datadog, Kafka, MQTT, NSQ, and others.

# Telegraf plugins

Integrations to a variety of metrics, events, and logs from popular containers and systems.
Telegraf also has output plugins to send metrics to a variety of other datastores, services, and message queues, including InfluxDB, Graphite, OpenTSDB, Datadog, Librato, Kafka, MQTT, NSQ, and many others.
Seamlessly connect with 300+ plugins to the platforms that make sense to you.


# Installation steps for Telegraf 

Commands to install telegraf in host machine
```
wget -q https://repos.influxdata.com/influxdata-archive_compat.key

echo'393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null

echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list

sudo apt-get update && sudo apt-get install telegraf

```

# InfluxDB – Time Based DataBase

InfluxDB is an open-source time series database (TSDB) developed by the company InfluxData. It is used for storage and retrieval of time series data in fields such as operations monitoring, application metrics, Internet of Things sensor data, and real-time analytics.

InfluxDB offers several benefits for storing and analyzing time series data. It supports efficient data aggregation and analysis with time interval constraints, making it suitable for IoT environments and other systems that collect data continuously over time
Install InfluxDB

# Install InfluxDB as a service with systemd
```
curl -O https://dl.influxdata.com/influxdb/releases/influxdb2_2.7.5-1_amd64.deb

sudo dpkg -i influxdb2_2.7.5-1_amd64.deb
```
   
Start the InfluxDB service:
```
       sudo service influxdb start
```
       
       Installing the InfluxDB package creates a service file at /lib/systemd/system/influxdb.service to start InfluxDB as a background service on startup.
       To verify that the service is running correctly, restart your system and then enter the following command in your terminal:
```
sudo service influxdb status
```
  
  Pass configuration options to the service
You can use systemd to customize InfluxDB configuration options and pass them to the InfluxDB service.
Edit the/etc/default/influxdb2 service configuration file to assign configuration directives to influxd command line flags–for example, add one or more <ENV_VARIABLE_NAME>=<COMMAND_LINE_FLAG> lines like the following:
```
       ARG1="--http-bind-address :8087"
       ARG2="—storage-wal-fsync-delay=15m"
```    

   Edit the /lib/systemd/system/influxdb.service file to pass the
   variables to the ExecStart value:

```    
       ExecStart=/usr/bin/influxd $ARG1 $ARG2
```

# Set up InfluxDB
      
As you get started with this tutorial, do the following to make sure everything you need is in place.
Run the initial setup process
Create an All Access API token
Configure authentication credentials
Create a bucket
      
   Run the initial setup process.
    After you install and start InfluxDB, run the initial setup process to create the following:
    ◦ An organization with the name you provide.
    ◦ A bucket with the name you provide.
    ◦ An admin authorization with the following properties:
            ▪ The username and password that you provide.
            ▪ An API Operator token.
            ▪ Read-write permissions for all resources in the InfluxDB instance.
   You can use the InfluxDB UI, the influx CLI, or the HTTP API to run the setup process.
   ◦ To run an interactive setup that prompts you for the required information, use the InfluxDB user interface (UI) or the influx command line interface (CLI).
   ◦ To automate the setup–for example, with a script that you write– use the influx command line interface (CLI) or the /api/v2/setup InfluxDB API endpoint.
       
   # Create an All Access API token.
  During the InfluxDB initial set up process, you created an admin user and Operator token that have permissions to manage everything in your InfluxDB instance.
       While you can use your Operator token to interact with InfluxDB, we recommend creating an All Access token that is scoped to an organization, and then using this token to manage InfluxDB.
    InfluxDB UIinflux CLIInfluxDB API
     ◦ Visit localhost:8086 in a browser to log in and access the InfluxDB UI.
     ◦ Navigate to Load Data > API Tokens using the left navigation bar.
     ◦ Click + GENERATE API TOKEN and select All Access API Token.
     ◦ Enter a description for the API token and click SAVE.
     ◦ Copy the generated token and store it for safe keeping.
       
    Create a bucket.
      In the initial setup process, you created a bucket. You can use that bucket or create one specifically for this getting started tutorial. All examples in this tutorial assume a bucket named get-started.
       Use theInfluxDB UI, influx CLI, or InfluxDB API to create a new bucket.
      InfluxDB UIinflux CLIInfluxDB API
      ◦ Visit localhost:8086 in a browser to log in and access the InfluxDB UI.
      ◦ Navigate to Load Data > Buckets using the left navigation bar.
      ◦ Click + CREATE BUCKET.
      ◦ Provide a bucket name (get-started) and select NEVER to create a bucket with an infinite retention period.
      ◦ Click CREATE.

# Line protocol elements

Each line of line protocol contains the following elements:
* Required
    • *measurement: String that identifies the measurement to store the data in.
    • tag set: Comma-delimited list of key value pairs, each representing a tag. Tag keys and values are unquoted strings.Spaces, commas, and equal characters must be escaped.
    • *field set: Comma-delimited list key value pairs, each representing a field. Field keys are unquoted strings.Spaces and commas must be escaped.Field values can be strings (quoted), floats, integers, unsigned integers, or booleans.
    • Timestamp: Unix timestamp associated with the data. InfluxDB supports up to nanosecond precision. If the precision of the timestamp is not in nanoseconds, you must specify the precision when writing the data to InfluxDB.
    •  Example – Input and Output Plugins:
      [[inputs.exec]]
      commands = ["python3 /etc/telegraf/custom_script/log_capture/amf-array-1.py"]
      timeout = "10s"
      data_format = "json"
      
      [[outputs.influxdb_v2]]
      urls = ["http://192.168.1.1:8086"]
      token = ""
      organization = "Netcon"
       bucket = "Telegraf"

# Python Script 

To run the Python script as a input, In telegraf we can use EXEC as input plugins,
Python script should have permission like, 
```
chmod 777 <filename.py>
chmod +x <filename.py>
```
and location should be etc/telegraf/customscripts/<filename.py>

# Set environment variables

In the Telegraf configuration file (/etc/telegraf.conf):

[agent]
  interval = "30s"
  round_interval = true
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  collection_jitter = "0s"
  flush_interval = "30s"
  flush_jitter = "5s"
  precision = ""
  debug = false
  quiet = true
  logfile = ""
 
[[inputs.exec]]
  commands = ["python3 /etc/telegraf/custom_script/log_capture/amf-array-1.py"]
  timeout = "10s"
  data_format = "json"

[[outputs.influxdb_v2]]
  urls = ["http://192.168.1.1:8086"]
  token = "SeTyO4LYasGO86Cwes2A1Z-11soPhpLcZN6-KpTQhhD5fD1bODNoFWQSqFoMTBKqdIu6dCxbQ_mcRrwzTLkUBA=="
  organization = "Netcon"
  bucket = "Telegraf"

# After editing the telegraf.conf file restart the telegraf service :

```
sudo systemctl stop telegraf.service 
sudo systemctl start telegraf.service
sudo systemctl status telegraf.service
journal -u telegraf.service -f
telegraf --test
```

Check whether the telegraf status without any error.

To check the collected logs, In the InfluxDB UI ,In the buckets section <Bucket – name> and  plugins – name (Exec) click and expand the measurement field, then you can view the logs collected from the telegraf.


# InfluxDB Data Source – Grafana

# Configure the data source
To configure basic settings for the data source, complete the following steps:
    1. Click Connections in the left-side menu.
    2. Under Your connections, click Data sources.
    3. Enter InfluxDB in the search bar.
    4. Select InfluxDB.
       The Settings tab of the data source is displayed.
    5. Set the data source’s basic configuration options carefully:
        
Influxdb : http://<192.168.1.1>:8086
Query Language : Flux
 
Basic auth -> With Credentials
Username  : Netcon
Passwd : Netcon

Organisation : <Netcon>
Bucket name : <Telegraf>
Token : “ ” -> <From Influx DB UI>

Finally, Click save and test to store the Data source of InfluxDB in the Grafana.

InfluxDB – Grafana_log Management

Create a new Dashboard and use InfluxDB as Data source, Flux language is required to view the data in the database  , we can get query from the Influx DB UI By clicking the Scrpit Editor to get  query and copy & paste in the Grafana dashboard. You can see view the logs from the InfluxDB and Finally click save and Apply option to create the Dashboard panel.



As a result, you can see the Above images as a output which Data is collected from the InfluxDB.
