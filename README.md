# Leave Time Optimizer

A one-file browser app that works out the latest time you can leave and still arrive on time, using Google's three traffic models to give three risk levels side by side.

[Live app](https://lyhjeremy.github.io/leave-time-optimizer/) (needs your own API key) · [Write-up](https://lyhjeremy.github.io/leave-time-optimizer/overview/) · [Article](https://lyhjeremy.github.io/leave-time-optimizer/leave-time-optimizer-article.html)

Design and copy follow [these standards](https://github.com/lyhjeremy/lyhjeremy/blob/main/DESIGN_STANDARDS.md).

![Dashboard for a UCLA to LAX Terminal 7 trip arriving 6:45 PM: Relaxed says leave at 6:15 PM, Usual at 6:02 PM, Important at 5:38 PM](images/screenshot-dashboard-ucla-lax.png)

## Example

UCLA to LAX Terminal 7, arrive 6:45 PM, 5-minute buffer (18 May 2026):

| Mode | Routes API `trafficModel` | Expected drive | Leave at |
|---|---|---|---|
| Relaxed | `OPTIMISTIC` | 24 min | 6:15 PM |
| Usual | `BEST_GUESS` | 37 min | 6:02 PM |
| Important | `PESSIMISTIC` | 61 min | 5:38 PM |

The 37-minute gap between Relaxed and Important is the reason all three are shown at once: it says how unpredictable the route is.

## How it works

- Each departure time is sent to the Google Routes API three times, with `trafficModel` set to `OPTIMISTIC`, `BEST_GUESS` and `PESSIMISTIC`. This only works with `routingPreference: TRAFFIC_AWARE_OPTIMAL`; with any other setting the API silently returns the same duration for all three.
- Sampling future departures: on Start, one anchor request for a departure 20 minutes before the target sizes a departure window (target minus buffer, back by the anchor duration plus 15 minutes). Each poll then samples two departure times inside that window plus one for now: 9 API calls per poll. Under 25 minutes to target, it samples only now.
- Polling interval: every 15 minutes when departure is over an hour away, every 8 at 30 to 60 minutes, every 3 under 30.
- A browser notification and an on-screen alert at the leave-by time, plus a poll-history table showing how the recommendation moved.

## Stack

One HTML file, vanilla JavaScript, Google Routes, Maps JavaScript, Places (classic) and Directions APIs. No server, no build step.

## How to run

1. Get a Google Maps API key in the [Google Cloud Console](https://console.cloud.google.com/).
2. Enable four APIs on its project: Routes API, Maps JavaScript API, Places API (classic, not "New") and Directions API. Restricting the key to those four is recommended.
3. Open `leave-time-optimizer-1.17.0.html` in a text editor and paste your key into `const API_KEY = 'fill in your Google API here';` near the top.
4. Save and open the file in a browser.

The article estimates about $0.05 of Routes API usage per poll and 5 to 15 polls per trip (May 2026 prices).

## Repo layout

| Path | Contents |
|---|---|
| `leave-time-optimizer-1.17.0.html` | The app |
| `index.html` | Redirect so the Pages root opens the app |
| `leave-time-optimizer-article.md` | Article (served as `.html` by GitHub Pages) |
| `overview/` | Write-up page |
| `images/` | Screenshot, diagrams and hero photo |

## Limitations

- You need your own API key; it is not a hosted service.
- The forecast is Google's. If the pessimistic model misses an incident, so does the app.
- Built for a desktop browser. Below 1080px the columns stack, with no phone-specific design, and the countdown depends on the tab staying open.
- Driving only, one destination per trip.

## License

[MIT](LICENSE) © 2026 Jeremy Lee
