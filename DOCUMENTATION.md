# 🎵 Vibestream — Comprehensive Documentation

> An offline music player web app with advanced audio controls, visualizations, and spatial effects. Built with vanilla JavaScript, modern CSS, and IndexedDB for local storage.

**Live Demo:** [https://vibestreamsong.netlify.app/](https://vibestreamsong.netlify.app/)  
**Repository:** [s0ura8hs/vibestream](https://github.com/s0ura8hs/vibestream)

---

## 📑 Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Project Structure](#project-structure)
4. [Technology Stack](#technology-stack)
5. [Getting Started](#getting-started)
6. [Architecture & Core Systems](#architecture--core-systems)
7. [Key Components](#key-components)
8. [API Reference](#api-reference)
9. [Styling & Theme System](#styling--theme-system)
10. [Audio Features](#audio-features)
11. [Data Persistence](#data-persistence)
12. [Deployment](#deployment)
13. [Development Guide](#development-guide)

---

## 🎯 Overview

**Vibestream** is a fully offline-capable music player with a Spotify-like interface. It allows users to:
- Upload and manage local audio files
- Create and organize playlists
- Use advanced audio controls (EQ, spatial effects, visualizations)
- Search and browse music library
- Enjoy ambient and full-screen visualizer modes
- Control playback with keyboard-friendly controls

The app stores everything locally in **IndexedDB**, requiring no server or backend. It's a perfect personal music hub for offline listening.

---

## ✨ Features

### Core Playback
- ▶️ Full playback controls (play, pause, next, previous)
- 🔀 Shuffle mode with state persistence
- 🔁 Multiple repeat modes (off, one, all)
- 📊 Progress bar with seek functionality
- 🔊 Volume control with smooth transitions

### Library Management
- 📤 Drag-and-drop file upload (mp3, wav, ogg, m4a, flac)
- 🏷️ Automatic metadata extraction (tags with jsmediatags)
- 📚 Library organized by songs, albums, artists
- ❤️ "Liked Songs" playlist with heart toggle
- ⏱️ "Recently Played" tracking

### Playlist System
- ✏️ Create custom playlists with name & description
- ➕ Add songs to multiple playlists
- 🗑️ Remove playlists and songs
- 📋 View playlist details with song counts
- 🎛️ Shuffle individual playlists

### Audio Processing
- 🎚️ 5-band parametric equalizer with presets:
  - Flat, Rock, Pop, Jazz, Classical, Bass Boost, Vocal
- 🌐 Spatial audio effects:
  - Normal stereo, 3D panning, 8D audio, Room reverb, Hall reverb, Cave echo
- Uses **Web Audio API** with BiquadFilters and ConvolverNode

### Visual Modes
- 🎨 **Ambient Mode**: Holi-inspired color animations + floating cover art
- 📈 **Visualizer Mode**: Multiple visualization styles:
  - Bars (frequency analyzer)
  - Wave (sine wave)
  - Circle (radial bars)
  - Grid (frequency grid)
- Real-time canvas animations synced to audio

### Interface
- 🌓 Light/Dark theme toggle (persisted)
- 🎤 Voice search with Web Speech API
- 🔍 Real-time search across library
- 🎵 History navigation (back/forward buttons)
- 📱 Responsive design (desktop + mobile)
- ⌨️ Keyboard shortcuts support

### User Experience
- 🔔 Toast notifications for feedback
- 🧵 Context menu (right-click song options)
- 📊 Library statistics (songs, artists, albums)
- 🌅 Time-based greeting in hero section
- 🎭 Smooth animations & transitions

---

## 📁 Project Structure

```
vibestream/
├── index.html        (Main HTML structure - 430 lines)
├── style.css         (Complete styling - 1000+ lines)
├── script.js         (Core logic & interactions - 2000+ lines)
└── DOCUMENTATION.md  (This file)
```

### File Sizes
- **HTML:** 20.9 KB (14.4% of codebase)
- **CSS:** 49.1 KB (33.8% of codebase)
- **JavaScript:** 75.1 KB (51.8% of codebase)

---

## 🛠 Technology Stack

| Layer | Technologies |
|-------|--------------|
| **Markup** | HTML5, semantic elements, ARIA attributes |
| **Styling** | CSS3 (Grid, Flexbox, Animations, Variables) |
| **JavaScript** | Vanilla ES6+, no frameworks required |
| **Audio API** | Web Audio API, HTML5 Audio element |
| **Storage** | IndexedDB (persistent local storage) |
| **Metadata** | jsMediaTags library (v3.9.7) |
| **Icons** | Font Awesome 6 (CDN) |
| **Fonts** | Google Fonts (Fraunces, Geist, Geist Mono) |
| **Deployment** | Netlify static hosting |

---

## 🚀 Getting Started

### Live Version
Visit **[https://vibestreamsong.netlify.app/](https://vibestreamsong.netlify.app/)** — no installation needed!

### Local Development

#### Prerequisites
- Modern browser (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- No build tools or Node.js required

#### Setup
```bash
# Clone the repository
git clone https://github.com/s0ura8hs/vibestream.git
cd vibestream

# Option 1: Use a simple server (Python)
python -m http.server 8000
# Visit http://localhost:8000

# Option 2: Use Node.js
npx http-server

# Option 3: VSCode Live Server extension
# Right-click index.html → "Open with Live Server"
```

#### First Run
1. Open the app in your browser
2. Click **"Upload"** or drag audio files into the dropzone
3. Select one or more audio files (mp3, wav, ogg, m4a, flac)
4. Wait for processing and metadata extraction
5. Start playing! 🎵

---

## 🏗 Architecture & Core Systems

### State Management
The app uses a centralized `state` object (JavaScript object, not Redux):

```javascript
const state = {
  // Library
  songs: {},           // Map of songId → song metadata
  albums: {},          // Map of albumName → album data
  artists: {},         // Map of artistName → artist data
  playlists: {},       // Map of playlistId → playlist data
  
  // Playback
  currentSongId: null, // Currently playing song
  queue: [],           // Playback queue
  queueIndex: 0,       // Current position in queue
  isPlaying: false,    // Play state
  
  // UI
  currentView: 'home', // Current page view
  theme: 'dark',       // Theme preference
  volume: 0.8,         // Volume level
  
  // Effects
  eqEnabled: true,
  eqPreset: 'flat',
  spatialEffect: 'none'
};
```

### Audio Pipeline
```
File Input
    ↓
jsmediatags (extract metadata)
    ↓
Web Audio API (decode audio data)
    ↓
HTML5 Audio Element (playback)
    ↓
AnalyserNode (frequency data for visualizer)
    ↓
GainNode → EQ Filters → Convolver (spatial effects) → output
```

### Data Flow
```
User Action (UI Event)
    ↓
Event Handler (JavaScript)
    ↓
Update State Object
    ↓
Persist to IndexedDB (background)
    ↓
Update DOM/Canvas Rendering
```

### Database Schema (IndexedDB)
**Store: "songs"**
```javascript
{
  id: "uid_timestamp",
  title: "Song Title",
  artist: "Artist Name",
  album: "Album Name",
  duration: 180.5,
  liked: false,
  playCount: 3,
  lastPlayed: 1684589200000,
  tags: { /* metadata from jsmediatags */ },
  blob: File // Actual audio data (ArrayBuffer in DB)
}
```

**Store: "playlists"**
```javascript
{
  id: "pl_uid",
  name: "My Playlist",
  description: "A mood playlist",
  songIds: ["uid1", "uid2", "uid3"],
  createdAt: 1684589200000,
  coverColor: "#ff8a3d"
}
```

---

## 🧩 Key Components

### 1. **Top Bar / Navigation**
- Brand logo with animated gradient dot
- History navigation (back/forward)
- Home & Library tabs
- Search bar with voice input button
- Theme toggle & Upload button

**HTML Selectors:** `.topbar`, `.brand`, `.search`, `.navbtns`

### 2. **Sidebar**
- Playlist list (dynamically populated)
- Library statistics (songs, artists, albums count)
- Scrollable with hover effects

**HTML Selectors:** `.sidebar`, `.playlists`, `.sidebar__stats`

### 3. **Main Content Area**
- **Home View:** Hero, dropzone, quick action cards, recent tracks, all tracks grid
- **Library View:** Tabbed interface (songs, albums, artists, playlists)
- **Search View:** Search results list
- **Playlist/Group Detail:** Cover, metadata, song list

**HTML Selectors:** `.main`, `.view`, `.hero`, `.dropzone`, `.songlist`, `.grid`

### 4. **Player (Footer)**
- Left: Album cover + song metadata + like button
- Center: Playback controls + seek bar + time display
- Right: Shuffle, repeat, EQ, visualizer, ambient, queue, volume

**HTML Selectors:** `.player`, `.player__controls`, `.player__progress`

### 5. **Panels (Floating)**
- **EQ Panel:** Toggle, presets, 5 band sliders, spatial effects
- **Queue Panel:** Next upcoming songs

**HTML Selectors:** `.panel`, `.panel--eq`, `.panel--queue`

### 6. **Modals**
- Create Playlist
- Add to Playlist
- Voice Input Status
- Context Menu

**HTML Selectors:** `.modal`, `.ctxmenu`

### 7. **Special Modes**
- **Ambient Mode:** Full-screen animated background + centered cover
- **Visualizer Mode:** Full-screen canvas visualization (4 modes)

**HTML Selectors:** `.ambient`, `.visualizer`, `#ambientCanvas`, `#visualizerCanvas`

---

## 📚 API Reference

### Core Functions

#### Audio Management
```javascript
// Play a song
playSong(songId, queue = null)

// Pause/Resume
togglePlayPause()

// Queue manipulation
nextSong()
prevSong()
addToQueue(songId)
clearQueue()

// Shuffle & Repeat
toggleShuffle()
cycleRepeat() // off → one → all → off

// Volume & Seek
setVolume(value) // 0-1
seekTo(percentage) // 0-100
```

#### Library Management
```javascript
// Upload & Process
handleFileUpload(files)
processAudioFile(file) // Returns { title, artist, album, duration }

// Song operations
likeSong(songId)
unlikeSong(songId)
deleteSong(songId)
recordPlayback(songId) // Updates recently played

// Search
searchLibrary(query) // Returns matching songs
```

#### Playlist Management
```javascript
// Create & Delete
createPlaylist(name, description)
deletePlaylist(playlistId)

// Modify
addSongToPlaylist(playlistId, songId)
removeSongFromPlaylist(playlistId, songId)

// Browse
getPlaylist(playlistId)
getAllPlaylists()
```

#### Audio Effects
```javascript
// Equalizer
enableEQ(enabled)
setEQPreset(presetName) // flat, rock, pop, jazz, classical, bass, vocal
setBandGain(bandIndex, gain) // bandIndex: 0-4, gain: -40 to +40 dB

// Spatial Effects
setSpatialEffect(effectName) // none, 3d, 8d, room, hall, cave

// Visualizer
startVisualizer(mode) // bars, wave, circle, grid
stopVisualizer()
```

#### UI/UX
```javascript
// Views
switchView(viewName) // home, library, search, playlist, group
openModal(id)
closeModal(id)

// Theme
toggleTheme()
setTheme(theme) // dark, light

// Notifications
toast(message, duration = 2500)
```

---

## 🎨 Styling & Theme System

### CSS Custom Properties (Variables)

#### Colors
```css
:root {
  --accent: #ff8a3d;           /* Warm amber (primary) */
  --accent-2: #ffd166;         /* Honey (secondary) */
  --accent-ink: #1a120b;       /* Dark brown (text on accent) */
  
  --text: #f4ece0;             /* Primary text (dark theme) */
  --text-dim: #a59c8e;         /* Secondary text */
  --text-mute: #6f6759;        /* Tertiary text */
  
  --bg: #0d0c0b;               /* Primary background */
  --bg-elev: #161413;          /* Elevated surface */
  --bg-elev-2: #1f1c1a;        /* More elevated surface */
  --bg-soft: #100f0e;          /* Soft background */
  
  --line: rgba(255,245,230,0.07);        /* Subtle borders */
  --line-strong: rgba(255,245,230,0.14); /* Stronger borders */
}

[data-theme="light"] {
  /* Light theme overrides */
}
```

### Layout System
```css
/* Grid Layout */
.app {
  display: grid;
  grid-template-columns: var(--sidebar-w) 1fr;
  grid-template-rows: var(--topbar-h) 1fr var(--player-h);
  grid-template-areas:
    "topbar topbar"
    "sidebar main"
    "player player";
  height: 100vh;
}

/* Responsive Breakpoints */
@media (max-width: 980px) {
  /* Hide sidebar on tablet */
}
@media (max-width: 640px) {
  /* Optimize for mobile */
}
```

### Component Classes
- `.primarybtn` — Orange primary button
- `.ghostbtn` — Transparent border button
- `.iconbtn` — Icon-only button
- `.linkbtn` — Inline link button
- `.chip` — Small tag/filter button
- `.card` — Rounded content card
- `.songrow` — Table row for songs
- `.modal` — Centered dialog overlay
- `.toast` — Notification message

### Animations
```css
@keyframes fadeUp { /* View transitions */ }
@keyframes slideUp { /* Panel appear */ }
@keyframes spin { /* Brand dot rotation */ }
@keyframes pulse { /* Pulsing effect */ }
@keyframes ping { /* Voice recording pulse */ }
@keyframes ambientFloat { /* Album cover float */ }
@keyframes toastIn/Out { /* Toast notifications */ }
```

---

## 🔊 Audio Features

### Equalizer (5-Band)
Uses **BiquadFilterNode** for each band:

| Band | Frequency | Type | Range |
|------|-----------|------|-------|
| 1 | 60 Hz | Lowshelf | -40 to +40 dB |
| 2 | 250 Hz | Peaking | -40 to +40 dB |
| 3 | 1 kHz | Peaking | -40 to +40 dB |
| 4 | 4 kHz | Peaking | -40 to +40 dB |
| 5 | 16 kHz | Highshelf | -40 to +40 dB |

**Presets:**
```javascript
presets: {
  flat: [0, 0, 0, 0, 0],
  rock: [5, 3, -2, 3, 8],
  pop: [-1, 2, 4, 2, -1],
  jazz: [2, 1, -3, 2, 5],
  classical: [0, -2, -1, -2, 4],
  bass: [10, 5, -2, -5, 0],
  vocal: [-1, 3, 4, 2, -2]
}
```

### Spatial Effects
```javascript
// Using Web Audio API Convolver Node
Effects:
1. Normal → No processing
2. 3D → Rotating stereo panning (StereoPanner)
3. 8D → Circular panning path
4. Room → Small room impulse response reverb
5. Hall → Concert hall reverb
6. Cave → Deep cave echo reverb
```

### Visualizer Modes

#### Bars Mode
```javascript
// Frequency analyzer with vertical bars
const analyser = audioContext.createAnalyser();
analyser.fftSize = 512;
const dataArray = new Uint8Array(analyser.frequencyBinCount);

// Draw 32 bars scaled to frequency data
for (let i = 0; i < bars.length; i++) {
  barHeight = dataArray[i * 3] / 255 * maxHeight;
  drawBar(x, barHeight);
}
```

#### Wave Mode
```javascript
// Oscilloscope-style waveform
const analyser = audioContext.createAnalyser();
analyser.fftSize = 2048;
const dataArray = new Uint8Array(analyser.frequencyBinCount);

// Draw continuous sine wave path
canvas.drawPath(dataArray.map(v => v / 255 * height));
```

#### Circle Mode
```javascript
// Radial frequency bars in circle
for (let angle = 0; angle < 360; angle += 360/32) {
  barHeight = frequencies[i];
  drawRadialBar(centerX, centerY, angle, barHeight);
}
```

#### Grid Mode
```javascript
// 2D frequency grid visualization
drawGrid(8x8);
for (each cell) {
  brightness = frequencies[cellIndex] / 255;
  drawCell(brightness);
}
```

---

## 💾 Data Persistence

### IndexedDB Schema
**Database Name:** `vibestream`  
**Version:** 1

**Object Stores:**
1. `songs` — Audio file data + metadata
2. `playlists` — Playlist definitions
3. `config` — App settings (theme, volume, EQ settings)
4. `history` — Recently played tracking

### Storage Operations
```javascript
// Save song
db.transaction('songs').objectStore('songs').add(songObj);

// Load all songs
const allSongs = await dbGet('songs');

// Update playlist
db.transaction('playlists', 'readwrite').objectStore('playlists').put(playlistObj);

// Delete song
db.transaction('songs', 'readwrite').objectStore('songs').delete(songId);
```

### Browser Storage Limits
- **Chrome/Edge:** 50% of available disk space
- **Firefox:** 10% of available disk space  
- **Safari:** ~50 MB per origin
- **Typical limit:** 50 MB - 1 GB+ depending on browser

### Backup Strategy
Data is automatically saved to IndexedDB after any modification. To backup:
1. Export songs via DevTools → Application → IndexedDB
2. Store JSON files in cloud storage
3. Import back by uploading files

---

## 🌐 Deployment

### Netlify (Current)
**Live:** https://vibestreamsong.netlify.app/

**Deployment Steps:**
```bash
# 1. Push to GitHub
git add .
git commit -m "Update Vibestream"
git push origin main

# 2. Netlify auto-deploys on push
# OR manually deploy via Netlify dashboard

# Build Command: (none required)
# Publish Directory: . (root)
```

### Deploy Elsewhere

#### GitHub Pages
```bash
# Push to gh-pages branch
git subtree push --prefix . origin gh-pages
# Access at: https://username.github.io/vibestream
```

#### Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Visit provided URL
```

#### Self-Hosted (Apache/Nginx)
```bash
# Copy files to web root
cp -r index.html style.css script.js /var/www/vibestream/

# Ensure proper MIME types:
# .js → application/javascript
# .css → text/css
# .html → text/html
```

**Required Headers (for CORS audio):**
```
Access-Control-Allow-Origin: *
Cross-Origin-Resource-Policy: cross-origin
```

---

## 👨‍💻 Development Guide

### Project Setup for Contributors

#### Prerequisites
```bash
# Install Git
git --version

# Install a modern browser (Chrome, Firefox, Safari, Edge)
```

#### Clone & Run
```bash
git clone https://github.com/s0ura8hs/vibestream.git
cd vibestream
python -m http.server 8000
# Open http://localhost:8000
```

### Code Organization

#### HTML (index.html)
- Lines 1-52: Header with navigation & search
- Lines 54-73: Sidebar with playlists
- Lines 76-216: Main views (home, library, search, details)
- Lines 219-260: Player footer
- Lines 263-420: Modals & overlays
- Lines 422-429: Script includes

#### CSS (style.css)
- Lines 1-85: Root variables & grain overlay
- Lines 87-240: Top bar styles
- Lines 241-304: Sidebar styles
- Lines 305-556: Main content & views
- Lines 558-657: Player styles
- Lines 659-787: Panels (EQ & queue)
- Lines 789-845: Ambient mode
- Lines 847-952: Modals & context menu
- Lines 953-984: Utilities & scrollbars
- Lines 985-1000: Responsive media queries

#### JavaScript (script.js)
- Lines 1-100: Utility functions & helpers
- Lines 101-180: Database initialization
- Lines 181-300: File upload & metadata extraction
- Lines 301-600: Playback engine (play, pause, next, seek)
- Lines 601-900: UI event handlers
- Lines 901-1200: Library & playlist management
- Lines 1201-1400: Search & view switching
- Lines 1401-1600: Visualizer & ambient mode
- Lines 1601-1800: Audio effects & EQ
- Lines 1801-2016: Initialization & state setup

### Common Tasks

#### Add a New EQ Preset
```javascript
// In script.js, find the presets object and add:
presets: {
  // ... existing presets
  custom: [2, -1, 0, 1, 3] // Your values
}

// Update HTML to add button:
// <button data-preset="custom" class="chip">Custom</button>
```

#### Add a New Visualizer Mode
```javascript
// In script.js, create draw function:
function drawVisualizerCustom() {
  const data = getFrequencyData();
  ctx.clearRect(0, 0, W(), H());
  // Your drawing logic
}

// Update the render loop:
case 'custom': drawVisualizerCustom(); break;

// Add HTML button:
// <button class="vchip" data-vmode="custom">Custom</button>
```

#### Customize Theme Colors
```css
/* In style.css, update :root variables */
:root {
  --accent: #your-color;        /* Primary color */
  --accent-2: #your-color-2;    /* Secondary */
  /* ... update other colors */
}
```

### Testing Checklist
- [ ] Upload various audio formats (mp3, wav, ogg, m4a, flac)
- [ ] Verify metadata extraction (title, artist, album)
- [ ] Test all playback controls
- [ ] Check all EQ presets
- [ ] Test all spatial effects
- [ ] Verify all visualizer modes
- [ ] Toggle theme switching
- [ ] Create/delete playlists
- [ ] Test search functionality
- [ ] Verify voice search (microphone access)
- [ ] Check responsive layout on mobile
- [ ] Ensure data persists after refresh
- [ ] Test context menu actions
- [ ] Verify toast notifications appear

### Performance Tips
1. **Limit visualizer rendering** when not in focus
2. **Cache frequently accessed DOM elements** using `const $ = sel => document.querySelector(sel)`
3. **Debounce search input** to reduce DOM updates
4. **Compress audio files** before upload (< 5 MB per file)
5. **Clear old history** to prevent IndexedDB bloat

### Browser DevTools Tips
```javascript
// In Console, access app state
state

// Check database
indexedDB.databases()

// Monitor playback
audio.onplay = () => console.log('Playing:', state.currentSongId)

// Debug EQ
console.log(eqBands.map(b => b.frequency.value))

// View all songs
dbGet('songs').then(songs => console.table(songs))
```

---

## 🐛 Known Limitations

1. **Browser Storage:** Audio files count toward IndexedDB quota (typically 50 MB - 1 GB)
2. **Audio Format Support:** Depends on browser codec support
3. **Mobile Performance:** Intensive visualizers may impact battery on older devices
4. **Web Audio Delay:** Slight latency between play button click and actual playback
5. **Metadata Extraction:** jsmediatags may fail on corrupt/unusual tag formats
6. **Spatial Effects:** Convolver node uses pre-recorded impulse responses (limited to 6 options)

---

## 🚀 Future Enhancements

- [ ] **Cloud Sync:** Sync library across devices via Google Drive/OneDrive
- [ ] **Import/Export:** Backup & restore playlists as JSON
- [ ] **Keyboard Shortcuts:** Full keyboard navigation (Space to play, Arrow keys to seek, etc.)
- [ ] **Advanced Search:** Filter by date added, play count, duration
- [ ] **Smart Playlists:** Auto-generated based on rules
- [ ] **Last.fm Integration:** Scrobble listening history
- [ ] **Lyrics Display:** Show lyrics while playing (via Genius API)
- [ ] **Recording:** Capture audio output to WAV/MP3
- [ ] **Normalization:** Audio loudness normalization across tracks
- [ ] **Multiple Queue Modes:** Shuffle variations, repeat smart modes
- [ ] **Browser Extension:** Control from another tab

---

## 📞 Support & Feedback

- **GitHub Issues:** [Report bugs or request features](https://github.com/s0ura8hs/vibestream/issues)
- **GitHub Discussions:** [Ask questions & share ideas](https://github.com/s0ura8hs/vibestream/discussions)
- **Email:** Contact repository owner for inquiries

---

## 📄 License

This project is open source. Check the repository for license details.

---

## 🙏 Acknowledgments

- **jsmediatags** — For audio metadata extraction
- **Font Awesome** — For beautiful icons
- **Google Fonts** — For typography
- **Web Audio API** — Core audio processing
- **IndexedDB** — Persistent local storage

---

## 📊 Project Stats

| Metric | Value |
|--------|-------|
| **Total Lines of Code** | ~2,400+ |
| **Languages** | HTML, CSS, JavaScript |
| **External Dependencies** | 2 (jsmediatags, Font Awesome) |
| **Database Type** | IndexedDB (Browser) |
| **Supported Audio Formats** | mp3, wav, ogg, m4a, flac |
| **Dark/Light Themes** | 2 |
| **Visualizer Modes** | 4 |
| **Spatial Effects** | 6 |
| **EQ Presets** | 7 |
| **Responsive Breakpoints** | 3 |

---

**Last Updated:** May 17, 2026  
**Version:** 1.0.0  
**Status:** ✅ Active & Maintained

🎵 **Enjoy your music!** 🎵
