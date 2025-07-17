# API Keys Authentication

Learn how to securely authenticate with the Vector AI API using API keys. This guide covers everything from generating your first key to implementing proper authentication in your applications.

## 📋 Table of Contents

* [🎯 Overview](api-keys-authentication.md#overview)
* [🔑 Getting Your API Key](api-keys-authentication.md#getting-your-api-key)
* [🛡️ Authentication Methods](api-keys-authentication.md#authentication-methods)
* [🔒 Security Best Practices](api-keys-authentication.md#security-best-practices)
* [⚡ Rate Limiting](api-keys-authentication.md#rate-limiting)
* [🔄 Key Management](api-keys-authentication.md#key-management)
* [❌ Common Errors](api-keys-authentication.md#common-errors)
* [💡 Examples](api-keys-authentication.md#examples)

***

## 🎯 Overview

### What are API Keys?

API keys are unique identifiers that authenticate your requests to the Vector AI API. They:

* ✅ **Authenticate** your identity
* ✅ **Authorize** access to specific features
* ✅ **Track** your usage and rate limits
* ✅ **Secure** your API interactions

### Key Features

| Feature                   | Description                            |
| ------------------------- | -------------------------------------- |
| **Unique Identification** | Each key is unique to your application |
| **Rate Limiting**         | Keys have specific request limits      |
| **Usage Tracking**        | Monitor API consumption                |
| **Revocation**            | Instantly disable compromised keys     |
| **Tier-based Access**     | Different features based on plan       |

***

## 🔑 Getting Your API Key

### Method 1: API Request (Recommended)

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/auth/request-key" \
  -H "Content-Type: application/json" \
  -d '{
    "client_name": "My Application",
    "email": "your@email.com",
    "intended_usage": "Token analysis for crypto trading bot"
  }'
```

### Method 2: Dashboard (Coming Soon)

Visit the [Vector AI Dashboard](https://dashboard.vector-ai.pro) to:

* Generate new API keys
* View usage statistics
* Manage existing keys
* Upgrade your plan

### API Key Request Parameters

| Parameter        | Type   | Required | Description                   |
| ---------------- | ------ | -------- | ----------------------------- |
| `client_name`    | string | ✅        | Name of your application      |
| `email`          | string | ✅        | Your email address            |
| `intended_usage` | string | ✅        | Brief description of use case |
| `tier`           | string | ❌        | Requested tier (free/premium) |

### Response Format

```json
{
  "success": true,
  "data": {
    "api_key": "vect_c39cf11bbdadf9cf1376203990ba15189de643f2599cbe3062e5a2a1cecc8957",
    "client_id": "client_abc123",
    "client_name": "My Application",
    "tier": "free",
    "rate_limit": "60/minute",
    "features": ["token_analysis", "basic_research"],
    "expires_at": "2025-02-16T10:30:00Z",
    "created_at": "2025-01-16T10:30:00Z"
  }
}
```

***

## 🛡️ Authentication Methods

### Primary Method: Header Authentication

**Recommended approach** for all API requests:

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: vect_your_api_key_here" \
  -d '{"contract_address": "0x...", "chain": "eth"}'
```

### Alternative: Query Parameter (Not Recommended)

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan?api_key=vect_your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{"contract_address": "0x...", "chain": "eth"}'
```

⚠️ **Warning**: Query parameters are logged in server logs and browser history.

### Headers Reference

| Header             | Value               | Required        |
| ------------------ | ------------------- | --------------- |
| `X-Vector-API-Key` | Your API key        | ✅               |
| `Content-Type`     | `application/json`  | ✅               |
| `User-Agent`       | Your app identifier | ❌ (recommended) |

***

## 🔒 Security Best Practices

### 🔐 Key Storage

**✅ DO:**

* Store keys in environment variables
* Use secure key management systems
* Implement key rotation
* Monitor key usage

**❌ DON'T:**

* Hardcode keys in source code
* Store keys in version control
* Share keys in public channels
* Use keys in client-side code

### Environment Variables

```bash
# .env file
VECTOR_API_KEY=vect_your_api_key_here
VECTOR_API_BASE_URL=https://api.vector-ai.pro

# Load in your application
export VECTOR_API_KEY=vect_your_api_key_here
```

### Key Validation

Always validate your API key before production use:

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/auth/validate" \
  -H "X-Vector-API-Key: vect_your_api_key_here"
```

**Valid Response:**

```json
{
  "success": true,
  "data": {
    "valid": true,
    "client_id": "client_abc123",
    "tier": "free",
    "remaining_requests": 45,
    "reset_time": "2025-01-16T11:00:00Z"
  }
}
```

***

## ⚡ Rate Limiting

### Rate Limit Tiers

| Tier           | Requests/Minute | Concurrent Requests | Features              |
| -------------- | --------------- | ------------------- | --------------------- |
| **Free**       | 60              | 5                   | Basic analysis        |
| **Starter**    | 300             | 10                  | + Research, Priority  |
| **Pro**        | 1000            | 25                  | + X Analysis, Premium |
| **Enterprise** | Custom          | Custom              | + White-label, SLA    |

### Rate Limit Headers

Every API response includes rate limit information:

```bash
curl -I "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "X-Vector-API-Key: vect_your_api_key_here"
```

**Response Headers:**

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1642337400
X-RateLimit-Window: 60
```

### Handling Rate Limits

```python
import time
import requests

def make_request_with_retry(url, headers, data, max_retries=3):
    for attempt in range(max_retries):
        response = requests.post(url, headers=headers, json=data)
        
        if response.status_code == 200:
            return response.json()
        elif response.status_code == 429:
            # Rate limit exceeded
            reset_time = int(response.headers.get('X-RateLimit-Reset', 0))
            wait_time = reset_time - int(time.time())
            
            if wait_time > 0:
                print(f"Rate limit exceeded. Waiting {wait_time} seconds...")
                time.sleep(wait_time)
            continue
        else:
            print(f"Request failed: {response.status_code}")
            break
    
    return None
```

***

## 🔄 Key Management

### Key Lifecycle

1. **Generation** - Create new API key
2. **Validation** - Verify key works
3. **Usage** - Make API requests
4. **Monitoring** - Track usage patterns
5. **Rotation** - Replace with new key
6. **Revocation** - Disable old key

### Key Rotation Strategy

```bash
# 1. Generate new key
NEW_KEY=$(curl -X POST "https://api.vector-ai.pro/api/v1/auth/request-key" \
  -H "Content-Type: application/json" \
  -d '{"client_name": "My App", "email": "me@example.com"}' \
  | jq -r '.data.api_key')

# 2. Test new key
curl -X POST "https://api.vector-ai.pro/api/v1/auth/validate" \
  -H "X-Vector-API-Key: $NEW_KEY"

# 3. Update application
export VECTOR_API_KEY=$NEW_KEY

# 4. Revoke old key (when ready)
curl -X POST "https://api.vector-ai.pro/api/v1/auth/revoke" \
  -H "X-Vector-API-Key: $OLD_KEY"
```

### Multiple Keys

For complex applications, you might need multiple keys:

```bash
# Production key
VECTOR_API_KEY_PROD=vect_production_key_here

# Development key
VECTOR_API_KEY_DEV=vect_development_key_here

# Testing key
VECTOR_API_KEY_TEST=vect_testing_key_here
```

***

## ❌ Common Errors

### 401 Unauthorized

**Problem:** Invalid or missing API key

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

**Solutions:**

* Check API key spelling
* Verify key hasn't expired
* Ensure proper header format

### 429 Too Many Requests

**Problem:** Rate limit exceeded

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

**Solutions:**

* Implement exponential backoff
* Upgrade to higher tier
* Optimize request frequency

### 403 Forbidden

**Problem:** Access denied for specific feature

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

* Upgrade your plan
* Use basic features only
* Contact support

***

## 💡 Examples

### Python Implementation

```python
import os
import requests
from typing import Dict, Any

class VectorAIClient:
    def __init__(self, api_key: str = None):
        self.api_key = api_key or os.getenv('VECTOR_API_KEY')
        self.base_url = "https://api.vector-ai.pro"
        self.headers = {
            'X-Vector-API-Key': self.api_key,
            'Content-Type': 'application/json',
            'User-Agent': 'VectorAI-Python/1.0'
        }
    
    def validate_key(self) -> bool:
        """Validate API key"""
        response = requests.post(
            f"{self.base_url}/api/v1/auth/validate",
            headers=self.headers
        )
        return response.status_code == 200
    
    def analyze_token(self, contract_address: str, chain: str = "eth") -> Dict[str, Any]:
        """Analyze token"""
        response = requests.post(
            f"{self.base_url}/api/v1/token/scan",
            headers=self.headers,
            json={
                "contract_address": contract_address,
                "chain": chain
            }
        )
        return response.json()

# Usage
client = VectorAIClient()
if client.validate_key():
    result = client.analyze_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
    print(f"Score: {result['data']['vector_score']}")
```

### JavaScript Implementation

```javascript
class VectorAIClient {
    constructor(apiKey) {
        this.apiKey = apiKey || process.env.VECTOR_API_KEY;
        this.baseUrl = 'https://api.vector-ai.pro';
        this.headers = {
            'X-Vector-API-Key': this.apiKey,
            'Content-Type': 'application/json',
            'User-Agent': 'VectorAI-JS/1.0'
        };
    }
    
    async validateKey() {
        try {
            const response = await fetch(`${this.baseUrl}/api/v1/auth/validate`, {
                method: 'POST',
                headers: this.headers
            });
            return response.ok;
        } catch (error) {
            return false;
        }
    }
    
    async analyzeToken(contractAddress, chain = 'eth') {
        const response = await fetch(`${this.baseUrl}/api/v1/token/scan`, {
            method: 'POST',
            headers: this.headers,
            body: JSON.stringify({
                contract_address: contractAddress,
                chain: chain
            })
        });
        return response.json();
    }
}

// Usage
const client = new VectorAIClient();
if (await client.validateKey()) {
    const result = await client.analyzeToken('0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234');
    console.log(`Score: ${result.data.vector_score}`);
}
```

***

## 🎯 Next Steps

* 📊 [**Token Analysis Endpoint**](broken-reference) - Start analyzing tokens
* 🔄 **Rate Limiting Guide** - Understand limits
* 🛡️ **Security Best Practices** - Secure your implementation
* 💻 [**Code Examples**](broken-reference) - See more implementations

***

**Need help with authentication?** Contact our support team at [support@vector-ai.pro](mailto:support@vector-ai.pro)
