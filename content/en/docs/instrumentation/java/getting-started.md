---
title: Getting Started
description: Get telemetry from a client-server app in less than 5 minutes!
spelling: cSpell:ignore helloworld javaagent
weight: 1
---

In this page, you'll learn how to set up and get tracing telemetry from a simple
hello-world client-server example.

You'll work with gRPC's [Java Quick start example][], which uses gRPC to
communicate between the client and server. You can work through this page even
if you aren't familiar with gRPC.

## Get and run the example

First, get and run the hello-world example without instrumentation:

 1. [Get the example code.][]
 2. [Run the example:][] you should see the client output "Hello world".
 3.  Stop the server before proceeding, if it is still running.

## Run the instrumented example

Next, you'll use a [Java agent to automatically instrument](../automatic) the
client and server at launch time. While you can [configure the Java agent][] in
a number of ways, the steps below use environment variables.

 1. Download [opentelemetry-javaagent.jar][] from [Releases][] of the
    `opentelemetry-java-instrumentation` repo. The JAR file contains the agent
    and all automatic instrumentation packages. {{% alert color="info" %}}<i class="fas fa-edit"></i> Take note of the path to the JAR file.{{% /alert %}}
 2. Set and export variables that specify the Java agent JAR and a [console
    trace exporter,][] using a notation suitable for your shell/terminal
    environment &mdash; we illustrate a notation for bash-like shells:
    ```console
    $ export JAVA_OPTS="-javaagent:PATH/TO/opentelemetry-javaagent.jar"
    $ export OTEL_TRACES_EXPORTER=logging
    ```
    {{% alert title="Important" color="warning" %}}Replace `PATH/TO` above, with
    your path to the JAR.{{% /alert %}}
 3. Run the **server** as a background process. For example, for bash-like
    shells run:
    ```console
    $ ./build/install/examples/bin/hello-world-server &
    [otel.javaagent 2022-03-19 13:38:16:712 +0000] [main] INFO io.opentelemetry.javaagent.tooling.VersionLogger - opentelemetry-javaagent - version: 1.12.0
    Mar 19, 2022 1:38:18 PM io.grpc.examples.helloworld.HelloWorldServer start
    ...
    ```
    Note the output from the `otel.javaagent`.
 4. From the _same_ terminal, run the **client**:
    ```console
    $ ./build/install/examples/bin/hello-world-client
    [otel.javaagent 2022-03-19 13:38:48:462 +0000] [main] INFO io.opentelemetry.javaagent.tooling.VersionLogger - opentelemetry-javaagent - version: 1.12.0
    Mar 19, 2022 1:38:50 PM io.grpc.examples.helloworld.HelloWorldClient greet
    INFO: Will try to greet world ...
    [Trace output elided -- see below for details]
    Mar 19, 2022 1:38:51 PM io.grpc.examples.helloworld.HelloWorldClient greet
    INFO: Greeting: Hello world
    ```
 5. Stop the server process. For example, for bash-like shells run:
    ```console
    $ jobs
    [1]+  Running                 ./build/install/examples/bin/hello-world-server &
    $ kill %1
    ...
    *** server shut down
    ```

At step 4, you should have seen trace output from the server and client that
looks something like this (trace output is line-wrapped for convenience):

```sh
[otel.javaagent 2022-03-19 13:38:51:310 +0000] [grpc-default-executor-0] INFO
io.opentelemetry.exporter.logging.LoggingSpanExporter -
'helloworld.Greeter/SayHello' : 150407692a813ceb4e9f49a02c8b1fd0
ee15310978e6f6d3 SERVER [tracer: io.opentelemetry.grpc-1.6:1.12.0]
AttributesMap{data={net.transport=ip_tcp, rpc.grpc.status_code=0,
rpc.service=helloworld.Greeter, thread.id=16, rpc.method=SayHello,
rpc.system=grpc, net.peer.ip=127.0.0.1, thread.name=grpc-default-executor-0,
net.peer.port=44186}, capacity=128, totalAddedValues=9}

[otel.javaagent 2022-03-19 13:38:51:368 +0000] [main] INFO
io.opentelemetry.exporter.logging.LoggingSpanExporter -
'helloworld.Greeter/SayHello' : 150407692a813ceb4e9f49a02c8b1fd0
6ed56cd008f241c5 CLIENT [tracer: io.opentelemetry.grpc-1.6:1.12.0]
AttributesMap{data={net.transport=ip_tcp, net.peer.name=localhost,
rpc.grpc.status_code=0, rpc.service=helloworld.Greeter, thread.id=1,
rpc.method=SayHello, rpc.system=grpc, thread.name=main, net.peer.port=50051},
capacity=128, totalAddedValues=9}
```

## What next?

For more:

- Run this example with another [exporter][] for telemetry data.
- Try [automatic instrumentation](../automatic/) on one of your own apps.
- For light-weight customized telemetry, try [annotations][].
- Learn about [manual instrumentation][] and try out more [examples](../examples).

[annotations]: ../automatic/annotations
[configure the Java agent]: ../automatic/#configuring-the-agent
[console trace exporter,]: https://github.com/open-telemetry/opentelemetry-java/blob/main/sdk-extensions/autoconfigure/README.md#logging-exporter
[exporter]: https://github.com/open-telemetry/opentelemetry-java/blob/main/sdk-extensions/autoconfigure/README.md#exporters
[Get the example code.]: https://grpc.io/docs/languages/java/quickstart/#get-the-example-code
[Java Quick start example]: https://grpc.io/docs/languages/java/quickstart/
[manual instrumentation]: ../manual
[opentelemetry-javaagent.jar]: https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v1.12.0/opentelemetry-javaagent.jar
[releases]: https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases
[Run the example:]: https://grpc.io/docs/languages/java/quickstart/#run-the-example
