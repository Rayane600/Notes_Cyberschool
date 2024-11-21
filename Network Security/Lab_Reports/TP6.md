
---

Galmel Yann
Jelidi--Daniel Rayane

---
### Network Security TP 6-7 Report

---

#### Part I: Services and Ports

1. **Question 1:** Reserved ports and protocols:

   - **HTTP**: Port 80 (TCP)
   - **HTTPS**: Port 443 (TCP)
   - **DNS**: Port 53 (TCP/UDP)
   - **SSH**: Port 22 (TCP)
   - **SMTP**: Port 25 (TCP), **SMTPS**: Port 465 (TCP) for secure submission
   - **POP3**: Port 110 (TCP), **POP3S**: Port 995 (TCP)
   - **IMAP**: Port 143 (TCP), **IMAPS**: Port 993 (TCP)
   - **FTP**: Command Port 21 (TCP), Data Port 20 (TCP)

2. **Question 2:** Other commonly used services include:
   - **HTTP Alternate (8080)** for web development.
   - **RDP (3389)** for Remote Desktop access.
   - **MySQL (3306)** for database connections.
   
---

#### Part II: HTTP Client

##### Part II.A: Playing with Your Browser

1. **Question 1.1**: **Wireshark Filtering** - Wireshark recognizes HTTP/HTTPS by monitoring the packets on ports 80 and 443. It uses the initial handshake and request lines (`GET`, `POST`, etc.) for HTTP and SSL/TLS packets for HTTPS.

2. **Question 1.2**: **Main resource requested** - When navigating to `http://example.org`, the main resource requested is the home page (`/`).

3. **Question 1.3**: **HTTP Request Line** - The main request line would look something like:  
   ```
   GET / HTTP/2
   ```

4. **Question 1.4**: **HTTP Response Line** - The main response line would be:
   ```
   HTTP/2 200
   ```
When i reloaded the page i got 
```
HTTP/2 304
```
Which indicates that the page wasn't modified from the version that was already cached.

