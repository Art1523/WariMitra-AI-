```
 ╔═══════════════════════════════════════════════════════════╗
 ║                    🎤 WariMitra AI 🎤                    ║
 ║          "One Call. Every Warkari." - वारी मदत         ║
 ╚═══════════════════════════════════════════════════════════╝
```

> **Voice-first AI assistance** for a safer, smarter, and more accessible Pandharpur Wari pilgrimage.
> 
> *Designed for Warkaris. Built for India. Open source. Community-driven.*

[![Prototype Badge](https://img.shields.io/badge/status-prototype-yellow)](https://github.com)
[![Open Source](https://img.shields.io/badge/open-source-green)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3+-blue)](https://www.typescriptlang.org/)
[![License MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![TanStack Start](https://img.shields.io/badge/powered%20by-TanStack%20Start-purple)](https://tanstack.com/router/latest)

## 🌟 What is WariMitra AI?

**WariMitra AI** (वारीमित्र) is a **voice-first AI assistant** built specifically for Warkaris — pilgrims undertaking the sacred Pandharpur Wari pilgrimage. 

No smartphone required. No app to install. Just **one call in Marathi** to get:
- 🏥 Nearest medical facilities, doctors, pharmacies
- 💧 Water stations and essential amenities  
- 🚻 Toilets and sanitation facilities
- 🍲 Food & shelter locations
- 🚨 Missing Warkari alerts & community updates

**The assistant speaks your language.** Responds in Marathi with directions, real-time help, and community intelligence.

### ✨ Core Features

| Feature | Description |
|---------|-------------|
| 🎤 **Voice Interface** | Designed for feature phones — no app, no download needed |
| 🇮🇳 **Marathi-First** | Understands Marathi, Hindi, English. Responds in your language |
| 🏥 **Smart Facility Finder** | Real-time location of medical, water, toilet, food & shelter |
| 🚨 **Missing Person System** | Report missing Warkaris, receive alerts within 100km |
| 📍 **Location-Aware** | Geo-fenced alerts & distance-based recommendations |
| 🤖 **AI-Powered** | Google Gemini NLU for natural, human-like conversations |
| 📢 **Community Updates** | Broadcast alerts, route changes, safety notices |
| 💾 **Works Offline** | localStorage-based state, no internet required for core features |

### 🧪 Prototype Status

> ⚠️ **This is a working prototype & proof-of-concept.**

The current version includes **demo/mock data** for demonstration purposes. Real-world deployment will need:

| Aspect | Current Status | Production Need |
|--------|---|---|
| **Emergency Integration** | ❌ Mock only | Real emergency dispatch systems |
| **GPS/Location** | 📍 Browser-based (demo estimates) | Actual device location + privacy |
| **Telephone Line** | 📞 Browser microphone only | IVR + GSM/telecom integration |
| **Data Persistence** | 💾 localStorage | Backend database (PostgreSQL/Firebase) |
| **Facility Database** | 🗂️ Mock data | Real, live facility records |
| **Missing Person System** | 📋 Demo scenarios | Government agency integration |

**See [Production Deployment](#deployment) section for roadmap.**

---

## Quick Start

### Prerequisites

- **Node.js** 18+ and npm (or yarn/bun)
- **Chrome or Edge** browser (for Web Speech API support)
- **Optional**: Google Gemini API key (for AI responses)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd wari-mitra-voice

# Install dependencies
npm install

# Start development server
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

### Environment Setup

By default, the app works without any API keys using a secure server-side proxy.

**For frontend Gemini calls (demo only):**

```bash
# Copy the example environment file
cp .env.example .env

# Add your Gemini API key
VITE_GEMINI_API_KEY=your_api_key_here
```

> ⚠️ **Security Warning**: Frontend API keys expose your credentials. This setup is for **local development and demos only**. Production deployments must use a secure backend proxy.

---

## 🏗️ How It Works

### The Voice Call Flow

```
┌─────────────────────┐
│  Browser Microphone │
└──────────┬──────────┘
           │ (speech-to-text)
           ▼
┌─────────────────────────┐
│  Web Speech API         │
│  (Chrome/Edge only)     │
└──────────┬──────────────┘
           │ (Marathi text)
           ▼
┌──────────────────────────┐
│  Gemini NLU              │
│  • Intent detection      │
│  • Entity extraction     │
│  • Location landmarks    │
└──────────┬───────────────┘
           │ (structured data)
           ▼
┌──────────────────────────────┐
│  facilityService             │
│  • Search mock database      │
│  • Calculate distances       │
│  • Verify facility status    │
└──────────┬───────────────────┘
           │ (facility record)
           ▼
┌───────────────────────────────┐
│  Gemini Response Generation   │
│  • Natural Marathi answer     │
│  • Directions & instructions  │
└──────────┬────────────────────┘
           │ (Marathi speech text)
           ▼
┌──────────────────────┐
│  Text-to-Speech      │
│  window.speechSynthesis
│  (mr-IN or hi-IN)    │
└──────────┬───────────┘
           │ (audio output)
           ▼
    🔊 Speaker Output
```

**🔒 Trust & Safety Principle**: Gemini AI never decides if a facility exists. The system always:
1. Searches the **verified facility database**
2. Confirms the facility is real with current status
3. Passes only confirmed data to Gemini for wording
4. Delivers factual information with natural language

This prevents AI hallucinations and ensures users get real, verified help.

### Project Structure

```
src/
├── components/              # React UI components
│   ├── ui/                 # Radix UI component library
│   ├── ArchitectureFlow.tsx # System architecture visualization
│   ├── LocationPicker.tsx   # Map-based location selection
│   ├── MapPanel.tsx        # Google Maps integration
│   └── DemoModeToggle.tsx   # Demo/production mode switcher
├── routes/                  # TanStack Router pages
│   ├── index.tsx           # Landing page
│   ├── voice.tsx           # Main voice call interface
│   ├── facilities.tsx       # Facility browser
│   ├── community.tsx        # Missing persons & announcements
│   └── emergency.tsx        # Emergency features
├── services/               # Business logic (no UI dependencies)
│   ├── voiceService.ts    # Call recording & demo scenarios
│   ├── facilityService.ts # Facility search & categorization
│   ├── geminiService.ts   # NLU & response generation
│   ├── locationService.ts # Geocoding & distance calculations
│   ├── missingPersonService.ts # Missing person alerts
│   ├── placesService.ts   # Google Places API integration
│   ├── announcementService.ts  # Broadcast messages
│   ├── distanceService.ts # Geo calculations
│   └── storage.ts         # localStorage abstraction
├── hooks/                  # React hooks
│   ├── useCallEngine.ts   # Main voice call state machine
│   ├── useSpeech.ts       # Web Speech API wrapper
│   └── useStore.ts        # Zustand store setup
├── lib/                    # Utilities & helpers
│   ├── ai.functions.ts    # Server-side Gemini proxy
│   ├── maps.functions.ts  # Google Maps/Places API wrapper
│   ├── utils.ts           # General utilities
│   └── error-capture.ts   # Error tracking
└── data/
    └── mockData.ts        # Demo facility, missing person, announcement data
```

---

## Features in Detail

### 🎤 Voice Interface

- **Web Speech API Integration**: Real-time speech-to-text in Marathi, Hindi, and English
- **Browser Support**: Chrome and Edge (desktop/mobile); Safari shows fallback UI
- **Demo Mode**: Pre-recorded scenario scripts for testing without speaking
- **Speech Synthesis**: Text-to-speech in Marathi with Hindi fallback

### 🏥 Facility Finder

Search by category or natural language:
- **Medical**: Hospitals, clinics, pharmacies, doctors
- **Essential Services**: Water, toilets, shelter, charging stations
- **Food**: Restaurants, meal services, food courts
- **Government/Safety**: Police, local government offices

Mock database includes realistic facilities around Pandharpur with:
- Distance calculations
- Operating hours
- Accessibility information
- Emergency prioritization

### 🚨 Missing Person System

- Report missing Warkaris with photo, name, and description
- Automatic alerts within 100km radius
- Sighting reports from community
- Phone number masking for privacy

### 📢 Announcements

- Route updates
- Safety alerts
- Facility closure notices
- Community messages

---

## 💻 Development & Customization

### Available Scripts

```bash
npm run dev          # Start dev server (Vite hot reload)
npm run build        # Production build
npm run build:dev    # Development build
npm run preview      # Preview production build
npm run lint         # Check code with ESLint
npm run format       # Format with Prettier
```

### Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | TanStack Start (full-stack React) |
| **Language** | TypeScript 5.3+ |
| **Styling** | Tailwind CSS 4 |
| **UI Components** | Radix UI + shadcn/ui |
| **Routing** | TanStack Router 1.170+ |
| **State** | Zustand |
| **AI/NLU** | Google Gemini API |
| **Maps** | Google Maps & Places API |
| **Build Tool** | Vite |
| **Package Manager** | Bun (bunfig.toml configured) |
| **Linting** | ESLint |
| **Formatting** | Prettier |

### Browser Support

| Feature | Chrome | Edge | Safari | Firefox |
|---------|--------|------|--------|---------|
| Voice Input (Web Speech API) | ✅ | ✅ | ⚠️ | ⚠️ |
| Voice Output (speechSynthesis) | ✅ | ✅ | ✅ | ✅ |
| Marathi `mr-IN` Voice | ✅ | ✅ | ⚠️ | ⚠️ |
| App Demo Mode | ✅ | ✅ | ✅ | ✅ |

For unsupported browsers, the app provides:
- Clear "speech not supported" messaging
- Full UI access through demo scenario buttons
- Text-based input/output fallbacks

### Data Persistence

All application state is managed through **`src/services/storage.ts`**, which abstracts localStorage:

```typescript
readKey<T>(key, fallback)   // Read from storage
writeKey<T>(key, value)     // Write to storage
clearKey(key)               // Remove a key
subscribe(key, callback)    // Watch for changes
```

This design allows replacing localStorage with a backend (FastAPI + PostgreSQL) without touching React components.

---

## API Configuration

### Google Gemini API

1. **Create an API key**:
   - Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
   - Create a new API key

2. **Add to environment**:
   ```bash
   VITE_GEMINI_API_KEY=sk-abc123xyz...
   ```

3. **In production**, move Gemini calls to a backend server:
   - See `src/lib/ai.functions.ts` for the server proxy implementation
   - Never expose API keys in frontend code

### Google Places & Maps API

1. **Enable APIs** in Google Cloud Console:
   - Maps JavaScript API
   - Places API
   - Geocoding API

2. **Create an API key** with restrictions to:
   - HTTP referrers (your domain)
   - APIs used (Places, Maps, Geocoding)

3. **Keys are embedded in code** (public API, not sensitive credentials)

---

## Configuration

### Mock Data

Edit `src/data/mockData.ts` to customize:
- Facilities database
- Missing persons
- Announcements
- Demo call scenarios

### Demo Scenarios

Edit `src/services/demoService.ts` to add new voice scenarios:

```typescript
export const DEMO_STEPS: DemoStep[] = [
  {
    id: "medical",
    name: "Medical Help",
    marathi: "वैद्यकीय मदत",
    transcript: "मुझे सिरदर्द है", // User's input
    intent: "medical",
  },
  // ... more scenarios
];
```

---

## Deployment

### Build for Production

```bash
npm run build
```

Output is in `dist/` — deploy to any static hosting:
- **Vercel** (recommended for TanStack Start)
- **Netlify**
- **AWS S3 + CloudFront**
- **GitHub Pages**

### Environment Variables

Set in your hosting platform:
- `VITE_GEMINI_API_KEY` (optional, for direct browser calls during demo)
- Backend API URL (if not using server functions)

### Important for Production

1. **Move Gemini calls to backend** — never expose API keys to clients
2. **Replace mock data** with real facility database
3. **Implement real missing person & announcement systems**
4. **Add authentication** if handling sensitive data
5. **Set up CORS** properly for API calls
6. **Enable HTTPS** for Web Speech API
7. **Consider SMS/IVR integration** for actual phone calls

---

## Troubleshooting

### Speech Recognition Not Working

**Problem**: "Speech is not available in your browser"

**Solution**:
- Use **Chrome or Edge** (latest version)
- Enable microphone permissions
- Ensure page is served over HTTPS (localhost OK)
- Check browser privacy settings

### Marathi Voice Not Available

**Problem**: Responses play in Hindi instead of Marathi

**Solution**:
- Install Marathi language pack on your OS
- Fallback to Hindi is automatic and acceptable
- Text output always shows in Marathi

### API Key Errors

**Problem**: "VITE_GEMINI_API_KEY is invalid"

**Solution**:
- Verify key in `.env` file
- Ensure `.env` is in root directory
- Restart dev server after changing `.env`
- Check key hasn't been revoked in Google AI Studio

### Build Errors

**Problem**: TypeScript or ESLint errors

**Solution**:
```bash
npm run lint    # See all issues
npm run format  # Auto-fix formatting
```

---

## 🤝 Contributing & Development Workflow

Contributions are welcome! Follow this workflow:

### 1️⃣ Local Development

```bash
# Fork and clone
git clone https://github.com/your-fork/wari-mitra-voice.git
cd wari-mitra-voice

# Create a feature branch
git checkout -b feature/amazing-feature

# Install & start dev server
npm install && npm run dev

# Make your changes
# Test locally, commit with clear messages
git commit -m "feat: add [feature] for [use case]"

# Push to your fork
git push origin feature/amazing-feature
```

### 2️⃣ Pull Requests

- Create a PR against `main` branch
- Include clear title and description of changes
- Reference related issues (e.g., `Fixes #123`)
- Keep commit history clean (no force pushes on shared branches)
- All tests and lints must pass

### 3️⃣ Code Style

- **TypeScript** strict mode enforced
- **Prettier** for formatting (`npm run format`)
- **ESLint** for code quality (`npm run lint`)
- Component-based architecture
- Services separate from UI components
- Comprehensive error handling & logging

---

## 📜 License

MIT License — See [LICENSE](LICENSE) for full details.

**Open source, free to use, modify, and redistribute.** Attribution appreciated but not required.

---

## 📚 About the Pandharpur Wari

**वारी (Wari)** is an annual pilgrimage where devotees walk to Pandharpur to visit the sacred Vithoba Temple. With 10+ million Warkaris and challenging conditions (heat, distance, medical needs), **WariMitra AI** aims to make the journey safer and more accessible.

This project is **community-driven** and open to contributions from developers, UX designers, medical professionals, and anyone passionate about improving pilgrim welfare.

### 🌍 Project Scope

- **Primary Language**: Marathi (मराठी)
- **Secondary Languages**: Hindi, English
- **Target Users**: Warkaris, volunteers, emergency responders, health workers
- **Focus**: Voice accessibility for feature phone users
- **Region**: Maharashtra, India
- **Vision**: Safe, equitable, and accessible pilgrimage experience

---

## 🔗 Resources & Links

### Documentation
- [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API) — Voice input/output
- [TanStack Start](https://tanstack.com/router/latest/docs/framework/react/start/overview) — Full-stack framework
- [Google Gemini API](https://ai.google.dev/) — AI conversation engine
- [Google Places API](https://developers.google.com/maps/documentation/places) — Facility search
- [Tailwind CSS](https://tailwindcss.com/) — Styling framework
- [Radix UI](https://www.radix-ui.com/) — Accessible components

### Community
- **GitHub**: Report issues, suggest features, contribute code
- **Questions?** Open an issue or start a discussion
- **Want to help?** See [Contributing](#contributing--development-workflow) section

---

<div align="center">

### Made with ❤️ for Warkaris

*By developers, for the community.*

**If this helps someone during their Wari, our mission is complete.** 🙏

</div>
