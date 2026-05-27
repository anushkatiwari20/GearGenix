# GearGenix — College Equipment Booking Bot

GearGenix is an AI-powered assistant that lets college clubs and departments
book shared equipment — projectors, microphones, speakers, laptops, and more —
through **natural conversation** instead of forms or spreadsheets.

Just chat with it ("*Is the projector free tomorrow 3–5pm?*", "*Book a mic for
Robotics Club on Friday 10am–12pm*") and the agent understands the request,
checks for clashes, and manages the booking for you. It is accessible through
both a **Telegram bot** and a **lightweight web UI**.

---

## 🛠️ Technology Stack and Tools Used

| Layer | Technology |
|---|---|
| **Language** | Python 3.11 |
| **AI / Agent** | OpenAI API (function-calling, ReAct agent pattern) |
| **Backend / API** | FastAPI, Uvicorn |
| **Chat Interface** | python-telegram-bot (Telegram), single-page web UI |
| **Database** | PostgreSQL |
| **ORM & Migrations** | SQLAlchemy, Alembic, psycopg2-binary |
| **Frontend** | HTML, CSS, JavaScript (glassmorphism theme, dark/light mode, mobile-responsive) |
| **Configuration** | python-dotenv (environment variables) |
| **Deployment** | Docker (Railway-ready Dockerfile) |

---

## ✨ Features and Functionalities Implemented

- 🤖 **Conversational AI agent** — understands plain-English requests using an
  OpenAI function-calling (ReAct) loop; no rigid commands needed.
- 💬 **Two interfaces** — chat via a **Telegram bot** or a **web UI**, both backed
  by the same engine.
- 📋 **Equipment listing** — view all available equipment with live availability.
- 🔎 **Availability check** — ask whether an item is free for a specific date and
  time range.
- 📅 **Smart booking** — create bookings with automatic **conflict detection** so
  two clubs can't double-book the same item.
- 🗂️ **Booking management** — list a club's bookings, cancel a booking, and mark
  equipment as returned.
- 👮 **Admin view & authentication** — see who currently holds each item, gated
  behind an admin login.
- 🧠 **Per-session memory** — the agent remembers the context of your conversation.
- 🕐 **Timezone-aware** — date/time handled in IST to avoid false "past date" errors.
- 🔄 **Auto-migration** — missing database columns are added automatically on startup.
- 📱 **Responsive UI** — glassmorphism design with dark/light mode that works on mobile.

---

## ⚙️ Installation / Execution Steps

### Prerequisites
- Python 3.11+
- PostgreSQL installed and running
- An OpenAI API key (and optionally a Telegram bot token)

### 1. Clone the repository
```bash
git clone https://github.com/anushkatiwari20/GearGenix.git
cd GearGenix
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Create the PostgreSQL database
```bash
createdb equipment_booking
```

### 4. Configure environment variables
Create a `.env` file in the project root with the following keys:
```env
OPENAI_API_KEY=your_openai_api_key
DATABASE_URL=postgresql://user:password@localhost:5432/equipment_booking
ADMIN_USERNAME=your_admin_username
TELEGRAM_BOT_TOKEN=your_telegram_token   # optional — bot is skipped if absent
```

### 5. Run the application
```bash
python main.py
```

### 6. Use it
- **Web UI:** open <http://localhost:8000/ui/index.html> in your browser.
- **Telegram:** search for your bot's username and start chatting.

#### Getting API keys
- **OpenAI** — create a key at <https://platform.openai.com> under *API Keys*.
- **Telegram** — message **@BotFather**, run `/newbot`, and copy the token.

---

## 👥 Team Members

| Name | Role | Roll No. / ID |
|---|---|---|
| [Member 1] | [Role] | [ID] |
| [Member 2] | [Role] | [ID] |
| [Member 3] | [Role] | [ID] |
| [Member 4] | [Role] | [ID] |

> _Replace the placeholders above with your actual team details._

---

## 📸 Project Screenshots / Output

> _Add your screenshots to a `screenshots/` folder in the repo, then the images
> below will display automatically. Replace the file names if yours differ._

**Web UI — Chat Interface**

![Web UI](screenshots/web-ui.png)

**Telegram Bot Conversation**

![Telegram Bot](screenshots/telegram-bot.png)

**Admin View**

![Admin View](screenshots/admin-view.png)

### Sample Conversation (Output)

```text
You:  What equipment do you have?
Bot:  Here's what's available:
        • Projector (2 units) — available
        • Wireless Microphone (4 units) — available
        • Speaker (2 units) — 1 in use
        • Laptop (3 units) — available

You:  Is the projector free tomorrow 3-5pm?
Bot:  Yes! A projector is free tomorrow from 3:00 PM to 5:00 PM. Want me to book it?

You:  Book a mic for Robotics Club on Friday 10am-12pm
Bot:  ✅ Booked! Wireless Microphone reserved for Robotics Club
      on Friday, 10:00 AM – 12:00 PM. Your booking ID is B012.

You:  Cancel booking B012
Bot:  Done — booking B012 has been cancelled and the microphone is free again.
```

---

## 🗂️ Project Structure

```
GearGenix/
├── agent/
│   ├── agent.py          # Core ReAct agent loop
│   ├── tools.py          # OpenAI function schemas
│   ├── tool_executor.py  # Maps tool calls → Python functions
│   ├── memory.py         # Per-session conversation history
│   └── prompts.py        # System prompt
├── bot/
│   └── telegram_bot.py   # Telegram interface
├── core/
│   └── booking_engine.py # All business logic
├── db/
│   ├── models.py         # SQLAlchemy ORM models
│   ├── database.py       # Engine + session management
│   └── seed.py           # Initial equipment data
├── api/
│   └── main.py           # FastAPI app
├── ui/
│   └── index.html        # Single-file web UI
├── config.py             # Environment variable loading
├── main.py               # Entry point
└── requirements.txt
```

---

## 💡 Example Queries

| What you type | What happens |
|---|---|
| `What equipment do you have?` | Lists all equipment with availability |
| `Is the projector free tomorrow 3-5pm?` | Checks availability |
| `Book a mic for Robotics Club on Friday 10am-12pm` | Starts booking flow |
| `Show bookings for Robotics Club` | Lists active bookings |
| `Cancel booking B007` | Cancels a booking |
| `Return equipment for booking B005` | Marks equipment as returned |
| `Who has the projector right now?` | Admin view of all active bookings |

### Telegram Bot Commands

| Command | Description |
|---|---|
| `/start` | Welcome message |
| `/help` | Example queries |
| `/clear` | Reset conversation history |
