# Nginx

## How it started
- Built at a Russian portal company to solve **C10K problem**  
  (10,000+ concurrent connections on one server).
- Traditional servers in 2002 failed under high concurrency.
- **Igor Sysoev** (Russian software engineer) developed and released Nginx in **2004**.
- Released as **open-source**.
- **2011** → Commercialized as Nginx Inc.

---

## What made it different
- **Event-driven, async I/O** (not thread per request).
- Much fewer resources needed → high scalability.
- Designed for modern internet traffic (reverse proxy first).

---

## Architecture Diagram
> (Use your attached images)

---

## Architecture Details

### 1️⃣ Event-Driven Core
- Master process manages multiple worker processes.
- Workers handle client traffic.
- Event loops (epoll/kqueue) process thousands of connections in one thread.
- Low memory / low CPU overhead.

---

### 2️⃣ Non-Blocking I/O
- No blocking on disk or network operations.
- Uses callbacks for I/O events.
- Stays responsive under heavy load.

---

### 3️⃣ Shared-Nothing Workers
- Workers don’t share state directly.
- Only share read-only segments (config, static data).
- If one worker dies → others keep going → **high stability**.

---

### 4️⃣ Modular Design
- Small core + many modules:
  - HTTP
  - TCP/UDP stream proxy
  - SSL/TLS engine
  - Load balancing algorithms
  - Caching
- Load only required modules → faster + safer.

---

### 5️⃣ Reverse Proxy First
Why it beat Apache:
- Built for proxying backend apps (PHP, Java, Python, Go).
- Built-in load balancing:
  - Round-Robin
  - Least Connections
  - IP Hash
- Easy SSL termination before sending to backend.

---

### 6️⃣ Built-In Cache
- Can store static responses in **RAM or disk**.
- Great for high-performance caching/CDN systems.
- Huge performance boost for repeat requests.

---

## Nginx vs Apache

| Feature | Apache | Nginx |
|--------|--------|-------|
| Architecture | Thread/Process per connection | Event-Driven |
| Concurrency | Poor under heavy load | Extremely scalable |
| Static Files | OK | Very fast |
| Reverse Proxy | Add-on modules | Native |
| Memory Use | High per request | Low and predictable |

> Apache = forklift moving each box alone  
> Nginx = conveyor belt moving thousands at once

---

## Real-World Performance
- Handles **10x–100x more concurrent connections** than Apache.
- Reliable under sudden heavy traffic spikes.
- Used by major platforms:
  - Netflix
  - GitHub
  - Cloudflare
  - Dropbox
  - Twitch
  - Reddit

---

## When NOT to Use Nginx
- You rely heavily on **.htaccess** (Apache feature).
- Everything is dynamic and backend performance sucks.
- Heavy WebSockets and want simpler config → consider Envoy / HAProxy.

---
