<div align="center">

# 🎓 ScholarFinder

### AI-Powered Study Abroad Platform

*Find scholarships, universities, and opportunities worldwide — matched to your profile, for free.*

**[🌐 Live Site](https://scholarfinder.pythonanywhere.com)** · **[📱 Telegram Bot](https://t.me/ScholarFinder_bot)**

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0-green?logo=flask&logoColor=white)
![AI](https://img.shields.io/badge/AI-LLaMA_3.3_70B-purple?logo=meta&logoColor=white)
![Scholarships](https://img.shields.io/badge/Scholarships-402+-orange)
![Universities](https://img.shields.io/badge/Universities-207+-blue)
![Opportunities](https://img.shields.io/badge/Opportunities-195+-green)

---

</div>

## 🚀 What Is ScholarFinder?

ScholarFinder is a free platform that helps students find and apply for scholarships, universities, and opportunities worldwide. It combines a comprehensive database with AI-powered tools to make the study abroad journey easier.

Built entirely by **Scott Antwi** (17, Ghana 🇬🇭) under **Alpha Global Minds**.

## ✨ Features

### 📊 Comprehensive Database
| Category | Count | Details |
|----------|-------|---------|
| 🎯 Scholarships | 402+ | Searchable by country, field, level, funding type |
| 🏫 Universities | 207+ | Rankings, tuition info, financial aid notes |
| 🚀 Opportunities | 195+ | Internships, fellowships, competitions, research programs |
| 💰 Cost of Living | 85 cities | Monthly breakdown: rent, food, transport, entertainment |
| 🛂 Visa Guides | 49 countries | Documents, costs, embassy links, processing tips |
| 📝 Test Prep | 6 tests | IELTS, TOEFL, SAT, GRE, GMAT, Duolingo |
| ❓ FAQ | 42 entries | Common questions about studying abroad |

### 🤖 AI-Powered Tools
- **Essay Rater** — Paste your personal statement, SOP, or motivation letter. Get a 1-10 score with specific, actionable feedback on hook, narrative, voice, and impact. Strict grading — most essays score 4-6.
- **Resume Reviewer** — Upload or paste your resume. AI analyzes structure, action verbs, quantified achievements, and ATS-friendliness.
- **School Matcher** — Enter your GPA, country preference, and field. Get matched with universities with High/Medium/Reach ratings.
- **AI Recommendations** — Personalized scholarship and university matches based on your profile, powered by LLaMA 3.3 70B. Addresses you by first name.

### 🧠 6 AI Agents
| Agent | Role |
|-------|------|
| 🔍 **Scout** | Scholarship Finder — matches scholarships to your profile with scores |
| 📝 **Writer** | Essay Assistant — helps draft and improve scholarship essays |
| 👤 **Profiler** | Student Matcher — builds your profile and calculates eligibility |
| 📅 **Tracker** | Deadline Manager — color-coded deadline timeline |
| 🎯 **Advisor** | Strategy Coach — recommends which scholarships to prioritize |
| 🎤 **Prep** | Interview Coach — practice questions with AI feedback |

### 🔐 Production Security
- PBKDF2-HMAC-SHA256 password hashing (100K iterations, per-user salt)
- CSRF token protection on all forms
- Email verification with 6-digit OTP (1-minute expiry)
- SQLite-backed rate limiting (survives restarts)
- Secure session cookies (HttpOnly, Secure, SameSite)
- Google OAuth 2.0 integration

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│                  Frontend                     │
│     Jinja2 Templates + Vanilla JavaScript     │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│              Flask Backend                    │
│                                               │
│  ┌─────────┐ ┌──────────┐ ┌──────────────┐  │
│  │  Routes  │ │ AI Proxy │ │  Webhooks    │  │
│  │  (auth,  │ │ (Groq    │ │  (scholarship│  │
│  │  pages,  │ │  12-key  │ │  management) │  │
│  │  API)    │ │  rotate) │ │              │  │
│  └────┬─────┘ └────┬─────┘ └──────┬───────┘  │
│       │             │              │           │
│  ┌────▼─────────────▼──────────────▼───────┐  │
│  │           Data Layer                     │  │
│  │  SQLite (users, bookmarks, logs, rates) │  │
│  │  JSON files (scholarships, unis, etc.)  │  │
│  │  In-memory cache (mtime-based refresh)  │  │
│  └──────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│          Daily Scraper (GitHub Actions)       │
│                                               │
│  40+ sources → BeautifulSoup → AI extraction │
│  → deduplication → expiry cleanup → webhook  │
└──────────────────────────────────────────────┘
```

## 🔧 Technical Highlights

### AI Key Rotation System
12 Groq API keys with intelligent rotation:
- Round-robin selection across all keys
- Reads `retry-after` header on 429 responses
- Marks keys as "dead" with exact recovery timestamps
- Immediately tries next key on rate limit
- 30-second total retry budget with sleep-until-soonest-recovery
- Zero downtime for users even under heavy AI usage

### Smart Scholarship Matching
Point-based scoring algorithm:
- Country match: +30 pts
- Field of study match: +25 pts  
- Education level match: +20 pts
- Interest keywords: +10 pts each
- Fully funded bonus: +5 pts

Plus AI-powered deep matching via LLaMA 3.3 70B for personalized recommendations with natural language reasons.

### Automated Web Scraper
- 40+ scholarship sources (Africa-focused + international)
- BeautifulSoup HTML parsing with configurable CSS selectors
- AI-powered data extraction (webpage text → structured JSON)
- Automatic deduplication (MD5 hash of name + university)
- Deadline parsing (10+ date formats) with 7-day grace period
- Expired scholarships archived, not deleted

### Field of Study Validation
- 150+ recognized fields across all disciplines
- Alias support (CS → Computer Science, AI → Artificial Intelligence)
- Fuzzy matching (Marine Biology → Biology)
- Gibberish detection for nonsense inputs
- Graceful fallback to general recommendations

### Caching Layer
- File modification time (mtime) based cache
- Instant re-reads when data changes, cached otherwise
- Auto-invalidation when webhooks write new data
- Zero configuration, zero stale data

### Persistent Rate Limiting
- SQLite-backed (survives app restarts)
- 5 login attempts per IP per 5 minutes
- 3 verification resends per email per 5 minutes
- Auto-cleanup of expired entries
- Fails open on database errors

## 📊 Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.10, Flask 3.0 |
| Database | SQLite (WAL mode) |
| AI | Groq API, LLaMA 3.3 70B Versatile |
| Frontend | Jinja2, Vanilla JS, CSS Custom Properties |
| Hosting | PythonAnywhere |
| Scraping | BeautifulSoup4, GitHub Actions |
| Auth | PBKDF2, Google OAuth 2.0, Email OTP |

## 🧪 Testing

Automated test suite with 50+ tests covering:
- Field validation (gibberish detection, aliases, fuzzy matching)
- Password security (unique salts, hash verification)
- Data integrity (all JSON files loaded, required fields present)
- Caching behavior (cache hits, invalidation)
- API endpoints (all routes, correct response formats)
- Authentication flows (protected routes, login required)
- Chatbot responses (keyword routing, edge cases)
- Search and pagination

## 🎯 Other Projects

| Project | Description | Tech |
|---------|-------------|------|
| **MNIST Neural Network** | From-scratch NN — 96% test, 99.685% Kaggle | Pure NumPy (no frameworks) |
| **Wally** | ESP32 voice assistant with speech recognition | C++, ESP32, TTS/STT |
| **VEX Line Follower** | Autonomous robot with PID control | C++, VEX Robotics |
| **Grid Pathfinder** | BFS/DFS/A* algorithm visualizer | Python |

## 📬 Contact

- 🌐 **Website:** [scholarfinder.pythonanywhere.com](https://scholarfinder.pythonanywhere.com)
- 📧 **Email:** scottantwi930@gmail.com
- 💬 **WhatsApp:** +233549545063
- 🐙 **GitHub:** [@ScottT2-spec](https://github.com/ScottT2-spec)
- 📸 **Instagram:** [@bb_scott1](https://instagram.com/bb_scott1)

---

<div align="center">

**Built with 💚 in Ghana 🇬🇭 by Scott Antwi | Alpha Global Minds**

*17 years old. 2,800+ lines of production code. 400+ scholarships helping students worldwide.*

</div>
