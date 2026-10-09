# Tracelify Python SDK

[![PyPI version](https://img.shields.io/pypi/v/tracelify-sdk)](https://pypi.org/project/tracelify-sdk/)
[![Python 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The official Python SDK for **Tracelify** — capture exceptions, track events, and monitor your applications in production with zero friction.

---

## Features

- **Automatic exception capture** — unhandled errors are caught and reported globally, no extra wrapping needed
- **Rich context** — attach user identity, custom tags, and breadcrumbs to every event
- **Non-blocking transport** — events are queued and sent asynchronously; your app never stalls on network I/O
- **DSN-based setup** — one config line, works with any Tracelify project
- **Multi-language** — Python, JavaScript, Go, Java, Kotlin, C++ SDKs included in one repo

---

## Installation

```bash
pip install tracelify-sdk
```

---

## Quick Start

```python
from tracelify import Tracelify

sdk = Tracelify(
    dsn="http://<public_key>@localhost:8000/api/<project_id>",
    release="1.0.0"
)

# Capture a handled exception
try:
    result = 1 / 0
except ZeroDivisionError as e:
    sdk.capture_exception(e)

# Enrich events with context
sdk.set_user({"id": "42", "email": "user@example.com"})
sdk.set_tag("environment", "production")
sdk.add_breadcrumb("User clicked checkout")

# Flush before exit to avoid dropping queued events
sdk.flush()
```

Unhandled exceptions are captured **automatically** — the SDK installs a global exception handler at initialization.

---

## API Reference

### `Tracelify(dsn, release)`

Initialize the SDK. Call once at application startup.

| Parameter | Type | Description |
|-----------|------|-------------|
| `dsn` | `str` | Your project DSN from the Tracelify dashboard |
| `release` | `str` | App version or release identifier |

### `capture_exception(error, stacktrace=None, level="error", fingerprint=None)`

Capture an exception and queue it for delivery.

| Parameter | Type | Description |
|-----------|------|-------------|
| `error` | `Exception` | The exception to report |
| `stacktrace` | `str \| list` | Custom stacktrace; defaults to `error.__traceback__` |
| `level` | `str` | Severity: `"error"`, `"warning"`, `"info"` |
| `fingerprint` | `any` | Custom grouping key for deduplication |

### `set_user(user: dict)`

Attach a user to all subsequent events. Common keys: `id`, `email`, `username`.

### `set_tag(key: str, value: str)`

Attach a key-value tag to all subsequent events.

### `add_breadcrumb(message: str)`

Append a breadcrumb message to the current scope's event trail.

### `flush(timeout: float = 5.0)`

Block until all queued events have been delivered (or `timeout` seconds elapses). Always call before `sys.exit()` in scripts.

---

## DSN Format

```
http://<public_key>@<host>:<port>/api/<project_id>
```

Find your DSN under **Project Settings → SDK Setup** in the Tracelify dashboard.

---

## How It Works

```
Your Application
       │
       ├─ capture_exception() ──► In-Memory Queue (thread-safe)
       │                                  │
       └─ set_user / set_tag /            └─► Background Worker Thread
          add_breadcrumb                          │
                                                  └─► POST /api/<project_id>/events
                                                            │
                                                      Tracelify Backend
```

Events are placed in a non-blocking queue. A background worker drains the queue and ships events to the Tracelify ingest endpoint. Your application thread is never delayed by network latency.

---

## Multi-Language SDKs

| Language   | Location        | Status    |
|------------|-----------------|-----------|
| Python     | `tracelify/`    | Stable    |
| JavaScript | `javascript/`   | Available |
| Go         | `go/`           | Available |
| Java       | `java/`         | Available |
| Kotlin     | `kotlin/`       | Available |
| C++        | `cpp/`          | Available |
| Mobile     | `Mobileapp/`    | Available |

---

## Running the Backend

A reference backend and Docker Compose setup are included in this repo:

```bash
cp .env.example .env
docker compose up --build
```

---

## License

MIT — see [LICENSE](LICENSE).