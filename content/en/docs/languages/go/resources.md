---
title: Resources
weight: 70
cSpell:ignore: sdktrace thirdparty
---

{{% docs/languages/resources-intro %}}

Resources should be assigned to a tracer provider at its initialization, and are
created much like attributes:

```go
resources := resource.NewWithAttributes(
    semconv.SchemaURL,
    semconv.ServiceNameKey.String("myService"),
    semconv.ServiceVersionKey.String("1.0.0"),
    semconv.ServiceInstanceIDKey.String("abcdef12345"),
)

provider := sdktrace.NewTracerProvider(
    ...
    sdktrace.WithResource(resources),
)
```

Note the use of the `semconv` package to provide
[conventional names](/docs/concepts/semantic-conventions/) for resource
attributes. This helps ensure that consumers of telemetry produced with these
semantic conventions can easily discover relevant attributes and understand
their meaning.

Resources can also be detected automatically through `resource.Detector`
implementations. These `Detector`s may discover information about the currently
running process, the operating system it is running on, the cloud provider
hosting that operating system instance, or any number of other resource
attributes.

```go
resources := resource.New(context.Background(),
    resource.WithFromEnv(), // pull attributes from OTEL_RESOURCE_ATTRIBUTES and OTEL_SERVICE_NAME environment variables
    resource.WithProcess(), // This option configures a set of Detectors that discover process information
    resource.WithOS(), // This option configures a set of Detectors that discover OS information
    resource.WithContainer(), // This option configures a set of Detectors that discover container information
    resource.WithHost(), // This option configures a set of Detectors that discover host information
    resource.WithDetectors(thirdparty.Detector{}), // Bring your own external Detector implementation
    resource.WithAttributes(attribute.String("foo", "bar")), // Or specify resource attributes directly
)
```
