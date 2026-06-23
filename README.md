# BrandMirror — Multi-Company Feedback Intelligence Platform

BrandMirror is a comprehensive public feedback intelligence platform that aggregates, analyzes, and generates AI-powered insights from multiple data sources including Reddit, Twitter/X, and mobile app stores. The platform uses persistent vector memory to maintain historical context, enabling intelligent insights that reference and compare feedback across weeks and companies in real time.

## Core Features

**Memory-Augmented Intelligence**: Leverages Hindsight persistent vector memory to store weekly analyses. This enables Week 3 insights to reference Week 1 data with specific metrics, identifying trends like "Swiggy improving while Zomato declining" backed by quantitative evidence from past weeks.

**Multi-Source Data Collection**: Automatically scrapes public sentiment from:
- Reddit (posts and top comments from targeted subreddits)
- Twitter/X (tweets matching company-specific queries)
- Google Play Store (app reviews and ratings)
- Apple App Store (app reviews and ratings)

**Intelligent Analysis Engine**: For each weekly data collection:
- Sentiment analysis (positive/negative/neutral breakdown with sentiment scores)
- Theme extraction (identifies recurring topics and concerns)
- Data credibility validation (detects bot patterns, duplicates, and spam)
- Rich enrichment (categorization, urgency levels, emotion tagging)

**Competitive Intelligence**: Compare multiple companies side-by-side with:
- Cross-company sentiment correlation analysis
- Shared theme identification and divergence
- Ranking by sentiment trajectory
- Insight generation highlighting market differentiators

**Interactive Dashboard**: Real-time visualization with:
- Sentiment trend charts across weeks
- Feedback breakdown by source and category
- Theme card analysis with mention counts
- Memory log showing historical insights
- AI chatbot for natural language queries

**Dynamic Company Management**: Add or remove companies without code changes. The system auto-generates realistic mock feedback using industry-specific templates.

## Default Tracked Companies

The platform comes pre-configured to track these companies:

| Company | Industry | Data Sources |
|---------|----------|--------------|
| Zomato | Food Delivery | Google Play Store, App Store, Reddit, Twitter |
| Swiggy | Food Delivery | Google Play Store, App Store, Reddit, Twitter |
| BYJU'S | Edtech | Google Play Store, App Store, Reddit, Twitter |
| Ola | Ride-hailing | Google Play Store, App Store, Reddit, Twitter |
| PhonePe | Fintech | Google Play Store, App Store, Reddit, Twitter |

Additional companies can be added through the **Add Company** interface without modifying any configuration files.

## Architecture Overview

The platform consists of a full-stack architecture:

```
BrandMirror/
├── backend/                    # Python FastAPI backend
│   ├── main.py                # API endpoints and FastAPI app
│   ├── agent/                 # Intelligent analysis modules
│   │   ├── analyzer.py        # Sentiment & theme extraction (Groq LLM)
│   │   ├── comparator.py      # Cross-company analysis
│   │   ├── data_validator.py  # Credibility & bot detection
│   │   ├── hindsight_client.py # Memory persistence layer
│   │   └── insight_generator.py # AI insights with memory context
│   ├── scraper/               # Data collection modules
│   │   ├── appstore_scraper.py # App Store & Play Store scraper
│   │   ├── reddit_scraper.py   # Reddit post & comment scraper
│   │   └── twitter_scraper.py  # Twitter/X scraper
│   ├── companies.json         # Company configurations
│   ├── requirements.txt       # Python dependencies
│   └── memory_store.json      # Local memory fallback
│
├── frontend/                  # React + Vite frontend
│   ├── src/
│   │   ├── App.jsx           # Main app component
│   │   ├── components/        # React components
│   │   │   ├── Dashboard.jsx
│   │   │   ├── CompareView.jsx
│   │   │   ├── AIChatbot.jsx
│   │   │   ├── SentimentChart.jsx
│   │   │   ├── ThemeCards.jsx
│   │   │   ├── FeedbackBreakdown.jsx
│   │   │   └── ...
│   │   ├── index.css
│   │   └── main.jsx
│   ├── package.json
│   ├── vite.config.js
│   └── index.html
│
└── data/                      # Weekly feedback data
    ├── zomato/
    │   ├── week1_feedback.json
    │   ├── week2_feedback.json
    │   └── week3_feedback.json
    ├── swiggy/
    └── ...
```

## Installation & Setup

### Prerequisites

