# Quick Start Guide

Get up and running with the Vector AI API in minutes! This guide will walk you through everything you need to know to make your first successful API call.

## 📋 Table of Contents

* [🎯 Prerequisites](quick-start-guide.md#prerequisites)
* [🔑 Step 1: Get Your API Key](quick-start-guide.md#step-1-get-your-api-key)
* [🧪 Step 2: Test Your Connection](quick-start-guide.md#step-2-test-your-connection)
* [🔍 Step 3: Analyze Your First Token](quick-start-guide.md#step-3-analyze-your-first-token)
* [📊 Step 4: Understand the Response](quick-start-guide.md#step-4-understand-the-response)
* [🚀 Step 5: Next Steps](quick-start-guide.md#step-5-next-steps)

***

## 🎯 Prerequisites

Before you begin, make sure you have:

* ✅ **Basic API knowledge** - Understanding of REST APIs and HTTP requests
* ✅ **Command line access** - Terminal or command prompt
* ✅ **cURL or API client** - For making HTTP requests
* ✅ **Valid email address** - For API key generation

## 🔑 Step 1: Get Your API Key

### Request an API Key

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/auth/request-key" \
  -H "Content-Type: application/json" \
  -d '{
    "client_name": "My Application",
    "email": "your@email.com",
    "intended_usage": "Token analysis for my crypto project"
  }'
```

### Example Response

```json
{
  "success": true,
  "data": {
    "api_key": "vect_c39cf11bbdadf9cf1376203990ba15189de643f2599cbe3062e5a2a1cecc8957",
    "client_id": "client_123456",
    "rate_limit": "60/minute",
    "tier": "free",
    "expires_at": "2025-02-16T10:30:00Z"
  }
}
```

### 🔒 **Important:** Save Your API Key!

Your API key is **only shown once**. Save it securely:

```bash
# Save to environment variable (recommended)
export VECTOR_API_KEY="vect_c39cf11bbdadf9cf1376203990ba15189de643f2599cbe3062e5a2a1cecc8957"

# Or save to a file
echo "vect_c39cf11bbdadf9cf1376203990ba15189de643f2599cbe3062e5a2a1cecc8957" > api_key.txt
```

***

## 🧪 Step 2: Test Your Connection

### Health Check

First, verify the API is accessible:

```bash
curl -X GET "https://api.vector-ai.pro/health"
```

### Expected Response

```json
{
  "status": "healthy",
  "timestamp": "2025-01-16T10:30:00.000Z",
  "version": "1.0.0",
  "components": {
    "database": "connected",
    "redis": "available",
    "external_apis": "operational"
  }
}
```

### Test Authentication

Verify your API key works:

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/auth/validate" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: your_api_key_here"
```

***

## 🔍 Step 3: Analyze Your First Token

### Basic Token Analysis

Let's analyze USDC (a stable, well-known token):

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{
    "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    "chain": "eth"
  }'
```

### Advanced Analysis with Features

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{
    "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    "chain": "eth",
    "features": ["security", "market", "holder_analysis"]
  }'
```

***

## 📊 Step 4: Understand the Response

### Full Response Structure

```json
{
  "success": true,
  "data": {
    "token_info": {
      "name": "USD Coin",
      "symbol": "USDC",
      "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
      "chain": "ethereum",
      "decimals": 6
    },
    "vector_score": 95,
    "grade": "A+",
    "recommendation": "🟢 SAFE TO TRADE",
    "analysis": {
      "security": {
        "honeypot_risk": "✅ SAFE",
        "rug_pull_risk": "✅ VERY_LOW",
        "contract_verified": true,
        "proxy_contract": false,
        "malicious_functions": []
      },
      "market": {
        "price": "$1.00",
        "market_cap": "$32.8B",
        "volume_24h": "$4.2B",
        "liquidity": "🟢 EXCELLENT",
        "price_change_24h": "+0.01%"
      },
      "holder_analysis": {
        "total_holders": 2500000,
        "top_10_percentage": 35.2,
        "whale_activity": "🟡 MODERATE",
        "distribution": "🟢 HEALTHY"
      }
    },
    "metadata": {
      "analysis_time": "2.3s",
      "timestamp": "2025-01-16T10:30:00.000Z",
      "api_version": "1.0.0"
    }
  }
}
```

### Key Response Fields

| Field                      | Description               | Example                      |
| -------------------------- | ------------------------- | ---------------------------- |
| `vector_score`             | Overall score (0-100)     | `95`                         |
| `grade`                    | Letter grade (F to A+)    | `"A+"`                       |
| `recommendation`           | Trading recommendation    | `"🟢 SAFE TO TRADE"`         |
| `analysis.security`        | Security analysis results | Risk assessments             |
| `analysis.market`          | Market data and metrics   | Price, volume, liquidity     |
| `analysis.holder_analysis` | Token holder distribution | Whale activity, distribution |

***

## 🚀 Step 5: Next Steps

### 🔥 Popular Use Cases

1.  **🔍 Token Screening**

    ```bash
    # Quick security check
    curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
      -H "X-Vector-API-Key: $VECTOR_API_KEY" \
      -d '{"contract_address": "0x...", "features": ["security"]}'
    ```
2.  **🧠 Deep Research**

    ```bash
    # Get AI-powered research report
    curl -X POST "https://api.vector-ai.pro/api/v1/token/research" \
      -H "X-Vector-API-Key: $VECTOR_API_KEY" \
      -d '{"contract_address": "0x...", "chain": "eth"}'
    ```
3.  **🔍 Social Analysis**

    ```bash
    # Analyze Twitter/X sentiment
    curl -X POST "https://api.vector-ai.pro/api/v1/token/x-analysis" \
      -H "X-Vector-API-Key: $VECTOR_API_KEY" \
      -d '{"contract_address": "0x...", "chain": "eth"}'
    ```

### 📚 Continue Learning

* **🔐** [**Authentication Guide**](broken-reference) - Advanced auth concepts
* **🎯** [**Endpoint Documentation**](broken-reference) - Detailed API reference
* **💻** [**Code Examples**](broken-reference) - Implementation in different languages
* **🛠️** [**Troubleshooting**](broken-reference) - Common issues and solutions

### 🎯 **Integration Examples**

#### Python Integration

```python
import requests

api_key = "your_api_key_here"
headers = {"X-Vector-API-Key": api_key}

response = requests.post(
    "https://api.vector-ai.pro/api/v1/token/scan",
    headers=headers,
    json={
        "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
        "chain": "eth"
    }
)

data = response.json()
print(f"Token: {data['data']['token_info']['name']}")
print(f"Score: {data['data']['vector_score']}")
```

#### JavaScript Integration

```javascript
const response = await fetch('https://api.vector-ai.pro/api/v1/token/scan', {
  method: 'POST',
  headers: {
    'X-Vector-API-Key': 'your_api_key_here',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    contract_address: '0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234',
    chain: 'eth'
  })
});

const data = await response.json();
console.log(`Token: ${data.data.token_info.name}`);
console.log(`Score: ${data.data.vector_score}`);
```

***

## 🎉 **Congratulations!**

You've successfully made your first Vector AI API call! You're now ready to:

1. **Build token analysis applications**
2. **Integrate security screening**
3. **Create trading signals**
4. **Develop crypto research tools**

### 🆘 Need Help?

* 📚 [Full Documentation](broken-reference)
* 💬 [Discord Community](https://discord.gg/vectorai)
* 📧 [Support Email](mailto:support@vector-ai.pro)
* 🐛 [Report Issues](https://github.com/vectorai/api-issues)

***

**Happy Building with Vector AI!** 🚀
