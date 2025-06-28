# Getting Started

1. **Invite the Bot**
   * Add @scopeaitg\_bot to your Telegram group
   * Ensure the bot has necessary permissions:
     * ✅ Read messages
     * ✅ Send messages
     * ✅ Delete messages (for moderation)
     * ✅ Pin messages (for important announcements)
2.  **Initial Configuration**

    ```
    /start                    # Initialize the bot
    /help                     # View all available commands
    /config                   # Access configuration menu (admin only)
    ```
3.  **Test Basic Features**

    ```
    /summary                  # Test summarization
    /ask How is everyone today? # Test Q&A system
    /status                   # Check bot health
    ```

###

### ⚙️ Configuration Options

#### **Admin Settings**

**Set Up Administrators**

```
/admin add @username         # Add new admin
/admin remove @username      # Remove admin
/admin list                  # View all admins
```

**Configure Intelligent Ask System**

```
/intelligentask status       # Check system status
/intelligentask triggers     # View trigger types
/intelligentask test         # Test the system
```

#### **Community Customization**

**Engagement Settings**

```
/points config               # Configure point system
/leaderboard settings        # Customize leaderboards
/sentiment config            # Adjust sentiment analysis
```

**Security Configuration**

```
/security config             # Access security settings
- Scammer detection sensitivity
- Auto-moderation rules
- Admin verification requirements
- Emergency lockdown settings
```

### 💰 Wallet Integration Setup

#### **For Individual Users**

**Connect Your Wallet**

1.  **Private Chat Setup**

    ```
    /start                    # Start private chat with bot
    /connect                  # Begin wallet setup process
    ```
2. **Secure Wallet Creation**
   * Bot generates encrypted wallet
   * Private key stored with AES-256 encryption
   * Backup phrase provided (store safely!)
3.  **Test Your Wallet**

    ```
    /balance                  # Check wallet balance
    /tip @friend 0.001 ETH    # Test tipping functionality
    ```

**Security Best Practices**

* ⚠️ **Never share your backup phrase**
* ✅ **Test with small amounts first**
* ✅ **Keep backup phrase offline and secure**
* ✅ **Verify recipient addresses before large transfers**

#### **For Community Admins**

**Enable Tipping in Your Group**

1.  **Group Configuration**

    ```
    /wallet enable            # Enable wallet features
    /tip config               # Configure tipping settings
    ```
2. **Set Tipping Rules**
   * Minimum tip amounts
   * Maximum daily tip limits
   * Allowed tokens for tipping
   * Gas fee management

### 🧠 AI Features Configuration

#### **Summarization Settings**

**Default Timeframes**

```
/summary config
- Default timeframe (24h, 3d, 1w)
- Language preferences
- Summary length (short, medium, detailed)
- Reference link inclusion
```

**Content Filtering**

* Include/exclude certain message types
* Filter out specific users or bots
* Focus on specific topics or keywords
* Sentiment-based filtering options

#### **Question & Answer Optimization**

**Context Settings**

```
/ask config
- Default search timeframe
- Source verification levels
- Response detail level
- Reference formatting
```

**Custom Prompts**

* Community-specific context prompts
* Industry-focused responses
* Language and tone preferences
* Specialized knowledge areas

### 🔒 Security Configuration

#### **Scammer Detection Settings**

**Detection Sensitivity**

```
/security scammer config
- Detection sensitivity (low, medium, high)
- Automatic action levels
- Warning message templates
- Admin notification preferences
```

**Whitelist Management**

```
/security whitelist add @user    # Add trusted user
/security whitelist remove @user # Remove from whitelist
/security whitelist view         # View whitelist
```

#### **Admin Verification**

**Verification Levels**

```
/security admin config
- Telegram API verification
- Database cross-reference
- Manual admin approval
- Automatic role assignment
```

### 📊 Analytics & Monitoring

#### **Community Insights**

**Engagement Tracking**

```
/stats community             # View community metrics
/leaderboard weekly         # See most active members
/sentiment trends           # Track sentiment over time
```

**Content Analysis**

```
/analyze trending           # See trending topics
/analyze sentiment $TOKEN   # Token-specific sentiment
/analyze user @username     # Individual user analysis
```

#### **Performance Monitoring**

**Bot Health Checks**

```
/status                     # Overall bot status
/status detailed            # Detailed system metrics
/logs recent                # Recent activity logs
```

### 🛠️ Advanced Features

#### **Business Intelligence**

**Market Analysis**

```
/sentiment $ETH             # Token sentiment analysis
/business                   # Generate business plans
/admin                      # Track admin announcements
```

**Custom Queries**

```
/query "alpha calls"        # Search for specific content
/ask about recent partnerships # Contextual questions
/translate spanish          # Multi-language support
```

#### **Automation Rules**

**Smart Notifications**

```
/notify config
- Urgent discussion alerts
- Confusion detection responses
- Market volatility notifications
- Technical discussion summaries
```

### 🚨 Troubleshooting

#### **Common Issues**

**Bot Not Responding**

1. Check bot permissions in group settings
2. Verify bot is not muted or restricted
3. Test with `/status` command
4. Contact support if issues persist

**Wallet Connection Issues**

1. Ensure you're in private chat with bot
2. Check your Telegram settings allow bot messages
3. Try `/disconnect` then `/connect` again
4. Contact support with error messages

**Intelligent Ask Not Triggering**

1. Check if 30-minute cooldown is active
2. Verify trigger keywords are being used
3. Test manually with `/intelligentask test`
4. Review trigger settings with `/intelligentask triggers`

#### **Error Messages**

**"Vector AI is experiencing technical difficulties"**

* **Cause**: AI API rate limits or temporary outage
* **Solution**: Wait 2-3 minutes and try again
* **Prevention**: Use commands moderately during peak hours

**"Insufficient balance for transaction"**

* **Cause**: Not enough ETH for gas fees or token balance too low
* **Solution**: Add more ETH to your wallet or reduce tip amount
* **Prevention**: Check `/balance` before large transactions

**"User not found or has no recent activity"**

* **Cause**: Username not found in recent chat history
* **Solution**: Ensure user has sent messages recently
* **Prevention**: Users must be active in chat to be analyzed



