# NextPitch — Architettura Scalabile
*Versione 1.0 — Giugno 2026*

---

## Stack Tecnologico Scelto

### Perché Next.js + Supabase + Vercel

Questo stack viene definito "God-tier" per startup nel 2025-26: lancia 3x più veloce dei setup tradizionali, scala a milioni di utenti senza riscrivere nulla, e ha un costo iniziale prossimo a zero.

```
Frontend:   Next.js 14 (App Router)   → React, SSR, SEO, routing
Styling:    Tailwind CSS               → design system rapido
Database:   Supabase (PostgreSQL)      → relational, RLS, realtime
Auth:       Supabase Auth              → OAuth Google/email magic link
Storage:    Supabase Storage           → video highlight, foto profilo
Deploy:     Vercel                     → CI/CD automatico da GitHub
Email:      Resend                     → notifiche transazionali
Pagamenti:  Stripe                     → subscription + success fee
```

**Costo infrastruttura mese 0-6 (MVP):** ~€0–25/mese (free tier Supabase + Vercel)
**Costo a 1.000 utenti attivi:** ~€50–100/mese
**Costo a 10.000 utenti:** ~€200–400/mese (ancora sostenibilissimo)

---

## Schema Database (PostgreSQL / Supabase)

### Tabelle Core

```sql
-- UTENTI (gestito da Supabase Auth, esteso)
profiles (
  id            uuid PRIMARY KEY references auth.users,
  role          text CHECK (role IN ('player','recruiter','agent','club','admin')),
  full_name     text,
  avatar_url    text,
  created_at    timestamptz DEFAULT now()
)

-- GIOCATORI
players (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id    uuid REFERENCES profiles(id) ON DELETE CASCADE,
  -- Info base
  date_of_birth date,
  nationality   text,
  second_nationality text,
  height_cm     int,
  weight_kg     int,
  preferred_foot text CHECK (preferred_foot IN ('right','left','both')),
  position      text,           -- ATT, CEN, DC, TS, TD, POR, TRQ, ALA
  secondary_pos text,
  -- Club attuale
  current_club  text,
  club_league   text,
  contract_until date,
  available     boolean DEFAULT true,
  -- Mercato
  market_value_eur int,
  agent_id      uuid REFERENCES agents(id),
  -- Meta
  bio           text,
  slug          text UNIQUE,    -- per URL pubblico: nextpitch.io/p/marco-ferretti
  is_published  boolean DEFAULT false,
  views_count   int DEFAULT 0,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
)

-- STATISTICHE STAGIONE (una riga per stagione per giocatore)
player_seasons (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  player_id     uuid REFERENCES players(id) ON DELETE CASCADE,
  season        text,           -- '2024-25'
  club          text,
  league        text,
  appearances   int DEFAULT 0,
  goals         int DEFAULT 0,
  assists       int DEFAULT 0,
  minutes       int DEFAULT 0,
  yellow_cards  int DEFAULT 0,
  red_cards     int DEFAULT 0,
  -- Attributi (0-100)
  attr_speed    int,
  attr_shooting int,
  attr_dribbling int,
  attr_passing  int,
  attr_physical int,
  attr_defending int,
  created_at    timestamptz DEFAULT now()
)

-- VIDEO HIGHLIGHT
player_videos (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  player_id     uuid REFERENCES players(id) ON DELETE CASCADE,
  title         text,
  storage_path  text,           -- Supabase Storage
  youtube_url   text,           -- alternativa
  duration_sec  int,
  thumbnail_url text,
  is_primary    boolean DEFAULT false,
  created_at    timestamptz DEFAULT now()
)

-- RECRUITER
recruiters (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id    uuid REFERENCES profiles(id) ON DELETE CASCADE,
  role_title    text,
  club          text,
  league        text,
  country       text,
  is_verified   boolean DEFAULT false,
  verified_at   timestamptz,
  specializations text[],       -- ['ATT','U21','CEN']
  bio           text,
  contact_email text,
  transfers_count int DEFAULT 0,
  created_at    timestamptz DEFAULT now()
)

-- AGENTI
agents (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id    uuid REFERENCES profiles(id) ON DELETE CASCADE,
  agency_name   text,
  license_number text,
  country       text,
  contact_email text,
  contact_phone text,
  created_at    timestamptz DEFAULT now()
)

-- WATCHLIST (recruiter salva giocatori)
watchlist (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  recruiter_id  uuid REFERENCES recruiters(id) ON DELETE CASCADE,
  player_id     uuid REFERENCES players(id) ON DELETE CASCADE,
  note          text,
  created_at    timestamptz DEFAULT now(),
  UNIQUE(recruiter_id, player_id)
)

-- CONTATTI / RICHIESTE
contact_requests (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  from_id       uuid REFERENCES profiles(id),
  to_player_id  uuid REFERENCES players(id),
  message       text,
  status        text DEFAULT 'pending' CHECK (status IN ('pending','read','responded','closed')),
  created_at    timestamptz DEFAULT now()
)

-- VISUALIZZAZIONI (analytics)
player_views (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  player_id     uuid REFERENCES players(id) ON DELETE CASCADE,
  viewer_id     uuid REFERENCES profiles(id),   -- null = anonimo
  viewer_role   text,
  created_at    timestamptz DEFAULT now()
)

-- TRASFERIMENTI (storico)
transfers (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  player_id     uuid REFERENCES players(id),
  from_club     text,
  to_club       text,
  fee_eur       int,
  transfer_type text CHECK (transfer_type IN ('permanent','loan','free')),
  facilitated_by_platform boolean DEFAULT false,
  season        text,
  confirmed_at  timestamptz
)

-- PIANI SUBSCRIPTION
subscriptions (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id    uuid REFERENCES profiles(id),
  stripe_customer_id text,
  stripe_sub_id text,
  plan          text CHECK (plan IN ('free','scout','club','agency','pro')),
  status        text DEFAULT 'active',
  current_period_end timestamptz,
  created_at    timestamptz DEFAULT now()
)
```

