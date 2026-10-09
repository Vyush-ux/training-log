# ⚓ Winter Arc — Training Log

A gamified daily habit tracker for a five-month **Winter Arc** (Oct 1 → Feb 28), wrapped in hand-drawn, One Piece–inspired anime scenery. Log workouts, nutrients, water, sleep, weight and school goals, earn XP, keep streaks alive and unlock badges — and it syncs across all your devices.

**▶ Live app: [vyush-ux.github.io/training-log](https://vyush-ux.github.io/training-log/)**

![Today tab in the Alabasta day theme](docs/screenshots/hero.jpg)

---

## Contents

- [Features](#features)
- [Themes](#themes)
- [Screenshots](#screenshots)
- [Getting started](#getting-started)
- [How XP, streaks and badges work](#how-xp-streaks-and-badges-work)
- [Cloud sync & privacy](#cloud-sync--privacy)
- [Running it yourself](#running-it-yourself)
- [Tech notes](#tech-notes)
- [Disclaimer](#disclaimer)

---

## Features

### Today
- **Daily progress ring** showing how much of today's checklist is done, with a celebration when you hit 100%.
- **Week strip** (Mon–Sun) with a mini ring per day and a ✓ for successful days — tap a day to open its log.
- **Workout quests** in four categories (Strength, Endurance, Speed, Flexibility), each with its own XP value.
- **Nutrients & supplements** — five core ones plus any you add.
- **Water tracker** — tap glasses to fill them; the daily goal is adjustable.
- **Sleep** — bedtime/wake-up with hours slept, distance from the 8 h goal and a 7-day average.
- **Weight** — log a weigh-in; the list shows the change since the previous one.
- **School goals** — subject boxes with their own goals and a bonus for finishing a whole subject.
- **Daily check-in** — pick a mood (Rough → Great) and leave a one-line note.
- **Per-habit streaks** (`9d`) on every habit you've done two or more days in a row.
- **Log history** — every day's goals, values and XP, grouped by category.

### Insights
- Headline numbers: current & best streak, success rate, perfect days, level, average sleep, badges unlocked.
- **Voyage** — a dot heatmap of the whole Winter Arc, coloured by how much of each day you completed.
- **Weight trend** chart with hover details.
- Attribute levels for Strength, Endurance, Speed and Flexibility.

### Badges
- 17 badges as brushed-gold medals (streaks, perfect days, levels, water, sleep, school, check-ins…).
- Tap any badge to see **what it is, exactly how to earn it, and your progress**.

### Everywhere
- Works on phone, tablet and desktop; can be added to your home screen.
- Cloud sync between devices with a short sync code, plus an on-device backup copy so a bad connection never wipes your data.
- Respects *reduce motion* — animations and effects switch off.

## Themes

The background is a living, hand-drawn anime scene that changes with the time of day (or pick one with the theme button):

| Theme | When (auto) | Scene |
|---|---|---|
| ☀︎ **Alabasta** | 06:00 – 17:00 | Desert kingdom: painted dunes, heat haze, the palace on the horizon and the crew's farewell salute on the deck |
| ◒ **Wano** | 17:00 – 06:00 | Sunset over the Flower Capital: the castle under the curling sakura, lantern-lit town, falling petals and Onigashima glowing red at sea |
| ☾ **Water 7** | via the theme button | Moonlit fountain city: a stacked town with glowing windows, the great fountain, the sea train and Yagara boats on the canal |

> **Easter egg:** in the Wano theme, click **Onigashima** (or tap **“· wano”** at the top on a phone). Something wakes up. 🐉

## Screenshots

| Wano (with the easter egg) | Water 7 |
|---|---|
| ![Wano theme](docs/screenshots/wano.jpg) | ![Water 7 theme](docs/screenshots/water7.jpg) |

| Insights | Badges |
|---|---|
| ![Insights tab](docs/screenshots/insights.jpg) | ![Badges tab](docs/screenshots/badges.jpg) |

| Phone — Today | Phone — Insights | Badge details |
|---|---|---|
| <img src="docs/screenshots/mobile-today.jpg" width="240" alt="Today on a phone"> | <img src="docs/screenshots/mobile-insights.jpg" width="240" alt="Insights on a phone"> | <img src="docs/screenshots/badge-detail.jpg" width="240" alt="Badge detail popup"> |

*Screenshots use sample data.*

## Getting started

1. Open the **[live app](https://vyush-ux.github.io/training-log/)**.
2. On first launch choose **Create a new sync code** and write the code down — or enter an existing code to connect another device.
3. Tick off goals through the day. Everything saves automatically; **Log it** buttons and **Log today's goals** also record unfinished items (for 0 XP) in your history.
4. On a phone, use your browser's **Add to Home Screen** to get it like an app.

## How XP, streaks and badges work

- **XP** — workout quests and school goals give the XP shown next to them; finishing a whole subject gives its bonus; each nutrient, reaching the water goal, 8 h+ of sleep, a weight log and the daily check-in give 5 XP. **Every 100 XP is one level.**
- **Successful day** — hit at least **2 of these 3**: one workout quest, all five core nutrients, bedtime + wake-up logged. Consecutive successful days build your **streak**.
- **Perfect day** — every item on the Today page done (the ring reaches 100%).
- **Badges** unlock automatically when you meet their goal; open any badge for the exact rule and your progress.
- Past days lock at midnight; today stays editable until the day ends.

## Cloud sync & privacy

- Your data is stored in **Firebase Cloud Firestore** as one document per sync code (`trainingLogs/<code>`).
- **The sync code works like a password** — anyone who has it can see and change that log, so keep it private.
- A copy is also kept in the browser so the app keeps working offline; changes made offline are sent up when the connection returns.
- **Reset all progress** at the bottom of the page wipes the log for the current code.

## Running it yourself

The whole app is a single file: [`index.html`](index.html). No build step, no dependencies to install.

**Open locally**

```bash
python -m http.server 8000
```

then visit `http://localhost:8000`.

**Use your own Firebase project** (for forks)

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com), add a **Web app**, and enable **Cloud Firestore**.
2. Paste your project's config into the `firebaseConfig` block near the top of `index.html`. These values are not secret — access is controlled by Firestore rules.
3. Add rules that only allow access to individual sync-code documents, for example:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trainingLogs/{code} {
      allow read, write: if code.size() >= 6;
    }
  }
}
```

**Deploy** — push to GitHub and enable **Settings → Pages → Deploy from a branch → `main` / root**.

## Tech notes

- Plain HTML, CSS and JavaScript in one file; Firebase JS SDK (Firestore) loaded from Google's CDN.
- All scenery, characters, badges and charts are original inline **SVG**, animated with CSS and a little `requestAnimationFrame` — no image assets besides the app icon.
- Painted look via SVG filters (texture only, no blur); animated parts live on separate layers so the paint is rendered once.
- Off-screen scenes pause their animations; `prefers-reduced-motion` disables motion entirely.
- The Onigashima roar is synthesised live with the Web Audio API.

```
training-log/
├── index.html           # the entire app
├── README.md
└── docs/
    └── screenshots/     # images used in this README
```

## Disclaimer

This is a personal, non-commercial fan project. *One Piece* and its characters, names and imagery are © Eiichiro Oda / Shueisha / Toei Animation. This project is not affiliated with or endorsed by them. The scenery and characters in the app are original drawings inspired by the anime's style; the app icon is fan artwork belonging to its respective owner.
