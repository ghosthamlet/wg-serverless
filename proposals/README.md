# Proposals

Use this folder to submit proposals for new work streams related to the
CNCF Serverless Working Group.

To submit a new proposal, open a PR adding a new document to this
"proposals" folder and add an agenda item to the Serverless WG's
[agenda](https://docs.google.com/document/d/1OVF68rpuPK5shIHILK9JOqlZBbfe91RNzQ7u_P7YCDE/edit?ts=5a1da559#)
document to raise aware of the PR.

If the work stream extends the scope of the Working Group beyond what the
TOC has previously agreed to, then the request might need to be taken to
the TOC after the Working Group has agreed to it.

## Proposed Work Streams

### Events

At this moment the group decided to move forward with `open-events`
leveraging `Cloudevents` and `Open-CADF-events` content as input. The
on-going work for the current spec is hosted at
[https://github.com/cloudevents/spec](https://github.com/cloudevents/spec).

### Workflows / Function Composition

Many serverless applications are not a simple function triggered by a single event, instead they are composed of a function workflow/graph with events and functions interleaved together. 

A user needs a standard way to specify their serverless use case workflow. For example, one use case could be "do image enhancement and then face recognition on a photo when a photo is uploaded onto the cloud storage (photo storage event happens)." Another IoT use case could be “do motion analysis” when a motion detection event is received, then depending on the result of the analysis function, either “trigger the house alarm plus call to the police department” or just “send the motion image to the house owner.” 

A user’s workflow may involve both events and functions. For example, in a workflow, the user can specify what combination of events trigger what functions, those functions are executed in sequence or in parallel, what information is passed from one function to the next function, whether the next step function execution needs to wait for another event to happen. 

Some information discussed in the CloudEvents, such as the [correlation id](https://github.com/cloudevents/spec/pull/128), is associated with a usecase workflow and do need to be specified in the workflow specification. While we wrok on workflow specification, we might find out that some attributes are missing in the CloudEvents. 

### Event Orchestration / Chaining

The way events are orchestrated and chaining functions were deemed to be not in scope for CloudEvents, but maybe in scope for the Serverless working group.

There are issues such as [event history/ chaining](https://github.com/cloudevents/spec/issues/204), [event nesting](https://github.com/cloudevents/spec/issues/72) that do need this to be defined. There are increasing number of question regarding who is allowed to modify a CloudEvent, how an event is forwarded and so on.

### Function Signatures

There are multiple providers that have different ways to handle functions. Using multiple providers, switching providers and developing functions would be significantly better experiences if there was a common structure for function signatures.

Some examples can be seen in [function-signatures-examples.md](function-signatures-examples.md) but the same issues are present in other supported languages too.

### APIs for accessing CloudEvents

### CE Client SDK

See: https://github.com/cloudevents/spec/issues/205


### Common function logging, observing, and monitoring 

Functions generate logs which are stored in the underline platform (e.g. Kubernetes logs, AWS Cloud watch, Azure App insight, elastic search..). each serverless platform has its own way of writing to a log. If we had a common way/api for logging it could have made functions portable AND allow simple integration between log services and function platform providers.

See proposal details in [function logging](function-logging.md)

In addition to logs standardizing and integrating other APIs for custom metrics counting and tracing can simplify developer work. 

### Common function model 

Each platform today has its own function spec file/API which describe the desired function resources, environment variables, triggers, etc [e.g. nuclio function spec doc]( https://github.com/nuclio/nuclio/blob/master/docs/reference/function-configuration/function-configuration-reference.md). This means deploying function on a new platform require adapting your deployment scripts or logic every time you shift providers.

It is even a greater burden in many cases since the function configuration may depend on external resources such as databases, API gateways, message queues, etc.

some efforts like AWS SAML or Serverless.com tried to deliver higher level abstraction which has a potential of delivering a common/cross-platform model, yet the such efforts may require participation from the platform providers and agreement on such a common model.

### Common Serverless Benchmark framework 

Users often try to compare Serverless frameworks on performance, which is faster and in what use-case, rather than having each vendor define their own benchmark which may be biased towards their own implementation it would be great to have a common standard like [SPECvirt]( https://en.wikipedia.org/wiki/SPECvirt) or [YCSB (NoSQL Benchmark)]( https://en.wikipedia.org/wiki/YCSB).

Performance benchmarks may include aspects of throughput, latency, scalability, cost/performance, cold/warm start, etc. There may be various use cases with different performance behaviors such as small HTTP requests, stream processing, image processing (each may have different bottleneck between network, data, CPU).

Nuclio team made a small step in that direction with simple request latency & throughput benchmark which can be used to benchmark various serverless platforms [see the link]( https://github.com/nuclio/nuclio/blob/master/docs/tasks/benchmarking.md)

