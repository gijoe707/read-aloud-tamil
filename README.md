# தமிழ் வாசிப்பகம் · Tamil Book Reader

> A zero-dependency, single-file Tamil reading app with AI-powered Tutor Mode, gamification, and offline support.

![Tamil Book Reader](https://img.shields.io/badge/language-Tamil-orange?style=flat-square)
![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen?style=flat-square)
![Single File](https://img.shields.io/badge/build-single%20HTML%20file-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)

---

## ✨ Overview

Tamil Book Reader is a fully self-contained HTML application for reading and learning Tamil. It runs entirely in the browser with no server, no build step, and no npm — just open `tamil_book_reader.html`.

It ships in two modes:

| Mode | Description |
|---|---|
| 📖 **Read to Me** | Word-by-word TTS playback with a moving highlight cursor |
| 🎤 **Tutor Mode** | Speech recognition listens to the student read aloud and gives real-time feedback |

---

## 🚀 Quick Start

### Option A — Double-click (Read Mode only)
Open `tamil_book_reader.html` directly in Chrome or Edge. Read Mode works immediately.

### Option B — Local server (recommended for Tutor Mode)

Tutor Mode uses the browser microphone. For mic permissions to be **saved permanently** (no re-prompt on every visit), the file must be served over `http://` rather than `file://`.

**Python (built into macOS / Linux):**
```bash
python -m http.server 8080
```
Then open [http://localhost:8080](http://localhost:8080) in Chrome or Edge.

**Node.js:**
```bash
npx serve .
```

**VS Code:** Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension → right-click `tamil_book_reader.html` → **Open with Live Server**.

> **Why localhost?** Chrome and Edge only persist microphone permissions for secure origins (`https://` or `http://localhost`). On `file://` the browser re-prompts every session. The app shows a warning banner when it detects `file://` protocol.

---

## 📖 Features

### Reader

- **Word-level rendering** — every Tamil word is a clickable `<span>`; click any word to hear its pronunciation
- **TTS warm-up** — plays a near-silent character before the first word to prevent the browser's "muted first word" bug
- **Auto-scroll** — active word is always centred in the viewport with smooth scrolling
- **Speed control** — 0.8× to 1.5× playback rate slider
- **Resume position** — `localStorage` saves your exact word index; refreshing the page resumes right where you left off

### Tutor Mode (Speech Recognition)

- **Silent-first** — the app does **not** read the word aloud automatically; the student reads first
- **Tap to hear** — a **👆 தட்டி கேளுங்கள்** pill appears beside the active word; tapping it speaks the word as a pronunciation model at any time
- **Three-strike escalation:**
  - 1st wrong → mic re-opens silently, "மீண்டும் முயலுங்கள்" label shown
  - 2nd wrong → app speaks the full word aloud as a model, then listens again
  - 3rd wrong → syllable-by-syllable hint spoken ("கவனி: ச - ர - ம்…"), auto-advance
- **Fuzzy matching** — handles Tamil agglutination by stripping common case/tense suffixes (`ஆல்`, `க்கு`, `கள்`, `ந்தது`…) and applying Levenshtein distance tolerance
- **Continuous mic** — recognition engine stays alive for the whole session; ghost-restarts silently after browser silence timeouts (~15–60 s) without re-prompting

### Gamification

| Element | Detail |
|---|---|
| ⚡ XP | +10 for first-try correct · +2 for correct after a retry |
| 🔥 Combo | Tracks consecutive correct words; 5+ streak doubles XP |
| ⭐ Stars | 3 stars = 100% accuracy · 2 = ≤20% errors · 1 = completed |
| 🏆 Badges | 🌱 புதியவர் (50 XP) · ⚔️ வாசிப்பு வீரன் (300 XP) · 🏛️ தமிழ் அறிஞர் (1000 XP) |
| 🎉 Confetti | Fires on story completion (intensity scales with star count) |
| 📊 Lifetime stats | Total XP, stars, and correct words persisted in `localStorage` |

### File Support

| Format | Method |
|---|---|
| `.txt` | Native `File.text()` |
| `.pdf` | [pdf.js](https://mozilla.github.io/pdf.js/) (CDN) — client-side text extraction |
| `.epub` | [JSZip](https://stuk.github.io/jszip/) (CDN) — HTML content extraction |

Drag-and-drop onto the page or use the upload button in the sidebar.

### Built-in Stories

Six stories are bundled — no internet required after the initial font/CDN load:

| Title | Source | Level |
|---|---|---|
| சிங்கமும் சுட்டியும் | Aesop's Fables | எளிது · Beginner |
| காகமும் குடமும் | Aesop's Fables | எளிது · Beginner |
| வெயிலும் காற்றும் | Aesop's Fables | எளிது · Beginner |
| உழைக்கும் தேனீ | Nature Story | மிகவும் எளிது · Very Easy |
| திருக்குறள் தேர்வு | Thiruvalluvar | இடைநிலை · Intermediate |
| சிங்கத்தின் உயிர் | Panchatantra | இடைநிலை · Intermediate |

---

## 🧰 Tech Stack

Everything is vanilla — no frameworks, no build tools, no package.json.

| Concern | Technology |
|---|---|
| UI & layout | HTML5 + CSS3 (CSS custom properties, Grid, animations) |
| Text-to-speech | [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis) — `window.speechSynthesis` |
| Speech recognition | [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) — `webkitSpeechRecognition` / `SpeechRecognition` |
| Sound effects | [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) — synthesised tones, no audio files |
| PDF parsing | [pdf.js 3.11](https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js) via CDN |
| EPUB parsing | [JSZip 3.10](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js) via CDN |
| Fonts | [Google Fonts](https://fonts.google.com/) — Noto Serif Tamil, Noto Sans Tamil, Playfair Display |
| Persistence | `localStorage` — session position, lifetime stats, story star ratings |

---

## 🖥️ Browser Compatibility

| Browser | Read Mode | Tutor Mode |
|---|---|---|
| Chrome 80+ | ✅ | ✅ Best |
| Edge 80+ | ✅ | ✅ Good |
| Safari 15+ | ✅ | ⚠️ Limited STT support |
| Firefox | ✅ | ❌ No `webkitSpeechRecognition` |

> **Tamil voice quality:** Microsoft Edge on Windows includes the **Microsoft Valluvar Neural** voice, which gives the best Tamil TTS. The app automatically selects Neural voices when available.

---

## 📁 Repository Structure

```
tamil_book_reader.html   ← the entire application (single file)
README.md
```

No build output. No `node_modules`. No configuration files.

---

## ⚙️ Customisation

### Adding your own stories

In the `STORIES` array near the top of the `<script>` block:

```javascript
{
  id: "my_story",           // unique identifier
  title: "என் கதை",         // Tamil title shown in sidebar
  subtitle: "எழுதியவர்",    // shown below title
  level: "எளிது · Beginner", // difficulty label
  text: `கதையின் உரை இங்கே...`
}
```

### Changing the voice

The app picks voices in this priority order:
1. Any `ta-IN` voice with "Neural" in the name
2. Any voice with "Valluvar" in the name
3. First available `ta-IN` voice
4. Falls back to browser default

To force a specific voice, modify `loadVoices()` in the script.

### Adjusting fuzzy match tolerance

In `fuzzyMatch()`, the Levenshtein tolerance is `0.3` (30% of the longer word's length). Increase to `0.4` for more lenient matching, decrease to `0.2` for stricter:

```javascript
return lev(s,t) <= Math.floor(Math.max(s.length, t.length) * 0.3);
```

---

## 🔒 Privacy

All processing is local. No audio is sent to any server by this application. Speech recognition uses the browser's built-in STT engine (Google's servers for Chrome, Microsoft's for Edge) — the same engine used by any website that requests mic access.

No analytics, no tracking, no cookies beyond `localStorage`.

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [Mozilla pdf.js](https://mozilla.github.io/pdf.js/) for client-side PDF rendering
- [JSZip](https://stuk.github.io/jszip/) for EPUB extraction
- [Google Fonts](https://fonts.google.com/noto/specimen/Noto+Serif+Tamil) for the Noto Tamil typefaces
- Aesop and Thiruvalluvar for the stories
