# SICK7

# 🤝 Social Sync

**Social Sync** is a comprehensive, full-stack event aggregation platform designed to centralize social gatherings from the fragmented web.

By leveraging **Python** for backend automation and **AI-driven Retrieval-Augmented Generation (RAG)**, Social Sync goes beyond simple keyword matching. It understands the *context* of events, allowing users to find "chill jazz nights" or "high-energy tech meetups" even if those exact words aren't in the title. The frontend is built with **React** and **Tailwind CSS** for a responsive, modern user experience.

## 💡 The story behind

Moving to a new city to start university is overwhelming. For many first-year students, the hardest part isn't the coursework—it's figuring out where you belong. You want to make friends and get involved, but it’s hard to know what’s happening around you when you don't know where to look. Most event sites are cluttered or hard to search, leaving you scrolling endlessly instead of actually going out.

**Social Sync** was built to fix that specific disconnect. The goal wasn't just to aggregate data, but to make it easier for newcomers to find their crowd. By using AI, we moved away from rigid keyword searches—because when you're new in town, you don't always know the "right" words to search for, you just know the vibe you want. This project is about removing the friction between feeling lonely and finding a community.

## 🚀 Key Features

- **Automated Data Pipeline:** Intelligent web scrapers (`scrape.py`) autonomously visit target websites to extract event metadata (Title, Date, Location, Description).
- **AI-Powered Search (RAG):** Uses Vector Embeddings to allow natural language queries.
- **Local Persistence:** Lightweight SQLite database (`events.db`) ensures fast retrieval without heavy database overhead.
- **Secure (Simple) Auth:** JSON-based user allow-listing for controlled access.
- **Modern UI:** A clean, responsive dashboard built with React and styled with Tailwind CSS.

---

## 🛠️ Tech Stack & Architecture

### Backend (Logic & Data)
- **Language:** Python 3.9+
- **API Framework:** Flask / FastAPI (implied via `main.py`)
- **Scraping:** BeautifulSoup4 / Selenium
- **AI Logic:** Custom RAG implementation (Vector Similarity Search)
- **Database:** SQLite (`events.db`)
- **Auth:** File-based store (`users.json`)

### Frontend (User Interface)
- **Library:** React.js (v18+)
- **Styling:** Tailwind CSS
- **State Management:** React Hooks
- **HTTP Client:** Axios / Fetch API

---

## 📂 Repository Structure

The project follows a monorepo-style structure containing both the API and Client logic.

```text
/
├── SocialSync/                       #  BACKEND ROOT
│   ├── main.py                 # API Entry Point (starts the server)
│   ├── scrape.py               # Bot: Fetches raw HTML from target URLs
│   ├── ingest.py               # ETL: Cleans data & saves to SQLite
│   ├── rag_logic.py            # AI: Vector search & context retrieval
│   ├── events.db               # Database: Stores structured event data
│   ├── users.json              # Config: User credentials/allow-list
│   ├── .env                    # Secrets: API keys & config variables
│   │
│   └── social_sync/            #  FRONTEND ROOT
│       ├── package.json        # NPM dependencies
│       ├── tailwind.config.js  # CSS framework configuration
│       ├── public/             # Static assets (favicons, index.html)
│       └── src/                # React Source Code
│           ├── index.js        # React Entry point
│           ├── App.js          # Main Application View
│           ├── context/        # Global State (Auth, Events)
│           └── components/     # UI Modules
│               ├── AuthModal.js    # Login overlay
│               ├── EventCard.js    # Individual event display
│               └── Sidebar.js      # Navigation & Filtering
```

---

## ⚙️ Installation & Setup Guide

### Prerequisites
- **Python** (3.8 or higher)
- **Node.js** (v16 or higher) & **npm**

### 1. Backend Setup

1. **Navigate to the backend directory:**
   ```bash
   cd SocialSync
   ```

2. **Create a virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Python dependencies:**
   ```bash
   pip install -r requirements.txt
   # If requirements.txt is missing, install manually:
   # pip install flask requests beautifulsoup4 pandas openai
   ```

4. **Configuration:**
   Create a `.env` file in the `send/` directory if your `rag_logic.py` requires API keys (e.g., OpenAI API Key).
   ```env
   OPENAI_API_KEY=your_key_here
   PORT=5000
   ```

5. **Initialize Data:**
   Run the scraper and ingestion scripts to populate the database.
   ```bash
   python scrape.py
   python ingest.py
   ```

6. **Start the API Server:**
   ```bash
   python main.py
   ```

### 2. Frontend Setup

1. **Open a new terminal and navigate to the frontend directory:**
   ```bash
   cd send/social_sync
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the React Development Server:**
   ```bash
   npm start
   ```
   *The app should now be running at `http://localhost:3000`.*

---

## 🔄 System Workflows

### The Data Ingestion Pipeline (Backend)
1. **Trigger:** `scrape.py` is executed.
2. **Extraction:** The script iterates through a list of target URLs, downloading raw HTML.
3. **Processing:** `ingest.py` receives the raw data.
   - *Cleaning:* Strips HTML tags, standardizes dates (ISO 8601).
   - *Vectorization:* Converts event descriptions into vector embeddings for AI search.
4. **Storage:** Cleaned records are committed to `events.db`.

### The RAG Search Process
1. **Query:** User types "Networking events for developers" in the React frontend.
2. **Request:** Frontend sends this string to `main.py`.
3. **Logic:** `rag_logic.py` converts the query into a vector.
4. **Retrieval:** The system calculates cosine similarity between the query vector and stored event vectors in `events.db`.
5. **Response:** The most contextually relevant events are returned to the user, even if they don't explicitly contain the word "networking."

---

## 📡 API Endpoints (Overview)

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| **GET** | `/api/events` | Retrieve a list of all upcoming events. |
| **POST** | `/api/search` | Send a natural language query for RAG processing. |
| **POST** | `/api/login` | Authenticate user against `users.json`. |
| **GET** | `/api/health` | Check if the scraper/database is active. |

---

## 🧪 Troubleshooting

- **Database Locked:** If `ingest.py` fails, ensure `main.py` isn't holding a lock on `events.db`. Stop the server before running heavy ingestion.
- **CORS Errors:** If the Frontend cannot talk to the Backend, ensure `main.py` includes CORS headers (e.g., `flask-cors`).
- **Empty Results:** Run `python ingest.py` to ensure the database is actually populated.


BESTEMv14
