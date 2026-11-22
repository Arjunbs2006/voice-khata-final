# Voice Khata — Final Presentation Practice Script

> Use this as your rehearsal guide. Read it top-to-bottom once, then run the live demo
> using the script below. Target total time: **8–10 minutes** (5 min demo + 3 min Q&A).

---

## 0. Pre-demo checklist (do 10 minutes before)

- [ ] Open https://voice-khata-production.up.railway.app on your phone **and** laptop.
- [ ] Confirm `/health` returns ok (open in browser tab).
- [ ] Log in with the demo shop email so the seeded data (Ramesh, Sunita, Suresh) is visible.
- [ ] Charge phone, enable Do-Not-Disturb, close other tabs.
- [ ] Have a backup: local dev server running at http://127.0.0.1:8000 (in case Wi-Fi fails).
- [ ] Keep the GitHub repo tab open for the "code" slide.
- [ ] Mic permission granted in the browser.

**Seeded demo data (use these names — they always exist):**

| Customer | Balance |
|---|---|
| Ramesh | ₹1,230 owed |
| Sunita | ₹0 |
| Suresh | ₹1,250 owed |

---

## 1. Opening pitch (30 sec, no slides needed)

> "Every kirana shop in India still runs on a handwritten *khata*. The shopkeeper
> writes credit, calculates balances by hand, and answers 'kitna baaki hai?' from
> memory — all while serving customers. **Voice Khata** turns that notebook into an
> AI that listens, understands, and replies in the shopkeeper's own language."

**Hook line for judges:** *"Speak once. It writes, calculates, and speaks back — in Hindi, English, Telugu, Kannada or Tamil."*

---

## 2. Live demo flow (5 min)

### Step 1 — Show the problem (15 sec)
Open the app on the phone. Point at the customer list + balances.
> "This is Ramesh's khata. He owes ₹1,230. The shopkeeper didn't type anything — it's all voice."

### Step 2 — Credit entry, Hindi (30 sec)
Tap mic → say: **"Ramesh ka 340 rupaye udhaar"**
- Expected: "Ramesh ka 340 rupaye udhaar likha. Balance ab 1570."
- Point out: amount parsed, balance updated instantly, spoken reply.

### Step 3 — Payment, English (20 sec)
Tap mic → say: **"Ramesh paid 200"**
- Expected: payment recorded, balance drops to 1370.

### Step 4 — Balance query (15 sec)
Tap mic → say: **"Ramesh ka kitna balance hai"**
- Expected: spoken reply "Ramesh ka baaki 1370 rupaye."

### Step 5 — Multilingual flex (60 sec) ⭐ the differentiator
Do three quick entries, one per language, narrating each:
- Telugu: **"Ramesh ki 500 appu raayu"** → credit ₹500
- Kannada: **"Suresh ge 200 appu baree"** → credit ₹200
- Tamil: **"Sunita kadan 100 ezhuthu"** → credit ₹100

> "Same pipeline. Same schema. The reply comes back in the language you spoke."

### Step 6 — Correction with audit trail (30 sec)
Say: **"Ramesh ka 340 nahi 430"**
- Expected: last entry corrected to ₹430, audit trail preserved.
> "Mistakes happen. Voice Khata fixes them without deleting history."

### Step 7 — Customer disambiguation (20 sec)
If two customers share a name prefix, show the clarification prompt.
> "It never guesses — it asks."

### Step 8 — Daily summary (15 sec)
Say: **"Aaj kitna udhaar gaya"**
- Expected: today's credit/payment/net totals spoken.

### Step 9 — Mobile + deployment (20 sec)
Rotate phone / show bottom nav / safe-area. Then open the live URL on laptop.
> "One Docker container on Railway serves both API and the built UI. Data persists on a volume."

---

## 3. Tech slide talking points (60 sec)

| Layer | What to say |
|---|---|
| STT | Gnani Prisma for 10 Indian languages + Groq Whisper fallback |
| LLM | OpenAI-compatible, structured JSON intent extraction |
| TTS | Gnani Timbre — replies in the same language |
| Fallback | Deterministic rule parser so it works **offline / without keys** |
| DB | SQLite on a Railway volume — survives restarts |
| Tests | 60 backend tests, frontend production build |

**Key engineering flex:** "We have a deterministic fallback parser, so even with zero API keys the mic demo still records transactions. The LLM only refines intent."

---

## 4. Anticipated judge questions & answers

**Q: What if there's no internet / no API keys?**
> "The deterministic fallback parser (fallback.py) handles all intents offline. Gnani/Groq only improve accuracy. We verified 60 tests pass with no keys."

**Q: How do you handle misheard names?**
> "Customer disambiguation — if multiple matches, we ask 'Ramesh ya Ramesh Kumar?' instead of guessing. Plus seeded customer IDs from name+phone."

**Q: Data privacy / where is data stored?**
> "Per-shop isolated SQLite on a Railway volume. JWT auth, passwordless email login. No third-party data sharing except STT/TTS providers for audio."

**Q: Why not just use ChatGPT?**
> "Structured JSON output + domain-specific fallback + Indian-language STT/TTS + offline capability. Generic LLMs hallucinate amounts; ours refuses to commit without a clear amount and customer."

**Q: Scalability?**
> "FastAPI async + stateless API. Swap SQLite for Postgres (asyncpg) for multi-tenant. Frontend is a static SPA served from the same container — CDN-friendly."

**Q: How accurate is the multilingual parsing?**
> "We have a 400+ example edge-case dataset and 12+ few-shot examples per new language. Fallback covers native script and transliteration."

**Q: What's next?**
> "WhatsApp/SMS receipts, GST invoice generation, multi-shop sync, and offline-first PWA."

---

## 5. Failure playbook (if something breaks live)

| Symptom | Fix |
|---|---|
| Mic returns mock transcript | Keys not set — explain "demo mode, real keys enable live STT" |
| Network down | Switch to local server `http://127.0.0.1:8000` |
| Wrong intent parsed | Show the text-input box (`/voice/text`) as manual fallback |
| App blank | Hard refresh; check `/health` |
| Language not detected | Use the language selector dropdown in Profile |

---

## 6. Closing (20 sec)

> "Voice Khata is not a chatbot — it's a **digital khata** that a shopkeeper can use
> while serving customers, in the language they think in. We built it end-to-end:
> voice in, voice out, persisted, deployed, and tested. Thank you."

**End with the live URL on screen:** https://voice-khata-production.up.railway.app

---

## Rehearsal tips

1. Time yourself — aim for ≤ 6 min demo + 3 min Q&A.
2. Practice the 3-language sequence (Step 5) until it's muscle memory — it's your wow moment.
3. Don't read the screen; narrate what the app is doing.
4. If a transaction fails, say "let me try once more" — judges prefer recovery over perfection.
5. Keep the seeded customer names (Ramesh, Sunita, Suresh) — they're guaranteed to exist.
