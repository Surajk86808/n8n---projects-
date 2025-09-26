# 🤖 AI Agent Apollo Scraper with Telegram Bot

An intelligent n8n workflow that transforms natural language requests sent via Telegram into automated lead scraping operations on Apollo.io. This AI-powered system can understand voice or text messages, intelligently construct Apollo search URLs, and deliver structured lead data directly to Google Sheets.

---

## ✨ Key Features

* **Multi-Modal Input**: Accepts both text messages and voice notes via Telegram
* **Voice Transcription**: Automatically converts voice messages to text using OpenAI Whisper
* **AI-Powered Intelligence**: Uses GPT-4o-mini to understand natural language requests and construct precise Apollo.io search URLs
* **Automated Lead Scraping**: Integrates with Apify's Apollo.io scraper for bulk lead extraction
* **Smart Data Management**: Automatically organizes scraped leads into Google Sheets with proper formatting
* **Industry Intelligence**: References industry ID mappings for accurate targeting

---

## 🛠️ Workflow Architecture

### 1. **Telegram Bot Interface**
* **Node:** `Telegram Trigger`
* **Function:** Listens for incoming messages (text or voice) from your Telegram bot
* **Features:** Handles both text and audio input seamlessly

### 2. **Input Processing**
* **Nodes:** `Audio or Text Message?` → `Get File` → `Transcribe message` / `Set Message`
* **Function:** 
  - Detects message type (voice vs text)
  - For voice: Downloads audio file and transcribes using OpenAI Whisper
  - For text: Extracts message content directly
  - Merges processed input for AI analysis

### 3. **AI Agent Brain**
* **Nodes:** `AI Agent1` + `OpenAI Chat Model`
* **Function:** 
  - Analyzes natural language requests
  - Understands lead scraping requirements
  - Constructs Apollo.io search URLs intelligently
  - Determines scraping parameters (number of leads, search criteria)

### 4. **Smart Tools Integration**
* **Google Sheets Tool:** References industry ID mappings to construct accurate search URLs
* **HTTP Request Tool:** Triggers Apify Apollo.io scraper with constructed parameters

### 5. **Data Collection & Storage**
* **Nodes:** `Gets latest actor run` → `Gets the data scraped` → `Puts data into Google Sheet`
* **Function:**
  - Retrieves scraping results from Apify
  - Formats data with comprehensive lead information
  - Stores in organized Google Sheets format

---

## 📊 Data Fields Captured

The workflow captures comprehensive lead information:

- **Personal Info:** First name, last name, title, email, LinkedIn headline
- **Company Details:** Organization name, website, LinkedIn URL, founding year
- **Geographic Data:** Country information
- **Professional Context:** Job titles, company roles

---

## 🚀 Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Telegram Bot Token (create via @BotFather)
- OpenAI API key
- Apify account with Apollo.io scraper access
- Google account for Sheets integration

### 1. Telegram Bot Setup
1. Message @BotFather on Telegram
2. Create new bot with `/newbot` command
3. Save the bot token for n8n configuration

### 2. Configure Credentials in n8n
- **Telegram:** Add bot token to Telegram trigger node
- **OpenAI:** Configure API key for transcription and AI agent
- **Google Sheets:** Set up OAuth2 authentication
- **Apify:** Add API token for scraper access

### 3. Google Sheets Preparation
Create two Google Sheets:
1. **Industry ID Sheet:** Maps industry names to Apollo.io industry IDs
2. **Leads Output Sheet:** Stores scraped lead data with proper column headers

### 4. Apify Integration
- The workflow uses the `code_crafter~apollo-io-scraper` actor
- Ensure you have access to this scraper in your Apify account
- Configure API token in the HTTP request nodes

---

## 💬 Usage Examples

Send these messages to your Telegram bot:

**Text Examples:**
- "Find 50 CEOs in the fintech industry"
- "Scrape 100 marketing managers from SaaS companies"
- "Get leads for VP of Sales in healthcare startups"

**Voice Examples:**
- Record a voice note: "I need contact information for founders in the AI space, about 25 leads"
- Voice message: "Can you find me decision makers in e-commerce companies?"

---

## 🔄 Workflow Flow

```
Telegram Message (Text/Voice)
    ↓
Input Processing & Transcription
    ↓
AI Agent Analysis
    ↓
Apollo.io URL Construction
    ↓
Apify Scraper Execution
    ↓
Data Retrieval & Formatting
    ↓
Google Sheets Storage
```

---

## 🛡️ Security & Best Practices

- **API Keys:** Store all credentials securely in n8n credential manager
- **Rate Limits:** Respect Apollo.io and Apify rate limitations
- **Data Privacy:** Ensure compliance with data protection regulations
- **Access Control:** Limit Telegram bot access to authorized users

---

## 🚨 Important Notes

- The AI Agent prompt is intentionally left as "homework" - customize it for your specific use case
- Industry ID mapping sheet must be populated with accurate Apollo.io industry identifiers
- Apify scraper may have usage limits based on your subscription plan
- Voice transcription uses OpenAI Whisper API (charges apply)

---

## 🔧 Customization Options

### Modify Scraping Parameters
Adjust the JSON body in the "Start Apify Scraper" node:
```json
{
    "getPersonalEmails": true,
    "getWorkEmails": true,
    "totalRecords": {amountOfLeadsScrapedGoesHere},
    "url": "{requestUrlGoeHere}"
}
```

### Extend Data Fields
Add more columns to the Google Sheets output by modifying the mapping in the final node.

### Enhanced AI Prompting
Improve the AI Agent's understanding by crafting detailed system prompts and examples.

---

## 📈 Benefits

- **Efficiency:** Convert natural language to structured lead data in minutes
- **Scalability:** Handle bulk lead generation requests automatically  
- **Accessibility:** Voice input makes it easy to request leads on-the-go
- **Integration:** Seamlessly fits into existing sales workflows via Google Sheets
- **Intelligence:** AI understands context and constructs optimal search parameters

---

## 🤝 Contributing

This workflow can be extended with:
- Multiple data sources beyond Apollo.io
- Enhanced AI prompting for better accuracy
- Additional output formats (CRM integration, CSV export)
- Real-time lead scoring and enrichment

---

## 📝 License

This workflow is provided as-is for educational and business purposes. Ensure compliance with Apollo.io, Telegram, and other service providers' terms of service.
