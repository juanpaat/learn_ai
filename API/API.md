# Introduction to APIs in Python

## Table of Contents

1. [What is an API?](#what-is-an-api)
2. [Types of APIs](#types-of-apis)
3. [Types of Web APIs (the most common)](#types-of-web-apis-the-most-common)
4. [Working with APIs in Python](#working-with-apis-in-python)
5. [The Basic Anatomy of an API Request](#the-basic-anatomy-of-an-api-request)
6. [Adding Query Parameters with `requests`](#adding-query-parameters-with-requests)
7. [HTTP Verbs (Most Common)](#http-verbs-most-common)
8. [Request Message and Response Message](#request-message-and-response-message)
9. [Status Codes](#status-codes)
10. [Common Headers](#common-headers)
11. [API Authentication](#api-authentication)
12. [Working with Structured Data](#working-with-structured-data)
13. [Error Handling](#error-handling)

---

## What is an API?

An **API** (Application Programming Interface) is a way for two programs to talk to each other. Think of it like a waiter at a restaurant: you (the client) tell the waiter (the API) what you want, the waiter takes your order to the kitchen (the server), and then brings back the food (the response).

You use APIs every day without realizing it:
- When a weather app on your phone shows today's forecast, it's calling a weather API.
- When you log in to a website using Google, that site is using Google's API.
- When you pay online, the checkout page calls a payment API.

APIs let you use functionality built by someone else without needing to know how it works internally. You just need to know how to ask.

---

## Types of APIs

There are many kinds of APIs, depending on who can use them and where they live:

| Type | Description | Example |
|---|---|---|
| **Open / Public** | Available to anyone | OpenWeatherMap, NASA, GitHub |
| **Private / Internal** | Used only inside a company | Internal employee database |
| **Partner** | Shared between specific organizations | Payment processors like Stripe |
| **Local** | Runs on the same machine (no network) | OS file system API |
| **Remote / Web** | Accessed over the internet | Most APIs you'll use in Python |

For this guide, we focus on **remote web APIs**, which are by far the most common when building applications in Python.

---

## Types of Web APIs (the most common)

Web APIs come in a few different styles, each with its own conventions:

| Style | Description | Common use |
|---|---|---|
| **REST** | Uses standard HTTP methods. Resources are accessed via URLs. Returns JSON or XML. | The most common. Used by Twitter, GitHub, Spotify, etc. |
| **GraphQL** | Client asks for exactly the data it needs in a single request. | Used by GitHub v4, Shopify |
| **SOAP** | Older, XML-based, very strict. More common in enterprise/banking. | Legacy financial and government systems |
| **WebSocket** | Keeps a persistent, two-way connection open. Good for real-time data. | Chat apps, live stock prices |

Throughout this guide, we'll work with **REST APIs**, since they are the standard and the easiest to start with.

---

## Working with APIs in Python

Python makes it very easy to talk to web APIs using the `requests` library. It is not part of the standard library, so you need to install it first:

```bash
pip install requests
```

Here is the simplest possible example — fetching data from a public API and printing the result:

```python
import requests

response = requests.get('https://api.github.com')

print(response.status_code)  # 200 means OK
print(response.text)         # The raw response body as a string
```

That's it. In just three lines you contacted a server on the internet, sent a request, and received data back. Everything else in this guide builds on this foundation.

---

## The Basic Anatomy of an API Request

Every API request is made to a **URL** (Uniform Resource Locator). A URL is not just a web address — it is a structured set of instructions that tells the internet *how* to reach a specific resource. Each part plays a different role:

```
http://350.5th-ave.com:80/unit/243?floor=2&side=north
 ───┬─  ──────┬──────── ─┬ ────┬─── ─────────┬────────
    │         │          │     │              │
Protocol    Domain      Port  Path          Query
```

### Protocol — the means of transportation

The protocol tells the network *how* to send your request. It's like choosing whether to travel by car, plane, or boat.

- `http://` — HyperText Transfer Protocol. Unencrypted.
- `https://` — HTTP Secure. Encrypted with TLS. Always prefer this for real APIs.

### Domain — the street address of the office building

The domain identifies the server you want to reach. It's the building's address.

```
api.openweathermap.org
api.github.com
350.5th-ave.com
```

### Port — the gate or door to use when entering the building

The port specifies which channel on the server to connect to. Think of it as a specific entrance or reception desk inside the building.

- Port `80` is the default for HTTP (usually hidden in the URL).
- Port `443` is the default for HTTPS (also usually hidden).
- Other ports are used for specific services, e.g., `:3000` for local development.

### Path — the specific office unit inside the building

The path tells the server which resource you want — the specific room or department inside the building.

```
/unit/243
/users/42/posts
/api/v1/weather
```

### Query — any additional instructions

The query string starts with a `?` and contains key-value pairs separated by `&`. It lets you filter, sort, or customize your request — like adding a note with extra instructions.

```
?floor=2&side=north
?city=London&units=metric
?page=1&limit=10
```

### Full example

```
http://350.5th-ave.com/unit/243
```

> Go to the office building at 350 5th Ave, enter the main door (port 80), and find Unit 243.

---

## Adding Query Parameters with `requests`

You could manually build query strings in the URL, but the `requests` library gives you a cleaner way: pass a dictionary to the `params` argument.

```python
import requests

params = {
    'city': 'London',
    'units': 'metric',
    'appid': 'your_api_key_here'
}

response = requests.get('https://api.openweathermap.org/data/2.5/weather', params=params)

print(response.url)   # Shows the full URL that was built
print(response.text)  # The response body
```

`requests` automatically encodes the dictionary into a valid query string:

```
https://api.openweathermap.org/data/2.5/weather?city=London&units=metric&appid=your_api_key_here
```

This approach is safer and easier to read than building URLs by hand, especially when values contain spaces or special characters.

---

## HTTP Verbs (Most Common)

Every request you send to an API includes an **HTTP verb** (also called a method). The verb tells the server *what action* you want to perform on a resource.

Think of it like visiting an office in a building:

> **Destination:** Unit 243 of the 350 5th Ave office building
> **URL:** `http://350.5th-ave.com/unit/243`

Depending on why you're there, you might be:
- **Picking up** a document (GET)
- **Dropping off** a new file (POST)
- **Replacing** an existing file entirely (PUT)
- **Shredding** a document (DELETE)

### Actions / HTTP Methods

| Verb | Action | Description |
|---|---|---|
| `GET` | Read | Retrieve data. Should not change anything on the server. |
| `POST` | Create | Send new data to the server to create a resource. |
| `PUT` | Replace | Replace an existing resource entirely with new data. |
| `DELETE` | Remove | Delete a resource on the server. |

In `requests`, each verb has a matching function:

```python
import requests

requests.get('http://api.example.com/users')           # Read all users
requests.post('http://api.example.com/users', ...)     # Create a new user
requests.put('http://api.example.com/users/5', ...)    # Replace user 5
requests.delete('http://api.example.com/users/5')      # Delete user 5
```

---

## Request Message and Response Message

Every API interaction has two sides: what you **send** and what you **receive**.

### The Request

When you call `requests.get(...)`, Python builds and sends an HTTP request message. It contains:

- **Method** — the HTTP verb (GET, POST, etc.)
- **URL** — where to send the request
- **Headers** — metadata about the request (what format you accept, auth tokens, etc.)
- **Body** — optional data sent with the request (mainly used with POST and PUT)

### The Response

The server processes your request and sends back an HTTP response message. It contains:

- **Status code** — a number indicating success or failure (e.g., `200`, `404`)
- **Headers** — metadata about the response (content type, encoding, etc.)
- **Body** — the actual data you asked for (usually JSON or plain text)

In Python, the `response` object from `requests` gives you access to all of this:

```python
import requests

response = requests.get('https://api.github.com')

print(response.status_code)       # 200
print(response.headers)           # Dictionary of response headers
print(response.text)              # Response body as a string
print(response.json())            # Response body parsed as JSON (if applicable)
```

---

## Status Codes

Every response includes a **status code** — a 3-digit number that tells you whether the request worked and why. They are grouped by their first digit:

| Range | Category | Meaning |
|---|---|---|
| `1XX` | Informational | The request was received, still processing |
| `2XX` | Success | Everything worked |
| `3XX` | Redirection | You need to go somewhere else to get the resource |
| `4XX` | Client Error | Something is wrong with *your* request |
| `5XX` | Server Error | Something went wrong *on the server's side* |

### 1XX — Informational
Rarely seen in practice. The server is acknowledging receipt and continuing to process.

- `100 Continue` — The server received the request headers and the client should proceed.

### 2XX — Success
The most common group you want to see.

- `200 OK` — Standard success. Data is in the response body.
- `201 Created` — A new resource was successfully created (common after POST).
- `204 No Content` — Success, but there is no body to return (common after DELETE).

### 3XX — Redirection
The resource has moved. Your client should follow the new location.

- `301 Moved Permanently` — The URL has changed forever.
- `302 Found` — Temporary redirect to another URL.

### 4XX — Client Errors
Something is wrong with how you made the request. These are usually fixable on your end.

- `400 Bad Request` — The server couldn't understand your request. Check your syntax or parameters.
- `401 Unauthorized` — You need to authenticate (missing or invalid credentials).
- `403 Forbidden` — You are authenticated but don't have permission.
- `404 Not Found` — The resource doesn't exist at that URL.
- `429 Too Many Requests` — You've hit a rate limit. Slow down.

### 5XX — Server Errors
The server failed. This is not your fault and usually requires waiting or contacting the API provider.

- `500 Internal Server Error` — Something crashed on the server.
- `502 Bad Gateway` — The server got an invalid response from an upstream server.
- `503 Service Unavailable` — The server is overloaded or down for maintenance.

```python
import requests

response = requests.get('https://api.github.com/users/octocat')
print(response.status_code)  # 200

response = requests.get('https://api.github.com/users/this_user_does_not_exist_xyz')
print(response.status_code)  # 404
```

---

## Common Headers

Headers are key-value pairs sent in both requests and responses. They carry metadata — information about the message itself, not the data being exchanged.

### Reading response headers

```python
import requests

response = requests.get('http://localhost:3000/lyrics')

# Print a specific response header
print(response.headers['Content-Type'])

# Print all response headers
print(dict(response.headers))
```

### Sending request headers

You pass headers as a dictionary to the `headers` argument. A very common use case is telling the server what format you want back:

```python
import requests

# Ask the server to return JSON
headers = {"Accept": "application/json"}
response = requests.get('http://localhost:3000/lyrics', headers=headers)

print(response.text)
```

The `Accept` header tells the server: *"I can handle this format, please send it this way."*

### Common headers you'll encounter

| Header | Direction | Purpose |
|---|---|---|
| `Content-Type` | Request / Response | Describes the format of the body (`application/json`, `text/html`) |
| `Accept` | Request | Tells the server what format the client wants |
| `Authorization` | Request | Carries authentication credentials (API keys, tokens) |
| `User-Agent` | Request | Identifies the client software making the request |
| `Cache-Control` | Request / Response | Controls caching behavior |

---

## API Authentication

Not all APIs are open to the public. Many require you to prove who you are before they'll respond. This is called **authentication**.

Without authentication, anyone could use an API without limits, access private data, or abuse the service.

### Authentication Methods

#### Basic Authentication

The simplest form: you send a username and password with every request, encoded in Base64. Because credentials are sent with every request, HTTPS is essential.

```python
import requests

response = requests.get(
    'http://api.example.com/data',
    auth=('my_username', 'my_password')
)
```

#### API Key / Token Authentication

You receive a unique key when you register with the API. You include this key in every request — either in a header or as a query parameter.

```python
import requests

# Option 1: Pass the token in the Authorization header (most common)
headers = {"Authorization": "Bearer 8apDFHaNJMxy8Kt818aa6b4a0ed0514b5d3"}
response = requests.get('http://localhost:3000/albums', headers=headers)

# Option 2: Pass the token as a query parameter
params = {'access_token': '8apDFHaNJMxy8Kt818aa6b4a0ed0514b5d3'}
response = requests.get('http://localhost:3000/albums', params=params)
```

The `Bearer` prefix in the `Authorization` header is a convention that means: *"I'm presenting a bearer token — whoever holds this token has access."*

Which option to use depends on what the API documentation says. Headers are generally preferred because they don't appear in browser history or server logs.

#### JWT Authentication (JSON Web Token)

A JWT is a signed token that encodes information (like your user ID or role) directly inside it. The server doesn't need to look anything up — it just verifies the signature.

```python
import requests

jwt_token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."

headers = {"Authorization": f"Bearer {jwt_token}"}
response = requests.get('http://api.example.com/profile', headers=headers)
```

JWTs are widely used in modern web apps. You typically receive a JWT after logging in, and then include it in every subsequent request.

#### OAuth 2.0

OAuth 2.0 is used when you want to access a user's data on a third-party service on their behalf — without ever seeing their password. It involves a multi-step flow where the user grants permission.

Common example: "Log in with Google" or "Connect your Spotify account."

In Python, libraries like `requests-oauthlib` or `authlib` handle the OAuth flow for you:

```python
from requests_oauthlib import OAuth2Session

oauth = OAuth2Session(client_id='your_client_id')
# ... redirect user to authorization URL, receive token, then:
response = oauth.get('https://api.example.com/me')
```

OAuth is more complex but is the standard when working with APIs that act on behalf of real users.

---

## Working with Structured Data

APIs rarely return plain, unstructured text. They return structured data — information organized in a predictable format that your code can parse and work with.

The two most common formats are:

| Format | Description | When you'll see it |
|---|---|---|
| **JSON** | Lightweight, text-based, uses key-value pairs and lists. Easy to read and write. | The default for almost all modern REST APIs |
| **XML** | Tag-based, more verbose. Common in older or enterprise APIs. | SOAP APIs, some government APIs |

JSON looks like Python dictionaries and lists, which makes it very natural to work with in Python.

### Encoding and Decoding: the `json` package

**Decoding** (JSON → Python): converting a JSON string into Python objects (dict, list, etc.)
**Encoding** (Python → JSON): converting Python objects into a JSON string

Python's built-in `json` module handles both:

```python
import json

# Decoding: JSON string → Python dict
json_string = '{"name": "Alice", "age": 30}'
data = json.loads(json_string)
print(data['name'])  # Alice

# Encoding: Python dict → JSON string
python_dict = {"city": "London", "units": "metric"}
json_string = json.dumps(python_dict)
print(json_string)  # {"city": "London", "units": "metric"}
```

### Getting JSON from an API (GET)

The `requests` library has a built-in `.json()` method that decodes the response body for you:

```python
import requests

response = requests.get('https://api.github.com/users/octocat')

data = response.json()           # Returns a Python dict
print(data['name'])              # 'The Octocat'
print(data['public_repos'])      # Number of public repos
```

### Sending JSON to an API (POST)

When creating a new resource, you typically send data *to* the API. Pass a dictionary to the `json` argument — `requests` will encode it automatically and set the correct `Content-Type` header:

```python
import requests

new_user = {
    "name": "Alice",
    "email": "alice@example.com"
}

response = requests.post(
    'https://api.example.com/users',
    json=new_user              # requests encodes this as JSON for you
)

print(response.status_code)   # 201 if successfully created
print(response.json())        # The server's response (often the created resource)
```

> Note: Use `json=` (not `data=`) when sending JSON. Using `json=` automatically sets `Content-Type: application/json`.

---

## Error Handling

When working with APIs, things will go wrong. The network might fail, the server might be down, or your request might be malformed. Good error handling makes the difference between a program that crashes unexpectedly and one that fails gracefully.

### 4XX vs 5XX Errors

| Category | Who is responsible | What to do |
|---|---|---|
| **4XX Client Errors** | You (the requester) | Fix your request — wrong URL, missing auth, bad data, etc. |
| **5XX Server Errors** | The API provider | Wait and retry, or contact support |

Most common errors you'll encounter:

| Code | Name | Cause | Fix |
|---|---|---|---|
| `400` | Bad Request | Malformed request, invalid parameters | Check your request body and parameters |
| `401` | Unauthorized | Missing or invalid credentials | Add or fix your API key / token |
| `403` | Forbidden | Valid credentials but not enough permission | Check your account's access level |
| `404` | Not Found | Wrong URL or resource doesn't exist | Double-check the endpoint path |
| `429` | Too Many Requests | Rate limit exceeded | Add delays or request limit increases |
| `500` | Internal Server Error | Bug or crash on the server | Not your fault — try again later |
| `502` | Bad Gateway | Upstream server issue | Retry after a short wait |
| `503` | Service Unavailable | Server is overloaded or in maintenance | Retry later |

### Basic error handling with `if` statements

```python
import requests

response = requests.get('https://api.github.com/users/octocat')

if response.status_code == 200:
    data = response.json()
    print(data['name'])
elif response.status_code == 404:
    print("User not found.")
elif response.status_code == 401:
    print("Authentication required. Check your API key.")
else:
    print(f"Unexpected error: {response.status_code}")
```

### `raise_for_status()`

Checking status codes manually for every request gets tedious. The `requests` library provides a convenient shortcut: `.raise_for_status()`.

It does nothing if the request was successful (2XX), but **raises an exception** for 4XX and 5XX status codes. This lets you handle errors with `try/except`:

```python
import requests

try:
    response = requests.get('https://api.github.com/users/octocat')
    response.raise_for_status()   # Raises an exception if status >= 400

    data = response.json()
    print(data['name'])

except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e}")           # e.g. 404 Client Error: Not Found
except requests.exceptions.ConnectionError:
    print("Could not connect. Check your internet connection.")
except requests.exceptions.Timeout:
    print("The request timed out.")
except requests.exceptions.RequestException as e:
    print(f"Something went wrong: {e}")
```

Using `raise_for_status()` is considered best practice. It keeps your code clean and ensures errors are never silently ignored.

> **Tip:** Always wrap API calls in `try/except` when writing production code. Networks are unreliable and APIs change.
