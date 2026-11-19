<div align="center">

<img src="docs/aso/screenshots/en-US-v2/feature_graphic_en_v2.png" alt="Bookble — Read Timeless Stories & Audio" width="100%" />

<br />
<br />

# Bookble.

**Your Pocket Library of Timeless Classics — 36,900+ Free Books, Offline Neural AI Read Aloud, Radio Drama & Curated Summaries.**

<p>
  <img src="https://img.shields.io/badge/Kotlin-2.2.0-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white" alt="Kotlin 2.2.0" />
  <img src="https://img.shields.io/badge/Compose_Multiplatform-1.9.0-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white" alt="Compose Multiplatform" />
  <img src="https://img.shields.io/badge/Android-API_24%2B_(Target_36)-3DDC84?style=for-the-badge&logo=android&logoColor=060608" alt="Android API 24+" />
  <img src="https://img.shields.io/badge/Languages-13_UI_%2F_24_Locales-C9FF00?style=for-the-badge" alt="13 UI Languages" />
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-GPL_v3-blue?style=for-the-badge" alt="GPL v3 License" /></a>
</p>

<p>
  <a href="#-features">Features</a> •
  <a href="#-screenshots">Screenshots</a> •
  <a href="#%EF%B8%8F-architecture">Architecture</a> •
  <a href="#%EF%B8%8F-tech-stack">Tech Stack</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-localization--aso">Localization</a> •
  <a href="#-license">License</a>
</p>

</div>

---

## ✨ Overview

**Bookble** (`net.bookble.app`) is an ad-free, multiplatform digital library, customizable EPUB reader, audiobook & radio drama player, and offline neural AI voice companion built with **Kotlin & Compose Multiplatform**.

Designed for calm, distraction-free reading anywhere — with or without Wi-Fi — Bookble pairs thousands of curated public-domain classics, Bookble Originals, and poetry collections with structured book summaries, offline text-to-speech (`Phonon` + `Supertonic`), reading habit analytics, and 1-tap shareable Quote Cards.

---

## 🚀 Features

| Capability | Highlights |
| :--- | :--- |
| 📚 **36,900+ Classic Library** | Curated home feed, **Top 10 Charts**, genre shelves (*Thriller*, *Historical*, *Philosophy*, *Classics*), and rich author pages. |
| 📖 **Customizable EPUB Reader** | Powered by `readest-engine` & `reader-domain` — custom fonts, spacing, margins, dark/sepia/light themes, bookmarks, highlights, notes, and TOC. |
| 🎙️ **Offline AI Read Aloud (TTS)** | Built-in offline neural TTS (`Phonon` English + `Supertonic` across 31 languages) plus cloud voices with adjustable speed and background playback. |
| 🎧 **Audiobooks & Radio Drama** | Circular media player backed by Media3 (`core:playback`), chapter navigation, variable speed, background play, and built-in sleep timer. |
| 📝 **Immersive Book Summaries** | Spoiler-free teasers, key takeaways, reading-time estimates, and seamless jump-to-full-edition navigation. |
| 💬 **1-Tap Quote Maker** | Turn any passage into styled shareable cards (*Olive*, *Sand*, *Paper*, *Current*) with instant export to WhatsApp, Instagram, X, Telegram & Gmail. |
| 📊 **Offline Bookshelf & Tracker** | Download books for flights or commutes, filter shelves (*All / Reading / Not started / Completed*), and track daily streaks, minutes, and WPM. |

---

## 📱 Screenshots

<div align="center">

| Discovery | Charts, Authors & Genres | Book Details & Summaries | Reader & AI Read Aloud |
| :---: | :---: | :---: | :---: |
| <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_1_discovery.png" width="210" alt="Discovery" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_2_top_genres.png" width="210" alt="Top Charts & Authors" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_3_book_details.png" width="210" alt="Book Details" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_4_reader_tts.png" width="210" alt="Reader & AI Voice" /> |

| Radio Drama & Audio | Offline Library | Shareable Quote Cards | Reading Tracker |
| :---: | :---: | :---: | :---: |
| <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_5_audiobooks.png" width="210" alt="Audiobooks & Radio Drama" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_6_library.png" width="210" alt="Offline Library" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_7_quotes.png" width="210" alt="Shareable Quote Cards" /> | <img src="docs/aso/screenshots/en-US-v2/en_v2_slide_8_tracker.png" width="210" alt="My Bookble Tracker" /> |

</div>

---

## 🏛️ Architecture

Bookble follows a modular **Clean / Multi-module Architecture** separating Compose Multiplatform UI screens, navigation, domain interactors, Room/SQLite persistence, Supabase & Firebase services, and the native EPUB/TTS engines:

```mermaid
flowchart TB
    subgraph Targets["App Targets"]
        APP[":app (Android)"]
        IOS[":iosApp (iOS)"]
        DESK[":desktop-app (JVM Desktop)"]
    end

    subgraph Presentation["Shared UI & Navigation"]
        SH[":shared:common / :shared:prod"]
        NAV[":navigation & :ui:root"]
        UIS[":ui:homepage · :ui:search · :ui:library · :ui:item:* · :ui:summary · :ui:playback · :ui:profile · :ui:author · :ui:auth"]
    end

    subgraph Domain["Domain Layer"]
        DOM[":domain & :base:domain"]
        RDOM[":reader-domain"]
    end

    subgraph EnginesAndData["Data, Playback & Native Engines"]
        DATA[":data:repo · :data:database (Room) · :data:models · :data:prefs"]
        CORE[":core:networking · :core:downloader · :core:playback · :core:image-loading · :core:analytics · :core:remote-config"]
        READ["readest-engine (Next.js / EPUB + Phonon ONNX TTS)"]
    end

    APP --> SH
    IOS --> SH
    DESK --> SH
    SH --> NAV --> UIS
    UIS --> DOM
    UIS --> RDOM
    DOM --> DATA
    RDOM --> READ
    DATA --> CORE
```

