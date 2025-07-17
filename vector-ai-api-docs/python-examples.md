# Python Examples

Complete Python examples for integrating with the Vector AI API. From basic token analysis to advanced trading bots - everything you need to get started.

## 📋 Table of Contents

* [🚀 Quick Start](python-examples.md#quick-start)
* [🔧 Installation](python-examples.md#installation)
* [🔑 Authentication](python-examples.md#authentication)
* [📊 Basic Analysis](python-examples.md#basic-analysis)
* [🤖 Advanced Examples](python-examples.md#advanced-examples)
* [🛠️ Production Ready](python-examples.md#production-ready)
* [🎯 Use Cases](python-examples.md#use-cases)
* [❌ Error Handling](python-examples.md#error-handling)

***

## 🚀 Quick Start

### Minimal Example

```python
import requests

# Your API key
api_key = "vect_your_api_key_here"

# Analyze a token
response = requests.post(
    "https://api.vector-ai.pro/api/v1/token/scan",
    headers={
        "X-Vector-API-Key": api_key,
        "Content-Type": "application/json"
    },
    json={
        "contract_address": "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
        "chain": "eth"
    }
)

if response.status_code == 200:
    data = response.json()
    print(f"Token: {data['data']['token_info']['name']}")
    print(f"Score: {data['data']['vector_score']}")
    print(f"Grade: {data['data']['grade']}")
else:
    print(f"Error: {response.status_code}")
```

***

## 🔧 Installation

### Required Dependencies

```bash
pip install requests python-dotenv aiohttp asyncio
```

### Optional Dependencies

```bash
# For advanced features
pip install pandas numpy matplotlib seaborn
pip install web3 eth-utils
pip install redis celery  # For caching and task queues
```

### Environment Setup

Create a `.env` file:

```env
VECTOR_API_KEY=vect_your_api_key_here
VECTOR_API_BASE_URL=https://api.vector-ai.pro
```

***

## 🔑 Authentication

### Environment Variables

```python
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

API_KEY = os.getenv('VECTOR_API_KEY')
BASE_URL = os.getenv('VECTOR_API_BASE_URL', 'https://api.vector-ai.pro')

if not API_KEY:
    raise ValueError("VECTOR_API_KEY environment variable is required")
```

### API Key Validation

```python
import requests

def validate_api_key(api_key: str) -> bool:
    """Validate API key before using it"""
    try:
        response = requests.post(
            f"{BASE_URL}/api/v1/auth/validate",
            headers={"X-Vector-API-Key": api_key},
            timeout=10
        )
        return response.status_code == 200
    except Exception as e:
        print(f"API key validation failed: {e}")
        return False

# Usage
if validate_api_key(API_KEY):
    print("✅ API key is valid")
else:
    print("❌ API key is invalid")
    exit(1)
```

***

## 📊 Basic Analysis

### Simple Token Analyzer

```python
import requests
from typing import Dict, Any, Optional
from dataclasses import dataclass

@dataclass
class TokenInfo:
    name: str
    symbol: str
    address: str
    chain: str
    score: int
    grade: str
    recommendation: str

class VectorAPI:
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.base_url = "https://api.vector-ai.pro"
        self.headers = {
            'X-Vector-API-Key': api_key,
            'Content-Type': 'application/json'
        }
    
    def analyze_token(self, contract_address: str, chain: str = "eth") -> Optional[TokenInfo]:
        """Analyze a token and return basic info"""
        try:
            response = requests.post(
                f"{self.base_url}/api/v1/token/scan",
                headers=self.headers,
                json={
                    "contract_address": contract_address,
                    "chain": chain
                },
                timeout=30
            )
            
            if response.status_code == 200:
                data = response.json()['data']
                return TokenInfo(
                    name=data['token_info']['name'],
                    symbol=data['token_info']['symbol'],
                    address=contract_address,
                    chain=chain,
                    score=data['vector_score'],
                    grade=data['grade'],
                    recommendation=data['recommendation']
                )
            else:
                print(f"API Error: {response.status_code}")
                return None
                
        except Exception as e:
            print(f"Analysis failed: {e}")
            return None

# Usage
api = VectorAPI(API_KEY)
token = api.analyze_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")

if token:
    print(f"Token: {token.name} ({token.symbol})")
    print(f"Score: {token.score}/100")
    print(f"Grade: {token.grade}")
    print(f"Recommendation: {token.recommendation}")
```

### Security-Focused Analysis

```python
def analyze_token_security(api: VectorAPI, contract_address: str, chain: str = "eth") -> Dict[str, Any]:
    """Focus on security analysis only"""
    try:
        response = requests.post(
            f"{api.base_url}/api/v1/token/scan",
            headers=api.headers,
            json={
                "contract_address": contract_address,
                "chain": chain,
                "features": ["security"]  # Only security analysis
            },
            timeout=30
        )
        
        if response.status_code == 200:
            data = response.json()['data']
            security = data['analysis']['security']
            
            return {
                "is_safe": security['honeypot_risk'] == "✅ SAFE",
                "rug_pull_risk": security['rug_pull_risk'],
                "contract_verified": security['contract_verified'],
                "malicious_functions": security['malicious_functions'],
                "can_mint": security['ownership_analysis']['can_mint'],
                "overall_score": data['vector_score']
            }
        else:
            return {"error": f"API Error: {response.status_code}"}
            
    except Exception as e:
        return {"error": str(e)}

# Usage
security_info = analyze_token_security(api, "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
print(f"Is safe: {security_info.get('is_safe', 'Unknown')}")
print(f"Contract verified: {security_info.get('contract_verified', 'Unknown')}")
```

***

## 🤖 Advanced Examples

### Batch Token Analyzer

```python
import asyncio
import aiohttp
from typing import List, Dict
import time

class AsyncVectorAPI:
    def __init__(self, api_key: str, max_concurrent: int = 5):
        self.api_key = api_key
        self.base_url = "https://api.vector-ai.pro"
        self.headers = {
            'X-Vector-API-Key': api_key,
            'Content-Type': 'application/json'
        }
        self.semaphore = asyncio.Semaphore(max_concurrent)
    
    async def analyze_token_async(self, session: aiohttp.ClientSession, contract_address: str, chain: str = "eth") -> Dict:
        """Analyze a single token asynchronously"""
        async with self.semaphore:  # Rate limiting
            try:
                async with session.post(
                    f"{self.base_url}/api/v1/token/scan",
                    headers=self.headers,
                    json={
                        "contract_address": contract_address,
                        "chain": chain
                    },
                    timeout=aiohttp.ClientTimeout(total=30)
                ) as response:
                    if response.status == 200:
                        data = await response.json()
                        return {
                            "address": contract_address,
                            "success": True,
                            "score": data['data']['vector_score'],
                            "grade": data['data']['grade'],
                            "name": data['data']['token_info']['name']
                        }
                    else:
                        return {
                            "address": contract_address,
                            "success": False,
                            "error": f"HTTP {response.status}"
                        }
                        
            except Exception as e:
                return {
                    "address": contract_address,
                    "success": False,
                    "error": str(e)
                }
    
    async def analyze_multiple_tokens(self, contract_addresses: List[str], chain: str = "eth") -> List[Dict]:
        """Analyze multiple tokens concurrently"""
        async with aiohttp.ClientSession() as session:
            tasks = [
                self.analyze_token_async(session, address, chain)
                for address in contract_addresses
            ]
            return await asyncio.gather(*tasks)

# Usage
async def main():
    api = AsyncVectorAPI(API_KEY, max_concurrent=3)
    
    tokens = [
        "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",  # USDC
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",  # WETH
        "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984"   # UNI
    ]
    
    start_time = time.time()
    results = await api.analyze_multiple_tokens(tokens)
    end_time = time.time()
    
    print(f"Analyzed {len(tokens)} tokens in {end_time - start_time:.2f} seconds")
    
    for result in results:
        if result['success']:
            print(f"✅ {result['name']}: {result['score']}/100 ({result['grade']})")
        else:
            print(f"❌ {result['address']}: {result['error']}")

# Run the async example
# asyncio.run(main())
```

### Trading Bot Example

```python
import time
from typing import List, Dict
import logging

# Set up logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class TradingBot:
    def __init__(self, api_key: str, min_score: int = 75):
        self.api = VectorAPI(api_key)
        self.min_score = min_score
        self.analyzed_tokens = set()
        self.watchlist = []
    
    def is_token_safe(self, contract_address: str, chain: str = "eth") -> bool:
        """Check if token meets safety criteria"""
        try:
            if contract_address in self.analyzed_tokens:
                return False  # Already analyzed
            
            self.analyzed_tokens.add(contract_address)
            
            # Get security analysis
            security = analyze_token_security(self.api, contract_address, chain)
            
            if security.get('error'):
                logger.error(f"Analysis failed for {contract_address}: {security['error']}")
                return False
            
            # Safety checks
            if not security.get('is_safe', False):
                logger.warning(f"Token {contract_address} failed safety check")
                return False
            
            if security.get('overall_score', 0) < self.min_score:
                logger.info(f"Token {contract_address} score too low: {security['overall_score']}")
                return False
            
            if security.get('can_mint', True):
                logger.warning(f"Token {contract_address} can mint - risky")
                return False
            
            return True
            
        except Exception as e:
            logger.error(f"Error checking token {contract_address}: {e}")
            return False
    
    def scan_new_tokens(self, token_addresses: List[str]) -> List[str]:
        """Scan new tokens and return safe ones"""
        safe_tokens = []
        
        for address in token_addresses:
            logger.info(f"Analyzing token: {address}")
            
            if self.is_token_safe(address):
                safe_tokens.append(address)
                logger.info(f"✅ Safe token found: {address}")
            
            # Rate limiting
            time.sleep(1)
        
        return safe_tokens
    
    def add_to_watchlist(self, token_address: str) -> None:
        """Add token to watchlist"""
        token_info = self.api.analyze_token(token_address)
        if token_info:
            self.watchlist.append({
                'address': token_address,
                'name': token_info.name,
                'symbol': token_info.symbol,
                'score': token_info.score,
                'added_at': time.time()
            })
            logger.info(f"Added {token_info.name} to watchlist")

# Usage
bot = TradingBot(API_KEY, min_score=80)

# Example: Scan some tokens
new_tokens = [
    "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",
    "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"
]

safe_tokens = bot.scan_new_tokens(new_tokens)
print(f"Found {len(safe_tokens)} safe tokens")

for token in safe_tokens:
    bot.add_to_watchlist(token)
```

***

## 🛠️ Production Ready

### Enterprise-Grade Client

```python
import requests
import time
import json
from typing import Dict, Any, Optional, List
from dataclasses import dataclass, asdict
from functools import wraps
import logging
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

@dataclass
class APIResponse:
    success: bool
    data: Optional[Dict[str, Any]] = None
    error: Optional[str] = None
    status_code: Optional[int] = None
    response_time: Optional[float] = None

class VectorAPIClient:
    def __init__(
        self,
        api_key: str,
        base_url: str = "https://api.vector-ai.pro",
        timeout: int = 30,
        max_retries: int = 3,
        enable_caching: bool = True
    ):
        self.api_key = api_key
        self.base_url = base_url
        self.timeout = timeout
        self.max_retries = max_retries
        self.enable_caching = enable_caching
        
        # Setup session with retry strategy
        self.session = requests.Session()
        retry_strategy = Retry(
            total=max_retries,
            backoff_factor=1,
            status_forcelist=[429, 500, 502, 503, 504],
            method_whitelist=["HEAD", "GET", "OPTIONS", "POST"]
        )
        adapter = HTTPAdapter(max_retries=retry_strategy)
        self.session.mount("http://", adapter)
        self.session.mount("https://", adapter)
        
        # Setup headers
        self.session.headers.update({
            'X-Vector-API-Key': api_key,
            'Content-Type': 'application/json',
            'User-Agent': 'VectorAI-Python-Client/1.0'
        })
        
        # Setup logging
        self.logger = logging.getLogger(__name__)
        
        # Simple in-memory cache
        self.cache = {} if enable_caching else None
    
    def _cache_key(self, endpoint: str, params: Dict) -> str:
        """Generate cache key"""
        return f"{endpoint}:{json.dumps(params, sort_keys=True)}"
    
    def _get_cached(self, cache_key: str) -> Optional[Dict]:
        """Get cached response"""
        if not self.cache:
            return None
        
        cached = self.cache.get(cache_key)
        if cached and time.time() - cached['timestamp'] < 300:  # 5 min TTL
            return cached['data']
        return None
    
    def _set_cache(self, cache_key: str, data: Dict) -> None:
        """Set cached response"""
        if self.cache is not None:
            self.cache[cache_key] = {
                'data': data,
                'timestamp': time.time()
            }
    
    def _make_request(self, endpoint: str, data: Dict[str, Any]) -> APIResponse:
        """Make API request with error handling"""
        cache_key = self._cache_key(endpoint, data)
        
        # Check cache
        if self.enable_caching:
            cached = self._get_cached(cache_key)
            if cached:
                return APIResponse(success=True, data=cached)
        
        start_time = time.time()
        
        try:
            response = self.session.post(
                f"{self.base_url}{endpoint}",
                json=data,
                timeout=self.timeout
            )
            
            response_time = time.time() - start_time
            
            if response.status_code == 200:
                result = response.json()
                
                # Cache successful response
                if self.enable_caching:
                    self._set_cache(cache_key, result)
                
                return APIResponse(
                    success=True,
                    data=result,
                    status_code=response.status_code,
                    response_time=response_time
                )
            else:
                error_msg = f"HTTP {response.status_code}: {response.text}"
                return APIResponse(
                    success=False,
                    error=error_msg,
                    status_code=response.status_code,
                    response_time=response_time
                )
                
        except requests.exceptions.RequestException as e:
            return APIResponse(
                success=False,
                error=str(e),
                response_time=time.time() - start_time
            )
    
    def analyze_token(
        self,
        contract_address: str,
        chain: str = "eth",
        features: Optional[List[str]] = None,
        include_metadata: bool = False
    ) -> APIResponse:
        """Analyze token with comprehensive error handling"""
        self.logger.info(f"Analyzing token: {contract_address} on {chain}")
        
        payload = {
            "contract_address": contract_address,
            "chain": chain
        }
        
        if features:
            payload["features"] = features
        if include_metadata:
            payload["include_metadata"] = True
        
        return self._make_request("/api/v1/token/scan", payload)
    
    def get_token_research(self, contract_address: str, chain: str = "eth") -> APIResponse:
        """Get AI research for token"""
        return self._make_request("/api/v1/token/research", {
            "contract_address": contract_address,
            "chain": chain
        })
    
    def validate_key(self) -> APIResponse:
        """Validate API key"""
        return self._make_request("/api/v1/auth/validate", {})
    
    def get_health(self) -> APIResponse:
        """Get API health status"""
        start_time = time.time()
        try:
            response = self.session.get(f"{self.base_url}/health", timeout=10)
            return APIResponse(
                success=response.status_code == 200,
                data=response.json() if response.status_code == 200 else None,
                status_code=response.status_code,
                response_time=time.time() - start_time
            )
        except Exception as e:
            return APIResponse(
                success=False,
                error=str(e),
                response_time=time.time() - start_time
            )

# Usage
client = VectorAPIClient(API_KEY)

# Validate connection
health = client.get_health()
if health.success:
    print(f"✅ API is healthy (response time: {health.response_time:.2f}s)")
else:
    print(f"❌ API health check failed: {health.error}")

# Analyze token
result = client.analyze_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")
if result.success:
    token_data = result.data['data']
    print(f"Token: {token_data['token_info']['name']}")
    print(f"Score: {token_data['vector_score']}")
    print(f"Response time: {result.response_time:.2f}s")
else:
    print(f"Analysis failed: {result.error}")
```

***

## 🎯 Use Cases

### Portfolio Tracker

```python
import pandas as pd
from datetime import datetime
import json

class PortfolioTracker:
    def __init__(self, api_key: str):
        self.client = VectorAPIClient(api_key)
        self.portfolio = []
    
    def add_token(self, contract_address: str, chain: str = "eth", amount: float = 0):
        """Add token to portfolio"""
        result = self.client.analyze_token(contract_address, chain)
        
        if result.success:
            token_data = result.data['data']
            self.portfolio.append({
                'address': contract_address,
                'chain': chain,
                'name': token_data['token_info']['name'],
                'symbol': token_data['token_info']['symbol'],
                'amount': amount,
                'vector_score': token_data['vector_score'],
                'grade': token_data['grade'],
                'recommendation': token_data['recommendation'],
                'last_updated': datetime.now().isoformat()
            })
            print(f"✅ Added {token_data['token_info']['name']} to portfolio")
        else:
            print(f"❌ Failed to add token: {result.error}")
    
    def update_scores(self):
        """Update scores for all tokens in portfolio"""
        for token in self.portfolio:
            result = self.client.analyze_token(token['address'], token['chain'])
            
            if result.success:
                token_data = result.data['data']
                token['vector_score'] = token_data['vector_score']
                token['grade'] = token_data['grade']
                token['recommendation'] = token_data['recommendation']
                token['last_updated'] = datetime.now().isoformat()
                print(f"✅ Updated {token['name']}")
            else:
                print(f"❌ Failed to update {token['name']}: {result.error}")
    
    def get_portfolio_summary(self) -> pd.DataFrame:
        """Get portfolio summary as DataFrame"""
        return pd.DataFrame(self.portfolio)
    
    def get_risky_tokens(self, min_score: int = 70) -> List[Dict]:
        """Get tokens below minimum score"""
        return [token for token in self.portfolio if token['vector_score'] < min_score]
    
    def save_portfolio(self, filename: str):
        """Save portfolio to file"""
        with open(filename, 'w') as f:
            json.dump(self.portfolio, f, indent=2)
    
    def load_portfolio(self, filename: str):
        """Load portfolio from file"""
        with open(filename, 'r') as f:
            self.portfolio = json.load(f)

# Usage
tracker = PortfolioTracker(API_KEY)

# Add tokens
tracker.add_token("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234", amount=1000)  # USDC
tracker.add_token("0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", amount=5)    # WETH

# Get summary
summary = tracker.get_portfolio_summary()
print(summary[['name', 'symbol', 'vector_score', 'grade']])

# Check for risky tokens
risky = tracker.get_risky_tokens(min_score=75)
if risky:
    print(f"⚠️ {len(risky)} risky tokens found:")
    for token in risky:
        print(f"  - {token['name']}: {token['vector_score']}/100")
```

### Token Screener

```python
class TokenScreener:
    def __init__(self, api_key: str):
        self.client = VectorAPIClient(api_key)
        self.criteria = {
            'min_score': 70,
            'max_rug_pull_risk': 'LOW',
            'require_verified': True,
            'allow_minting': False
        }
    
    def set_criteria(self, **kwargs):
        """Update screening criteria"""
        self.criteria.update(kwargs)
    
    def screen_token(self, contract_address: str, chain: str = "eth") -> Dict:
        """Screen a single token"""
        result = self.client.analyze_token(contract_address, chain)
        
        if not result.success:
            return {
                'address': contract_address,
                'passed': False,
                'reason': f"Analysis failed: {result.error}"
            }
        
        data = result.data['data']
        security = data['analysis']['security']
        
        # Check criteria
        checks = {
            'min_score': data['vector_score'] >= self.criteria['min_score'],
            'honeypot_safe': security['honeypot_risk'] == "✅ SAFE",
            'contract_verified': security['contract_verified'] if self.criteria['require_verified'] else True,
            'minting_allowed': not security['ownership_analysis']['can_mint'] if not self.criteria['allow_minting'] else True
        }
        
        passed = all(checks.values())
        
        return {
            'address': contract_address,
            'name': data['token_info']['name'],
            'symbol': data['token_info']['symbol'],
            'score': data['vector_score'],
            'grade': data['grade'],
            'passed': passed,
            'checks': checks,
            'recommendation': data['recommendation']
        }
    
    def screen_multiple(self, addresses: List[str], chain: str = "eth") -> List[Dict]:
        """Screen multiple tokens"""
        results = []
        
        for address in addresses:
            result = self.screen_token(address, chain)
            results.append(result)
            
            # Rate limiting
            time.sleep(1)
        
        return results

# Usage
screener = TokenScreener(API_KEY)

# Set strict criteria
screener.set_criteria(
    min_score=80,
    require_verified=True,
    allow_minting=False
)

# Screen tokens
tokens_to_screen = [
    "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",  # USDC
    "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"   # WETH
]

results = screener.screen_multiple(tokens_to_screen)

# Print results
for result in results:
    status = "✅ PASSED" if result['passed'] else "❌ FAILED"
    print(f"{status} {result['name']} ({result['symbol']})")
    print(f"  Score: {result['score']}/100")
    if not result['passed']:
        failed_checks = [k for k, v in result['checks'].items() if not v]
        print(f"  Failed: {', '.join(failed_checks)}")
```

***

## ❌ Error Handling

### Comprehensive Error Handler

```python
import requests
from typing import Optional
from enum import Enum

class APIError(Exception):
    """Base API error"""
    pass

class RateLimitError(APIError):
    """Rate limit exceeded"""
    pass

class AuthenticationError(APIError):
    """Authentication failed"""
    pass

class ValidationError(APIError):
    """Input validation failed"""
    pass

class ErrorType(Enum):
    RATE_LIMIT = "rate_limit_error"
    AUTH = "authentication_error"
    VALIDATION = "validation_error"
    NETWORK = "network_error"
    UNKNOWN = "unknown_error"

def handle_api_error(response: requests.Response) -> Optional[Exception]:
    """Convert API response to appropriate exception"""
    if response.status_code == 200:
        return None
    
    try:
        error_data = response.json()
        error_type = error_data.get('error', {}).get('type', 'unknown_error')
        message = error_data.get('error', {}).get('message', 'Unknown error')
        
        if error_type == ErrorType.RATE_LIMIT.value:
            return RateLimitError(f"Rate limit exceeded: {message}")
        elif error_type == ErrorType.AUTH.value:
            return AuthenticationError(f"Authentication failed: {message}")
        elif error_type == ErrorType.VALIDATION.value:
            return ValidationError(f"Validation failed: {message}")
        else:
            return APIError(f"API error: {message}")
            
    except json.JSONDecodeError:
        return APIError(f"HTTP {response.status_code}: {response.text}")

# Usage in client
class RobustVectorAPI:
    def __init__(self, api_key: str):
        self.client = VectorAPIClient(api_key)
    
    def analyze_token_safe(self, contract_address: str, chain: str = "eth", max_retries: int = 3) -> Optional[Dict]:
        """Analyze token with robust error handling"""
        for attempt in range(max_retries):
            try:
                result = self.client.analyze_token(contract_address, chain)
                
                if result.success:
                    return result.data
                
                # Handle different error types
                if result.status_code == 429:
                    wait_time = 60 * (2 ** attempt)  # Exponential backoff
                    print(f"Rate limit hit, waiting {wait_time}s...")
                    time.sleep(wait_time)
                    continue
                elif result.status_code == 401:
                    raise AuthenticationError("Invalid API key")
                elif result.status_code == 404:
                    print(f"Token not found: {contract_address}")
                    return None
                else:
                    print(f"API error: {result.error}")
                    if attempt == max_retries - 1:
                        return None
                    time.sleep(2 ** attempt)
                    
            except Exception as e:
                print(f"Attempt {attempt + 1} failed: {e}")
                if attempt == max_retries - 1:
                    return None
                time.sleep(2 ** attempt)
        
        return None

# Usage
robust_api = RobustVectorAPI(API_KEY)
result = robust_api.analyze_token_safe("0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234")

if result:
    print("✅ Analysis successful")
else:
    print("❌ Analysis failed after retries")
```

***

## 🔧 Complete Example: Trading Signal Generator

```python
import time
import json
from datetime import datetime
from typing import List, Dict, Optional
import pandas as pd

class TradingSignalGenerator:
    def __init__(self, api_key: str):
        self.client = VectorAPIClient(api_key)
        self.signals = []
        self.config = {
            'buy_threshold': 80,
            'sell_threshold': 40,
            'hold_min': 60,
            'hold_max': 75
        }
    
    def generate_signal(self, contract_address: str, chain: str = "eth") -> Optional[Dict]:
        """Generate trading signal for a token"""
        result = self.client.analyze_token(contract_address, chain)
        
        if not result.success:
            return None
        
        data = result.data['data']
        score = data['vector_score']
        
        # Determine signal
        if score >= self.config['buy_threshold']:
            signal = "BUY"
        elif score <= self.config['sell_threshold']:
            signal = "SELL"
        elif self.config['hold_min'] <= score <= self.config['hold_max']:
            signal = "HOLD"
        else:
            signal = "NEUTRAL"
        
        # Calculate confidence
        if signal == "BUY":
            confidence = min(100, (score - self.config['buy_threshold']) * 5)
        elif signal == "SELL":
            confidence = min(100, (self.config['sell_threshold'] - score) * 5)
        else:
            confidence = 50
        
        signal_data = {
            'timestamp': datetime.now().isoformat(),
            'address': contract_address,
            'chain': chain,
            'token_name': data['token_info']['name'],
            'token_symbol': data['token_info']['symbol'],
            'vector_score': score,
            'grade': data['grade'],
            'signal': signal,
            'confidence': confidence,
            'recommendation': data['recommendation'],
            'analysis': {
                'security_safe': data['analysis']['security']['honeypot_risk'] == "✅ SAFE",
                'contract_verified': data['analysis']['security']['contract_verified'],
                'market_cap': data['analysis']['market']['market_cap'],
                'liquidity': data['analysis']['market']['liquidity']['status']
            }
        }
        
        self.signals.append(signal_data)
        return signal_data
    
    def get_signals_df(self) -> pd.DataFrame:
        """Get signals as DataFrame"""
        return pd.DataFrame(self.signals)
    
    def save_signals(self, filename: str):
        """Save signals to file"""
        with open(filename, 'w') as f:
            json.dump(self.signals, f, indent=2)
    
    def get_buy_signals(self) -> List[Dict]:
        """Get only buy signals"""
        return [s for s in self.signals if s['signal'] == 'BUY']
    
    def get_sell_signals(self) -> List[Dict]:
        """Get only sell signals"""
        return [s for s in self.signals if s['signal'] == 'SELL']

# Usage
generator = TradingSignalGenerator(API_KEY)

# Generate signals for multiple tokens
tokens = [
    "0xA0b86991c31cC17C4aC3ee2Ca90C7b8d2e5f234",  # USDC
    "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",  # WETH
    "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984"   # UNI
]

print("Generating trading signals...")
for token in tokens:
    signal = generator.generate_signal(token)
    if signal:
        print(f"🎯 {signal['token_name']} ({signal['token_symbol']}):")
        print(f"   Signal: {signal['signal']} (Confidence: {signal['confidence']}%)")
        print(f"   Score: {signal['vector_score']}/100")
        print(f"   Grade: {signal['grade']}")
        print()
    time.sleep(1)  # Rate limiting

# Get summary
signals_df = generator.get_signals_df()
print("Signal Summary:")
print(signals_df[['token_name', 'signal', 'confidence', 'vector_score']].to_string(index=False))

# Save results
generator.save_signals('trading_signals.json')
```

***

## 🎯 Next Steps

1. **Install dependencies** and set up your environment
2. **Get your API key** from the Vector AI dashboard
3. **Start with basic examples** to understand the API
4. **Implement error handling** for production use
5. **Build your specific use case** using the advanced examples

### 📚 Related Documentation

* [**Authentication Guide**](broken-reference) - API key management
* [**Token Analysis Endpoint**](broken-reference) - Detailed API reference
* [**Error Handling**](broken-reference) - Common issues and solutions

***

**Ready to build amazing crypto applications with Python?** Start with the basic examples and work your way up! 🚀
