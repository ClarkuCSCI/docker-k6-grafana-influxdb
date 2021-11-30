# docker-k6-grafana-influxdb
Demonstrates how to run load tests with containerized instances of [k6](https://k6.io), [Grafana](https://grafana.com) and [InfluxDB](https://www.influxdata.com).

## Getting Started
First, ensure that you have Docker [installed](https://docs.docker.com/get-docker/) and running on your computer.

Next, start the containers in [the csci220-django project](https://github.com/ClarkuCSCI/csci220-django). We will be load testing <http://127.0.0.1:8080/minifacebook/>, so **be sure you can load that page** before continuing. Also, ensure that the page shows users' statuses: if you don't see any statuses, you should run `add_fb_bots.py`, as described in the "Query Processing and Optimization" lab.

Next, start the [InfluxDB](https://www.influxdata.com) and [Grafana](https://grafana.com) containers by running:
```
docker compose up -d influxdb grafana
```
These containers will record and display metrics on load testing, respectively. 

Open <http://localhost:3000/d/k6/k6-load-testing-results> to view the graph dashboard. Since you haven't yet run any tests, the dashboard won't yet contain any data.

Finally, run the load testing script:
```
docker compose run --rm k6 run /scripts/minifacebook.js
```
As the script runs, results will appear in the graph dashboard.

## Article
This is the accompanying source code for [this article](https://medium.com/swlh/beautiful-load-testing-with-k6-and-docker-compose-4454edb3a2e3). Please read for a detailed breakdown of the code and how K6, Grafana and InfluxDB work together using Docker Compose.

## Dashboards
The dashboard in `/dashboards` is adapted from [this k6 dashboard](https://grafana.com/grafana/dashboards/2587).

There are only two small modifications:
* The data source is configured to use the docker created InfluxDB data source
* The time period is set to now-15m, which I feel is a better view for most tests
