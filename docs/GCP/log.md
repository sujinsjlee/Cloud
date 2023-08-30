# Logging and Monitoring in the Cloud
> In Google's Site Reliability Engineering book, which is available to read at `landing.google.com/sre/books`, monitoring is defined as: "Collecting, processing, aggregating, and displaying real-time quantitative data about a system, such as query counts and types, error counts and types, processing times, and server lifetimes." 

## SLI, SLOs, SLAs
- **Service level indicators** or SLIs, are carefully selected monitoring metrics that measure one aspect of a services reliability. I

- A **service level objective**, or SLO, combines a service level indicator with a target reliability.

- **service-level agreements** or SLAs, which are commitments made to your customers that your systems and applications will only have a certain amount of downtime. An SLA describes the minimum levels of service that you promised to provide to your customers and what happens when you break that promise. If your service has paying customers, an SLA might include some way of compensating them with refunds or credits when that service has an outage that is longer than this agreement allows.

## Monitoring tools
>  Cloud monitoring provides visibility into performance, uptime, and overall health of Cloud powered applications. It collects metrics, events, and metadata from projects, logs, services, systems, agents, custom code, and various common application components

## Logging tools
> Cloud Logging allows users to collect, store, search, analyze, monitor, and alert on log entries and events.

## Error reporting and debugging tools 
- **Error Reporting** counts, analyzes, and aggregates the crashes in your running cloud services. Crashes in most modern languages are “Exceptions,” which aren’t caught and handled by the code itself. 

- **Cloud Trace**, based on the tools Google uses on its production services, is a tracing system that collects latency data from your distributed applications and displays it in the Google Cloud console.

- **Cloud Profiler** changes this by using statistical techniques and extremely low-impact instrumentation that runs across all production application instances to provide a complete CPU and heap picture of an application without slowing it down. 