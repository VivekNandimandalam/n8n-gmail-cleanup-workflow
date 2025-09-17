# Gmail Promotional Mail Auto-Deleter 🗂️

An automated n8n workflow that intelligently deletes promotional emails from your Gmail account with Telegram-based user confirmation for safety and control.

## ✨ Features

- **Daily Automation**: Runs automatically every day at 9:00 AM
- **Smart Filtering**: Targets promotional category emails received before September 1, 2025
- **Interactive Confirmation**: Sends Telegram notification with Continue/Cancel buttons
- **User Control**: Proceed only with explicit user confirmation
- **Safe Operation**: Includes confirmation messages and error handling
- **Batch Processing**: Efficiently handles multiple emails

## 🏗️ Architecture

The workflow consists of two main execution paths:

### Path 1: Daily Schedule & Notification
```
Schedule Trigger (9 AM daily) → Send Telegram Message → End
```

### Path 2: User Response Handler
```
Telegram Trigger (Button Click) → IF Condition → Continue/Cancel Actions
```

## 📋 Prerequisites

### Required Accounts & Setup
1. **n8n Instance** (Cloud or Self-hosted)
2. **Google Cloud Project** with Gmail API enabled
3. **Telegram Bot** created via BotFather
4. **Gmail OAuth2 Credentials**

### Required n8n Nodes
- Schedule Trigger
- Telegram (Send Message)
- Telegram Trigger
- IF Node
- Gmail (Get All & Delete)

## 🚀 Installation

### Step 1: Import Workflow
1. Download the `Auto-gmail-promotional-mail-deleter.json` file
2. In n8n, go to **Workflows** → **Import from File**
3. Select the downloaded JSON file

### Step 2: Configure Credentials

#### Gmail OAuth2 Setup
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create/Select a project
3. Enable Gmail API
4. Create OAuth 2.0 credentials
5. Configure authorized redirect URIs:
   ```
   https://your-n8n-domain.com/rest/oauth2-credential/callback
   ```
6. In n8n: **Settings** → **Credentials** → **Add Gmail OAuth2**

#### Telegram Bot Setup
1. Message [@BotFather](https://t.me/BotFather) on Telegram
2. Create new bot: `/newbot`
3. Save your bot token
4. Get your Chat ID:
   - Send `/start` to your bot
   - Visit: `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
   - Find your `chat.id` in the response
5. In n8n: **Settings** → **Credentials** → **Add Telegram**

### Step 3: Update Configuration
1. **Update Chat ID**: Replace `1014408680` with your Telegram Chat ID in both Telegram nodes
2. **Configure Gmail Filter**: Modify the date filter in "Get many messages" node if needed
3. **Test Credentials**: Verify both Gmail and Telegram connections work

## ⚙️ Configuration Options

### Email Filtering
Current filter targets promotional emails received before September 1, 2025:
```json
{
  "q": "=category:promotions",
  "receivedBefore": "2025-09-01T00:00:00"
}
```

### Customize Filters
- **Multiple Categories**: `category:promotions OR category:social`
- **Date Range**: Modify `receivedBefore` parameter
- **Sender Filter**: Add `from:sender@example.com`
- **Subject Filter**: Add `subject:"keyword"`

### Schedule Modification
Default: Daily at 9:00 AM
```json
{
  "triggerAtHour": 9
}
```

Change to different time or frequency as needed.

## 🔧 Usage

### Initial Setup
1. Import and configure the workflow
2. **Test manually** before activating automation
3. **Activate** the workflow

### Daily Operation
1. At 9:00 AM, you'll receive a Telegram message
2. Click **Continue** to proceed with deletion
3. Click **Cancel** to skip for the day
4. Receive confirmation message after action

### Monitoring
- Check n8n **Executions** tab for workflow runs
- Monitor Telegram for daily notifications
- Review Gmail to confirm deletions

## 🛡️ Safety Features

- **User Confirmation Required**: No automatic deletion without approval
- **Targeted Filtering**: Only specific email categories affected
- **Date Boundaries**: Configurable date filters prevent unintended deletions
- **Status Notifications**: Real-time updates via Telegram
- **Manual Override**: Daily choice to proceed or skip

## 🔍 Troubleshooting

### Common Issues

#### Telegram Buttons Not Working
- Verify bot token is correct
- Check Chat ID matches your account
- Ensure Telegram Trigger is active

#### Gmail Access Issues
- Confirm OAuth2 credentials are valid
- Check Gmail API quotas in Google Cloud Console
- Verify OAuth consent screen is configured

#### Workflow Not Triggering
- Check if workflow is **Active** (toggle switch)
- Verify schedule trigger configuration
- Review execution history for errors

### Debug Tips
1. **Test Each Node Individually**: Use "Test workflow" feature
2. **Check Node Outputs**: Review data passed between nodes
3. **Monitor Executions**: Use n8n's execution history
4. **Telegram Webhook**: Verify callback_query data structure

## 📊 Workflow Specifications

| Component | Details |
|-----------|---------|
| **Trigger Schedule** | Daily at 9:00 AM |
| **Email Filter** | `category:promotions` before Sept 1, 2025 |
| **User Timeout** | Immediate (no timeout on button response) |
| **Batch Processing** | All matching emails in single execution |
| **Confirmation** | Required via Telegram inline buttons |

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

- **Use at your own risk**: This tool permanently deletes emails
- **Test thoroughly**: Always test with a small subset first
- **Backup important emails**: Consider exporting important emails before running
- **Gmail limits apply**: Respect Gmail API quotas and rate limits

## 🆘 Support

- **Issues**: Report bugs via GitHub Issues
- **n8n Documentation**: [n8n.docs.com](https://docs.n8n.io/)
- **Gmail API**: [Google Gmail API Docs](https://developers.google.com/gmail/api)

## 📸 Screenshots

### Telegram Notification
The daily notification includes clear options:
- ✅ **Continue**: Proceed with email deletion
- ❌ **Cancel**: Skip cleanup for today

### n8n Workflow View
Clean, organized node structure with clear execution flow and error handling.

---

**Made with ❤️ for email productivity**

*Last updated: September 2025*