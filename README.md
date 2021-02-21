# KAA:  Collecting-data-from-a-device-Kaa-Cloud-Platform
learned some additional concepts of the Kaa platform and discover how to:  collect telemetry data from a device transform raw telemetry data into well-structured time series use the time series auto-extraction feature visualize time series data on the Kaa UI.

##Data sample
Think of data sample as a block of data in JSON format that a client sends to the platform for data collection.

Let’s consider an example.

You have a greenhouse monitoring station that consists of temperature, humidity and light sensors, and a gateway (e.g. Arduino) that all sensors are connected to. Every minute Arduino polls all connected sensors for their current values. Then the Arduino formats sensor values into a JSON object: this is a data sample.

#########DATA SAMPLE IN JSON################
{
  "temperature": 25,
  "humidity": 46,
  "lux": 8600
}

##Time series
Time series is a named sequence of data points. Each data point contains a timestamp and one or more named values. A set of value names and their types (numeric, string, boolean) defines a time series.

You may want different time series defined for various things. For example, a fuel level time series may only have one numeric value: level. On the other hand, a geo-location time series may have several numeric values: latitude, longitude, and in some applications altitude.

You can configure Kaa to transform data samples received from endpoints into time series for displaying them on charts, gauges, maps, etc. The microservice responsible for extracting data points from data samples, storing and retrieving them, is the Endpoint Time Series service (EPTS). It can use a timestamp present in a data sample or use a data sample receipt timestamp. For simplicity, in this tutorial we will use data receipt timestamps.

Also, EPTS has an auto-extraction feature that stores each numeric top-level data sample field into a separate time-series. All auto-extracted time series have a name that follows the pattern auto~<field name> and one numeric value with the name value. So, if your endpoint sends data a sample with two fields, e.g.:
  
  {
    "temperature": 23,
    "humidity": 48
  }
  
  and the auto-extraction feature is enabled, EPTS creates two time-series: auto~temperature and auto~humidity.
  
###Enable the time series auto-extraction
To keep things simple, at this time we will not define any configurable time series, but rather just use the EPTS auto-extraction feature. Let us make sure that it is turned on in your application. Go to the Device management -> Applications -> expand your application -> select “epts” and enable the “Autoextract” checkbox.

With this function enabled, Kaa will automatically create a time series for each numeric field it encounters at the root of data samples your endpoints in this application will submit. You will then be able to view these time series in Kaa UI, no extra configuration required.

Go to the pre-defined endpoint dashboard and scroll to the “Device telemetry” widget. (The endpoint dashboard is available from Device management -> Devices -> select your endpoint in the list.) Now we are all set to send our first data sample to the platform.

####Send data samples
You can submit telemetry data via a variety of MQTT- and HTTP-based interfaces. Let’s take a look at a couple of the simplest ones: plain HTTP and plain MQTT.
