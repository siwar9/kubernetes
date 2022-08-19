## Basics for Prometheus

- Alerting
- Monitoring

**Main Component :** Prometheus Server (does the actual monitoring work!)

The Prometheus Server has the following elements:
- **Time Series Database :** which stores metrcis data like current CPU usage or number of exceptions in a notification.
- **Data Retrieval worker :** responible for pulling those metrics from applications, services, and servers or other target resources and pushing them into the database (time series database).
-**PromQL :** a web server or a server API that accepts queries for that stored data and then displays this data in a dashboard either through a Prometheus dashboard or other data virtualization tool like Grafana.