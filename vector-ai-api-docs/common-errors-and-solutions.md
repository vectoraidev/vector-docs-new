# Common Errors & Solutions

Quick solutions to the most common issues you might encounter when using the Vector AI API.

## 📋 Table of Contents

* [🔐 Authentication Errors](common-errors-and-solutions.md#authentication-errors)
* [⚡ Rate Limiting Issues](common-errors-and-solutions.md#rate-limiting-issues)
* [📝 Request Format Errors](common-errors-and-solutions.md#request-format-errors)
* [🔍 Token Analysis Errors](common-errors-and-solutions.md#token-analysis-errors)
* [🌐 Network & Connection Issues](common-errors-and-solutions.md#network--connection-issues)
* [📊 Response Format Issues](common-errors-and-solutions.md#response-format-issues)
* [🛠️ Implementation Tips](common-errors-and-solutions.md#implementation-tips)

***

## 🔐 Authentication Errors

### 401 Unauthorized: Invalid API Key

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 401,
    "message": "Invalid API key",
    "type": "authentication_error"
  }
}
```

**Common Causes:**

* ❌ Wrong API key format
* ❌ Expired API key
* ❌ Missing `X-Vector-API-Key` header
* ❌ API key not activated

**Solutions:**

```python
# ✅ Correct header format
headers = {
    "X-Vector-API-Key": "vect_your_api_key_here",  # Must start with "vect_"
    "Content-Type": "application/json"
}

# ✅ Validate your API key first
def validate_api_key(api_key):
    response = requests.post(
        "https://api.vector-ai.pro/api/v1/auth/validate",
        headers={"X-Vector-API-Key": api_key}
    )
    return response.status_code == 200
```

### 403 Forbidden: Feature Access Denied

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 403,
    "message": "Premium feature requires paid plan",
    "type": "authorization_error"
  }
}
```

**Solutions:**

* ✅ Upgrade to premium plan
* ✅ Remove premium features from request
* ✅ Check feature availability in your tier

```python
# ✅ Check your tier and available features
def check_tier_features(api_key):
    response = requests.post(
        "https://api.vector-ai.pro/api/v1/auth/validate",
        headers={"X-Vector-API-Key": api_key}
    )
    if response.status_code == 200:
        data = response.json()
        print(f"Tier: {data['data']['tier']}")
        print(f"Features: {data['data']['features']}")
```

***

## ⚡ Rate Limiting Issues

### 429 Too Many Requests

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 429,
    "message": "Rate limit exceeded",
    "type": "rate_limit_error",
    "retry_after": 60
  }
}
```

**Rate Limits by Tier:**

| Tier       | Requests/Minute | Burst Limit |
| ---------- | --------------- | ----------- |
| Free       | 60              | 10          |
| Premium    | 300             | 50          |
| Enterprise | 1000+           | 100+        |

**Solutions:**

```python
import time
import requests
from functools import wraps

# ✅ Implement exponential backoff
def retry_with_backoff(max_retries=3, base_delay=1):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_retries):
                try:
                    response = func(*args, **kwargs)
                    if response.status_code == 429:
                        wait_time = base_delay * (2 ** attempt)
                        print(f"Rate limit hit, waiting {wait_time}s...")
                        time.sleep(wait_time)
                        continue
                    return response
                except Exception as e:
                    if attempt == max_retries - 1:
                        raise e
                    time.sleep(base_delay * (2 ** attempt))
        return wrapper
    return decorator

# ✅ Use rate limiting decorator
@retry_with_backoff(max_retries=3)
def make_api_request(url, headers, data):
    return requests.post(url, headers=headers, json=data)

# ✅ Check rate limit headers
def check_rate_limit(response):
    remaining = response.headers.get('X-RateLimit-Remaining')
    reset_time = response.headers.get('X-RateLimit-Reset')
    
    if remaining and int(remaining) < 5:
        print(f"⚠️ Only {remaining} requests remaining")
        print(f"Reset time: {reset_time}")
```

### Rate Limit Best Practices

```python
# ✅ Implement request spacing
import asyncio
import aiohttp

class RateLimitedAPI:
    def __init__(self, requests_per_minute=60):
        self.min_interval = 60.0 / requests_per_minute
        self.last_request = 0
    
    async def make_request(self, session, url, data):
        # Wait if needed
        elapsed = time.time() - self.last_request
        if elapsed < self.min_interval:
            await asyncio.sleep(self.min_interval - elapsed)
        
        self.last_request = time.time()
        
        async with session.post(url, json=data) as response:
            return await response.json()
```

***

## 📝 Request Format Errors

### 400 Bad Request: Invalid Contract Address

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 400,
    "message": "Invalid contract address format",
    "type": "validation_error",
    "details": {
      "field": "contract_address",
      "value": "invalid_address"
    }
  }
}
```

**Solutions:**

```python
import re

# ✅ Validate contract address format
def is_valid_contract_address(address):
    # Must be 42 characters, start with 0x, followed by 40 hex chars
    pattern = r'^0x[a-fA-F0-9]{40}$'
    return bool(re.match(pattern, address))

# ✅ Validate before sending
def analyze_token_safe(contract_address, chain="eth"):
    if not is_valid_contract_address(contract_address):
        return {"error": "Invalid contract address format"}
    
    # Proceed with API call
    response = requests.post(
        "https://api.vector-ai.pro/api/v1/token/scan",
        headers=headers,
        json={
            "contract_address": contract_address,
            "chain": chain
        }
    )
    return response.json()
```

### 400 Bad Request: Invalid Chain

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 400,
    "message": "Unsupported chain: invalid_chain",
    "type": "validation_error"
  }
}
```

**Solutions:**

```python
# ✅ Supported chains
SUPPORTED_CHAINS = ["eth", "bsc", "polygon", "arbitrum", "optimism"]

