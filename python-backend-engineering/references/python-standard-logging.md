# Python Backend Engineering Standard — Logging

## 1. Core Principles

Logging is an **infrastructure concern** that must be configured centrally and consumed consistently across all layers.

### Key Rules
1. Logging configuration must be centralized in `logging.conf` (text) or a JSON formatter for structured runtimes.
2. Loggers must be obtained through `logging_resolver.py`, which loads the configuration.
3. Log entries must include class, method, timestamp, and line number when configured.
4. Domain and Application layers must not know about logging implementation details.
5. Sensitive information must never be logged.
6. Log levels must be configurable without code changes.

### Domain Layer Logging — Pragmatic Exception

The architecture standard requires that the domain layer has no infrastructure dependencies. Logging sits at the boundary of this rule: obtaining a logger via `get_logger(__name__)` is generally accepted as a pragmatic exception because the stdlib `logging` module is a language primitive, not a framework.

However, **do not log from the domain** when:
- the log event is really an application-level observability concern (e.g., "use case started"), or
- the log would couple domain behavior to a specific observability format.

**Preferred patterns by context:**

| Context | Preferred approach |
|---|---|
| Domain invariant violation | Raise a domain exception; let the application or exception handler log it. |
| Meaningful state transition | Domain may log via `get_logger(__name__)` — this is acceptable. |
| Use-case outcome | Log in the application layer, not the domain. |
| Audit trail / event sourcing | Emit a domain event; infrastructure subscribes and records it. |

The goal is that the domain remains independently testable and free of framework logging config. Using `logging.getLogger` does not violate that goal; importing a log formatter or a custom observability library does.

---

## 2. logging_resolver.py

> **Note:** This file was previously called `resolve_path.py`. It has been renamed to `logging_resolver.py` to accurately reflect its responsibility as a logger factory.

### Implementation

```python
# infrastructure/logging_resolver.py
import logging
import logging.config
from pathlib import Path

class LoggingResolver:
    """Finds and loads logging configuration, then exposes a logger factory."""

    def __init__(self) -> None:
        self.config_path = self._find_config()
        self._load_config()

    def _find_config(self) -> Path:
        """Find logging.conf in common project locations."""
        candidates = [
            Path.cwd() / "logging.conf",
            Path.cwd() / "config" / "logging.conf",
            Path(__file__).parent / "logging.conf",
            Path(__file__).parent.parent / "logging.conf",
        ]
        for path in candidates:
            if path.exists():
                return path
        return Path.cwd() / "logging.conf"

    def _load_config(self) -> None:
        """Load logging configuration from file, falling back to basicConfig."""
        if self.config_path.exists():
            try:
                logging.config.fileConfig(self.config_path, disable_existing_loggers=False)
            except Exception as e:
                print(f"Warning: Could not load logging config from {self.config_path}: {e}")
                self._fallback()
        else:
            self._fallback()

    def _fallback(self) -> None:
        logging.basicConfig(
            level=logging.INFO,
            format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
        )


_resolver: LoggingResolver | None = None


def get_logger(name: str) -> logging.Logger:
    """Logger factory. Ensures configuration is loaded before returning a logger."""
    global _resolver
    if _resolver is None:
        _resolver = LoggingResolver()
    return logging.getLogger(name)


# Backward-compatible alias
get_configured_logger = get_logger
```

---

## 3. logging.conf — Plaintext Format (non-Lambda)

Use this for local development, Django, Flask, or any runtime that writes to files or a terminal.

```ini
[loggers]
keys=root,app,domain,infrastructure,entrypoints

[handlers]
keys=consoleHandler,fileHandler

[formatters]
keys=default,detailed

[logger_root]
level=INFO
handlers=consoleHandler

[logger_app]
level=DEBUG
handlers=consoleHandler,fileHandler
qualname=app
propagate=0

[logger_domain]
level=DEBUG
handlers=consoleHandler
qualname=domain
propagate=0

[logger_infrastructure]
level=INFO
handlers=fileHandler
qualname=infrastructure
propagate=0

[logger_entrypoints]
level=INFO
handlers=consoleHandler
qualname=entrypoints
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=default
args=(sys.stdout,)

[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=detailed
args=('app.log', 'a')

[formatter_default]
format=%(asctime)s - %(name)s - %(levelname)s - %(module)s - %(funcName)s - line %(lineno)d - %(message)s
datefmt=%Y-%m-%d %H:%M:%S

[formatter_detailed]
format=%(asctime)s - [%(levelname)s] - %(name)s - %(funcName)s - line %(lineno)d - %(message)s
datefmt=%Y-%m-%d %H:%M:%S.%f
```

---

## 4. Structured / JSON Logging (Lambda and CloudWatch)

AWS Lambda writes to CloudWatch Logs. CloudWatch can parse structured JSON natively, enabling log filtering, metric extraction, and tracing correlation without regex. **Use JSON logging for all Lambda functions.**

### When to use JSON logging

