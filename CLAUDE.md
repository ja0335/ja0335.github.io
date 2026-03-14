# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page Danish language learning application ("Danish Verb Master 🇩🇰") that runs entirely client-side with no build process. The root `index.html` simply redirects to `/danish/` where the main app lives.

## Architecture

**Single-file structure:** The entire application (`danish/index.html`) contains:
- Embedded CSS using CSS variables for theming (light/dark mode)
- Vanilla JavaScript with no external frameworks
- Inline event handlers and DOM manipulation

**External API integrations:**
- **Gemini (Google)** - LLM for generating Danish sentences, conversations, and translations
- **ElevenLabs** - TTS (text-to-speech) provider with Danish voices
- **MiniMax Audio** - Alternative TTS provider with Danish voices

**Data persistence:**
- All settings saved to `localStorage` (API keys, model selections, speed sliders, theme, last inputs)
- Generated content cached in `localStorage` for quick reload
- Audio cached in memory (`audioCache` object) plus localStorage for MiniMax URLs

**Tab system:** Four main sections controlled via `showTab()`:
- `apikeys` - Settings/API configuration
- `verbs` - Verb Master (generate sentences using specific verb + tense)
- `conversation` - Generate dialogues between two speakers
- `composer` - Write Danish text, translate to English, listen to TTS

## Key Technical Details

- **CSS variables:** All theming uses `--bg-color`, `--card-bg`, `--text-main`, etc. Dark mode toggles `data-theme="dark"` on body
- **TTS routing:** `speak()` function routes to either `speakElevenLabs()` or `speakMiniMax()` based on `ACTIVE_TTS()` selection
- **Voice synchronization:** All three voice selects (`voiceSelectVerbs`, `voiceSelectConv`, `voiceSelectComposer`) stay in sync and save per-provider voice preference
- **Speed control:** Playback rate controlled via `<input type="range">` sliders (0.5-1.0×) applied via `audio.playbackRate`
- **Hide-text feature:** CSS blur effect on Danish text, click to reveal temporarily for listening comprehension practice
- **Debug panel:** Collapsible `<details>` element showing raw API requests/responses with Matrix animation

## File Structure

```
ja0335.github.io/
├── index.html           # Redirects to /danish/
└── danish/
    └── index.html       # Main application (single file)
```

## No Build Process

This is a static site with no npm, build tools, or testing framework. Files can be edited directly and served from any web server or GitHub Pages.

## Danish Tense Descriptions

The Verb Master uses five Danish tenses with specific context:
- **Infinitiv** - "Potential" - the raw verb form, idea/intent
- **Nutid** - "Habit" - present tense, especially for habitual actions
- **Datid** - "History" - past tense at specific point in time
- **Førnutid** - "Result" - past tense emphasizing result/completion
- **Imperativ** - "Command" - direct order or instruction