def validate_chain(chain):
    if chain not in SUPPORTED_CHAINS:
        raise ValueError(f"Unsupported chain: {chain}. Use one of: {SUPPORTED_CHAINS}")
    return chain

# ✅ Usage
try:
    chain = validate_chain("ethereum")  # This will fail
except ValueError as e:
    print(f"Error: {e}")
    chain = "eth"  # Use correct format
```

***

## 🔍 Token Analysis Errors

### 404 Not Found: Token Not Found

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 404,
    "message": "Token not found or not supported",
    "type": "not_found_error"
  }
}
```

**Common Causes:**

* ❌ Token doesn't exist on the specified chain
* ❌ Wrong chain specified
* ❌ Token not yet indexed

**Solutions:**

```python
# ✅ Check multiple chains
def find_token_chain(contract_address):
    chains = ["eth", "bsc", "polygon"]
    
    for chain in chains:
        response = requests.post(
            "https://api.vector-ai.pro/api/v1/token/scan",
            headers=headers,
            json={
                "contract_address": contract_address,
                "chain": chain
            }
        )
        
        if response.status_code == 200:
            return chain
    
    return None  # Token not found on any chain

# ✅ Graceful handling
def analyze_with_fallback(contract_address, preferred_chain="eth"):
    # Try preferred chain first
    response = requests.post(
        "https://api.vector-ai.pro/api/v1/token/scan",
        headers=headers,
        json={
            "contract_address": contract_address,
            "chain": preferred_chain
        }
    )
    
    if response.status_code == 200:
        return response.json()
    
    if response.status_code == 404:
        # Try to find on other chains
        found_chain = find_token_chain(contract_address)
        if found_chain:
            print(f"Token found on {found_chain} instead of {preferred_chain}")
            # Retry with correct chain
            return analyze_with_fallback(contract_address, found_chain)
    
    return {"error": f"Token not found: {contract_address}"}
```

### 422 Unprocessable Entity: Analysis Failed

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": 422,
    "message": "Analysis failed: Unable to fetch token data",
    "type": "processing_error"
  }
}
```

**Common Causes:**

* ❌ Token contract has unusual implementation
* ❌ Network congestion
* ❌ External API unavailable

**Solutions:**

```python
# ✅ Retry with minimal features
def analyze_with_minimal_features(contract_address, chain="eth"):
    # Try full analysis first
    response = requests.post(
        "https://api.vector-ai.pro/api/v1/token/scan",
        headers=headers,
        json={
            "contract_address": contract_address,
            "chain": chain
        }
    )
    
    if response.status_code == 200:
        return response.json()
    
    if response.status_code == 422:
        # Try with only basic security features
        response = requests.post(
            "https://api.vector-ai.pro/api/v1/token/scan",
            headers=headers,
            json={
                "contract_address": contract_address,
                "chain": chain,
                "features": ["security"]  # Minimal analysis
            }
        )
        
        if response.status_code == 200:
            return response.json()
    
    return {"error": "Analysis failed even with minimal features"}
