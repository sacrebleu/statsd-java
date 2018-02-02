statsd-java
===========

### Version 1.0.15

A lightweight, concurrent, threaded [Statsd](https://github.com/etsy/statsd) client for use in Java or JRuby applications which require only basic or custom instrumentation.

### Description

`statsd-java` is intended to be a lightweight statsd-compliant client which is able to send statsd instrumentation messages via UDP to a statsd server. It makes use of a single internal thread and a Concurrent Queue in order to buffer
data for dispatch asynchronously to the dispatch of this data.

### Protocol format

`statsd-java` delivers messages via UDP in the following format: 

    app_name.bucket,tag1=v1,tag2=v2...tagn=vn:<measurement>|<units>

Messages are sent in `ISO-8859-1` and an attempt is made to santise app and bucket names to prevent strange timeseries in influxdb

It complies with the [Influx Statsd enhancements](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/statsd#influx-statsd) as supported by influx import services such as Telegraf and
[currencycloud-statsd-influxdb-backend](https://www.npmjs.com/package/currencycloud-statsd-influxdb-backend)

### Usage

        Map<String,String> configuration = new HashMap<String,String>();
        configuration.put("statsd.host", "localhost");
        configuration.put("statsd.port", "8080");

        // create and obtain an instance
        Statsd instance = Statsd.getInstance(configuration);
        
        Map<String,String> tags = new HashMap<String,String>();
        
        tags.put("hostname", "machine-hostname");
        tags.put("service", "service-name");
        
        // increment a counter
        instance.incrementCounter("funkyapp", "test_counter", tags, 1);
        
        // disconnect, cleanly close the thread and ensure any unsent buffered data is sent
        instance.disconnect();
        

