# Core Analysis Features

VECTOR IQ+ Bot provides comprehensive analysis across six key areas, each with detailed grading and risk assessment.

## 🔒 Security Analysis (A+ to F Grade)
**Data Sources:** EVA AI, GoPlus, Token Sniffer, Etherscan

- **Contract Security Assessment**: Smart contract vulnerability scanning using EVA AI as primary source
- **Ownership & Control Analysis**: Detection of renounced ownership, active owners, and control mechanisms  
- **Honeypot & Rug Pull Detection**: Advanced algorithms identify potential scam contracts
- **Tax Analysis**: Automatic detection of buy/sell taxes and hidden fees
- **Malicious Database Checks**: Cross-references tokens against Token Sniffer's malicious database
- **LP Lock Verification**: Checks liquidity pool lock status and duration

## 📊 Market Analysis (A+ to F Grade)  
**Data Sources:** DexScreener, Enhanced DexScreener Integration

- **Real-time Price & Volume Analysis**: Live market data integration
- **Liquidity Assessment**: Evaluates liquidity depth and price impact risks
- **Market Cap to Liquidity Ratios**: Identifies tokens with dangerous MC/LP ratios
- **Trading Volume Patterns**: Analyzes 24h volume relative to liquidity
- **Price Impact Assessment**: Calculates potential slippage for trades
- **Enhanced Listing Detection**: Identifies tokens with paid DexScreener features

## 👥 Holder Analysis (A+ to F Grade)
**Data Sources:** GoPlus, EVA, Etherscan, Token Sniffer  

- **Holder Distribution Analysis**: Evaluates concentration of token ownership
- **Whale Concentration Assessment**: Identifies large holder risks
- **Contract vs Real Holder Detection**: Distinguishes between contract addresses and real users
- **Growth Metrics**: Analyzes holder count trends and distribution patterns
- **Top Holder Identification**: Tracks largest token holders and their behavior

## 🚨 Wash Trading Analysis (A+ to F Grade)
**Advanced Transaction Pattern Detection**

- **Launch Pattern Analysis**: Examines first 20 buyers for coordinated behavior (70% weight)
- **Recent Trading Analysis**: Analyzes last 200 transactions for manipulation (30% weight)
- **Rapid Trading Detection**: Identifies suspiciously fast consecutive trades
- **Round-trip Pattern Recognition**: Detects circular trading between same wallets
- **Volume Concentration Analysis**: Flags when few wallets generate most volume
- **Identical Amount Detection**: Identifies repeated exact transaction amounts

## 🎯 Sniper & Seller Analysis (A+ to F Grade)
**Early Buyer & Exit Strategy Detection**

- **Early Buyer Identification**: Tracks first purchasers and their behavior
- **Large Transaction Monitoring**: Monitors whale movements and large trades
- **Exit Strategy Detection**: Identifies coordinated selling patterns
- **Sniper Bot Detection**: Recognizes automated trading bots at launch

## 🔗 AI Link Analysis (Informational)
**OpenAI-Powered Contract Analysis**

- **Smart Contract Code Analysis**: AI extracts project links from contract source code
- **Social Media Pattern Recognition**: Identifies Twitter, Telegram, Discord references
- **Website Link Extraction**: Finds official project websites and documentation
- **Link Verification**: Filters out development libraries and focuses on project-specific links

---

*Each analysis category provides detailed risk factors and positive indicators to help you make informed decisions.* 