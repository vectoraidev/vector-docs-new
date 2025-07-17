# Token Analysis Endpoint

The **Token Analysis endpoint** is the core of the Vector AI API, providing comprehensive cryptocurrency token analysis including security, market data, and AI-powered insights.

## 📋 Table of Contents

* [🎯 Overview](token-analysis-endpoint.md#overview)
* [📝 Request Format](token-analysis-endpoint.md#request-format)
* [📊 Response Structure](token-analysis-endpoint.md#response-structure)
* [🔍 Analysis Types](token-analysis-endpoint.md#analysis-types)
* [💡 Examples](token-analysis-endpoint.md#examples)
* [❌ Error Handling](token-analysis-endpoint.md#error-handling)
* [⚡ Performance Tips](token-analysis-endpoint.md#performance-tips)
* [🔗 Related Endpoints](token-analysis-endpoint.md#related-endpoints)

***

## 🎯 Overview

### Endpoint URL

```
POST https://api.vector-ai.pro/api/v1/token/scan
```

### What it does

* 🔍 **Security Analysis** - Honeypot detection, rug pull risk, contract security
* 📈 **Market Analysis** - Price, volume, liquidity, market cap
* 👥 **Holder Analysis** - Distribution, whale activity, holder patterns
* 🤖 **AI Insights** - Smart contract analysis, risk assessment
* 🏆 **Vector Score** - Proprietary scoring algorithm (0-100)

### Use Cases

* **Due Diligence** - Before investing in tokens
* **Risk Assessment** - Evaluate token security
* **Portfolio Management** - Monitor existing holdings
* **Trading Bots** - Automated token screening
* **DeFi Integration** - Token whitelisting

***

## 📝 Request Format

### Headers

```http
Content-Type: application/json
X-Vector-API-Key: your_api_key_here
```

### Request Body

#### Basic Request

```json
{
  "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
  "chain": "eth"
}
```

#### Advanced Request

```json
{
  "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
  "chain": "eth",
  "features": ["security", "market", "holder_analysis"],
  "include_metadata": true,
  "cache_ttl": 300
}
```

### Parameters

| Parameter          | Type    | Required | Description                         | Example                                       |
| ------------------ | ------- | -------- | ----------------------------------- | --------------------------------------------- |
| `contract_address` | string  | ✅        | Token contract address              | `"0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234"` |
| `chain`            | string  | ❌        | Blockchain network (default: "eth") | `"eth"`, `"bsc"`, `"polygon"`                 |
| `features`         | array   | ❌        | Specific analysis features to run   | `["security", "market"]`                      |
| `include_metadata` | boolean | ❌        | Include analysis metadata           | `true`                                        |
| `cache_ttl`        | integer | ❌        | Cache duration in seconds           | `300`                                         |

### Supported Chains

| Chain                   | Identifier | Description             |
| ----------------------- | ---------- | ----------------------- |
| **Ethereum**            | `eth`      | Ethereum mainnet        |
| **Binance Smart Chain** | `bsc`      | BSC mainnet             |
| **Polygon**             | `polygon`  | Polygon (Matic) mainnet |
| **Arbitrum**            | `arbitrum` | Arbitrum One            |
| **Optimism**            | `optimism` | Optimism mainnet        |

### Available Features

| Feature           | Description                           | Premium |
| ----------------- | ------------------------------------- | ------- |
| `security`        | Honeypot, rug pull, contract analysis | ❌       |
| `market`          | Price, volume, liquidity data         | ❌       |
| `holder_analysis` | Token distribution, whale activity    | ❌       |
| `wash_trading`    | Wash trading detection                | ✅       |
| `sniper_analysis` | Bot and sniper detection              | ✅       |
| `link_analysis`   | Social links and website analysis     | ✅       |

***

## 📊 Response Structure

### Success Response

```json
{
  "success": true,
  "data": {
    "token_info": {
      "name": "USD Coin",
      "symbol": "USDC",
      "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
      "chain": "ethereum",
      "decimals": 6,
      "total_supply": "32824891556.123456"
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
        "malicious_functions": [],
        "ownership_analysis": {
          "owner_address": "0x807a96288A1A408dBC13DE2b1d087d10356395d2",
          "can_mint": false,
          "can_pause": false,
          "can_blacklist": false
        }
      },
      "market": {
        "price": "$1.00",
        "price_change_24h": "+0.01%",
        "market_cap": "$32.8B",
        "volume_24h": "$4.2B",
        "liquidity": {
          "total_liquidity": "$45.2M",
          "status": "🟢 EXCELLENT"
        },
        "trading_pairs": [
          {
            "dex": "Uniswap V3",
            "pair": "USDC/WETH",
            "liquidity": "$12.5M",
            "volume_24h": "$2.1B"
          }
        ]
      },
      "holder_analysis": {
        "total_holders": 2500000,
        "top_10_percentage": 35.2,
        "top_100_percentage": 65.8,
        "whale_activity": "🟡 MODERATE",
        "distribution": "🟢 HEALTHY",
        "recent_activity": {
          "new_holders_24h": 1250,
          "large_transactions": 15
        }
      }
    },
    "metadata": {
      "analysis_time": "2.3s",
      "timestamp": "2025-01-16T10:30:00.000Z",
      "api_version": "1.0.0",
      "cache_status": "FRESH"
    }
  }
}
```

### Response Fields Reference

#### Token Info

| Field              | Type    | Description        |
| ------------------ | ------- | ------------------ |
| `name`             | string  | Token name         |
| `symbol`           | string  | Token symbol       |
| `contract_address` | string  | Contract address   |
| `chain`            | string  | Blockchain network |
| `decimals`         | integer | Token decimals     |
| `total_supply`     | string  | Total token supply |

#### Vector Score & Grade

| Field            | Type    | Description            | Range             |
| ---------------- | ------- | ---------------------- | ----------------- |
| `vector_score`   | integer | Overall score          | 0-100             |
| `grade`          | string  | Letter grade           | F, D, C, B, A, A+ |
| `recommendation` | string  | Trading recommendation | Various           |

#### Scoring Breakdown

| Score Range | Grade | Recommendation    | Description               |
| ----------- | ----- | ----------------- | ------------------------- |
| 90-100      | A+    | 🟢 SAFE TO TRADE  | Excellent, very low risk  |
| 80-89       | A     | 🟢 SAFE TO TRADE  | Good, low risk            |
| 70-79       | B     | 🟡 MODERATE RISK  | Acceptable, moderate risk |
| 60-69       | C     | 🟡 MODERATE RISK  | Caution advised           |
| 50-59       | D     | 🔴 HIGH RISK      | High risk, avoid          |
| 0-49        | F     | 🔴 VERY HIGH RISK | Extremely risky           |

***

## 🔍 Analysis Types

### 🛡️ Security Analysis

**What it checks:**

* Honeypot detection
* Rug pull risk assessment
* Contract verification status
* Malicious function detection
* Ownership analysis

**Example Response:**

```json
{
  "security": {
    "honeypot_risk": "✅ SAFE",
    "rug_pull_risk": "✅ VERY_LOW",
    "contract_verified": true,
    "proxy_contract": false,
    "malicious_functions": [],
    "ownership_analysis": {
      "owner_address": "0x807a96288A1A408dBC13DE2b1d087d10356395d2",
      "can_mint": false,
      "can_pause": false,
      "can_blacklist": false
    }
  }
}
```

### 📈 Market Analysis

**What it includes:**

* Current price and price changes
* Market capitalization
* Trading volume
* Liquidity analysis
* Trading pairs

**Example Response:**

```json
{
  "market": {
    "price": "$1.00",
    "price_change_24h": "+0.01%",
    "market_cap": "$32.8B",
    "volume_24h": "$4.2B",
    "liquidity": {
      "total_liquidity": "$45.2M",
      "status": "🟢 EXCELLENT"
    }
  }
}
```

### 👥 Holder Analysis

**What it analyzes:**

* Token distribution
* Whale activity
* Holder concentration
* Recent activity patterns

**Example Response:**

```json
{
  "holder_analysis": {
    "total_holders": 2500000,
    "top_10_percentage": 35.2,
    "top_100_percentage": 65.8,
    "whale_activity": "🟡 MODERATE",
    "distribution": "🟢 HEALTHY"
  }
}
```

***

## 💡 Examples

### Basic Token Analysis

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{
    "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    "chain": "eth"
  }'
```

### Security-Only Analysis

```bash
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "Content-Type: application/json" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{
    "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    "chain": "eth",
    "features": ["security"]
  }'
```

### Multi-Chain Analysis

```bash
# Ethereum
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{"contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234", "chain": "eth"}'

# BSC
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{"contract_address": "0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d", "chain": "bsc"}'

# Polygon
curl -X POST "https://api.vector-ai.pro/api/v1/token/scan" \
  -H "X-Vector-API-Key: your_api_key_here" \
  -d '{"contract_address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174", "chain": "polygon"}'
```

### Python Implementation

```python
import requests
from typing import Dict, Any, Optional, List

class VectorTokenAnalyzer:
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.base_url = "https://api.vector-ai.pro"
        self.headers = {
            'X-Vector-API-Key': api_key,
            'Content-Type': 'application/json'
        }
    
    def analyze_token(
        self,
        contract_address: str,
        chain: str = "eth",
        features: Optional[List[str]] = None,
        include_metadata: bool = False
    ) -> Dict[str, Any]:
        """
        Analyze a token using the Vector AI API
        
        Args:
            contract_address: Token contract address
            chain: Blockchain network (eth, bsc, polygon)
            features: Specific features to analyze
            include_metadata: Include analysis metadata
            
        Returns:
            Analysis results dictionary
        """
        payload = {
            "contract_address": contract_address,
            "chain": chain
        }
        
        if features:
            payload["features"] = features
        if include_metadata:
            payload["include_metadata"] = True
            
        response = requests.post(
            f"{self.base_url}/api/v1/token/scan",
            headers=self.headers,
            json=payload
        )
        
        if response.status_code == 200:
            return response.json()
        else:
            raise Exception(f"API Error: {response.status_code} - {response.text}")
    
    def get_token_score(self, contract_address: str, chain: str = "eth") -> int:
        """Get just the Vector score for a token"""
        result = self.analyze_token(contract_address, chain)
        return result['data']['vector_score']
    
    def is_safe_token(self, contract_address: str, chain: str = "eth", min_score: int = 70) -> bool:
        """Check if a token is safe based on Vector score"""
        score = self.get_token_score(contract_address, chain)
        return score >= min_score

# Usage Example
analyzer = VectorTokenAnalyzer("your_api_key_here")

# Basic analysis
result = analyzer.analyze_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
print(f"Token: {result['data']['token_info']['name']}")
print(f"Score: {result['data']['vector_score']}")
print(f"Grade: {result['data']['grade']}")

# Security-only analysis
security_result = analyzer.analyze_token(
    "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    features=["security"]
)

# Quick safety check
is_safe = analyzer.is_safe_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
print(f"Is safe: {is_safe}")
```

### JavaScript Implementation

```javascript
class VectorTokenAnalyzer {
    constructor(apiKey) {
        this.apiKey = apiKey;
        this.baseUrl = 'https://api.vector-ai.pro';
        this.headers = {
            'X-Vector-API-Key': apiKey,
            'Content-Type': 'application/json'
        };
    }
    
    async analyzeToken(contractAddress, options = {}) {
        const {
            chain = 'eth',
            features = null,
            includeMetadata = false
        } = options;
        
        const payload = {
            contract_address: contractAddress,
            chain: chain
        };
        
        if (features) payload.features = features;
        if (includeMetadata) payload.include_metadata = true;
        
        const response = await fetch(`${this.baseUrl}/api/v1/token/scan`, {
            method: 'POST',
            headers: this.headers,
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`API Error: ${response.status}`);
        }
        
        return response.json();
    }
    
    async getTokenScore(contractAddress, chain = 'eth') {
        const result = await this.analyzeToken(contractAddress, { chain });
        return result.data.vector_score;
    }
    
    async isSafeToken(contractAddress, chain = 'eth', minScore = 70) {
        const score = await this.getTokenScore(contractAddress, chain);
        return score >= minScore;
    }
}

// Usage Example
const analyzer = new VectorTokenAnalyzer('your_api_key_here');

// Basic analysis
const result = await analyzer.analyzeToken('0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234');
console.log(`Token: ${result.data.token_info.name}`);
console.log(`Score: ${result.data.vector_score}`);
console.log(`Grade: ${result.data.grade}`);

// Security-only analysis
const securityResult = await analyzer.analyzeToken(
    '0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234',
    { features: ['security'] }
);

// Quick safety check
const isSafe = await analyzer.isSafeToken('0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234');
console.log(`Is safe: ${isSafe}`);
```

***

## ❌ Error Handling

### Common Errors

#### 400 Bad Request

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

#### 404 Not Found

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

#### 429 Rate Limit

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

### Error Handling Best Practices

```python
def safe_analyze_token(analyzer, contract_address, chain="eth", max_retries=3):
    for attempt in range(max_retries):
        try:
            return analyzer.analyze_token(contract_address, chain)
        except Exception as e:
            if "429" in str(e):
                # Rate limit - wait and retry
                time.sleep(60)
                continue
            elif "404" in str(e):
                # Token not found - don't retry
                return None
            else:
                # Other error - log and retry
                print(f"Attempt {attempt + 1} failed: {e}")
                time.sleep(2 ** attempt)  # Exponential backoff
    
    return None
```

***

## ⚡ Performance Tips

### 1. Use Specific Features

Only request features you need:

```json
{
  "contract_address": "0x...",
  "features": ["security"]  // Faster than full analysis
}
```

### 2. Implement Caching

Cache results for frequently analyzed tokens:

```python
import time
from functools import lru_cache

@lru_cache(maxsize=100)
def cached_analyze_token(contract_address, chain="eth"):
    return analyzer.analyze_token(contract_address, chain)
```

### 3. Batch Processing

For multiple tokens, implement proper rate limiting:

```python
import asyncio
import aiohttp

async def analyze_multiple_tokens(contract_addresses, max_concurrent=5):
    semaphore = asyncio.Semaphore(max_concurrent)
    
    async def analyze_one(address):
        async with semaphore:
            # Analyze token with rate limiting
            return await analyzer.analyze_token(address)
    
    tasks = [analyze_one(addr) for addr in contract_addresses]
    return await asyncio.gather(*tasks)
```

### 4. Error Recovery

Implement robust error handling:

```python
def analyze_with_fallback(contract_address, chain="eth"):
    try:
        return analyzer.analyze_token(contract_address, chain)
    except Exception as e:
        # Log error and return safe defaults
        logger.error(f"Analysis failed for {contract_address}: {e}")
        return {
            "success": False,
            "vector_score": 0,
            "grade": "F",
            "recommendation": "🔴 ANALYSIS FAILED"
        }
```

***

## 🔗 Related Endpoints

* **🧠 AI Research** - Get detailed research reports
* **🔍 X Analysis** - Social media sentiment analysis
* **🤖 VGPT Chat** - Ask questions about tokens
* **💊 Health Check** - API status monitoring

***

## 🎯 Next Steps

1. **Test the endpoint** with your API key
2. **Implement error handling** for production use
3. **Add caching** for better performance
4. **Explore advanced features** like wash trading detection
5. **Combine with other endpoints** for comprehensive analysis

***

**Ready to start analyzing tokens?** Try the endpoint now with your API key! 🚀
