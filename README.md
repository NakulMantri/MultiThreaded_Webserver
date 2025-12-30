## Overview

This project implements a **high-throughput, low-latency Java TCP server** designed
to explore the scalability trade-offs of different **concurrency models**.
Three architectures—single-threaded, thread-per-client, and fixed thread-pool—are
implemented and evaluated under heavy concurrent load.

The system was **stress-tested using Apache JMeter (TCP Sampler)** and demonstrated
the ability to process **1,000,000+ TCP requests** with sustained throughput of
**~834K requests/min (~13.9K req/sec)** while maintaining **low average latency
(~56 ms)**. Results highlight the effectiveness of controlled parallelism via
thread pools in achieving predictable performance under load.

The project focuses on **throughput–latency trade-offs, multithreaded execution,
and server-side performance benchmarking**, making it relevant for large-scale
backend systems.
