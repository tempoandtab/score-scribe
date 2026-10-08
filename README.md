# ScoreScribe v1

Photograph a handwritten full score → get clean, engraved digital notation.
One file: `index.html`. No build step, no server, no cost.

## How to open it

- **iPad:** save `index.html` to Files, tap it — it opens in Safari. (The
  "Take photo" button will offer the camera.)
- **Any computer:** double-click the file, or drag it into a browser window.
- **To share/host it free:** drag the file into Netlify Drop, or commit it to
  a GitHub Pages repo. Needs internet access for the Verovio CDN + Gemini API.

## The 2-minute setup (free)

Transcription uses Google's Gemini API free tier — no credit card, and a
teacher's usage is a tiny fraction of the free quota.

1. Go to <https://aistudio.google.com/apikey> and sign in with Google.
2. Click **Create API key**, copy it.
3. Open ScoreScribe → **Settings** → paste the key. It is stored only in
   your browser (localStorage) — never sent anywhere except Google's API.

No key? Tap **"Try the demo"** — it loads a sample string-orchestra excerpt
so you can explore the review, correction, and export tools immediately.

## The workflow

1. **Photo** — one page per photo. Lay the page flat, fill the frame.
   Rotate buttons fix sideways shots; the photo is downscaled in-browser
   before sending (faster + cheaper).
2. **Transcribe** — the photo goes to Gemini with a prompt that demands
   MusicXML back: one `<part>` per staff, dynamics/articulations/text kept.
3. **Review & fix** — original photo and engraved score side by side.
   Anything the model was unsure about is listed under "Double-check these."
   Describe mistakes in plain words ("Viola, bar 3: second note is E, not D")
   and press **Apply correction** — the score is re-transcribed and
   re-engraved, nothing else changes.
4. **Export** — MusicXML (MuseScore/Dorico/Finale/Sibelius), MIDI, SVG,
   or Print → Save as PDF.

## Notes for future tinkering

- The whole "recognition layer" is one function: `callGemini()` in the
  script. Image in → MusicXML text out. Swap the provider/model there and
  nothing downstream changes.
- Engraving is the Verovio WASM toolkit (free, open source) via CDN.
- Prompts live in `TRANSCRIBE_PROMPT` / `correctionPrompt()` — tune them
  freely; that's the highest-leverage editing you can do.
- `DEMO_XML` / `DEMO_NOTES` at the bottom power the demo button.

## Troubleshooting

- *"Model not found" error* → Google renames models; check AI Studio for the
  current Flash model name and update the **Model name** field in Settings.
- *Weird transcription* → use the correction box; short, specific corrections
  ("Cello, bar 5: the rhythm is two eighths + quarter, not three quarters")
  work best. If it's badly off, retake the photo with flatter lighting.
- *"Couldn't load the notation engine"* → the engine downloads from a CDN on
  first visit. If it keeps failing, you may have opened the file inside
  another app's preview (which can block downloads) — open `index.html` in
  Safari or Chrome instead, then tap "Try again".
- *Notation engine still loading* → the Verovio bundle is ~8 MB; first load
  on slow connections can take ~30 seconds.
