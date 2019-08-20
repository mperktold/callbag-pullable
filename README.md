# Pullable Callbags 👜

This specification extends [callbag/callbag](https://github.com/callbag/callbag), defining what it means
for a source to be "pullable".

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### Pull
A sink delivers data to the source to signal that the source MAY deliver data back to the sink.
We call this a _pull_.

### Response
For each pull, a listenable source MAY respond by either delivering data to the sink or terminating
the sink.
The source MUST NOT respond multiple times for a single pull.

### Timing and Ordering
A sink MAY send multiple pull messages without waiting for responses.
In that case, the source SHOULD respond to pull messages in the order it received them
such that the sink can easily associate each response to a pull.

### Robustness
There is a risk of stack overflow when both the source and the sink deliver data to each inside
a call that delivers data to them.
Therefore, when a pullable source responds to a pull from a sink by delivering data and the sink
pulls more data inside that call, the source SHOULD wait for the response to return before delivering
more data to the sink.

### Specializations
This section defines terminology for some common specializations of pullable sources.

#### Synchronous
A _synchronous_ pullable source immediately responds to every pull while preventing stackoverflow
as described in [Robustness](#robustness).
More precisely, when one or more pulls to a synchronous pullable source are on the stack, the source
MUST respond to all of them before the outermost pull returns.

#### Asynchronous
An _asynchronous_ pullable source never responds immediately to a pull.
More precisely, when one or more pulls to an asynchronous pullable source are on the stack, the source
MUST NOT respond to any of them until the ouotermost pull returns.
