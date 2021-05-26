# Non-descriptive attributes

## Motivation

OpenTelemetry has a data model for API-level events across multiple
signals, where there is an implied relational aspect to the input and
output data.  API-level events have _attributes_, as do the outputs of
the SDK following those events.

In most cases, attributes are descriptive properties of the event,
they describe an operation being instrumented, a thing being counted,
the event being logged, and so on.  However, there are well-known
cases where the semantics expressed by an attribute are not meant to
describe properties of the event.  Instead, these _non-descriptive_
attributes are properties of the collection process.

The examples considered:

- To signal replicated data. The Prometheus convention for High
  Availability is to set the `prometheus_replica` label to distinguish
  duplicate streams of data.  Cortex backends are configured to erase
  the attribute de-duplicate the resulting streams.  If these
  attributes are treated as ordinary attributes, the result is
  unintended duplication of data.  Counters, in particular, will be
  magnified by the replication factor.
- To convey sampling inclusion probability.  An attribute that
  describes how a span was sampled can be expressed in the form of a
  probability or an adjusted count.  An attribute named
  `sampling.traceidratio.adjusted_count` might be used to signify the
  use of the built-in [TraceIDRatio Sampler](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/sdk.md#traceidratiobased).

## Detailed design

TODO

```yaml
# Definitions for each schema version in this resource family.
# Note: the ordering of versions is defined according to semver
# version number ordering rules.
versions:
  <version_number>:
    all:
      attributes:
	    nondescriptive:
		  prometheus
		  prometheus_replica
		  sampling.traceidratio.adjusted_count
```
