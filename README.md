# Multithreaded TCP Server (Java)

A high-throughput, low-latency TCP server implemented in Java to study scalability and performance behavior under concurrent client load.  
The project evaluates multiple concurrency models and demonstrates how controlled parallelism (thread pools) improves throughput stability and response latency in real server workloads.

---

## Problem Statement

Traditional blocking servers struggle when multiple clients connect simultaneously.  
Naïve solutions (like spawning a new thread per client) scale poorly due to:

- excessive context switching
- memory overhead
- thread scheduling contention

This project explores how different concurrency strategies affect server performance and system stability.

---

## Implemented Concurrency Models

### 1. Single-Threaded Server
- Handles one client at a time
- Simple and predictable
- Becomes a bottleneck under concurrent connections

### 2. Thread-Per-Client Model
- New thread created for each connection
- Improves responsiveness
- Fails under heavy load due to unbounded thread creation

### 3. Fixed Thread Pool Server (Optimized)
- Uses Java `ExecutorService`
- Reuses worker threads
- Limits resource usage
- Provides stable performance under high concurrency

This architecture achieved the best throughput-latency balance.

---

## Architecture

Client → TCP Socket → Connection Acceptor → Worker Queue → Thread Pool → Request Handler → Response

### Server Workflow
1. Server listens on a TCP port using `ServerSocket`
2. Incoming client connections are accepted
3. Connections are placed into a task queue
4. Worker threads from the pool process requests
5. Response is written back to the client socket

Key components:
- Connection handler
- Request processor
- Worker thread pool
- Load testing client

---

## Performance Testing

Load testing was performed using **Apache JMeter (TCP Sampler)** to simulate thousands of concurrent clients.

### Results

| Metric | Value |
|------|------|
| Total Requests | 1,000,000+ |
| Peak Throughput | ~834,000 requests/min |
| Requests/sec | ~13,900 |
| Average Latency | ~56 ms |
| Concurrent Clients | Thousands (JMeter simulated) |

### Observations
- Single-threaded model saturates CPU quickly
- Thread-per-client causes heavy context switching
- Fixed thread pool maintains predictable latency
- Throughput stabilizes once optimal pool size is reached

---

## Technologies Used
- Java (Core)
- Java Sockets (TCP)
- Multithreading & Synchronization
- ExecutorService Thread Pools
- Apache JMeter (Load Testing)

---

## How to Run

### Prerequisites
- Java JDK 11 or higher
- Git
- (Optional) Apache JMeter for load testing

Verify Java installation:
```bash
java -version
javac -version