### Module Directory

<details>
<summary><b>Click to expand full Gradle module map (from <code>settings.gradle</code>)</b></summary>

| Layer | Modules |
| :--- | :--- |
| **Targets & Shared** | `:app`, `:desktop-app`, `:desktop-kms`, `:baselineprofile`, `:shared:common`, `:shared:prod` |
| **Navigation & Theme** | `:navigation`, `:ui:root`, `:ui:theme`, `:ui:common`, `:ui:components`, `:ui:webview` |
| **Feature UI** | `:ui:homepage`, `:ui:search`, `:ui:library`, `:ui:item:detail`, `:ui:item:reviews`, `:ui:summary`, `:ui:playback`, `:ui:author`, `:ui:profile`, `:ui:auth` |
| **Domain & Reader** | `:domain`, `:reader-domain`, `:readest-engine`, `:base:domain`, `:base:annotations` |
| **Data** | `:data:repo`, `:data:database`, `:data:models`, `:data:platform`, `:data:prefs` |
| **Core Infrastructure** | `:core:networking`, `:core:downloader`, `:core:playback`, `:core:image-loading`, `:core:play`, `:core:purchase`, `:core:analytics`, `:core:remote-config` |

</details>

---

## 🛠️ Tech Stack

| Category | Technology / Library |
| :--- | :--- |
| **Language & UI** | Kotlin `2.2.0`, Compose Multiplatform `1.9.0-beta01`, Material 3 Adaptive `1.1.2`, Haze `1.6.9`, Lottie `6.6.7` |
| **Navigation & DI** | JetBrains Navigation Compose `2.8.0-alpha13`, Kotlin-Inject `0.7.2`, Coroutines `1.10.2` |
| **Persistence** | AndroidX Room `2.7.2`, SQLite Bundled `2.5.2`, Multiplatform Settings `1.3.0` |
| **Networking & Media** | Ktor `3.2.2`, OkHttp `5.1.0`, Coil 3 `3.3.0`, AndroidX Media3 `1.6.1`, FileKit `0.8.8` |
| **Backend & Cloud** | Supabase KT `3.2.6`, Firebase BOM `33.13.0` (Analytics, Crashlytics, Remote Config, Messaging), Credential Manager |
| **Reader & AI Audio** | Embedded `readest-engine` (`_next` static chunks bundled in APK assets), `libonnxruntime` (`phonon` offline neural TTS) |
| **Quality & Performance** | Spotless `7.2.1`, Baseline Profiles `1.3.4`, Maestro UI flows (`maestro/`) |

---

## ⚙️ Getting Started

### Prerequisites
- **JDK 17** (`JavaVersion.VERSION_17`)
- **Android Studio** (supporting AGP `8.11.1`, `compileSdk 36`, `minSdk 24`, NDK `28.2.13676358`)
- Valid `local.properties` and Firebase `google-services.json` (plus `keystore.jks` & `RELEASE_SIGNING_PASSWORD` env var for release builds)

### Common Build Commands

```bash
# Build Android Debug APK
./gradlew :app:assembleDebug

# Build slim arm64-v8a-only APK for physical 64-bit phones (ships single ONNX Runtime ABI)
./gradlew :app:assembleDebug -PbookbleArm64Only=true

# Run code formatting verification
./gradlew spotlessCheck

# Apply Spotless formatting
./gradlew spotlessApply
```

---

## 🌍 Localization & ASO

Bookble supports **13 in-app UI languages** across **24 Google Play Console locales** (`en-US/GB/CA/AU/IN/SG/ZA`, `ar`, `es-ES/419/US`, `pt-BR/PT`, `fr-FR/CA`, `de-DE`, `it-IT`, `ru-RU`, `ja-JP`, `pl-PL`, `nl-NL`, `fi-FI`, `zh-TW/HK`, `zh-CN`):

- 📄 **Store Listings (24 Locales):** [`docs/aso/google-play-multilang-listings.md`](docs/aso/google-play-multilang-listings.md)
- 👤 **Developer Page Promotional Copy:** [`docs/aso/google-play-developer-page-texts.md`](docs/aso/google-play-developer-page-texts.md)
- 🎨 **Play Store Creative Pack:** [`docs/aso/screenshots/en-US-v2/`](docs/aso/screenshots/en-US-v2/) (Screenshots, Feature Graphic, Developer Cover `4096×2304`, Promo Reel)

---

## 📄 License

Bookble is licensed under the **GNU General Public License v3.0 (GPL-3.0)** — see the [LICENSE](LICENSE) file for full details.

<div align="center">
  <br />
  <img src="docs/aso/screenshots/en-US-v2/developer_cover_4096x2304.png" alt="Bookble Developer Showcase" width="100%" />
</div>
