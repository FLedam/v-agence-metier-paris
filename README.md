# V Agence Métier Paris

A single-file HTML web application simulating a high-end Parisian executive recruitment agency. Built as a prop for a *Vampire: The Masquerade* tabletop RPG campaign set in Paris in 2026.

## What it is

The site presents itself as a legitimate executive headhunting firm offering a confidential behavioral evaluation to match candidates with a professional coach. In reality, the 25-question personality questionnaire secretly scores answers against the 10 Vampire: The Masquerade clans, and the "recommended coach" result corresponds to the dominant clan — without ever revealing it.

## Key technologies

- Pure HTML5, CSS3, JavaScript — no dependencies, no build step
- Single self-contained file (`index.html`)
- Responsive mobile-first design
- Embedded SVG logo and avatar portraits

## How to run locally

Simply open `index.html` in any modern web browser. No server required.

## Features

- **Landing page** — premium recruitment agency design with animations
- **25-question behavioral quiz** — questions designed as plausible HR/coaching content
- **Hidden scoring system** — each answer quietly assigns points to one or more of 10 clans
- **Coach result page** — shows a fictional coach profile (name, specialty, quote, bio, contact)
- **Admin / Game Master mode** — add `?admin=true` to the URL to reveal the matched clan, all scores, and the full answer → clan mapping

## Admin (MJ) mode

Append `?admin=true` to the URL after completing the quiz to unlock the Game Master panel, which displays:
- The identified clan
- Score breakdown for all 10 clans
- Per-answer clan attribution log

## The 10 clans (never displayed to players)

Ventrue, Toreador, Brujah, Nosferatu, Tremere, Gangrel, Malkavian, Banu Haqim, Lasombra, Hecata
