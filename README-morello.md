# Background

This software is an experimental Morello port of Redis 7.0.5.

## Build instructions

Redis is build with GNU make (gmake), first install this build dependency
as follows:

`$ sudo pkg64 install gmake`

Once installed Redis can be built as follows:

`$ gmake`

Further instructions for building Redis can be found in the README.

## Run instructions

Once built Redis can be started as:
 
```
src/redis-server
95908:C 20 Jul 2023 10:22:59.854 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
95908:C 20 Jul 2023 10:22:59.854 # Redis version=7.0.5, bits=64, commit=2628c16b, modified=0, pid=95908, just started
95908:C 20 Jul 2023 10:22:59.854 # Warning: no config file specified, using the default config. In order to specify a config file use src/redis-server /path/to/redis.conf
95908:M 20 Jul 2023 10:22:59.856 * monotonic clock: POSIX clock_gettime
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 7.0.5 (2628c16b/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 95908
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           https://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

95908:M 20 Jul 2023 10:22:59.858 # Server initialized
95908:M 20 Jul 2023 10:22:59.858 * Ready to accept connections
```

Further instructions for running and configuring Redis can be found in the
README.

## Testing

### Unit testing

Redis's unit tests can be executed with:

`gmake test`

### Performance testing

Benchmarking can be performed running the Redis benchmark utility.
First start a Redis server instance:

`src/redis-server`

Then run the benchmark utility:

```
src/redis-benchmark
...
99.945% <= 2.103 milliseconds (cumulative count 99945)
99.988% <= 3.103 milliseconds (cumulative count 99988)
100.000% <= 4.103 milliseconds (cumulative count 100000)

Summary:
  throughput summary: 46970.41 requests per second
  latency summary (msec):
          avg       min       p50       p95       p99       max
        0.951     0.368     0.943     1.231     1.295     3.407
```

## Notes and Limitations

Whilst a large number of the Redis unit tests pass, one of the tests results
in a server crash (likely from a CHERI protection fault). This crash has
not been investigated.

Adaptations to nginx to the memory-safe CheriABI have been driven by:
compiler warnings and errors, and dynamic testing. Where the compiler
emits a warning or error we are able to rigorously review this and
correct. However, some issues only manifest dynamically (at runtime),
such as invalidation of capabilities by pointer arithmetic,
non-blessed memory copies, or insufficient pointer alignment.
Enhancements such as CHERI UBsan have modestly improved the ability to
identify problems previously only found during dynamic testing. However,
 we are still greatly reliant on dynamic testing. This testing is
constrained by both the completeness of the test suites (which in some
cases provide poor coverage) and the time available within the project
to perform testing. 

## Acknowledgement

This work has been undertaken within DSTL contract
ACC6036483: CHERI-based compartmentalisation for web services on Morello.
