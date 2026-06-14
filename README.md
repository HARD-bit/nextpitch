# ⚽ NextPitch

**La piattaforma per scoprire i prossimi talenti del calcio.**

NextPitch connette giovani calciatori (16–23 anni) con scout e recruiter di club semi-professionistici e professionistici in Italia e in Europa. Profili pubblici, statistiche, video highlight e contatto diretto — senza barriere enterprise.

---

## 🚀 Demo Live

Apri `nextpitch.html` direttamente nel browser. Nessun server richiesto.

**Funzionalità incluse nel prototipo:**
- 🏃 Catalogo giocatori con filtri (posizione, età, disponibilità)
- 🎯 Directory recruiter & scout verificati
- 🏆 Leaderboard ordinabile (gol, assist, valore, scout views)
- 📌 Watchlist persistente (localStorage)
- ⚖️ Confronto fianco a fianco tra 2 giocatori (radar sovrapposto)
- ⚽ Posizione tattica SVG nel profilo
- 👁️ Badge "scout questa settimana" per senso di urgenza

---

## 📁 Struttura Repository

```
nextpitch/
├── nextpitch.html          # Prototipo MVP (demo, apribile offline)
├── README.md               # Questo file
├── COMPETITIVE_ANALYSIS.md # Ricerca competitor + gap di mercato
├── ARCHITECTURE.md         # Stack scalabile + DB schema + roadmap tecnica
├── ROADMAP.md              # Prossimi passi strategici e operativi
└── docs/
    └── (future: wireframes, API spec, pitch deck)
```

---

## 🛠️ Stack (MVP Scalabile — Fase 1)

| Layer | Tecnologia | Perché |
|---|---|---|
| Frontend | Next.js 14 (App Router) | SSR, SEO, React, full-stack |
| Styling | Tailwind CSS | Rapido, design system |
| Database | Supabase (PostgreSQL) | Auth + DB + Storage + Realtime |
| Deploy | Vercel | CI/CD da GitHub, free tier |
| Email | Resend | Transazionale, DX ottima |
| Pagamenti | Stripe | Subscription + success fee |

---

## 📊 Posizionamento

> *"Il LinkedIn del calcio semi-professionistico"*

**Chi usiamo:** giocatori 16–23 anni in Serie D / Eccellenza / campionati EU equivalenti  
**Chi cerca:** scout e DS di club Serie C, estero (Ligue 2, 2.Bundesliga, LaLiga2)  
**Gap coperto:** nessuna piattaforma esistente serve questa fascia con profili attivi + recruiter visibili + contatto diretto

**Competitor principali:** Wyscout (troppo caro/pro), Playerhunter (no Italy focus), TransferRoom (solo club pro), Scoutpad (solo lato scout)

---

## 💡 Modello di Business

| Piano | Target | Prezzo |
|---|---|---|
| **Free** | Giocatori | €0 |
| **Scout** | Recruiter individuali | €29–49/mese |
| **Club** | Club Serie C/D | €99–299/mese |
| **Agency** | Procuratori | €19–49/mese |
| **Success Fee** | Trasferimento facilitato | 0.5–2% del valore |

---

## 🗺️ Roadmap

Vedi `ROADMAP.md` per il piano dettagliato.

**Fasi:**
1. ✅ **Fase 0** — Prototipo HTML (completata)
2. 🔄 **Fase 1** — MVP con backend Supabase + auth + profili reali (2–4 settimane)
3. ⏳ **Fase 2** — Subscription Stripe + SEO + email (1–3 mesi)
4. ⏳ **Fase 3** — Messaggistica + match AI + mobile (3–12 mesi)

---

## 🤝 Come Contribuire

1. `git clone https://github.com/tuousername/nextpitch`
2. Per il prototipo: apri `nextpitch.html` nel browser
3. Per la versione scalabile: vedi `ARCHITECTURE.md`

---

## 📄 Licenza

MIT — libero di usare, modificare, distribuire.

---

*NextPitch — Built in Italy 🇮🇹 | Giugno 2026*
