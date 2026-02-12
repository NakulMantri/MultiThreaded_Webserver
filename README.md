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

Check Java installation:
```bash
java -version
javac -version
```

---

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/Multithreaded-TCP-Server.git
cd Multithreaded-TCP-Server
```

---

### 2. Compile the Server

#### Case A — Files in project root
(If `Server.java` is in the main directory)

```bash
javac Server.java
```

#### Case B — Files inside `src/` directory
```bash
mkdir out
javac -d out src/*.java
```

---

### 3. Run the Server

#### Root structure
```bash
java Server 8080
```

#### `src/` compiled to `out/`
```bash
java -cp out Server 8080
```

You should see:
```
Server started on port 8080
Waiting for client connections...
```

The server is now listening for TCP connections.

---

### 4. Test the Server (Manual Client)

Using Netcat:
```bash
nc localhost 8080
```

Or Telnet:
```bash
telnet localhost 8080
```

Type any message and press **Enter**.  
The server will process the request and send a response.

---

### 5. Load Testing (Apache JMeter)

1. Open JMeter
2. Add **Thread Group**
3. Add **TCP Sampler**

Configure TCP Sampler:

| Setting | Value |
|--------|------|
| Server Name | localhost |
| Port Number | 8080 |
| Timeout | 5000 |

Then:
- Increase **Number of Threads (Users)**
- Increase **Loop Count**
- Run the test

Monitor:
- throughput (requests/sec)
- latency
- error rate




