## Basics for Prometheus
Prometheus is an open-source systems monitoring and alerting toolkit. Monitoring targets, e.g: Linux/windows server, single app, apache server...
Prometheus works well for recording any **purely numeric time series**. It fits both machine-centric monitoring as well as monitoring of highly dynamic service-oriented architectures. In a world of microservices, its support for **multi-dimensional** data collection and querying is a particular strength.

Prometheus is designed for reliability, to be the system you go to during an outage to allow you to quickly diagnose problems.

Prometheus values reliability. You can always view what statistics are available about your system, even under failure conditions. If you need 100% accuracy, such as for per-request billing, Prometheus is not a good choice as the collected data will likely not be detailed and complete enough.

**Main Component :** Prometheus Server (does the actual monitoring work!)

The Prometheus Server has the following elements:
- **Time Series Database :** which stores metrcis data like current CPU usage or number of exceptions in a notification.
- **Data Retrieval worker :** responible for pulling those metrics from applications, services, and servers or other target resources and pushing them into the database (time series database).
-**PromQL :** a web server or a server API that accepts queries for that stored data and then displays this data in a dashboard either through a Prometheus dashboard or other data virtualization tool like Grafana.

For each target, Prometheus serveils specific **units**. For a Linux server it could be CPU status, Memory/Disk space usage. For an app it could be number of excpetions/requests, or request durations.

A unit you would like to monitor for a specific target is called **metric**. And a metric is what gets into Prometheus database component. Prometheus provide human-readable text based format for these metrics. Metrics entries(data) has **HELP** or **TYPE** attributes to increase its readability.
- HELP : The description of chat the metrics is
- TYPE : There are 3 types ; **Counter** for frequency (how many times x happened?), **Gouge** for value (what is the value of x now), and **Histogram** for details (how long or large?)

## How does Prometheus collect those metrics from targets?

