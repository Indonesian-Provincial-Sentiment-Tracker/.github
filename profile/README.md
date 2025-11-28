# 🇮🇩 Indonesian Provincial Sentiment Tracker

<div align="center">

![Organization Banner](https://img.shields.io/badge/Indonesian-Sentiment_Tracker-red?style=for-the-badge&logo=twitter&logoColor=white)

**Real-time sentiment analysis platform for Indonesian provincial tweets**

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)](https://golang.org)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org)
[![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)](https://rabbitmq.com)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white)](https://supabase.com)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docker.com)

</div>

---

## 📖 About

**Indonesian Provincial Sentiment Tracker** is a comprehensive microservices-based platform designed to crawl, analyze, and visualize public sentiment from Indonesian tweets across all 38 provinces. The system leverages cutting-edge AI/ML models including **IndoBERT** for sentiment classification and advanced topic modeling techniques.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              SYSTEM ARCHITECTURE                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

                                    ┌──────────────┐
                                    │   Frontend   │
                                    │ (React + TS) │
                                    └──────┬───────┘
                                           │ HTTP/SSE
                                           ▼
                              ┌────────────────────────┐
                              │   Backend (Golang)     │
                              │   - Fiber Framework    │
                              │   - ETag Caching       │
                              │   - SSE Notifications  │
                              │   - CORS Best Practice │
                              └────────────┬───────────┘
                                           │
                    ┌──────────────────────┼──────────────────────┐
                    │                      │                      │
                    ▼                      ▼                      ▼
        ┌───────────────────┐  ┌───────────────────┐  ┌───────────────────┐
        │  API Service      │  │  Server           │  │  Supabase         │
        │  (FastAPI/Python) │  │  (Golang)         │  │  (PostgreSQL)     │
        └───────────────────┘  └───────────────────┘  └─────────┬─────────┘
                                                                │
                    ┌───────────────────────────────────────────┤
                    │                                           │
                    ▼                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                               MESSAGE QUEUE                                      │
│                              (RabbitMQ)                                          │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐              │
│  │ Tweet Exchange  │───▶│ Sentiment Queue │    │  Topic Queue    │              │
│  └────────┬────────┘    └────────┬────────┘    └────────┬────────┘              │
└───────────┼──────────────────────┼──────────────────────┼───────────────────────┘
            │                      │                      │
            │              ┌───────┴───────┐      ┌───────┴───────┐
            │              ▼               │      ▼               │
            │   ┌───────────────────┐      │   ┌───────────────────┐
            │   │ Sentiment Service │      │   │ Topic Modelling   │
            │   │ (IndoBERT)        │      │   │ Service           │
            │   └─────────┬─────────┘      │   └─────────┬─────────┘
            │             │                │             │
            │             ▼                │             ▼
            │   ┌───────────────────┐      │   ┌───────────────────┐
            │   │ Topic Classifier  │──────│──▶│ Update Database   │
            │   │ Transformer       │      │   │                   │
            │   └───────────────────┘      │   └───────────────────┘
            │                              │
            ▼                              │
┌───────────────────────────┐              │
│ Tweet Indonesia Crawler   │◀─────────────┘
│ (Data Collection)         │
└───────────────────────────┘
```

---

## 📦 Repositories

| Repository | Description | Tech Stack |
|------------|-------------|------------|
| 🎨 **[frontend](https://github.com/Indonesian-Provincial-Sentiment-Tracker/frontend)** | Interactive dashboard for sentiment visualization with real-time updates | `TypeScript` `React` `Vite` `TailwindCSS` |
| ⚡ **[Backend](https://github.com/Indonesian-Provincial-Sentiment-Tracker/Backend)** | High-performance REST API with ETag caching and SSE notifications | `Go` `Fiber` `PostgreSQL` |
| 🐦 **[TweetIndonesianStateCrawler](https://github.com/Indonesian-Provincial-Sentiment-Tracker/TweetIndonesianStateCrawler)** | Tweet crawler service for Indonesian provincial data collection | `Python` `Supabase` `RabbitMQ` |
| 🧠 **[TopicModelling](https://github.com/Indonesian-Provincial-Sentiment-Tracker/TopicModelling)** | Topic modeling consumer for tweet categorization and analysis | `Python` `RabbitMQ` `NLP` |
| 💬 **[SentimentService](https://github.com/Indonesian-Provincial-Sentiment-Tracker/SentimentService)** | Real-time sentiment analysis using IndoBERT transformer model | `Python` `IndoBERT` `RabbitMQ` `Docker` |
| 🔬 **[TopicClassificationTransformer](https://github.com/Indonesian-Provincial-Sentiment-Tracker/TopicClassificationTransformer)** | Advanced topic classification using transformer models | `Python` `Transformers` `PyTorch` |
| 🔌 **[API](https://github.com/Indonesian-Provincial-Sentiment-Tracker/API)** | FastAPI service for data aggregation and analytics | `Python` `FastAPI` `Supabase` |

---

## 🔄 Data Flow

```
1️⃣ CRAWLING
   TweetIndonesianStateCrawler → Collects tweets from Indonesian provinces
                              → Stores raw data in Supabase PostgreSQL
                              → Publishes messages to RabbitMQ

2️⃣ PROCESSING
   RabbitMQ → Distributes messages to consumers
           ├── SentimentService (IndoBERT analysis)
           └── TopicModelling (Topic extraction)

3️⃣ ENRICHMENT
   AI Services → Process and classify tweets
              → Update database with sentiment scores
              → Categorize by topics and provinces

4️⃣ SERVING
   Backend (Golang) → Fetches aggregated data from Supabase
                   → Applies ETag caching for performance
                   → Sends SSE notifications for real-time updates
                   → Serves data to Frontend

5️⃣ VISUALIZATION
   Frontend → Displays interactive sentiment maps
           → Shows provincial comparisons
           → Real-time dashboard updates via SSE
```

---

## ✨ Key Features

### 🎯 Real-time Sentiment Analysis
- **IndoBERT-powered** sentiment classification optimized for Indonesian language
- Supports **Positive**, **Negative**, and **Neutral** sentiment detection
- Province-level sentiment aggregation

### 🗺️ Provincial Coverage
- All **38 Indonesian provinces** supported
- Geographic sentiment mapping and visualization
- Regional trend comparison

### ⚡ High Performance
- **ETag caching** for optimized API responses
- **SSE (Server-Sent Events)** for real-time notifications
- **RabbitMQ** message queue for scalable processing
- **Docker-ready** deployment

### 📊 Analytics Dashboard
- Daily, weekly, and monthly sentiment trends
- Topic modeling and categorization
- Interactive data visualization

---

## 🛠️ Tech Stack

<table>
<tr>
<td align="center"><strong>Frontend</strong></td>
<td align="center"><strong>Backend</strong></td>
<td align="center"><strong>AI/ML</strong></td>
<td align="center"><strong>Infrastructure</strong></td>
</tr>
<tr>
<td>
  
- React 18
- TypeScript
- Vite
- TailwindCSS
- Leaflet Maps

</td>
<td>

- Go (Fiber)
- FastAPI
- PostgreSQL
- Supabase

</td>
<td>

- IndoBERT
- Transformers
- Topic Modeling
- PyTorch

</td>
<td>

- Docker
- RabbitMQ
- Nginx
- SSE

</td>
</tr>
</table>


## 📈 System Requirements

| Service | Min RAM | Min CPU | Storage |
|---------|---------|---------|---------|
| Frontend | 512MB | 1 Core | 1GB |
| Backend | 512MB | 1 Core | 1GB |
| Sentiment Service | 4GB | 2 Core | 10GB |
| Topic Modelling | 2GB | 2 Core | 5GB |
| Crawler | 1GB | 1 Core | 2GB |
| RabbitMQ | 1GB | 1 Core | 5GB |

---

## 🤝 Contributing

We welcome contributions! Please read our contributing guidelines in each repository before submitting pull requests.

---

<div align="center">

**Made with ❤️ for Indonesian Sentiment Analysis**

*Tracking public sentiment across 38 Indonesian provinces in real-time*

</div>