5. **Question 1.5**: **Main Headers from Client**:
   - `User-Agent`: Describes the browser and version (e.g., `Mozilla/5.0)`
   - `Accept`: Specifies types of content the client can process (e.g., `text/html,application/xhtml+xml`)
   - `Host`: Specifies the domain (e.g., `example.org`)
   
6. **Question 1.6**: **Main Headers from Server**:
   - `Content-Type`: Indicates the format of the response data (e.g., `text/html`)
   - `Content-Length`: Length of the response content
   - `Server`: Server software and version (e.g., `ECAcc (dcd/7D32)`)
   - `Kepp-Alive`: tells the client the server will keep the connection open for additional requests, specifying a timeout and max requests allowed.

7. **Question 1.7 - 1.10**: **HTTP Authentication** 
- When giving a wrong login/password we this packet:
  `HTTP/1.1 401 Unauthorized`
- When accessing the protected area, the server sends a `WWW-Authenticate` header prompting the client for credentials. The client then responds with an `Authorization` header (e.g., `Authorization: Basic <base64-encoded-message>`). This encoding (base64) is not secure over HTTP but becomes secure over HTTPS since it’s encrypted in transit.
8. **Question 1.8: What are the main HTTP headers sent by the client**
```
Authorization: Basic Zm9vYmFyOkkgbGlrZSBhcHBsZXM=\r\n
    Credentials: foobar:I like apples
```
9. **Can this authentication being considered secure**
No it is not secure since it is just encrypted in base64 and easily reversible.
10. **Can this authentication be considered secure**
It is secure since we don't have the authentication field, so we can't trace back the password.
##### Part II.B: Command Line Clients

1. **Question 2.1**: **HTTP/Port Mapping** - The command `telnet <hostname> http` works by using `/etc/services`, where `http` is mapped to port 80.

2. **Question 2.2**: **Telnet HTTP Request**:
  ```

HTTP/1.1 200 OK  
Date: Tue, 12 Nov 2024 15:43:35 GMT  
Server: Apache/2.4.62 (Debian)  
Last-Modified: Fri, 03 Nov 2023 19:01:09 GMT  
ETag: "502-6094420916962"  
Accept-Ranges: bytes  
Content-Length: 1282  
Vary: Accept-Encoding  
Content-Type: text/html  
<!doctype html>  
<html>  
<head>  
   <title>Copy of Example Domain</title>  
   <meta charset="utf-8" />  
   <meta http-equiv="Content-type" content="text/html; charset=utf-8" /> 
   <meta name="viewport" content="width=device-width, initial-scale=1" />  
   <style type="text/css">  
   body {  
       background-color: #f0f0f2;  
       margin: 0;  
       padding: 0;  
       font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Ne  
ue", Helvetica, Arial, sans-serif;  
   }  
   div {  
       width: 600px;  
       margin: 5em auto;  
       padding: 2em;  
       background-color: #fdfdff;  
       border-radius: 0.5em;  
       box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);  
   }  
   a:link, a:visited {  
       color: #38488f;  
       text-decoration: none;  
   }  
   @media (max-width: 700px) {  
       div {  
           margin: 0 auto;  
           width: auto;  
       }  
   }  
   </style>       
</head>  
  
<body>  
<div>  
   <h1>This is a copy of Example Domain</h1>  
   <p>This domain is for use in illustrative examples in documents. You may use this  
   domain in literature without prior coordination or asking for permission.</p>  
   <p><a href="https://www.iana.org/domains/example">More information...</a></p>  
</div>  
</body>  
</html>
```
3. **Question 2.3 - 2.5**: **Wget Observations** 
- Wget  saves the output to the current directory with a file named after the page. 
- Firefox’s requests differ as they contain more headers like `User-Agent` and cookies. 
- To impersonate Firefox, `wget --user-agent="Mozilla/5.0"` could be used.
4. **Question 2.6 - 2.8**: **Curl Observations** 
- Curl outputs responses directly to stdout unless redirected. 
- Similar to wget, curl can also be configured to impersonate Firefox with `curl -A "Mozilla/5.0..."`.

5. **Question 2.9 - 2.10**: **Custom Auth Header** 
- Use `curl -H "Authorization: Basic <base64-encoded-credentials>"` to authenticate, and `--libcurl foo.c` generates C source code for making similar requests.
6. **Question 2.11: What do these commands do? Could an attacker intercept our exchange?**

- **Command 1:** `openssl s_client -connect example.org:443`
    - This command connects securely to `example.org` over HTTPS, letting us send and receive messages directly.
- **Command 2:** `socat openssl-connect:example.org:443 stdin`
    - This command also connects securely to `example.org` over HTTPS but uses `socat`, which can redirect input and output.
- **Interception Possibility:**
    - If certificate validation is skipped, an attacker might intercept this connection by pretending to be the server (a man-in-the-middle attack). Proper validation helps make this less likely.
7. **Question 2.12: DIY HTTP client in python
```python
import socket
def simple_http_client(host):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((host, 80))
    request = f"GET / HTTP/1.1\r\nHost: {host}\r\n\r\n"
    client_socket.send(request.encode())
    response = client_socket.recv(4096)
    client_socket.close()
    print(response.decode())
simple_http_client("example.org")
```
---

#### Part III: HTTP from a Server Perspective

##### Part III.A: HTTP Server with Netcat

1. **Question 3.1**: **Observation**
- Netcat simply prints the HTTP request it receives. Any response, such as `HTTP/1.1 200 OK`, must be manually typed or scripted.

##### Part III.B: Apache Web Server Setup

1. **Question 3.2 - 3.4**: **Apache Test** 
- Confirm Apache is running with `ss -tln | grep ':80'`. 
- Accessing `localhost/test`  displays the contents of `test`, while `localhost/test2` shows a directory listing.
##### Part III.C: Python Server
---

 **Question 3.5 - 3.7**: **Python Web Server Setup** - `python3 -m http.server 8080` This command starts a basic HTTP server on port 8080. It serves the contents of the current working directory, allowing a browser to access files via `http://localhost:8080`.
**Question 3.6** 
```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

class myHTTPRequestHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b"Hello world!")
        # return

httpd = HTTPServer(("localhost", 8080), myHTTPRequestHandler)
httpd.serve_forever()

```
This code starts an HTTP server on port 8080. When a client sends a GET request, it responds with "Hello world!" in plain text.Unlike the `http.server` command, this server is custom-built to handle specific requests with a personalized response.
**Question 3.7**
```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

class myHTTPRequestHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        return SimpleHTTPRequestHandler.do_GET(self)

httpd = HTTPServer(("localhost", 8080), myHTTPRequestHandler)
httpd.serve_forever()

```
This code acts like the default Python HTTP server started with `python3 -m http.server`. It serves the contents of the working directory. The difference is that this implementation uses a custom handler class for potential future modifications, while the default command does not offer customization.
**Question 3.9**
This code implements a server that:
1. Responds with "Hello world!" for requests to `/hello`.
2. Serves files from a `/www` directory.
3. Returns a 501 error for unsupported paths.
**Question 3.9**: **How would you adapt it to disallow `/www/restricted/`?** Modify the `do_GET` method to check for `/www/restricted/` in the request path and return a 403 Forbidden response:
**Question 3.9**: **How would you adapt it to disallow `/www/restricted/`?** Modify the `do_GET` method to check for `/www/restricted/` in the request path and return a 403 Forbidden response:
```python
if "/www/restricted/" in self.path:
    self.send_response(403, "Forbidden")
    self.end_headers()
    self.wfile.write(b"Error 403: Forbidden")
    return
```
**Question 3.10**: **How would you adapt it to only allow certain IPs?** Add IP filtering in the `__init__` method of `myHTTPRequestHandler`:
```python
allowed_ips = ["127.0.0.1", "192.168.1.42"]
client_ip = self.client_address[0]
if client_ip not in allowed_ips:
    self.send_response(403, "Forbidden")
    self.end_headers()
    self.wfile.write(b"Error 403: Forbidden")
    return

```
**Question 3.11**: **How would you require a password for specific paths?** Use an Authorization header to enforce basic authentication:
```python
auth = self.headers.get("Authorization")
if auth != "Basic base64encodedcredentials":
    self.send_response(401, "Unauthorized")
    self.end_headers()
    self.wfile.write(b"Error 401: Unauthorized")
    return
```
 **Question 3.12: Can this authentication be considered secure?**
- No, not without TLS. Basic Authentication sends the password in Base64, which can be decoded easily. TLS encrypts the communication, making it secure.
**Question 3.13: How can you add a `/www/cookie` that displays and sets a cookie?** :-)
```python
if "/www/cookie" in self.path:
    self.send_response(200)
    self.send_header("Content-Type", "text/html")
    self.end_headers()
    html_content = """
    <html>
        <body>
            <h1>Here is your cookie!</h1>
            <img src="https://example.com/cookie.png" alt="A delicious cookie" width="300">
        </body>
    </html>
    """
    self.wfile.write(html_content.encode("utf-8"))
    return
```

 **Question 3.14: What does the first command do?**
- `openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out cert.pem`:
    - Generates a self-signed SSL certificate valid for 365 days.
 **What about the second command?**
- `socat openssl-listen:8443,cert=cert.pem,key=key.pem,fork,reuseaddr tcp-connect:localhost:8080`:
    - Creates an encrypted connection on port 8443 that forwards traffic to port 8080.
 **Question 3.15: What does the code modification do?**
- It enables SSL/TLS on the Python HTTP server using the generated certificate and key.

 **Question 3.16: Why did we have to validate the certificate manually?**

- The certificate is self-signed, so it is not trusted by default. Browsers require explicit approval for self-signed certificates.

#### **Question 3.17: Is our previous authentication secure now?**
- Yes, with TLS, the communication is encrypted, including the credentials sent in the `Authorization` header.

---