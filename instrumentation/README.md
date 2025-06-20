# Instrumentation

Code contained in this directory contains instrumentation for 3rd-party Go packages and some packages from the standard library.

## Instrumentation Packages

The [OpenTelemetry registry](https://opentelemetry.io/registry/) is the best place to discover instrumentation packages.
It will include packages outside of this project.

The following instrumentation packages are provided for popular Go packages and use-cases.

| Instrumentation Package | Metrics | Traces |
| :---------------------: | :-----: | :----: |
| [github.com/aws/aws-sdk-go-v2](./github.com/aws/aws-sdk-go-v2/otelaws)|  | ✓ |
| [github.com/emicklei/go-restful](./github.com/emicklei/go-restful/otelrestful) |  | ✓ |
| [github.com/gin-gonic/gin](./github.com/gin-gonic/gin/otelgin) |  | ✓ |
| [github.com/gorilla/mux](./github.com/gorilla/mux/otelmux) |  | ✓ |
| [github.com/labstack/echo](./github.com/labstack/echo/otelecho) |  | ✓ |
| [go.mongodb.org/mongo-driver](./go.mongodb.org/mongo-driver/mongo/otelmongo) |  | ✓ |
| [go.mongodb.org/mongo-driver/v2](./go.mongodb.org/mongo-driver/v2/mongo/otelmongo) |  | ✓ |
| [google.golang.org/grpc](./google.golang.org/grpc/otelgrpc) | ✓ | ✓ |
| [host](./host) | ✓ |  |
| [net/http](./net/http/otelhttp) | ✓ | ✓ |
| [net/http/httptrace](./net/http/httptrace/otelhttptrace) |  | ✓ |
| [runtime](./runtime) | ✓ |  |

## Organization

In order to ensure the maintainability and discoverability of instrumentation packages, the following guidelines MUST be followed.

### Packaging

All instrumentation packages SHOULD be of the form:

```sh
go.opentelemetry.io/contrib/instrumentation/{IMPORT_PATH}/otel{PACKAGE_NAME}
```

Where the [`{IMPORT_PATH}`](https://golang.org/ref/spec#ImportPath) and [`{PACKAGE_NAME}`](https://golang.org/ref/spec#PackageName) are the standard Go identifiers for the package being instrumented.

For example:

- `go.opentelemetry.io/contrib/instrumentation/github.com/gorilla/mux/otelmux`
- `go.opentelemetry.io/contrib/instrumentation/gopkg.in/macaron.v1/otelmacaron`
- `go.opentelemetry.io/contrib/instrumentation/database/sql/otelsql`

Exceptions to this rule exist.
For example, the [runtime](./runtime) and [host](./host) instrumentation do not instrument any Go package and therefore do not fit this structure.

### Contents

All instrumentation packages MUST adhere to [the projects' contributing guidelines](../CONTRIBUTING.md).
Additionally the following guidelines for package composition need to be followed.

- All instrumentation packages MUST be a Go module.
   Therefore, an appropriately configured `go.mod` and `go.sum` need to exist for each package.
- To help understand the instrumentation a Go package documentation SHOULD be included.
   This documentation SHOULD be in a dedicated `doc.go` file if the package is more than one file.
   It SHOULD contain useful information like what the purpose of the instrumentation is, how to use it, and any compatibility restrictions that might exist.
- Examples of how to actually use the instrumentation SHOULD be included.
- All instrumentation packages MUST provide an option to accept a `TracerProvider` if it uses a Tracer, a `MeterProvider` if it uses a Meter, and `Propagators` if it handles any context propagation.
  Also, packages MUST use the default `TracerProvider`, `MeterProvider`, and `Propagators` supplied by the `global` package if no optional one is provided.
- All instrumentation packages MUST NOT provide an option to accept a `Tracer` or `Meter`.
- All instrumentation packages MUST define a `ScopeName` constant with a value matching the instrumentation package and use it when creating a `Tracer` or `Meter`.
- All instrumentation packages MUST define a `Version` function returning the version of the module containing the instrumentation and use it when creating a `Tracer` or `Meter`.
