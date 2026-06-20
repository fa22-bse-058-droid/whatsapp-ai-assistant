
# 🤖 WhatsApp AI Assistant

**An AI-powered WhatsApp Business Assistant that handles customer support, lead capture, and appointment booking — 24/7, automatically.**

---

## 📌 What is This?

WhatsApp AI Assistant is a SaaS platform where any business can connect their WhatsApp number and get an AI employee that handles everything automatically — customer queries, lead capture, appointment booking, and follow-ups.

No more missed messages. No more manual replies at 2am.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🗣️ **Smart Customer Support** | AI replies instantly to customer queries using business's own knowledge base |
| 🎯 **Lead Capture** | Captures customer name, phone, and interest — saves to dashboard automatically |
| 📅 **Appointment Booking** | Checks availability, books slots, sends confirmation — all via WhatsApp |
| 🔔 **Auto Follow-ups** | Sends reminders before appointments and follows up with cold leads |
| 🏢 **Multi-Business Support** | Works for Salons, Clinics, Restaurants, Gyms, Real Estate, and more |
| 📊 **Business Dashboard** | View chats, manage leads, track appointments, edit FAQs — all in one place |

---

## 🏗️ System Architecture

```
Customer sends WhatsApp message
            ↓
   Meta Cloud API (Webhook)
            ↓
     FastAPI Backend
            ↓
    LangGraph AI Agent
     /       |       \
    FAQ    Lead    Booking
     \       |       /
   Qdrant Knowledge Base
   (RAG — business info)
            ↓
    Groq LLM generates reply
    (Llama 3.3 70B — free)
            ↓
  Customer receives response
  + Data saved to PostgreSQL
```

---

## 🛠️ Tech Stack

### Backend
- **FastAPI** — API server + WhatsApp webhook handler
- **PostgreSQL** — Main database
- **Redis (Upstash)** — Caching + task queues
- **Celery** — Background jobs (follow-ups, reminders)

### AI Layer
- **Groq API** — Llama 3.3 70B (fast, free tier)
- **LangChain** — AI orchestration
- **LangGraph** — Agent logic and decision flows
- **Qdrant Cloud** — Vector database for RAG

### Frontend
- **Next.js 14** — Dashboard + landing page
- **TypeScript** — Type safety
- **Tailwind CSS** — Styling
- **shadcn/ui** — UI components

### WhatsApp
- **Meta Cloud API** — Official WhatsApp Business API

### Deployment
- **Railway** — Backend + PostgreSQL + Redis
- **Vercel** — Frontend
- **Qdrant Cloud** — Vector DB (free 1GB cluster)

---

## 📁 Project Structure

```
whatsapp-ai-assistant/
│
├── backend/
│   ├── main.py                  # FastAPI entry point
│   ├── config.py                # Environment variables
│   ├── database.py              # DB connection
│   ├── requirements.txt
│   │
│   ├── api/
│   │   ├── webhook.py           # Receives WhatsApp messages
│   │   ├── auth.py              # Login / Register
│   │   ├── business.py          # Business management
│   │   ├── leads.py             # Lead management
│   │   └── appointments.py      # Booking management
│   │
│   ├── agent/
│   │   ├── graph.py             # LangGraph agent
│   │   ├── nodes.py             # Agent decision nodes
│   │   ├── tools.py             # FAQ, booking tools
│   │   └── prompts.py           # AI system prompts + templates
│   │
│   ├── rag/
│   │   ├── embedder.py          # Text to vectors
│   │   └── retriever.py         # Qdrant search
│   │
│   ├── models/
│   │   ├── business.py
│   │   ├── lead.py
│   │   ├── appointment.py
│   │   └── conversation.py
│   │
│   └── services/
│       ├── whatsapp.py          # Meta API — send messages
│       └── scheduler.py         # Follow-up scheduling
│
├── frontend/
│   ├── app/
│   │   ├── page.tsx             # Landing page
│   │   ├── auth/
│   │   │   ├── login/
│   │   │   └── register/
│   │   └── dashboard/
│   │       ├── page.tsx         # Overview
│   │       ├── chats/
│   │       ├── leads/
│   │       ├── appointments/
│   │       └── settings/
│   │
│   ├── components/
│   │   ├── ui/                  # shadcn components
│   │   ├── dashboard/
│   │   └── onboarding/
│   │
│   └── lib/
│       ├── api.ts
│       └── types.ts
│
└── .env.example
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+
- PostgreSQL
- Accounts: [Groq](https://console.groq.com) · [Qdrant Cloud](https://cloud.qdrant.io) · [Upstash](https://upstash.com) · [Meta Developers](https://developers.facebook.com)

### 1. Clone the repo

```bash
git clone https://github.com/your-username/whatsapp-ai-assistant.git
cd whatsapp-ai-assistant
```

### 2. Backend Setup

```bash
cd backend
python -m venv venv
venv\Scripts\activate        # Windows
pip install -r requirements.txt
```

### 3. Environment Variables

```bash
cp .env.example .env
# Fill in your API keys
```

```env
# WhatsApp
WHATSAPP_TOKEN=your_meta_access_token
WHATSAPP_VERIFY_TOKEN=your_custom_verify_token
PHONE_NUMBER_ID=your_phone_number_id

# Database
DATABASE_URL=postgresql://user:password@localhost/dbname

# AI
GROQ_API_KEY=your_groq_api_key

# Vector DB
QDRANT_URL=your_qdrant_cluster_url
QDRANT_API_KEY=your_qdrant_api_key

# Redis
REDIS_URL=your_upstash_redis_url

# Auth
SECRET_KEY=your_secret_key
```

### 4. Run Backend

```bash
uvicorn main:app --reload
```

### 5. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

### 6. Expose Webhook (Development)

```bash
# Install ngrok
ngrok http 8000

# Copy the public URL → paste in Meta Developer Console
# Webhook URL: https://your-ngrok-url/api/webhook
```

---

## 🏢 Supported Business Types

| Type | AI Name | Use Case |
|---|---|---|
| 💇 Salon / Parlor | Sara | Appointments, services, prices |
| 🏥 Clinic | Mariam | Doctor availability, OPD timings |
| 🍽️ Restaurant | Bilal | Menu, reservations, delivery |
| 💪 Gym | Usman | Memberships, timings, trainers |
| 🏠 Real Estate | Zara | Property info, site visits |
| ⚙️ Custom | You choose | Any business |

---

## 📊 Dashboard Preview

```
┌─────────────────────────────────────────┐
│  CodeEater AI Assistant — Dashboard     │
├──────────┬──────────────────────────────┤
│          │  Today's Overview            │
│ Chats    │  Leads: 12  Appointments: 5  │
│ Leads    │                              │
│ Bookings │  Recent Conversations        │
│ Settings │  ─────────────────────────  │
│          │  Ahmed → "Appointment?"  ✅  │
│          │  Sara  → "Prices?"       ✅  │
│          │  Usman → "Open Sunday?"  ✅  │
└──────────┴──────────────────────────────┘
```

---

## 🗺️ Roadmap

- [x] Project architecture
- [ ] FastAPI backend + auth
- [ ] WhatsApp webhook integration
- [ ] LangGraph AI agent
- [ ] RAG pipeline (Qdrant)
- [ ] Lead capture flow
- [ ] Appointment booking flow
- [ ] Follow-up scheduler
- [ ] Next.js dashboard
- [ ] Multi-business onboarding
- [ ] Railway + Vercel deployment

---





## 📄 License

MIT License — free to use, modify, and distribute.
