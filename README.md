Design and copy follow [these standards](https://github.com/lyhjeremy/lyhjeremy/blob/main/DESIGN_STANDARDS.md).
﻿# Leave Time Optimizer

> **Product overview:** https://lyhjeremy.github.io/leave-time-optimizer/overview/

A single-file web app that watches traffic and tells you the latest moment to leave home without being late. Built because I was tired of sitting in front of Google Maps doing mental subtraction before every LA trip.

Uses Google Routes API with three traffic models (optimistic / best-guess / pessimistic) to give you Relaxed, Usual, and Important departure times side by side.

## Quick start

1. Get a Google Maps API key at [Google Cloud Console](https://console.cloud.google.com/)
2. Enable these 4 APIs on your key''s Google Cloud project:
   - Routes API
   - Maps JavaScript API
   - Places API (classic, not "New")
   - Directions API
3. Open `leave-time-optimizer-1.17.0.html` in a text editor
4. Paste your key into the `const API_KEY = ''...''` line near the top
5. Save and open the file in a browser

## Full writeup

See [leave-time-optimizer-article.md](leave-time-optimizer-article.md) for the story behind the tool, the math, and the design rationale.

## License

MIT. Do whatever you want with it.

Jeremy Lee
