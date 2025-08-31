
---

Galmel Yann
Jelidi--Daniel Rayane

---
### Network Security TP 6-7 Report

---
###  Part I: Services and Ports

#### **Question 1: Reserved ports and protocols**

- **HTTP**: Port 80 (TCP)
- **HTTPS**: Port 443 (TCP)
- **DNS**: Port 53 (TCP/UDP)
- **SSH**: Port 22 (TCP)
- **SMTP**: Port 25 (TCP)
- **SMTPS**: Port 465 (TCP) for secure submission
- **POP3**: Port 110 (TCP)
- **POP3S**: Port 995 (TCP)
- **IMAP**: Port 143 (TCP)
- **IMAPS**: Port 993 (TCP)
- **FTP Command**: Port 21 (TCP)
- **FTP Data**: Port 20 (TCP)

#### **Question 2: Other commonly used services**

- **HTTP Alternate**: Port 8080, often used in web development.
- **RDP**: Port 3389, for Remote Desktop Protocol access.
- **MySQL**: Port 3306, for database connections.

---

### Part II: HTTP Client

---

#### Part II.A: Playing with Your Browser

##### **Question 1.1: How does Wireshark filter HTTP/HTTPS traffic?**

Wireshark recognizes HTTP/HTTPS packets by analyzing traffic on ports 80 (HTTP) and 443 (HTTPS). It identifies HTTP packets based on request/response headers like `GET`, `POST`, or `HTTP/1.1`. HTTPS packets are distinguished by the SSL/TLS handshake.

##### **Question 1.2: What is the main resource requested?**

When navigating to `http://example.org`, the main resource requested is the home page (`/`).

##### **Question 1.3: What is the main HTTP request line?**

```
GET / HTTP/2
```

##### **Question 1.4: What is the main HTTP response line?**

- Initial request:
    
    ```
    HTTP/2 200
    ```
    
- On reload with cached content:
    
    ```
    HTTP/2 304
    ```
    
    This indicates no modification since the last request.

##### **Question 1.5: What are the main headers sent by the client?**

- **`User-Agent`**: Provides details about the browser and operating system (e.g., `Mozilla/5.0`).
- **`Accept`**: Lists the content types acceptable by the client (e.g., `text/html,application/xhtml+xml`).
- **`Host`**: Specifies the server's domain name (e.g., `example.org`).

##### **Question 1.6: What are the main headers sent by the server?**

- **`Content-Type`**: Specifies the data format (e.g., `text/html`).
- **`Content-Length`**: Indicates the size of the response body.
- **`Server`**: Describes the server software and version.
- **`Keep-Alive`**: States whether the connection will remain open for additional requests.

---

#### **HTTP Authentication**

##### **Question 1.7: What are the server headers prompting for authentication?**

