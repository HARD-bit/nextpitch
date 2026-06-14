# NextPitch — Roadmap Strategica
*Giugno 2026 — Documento Operativo*

---

## Fase 0 — Completata ✅
**Prototipo HTML dimostrabile**
- [x] Design e brand "NextPitch"
- [x] Catalogo giocatori + recruiter mock
- [x] Filtri, leaderboard, watchlist, confronto, posizione tattica
- [x] Analisi competitiva completata
- [x] Architettura scalabile documentata
- [x] Repository GitHub-ready

---

## Fase 1 — Validazione (Settimane 1–6)
*Obiettivo: provare che il mercato esiste. Non costruire nulla prima di aver parlato con persone reali.*

### A) Recruiter Outreach (priorità #1)

**Chi contattare prima:**
Il primo errore di ogni startup è costruire troppo prima di validare. Prima di scrivere una riga di backend, devi parlare con almeno 10 recruiter/DS reali e fare queste domande:

**Script intervista (30 minuti, anche via LinkedIn/WhatsApp):**
```
1. Come trovi attualmente i giocatori U23 per la tua rosa?
2. Usi piattaforme tipo Wyscout / Transfermarkt? Cosa ti manca?
3. Quanto pagheresti per uno strumento dedicato a quella fascia?
4. Il problema principale è trovare i giocatori o contattarli?
5. Cosa rende un profilo giocatore "credibile" ai tuoi occhi?
6. Ti mostrerei un prototipo — puoi darmi 5 minuti di feedback?
```

**Dove trovare i recruiter:**
- LinkedIn: cerca "Direttore Sportivo Serie C", "Responsabile Scouting Serie D"
- Federcalcio FIGC: lista club affiliati con contatti
- Lega Pro (Serie C): sito ufficiale, sezione club
- Telegram: gruppi di DS e direttori sportivi (esistono diversi)
- Twitter/X: #scouting #calcio #SerieC — molti DS sono attivi

**Target numero interviste prima di procedere:** 10 recruiter, 5 giocatori, 3 agenti

**Segnale verde per andare avanti:** almeno 3 persone dicono "lo pagherei" e indicano un prezzo
**Segnale rosso:** tutti dicono "lo userei ma gratis" → pivot necessario

---

### B) Giocatori Outreach

