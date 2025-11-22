# Request Flow for Angular Frontend + Java Backend + External APIs

## Technologies
- Angular (Browser SPA)
- Nginx (Edge Layer)
- Spring Cloud Gateway (API Gateway)
- Java Microservices (Business Logic)
- External APIs (3rd-party integrations)

---

## 1️⃣ User Request → Backend Flow

Browser (Angular)
   |
   | HTTPS Request (/api/**)
   v
Nginx (Edge)
   - TLS termination
   - WAF / rate limit / blocking
   - Security headers injection
   |
   | Internal HTTP/HTTPS
   v
Spring Cloud Gateway
   - Auth (JWT token validation)
   - Routing to microservices
   - Circuit breaking & retries
   |
   v
Java Microservice
   ↓
Database

---

### Step-by-step:
1. Angular calls an API endpoint.
2. Nginx handles SSL and security, forwards request.
3. Gateway checks auth and selects a microservice.
4. Microservice executes business logic, DB I/O.
5. Response flows back the same path.

---

## 2️⃣ Static Content Flow

Browser → Nginx (HTML, CSS, JS, images)
(Java backend does **not** serve static files)

---

## 3️⃣ External API Flow (Correct Method)

Browser (Angular)
   |
   v
Nginx → Spring Gateway → Microservice
                              |
                              v
                        External API
                              |
                              v
                        Microservice
                              |
                              v
                           Response

---

## 4️⃣ Security Responsibilities

| Layer | Responsibility |
|------|----------------|
| Nginx | TLS, WAF, IP filtering, bot blocking |
| Spring Cloud Gateway | JWT validation, routing, rate limiting per user |
| Microservices | Business authorization (RBAC/ABAC) |

---

## Combined Architecture Diagram

<img width="541" height="637" alt="image" src="https://github.com/user-attachments/assets/9faea847-f5d2-4525-b392-62d39e5768fc" />

---

## Key Rules
- Angular only calls `/api/**`
- No direct external API calls from browser
- Backend owns **all** auth + business logic
- Nginx protects the perimeter
