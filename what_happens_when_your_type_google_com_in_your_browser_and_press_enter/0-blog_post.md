# What Happens When You Type `https://www.google.com` and Press Enter?

> A deep dive into the infrastructure layers powering every web request.

---

## 1. DNS Resolution — Translating a Name into an Address

Before your browser can connect to Google, it needs to know *where* Google lives on the internet — specifically, its IP address.

**Step-by-step DNS lookup:**

1. **Browser cache**: Your browser first checks if it already has a cached DNS record for `www.google.com`.
2. **OS cache / hosts file**: If not found, the operating system checks its own cache and the local `/etc/hosts` file.
3. **Recursive resolver**: If still unresolved, the OS queries a DNS resolver (usually provided by your ISP or a public DNS like `8.8.8.8`).
4. **Root nameserver**: The resolver asks a root DNS server which nameservers are authoritative for `.com`.
5. **TLD nameserver**: The `.com` TLD nameserver returns the nameservers for `google.com` (e.g., `ns1.google.com`).
6. **Authoritative nameserver**: Google's own nameserver finally returns the IP address for `www.google.com` (e.g., `142.250.74.68`).
7. **Response cached**: The answer is cached at multiple levels with a TTL (Time To Live).

The whole process typically takes **20–120ms**.

---

## 2. TCP/IP — Establishing the Connection

With the IP address in hand, your browser initiates a **TCP connection** using the TCP/IP protocol stack.

**The TCP 3-way handshake:**

```
Client → Server : SYN  (I want to connect)
Server → Client : SYN-ACK  (I acknowledge, ready)
Client → Server : ACK  (Connection established)
```

- **IP layer**: Routes packets across the internet by choosing the best path through routers.
- **TCP layer**: Ensures reliable, ordered delivery of data.
- **Port 443**: Since the URL uses `https://`, the connection targets port **443** (HTTPS).

---

## 3. Firewall — The Security Gatekeeper

Before the request reaches Google's servers, it passes through multiple firewalls:

- **Client-side**: Your OS or router firewall may inspect outgoing traffic.
- **Network-level**: ISP or enterprise firewalls check packets against rules.
- **Server-side**: Google's infrastructure employs firewalls that:
  - Block traffic on unauthorized ports
  - Rate-limit suspicious IPs
  - Inspect packet headers for malformed requests
  - Apply DDoS protection (e.g., scrubbing centers)

Firewalls operate at **Layer 3 (Network)** and **Layer 7 (Application)** of the OSI model.

---

## 4. HTTPS/SSL — Encrypting the Communication

Since the URL starts with `https://`, the browser performs a **TLS handshake** *on top of* the TCP connection:

1. **ClientHello**: Browser sends supported TLS versions, cipher suites, and a random number.
2. **ServerHello**: Server selects a cipher suite, sends its **SSL/TLS certificate**.
3. **Certificate verification**: Browser checks the certificate against trusted Certificate Authorities (CAs).
4. **Key exchange**: Both sides derive a **shared session key** using asymmetric cryptography (e.g., ECDHE).
5. **Encrypted channel**: All subsequent communication is encrypted with symmetric encryption (e.g., AES-256-GCM).

This guarantees:
- **Confidentiality**: No one can read the data in transit.
- **Integrity**: Data hasn't been tampered with.
- **Authentication**: You're talking to the real Google, not an impostor.

---

## 5. Load Balancer — Distributing Traffic

Google handles **billions of requests per day**. A single server can't handle this. When your encrypted request arrives at Google's network edge:

- A **load balancer** (e.g., based on Google's Maglev system) receives the request.
- It selects an appropriate backend server based on:
  - **Round-robin** or **least-connections** algorithm
  - Server **health checks**
  - Geographic proximity (via **Anycast** routing)
  - Current server load

Load balancers also handle **SSL termination** — decrypting HTTPS traffic so backend servers don't have to.

---

## 6. Web Server — Handling the HTTP Request

The selected backend server runs a **web server** (Google uses custom-built servers, but concepts apply to Nginx/Apache too):

- Receives the decrypted HTTP/2 request: `GET / HTTP/2`
- Parses headers (cookies, Accept-Language, User-Agent, etc.)
- Decides whether to serve a **static resource** directly (cached HTML, CSS, JS) or forward to the application layer.
- For `www.google.com`, the root page requires **dynamic generation**.

---

## 7. Application Server — Generating the Response

The application server is where the **business logic** lives:

- Handles the actual search logic (or in this case, the Google homepage rendering).
- Processes user session data (are you logged in? what language? which experiment bucket?).
- Calls internal microservices for personalization, localization, feature flags.
- Assembles the response — the HTML page.

Google's app servers are written in a mix of **C++, Java, Go, and Python**, running on their custom Borg/Kubernetes orchestration platform.

---

## 8. Database — Fetching Persistent Data

To personalize and render the page, the application server may query:

- **User databases**: Account info, preferences, search history (if logged in).
- **Configuration stores**: Feature flags, A/B test assignments.
- **Cache layers**: **Memcached** or **Redis** for fast key-value lookups before hitting disk.
- **Distributed datastores**: Google's **Bigtable** or **Spanner** for large-scale data with global consistency.

Databases are horizontally sharded and replicated across multiple **data centers** for reliability.

---

## 9. The Response Journey Back

Once the HTML is assembled:

1. Application server → Web server → Load balancer
2. Response is **compressed** (gzip/Brotli), encrypted (TLS), and sent back over TCP.
3. Your browser receives packets, **reassembles** them, and begins **parsing the HTML**.
4. Sub-resources (CSS, JS, images) trigger **additional HTTP requests** — often multiplexed over a single **HTTP/2** connection.
5. The browser **renders the DOM**, executes JavaScript, and displays the page.

Total time from Enter key to rendered page: **typically under 300ms** for a warm connection.

---

## Summary

```
[You] → DNS lookup → TCP handshake → TLS handshake
     → Firewall → Load Balancer → Web Server
     → App Server → Database → [Response]
     → [Rendered Page]
```

Every time you press Enter, this entire orchestration happens invisibly in a fraction of a second. Understanding it is foundational to building robust, secure, and scalable web applications.

---

*Published as part of the Holberton School — Systems & DevOps track.*