| Runtime | Recommended format |
|---|---|
| AWS Lambda | JSON (structured) |
| ECS / Fargate with CloudWatch | JSON (structured) |
| Local development | Plaintext (human-readable) |
| Django / Flask with file logging | Plaintext or JSON depending on log aggregator |

### JSON formatter setup (no extra dependencies)

```python
# infrastructure/json_logging.py
import json
import logging
import traceback
from datetime import datetime, timezone


class JsonFormatter(logging.Formatter):
    """
    Emits log records as single-line JSON.
    Compatible with CloudWatch Logs Insights and OpenSearch.
    """

    def format(self, record: logging.LogRecord) -> str:
        payload: dict = {
            "timestamp": datetime.fromtimestamp(record.created, tz=timezone.utc).isoformat(),
            "level": record.levelname,
            "logger": record.name,
            "module": record.module,
            "function": record.funcName,
            "line": record.lineno,
            "message": record.getMessage(),
        }

        # Correlation IDs injected via LoggerAdapter extra (see section 5)
        for key in ("request_id", "correlation_id", "trace_id"):
            if hasattr(record, key):
                payload[key] = getattr(record, key)

        if record.exc_info:
            payload["exception"] = traceback.format_exception(*record.exc_info)

        return json.dumps(payload, default=str)


def configure_json_logging(level: str = "INFO") -> None:
    """
    Replace all handlers on the root logger with a single JSON stdout handler.
    Call this once at the Lambda module level (outside the handler function)
    so it runs during the cold start and is reused across warm invocations.
    """
    handler = logging.StreamHandler()
    handler.setFormatter(JsonFormatter())

    root = logging.getLogger()
    root.handlers.clear()
    root.addHandler(handler)
    root.setLevel(getattr(logging, level.upper(), logging.INFO))
```

### Lambda handler bootstrap

```python
# entrypoints/lambdas/my_handler.py
import os
from infrastructure.json_logging import configure_json_logging
from infrastructure.logging_resolver import get_logger

# Cold-start: configure once, reused on warm invocations
configure_json_logging(level=os.getenv("LOG_LEVEL", "INFO"))

logger = get_logger(__name__)


def handler(event: dict, context) -> dict:
    logger.info("Lambda invoked", extra={"request_id": context.aws_request_id})
    # ... delegate to use case
```

### Environment-aware resolver

When the code must run both locally and in Lambda, detect the runtime and apply the correct format:

```python
# infrastructure/logging_resolver.py  (extended version)
import os

def setup_logging() -> None:
    """Configure logging based on the current runtime environment."""
    if os.getenv("AWS_LAMBDA_FUNCTION_NAME"):
        from infrastructure.json_logging import configure_json_logging
        configure_json_logging(level=os.getenv("LOG_LEVEL", "INFO"))
    else:
        # falls through to fileConfig / basicConfig path in LoggingResolver
        LoggingResolver()
```

Call `setup_logging()` once at the composition root or Lambda module scope.

---

## 5. LoggerAdapter — Thread-Safe Context Enrichment

> **Replaces the previous `LogContext` pattern.**
>
> `LogContext` used `logging.setLogRecordFactory()`, which is **process-global and not thread-safe**. In any async or multi-threaded runtime (FastAPI, Lambda with concurrency, Celery), two concurrent requests would overwrite each other's factory, producing incorrect log context. `LoggerAdapter` is the correct tool: it enriches only the logger instances it wraps, without touching global state.

### Usage

```python
import logging
from infrastructure.logging_resolver import get_logger


class ContextLogger:
    """
    Wraps a standard logger with per-instance extra context.
    Safe for concurrent use — does not modify global logging state.
    """

    def __init__(self, name: str, extra: dict | None = None) -> None:
        base = get_logger(name)
        self._adapter = logging.LoggerAdapter(base, extra or {})

    def with_context(self, **kwargs) -> "ContextLogger":
        """Return a new ContextLogger with additional key-value context merged in."""
        merged = {**self._adapter.extra, **kwargs}
        new = ContextLogger.__new__(ContextLogger)
        new._adapter = logging.LoggerAdapter(self._adapter.logger, merged)
        return new

    def __getattr__(self, name: str):
        return getattr(self._adapter, name)
```

### Example — application layer

```python
class CreateOrderUseCase:
    def __init__(self, repository: OrderRepository) -> None:
        self.repository = repository
        self._logger = ContextLogger("application.orders")

    def execute(self, command: CreateOrderCommand) -> OrderResult:
        log = self._logger.with_context(customer_id=str(command.customer_id))
        log.info("Creating order")
        order = Order.create(command.customer_id, command.items)
        self.repository.save(order)
        log.info("Order created", extra={"order_id": str(order.id)})
        return OrderResult(order_id=order.id)
```

In JSON output this produces:
```json
{"level": "INFO", "logger": "application.orders", "message": "Order created", "customer_id": "...", "order_id": "..."}
```

---

## 6. Layer-Specific Guidelines

### Domain Layer

Apply the pragmatic exception described in Section 1. Log meaningful state transitions only. Raise domain exceptions for invariant violations — let the application or exception handler log those.

