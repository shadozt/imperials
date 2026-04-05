# 🦅 Vienna Imperials — Coach App 2026

> Wiens erster Foam-Dodgeball-Verein · D1-Trainingsprogramm · Saison 2026

Eine Progressive Web App (PWA) für das Training-Tracking, Turnierverwaltung und Periodisierung der Vienna Imperials — gebaut mit Vanilla JavaScript, Supabase und reinem HTML. Kein React, kein Framework, kein Build-Step.

---

## 🚀 Live App

**[imperials.github.io/imperials-training](https://shadozt.github.io/imperials)**

---

## ✨ Features

### 📅 Trainingsplan
- Fixer Wochenplan (Mo Wurf & Catch · Do Social League · Fr Athletik & Taktik)
- Ferientraining-Rotation (14 alternative Einheiten)
- Aufgaben-Checklisten mit Abhaken per Tippen
- Terraband J-Band Protokoll \[TB\] farblich hervorgehoben
- Wochenfortschritt-Streifen

### 🏆 Turniere & Periodisierung
- London WDF Championship · Turnier Italien · Bangkok Weltmeisterschaft voreingestellt
- Eigene Turniere hinzufügen (Name, Datum, Ort, Farbe)
- **Automatische Periodisierung:** Jedes Turnier erzeugt automatisch
  - 5 Wochen vorher → `WETTKAMPF PEAK`
  - 2 Wochen vorher → `ZUSPITZEN` (Taper)
  - Während → `TURNIER`
  - 1 Woche danach → `REGENERATION`
- Countdown-Anzeige im Header

### 👤 User-Accounts
- Registrierung mit E-Mail + Passwort + Username
- Angemeldet bleiben (Supabase Session)
- Profilbild hochladen (Supabase Storage)
- Username ändern (30-Tage Cooldown)
- Passwort ändern

### 📊 Stats
- Imperial Score (0–100) aus Streak + Wochenquote + Saisondurchschnitt
- Monatsbalken-Chart
- Streak-Counter
- Export/Import als JSON-Backup

### 🗓 Kalender
- Jahreskalender 2026 mit Farbkodierung
- Fortschrittsanzeige pro Tag
- Feiertage & Schulferien (Wien/Österreich)
- Turnierdaten hervorgehoben

---

## 🏗 Tech Stack

| Was | Womit |
|-----|-------|
| Frontend | Vanilla JavaScript (kein Framework) |
| Auth & Datenbank | [Supabase](https://supabase.com) |
| Storage (Avatare) | Supabase Storage |
| Hosting | GitHub Pages |
| Fonts | Google Fonts (Bebas Neue + DM Sans) |
| Build | Keiner — direkt `index.html` |

---

## 🗄 Supabase Setup

### 1. Projekt erstellen
[supabase.com](https://supabase.com) → New Project → URL & anon key in `index.html` eintragen:

```js
var SB_URL = "https://dein-projekt.supabase.co";
var SB_KEY = "eyJ...";
```

### 2. Datenbank-Tabellen anlegen
Im Supabase Dashboard → **SQL Editor** → ausführen:

```sql
-- Profile
create table if not exists profiles (
  id uuid references auth.users primary key,
  username text unique not null,
  email text,
  avatar_url text,
  username_changed_at timestamptz,
  created_at timestamptz default now()
);
alter table profiles enable row level security;
create policy "own_profile" on profiles for all using (auth.uid() = id);

-- Trainings-Logs
create table if not exists logs (
  user_id uuid references auth.users not null,
  date text not null,
  tasks jsonb default '[]',
  updated_at timestamptz default now(),
  primary key (user_id, date)
);
alter table logs enable row level security;
create policy "own_logs" on logs for all using (auth.uid() = user_id);

-- Turniere
create table if not exists tournaments (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  name text not null,
  date_start text not null,
  date_end text not null,
  location text default '',
  color text default '#E8520A',
  is_official boolean default false,
  created_at timestamptz default now()
);
alter table tournaments enable row level security;
create policy "own_tourneys" on tournaments for all using (auth.uid() = user_id);
```

### 3. Storage Bucket für Avatare
Dashboard → **Storage** → New bucket → Name: `avatars` → **Public** ✓

---

## 📱 Als Handy-App installieren

**iOS (Safari):** Teilen → "Zum Home-Bildschirm" → App erscheint mit Imperials-Icon

**Android (Chrome):** Menü → "Zum Startbildschirm hinzufügen"

---

## 📂 Struktur

```
imperials-training/
├── index.html      # Gesamte App (HTML + 2× JS + Logos eingebettet)
└── README.md
```

Die App ist bewusst **eine einzige Datei** — kein Build-Prozess, kein `node_modules`, keine Dependencies außer zwei CDN-Skripten (Supabase + Google Fonts).

---

## 🗓 Trainingsplan 2026

| Tag | Training | Ort |
|-----|----------|-----|
| **Mo** | Wurf & Catch — Technik | Stella-Klein-Löw-Weg |
| Di | Gym: Explosiver Unterkörper | — |
| Mi | Zone 2 + Regeneration | — |
| **Do** | Social League — Game IQ | Stella-Klein-Löw-Weg |
| **Fr** | Athletik & Taktik D1 | Am Kaisermühlendamm |
| Sa | Kraft + Zone 2 | — |
| So | Ruhetag + Aktive Erholung | — |

Fett = **fixer Vereinstermin**

---

## 🌍 Turnier-Saison 2026

| Turnier | Datum | Ort |
|---------|-------|-----|
| 🏆 London WDF Championship | 4.–8. Juli | London, UK |
| 🇮🇹 Turnier Italien | 6.–7. September | Italien |
| 🌏 Bangkok Weltmeisterschaft | 7.–13. Dezember | Bangkok, TH |

---

## 🔒 Datenschutz

- Trainingsdaten werden lokal im Browser gespeichert (localStorage)
- Optional: Cloud-Sync via Supabase (eigene Datenbank, unter eigener Kontrolle)
- Kein Tracking, keine Werbung, keine externen Analytics
- Offline vollständig nutzbar (ohne Supabase-Konfiguration)

---

## 📄 Lizenz

Privates Projekt — Vienna Imperials Dodgeball Club, Wien 🇦🇹

---

*Built with ♔ for the Vienna Imperials — Wiens erster Foam-Dodgeball-Verein*
