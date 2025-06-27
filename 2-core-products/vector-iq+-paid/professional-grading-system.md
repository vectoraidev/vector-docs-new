# Professional Grading System

VECTOR IQ+ Bot uses a sophisticated, multi grading system similar to credit ratings, providing clear A+ to F grades across all analysis categories.

## Grade Scale

### 🟢 A+ / A Grades (85-100 points)

* **Excellent Risk Profile**: Minimal identified risks
* **Strong Fundamentals**: Solid security, liquidity, and distribution
* **Recommended**: Generally safe for investment consideration
* **Color Coding**: Green indicators for immediate recognition

### 🟡 B Grades (70-84 points)

* **Good Risk Profile**: Manageable risk factors identified
* **Solid Fundamentals**: Good security with minor concerns
* **Acceptable**: Reasonable investment consideration with due diligence
* **Color Coding**: Yellow/Orange indicators

### 🟠 C Grades (55-69 points)

* **Moderate Risk Profile**: Several risk factors present
* **Mixed Fundamentals**: Some concerns across multiple categories
* **Use Caution**: Requires careful analysis and risk tolerance
* **Color Coding**: Orange indicators for caution

### 🔴 D Grades (30-54 points)

* **Poor Risk Profile**: Significant risks identified
* **Weak Fundamentals**: Major concerns in key areas
* **High Risk**: Not recommended for most users
* **Color Coding**: Red indicators for warning

### 🔴 F Grades (0-29 points)

* **Critical Risk Profile**: Severe risks or red flags
* **Failed Analysis**: Major security or manipulation concerns
* **Avoid**: Strong recommendation to avoid
* **Color Coding**: Red indicators with critical warnings

## Weighted Scoring System

Each analysis feature contributes to the overall grade based on its importance for token safety and legitimacy:

### Primary Risk Factors (Highest Weight)

* **Security Analysis**: 2.0x weight - Most critical for user safety
* **Holder Analysis**: 1.5x weight - Whale concentration affects price stability
* **Wash Trading Analysis**: 1.3x weight - Market manipulation detection

### Secondary Risk Factors (Standard Weight)

* **Market Analysis**: 1.0x weight - Liquidity and trading health
* **Sniper Analysis**: 1.0x weight - Launch pattern analysis
* **Link Analysis**: 0.5x weight - Informational transparency indicator

## Grade Calculation Process

### 1. Individual Feature Scoring

Each feature starts with a base score (typically 50) and is adjusted based on:

* **Positive Factors**: Add points for good indicators
* **Risk Factors**: Subtract points for concerning patterns
* **Data Quality**: Adjust for data availability and confidence

### 2. Weighted Combination

Individual scores are combined using the weight system:

```
Overall Score = (Security×2.0 + Holder×1.5 + WashTrading×1.3 + Market×1.0 + Sniper×1.0) / Total Weights
```

### 3. Grade Assignment

The numerical score is converted to letter grades:

* **90-100**: A+ (Exceptional)
* **85-89**: A (Excellent)
* **80-84**: A- (Very Good)
* **75-79**: B+ (Good)
* **70-74**: B (Above Average)
* **65-69**: B- (Acceptable)
* **60-64**: C+ (Below Average)
* **55-59**: C (Poor)
* **50-54**: C- (Very Poor)
* **40-49**: D (High Risk)
* **30-39**: D- (Very High Risk)
* **0-29**: F (Critical Risk)

## Risk Factor Analysis

### Security Risk Factors

* Unverified contracts
* Active ownership with concerning permissions
* High taxes or honeypot indicators
* Known malicious patterns

### Market Risk Factors

* Very low liquidity (< $2,000)
* Extreme market cap to liquidity ratios (> 100x)
* Minimal trading volume
* Price impact concerns

### Holder Risk Factors

* Extreme whale concentration (> 50% by few holders)
* Very few real holders
* Suspicious distribution patterns
* Contract-heavy holder base

### Trading Risk Factors

* Rapid consecutive trades between same wallets
* Circular trading patterns
* Volume concentration in few wallets
* Identical transaction amounts

## Positive Indicators

### Security Strengths

* Verified contract source code
* Renounced ownership
* No taxes or reasonable taxes
* Clean security audit results

### Market Strengths

* Sufficient liquidity (> $30,000)
* Healthy MC/LP ratios (< 15x)
* Consistent trading volume
* Multiple trading pairs

### Holder Strengths

* Distributed ownership
* Growing holder base
* Mix of small and medium holders
* Low whale concentration

### Trading Strengths

* Organic trading patterns
* Diverse wallet participation
* Reasonable trade timing
* Volume spread across many wallets

***

_The grading system is designed to be conservative, prioritizing user safety over potential upside._