The server sends the following response to request authentication:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="Restricted Area"
```

##### **Question 1.8: What are the client headers for authentication?**

The client responds with:

```
Authorization: Basic Zm9vYmFyOklfbGlrZV9hcHBsZXM=
```

This header encodes the credentials `foobar:I_like_apples` in Base64 format.

##### **Question 1.9: Is this authentication secure over HTTP?**

No, it is not secure. The credentials are only encoded in Base64, which is easily reversible. Without TLS, an attacker can intercept the credentials.

##### **Question 1.10: Is this authentication secure over HTTPS?**

Yes, it is secure. TLS encrypts the entire communication, preventing interception of the credentials.

---


####  Part II.B: Command Line Clients

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

### Part III: HTTP from a Server Perspective

#### Part III.A: HTTP Server with Netcat

1. **Question 3.1**: **Observation**
- Netcat simply prints the HTTP request it receives. Any response, such as `HTTP/1.1 200 OK`, must be manually typed or scripted.

#### Part III.B: Apache Web Server Setup

1. **Question 3.2 - 3.4**: **Apache Test** 
- Confirm Apache is running with `ss -tln | grep ':80'`. 
- Accessing `localhost/test`  displays the contents of `test`, while `localhost/test2` shows a directory listing.
#### Part III.C: Python Server
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
**Question 3.13: How can you add a `/www/cookie` that displays and sets a cookie?** 
```python
if "/www/cookie" in self.path:
    self.send_response(200)
    self.send_header("Content-Type", "text/html")
    self.send_header("Set-Cookie", "user=example_cookie_value")
    self.end_headers()
    html_content = """
    <html>
        <body>
            <h1>Cookie Set!</h1>
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
### Part IV: Going REST

---

#### **Question 4.1: What does the given code do? How does it compare to previous examples?**

The provided code implements RESTful API using Flask.  features include:

- A `GET` endpoint (`/hello`) returning a greeting.
- A dynamic `GET` endpoint (`/hello/<name>`) responding with a personalized greeting.
- A `POST` endpoint (`/hello`) processing form data and returning a greeting.

**Comparison:**

- Unlike earlier Python HTTP server examples, this code uses Flask.
- It offers route-specific functionality.
---

#### **Question 4.2: Add a `/store` route to list objects in the store**

```python
@app.route("/store", methods=["GET"])
def get_store():
    return jsonify(storage)
```

This route returns the list of all objects in the `storage` dictionary as JSON.

---

#### **Question 4.3: Add a `/store/$x` route to get an object by its key**

```python
@app.route("/store/<key>", methods=["GET"])
def get_object(key):
    if key in storage:
        return jsonify({key: storage[key]})
    else:
        abort(404) 
```

This route retrieves the object corresponding to the specified key, or returns a `404` error if the key is not found.

---

#### **Question 4.4: Add a `/store/` route to create a new object**

```python
@app.route("/store", methods=["POST"])
def add_object():
    key = request.form.get("key")
    value = request.form.get("value")
    if not key or not value:
        abort(404)  
    storage[key] = value
    return jsonify({key: value})
```

This route allows users to send a `POST` request with form data to add a new object.

---

#### **Question 4.5: Add a `/store/$x` route to replace an object**

```python
@app.route("/store/<key>", methods=["POST"])
def update_object(key):
    value = request.form.get("value")
    if not value:
        abort(404)  
    if key not in storage:
        abort(404) 
    storage[key] = value
    return jsonify({key: value}), 200 
```

This route updates the value of an existing object.

---

#### **Question 4.6: Add a `/store/$x` route to delete an object**

```python
@app.route("/store/<key>", methods=["DELETE"])
def delete_object(key):
    if key in storage:
        del storage[key]
        return jsonify({"deleted"})  
    else:
        abort(404) 
```

This route removes the object associated with the given key.

---

#### **Question 4.7: Restrict modifications to authorized users**

```python
@app.before_request
def check_token():
    if request.method in ["POST", "DELETE"]:
        token = request.form.get("token")
        if token != "foobar":  
            abort(404)  
```



---

#### **Question 4.8: Use TLS for secure communication**

Added the following to enable TLS:

```python
app.run("0.0.0.0", port=8443, debug=False, ssl_context="adhoc")
```

This ensures that all communication is encrypted. Without TLS, sensitive data can be intercepted.

---

#### **Question 4.9: Add a route to save the store object to a file**

```python
@app.route("/store/save", methods=["POST"])
def save_store():
    with open("store.json", "w") as f:
        json.dump(storage, f)
    return jsonify({"Store saved"})

@app.before_first_request
def load_store():
    try:
        with open("store.json", "r") as f:
            global storage
            storage = json.load(f)
    except FileNotFoundError:
        pass  
```



---

#### **Question 4.10: Use `http.cat` for error handling**

Update error handling to use `http.cat` for user-friendly error pages:

```python
@app.errorhandler(404)
def not_found(error):
    return '<img src="https://http.cat/404" alt="404 Not Found">', 404

@app.errorhandler(403)
def forbidden(error):
    return '<img src="https://http.cat/403" alt="403 Forbidden">', 403

@app.errorhandler(400)
def bad_request(error):
    return '<img src="https://http.cat/400" alt="400 Bad Request">', 400
etc ...
```



---