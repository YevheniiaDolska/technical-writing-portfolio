# HTTPX Python SDK Documentation

A comprehensive guide to HTTPX - the modern, async-capable HTTP client for Python.

---

## Overview

**HTTPX** is a fully-featured HTTP client library for Python 3 that provides sync and async APIs, HTTP/1.1 and HTTP/2 support, and a familiar requests-compatible interface.

!!! info "Why HTTPX?"
    - **Async support**: Native `async`/`await` for high-performance applications
    - **HTTP/2**: Built-in HTTP/2 support for faster connections
    - **Type hints**: Full type annotation coverage
    - **Timeout handling**: Fine-grained timeout configuration
    - **Connection pooling**: Efficient connection reuse

---

## Installation

=== "pip"

    ```bash
    pip install httpx
    ```

=== "pip with HTTP/2"

    ```bash
    pip install httpx[http2]
    ```

=== "poetry"

    ```bash
    poetry add httpx
    ```

=== "conda"

    ```bash
    conda install -c conda-forge httpx
    ```

**Requirements:**

- Python 3.8+
- No additional dependencies for basic usage

---

## Quick Start

### Basic GET Request

```python
import httpx

# Simple GET request
response = httpx.get("https://api.example.com/users")
print(response.status_code)  # 200
print(response.json())       # {'users': [...]}
```

### POST with JSON Body

```python
import httpx

data = {
    "name": "John Doe",
    "email": "john@example.com"
}

response = httpx.post(
    "https://api.example.com/users",
    json=data
)

print(response.status_code)  # 201
print(response.json())       # {'id': 123, 'name': 'John Doe', ...}
```

### Async Request

```python
import httpx
import asyncio

async def fetch_users():
    async with httpx.AsyncClient() as client:
        response = await client.get("https://api.example.com/users")
        return response.json()

# Run async function
users = asyncio.run(fetch_users())
```

---

## Client Configuration

### Using a Client Instance

For multiple requests, use a `Client` instance to benefit from connection pooling:

```python
import httpx

# Synchronous client
with httpx.Client() as client:
    response1 = client.get("https://api.example.com/users")
    response2 = client.get("https://api.example.com/posts")

# Async client
async with httpx.AsyncClient() as client:
    response = await client.get("https://api.example.com/users")
```

!!! warning "Always close clients"
    Use context managers (`with`/`async with`) or call `client.close()` to release connections properly.

### Base URL Configuration

```python
import httpx

client = httpx.Client(base_url="https://api.example.com/v1")

# Now you can use relative URLs
response = client.get("/users")      # GET https://api.example.com/v1/users
response = client.get("/posts/123")  # GET https://api.example.com/v1/posts/123
```

### Default Headers

```python
import httpx

client = httpx.Client(
    base_url="https://api.example.com",
    headers={
        "Authorization": "Bearer your-token-here",
        "Accept": "application/json",
        "User-Agent": "MyApp/1.0"
    }
)
```

---

## Authentication

### Bearer Token

```python
import httpx

client = httpx.Client(
    headers={"Authorization": "Bearer your-access-token"}
)
```

### Basic Auth

```python
import httpx

# Using tuple
response = httpx.get(
    "https://api.example.com/protected",
    auth=("username", "password")
)

# Using BasicAuth class
auth = httpx.BasicAuth("username", "password")
response = httpx.get("https://api.example.com/protected", auth=auth)
```

### Custom Authentication

```python
import httpx

class APIKeyAuth(httpx.Auth):
    def __init__(self, api_key: str):
        self.api_key = api_key

    def auth_flow(self, request):
        request.headers["X-API-Key"] = self.api_key
        yield request

# Usage
auth = APIKeyAuth("your-api-key")
client = httpx.Client(auth=auth)
```

---

## Request Methods

### GET with Query Parameters

