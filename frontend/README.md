# 💻 Krishi Mitra — Frontend Single Page Application (SPA)

This directory contains the complete React 18 + Vite frontend application for Krishi Mitra.

---

## 📁 Directory Structure

```text
frontend/
├── src/
│   ├── main.jsx                 # React root mounting
│   ├── App.jsx                  # Main router & layout providers
│   ├── App.css                  # Design tokens, color system & grid utilities
│   ├── index.css                # Base reset & typography
│   ├── components/              # Reusable UI components
│   ├── context/                 # AuthContext & LanguageContext
│   ├── data/                    # Static databases (schemes.js)
│   ├── hooks/                   # useDiseaseScan, useMarketPrices, useWeather
│   ├── pages/                   # Primary page views (Chat, Crop, Market, Weather, Schemes)
│   ├── services/                # API service wrappers (FastAPI endpoints)
│   ├── styles/                  # Component styles
│   └── utils/                   # Helper utilities
├── index.html                   # HTML entry point
├── vite.config.js               # Vite bundler configuration
├── package.json                 # Dependencies & scripts
└── eslint.config.js             # ESLint configuration
```

---

## 🚀 Running the Frontend

### 1. Install Dependencies
```bash
cd frontend
npm install
```

### 2. Run Development Server
```bash
npm run dev
```
The app will run at: `http://localhost:5173/`

### 3. Production Build
```bash
npm run build
```
Outputs static bundle to `frontend/dist/`.
