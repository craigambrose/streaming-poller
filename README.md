# streaming-poller

A node.js stream that executes another stream at regular intervals

## Installation

Install via npm:

```bash
$ npm install streaming-poller
```

## Usage

Provide a factory function that returns your inner stream, and a time interval in milliseconds. The resultant
streaming-poller stream will immediately act as the inner stream would, but once the inner stream has ended,
it will wait for the time interval and then use the factory to re-create the inner stream.

The obvious use case for this is regularly polling some source of IO that is itself a stream. The original import
source is streamed through the streaming-poller stream, handling piping and backpressure in the normal way. Once
it has finished and the streaming-poller stream is waiting for the specified interval, it can also be paused
(which will pause the interval timer), meaning that backpressure can cause the polling to slow down.

## Examples

### Stream the numbers 4, 5 & 6 every two seconds.

```coffeescript
streamingPoller = require('streaming-poller')
streamify = require('stream-array')

streamFactory = () ->
  streamify([4,5,6])

poller = streamingPoller streamFactory, interval: 2000
```