```python
import httpx

# Method 1: params argument
response = httpx.get(
    "https://api.example.com/search",
    params={
        "q": "python",
        "page": 1,
        "limit": 20
    }
)
# Result: GET https://api.example.com/search?q=python&page=1&limit=20

# Method 2: List for multiple values
response = httpx.get(
    "https://api.example.com/items",
    params={"tag": ["python", "async", "http"]}
)
# Result: GET https://api.example.com/items?tag=python&tag=async&tag=http
```

### POST with Different Content Types

=== "JSON"

    ```python
    response = httpx.post(
        "https://api.example.com/users",
        json={"name": "John", "email": "john@example.com"}
    )
    ```

=== "Form Data"

    ```python
    response = httpx.post(
        "https://api.example.com/login",
        data={"username": "john", "password": "secret"}
    )
    ```

=== "File Upload"

    ```python
    files = {"file": open("document.pdf", "rb")}
    response = httpx.post(
        "https://api.example.com/upload",
        files=files
    )
    ```

=== "Mixed (File + Data)"

    ```python
    response = httpx.post(
        "https://api.example.com/upload",
        data={"description": "My document"},
        files={"file": ("doc.pdf", open("doc.pdf", "rb"), "application/pdf")}
    )
    ```

### PUT, PATCH, DELETE

```python
import httpx

# PUT - Full update
response = httpx.put(
    "https://api.example.com/users/123",
    json={"name": "John Updated", "email": "john.new@example.com"}
)

# PATCH - Partial update
response = httpx.patch(
    "https://api.example.com/users/123",
    json={"name": "John Updated"}
)

# DELETE
response = httpx.delete("https://api.example.com/users/123")
```

---

## Response Handling

### Response Properties

```python
import httpx

response = httpx.get("https://api.example.com/users/123")

# Status
response.status_code      # 200
response.is_success       # True (2xx status)
response.is_redirect      # False
response.is_client_error  # False (4xx status)
response.is_server_error  # False (5xx status)

# Headers
response.headers          # Headers({'content-type': 'application/json', ...})
response.headers["content-type"]  # 'application/json'

# Content
response.text             # '{"id": 123, "name": "John"}'
response.json()           # {'id': 123, 'name': 'John'}
response.content          # b'{"id": 123, "name": "John"}'

# Request info
response.url              # URL('https://api.example.com/users/123')
response.request          # <Request('GET', 'https://...')>
```

### Raise for Status

```python
import httpx

response = httpx.get("https://api.example.com/users/999")

# Raise exception for 4xx/5xx responses
response.raise_for_status()  # Raises httpx.HTTPStatusError if not 2xx
```

### Handling Errors

```python
import httpx

try:
    response = httpx.get("https://api.example.com/users")
    response.raise_for_status()
    data = response.json()

except httpx.ConnectError:
    print("Failed to connect to server")

except httpx.TimeoutException:
    print("Request timed out")

except httpx.HTTPStatusError as e:
    print(f"HTTP error {e.response.status_code}: {e.response.text}")

except httpx.RequestError as e:
    print(f"Request failed: {e}")
```

---

## Timeout Configuration

### Simple Timeout

```python
import httpx

# 10 second timeout for everything
response = httpx.get(
    "https://api.example.com/slow-endpoint",
    timeout=10.0
)
```

### Fine-Grained Timeouts

```python
import httpx

timeout = httpx.Timeout(
    connect=5.0,    # Time to establish connection
    read=10.0,      # Time to receive response
    write=5.0,      # Time to send request
    pool=5.0        # Time to acquire connection from pool
)

client = httpx.Client(timeout=timeout)
```

### Disable Timeout

```python
import httpx

# Not recommended for production
response = httpx.get(
    "https://api.example.com/endpoint",
    timeout=None
)
```

---

## HTTP/2 Support

```python
import httpx

# Enable HTTP/2 (requires httpx[http2])
client = httpx.Client(http2=True)

response = client.get("https://http2.example.com")
print(response.http_version)  # 'HTTP/2'
```

---

## Async Usage

### Concurrent Requests

