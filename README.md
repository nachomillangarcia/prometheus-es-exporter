Prometheus Elasticsearch Exporter
====
This Prometheus exporter periodically runs configured queries against an Elasticsearch cluster and exports the results as Prometheus gauge metrics.

Values are parsed out of the Elasticsearch results automatically, with the path through the JSON to the value being used to construct metric names.

Metrics are only extracted from aggregation results, with the exception of the query doc count. The keys of any buckets are converted to labels, rather than being inserted into the metric name.
See `tests/test_parser.py` for all the supported queries/metrics.

# Installation
You will need Python 3 and pip 3 installed to run the exporter.

Run the following in the root project directory to install (i.e. download dependencies, create start script):
```
> pip3 install .
```
Note that you may need to add the start script location (see pip output) to your `PATH`.

# Usage
Once installed, you can run the exporter with:
```
> prometheus-es-exporter
```
Configuration is provided by an `exporter.cfg` file in your working directory. See the provided file for configuration examples and explanation.

# Docker
You can build a docker image using the provided Dockerfile. Run the following in the root project directory:
```
> sudo docker build -t prometheus-es-exporter .
```
To run a contain successfully, you will need to mount a `exporter.cfg` to `/usr/src/app/exporter.cfg`. You probably also need to expose the port configured in your config file. For example:
```
> sudo docker run --rm --name exporter \
    -v <path to exporter.cfg>:/usr/src/app/exporter.cfg \
    -p 8080:8080 \
    prometheus-es-exporter
```

# Development
The exporter can be installed in "editable" mode, using pip's `-e` flag. This allows you to test out changes without having to re-install.
```
> pip3 install -e .
```
To run tests (as usual, from the root project directory), use:
```
> python3 -m tests.test_parser
```
Note that these tests currently only cover the response parsing functionality - there are no automated system tests as of yet.

Send me a PR if you have a change you want to contribute!
