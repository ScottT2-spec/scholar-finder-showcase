# ScholarFinder

Free platform that helps students find scholarships, universities, and study abroad opportunities. Built with Flask, SQLite, and Groq (LLaMA 3.3 70B).

**Live:** [scholarfinder.pythonanywhere.com](https://scholarfinder.pythonanywhere.com)  
**Telegram bot:** [@ScholarFinder_bot](https://t.me/ScholarFinder_bot)

## what it does

- search 400+ scholarships by country, field, level, or just type what you're looking for
- browse 200+ universities with rankings, tuition, and financial aid info
- 195+ opportunities (internships, fellowships, research programs, competitions)
- cost of living data for 85 cities worldwide
- visa guides for 49 countries
- test prep for IELTS, TOEFL, SAT, GRE, GMAT, Duolingo

## ai tools

6 AI agents built on LLaMA 3.3 70B through Groq:

- **scout** — finds scholarships that match your profile and scores them
- **writer** — helps draft and improve scholarship essays
- **profiler** — builds your student profile and checks eligibility
- **tracker** — manages deadlines with color-coded timeline
- **advisor** — recommends which scholarships to prioritize
- **prep** — mock interview questions with feedback

plus an essay rater (strict 1-10 scoring, most get 4-6), resume reviewer, and school matcher.

## how matching works

scholarships get scored based on how well they fit you:
- country match: 30 pts
- field of study: 25 pts
- education level: 20 pts
- interest keywords: 10 pts each
- fully funded bonus: 5 pts

also has AI-powered deep matching that gives personalized recommendations with reasons.

## some technical stuff

- 12 Groq API keys with round-robin rotation (auto-skips rate-limited keys)
- 40+ scholarship sources scraped daily via GitHub Actions
- AI extracts structured data from raw HTML
- mtime-based caching (re-reads files only when they change on disk)
- SQLite-backed rate limiting that survives restarts
- 150+ recognized fields of study with alias support and fuzzy matching
- PBKDF2 password hashing, CSRF protection, email OTP verification
- 50+ automated tests

## stack

python 3.10, flask, sqlite, jinja2, vanilla js, beautifulsoup, groq api, pythonanywhere, github actions

## contact

email: scottantwi930@gmail.com  
whatsapp: +233549545063  
instagram: [@bb_scott1](https://instagram.com/bb_scott1)