```python
import httpx
import asyncio

async def fetch_all():
    async with httpx.AsyncClient() as client:
        # Create tasks for concurrent execution
        tasks = [
            client.get("https://api.example.com/users"),
            client.get("https://api.example.com/posts"),
            client.get("https://api.example.com/comments"),
        ]

        # Wait for all requests to complete
        responses = await asyncio.gather(*tasks)

        return [r.json() for r in responses]

# Run
results = asyncio.run(fetch_all())
users, posts, comments = results
```

### Streaming Responses

```python
import httpx

async def download_file():
    async with httpx.AsyncClient() as client:
        async with client.stream("GET", "https://example.com/large-file.zip") as response:
            with open("large-file.zip", "wb") as f:
                async for chunk in response.aiter_bytes(chunk_size=8192):
                    f.write(chunk)
```

---

## API Reference

### Client Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `base_url` | `str` | `None` | Base URL for all requests |
| `headers` | `dict` | `None` | Default headers for all requests |
| `auth` | `Auth` | `None` | Authentication handler |
| `timeout` | `Timeout` | `5.0` | Request timeout configuration |
| `http2` | `bool` | `False` | Enable HTTP/2 support |
| `verify` | `bool` | `True` | Verify SSL certificates |
| `follow_redirects` | `bool` | `False` | Automatically follow redirects |

### Request Methods

| Method | Description |
|--------|-------------|
| `client.get(url, **kwargs)` | Send GET request |
| `client.post(url, **kwargs)` | Send POST request |
| `client.put(url, **kwargs)` | Send PUT request |
| `client.patch(url, **kwargs)` | Send PATCH request |
| `client.delete(url, **kwargs)` | Send DELETE request |
| `client.head(url, **kwargs)` | Send HEAD request |
| `client.options(url, **kwargs)` | Send OPTIONS request |

### Common Request Arguments

| Argument | Type | Description |
|----------|------|-------------|
| `params` | `dict` | URL query parameters |
| `json` | `dict` | JSON request body |
| `data` | `dict` | Form-encoded request body |
| `files` | `dict` | File uploads |
| `headers` | `dict` | Request headers |
| `auth` | `Auth` | Request authentication |
| `timeout` | `float` | Request timeout |

---

## Error Types

| Exception | Description |
|-----------|-------------|
| `httpx.RequestError` | Base class for all request errors |
| `httpx.ConnectError` | Failed to establish connection |
| `httpx.ConnectTimeout` | Connection attempt timed out |
| `httpx.ReadTimeout` | Timeout while reading response |
| `httpx.WriteTimeout` | Timeout while sending request |
| `httpx.PoolTimeout` | Timeout waiting for connection from pool |
| `httpx.HTTPStatusError` | Response has 4xx or 5xx status code |
| `httpx.TooManyRedirects` | Maximum redirects exceeded |

---

## Best Practices

!!! tip "Performance Tips"

    1. **Use a Client instance** for multiple requests to benefit from connection pooling
    2. **Enable HTTP/2** when the server supports it for better performance
    3. **Set appropriate timeouts** to prevent hanging requests
    4. **Use async** for I/O-bound applications with many concurrent requests

!!! warning "Common Pitfalls"

    1. **Not closing clients** - Always use context managers or call `close()`
    2. **Ignoring timeouts** - The default 5s timeout may be too short for some APIs
    3. **Not checking status codes** - Always verify `response.is_success` or use `raise_for_status()`

---

## Migration from Requests

If you're coming from the `requests` library, here's a quick comparison:

| requests | httpx |
|----------|-------|
| `requests.get(url)` | `httpx.get(url)` |
| `requests.Session()` | `httpx.Client()` |
| `response.ok` | `response.is_success` |
| N/A | `httpx.AsyncClient()` |
| N/A | HTTP/2 support |

Most code using `requests` can be migrated to `httpx` with minimal changes.

---

## Additional Resources

- [Official Documentation](https://www.python-httpx.org/)
- [GitHub Repository](https://github.com/encode/httpx)
- [HTTP/2 Guide](https://www.python-httpx.org/http2/)

---

*This SDK documentation was created as a portfolio sample to demonstrate technical writing skills for developer-focused documentation.*