```

***

## 🌐 Network & Connection Issues

### Connection Timeout

**Error:**

```python
requests.exceptions.ReadTimeout: HTTPSConnectionPool(host='api.vector-ai.pro', port=443): Read timed out.
```

**Solutions:**

```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

# ✅ Configure robust session
def create_robust_session():
    session = requests.Session()
    
    # Retry strategy
    retry_strategy = Retry(
        total=3,
        backoff_factor=1,
        status_forcelist=[429, 500, 502, 503, 504],
        method_whitelist=["HEAD", "GET", "OPTIONS", "POST"]
    )
    
    adapter = HTTPAdapter(max_retries=retry_strategy)
    session.mount("http://", adapter)
    session.mount("https://", adapter)
    
    return session

# ✅ Use appropriate timeouts
def make_request_with_timeout(url, data):
    session = create_robust_session()
    
    try:
        response = session.post(
            url,
            json=data,
            timeout=(10, 30)  # (connect timeout, read timeout)
        )
        return response
    except requests.exceptions.Timeout:
        print("Request timed out, retrying...")
        # Implement retry logic
        return None
```

### DNS Resolution Issues

**Error:**

```python
requests.exceptions.ConnectionError: HTTPSConnectionPool(host='api.vector-ai.pro', port=443): Max retries exceeded with url: /api/v1/token/scan
```

**Solutions:**

```python
import socket

# ✅ Test DNS resolution
def test_dns_resolution():
    try:
        socket.gethostbyname("api.vector-ai.pro")
        print("✅ DNS resolution working")
        return True
    except socket.gaierror:
        print("❌ DNS resolution failed")
        return False

# ✅ Use IP address as fallback
def get_api_url():
    if test_dns_resolution():
        return "https://api.vector-ai.pro"
    else:
        # Use IP address as fallback (example)
        return "https://1.2.3.4"  # Replace with actual IP
```

***

## 📊 Response Format Issues

### JSON Decode Error

**Error:**

```python
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```

**Solutions:**

```python
import json

# ✅ Safe JSON parsing
def safe_json_parse(response):
    try:
        return response.json()
    except json.JSONDecodeError:
        print(f"Invalid JSON response: {response.text}")
        return {
            "success": False,
            "error": "Invalid JSON response",
            "raw_response": response.text
        }

# ✅ Validate response before parsing
def make_safe_request(url, data):
    response = requests.post(url, json=data, headers=headers)
    
    # Check if response is JSON
    content_type = response.headers.get('Content-Type', '')
    if 'application/json' not in content_type:
        return {
            "success": False,
            "error": f"Unexpected content type: {content_type}",
            "raw_response": response.text
        }
    
    return safe_json_parse(response)
```

### Missing Fields in Response

**Error:**

```python
KeyError: 'vector_score'
```

**Solutions:**

```python
# ✅ Safe field access
def safe_get(data, *keys, default=None):
    """Safely get nested dictionary values"""
    for key in keys:
        if isinstance(data, dict) and key in data:
            data = data[key]
        else:
            return default
    return data

# ✅ Usage
response_data = api_response.json()
vector_score = safe_get(response_data, 'data', 'vector_score', default=0)
token_name = safe_get(response_data, 'data', 'token_info', 'name', default="Unknown")

# ✅ Validate response structure
def validate_response_structure(data):
    required_fields = ['success', 'data']
    
    for field in required_fields:
        if field not in data:
            return False, f"Missing required field: {field}"
    
    if data['success'] and 'data' in data:
        data_fields = ['vector_score', 'grade', 'token_info']
        for field in data_fields:
            if field not in data['data']:
                return False, f"Missing data field: {field}"
    
    return True, "Valid response structure"
```

***

## 🛠️ Implementation Tips

### Production-Ready Error Handling

```python
import logging
from enum import Enum

class APIErrorType(Enum):
    AUTHENTICATION = "authentication_error"
    RATE_LIMIT = "rate_limit_error"
    VALIDATION = "validation_error"
    NOT_FOUND = "not_found_error"
    PROCESSING = "processing_error"
    NETWORK = "network_error"
    UNKNOWN = "unknown_error"

