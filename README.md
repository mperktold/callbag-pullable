# Pullable Callbags 👜

This specification extends [callbag/callbag](https://github.com/callbag/callbag), defining what it means for a source to be "pullable".

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### Pull
A sink delivers data to the source to signal that the source MAY deliver data back to the sink.
We call this a _pull_.

### Response
For each pull, a listenable source MAY respond by either delivering data to the sink or terminating the sink.
The source MUST NOT respond multiple times for a single pull.

### Timing and Ordering
A source MAY send multiple pull messages without waiting for responses.
In that case, the source SHOULD respond to pull messages in the order it received them
such that the sink can easily associate each response to a pull.

### Robustness
When a pullable source responds to a sink by delivering data and the sink pulls more data inside that call,
the source SHOULD wait for the response to return before delivering more data.
Otherwise, a stack overflow error could occur.