- Python 3.10 or later
- Node.js 18+ with npm
- Optional: API keys for enhanced data sources (see Configuration section)

### Backend Setup

1. Navigate to the backend directory and install dependencies:

```bash
cd antigravity/backend
pip install -r requirements.txt
```

2. Configure environment variables (optional for full functionality):

```bash
# Create .env file with your API keys
cp .env.example .env
```

See the Configuration section below for API key requirements.

### Frontend Setup

1. Navigate to the frontend directory and install dependencies:

```bash
cd antigravity/frontend
npm install
```

## Configuration

### API Keys & Credentials

The platform uses several optional API keys for enhanced data collection. Set them in a `.env` file in the `backend/` directory:

```
GROQ_API_KEY=your_groq_api_key_here
HINDSIGHT_API_KEY=your_hindsight_api_key_here
REDDIT_CLIENT_ID=your_reddit_client_id_here
REDDIT_CLIENT_SECRET=your_reddit_client_secret_here
REDDIT_USER_AGENT=AntiGravity/1.0
```

**Key Details**:
- **GROQ_API_KEY**: Required for AI-powered insight generation. Get from Groq console
- **HINDSIGHT_API_KEY**: Optional. If not provided, memory is stored locally as JSON
- **REDDIT_***: Optional. Required only for Reddit scraping beyond mock data
- **App Store/Play Store**: No credentials needed — scrapers use public APIs

### Company Configuration

Companies are stored in `backend/companies.json`. Each entry specifies:

```json
{
  "id": "company_identifier",
  "display_name": "Display Name",
  "industry": "Industry Category",
  "color": "#HEX_COLOR",
  "reddit_subreddits": ["r/subreddit1", "r/subreddit2"],
  "reddit_keywords": ["keyword1", "keyword2"],
  "twitter_queries": ["twitter query"],
  "google_play_id": "com.package.id",
  "app_store_id": "app_store_app_id",
  "app_store_name": "app_store_app_name",
  "country": "in"
}
```

Add companies through the UI or manually edit this file to configure scraping parameters.

## Running the Application

### Start the Backend Server

```bash
cd antigravity/backend
python -m uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at `http://localhost:8000`. API documentation is at `http://localhost:8000/docs`.

### Start the Frontend Development Server

In a new terminal:

```bash
cd antigravity/frontend
npm run dev
```

The application will be available at `http://localhost:5173`.

### Data Workflow

1. **Collect Data**: Run scrapers to fetch feedback from public sources

   ```bash
   # Scrape Google Play Store and App Store (no credentials needed)
   python scraper/appstore_scraper.py --company zomato
   
   # Scrape Reddit (requires REDDIT_CLIENT_ID and REDDIT_CLIENT_SECRET)
   python scraper/reddit_scraper.py --company zomato
   
   # Scrape Twitter/X (uses snscrape, no credentials needed)
   python scraper/twitter_scraper.py --company zomato
   ```

   Pre-generated mock data is included for demonstration purposes.

2. **Analyze Feedback**: Use the dashboard to analyze collected feedback

   - Select a company from the sidebar
   - Choose a week to analyze
   - The system automatically processes the data to extract themes and sentiment

3. **Store in Memory**: Analyses are automatically stored in persistent memory (Hindsight or local JSON fallback)

4. **Generate Insights**: The system uses Groq LLM with memory context to generate actionable insights

5. **Compare Companies**: Switch to the Compare view to analyze multiple companies side-by-side

## API Reference

### Core Endpoints

**Health Check**
```
GET /api/health
Returns: { status: "ok", companies_tracked: number }
```

**Company Management**
```
GET /api/companies
Returns: Array of company configurations

POST /api/companies
Body: { id, display_name, industry, color, reddit_*, twitter_*, google_play_id, app_store_id, country }
Returns: { message, companies }

DELETE /api/companies/{company_id}
Returns: { message, companies }
```

**Feedback Analysis**
```
POST /api/analyze
Body: {
  company_id: string,
  week: number,
  feedback: Array of FeedbackItem
}
Returns: {
  company_id, company_name, week, themes, sentiment, insight,
  memory_log, data_credibility, feedback
}
```

**Competitive Comparison**
```
POST /api/compare
Body: {
  company_ids: Array of strings,
  week: number
}
Returns: {
  companies, comparison_insight, sentiment_comparison,
  week, comparison_stats
}
```

**Memory Retrieval**
```
GET /api/memory/{company_id}
Returns: { company_id, memory: Array }

GET /api/memory
Returns: { memory: All stored analyses }
```