class VectorAPIError(Exception):
    def __init__(self, message, error_type=APIErrorType.UNKNOWN, retry_after=None):
        self.message = message
        self.error_type = error_type
        self.retry_after = retry_after
        super().__init__(self.message)

class RobustVectorClient:
    def __init__(self, api_key):
        self.api_key = api_key
        self.logger = logging.getLogger(__name__)
        self.session = create_robust_session()
    
    def analyze_token(self, contract_address, chain="eth", max_retries=3):
        """Analyze token with comprehensive error handling"""
        for attempt in range(max_retries):
            try:
                response = self.session.post(
                    "https://api.vector-ai.pro/api/v1/token/scan",
                    headers={"X-Vector-API-Key": self.api_key},
                    json={"contract_address": contract_address, "chain": chain},
                    timeout=(10, 30)
                )
                
                # Handle different status codes
                if response.status_code == 200:
                    return response.json()
                
                elif response.status_code == 401:
                    raise VectorAPIError(
                        "Invalid API key", 
                        APIErrorType.AUTHENTICATION
                    )
                
                elif response.status_code == 429:
                    retry_after = int(response.headers.get('Retry-After', 60))
                    if attempt < max_retries - 1:
                        self.logger.warning(f"Rate limit hit, waiting {retry_after}s...")
                        time.sleep(retry_after)
                        continue
                    raise VectorAPIError(
                        "Rate limit exceeded", 
                        APIErrorType.RATE_LIMIT,
                        retry_after
                    )
                
                elif response.status_code == 404:
                    raise VectorAPIError(
                        f"Token not found: {contract_address}",
                        APIErrorType.NOT_FOUND
                    )
                
                else:
                    error_data = safe_json_parse(response)
                    raise VectorAPIError(
                        error_data.get('error', {}).get('message', 'Unknown error'),
                        APIErrorType.UNKNOWN
                    )
                    
            except requests.exceptions.RequestException as e:
                if attempt < max_retries - 1:
                    self.logger.warning(f"Network error, retrying: {e}")
                    time.sleep(2 ** attempt)
                    continue
                raise VectorAPIError(
                    f"Network error: {e}",
                    APIErrorType.NETWORK
                )
        
        raise VectorAPIError("Max retries exceeded", APIErrorType.UNKNOWN)

# Usage
client = RobustVectorClient(API_KEY)

try:
    result = client.analyze_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
    print(f"Analysis successful: {result}")
except VectorAPIError as e:
    if e.error_type == APIErrorType.RATE_LIMIT:
        print(f"Rate limit error: {e.message}")
        if e.retry_after:
            print(f"Retry after: {e.retry_after} seconds")
    elif e.error_type == APIErrorType.AUTHENTICATION:
        print(f"Authentication error: {e.message}")
        # Handle API key renewal
    else:
        print(f"API error: {e.message}")
```

### Logging Best Practices

```python
import logging
import sys

# ✅ Configure logging
def setup_logging():
    logging.basicConfig(
        level=logging.INFO,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        handlers=[
            logging.FileHandler('vector_api.log'),
            logging.StreamHandler(sys.stdout)
        ]
    )
    
    # Create logger
    logger = logging.getLogger('vector_api')
    return logger

# ✅ Use structured logging
logger = setup_logging()

def log_api_call(contract_address, chain, response_time, success):
    logger.info(
        "API call completed",
        extra={
            'contract_address': contract_address,
            'chain': chain,
            'response_time': response_time,
            'success': success
        }
    )
```

***

## 📞 Getting Help

### When to Contact Support

Contact support at [support@vector-ai.pro](mailto:support@vector-ai.pro) if you encounter:

* ✅ Persistent 500 errors
* ✅ Unexpected API behavior
* ✅ Performance issues
* ✅ Feature requests

### What to Include

When contacting support, please include:

1. **API Key** (first 8 characters only)
2. **Error message** and status code
3. **Request payload** (without sensitive data)
4. **Timestamp** of the issue
5. **Steps to reproduce**

### Self-Help Resources

* 📚 [Full Documentation](broken-reference)
* 💻 [Code Examples](broken-reference)
* 🔐 [Authentication Guide](broken-reference)
* 🎯 [API Reference](broken-reference)

***

**Still having issues?** Check our FAQ or join our [Discord community](https://discord.gg/vectorai) for help! 🚀
