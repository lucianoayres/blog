---
title: "Exploring HTTP in Python: socket vs. requests"
date: 2024-12-08
author: "Luciano Ayres"
---

Python's `requests` library is a favorite for handling HTTP requests, but have you ever wondered what it simplifies for you? I decided to dig deeper by crafting an HTTP GET request using the `socket` library to understand the details it abstracts.

Here is the raw `socket` implementation:

```python
import socket, ssl, json

host, port = "fakestoreapi.com", 443
with socket.create_connection((host, port)) as raw_socket:
    with ssl.create_default_context().wrap_socket(raw_socket, server_hostname=host) as s:
        s.sendall(b"GET /products HTTP/1.1\r\nHost: fakestoreapi.com\r\nConnection: close\r\n\r\n")
        response = b""
        while chunk := s.recv(4096):
            response += chunk

headers, body = response.decode().split("\r\n\r\n", 1)
print("Response Headers:\n", headers)
print("\nResponse Body:\n", json.dumps(json.loads(body), indent=4))
```

Now compare it to the same task using `requests`:

```python
import requests, json

response = requests.get("https://fakestoreapi.com/products")
print("Response Headers:\n", response.headers)
print("\nResponse Body:\n", json.dumps(response.json(), indent=4))
```

The `socket` approach involves manually handling connections, SSL wrapping, request creation, and response parsing. In contrast, `requests` manages all these aspects, allowing you to focus on the result rather than the process.

Building the request manually gave me a better understanding of HTTP, but for practical purposes, `requests` is the way to go. It simplifies the process while handling common issues like redirects and SSL errors efficiently. Understanding both methods, however, made me appreciate how much `requests` does behind the scenes.

### Try It Out

Test the code live in this [Google Colab notebook](https://colab.research.google.com/drive/1kWndt3AgXBPEdYyBdEv5YNcEHcRcYCsS?usp=sharing
). Explore both the `socket` and `requests` implementations and see the differences for yourself!
