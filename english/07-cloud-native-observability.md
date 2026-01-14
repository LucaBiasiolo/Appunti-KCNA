# 07. Cloud Native Observability   
## Observability   
The term *observability* is closely related to the [control theory](https://en.wikipedia.org/wiki/Control_theory) which deals with behavior of dynamic systems. In essence, ***the control theory describes how external outputs of systems can be measured to manipulate the behavior of the system***.   
The higher goal of observability is to allow **analysis** of the collected data.   
   
## Telemetry   
The term *telemetry* has Greek roots and means remote or distance (tele) and measuring (metry).   
Measuring and collecting data points and then transferring it to another system is of course not exclusive to cloud native or even IT systems.   
In container systems, each and every application should have tools built in that generate information data, which is then collected and transferred in a centralized system. The data can be divided into three categories:   
- **Logs** messages that are emitted from an application when errors, warnings or debug information should be presented   
- **Metrics** quantitative measurements taken over time   
- **Traces** track the progression of a request while it's passing through the system   
   
   
## Logging   
Unix and Linux programs provide three I/O streams from which two can be used to output logs from a container:   
- standard input (stdin): Input to a program e.g. via keyboard   
- standard output (stdout): The output a program writes on the screen   
- standard error (stderr): Errors that a program writes on the screen   
   
Command line tools like **docker**, **kubectl** or **podman** provide a command to show the logs of containerized processes if you let them log directly to the console or to **/dev/stdout** and **/dev/stderr**.   
**$ docker logs nginx**   
Kubernetes provides the same functionality with the **kubectl** command line tool.   
**$ kubectl logs nginx**   
Other logging examples:   
Return snapshot of previous terminated ruby container logs from pod web-1
**kubectl logs -p -c ruby web-1**   
Begin streaming the logs of the ruby container in pod web-1
**kubectl logs -f -c ruby web-1**   
Display only the most recent 20 lines of output in pod nginx
**kubectl logs —tail=20 nginx**   
Show all logs from pod nginx written in the last hour
**kubectl logs —since=1h nginx**   
To ship the logs, different methods can be used:   
- **Node-level logging** (fluentd or filebeat)
The most efficient way to collect logs. An administrator configures a log shipping tool that collects logs and ships them to a central store.   
- **Logging via sidecar container** (fluentd or filebeat)
The application has a sidecar container that collects the logs and ships them to a central store.   
- **Application-level logging**
The application pushes the logs directly to the central store. While this seems very convenient at first, it requires configuring the logging adapter in every application that runs in a cluster.   
   
To make logs easy to process and searchable make sure you log in a structured format like JSON instead of plaintext.   
   
## Prometheus   
[Prometheus](https://prometheus.io/) is an open source monitoring system, originally developed at SoundCloud, which became the second CNCF hosted project in 2016.   
Prometheus can collect metrics that were emitted by applications and servers as time series data and provides **4 core metrics**:   
- **Counter** a value that increases, like a request or error count   
- **Gauge** values that increase or decrease, like memory size   
- **Histogram** a sample of observations, like request duration or response size   
- **Summary** similar to a histogram, but also provides the total count of observations   
   
To expose these metrics, applications can expose an HTTP endpoint under **/metrics** instead of implementing it yourself.   
Prometheus has built-in support for Kubernetes and can be configured to automatically discover all services in your cluster and collect the metric data in a defined interval to ***save them in a time series database***.   
To query data that is stored in the time series database, Prometheus provides a querying language called [PromQL (Prometheus Query Language)](https://prometheus.io/docs/prometheus/latest/querying/basics/).   
The ***most used companion for Prometheus is Grafana***, which can be used to build dashboards from the collected metrics.   
***Another tool from the Prometheus ecosystem is the [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)***. The Prometheus server itself allows you to configure alerts when certain metrics reach or pass a threshold.   
   
## Tracing   
A trace describes the tracking of a request while it passes through the services. A trace consists of multiple units of work which represent the different events that occur while the request is passing the system.   
These traces can be stored and analyzed in a tracing system like [Jaeger](https://www.jaegertracing.io/).   
The OpenTelemetry project is a set of application programming interfaces (APIs), software development kits (SDKs) and tools that can be used to integrate telemetry such as metrics, protocols, but especially traces into applications and infrastructures. The OpenTelemetry clients can be used to export telemetry data in a standardized format to central platforms like Jaeger.   
   
## Cost Management   
The possibilities of cloud computing allow us to draw from a theoretically infinite pool of resources and only pay for them when they are really needed.   
There are several ways to do automatic and manual optimization:   
- **Identify wasted and unused resources** with a good monitoring of your resource usage, it is very easy to find unused resources or servers that don’t have a lot of idle time. A lot of cloud vendors have cost explorers that can break down costs for individual services. Autoscaling helps to shut down instances that are not needed   
- **Right-Sizing** it can be a good idea to choose servers and systems with a lot more power than actually needed. Again, good monitoring can give you indications over time of how much resources are actually needed for your application. Don’t buy powerful machines if you only need half of their capacity   
- **Reserved Instances** on-demand pricing models are great if you really need resources on-demand. A method to save a lot of money is to reserve resources and even pay for them upfront   
- **Spot Instances** if you have a batch job or heavy load for a short amount of time, you can use spot instances to save money. The idea of spot instances is that you get unused resources that have been over-provisioned by the cloud vendor for very low prices   
   
   
