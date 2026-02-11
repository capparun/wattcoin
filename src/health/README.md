# Health Check Endpoint

## Bounty
- **Issue**: https://github.com/WattCoin-Org/wattcoin/issues/90
- **Reward**: 2,000 WATT
- **Status**: Completed ✅

## Features

- ✅ **Comprehensive Health Checks**
  - API service status
  - Database connectivity
  - External service dependencies

- ✅ **Standardized Response Format**
  ```json
  {
    "status": "healthy|degraded|unhealthy",
    "timestamp": "2026-02-11T16:00:00Z",
    "uptime_seconds": 3600,
    "version": "1.0.0",
    "checks": {
      "api": { "status": "healthy", "response_time_ms": 10 },
      "database": { "status": "healthy", "response_time_ms": 50 },
      "external": { "status": "healthy", "response_time_ms": 100 }
    }
  }
  ```

- ✅ **Async Support** - Non-blocking health checks
- ✅ **Timeout Handling** - Configurable timeouts per check
- ✅ **Detailed Reporting** - Response times and error details

## Installation

```bash
pip install -r requirements.txt
```

## Usage

### Standalone

```python
import asyncio
from health_check import HealthCheck

async def main():
    checker = HealthCheck(timeout=5)
    result = await checker.check()
    print(result)

asyncio.run(main())
```

### FastAPI Integration

```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse
from health_check import HealthCheck

app = FastAPI()
health_checker = HealthCheck()

@app.get("/health")
async def health():
    result = await health_checker.check()
    status_code = 200 if result['status'] == 'healthy' else 503
    return JSONResponse(content=result, status_code=status_code)
```

### Flask Integration

```python
from flask import Flask, jsonify
from health_check import HealthCheck

app = Flask(__name__)
health_checker = HealthCheck()

@app.route("/health")
async def health():
    result = await health_checker.check()
    status_code = 200 if result['status'] == 'healthy' else 503
    return jsonify(result), status_code
```

## Configuration

```python
from health_check import HealthCheck

# Custom configuration
checker = HealthCheck(
    timeout=10,  # 10 seconds timeout
)
```

## Testing

```bash
pytest test_health_check.py -v
```

## API Response Codes

| Status | HTTP Code | Description |
|--------|-----------|-------------|
| healthy | 200 | All checks passed |
| degraded | 200 | Some checks degraded but functional |
| unhealthy | 503 | Critical checks failed |

## Check Types

### API Check
- Verifies API service is running
- Measures response time

### Database Check
- Tests database connectivity
- Executes simple query (SELECT 1)

### External Services Check
- Checks external API dependencies
- Includes Solana RPC status
- Reports individual service health

## License

MIT License - Part of WattCoin Bounty Program