### Row Level Security (RLS) — esempi chiave

```sql
-- Giocatori: chiunque vede profili pubblicati; solo il proprietario modifica
CREATE POLICY "Public players visible" ON players
  FOR SELECT USING (is_published = true);

CREATE POLICY "Owner can edit own player" ON players
  FOR ALL USING (profile_id = auth.uid());

-- Watchlist: solo il recruiter proprietario vede la sua lista
CREATE POLICY "Recruiter sees own watchlist" ON watchlist
  FOR ALL USING (
    recruiter_id IN (SELECT id FROM recruiters WHERE profile_id = auth.uid())
  );
```

---

## Struttura File Next.js (App Router)

```
nextpitch/
├── app/
│   ├── layout.tsx              # Root layout (nav, footer)
│   ├── page.tsx                # Homepage
│   ├── players/
│   │   ├── page.tsx            # Lista giocatori con filtri
│   │   └── [slug]/
│   │       └── page.tsx        # Profilo pubblico (SSG per SEO)
│   ├── recruiters/
│   │   └── page.tsx            # Lista recruiter
│   ├── leaderboard/
│   │   └── page.tsx            # Top giocatori
│   ├── dashboard/              # Area privata (autenticata)
│   │   ├── layout.tsx
│   │   ├── page.tsx            # Overview
│   │   ├── profile/            # Modifica profilo giocatore
│   │   ├── watchlist/          # Lista salvati (recruiter)
│   │   ├── messages/           # Messaggi/richieste
│   │   └── settings/           # Abbonamento, account
│   ├── auth/
│   │   ├── login/page.tsx
│   │   └── callback/route.ts   # OAuth callback
│   └── api/
│       ├── players/route.ts    # REST endpoint (server)
│       ├── contact/route.ts    # Invio richieste contatto
│       └── stripe/
│           └── webhook/route.ts
│
├── components/
│   ├── ui/                     # Button, Card, Badge, Modal...
│   ├── PlayerCard.tsx
│   ├── PlayerModal.tsx
│   ├── CompareModal.tsx
│   ├── PitchSVG.tsx
│   ├── RadarChart.tsx
│   ├── RecruiterCard.tsx
│   ├── Leaderboard.tsx
│   ├── Filters.tsx
│   └── Navbar.tsx
│
├── lib/
│   ├── supabase/
│   │   ├── client.ts           # Browser client
│   │   └── server.ts           # Server client (RSC)
│   ├── stripe.ts
│   ├── types.ts                # TypeScript types dal DB
│   └── utils.ts
│
├── hooks/
│   ├── useWatchlist.ts
│   ├── usePlayers.ts
│   └── useAuth.ts
│
├── public/
│   └── og-image.png            # Open Graph per SEO social
│
├── .env.local                  # Keys (non committate)
├── next.config.ts
├── tailwind.config.ts
└── package.json
```

---

## Fasi di Scalabilità

### Fase 0 — Ora (MVP statico)
- HTML single file
- Dati mock
- Demo funzionante
- Costo: €0

### Fase 1 — MVP con Backend (2–4 settimane)
- Next.js + Supabase free tier
- Auth email/Google
- CRUD profili giocatore
- Upload foto (Supabase Storage)
- Deploy Vercel (dominio nextpitch.it)
- Costo infrastruttura: €0–10/mese

### Fase 2 — Traction (1–3 mesi)
- Subscription Stripe (recruiter plan)
- Video upload o link YouTube
- Profili recruiter con verifica manuale
- SEO: ogni giocatore = pagina indicizzata
- Email onboarding (Resend)
- Costo: €25–50/mese

### Fase 3 — Crescita (3–12 mesi)
- Messaggistica interna
- Match algoritmico (recruiter ↔ giocatore per fit)
- App mobile (React Native con Expo, stesso codebase)
- API pubblica per club
- Success fee Stripe (pagamento trasferimenti)
- Costo: €100–300/mese

### Fase 4 — Scale (12+ mesi)
- AI recommendation engine
- Video AI analysis (come aiScout ma semi-pro)
- White-label per federazioni regionali
- Espansione: Spagna, Francia, Polonia
- Costo: €500+/mese (ma revenue >> costi)

---

## Metriche da Tracciare dal Day 1

```
Acquisizione:   # profili giocatori creati / settimana
Attivazione:    % profili completati (foto + stats + almeno 1 video)
Retention:      % recruiter attivi dopo 30 giorni
Revenue:        MRR, ARR, ARPU per piano
Virality:       # giocatori invitati da agenti
North Star:     # contatti recruiter → giocatore / mese
```
