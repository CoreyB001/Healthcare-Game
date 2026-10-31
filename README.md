# Health Quest by Beebe Healthcare — Version 1.5

A browser-based, five-lane health education game with **288 behaviors imported from the two user-supplied CSV lists** and their **exact original scores**. This version preserves the Beebe Healthcare logo, seven landscapes, and Easy / Normal / Challenging difficulty selection.

## What's new in 1.5

- All **144 healthy behaviors** and **144 unhealthy behaviors** from the two CSVs replace the previous 14 example choices.
- Every source description and score is preserved *exactly*, including the original score signs. Positive behaviors add points; negative behaviors subtract points.
- A suitable emoji is assigned based on the behavior text or category. The emoji itself is decorative; the behavior text and exact score come from the CSVs.
- Points remain **hidden until collection**. When collected, a positive/negative point popup appears, and the behavior, score and source category display below the game.
- Icons come from a randomized **shuffle deck**, so a behavior isn't repeated until all 288 have been drawn. Because a playthrough can end before the deck is exhausted, not every behavior will necessarily appear in every game.
- More space is allowed for longer behavior descriptions in the five-lane layout.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Complete five-lane game and UI |
| `behaviors.js` | 288 behavior entries generated from the CSVs; loaded automatically by `index.html` |
| `beebe-logo.png` | Beebe Healthcare branding |
| `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png` | Installable web app and offline support |
| `source-data/healthy_behaviors.csv` | Original positive behaviors and scores |
| `source-data/unhealthy_behaviors.csv` | Original negative behaviors and scores |
| `tools/build_behaviors.py` | Optional offline Python generator to rebuild `behaviors.js` from modified CSV files |

## How to play locally

Open `index.html` in a modern browser, choose a difficulty, then press **Start game**. On a phone, tap/drag to a lane or use the left/right buttons. Arrow keys also work on a computer.

For full PWA/offline testing, from the game folder run:

```sh
python -m http.server 8000
```

Visit `http://localhost:8000` in the same computer's browser. For testing on a separate mobile device, publish to HTTPS instead.

## Gameplay

- Five columns; the player begins in the middle lane.
- Seven landscapes, each lasting 18 seconds (about 126 seconds total).
- **100 starting points**; immediate game over if the score falls **below 0**.
- Health choices move faster and appear in greater numbers as levels advance.
- Difficulty selected before starting or replaying:

| Mode | Falling speed | Wave frequency | Icons per wave |
| --- | --- | --- | --- |
| Easy | 0.75× normal | 1.30× interval (less frequent) | Normal minus 1 (minimum 1) |
| Normal | 1.00× | Baseline | Baseline |
| Challenging | 1.30× normal | 0.80× interval (more frequent) | Normal plus 1 (maximum 5) |

**Note:** The supplied values reach **+100** and **−100**, substantially more than the earlier demonstration scoring system. They are not normalized or altered. Players may lose quickly if they collect highly negative choices.

## Publish with GitHub Pages

1. Open an existing public `health-quest` GitHub repository or create a new one.
2. Upload the **seven runtime files** from the project root to the repository root: `index.html`, `behaviors.js`, `beebe-logo.png`, `icon-192.png`, `icon-512.png`, `manifest.webmanifest`, and `sw.js`. The `README.md`, `source-data`, and `tools` are optional for public hosting, but useful for maintaining the game.
3. Open **Settings → Pages**, choose **Deploy from a branch**, `main`, `/(root)`, and save.
4. Open the published URL and check that the game starts and shows health icons. If updating an older version, reload once while online so the PWA refreshes its cached files.
5. Share the URL or its QR code.

## Editing behavior lists

Edit `source-data/healthy_behaviors.csv` or `source-data/unhealthy_behaviors.csv`, preserving columns `Category,Behavior,Points`. Then run from the project root:

```sh
python tools/build_behaviors.py
```

This regenerates `behaviors.js`; the script validates the original 288-row version. If changing the number of rows, update that validation in `tools/build_behaviors.py` and the corresponding check in `index.html`. After editing the dataset, increment the `CACHE` version in `sw.js` so users' devices refresh their files.

## Privacy and clinical review

The game does not collect accounts, personal health information, analytics, or other player data. Behavior scores are **illustrative game values supplied by the content owner**, **not medical risk weights or clinical guidance**. Have educational content and any clinical claims reviewed before a public health-system campaign.