**Data Access**
```
GET /api/data/{company_id}/{week}
Returns: { company_id, week, count, feedback: Array }
```

**AI Chatbot**
```
POST /api/chat
Body: {
  message: string,
  conversation_history: Array of { role, content }
}
Returns: { response: string }
```

## Data Structure

### Feedback Item Schema

Each feedback item collected from sources includes:

```json
{
  "id": "unique_identifier",
  "company_id": "company_identifier",
  "source": "reddit|twitter|google_play|app_store",
  "platform_detail": "r/subreddit|twitter/x|google_play|app_store",
  "text": "feedback text",
  "author": "author_username",
  "timestamp": "ISO 8601 timestamp",
  "url": "source_url",
  "score": 0-100,
  "rating": 1-5,
  "type": "post|tweet|app_review",
  "language": "en",
  "engagement_score": 0-1,
  "reply_count": number,
  "share_count": number,
  "verified_author": boolean,
  "contains_media": boolean,
  "feedback_category": "delivery|payment|app_ux|customer_support|pricing|product_quality|general",
  "urgency_level": "low|medium|high",
  "emotion_tags": ["emotion1", "emotion2"],
  "resolution_requested": boolean,
  "mentioned_competitors": []
}
```

### Analysis Output Schema

Analysis results include:

```json
{
  "themes": [
    {
      "theme": "theme_name",
      "count": number,
      "sentiment": "positive|negative|neutral",
      "examples": ["quote1", "quote2"]
    }
  ],
  "sentiments": {
    "positive": percentage,
    "negative": percentage,
    "neutral": percentage,
    "score": -1 to 1
  },
  "insight": "AI-generated insight with memory context",
  "memory_log": [
    {
      "week": number,
      "themes": Array,
      "sentiments": Object,
      "raw_summary": "summary text"
    }
  ],
  "data_credibility": {
    "duplicate_ratio": 0-1,
    "flagged_items": Array,
    "credibility_score": 0-100
  }
}
```

## Frontend Components

The React frontend includes specialized components for different views:

- **Dashboard**: Main company view with sentiment trends, themes, feedback breakdown
- **CompareView**: Multi-company competitive analysis with shared metrics
- **AIChatbot**: Natural language interface for querying brand data
- **SentimentChart**: Line chart visualization of sentiment over weeks
- **ThemeCards**: Card-based display of identified feedback themes
- **FeedbackBreakdown**: Source and category breakdown visualizations
- **CompanySelector**: Sidebar navigation between companies
- **AddCompanyModal**: Dynamic company onboarding interface
- **MemoryLog**: Historical analysis timeline view
- **DataCredibilityBadge**: Data quality indicator

## Technologies Used

**Backend**:
- FastAPI: Modern async web framework
- Groq: AI LLM for insight generation
- Hindsight: Vector memory for persistent analysis storage
- PRAW: Reddit API wrapper
- snscrape: Twitter/X scraper
- google-play-scraper: Play Store review scraper
- app-store-scraper: App Store review scraper
- Pydantic: Data validation
- python-dotenv: Environment management

**Frontend**:
- React 19: UI framework
- Vite: Build tool and dev server
- Recharts: Chart visualization library
- Axios: HTTP client
- CSS: Styling

## Project Structure Details

### Backend Module Functions

**analyzer.py**: Core sentiment and theme extraction
- `calculate_sentiment()`: Computes positive/negative/neutral percentages and sentiment score
- `extract_themes()`: Uses Groq to identify recurring topics in feedback
- `enrich_feedback_items()`: Adds computed fields like categorization and emotion tags

**comparator.py**: Cross-company intelligence
- `generate_comparison()`: Creates comparative insights using Groq
- `compute_comparison_stats()`: Calculates ranking and trend metrics

**data_validator.py**: Quality assurance
- `validate_feedback_batch()`: Detects bot patterns, duplicates, spam
- `check_bot_patterns()`: Identifies suspicious feedback
- `check_duplicate_ratio()`: Measures data uniqueness

**hindsight_client.py**: Memory persistence
- `store_weekly_analysis()`: Saves to Hindsight or JSON fallback
- `get_company_memory_context()`: Retrieves full memory for a company
- `get_cross_company_memory_context()`: Retrieves cross-company comparisons

**insight_generator.py**: AI-powered insights
- `generate_insight()`: Creates memory-augmented weekly analysis using Groq
- Outputs structured insights with negative themes, solutions, and recommendations