```python
class Order:
    def cancel(self) -> None:
        logger = get_logger(__name__)

        if self.status == OrderStatus.SHIPPED:
            # Do NOT log here — raise and let the application layer observe it
            raise OrderCannotBeCancelled()

        self.status = OrderStatus.CANCELLED
        logger.info("Order cancelled", extra={"order_id": self.id})
```

### Application Layer

```python
class CreateOrderUseCase:
    def __init__(self, repository: OrderRepository) -> None:
        self.repository = repository
        self._logger = ContextLogger("application.orders")

    def execute(self, command: CreateOrderCommand) -> OrderResult:
        log = self._logger.with_context(customer_id=str(command.customer_id))
        log.info("Use case started")
        # ... business logic ...
        log.info("Use case completed", extra={"order_id": str(order.id)})
        return OrderResult(order_id=order.id)
```

### Infrastructure Layer

```python
class DynamoDBOrderRepository:
    def __init__(self, table_name: str) -> None:
        self.table_name = table_name
        self._logger = get_logger("infrastructure.dynamodb")

    async def get_by_id(self, order_id: str) -> Order | None:
        self._logger.debug("Fetching order", extra={"order_id": order_id})
        try:
            # DynamoDB operations...
            return result
        except Exception:
            self._logger.error("DynamoDB read failed", exc_info=True, extra={"order_id": order_id})
            raise
```

### Entrypoint Layer

```python
@router.post("/orders")
async def create_order(request: CreateOrderRequest):
    logger = get_logger("entrypoints.api")
    logger.info("Order request received", extra={"customer_id": str(request.customer_id)})

    try:
        result = use_case.execute(command)
        logger.info("Order created", extra={"order_id": str(result.order_id)})
        return {"order_id": result.order_id}
    except ValidationError as e:
        logger.warning("Validation failed", extra={"detail": str(e)})
        raise HTTPException(status_code=400, detail=str(e))
    except Exception:
        logger.error("Unexpected error", exc_info=True)
        raise HTTPException(status_code=500)
```

---

## 7. Best Practices

### DO ✅
- Use `get_logger(__name__)` consistently.
- Use `LoggerAdapter` / `ContextLogger` to attach request or correlation context.
- Log meaningful business events and state changes.
- Use appropriate levels: DEBUG, INFO, WARNING, ERROR, CRITICAL.
- Include context (IDs, state, relevant data) as `extra` fields — especially in JSON mode.
- Log exceptions with `exc_info=True`.
- Guard expensive debug operations:
  ```python
  if logger.isEnabledFor(logging.DEBUG):
      payload = render_debug_payload(data)
      logger.debug("Payload details", extra={"payload": payload})
  ```
- Use JSON logging for Lambda / CloudWatch runtimes.
- Configure logging once at module/composition-root scope for Lambda.

### DON'T ❌
- Never log sensitive information (passwords, tokens, secrets, PII).
- Don't hardcode log levels.
- Don't use `print()` for logging.
- Don't over-log in tight loops.
- Don't use `logging.setLogRecordFactory()` for per-request context — use `LoggerAdapter`.
- Don't configure logging inside use cases or domain objects.
- Don't log framework internals.

---

## 8. Testing Logging

### Using caplog

```python
import logging

def test_order_cancellation_logged(caplog):
    caplog.set_level(logging.INFO)

    order = Order(id="order-123", status=OrderStatus.PENDING)
    order.cancel()

    assert any("Order cancelled" in r.message for r in caplog.records)
```

### Using LoggerAdapter / ContextLogger in tests

```python
from unittest.mock import MagicMock, patch

def test_use_case_logs_start():
    mock_repo = MagicMock()

    with patch("application.orders.create_order.get_logger") as mock_get_logger:
        mock_logger = MagicMock()
        mock_get_logger.return_value = mock_logger

        use_case = CreateOrderUseCase(mock_repo)
        use_case.execute(CreateOrderCommand(customer_id="cust-1", items=[]))

        mock_logger.info.assert_any_call("Use case started")
```

---

## 9. Logging Checklist

### Configuration
- [ ] `logging_resolver.py` implemented and used as the single logger factory.
- [ ] JSON logging configured for Lambda / CloudWatch runtimes.
- [ ] Plaintext logging configured for local / non-Lambda runtimes.
- [ ] Log levels are environment-configurable (e.g., `LOG_LEVEL` env var).
- [ ] `logging.setLogRecordFactory()` is NOT used for per-request context.

### Implementation
- [ ] Domain logs only meaningful state transitions; invariant violations are raised, not logged.
- [ ] Application logs use-case boundaries (start, success, failure).
- [ ] Infrastructure logs technical operations with IDs and `exc_info=True` on errors.
- [ ] Entrypoints log request/response boundaries.
- [ ] `LoggerAdapter` / `ContextLogger` used for correlation ID propagation.

### Quality
- [ ] No sensitive information logged.
- [ ] Appropriate log levels used.
- [ ] Exceptions logged with `exc_info=True`.
- [ ] Performance impact considered (guard clauses on DEBUG).
- [ ] Logs are useful for debugging and production incident investigation.
