# miniroute

**miniroute** is a minimal HTTP routing framework based on Python’s built-in `http.server`, designed for lightweight local servers and IPC tooling.
This module is made for users familiar with the `http.server` module, with which it shares the same qualities and flaws.

## Features

- Simple route declaration using decorators
- Pure standard library
- No magic, no dependencies
- Good for local-only HTTP endpoints

## Installation

```bash
pip install miniroute
```

## Usage

```python
from miniroute import Miniroute
import json

app = Miniroute()

@app.router.get("/hello")
def hello(handler):
    body = json.dumps({"message": "Hello, world!"}).encode()
    headers = [("Content-Type", "application/json")]
    return 200, headers, body

@app.router.post("/echo")
def echo(handler):
    length = int(handler.headers.get("Content-Length", 0))
    data = handler.rfile.read(length)
    return 200, [("Content-Type", "application/json")], data

if __name__ == "__main__":
    app.run()
```

## Route handlers

Each route function:

- Receives the current `BaseHTTPRequestHandler` as argument
- Must return a tuple:
  `status_code: int, headers: dict[str, str], body: bytes`

Example:

```python
return 200, [("Content-Type", "text/plain")], b"OK"
```

## License

PSF License
You are free to use, modify, and distribute this software under the terms of the Python Software Foundation License.

> This project is not affiliated with or endorsed by the Python Software Foundation.

