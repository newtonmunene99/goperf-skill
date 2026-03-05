---
name: goperf-skill
description: Recommendations and best practices for writing performant Go code from goperf.dev. Use when the user is writing or reviewing Go for performance—optimizing APIs, services, or pipelines; reducing allocations or GC pressure; improving networking (HTTP, TCP, gRPC, connections); or applying patterns from the Go optimization guide. Trigger for allocation/GC, hot paths, net/http tuning, connection scaling, or "goperf" / "Go performance" requests.
license: Apache-2.0
metadata:
  version: "1.0"
---

# Go Performance (goperf)

## Overview

Apply measurement-driven Go performance patterns from the [Go Optimization Guide](https://goperf.dev). Measure first (benchmarks, pprof, escape analysis); then apply targeted patterns. Focus on production workloads: backend services, pipelines, and systems where latency and throughput matter.

## Workflow

1. **Establish a baseline** — Add or run benchmarks; profile under load with pprof (CPU, memory). Do not optimize without numbers.
2. **Identify the bottleneck** — Allocations/GC, I/O, scheduler, or networking. Use `go build -gcflags="-m" ./pkg` to see what escapes to the heap.
3. **Apply patterns from references** — Use the appropriate reference file below; all recommendations are self-contained in this skill. For extended articles and examples, see [goperf.dev](https://goperf.dev).

## When to Read Which Reference

- **Memory, allocations, GC, concurrency, I/O, compiler flags, escape analysis** → Read [references/common-patterns.md](references/common-patterns.md).
- **HTTP, Transport tuning, connection reuse, 10k+ connections, TLS, DNS, load shedding, backpressure, TCP/HTTP/2/gRPC/QUIC** → Read [references/networking.md](references/networking.md).

## Quick Cues

- **Connection reuse (HTTP)**: Drain response body before closing (e.g. `io.Copy(io.Discard, resp.Body)` then `resp.Body.Close()`); otherwise the client will not reuse connections.
- **Escape analysis**: Run `go build -gcflags="-m" ./path/to/pkg` to see which values move to the heap; reduce escapes on hot paths to lower GC pressure.

## Edge cases and examples

- **Optimizing without data**: Do not suggest pooling, prealloc, or GOGC changes until the user has (or is guided to) benchmarks or pprof evidence; otherwise recommend measuring first.
- **Networking**: When suggesting Transport or connection changes, remind to drain response bodies for connection reuse and to tune timeouts and limits to match the workload.
