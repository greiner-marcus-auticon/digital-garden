---
title: OpenTelemetry
feed: show
---

## TLDR

How to instrument telemetry

![[assets/Untitled.png]]

Recap

![[assets/Untitled 1.png]]

## Notes

### **Three signals**

- Traces
  - Relational graph visualizing a business transaction
  - Across multiple services and subsystems
  - Chain of spans which include attributes and events
  - Propagation through standardized protocol
- Logs
  - Unified log data model
  - Support for distributed context propagation
  - Standardized log correlation
- Metrics
  - Work with existing metrcis protocols and standards
  - Migration path for OpenCensus users
  - Ability to correlate metrics to other signals

Mutliple signals can give us more insight into our system

Make us better spies at understanding what is going on inside the system

### Which one when?

It depends

![[assets/Untitled 2.png]]

### Where to start?

- start at the edges of the system
- where inter-service communication occurs (e.g. between HTTP client, [ASP.NET](http://ASP.NET) Core, SQL Client, AWS, Azure SDK, Elastic, HangFire, Entity Framework Core etc.)
- work your way inward, step by step

![[Untitled 3.png]]

### When is it too much?

**High-quality instrumentation**

- alignment in the organization and team
- **who will benefit from the telemetry data**
- which signals are udes and how are they used?
- is there room to introduce new signals? new tools?
- who will be looking at the telemtry information?

**Observability strategy**

- **what purpose will the telemetry serve?**
  - failure investigation
  - performance insights
  - system behaviour
- **what questions should the telemetry be able to answer?**

**Observability questions to ask**

- can you easily connect a user to your telemetry
- what types of behaviour do you want to group?
- can you identify the most load generating operations?
- are you able to debug this from the telemetry alone?
- can you find suspicious events throughout the system?

### **Observability guidelines**

- [ ] which signals are available to us?
- [ ] which scenarios best fit each signal?
- [ ] do we need more than one signal? Why?
- [ ] when is it enough?

### W**hat best practices should be considered?**

**Handling error scenarios**

- [ ] capture any possible failure scenario
- [ ] signal failures in the observability backend
- [ ] include error information
- [ ] consider adding additional information in case of failure
- [ ] instrumentation shouldn’t cause business failures

**consider context propagation**

- [ ] create baggage, which is a standard to propagate context across services
- [ ] check for different signals

**use with care**

- [ ] check impact on latency due to added data load
- [ ] check what will exist on every hop after it is added

**sensitive information**

- [ ] keep it out
  - [ ] personally identifiable information
  - [ ] financial or healthcare data
  - [ ] passwords, connection or infrastructure information
  - [ ] **do code reviews to keep all sensitive information out**
- [ ] encrypt, tokenize data, mask and redact

**processors**

- [ ] manipulate data before it is exported to the backend
- [ ] enrich or filter
- [ ] different types of processors per telemetry signal
- [ ] beware if performance & latency impact

**telemetry correlation**

- [ ] each individual signal provides value
- [ ] connecting the signals further strengthens observability
- [ ] reduces mean time to resolve
- [ ] connect logs and traces through TraceID and SpanID
- [ ] consider capturing TraceID in exemplars

**consider cardinality explosion**

![[assets/Untitled 4.png]]

**Additional best practices**

![[assets/Untitled 5.png]]

### What do you really need?

- i want to be able to debug any request in the system
  - observe all
- i want to debug system-wide problems
- i want to understand the baseline behaviour
- i want to understand the system architecture

### Choose random samples

**Head sampling**

- decide at the beginning whether to sample or not
- root span makes the decision
- default: parent-based with TraceIDRatioBased sampler
- main benefit is unbiased sampling
- remember: spans are still created, not recorded

**Tail sampling**

- collect all spans until the trace completes
- defer decision to the end of the trace

    based on:

  - overall trace duration
  - status
  - span attributes’ values

**Which sampling strategy?**

![[assets/Untitled 6.png]]

**OpenTelemetry Collector**

A mediation layer between your application and your observability backend.

- vending-agnostic implementation
- receive, process and export telemetry data
- centralizes configuration and management
- offloads risk of failures & handles error scenarios
- allows scaling per signal

Agents and gateway deployment model

![[assets/Untitled 7.png]]

**Which to choose**

- be mindful of how you define skill
- do not overdo it from the beginning
- look at what data is coming in
- adjust for the skill that you actually have, not for the skill that you think you will at some point have
- add complexity as you need it

### Sources

[JetBrains .NET Day Online '23 - OpenTelemetry - Starts at around 1:05:22 / 11:22:22](https://www.youtube.com/watch?v=8ddzYsZVIYY&t=6699s)

[talks/how-to-effectively-spy-on-your-systems at main · lailabougria/talks (github.com)](https://github.com/lailabougria/talks/tree/main/how-to-effectively-spy-on-your-systems)

[The Best (and Worst) Reasons to Adopt OpenTelemetry - DevOps.com](https://devops.com/the-best-and-worst-reasons-to-adopt-opentelemetry/)

[Getting Started | OpenTelemetry](https://opentelemetry.io/docs/instrumentation/net/getting-started/)

[[Links]]
