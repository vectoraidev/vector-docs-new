# Bot Commands & Supported Chains

VECTOR IQ+ Bot provides simple, powerful commands for comprehensive token analysis across multiple blockchain networks.

## Supported Blockchain Networks

### ✅ Fully Supported Chains
- **Ethereum (ETH)**: Complete analysis with all data sources
- **Binance Smart Chain (BSC)**: Full multi-source analysis support  
- **Base Network**: Comprehensive data integration and analysis
- **Polygon (MATIC)**: Complete feature support across all categories
- **Arbitrum**: Full analysis capabilities with all features

### Chain-Specific Features
All supported chains include:
- Security analysis with multiple data sources
- Real-time market data integration
- Holder distribution analysis
- Wash trading pattern detection
- Professional scoring card generation

## Primary Bot Commands

### `/analyze <address>`
**Complete Token Analysis with Professional Grading**
- Performs comprehensive analysis across all 6 categories
- Generates professional A+ to F grades
- Creates custom scoring card with visual representation
- Provides detailed risk factors and positive indicators
- Includes market data and security assessment

**Usage Examples:**
```
/analyze 0x1234567890abcdef1234567890abcdef12345678
/analyze eth:0x1234567890abcdef1234567890abcdef12345678
/analyze bsc:0x1234567890abcdef1234567890abcdef12345678
```

### `/scan <address>`
**Alias for Analyze Command**
- Identical functionality to `/analyze`
- Alternative command for user preference
- Same comprehensive analysis and grading

### `/start`
**Welcome Message and Bot Introduction**
- Comprehensive feature overview
- Grading system explanation
- Usage instructions and examples
- Supported chains and capabilities

### `/help`
**Detailed Help and Documentation**
- Complete command reference
- Grading system details (A+ to F scale)
- Chain specification instructions
- Feature explanations and tips
- Contact information for support

## AI Assistant Commands

### `/ask <question>`
**VGPT AI Assistant for DeFi Education**
- Educational assistance for DeFi concepts
- Token analysis guidance and interpretation
- Trading and investment education
- Risk assessment explanations

**Usage Examples:**
```
/ask What is a honeypot token?
/ask How do I identify wash trading?
/ask What should I look for in token analysis?
/ask Explain market cap to liquidity ratios
```

## Direct Input Methods

### Contract Address Input
**Simple Address Submission**
- Send any valid contract address directly
- Automatic chain detection when possible
- Triggers full analysis automatically
- No command prefix required

### Chain-Specific Address Format
**Specify Blockchain Network**
```
eth:0x1234567890abcdef1234567890abcdef12345678
bsc:0x1234567890abcdef1234567890abcdef12345678
base:0x1234567890abcdef1234567890abcdef12345678
polygon:0x1234567890abcdef1234567890abcdef12345678
arbitrum:0x1234567890abcdef1234567890abcdef12345678
```

## Developer & Testing Commands

### `/testcard`
**Generate Test Scoring Card**
- Development and testing functionality
- Generates sample scoring card with test data
- Used for design validation and testing

### Deep Link Support
**Group Chat Integration**
- Supports deep links from group chats
- Automatic analysis initiation from external links
- Seamless integration with other Telegram bots

## Command Response Format

### Analysis Results Include:
1. **Individual Category Grades**: A+ to F for each analysis type
2. **Overall Score**: Weighted combination of all categories
3. **Risk Factors**: Specific concerns identified in analysis
4. **Positive Indicators**: Strengths and good signs found
5. **Professional Scoring Card**: Visual summary with branding
6. **Market Data**: Real-time pricing, volume, and liquidity
7. **Action Buttons**: Interactive elements for additional information

### Response Time
- **Typical Analysis**: 10-15 seconds for complete analysis
- **Complex Tokens**: Up to 30 seconds for tokens with extensive data
- **Error Handling**: Immediate feedback for invalid addresses
- **Status Updates**: Progress indicators during analysis

## Usage Tips

### For Best Results:
- **Use Full Contract Addresses**: Complete 42-character addresses (0x...)
- **Specify Chain When Unclear**: Use chain prefix for accuracy
- **Wait for Complete Analysis**: Allow full analysis to complete
- **Review All Categories**: Check each grade category for complete picture
- **Consider Risk Factors**: Pay attention to specific risks identified

### Common Use Cases:
- **Pre-Investment Analysis**: Comprehensive due diligence
- **Portfolio Review**: Analyze existing holdings
- **Comparison Analysis**: Compare multiple tokens
- **Educational Learning**: Understand token analysis principles
- **Risk Assessment**: Identify potential red flags

---

*Simple commands provide access to sophisticated analysis - just send a contract address to get started.* 