Here are additional strategies to minimize latency in Java applications beyond the GC optimizations you've already implemented:

## JVM and Runtime Optimizations

**GC Tuning Beyond Object Management:**
- Use low-latency collectors like ZGC or Shenandoah for sub-10ms pause times
- Tune heap sizing (typically 25-50% of available RAM) to avoid allocation pressure
- Set `-XX:+UseStringDeduplication` to reduce memory footprint
- Configure `-XX:MaxGCPauseMillis` for G1GC to target specific latency goals

**JIT Compiler Optimization:**
- Use `-XX:+UnlockExperimentalVMOptions -XX:+UseJVMCICompiler` for Graal compiler
- Warm up critical code paths during startup to trigger compilation earlier
- Use `-XX:CompileThreshold` to lower compilation threshold for hot methods
- Profile-Guided Optimization (PGO) with `-XX:+UseProfiledInlineDecisions`

## Code-Level Optimizations

**Data Structure Selection:**
- Use `TIntObjectHashMap` from Trove instead of `HashMap<Integer, Object>`
- Prefer arrays over collections for fixed-size data
- Use `BitSet` for boolean flags instead of `boolean[]`
- Consider off-heap data structures like Chronicle Map for large datasets

**Method and Algorithm Optimization:**
- Avoid method calls in tight loops (manual inlining)
- Use bitwise operations instead of arithmetic where possible
- Implement custom comparators to avoid boxing/unboxing
- Cache expensive computations and use lookup tables

**Memory Access Patterns:**
- Optimize for CPU cache locality - process data sequentially
- Use struct-of-arrays instead of array-of-structs for better cache utilization
- Minimize pointer chasing through object references

## Thread and Concurrency

**Lock-Free Programming:**
- Use `AtomicReference` and compare-and-swap operations
- Implement lock-free data structures (LMAX Disruptor pattern)
- Use `VarHandle` for low-level atomic operations

**Thread Management:**
- Pin threads to CPU cores using thread affinity
- Use dedicated threads for critical paths to avoid context switching
- Implement custom thread pools sized for your workload
- Consider virtual threads (Project Loom) for I/O-bound operations

## I/O and System Optimizations

**Memory Mapping:**
- Use `MappedByteBuffer` for large file access
- Implement custom serialization instead of Java serialization
- Use direct ByteBuffers to avoid heap allocation for I/O

**Network Optimization:**
- TCP_NODELAY to disable Nagle's algorithm
- Adjust socket buffer sizes
- Use connection pooling and keep-alive
- Consider RDMA or kernel bypass networking (DPDK)

**System-Level Tuning:**
- Disable OS features like CPU frequency scaling and power management
- Set processor affinity to isolate critical processes
- Use huge pages to reduce TLB misses
- Configure OS network stack parameters (TCP window scaling, etc.)

## Measurement and Monitoring

**Micro-benchmarking:**
- Use JMH (Java Microbenchmark Harness) for accurate latency measurement
- Profile allocation rates with allocation profilers
- Monitor method-level latency with tools like async-profiler

**Production Monitoring:**
- Implement percentile-based latency tracking (P99.9, P99.99)
- Use low-overhead profiling in production
- Set up alerts for latency regressions

The most impactful optimizations often depend on your specific use case - CPU-bound vs I/O-bound, batch processing vs request-response, etc. Start with profiling to identify the actual bottlenecks before applying these techniques.