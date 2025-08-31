### HTTP

---

#### **What is HTTP?**

- HyperText Transfer Protocol (HTTP) enables the transfer of web resources.
- **Client-server model**:
    - Client initiates requests, and the server responds.
    - Stateless protocol: No inherent state is maintained between transactions.

**Key Features**:

- **Resources** identified using **URLs**:
    - `https://example.com:443/path?query#fragment`
        - **Scheme**: `https`
        - **Host**: `example.com`
        - **Port**: `443` (default for HTTPS)
        - **Path**: `/path`
        - **Query**: `?query`
        - **Fragment**: `#fragment`

---

#### **HTTP Evolution**

1. **HTTP/0.9** (1991): Original, minimalistic version.
2. **HTTP/1.0** (1996): Introduced headers and non-HTML content support.
3. **HTTP/1.1** (1997-1999): Persistent connections, Host header.
4. **HTTP/2** (2015): Multiplexing and pipelining over a single connection.
5. **HTTP/3** (2022): Uses QUIC over UDP, improved latency and security.

---

#### **HTTP Messages**

1. **Requests**:
    
    - Format: `METHOD PATH HTTP_VERSION`
    - Methods:
        - `GET`: Retrieve resources.
        - `POST`: Send data to server.
        - `HEAD`: Metadata only.
        - `PUT`, `PATCH`, `DELETE`: Modify server data.
    - Example:
        
        ```
        GET / HTTP/1.1
        Host: example.com
        User-Agent: curl/7.88.1
        ```
        
2. **Responses**:
    
    - Format: `HTTP_VERSION STATUS_CODE STATUS_MESSAGE`
    - Example:
        
        ```
        HTTP/1.1 200 OK
        Content-Type: text/html
        ```
        
    - Common Status Codes:
        - `200 OK`, `404 Not Found`, `500 Internal Server Error`.

---

#### **HTTP Headers**

- Extend functionality and provide metadata.
- Examples:
    - Request: `User-Agent`, `Accept`, `Authorization`.
    - Response: `Content-Type`, `Cache-Control`.

---

#### **Cookies**

- Used to maintain state across HTTP's stateless transactions.
- Components:
    1. `Set-Cookie` in HTTP response.
    2. Cookie sent back in subsequent requests.
    3. Stored on the client.
    4. Server-side database to track user sessions.
- Use Cases:
    - Authentication.
    - Shopping carts.
    - Recommendations.
- Privacy Concerns:
    - Third-party cookies enable tracking across websites.

---

#### **Web Caching**

- **Proxy servers** store frequently accessed resources.
- Benefits:
    - Reduce response times.
    - Lower bandwidth usage.
- Mechanisms:
    - `If-Modified-Since`: Conditional GET to validate cache freshness.
    - Cache-Control directives.

---

#### **Security Enhancements**

- **HTTPS**:
    - HTTP over TLS (Transport Layer Security).
    - Encrypts communication to protect sensitive data.
- **Authentication**:
    - Mechanisms:
        - `Basic`: Vulnerable without HTTPS.
        - `Digest`, OAuth for enhanced security.

---

#### **Modern Applications**

- **REST APIs**:
    - Use HTTP methods for CRUD operations:
        - `GET`, `POST`, `PUT`, `DELETE`.
- **HTTP/3**:
    - Benefits:
        - Reduces head-of-line blocking with UDP-based QUIC.
        - Improved latency and reliability.

---