**Scrapers**:
- `appstore_scraper.py`: Collects from Google Play Store and App Store simultaneously
- `reddit_scraper.py`: Targets specific subreddits and keywords
- `twitter_scraper.py`: Uses snscrape or tweepy for Twitter/X data

### Frontend Organization

Components are organized by functionality:
- **View Components**: Dashboard, CompareView (main pages)
- **UI Components**: Charts, Cards, Modals, Badges
- **Feature Components**: Chatbot, Company Selector, Upload
- **Helper Components**: Logo, Error states

Communication with backend via Axios using the API base URL configured for development proxy.

## Development Workflow

### Adding a New Feature

1. **Backend**: Add new endpoint in `main.py` or create new module in `agent/` directory
2. **Frontend**: Create React component in `src/components/`
3. **API Integration**: Use Axios to connect frontend to backend
4. **Styling**: Add CSS to component or `index.css`

### Extending Analysis

To add new analysis types:

1. Create new function in appropriate module (`agent/` directory)
2. Call from `/api/analyze` endpoint
3. Return results in analysis response
4. Create frontend component to display results

### Adding Data Sources

1. Create new scraper in `backend/scraper/`
2. Implement source-specific collection logic
3. Return data in standard FeedbackItem format
4. Add configuration to `companies.json` if needed

## Deployment

### Production Build

**Frontend**:
```bash
cd frontend
npm run build
```

This creates optimized static files in the `dist/` directory.

**Backend**:
```bash
# Use production ASGI server instead of development server
pip install gunicorn
gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app
```

### Environment Setup

Set required API keys in production environment:
```
GROQ_API_KEY
HINDSIGHT_API_KEY
REDDIT_CLIENT_ID
REDDIT_CLIENT_SECRET
```

### Docker Deployment

Create `Dockerfile` for backend and `frontend.Dockerfile` for frontend, then deploy using your preferred container orchestration platform.

## Troubleshooting

**No data loading**: Check that mock data exists in `data/` directory or run scrapers

**API errors**: Ensure backend is running on `http://localhost:8000` and check browser console for CORS issues

**Missing insights**: Verify GROQ_API_KEY is set; fallback insights will be generated without it

**Slow analysis**: First analysis takes longer as memory is populated; subsequent analyses use memory context for faster results

**Memory not persisting**: Check HINDSIGHT_API_KEY; system falls back to local `memory_store.json` without it

## Future Enhancements

Planned features for future releases:
- Real-time dashboard updates with WebSocket
- Multi-language support for international markets
- Advanced filtering and segmentation
- Custom report generation
- Email alerts for sentiment spikes
- Predictive trend analysis
- Integration with external BI tools

## Contributing

To contribute to BrandMirror:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is part of the BrandMirror initiative. See LICENSE file for details.

## Support & Contact

For issues, feature requests, or questions about the platform, please open an issue or contact the development team.

# Scrape Twitter (needs snscrape or TWITTER_BEARER_TOKEN in .env)
python scraper/twitter_scraper.py --company zomato

# Merge all sources and split into week1/2/3
python scraper/merge_feedback.py                    # all companies
python scraper/merge_feedback.py --company zomato   # one company
```

### 4. Start the Backend

```bash
cd antigravity/backend
uvicorn main:app --reload
# API runs at http://localhost:8000
# Docs at http://localhost:8000/docs
```

### 5. Start the Frontend

```bash
cd antigravity/frontend
npm install
npm run dev
# App runs at http://localhost:5173
```

---

## 🔑 API Keys

| Service | What for | Where to get |
|---------|----------|--------------|
| **Groq** | LLM (qwen3-32b) for theme extraction and insight generation | [groq.com](https://groq.com) — free tier available |
| **Hindsight** | Persistent vector memory | [ui.hindsight.vectorize.io](https://ui.hindsight.vectorize.io) — use promo code **MEMHACK6** for $50 free credits |
| **Reddit** | Scraping r/india, r/zomato, etc. | [reddit.com/prefs/apps](https://www.reddit.com/prefs/apps) — create a script app |
| **Twitter** (optional) | Fallback if snscrape fails | [developer.twitter.com](https://developer.twitter.com) — free tier |

---

## 🎬 Demo Flow (The Winning Story)

Follow this exact sequence to demonstrate cross-week memory:

```
1. Select Zomato in sidebar
   → Click "Load Week 1"
   → Insight generated: "Week 1 baseline — 60% negative, top concern: delivery delays"
   → Memory stored in Hindsight ✓