**Chi contattare:**
- Giocatori Serie D con profilo Instagram (cercali con hashtag #serieD #calcio)
- Settori giovanili di club Serie C (chiedere al coordinatore)
- Forum e gruppi Facebook di calciatori semi-pro italiani

**Proposta di valore per loro:**
> "Crei un profilo gratis, ti troviamo i recruiter. Nessuna commissione finché non vai a buon fine."

**Cosa chiedere:**
```
1. Come fai vedere le tue qualità a un club che non ti conosce?
2. Hai mai perso un'opportunità per mancanza di visibilità?
3. Un profilo online con le tue stats — lo useresti? Cosa ci metteresti?
4. Il tuo agente ti aiuta in questo o sei da solo?
```

---

### C) MVP Tecnico Minimo (solo dopo validazione)

**Cosa costruire per primo — in ordine di priorità:**

1. **Pagina profilo pubblica e indicizzata** (Next.js + Supabase)
   - URL tipo `nextpitch.it/p/marco-ferretti`
   - Tutti i campi del prototipo + foto
   - Senza login per chi legge
   - *Perché prima:* ogni profilo diventa un asset SEO. I recruiter trovano il giocatore su Google.*

2. **Registrazione giocatore** (auth email Supabase)
   - Form semplice: nome, posizione, club, stats stagione corrente
   - Upload foto (Supabase Storage)
   - Link video YouTube (no upload proprio per ora)
   - *Perché:* il costo di acquisizione è zero se il giocatore crea da solo il profilo*

3. **Ricerca recruiter** (read-only, no auth)
   - Filtri base: posizione cercata, lega
   - CTA: "Contatta questo scout" → apre email
   - *Perché:* dimostra valore immediato senza infrastruttura complessa*

4. **Registrazione recruiter** (form manuale + verifica via email)
   - Verifica manuale in fase 1 (tu controlla ogni recruiter)
   - Badge "Verificato" solo dopo check
   - *Perché:* la fiducia è il moat. Un recruiter fake brucia la piattaforma.*

**Cosa NON costruire ancora:**
- ❌ Messaggistica interna (usa email per ora)
- ❌ Pagamenti (valida prima che qualcuno vuole pagare)
- ❌ App mobile
- ❌ AI/algoritmo matching

---

## Fase 2 — Traction (Mesi 2–6)
*Obiettivo: 100 profili giocatori pubblicati, 20 recruiter attivi, primo MRR*

### Acquisizione Giocatori (cold start solving)

**Strategia del "canale caldo":**
Non puoi aspettare che i giocatori arrivino organicamente. Devi portarli tu.

- **Agenti / procuratori come moltiplicatori**: un agente con 15 assistiti ti porta 15 profili in un click. Offrigli un piano "Agency" gratuito per 6 mesi in cambio dei profili.
- **Settori giovanili di club**: contatta il coordinatore, offri profili gratuiti per tutti i loro U23. Un club = 20–30 profili.
- **Influencer calcio italiano**: profili Instagram/TikTok di ex giocatori o commentatori calcio semi-pro. Un post = centinaia di iscrizioni.
- **SEO a lungo termine**: ogni profilo pubblicato con il nome reale del giocatore diventa una pagina indicizzata su Google.

### Acquisizione Recruiter

- **Evento FIGC / LegaPro**: partecipare come sponsor o espositore a eventi di categoria
- **Newsletter DS**: molti DS leggono newsletter di mercato. Partnership con una newsletter esistente.
- **Cold outreach LinkedIn**: 50 messaggi/settimana a DS di Serie C con demo del prototipo allegata

### Primo MRR

Quando hai 50+ profili attivi, lancia il **piano Scout a €29/mese**:
- Accesso illimitato ai contatti
- Export lista watchlist
- Alert email quando un nuovo giocatore nella categoria cercata si iscrive

Target: 10 recruiter paganti = €290/mese MRR → proof of concept

---

## Fase 3 — Revenue (Mesi 6–18)
*Obiettivo: €10.000 MRR, primo trasferimento facilitato dalla piattaforma*

- Lancio piano Club (€199/mese) per club con accesso team scouting
- Lancio piano Agency per procuratori
- Success fee contrattualizzata per trasferimenti tracciati
- Messaggistica interna (Supabase Realtime)
- Notifiche "X scout ha visto il tuo profilo" per giocatori
- Match algoritmico semplice (filtro inverso: recruiter cerca ATT U21 → giocatori notificati)

---

## Prossimi Passi Concreti (questa settimana)

| # | Azione | Tempo | Priorità |
|---|---|---|---|
| 1 | Pubblica il prototipo su GitHub (guida sotto) | 30 min | 🔴 Alta |
| 2 | Crea profilo LinkedIn "NextPitch" con link demo | 20 min | 🔴 Alta |
| 3 | Identifica 10 DS/scout su LinkedIn da contattare | 1 ora | 🔴 Alta |
| 4 | Scrivi e invia i primi 5 messaggi di outreach recruiter | 1 ora | 🔴 Alta |
| 5 | Registra `nextpitch.it` su Namecheap (~€12/anno) | 10 min | 🟡 Media |
| 6 | Crea account Supabase + Vercel (gratuiti) | 30 min | 🟡 Media |
| 7 | Fai vedere il prototipo a 3 persone del settore questa settimana | Ongoing | 🔴 Alta |

---

## Come Pubblicare su GitHub (guida step-by-step)

```bash
# 1. Crea account su github.com se non ce l'hai

# 2. Crea nuovo repository:
#    github.com/new → nome: "nextpitch" → Public → Create

# 3. Installa Git sul tuo PC (git-scm.com) se non ce l'hai

# 4. Da terminale (PowerShell o Git Bash):
cd "percorso/alla/cartella/outputs"
git init
git add .
git commit -m "feat: NextPitch MVP prototipo + analisi competitiva"
git branch -M main
git remote add origin https://github.com/TUOUSERNAME/nextpitch.git
git push -u origin main

# 5. Su github.com → repository → Settings → Pages
#    Source: Deploy from branch → main → /root → Save
#    URL: https://tuousername.github.io/nextpitch/nextpitch.html
#    Il sito è online in ~2 minuti, gratis, per sempre
```

**Risultato:** il prototipo è online con un URL condivisibile, gratis, con versionamento.

---

## Template Messaggi Outreach

### Per Recruiter / DS (LinkedIn)
```
Ciao [Nome],

sto sviluppando NextPitch, una piattaforma per connettere scout 
con giocatori U23 di Serie D/C e campionati europei equivalenti — 
la fascia che Wyscout non copre.

Ho costruito un prototipo funzionante e vorrei capire se risolve 
un problema reale per chi fa scouting nella tua posizione.

Posso mandarti il link del demo? Ci vogliono 5 minuti e sarei 
curioso del tuo feedback da addetto ai lavori.

Grazie,
Gabriele
```

### Per Agenti / Procuratori (WhatsApp / email)
```
Ciao [Nome],

sto lanciando NextPitch: profili pubblici professionali per 
giocatori semi-pro, cercabili da recruiter italiani ed europei.

Per i tuoi assistiti è completamente gratis. Tu carichi le loro 
stats e highlight, i recruiter li trovano.

Ti mando il prototipo? Se ti interessa, nella fase beta offro 
l'account Agency gratis in cambio del feedback.
```

---

## Metriche Settimanali da Tracciare

```
Settimana 1-4 (validazione):
  □ Interviste recruiter completate: _/10
  □ Interviste giocatori completate: _/5
  □ "Lo pagherei" sentito: _/10

Settimana 5-12 (MVP live):
  □ Profili giocatori pubblicati: ___
  □ Recruiter registrati: ___
  □ Contatti recruiter→giocatore: ___
  □ Utenti tornati dopo 7 giorni: ___%

Mese 3+:
  □ MRR: €___
  □ Profili totali: ___
  □ Tasso conversione free→paid: ___%
```
