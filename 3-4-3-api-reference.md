# ⚠️ API Reference

## Overview

The Vector AI API provides programmatic access to all VGPT capabilities, enabling developers to integrate blockchain intelligence into their applications. This comprehensive reference covers authentication, endpoints, parameters, and response formats.

## 🔑 Authentication

### API Key Authentication
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     https://api.vector-ai.pro/v1/analyze/token
```

### Token-Based Authentication (Premium)
```bash
curl -H "Authorization: Token YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     https://api.vector-ai.pro/v1/analyze/token
```

## 📊 Core Analysis Endpoints

### Token Analysis
**Endpoint**: `POST /v1/analyze/token`

**Request Body**:
```json
{
  "address": "0x1234567890abcdef1234567890abcdef12345678",
  "chain": "ethereum",
  "analysis_depth": "comprehensive",
  "include_social": true,
  "include_technical": true
}
```

**Response**:
```json
{
  "analysis_id": "analysis_123456",
  "token": {
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "name": "Example Token",
    "symbol": "EXAMPLE",
    "decimals": 18
  },
  "risk_score": 3.2,
  "social_score": 8.1,
  "security_analysis": {
    "contract_verified": true,
    "honeypot_risk": "low",
    "ownership_renounced": true,
    "liquidity_locked": true
  },
  "market_data": {
    "price_usd": 0.00123,
    "market_cap": 1234567,
    "volume_24h": 98765,
    "liquidity_usd": 456789
  },
  "social_validation": {
    "twitter_verified": true,
    "telegram_active": true,
    "website_quality": "high",
    "team_doxxed": false
  }
}
```

### Wallet Analysis
**Endpoint**: `POST /v1/analyze/wallet`

**Request Body**:
```json
{
  "address": "0xabcdef1234567890abcdef1234567890abcdef12",
  "chain": "ethereum",
  "transaction_limit": 100,
  "include_nfts": false
}
```

**Response**:
```json
{
  "wallet": {
    "address": "0xabcdef1234567890abcdef1234567890abcdef12",
    "classification": "whale",
    "total_value_usd": 1234567.89
  },
  "activity_summary": {
    "total_transactions": 1543,
    "first_transaction": "2021-01-15T10:30:00Z",
    "last_transaction": "2024-01-15T14:22:00Z",
    "avg_transaction_value": 5432.10
  },
  "top_holdings": [
    {
      "token": "ETH",
      "balance": "123.456",
      "value_usd": 234567.89,
      "percentage": 18.9
    }
  ],
  "trading_patterns": {
    "preferred_dexes": ["Uniswap V3", "1inch"],
    "trading_frequency": "high",
    "risk_tolerance": "medium"
  }
}
```

## 🚨 Monitoring & Alerts

### Create Monitor
**Endpoint**: `POST /v1/monitors`

**Request Body**:
```json
{
  "type": "wallet",
  "target": "0xabcdef1234567890abcdef1234567890abcdef12",
  "conditions": {
    "transaction_threshold": 10000,
    "new_token_purchases": true,
    "large_transfers": true
  },
  "webhook_url": "https://your-app.com/webhook",
  "notification_channels": ["email", "telegram"]
}
```

### Create Alert
**Endpoint**: `POST /v1/alerts`

**Request Body**:
```json
{
  "name": "High Risk Token Alert",
  "conditions": {
    "risk_score": {
      "operator": "greater_than",
      "value": 7
    },
    "social_mentions": {
      "operator": "spike",
      "threshold": 500,
      "timeframe": "1h"
    }
  },
  "actions": {
    "webhook": "https://your-app.com/alerts",
    "email": "alerts@your-company.com"
  }
}
```

## 📈 Market Data Endpoints

### Price Data
**Endpoint**: `GET /v1/data/prices`

**Parameters**:
- `tokens`: Comma-separated list of token addresses
- `vs_currency`: Base currency (default: usd)
- `timeframe`: 1h, 24h, 7d, 30d

### Social Data
**Endpoint**: `GET /v1/data/social`

**Parameters**:
- `token`: Token address
- `platforms`: twitter,telegram,discord,reddit
- `timeframe`: 1h, 24h, 7d

### Blockchain Data
**Endpoint**: `GET /v1/data/blockchain`

**Parameters**:
- `chain`: ethereum, bsc, polygon, arbitrum
- `data_type`: transactions, contracts, events
- `from_block`: Starting block number
- `to_block`: Ending block number

## 🔧 Utility Endpoints

### Health Check
**Endpoint**: `GET /v1/health`

**Response**:
```json
{
  "status": "healthy",
  "version": "1.2.3",
  "uptime": 99.99,
  "response_time_ms": 45
}
```

### Rate Limit Status
**Endpoint**: `GET /v1/rate-limit`

**Response**:
```json
{
  "limit": 1000,
  "remaining": 847,
  "reset_time": "2024-01-15T15:00:00Z",
  "tier": "premium"
}
```

## 📝 Request/Response Formats

### Standard Error Response
```json
{
  "error": {
    "code": "INVALID_ADDRESS",
    "message": "The provided address is not a valid Ethereum address",
    "details": {
      "field": "address",
      "provided_value": "0xinvalid"
    }
  }
}
```

### Pagination
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 50,
    "total": 1234,
    "total_pages": 25
  }
}
```

## 🚦 Rate Limits

### Free Tier
- **Requests per minute**: 60
- **Requests per day**: 1,000
- **Concurrent requests**: 5

### Premium Tier
- **Requests per minute**: 600
- **Requests per day**: 50,000
- **Concurrent requests**: 50

### Enterprise Tier
- **Custom limits**: Negotiated based on usage
- **Dedicated resources**: Isolated infrastructure
- **SLA guarantees**: 99.9% uptime commitment

## 🔐 Security Best Practices

### API Key Security
- Store API keys securely (environment variables, key management systems)
- Rotate keys regularly (recommended: every 90 days)
- Use different keys for different environments
- Monitor key usage for anomalies

### Request Security
- Always use HTTPS for API requests
- Validate and sanitize all input parameters
- Implement proper error handling
- Use webhook signatures for callback verification

## 📚 SDK Libraries

### Python SDK
```bash
pip install vectorai-python
```

```python
from vectorai import VectorAI

client = VectorAI(api_key="your_api_key")
analysis = client.analyze_token("0x1234...abcd")
```

### JavaScript/Node.js SDK
```bash
npm install vectorai-js
```

```javascript
const VectorAI = require('vectorai-js');

const client = new VectorAI({ apiKey: 'your_api_key' });
const analysis = await client.analyzeToken('0x1234...abcd');
```

### Go SDK
```bash
go get github.com/vectorai/vectorai-go
```

```go
import "github.com/vectorai/vectorai-go"

client := vectorai.NewClient("your_api_key")
analysis, err := client.AnalyzeToken("0x1234...abcd")
```

## 🆘 Support & Resources

- **Documentation**: [docs.vector-ai.pro](https://docs.vector-ai.pro)
- **API Status**: [status.vector-ai.pro](https://status.vector-ai.pro)
- **Support Email**: api-support@vector-ai.pro
- **Community**: [Telegram Developer Group](https://t.me/vectorai_developers)

⚠️ **Note**: This API is currently in beta. Breaking changes may occur with advance notice. Subscribe to our developer newsletter for updates. 