2. Select Swiggy in sidebar
   → Click "Load Week 1"
   → Swiggy baseline stored

3. Back to Zomato → Click "Load Week 2"
   → Insight NOW SAYS: "Compared to Week 1, Zomato's delivery complaint rate
     has increased by 14%..." ← THIS IS THE MEMORY DEMO

4. Back to Swiggy → Click "Load Week 2"
   → Swiggy insight shows improvement from Week 1

5. Zomato → Load Week 3
   → Insight says: "Over the 3-week tracking period, Zomato's delivery
     complaints increased from baseline... compared to Swiggy which improved..."

6. Swiggy → Load Week 3

7. Click "Compare Companies" button
   → Select Zomato + Swiggy
   → Click "Generate Comparison"
   → Competitive insight: "Since Week 1, Swiggy has improved its sentiment
     score from X to Y while Zomato has declined from A to B..."

8. Demo the Memory Log:
   → Collapse/expand the memory log panel
   → Toggle "This Company" vs "All Companies"
   → Show judges the actual Hindsight memory entries

9. Demo Add Company:
   → Click "Add Company" in sidebar
   → Fill in any company (e.g. Meesho, Flipkart)
   → It appears instantly in the sidebar
```

---

## 🏗️ Architecture

```
antigravity/
├── backend/
│   ├── main.py                    # FastAPI — 7 endpoints
│   ├── companies.json             # Company registry (add here = tracked everywhere)
│   ├── scraper/
│   │   ├── reddit_scraper.py      # PRAW — company-agnostic
│   │   ├── twitter_scraper.py     # snscrape + tweepy fallback
│   │   ├── appstore_scraper.py    # Play Store + App Store (no credentials)
│   │   └── merge_feedback.py      # Combine, clean, split → week1/2/3
│   ├── agent/
│   │   ├── hindsight_client.py    # Hindsight SDK wrapper (+ local JSON fallback)
│   │   ├── analyzer.py            # Groq: theme extraction + sentiment
│   │   ├── insight_generator.py   # Groq: memory-augmented weekly insights
│   │   └── comparator.py         # Groq: cross-company competitive insights
│   └── requirements.txt
├── frontend/
│   └── src/
│       ├── App.jsx                # Layout + state management
│       └── components/
│           ├── Dashboard.jsx      # Main company view
│           ├── CompanySelector.jsx# Sidebar company list
│           ├── FeedbackUpload.jsx # Week 1/2/3 loader
│           ├── SentimentChart.jsx # Recharts — single + multi-company
│           ├── ThemeCards.jsx     # Theme grid with trend arrows
│           ├── InsightPanel.jsx   # AI insight display
│           ├── MemoryLog.jsx      # Hindsight memory timeline
│           ├── CompareView.jsx    # Cross-company comparison
│           └── AddCompanyModal.jsx# Add company form
└── data/
    ├── zomato/                    # week1/2/3_feedback.json
    └── swiggy/                    # week1/2/3_feedback.json
```

---

## 🧪 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/health` | Health check |
| `GET` | `/api/companies` | List all tracked companies |
| `POST` | `/api/companies` | Add a new company |
| `POST` | `/api/analyze` | Analyze feedback + generate insight + store memory |
| `POST` | `/api/compare` | Cross-company comparison using memory |
| `GET` | `/api/memory/{company_id}` | Get memory for one company |
| `GET` | `/api/memory` | Get all memory across all companies |
| `GET` | `/api/data/{company_id}/{week}` | Serve week data JSON |

---

## 💡 How Memory Works

1. Every time you POST to `/api/analyze`, the analysis is stored in **Hindsight** under the namespace `antigravity::{company_id}`
2. Before generating each insight, the agent retrieves ALL past analyses for that company
3. The Groq prompt explicitly tells the LLM to **reference specific past week numbers**
4. For cross-company insights, the comparator reads memory from multiple namespaces simultaneously
5. If Hindsight is unavailable, a local JSON fallback (`backend/memory_store.json`) is used automatically

---

## 🛠️ Built With

- **FastAPI** — Python async API framework
- **Groq + qwen3-32b** — Fast, free LLM inference
- **Hindsight** — Persistent vector memory with namespace isolation
- **React + Vite** — Frontend framework
- **Recharts** — Sentiment trend charts
- **PRAW** — Reddit scraping
- **snscrape + tweepy** — Twitter scraping
- **google-play-scraper + app-store-scraper** — App review scraping
