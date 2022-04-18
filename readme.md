## Loud ML

Loud ML is an open source inference engine for metrics and events, and the fastest way to embed machine learning in your time series application. This includes APIs for storing and querying data, processing it in the background for ML or detecting outliers for alerting purposes, and more.  More information can be found [here](https://github.com/regel/loudml).


This block attempts to create a simple balena Block for inclusion in any project.  Please be aware that this block only runs the Loud ML service

## Usage

### docker-compose file

To use this image, create a container in your `docker-compose.yml` file as shown below:

```
version: "2.1"
volumes:
    loudml-data:
#
services:
  loudml:
    restart: always
    build: ./loudml
    volumes:
      - loudml-data:/var/lib/loudml
    ports:
      - 8077:8077
```

### config.yml

The config.yml tells Loud ML what data sources are available for use and can be found in ./loudml/etc.  The default is to use InfluxDB as both a source and destination for data, the bucket "balena" contains the information to be analyzed and the bucket "loudml" will contain he results.

```
buckets:
  - name: balena
    type: influxdb
    addr: influxdb:8086
    database: balena
    retention_policy: autogen
  - name: loudml
    type: influxdb
    addr: influxdb:8086
    database: loudnml
    retention_policy: autogen
    measurement: loudml
    annotation_db: loudmlannotations
storage:
  path: /var/lib/loudml

server:
  listen: 0.0.0.0:8077
```


## Accessing

By default the louadml runs an HTTP server on port `8077`, which will be accessible externally to the device.

You can also access the server via the command line within container:

```
$ loudml 
Connected to localhost:8077 version 1.6.0
Loud ML shell 1.6.3
>
```

## Heatlh Check

You can query the API from within the container to check the service is running properly:

```
$ curl http://localhost:8077

{"host_id":"86fe1a2bf19f89e83cee363f934d6bbe","tagline":"The Disruptive Machine Learning API","version":"1.6.0"}
```

## Using Loud ML

Follow the [getting started guide](https://loudml.io/en/loudml/reference/current/getting-started.html) to learn more about the basics of Loud ML and how to use it in other applications.