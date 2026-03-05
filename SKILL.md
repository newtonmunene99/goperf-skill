---
name: goperf-skill
description: Recommendations and best practices for writing performant Go code from goperf.dev. Use this skill whenever the user is working in Go and care about speed, resources, or scale—including optimizing or reviewing APIs, services, or pipelines; reducing allocations, GC pressure, or memory; improving networking (HTTP, Transport, TCP, gRPC, connection reuse, 10k connections); tuning hot paths, sync.Pool, preallocation, or struct layout; or when they mention goperf, "Go performance", latency, throughput, or profiling. Use it even if they do not explicitly say "optimize" or "performance"—e.g. "my Go service is slow", "too many allocations", "HTTP client is opening too many connections", or "how do I make this faster" all warrant this skill.
license: Apache-2.0
metadata:
  version: "1.0"
---

# Go Performance (goperf)

## Overview

Apply measurement-driven Go performance patterns from the [Go Optimization Guide](https://goperf.dev). Measure first (benchmarks, pprof, escape analysis); then apply targeted patterns. Focus on production workloads: backend services, pipelines, and systems where latency and throughput matter.

## Workflow

1. **Establish a baseline** — Add or run benchmarks; profile under load with pprof (CPU, memory). Optimizing without numbers often targets the wrong place; a baseline makes the impact of changes observable and avoids wasted effort.
2. **Identify the bottleneck** — Allocations/GC, I/O, scheduler, or networking. Use `go build -gcflags="-m" ./pkg` to see what escapes to the heap.
3. **Apply patterns from references** — Use the appropriate reference file below; all recommendations are self-contained in this skill. Each reference is organized by section with tables and code examples; jump to the section that matches the bottleneck. For extended articles, see [goperf.dev](https://goperf.dev).

## When to Read Which Reference

- **Memory, allocations, GC, concurrency, I/O, compiler flags, escape analysis** → Read [references/common-patterns.md](references/common-patterns.md) (see its table of contents to find the right section).
- **HTTP, Transport tuning, connection reuse, 10k+ connections, TLS, DNS, load shedding, backpressure, TCP/HTTP/2/gRPC/QUIC** → Read [references/networking.md](references/networking.md) (see its table of contents to find the right section).

## Quick Cues

- **Connection reuse (HTTP)**: Drain response body before closing (e.g. `io.Copy(io.Discard, resp.Body)` then `resp.Body.Close()`); otherwise the client will not reuse connections.
- **Escape analysis**: Run `go build -gcflags="-m" ./path/to/pkg` to see which values move to the heap; reduce escapes on hot paths to lower GC pressure.

## Edge cases and examples

- **Optimizing without data**: Avoid suggesting pooling, prealloc, or GOGC changes until the user has (or is guided to) benchmarks or pprof evidence—suggestions without data often target the wrong bottleneck and add complexity without benefit. Recommend measuring first.
- **Networking**: When suggesting Transport or connection changes, remind to drain response bodies for connection reuse and to tune timeouts and limits to match the workload; otherwise connections may not be reused or may be held too long.
