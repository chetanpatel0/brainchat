# BrainChat

> **Private messaging, public groups & calls — powered by Nostr.**

BrainChat is a free, open, privacy-first messenger. No phone numbers. No emails. No sign-ups. No company database. Your identity is a cryptographic keypair generated on your device — and your messages are end-to-end encrypted before they ever leave it.

[![PWA Ready](https://img.shields.io/badge/PWA-Ready-blueviolet)](#-install-as-an-app)
[![Nostr](https://img.shields.io/badge/Nostr-NIP--17%20·%20NIP--29%20·%20NIP--46-1c84c6)](#-how-it-works)
[![End-to-End Encrypted](https://img.shields.io/badge/E2E-Encrypted-brightgreen)](#-privacy-first)
[![Static Hosting](https://img.shields.io/badge/Hosting-Any%20static%20host-orange)](#-hosting)
[![Iroh Calls](https://img.shields.io/badge/Calls-Iroh%20P2P-e6521f)](#-features)

**Live:** [BrainChat](https://brainchat.puter.site/)
**Repository:** [https://github.com/chetanpatel0/brainchat/](https://github.com/chetanpatel0/brainchat/)

---

## Table of Contents

- [Highlights](#-highlights)
- [How It Works](#-how-it-works)
- [Features](#-features)
- [Privacy First](#-privacy-first)
- [Install as an App (PWA)](#-install-as-an-app-pwa)
- [For New Users](#-for-new-users)
- [Bots](#-bots)
- [Browser & Device Support](#-browser--device-support)
- [Project Structure](#-project-structure)
- [Credits](#-credits)

---

## Highlights

- **Zero servers of our own** — the app is 100% static; your browser *is* the computer
- **End-to-end encrypted DMs** — gift-wrapped (NIP-17 + NIP-44); relays can never read them
- **Public & private groups** — NIP-29 rooms with invites, members, moderation & recovery
- **Audio / video / screen-share calls** — peer-to-peer, powered by Rust→WASM (Iroh)
- **No accounts** — login with a key you own: local key, browser extension (NIP-07), or remote signer (NIP-46)
- **21 languages** built in
- **Installable PWA** — works offline as an app on Android, iOS, Windows, macOS, Linux

---

## How It Works

```
┌─────────────────────┐      encrypted event       ┌──────────────────────┐
│   Your Browser      │ ─────────────────────────▶ │  Public Nostr Relays  │
│  (BrainChat app)    │                            │  (dumb mailboxes)     │
│                     │ ◀───────────────────────── │  cannot read content  │
│ • keys (yours only) │      encrypted event       └──────────────────────┘
│ • chats (IndexedDB) │
│ • crypto (WASM/JS)  │      P2P media stream      ┌──────────────────────┐
│                     │ ◀───────────────────────▶ │  Other users' browsers │
└─────────────────────┘      (WebRTC / Iroh)       └──────────────────────┘
```

1. **Puter (or any static host)** only serves the app *files* — like hosting a program, not a service.
2. Your **browser** runs everything: key generation, encryption, signing, UI.
3. Messages go out as **encrypted events** to public Nostr relays you choose. Relays store and forward sealed envelopes — they hold no keys.
4. Calls connect **browser-to-browser** (peer-to-peer). Iroh relays only help introductions; media never detours through a central server.
5. Everything personal (keys, contacts, chat cache, settings) stays in your device's **IndexedDB / localStorage**.

No database. No API server. No signup system. If a relay goes down, add another — there are hundreds, and you own the switch.

---

## Features

### Messaging
- Private 1:1 chats with **end-to-end encryption** (NIP-17 gift wraps, NIP-44 payloads)
- **Public groups** & **private groups** (NIP-29) with invites, member lists & moderation
- Replies, emoji reactions, pin/unpin, forward, copy, delete (NIP-09)
- Markdown & spoilers in messages
- Message requests & inbox review, pending chats
- Block & mute users
- Group history sync (Negentropy) — catch up even after being offline
- Contacts list synced to relays (NIP-51) + NIP-65 relay lists

### Calls
- 1:1 and group **audio & video calls**
- **Screen sharing**, pop-out presentation window
- Mute mic/speaker, camera toggle, device selection
- Gallery & speaker views, call history, minimize

### Identity & Profiles
- Create an account in seconds (key + **12 recovery words**)
- Import an existing key (nsec), login via **browser extension** (NIP-07) or **remote signer / bunker** (NIP-46)
- Download account secret backup
- Profiles: name, avatar, banner, about, website, NIP-05, Lightning address (LUD16/LUD06)

### Extras
- Custom relay management with live status
- Media sharing via Blossom servers with **trusted-sender protection**
- QR code scanning to add contacts
- Browser notifications
- Dark theme, responsive design (phone → desktop)
- **21 languages**
- Join links for chats & calls with rich previews

---

## Privacy First

| Your data | Where it lives |
|---|---|
| Private key | **Your device only** |
| Chats & contacts | **Your device** (IndexedDB) |
| Message content | End-to-end encrypted — relays see ciphertext only |
| Analytics / trackers | **None. Zero.** |
| Accounts / emails / phone | **Never collected** |

- Strict Content-Security-Policy blocks all third-party scripts
- `no-referrer` on every request
- No captcha, no telemetry, no fingerprinting scripts
- Media uploads are intentionally public (the app warns you before upload)

**Honest limits:** relays can observe *metadata* (who talks to whom, when, IP addresses) — that's the nature of any communication network. For maximum anonymity, use Tor/browser hardening and your own or trusted relays.

---

## Install as an App (PWA)

1. Open **[brainchat](https://brainchat.puter.site/)** in Chrome / Edge / Firefox (or Safari on iOS)
2. Tap **Install** in the address bar (or *Add to Home Screen* on iOS)
3. BrainChat runs full-screen, offline-capable, with its own icon

> Messages notify you while the app is open or recently closed. True background push (like native apps, app fully closed) requires an external Web Push service — not included in this static build.

---

**Upload rules:**
1. Upload the **entire folder structure** (`_app/`, `join/`, `pwa/`, `social/`, `fonts/`, `iroh/`, `licenses/` + root files) — do not flatten folders
2. Serve over **HTTPS** (required for camera/mic, WASM calls, and service worker)
3. That's it — no backend, no environment variables, no database

---

## Bots

BrainChat speaks plain Nostr — **any Nostr bot works**. A bot is simply another keypair that automates DMs (kind-14 gift wraps), so it works with BrainChat without any special integration.

Run it with your own key, point it at the same public relays, and users can DM it like any other contact.

---

## Browser & Device Support

| Platform | Status |
|---|---|
| Chrome / Edge (desktop & Android) | ✅ Full — calls, PWA, notifications |
| Firefox (desktop & Android) | ✅ Full |
| Samsung Internet | ✅ Full |
| Safari (macOS) | ✅ Full |
| Safari (iOS / iPadOS) | ✅ Chat, groups, PWA — calls supported with WebRTC limits |
| Desktop OS (Win/macOS/Linux) | ✅ Installable as app |

Requires a modern browser with **WebAssembly**, **IndexedDB**, and **WebRTC** (all standard since ~2020).

---

## Project Structure

```
brainchat/
├── index.html              # App entry (SPA)
├── manifest.webmanifest    # PWA manifest
├── service-worker.js       # Offline cache & navigation shell
├── _app/immutable/         # Compiled app (JS chunks, CSS, crypto worker)
├── iroh/                   # Rust→WASM call transport (Iroh)
├── join/                   # Static chat/call invite landing pages
├── pwa/                    # App icons (192 / 512)
├── social/                 # Social preview images & join pages
├── fonts/                  # Self-hosted variable fonts
└── licenses/               # Third-party licenses
```

---

## Credits

- Calls powered by **[Iroh](https://iroh.nostr.com/)** (n0-computer)
- Protocol: **[Nostr](https://nostr.com/)** — NIP-01, 05, 07, 09, 17, 19, 24, 29, 44, 46, 51, 59, 65
- Fonts: Space Grotesk & Manrope (OFL)

**Creator & Maintainer:** [Chetan Patel](https://github.com/chetanpatel0/)

---

## License

Based on Apache-2.0 Third-party component licenses are preserved in [`licenses/`](./licenses) and [`iroh/THIRD_PARTY_NOTICES.txt`](./iroh/THIRD_PARTY_NOTICES.txt).

---

<p align="center">
  <b>BrainChat</b> — your keys, your chats, your rules.<br/>
  <a href="https://brainchat.puter.site/">brainchat</a> ·
  <a href="https://github.com/chetanpatel0/brainchat/">GitHub</a>
</p>
