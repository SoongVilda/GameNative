# GameNative

<p align="center">
  <img src="app/src/main/ic_launcher-playstore.png" width="128" height="128" alt="GameNative Logo">
</p>

<p align="center">
  <strong>Play your PC games on Android — natively.</strong>
</p>

<p align="center">
  <a href="https://github.com/utkarshdalal/GameNative/releases"><img alt="Latest Release" src="https://img.shields.io/github/v/release/utkarshdalal/GameNative?style=flat-square&color=blue"></a>
  <a href="LICENSE"><img alt="License: GPL v3" src="https://img.shields.io/badge/License-GPLv3-blue.svg?style=flat-square"></a>
  <a href="https://discord.gg/2hKv4VfZfE"><img alt="Discord" src="https://img.shields.io/discord/1233792078236?style=flat-square&logo=discord&label=Discord&color=5865F2"></a>
  <a href="https://ko-fi.com/gamenative"><img alt="Support on Ko-fi" src="https://img.shields.io/badge/Support-Ko--fi-ff5e5b?style=flat-square&logo=ko-fi"></a>
</p>

GameNative is an advanced open-source Android application that lets you play PC games from **Steam**, **Epic Games**, **GOG**, and **Amazon Games** directly on your Android device. It combines a modern Kotlin/Jetpack Compose frontend with a powerful x86/x64 emulation backend — built on [Winlator](https://github.com/brunodev85/winlator), [Wine](https://www.winehq.org/), and [Box86/Box64](https://github.com/ptitSeb/box64) — to deliver a genuine desktop gaming experience in the palm of your hand.

[Playing Stray on Poco F6](https://github.com/user-attachments/assets/1870fd14-7de9-4054-ba92-d3a5c73686b5)

### 📸 Screenshots

<p align="center">
  <img src="media/play_store_odin_screenshot_library.png" width="220" alt="Game Library">
  <img src="media/play_store_odin_screenshot_search.png" width="220" alt="Game Search">
  <img src="media/play_store_odin_screenshot_presets.png" width="220" alt="Container Presets">
  <img src="media/play_store_odin_screenshot_default_config.png" width="220" alt="Default Config">
  <img src="media/play_store_odin_screenshot_app_noita.png" width="220" alt="Running Noita">
</p>

## 🚀 Getting Started

### System Requirements

| Requirement | Details |
|---|---|
| **Android Version** | 8.0 (Oreo / API 26) or higher |
| **Architecture** | `arm64-v8a` or `armeabi-v7a` |
| **GPU** | Qualcomm Adreno recommended (best driver support) |
| **Recommended SoCs** | Snapdragon 8 Gen 2, 8 Gen 3, 8 Elite, or newer |
| **Storage** | Varies by game — plan for 10–50 GB+ per title |

### Installation

1. Download the latest APK from the [**Releases**](https://github.com/utkarshdalal/GameNative/releases) page.
2. Install the APK (you may need to enable *Install from unknown sources*).
3. Open GameNative and log in to your preferred gaming services.
4. Browse your library, download a game, configure the container, and play!

> 💡 **Self-Update**: GameNative checks for new releases automatically, can download the APK in-app, and prompts you to install it — no need to manually visit GitHub.

## ✨ Features

### 🎮 Multi-Store Game Library

Authenticate and browse your complete game libraries from four major PC storefronts — all in one app:

| Store | Auth Method | Download Method | Cloud Saves |
|---|---|---|---|
| **Steam** | Steam Guard / QR code | [JavaSteam](https://github.com/Longi94/JavaSteam) + DepotDownloader | ✅ Steam Cloud & Auto Cloud (upload, download, conflict detection, file-pattern sync) |
| **Epic Games** | In-app OAuth WebView | Epic API | Planned |
| **GOG** | In-app OAuth WebView | GOG API | ✅ GOG Cloud |
| **Amazon Games** | Amazon PKCE OAuth | Amazon API | Planned |

- Automatically fetch beautiful cover art via [SteamGridDB](https://www.steamgriddb.com/) integration.
- Offline-first library management with full local caching.

### 🖥️ GPU Driver Management

GameNative ships with a curated catalog of **60+ downloadable GPU drivers** that users can swap at will:

- **Turnip** (Mesa Vulkan for Adreno) — versions `v24.2.0` through `v26.0.0`, including `_mem` memory-optimized builds.
- **Qualcomm proprietary** — `qcom-800.x` through `qcom-849`.
- **Adreno SDK** — `762`, `805`, `814`, `819` series.
- **Snapdragon 8 Elite** — dedicated driver builds (`800.21` – `842.8`).

All drivers are managed via the [`manifest.json`](manifest.json) catalog and downloaded on-demand.

### 🎛️ Controls & Display

- **Custom touch controllers** with remappable on-screen controls (gamepad, keyboard, and mouse overlays using [Kenney Input Prompts](https://kenney.nl/assets/input-prompts)).
- **External display support** — mirror or extend to an external screen.
- **Oculus Quest VR headset support** — free window resizing enabled for VR environments.
- **Per-game container configuration** — each game runs in its own isolated Wine container with independently configurable settings.

### 🔧 Game-Specific Fixes

GameNative automatically applies known compatibility fixes for specific titles via a built-in registry:

| Game | Steam | GOG | Epic | Fix Type |
|---|:---:|:---:|:---:|---|
| **Fallout 3** | ✅ | ✅ | ✅ | Registry key (`Installed Path`) |
| **Fallout: New Vegas** | ✅ | ✅ | ✅ | Registry key (`Installed Path`) |
| **The Elder Scrolls IV: Oblivion** | ✅ | ✅ | ✅ | Registry key (`Installed Path`) |
| **Moonlighter** | — | ✅ | — | MSVC 2017 dependency install |

> **Note**: Many more games work without needing any fixes — these are just titles with *dedicated* compatibility patches. The video above shows **Stray** running on a Poco F6.

### 🌍 Internationalization

Localized in **12 languages**:

`en` · `es` · `da` · `pt-BR` · `zh-TW` · `zh-CN` · `fr` · `de` · `uk` · `it` · `ro` · `pl`

## 🤝 Community & Support

- 💬 **Discord**: [Join our server](https://discord.gg/2hKv4VfZfE) for support, updates, and community discussion.
- 🐛 **Bug Reports**: Please report issues via Discord. GitHub Issues are reserved for tracked development milestones.
- ☕ **Support the Project**: Help keep development going on [Ko-fi](https://ko-fi.com/gamenative).

# Developer Guide

The sections below are intended for developers and contributors.

## 🚀 How It Works

GameNative runs Windows games through a sophisticated emulation stack:

```
┌───────────────────────────────────────────┐
│           Android (ARM64/ARMv7)           │
├───────────────────────────────────────────┤
│  Kotlin/Compose UI  ←→  Native C++ Layer │
├───────────────────────────────────────────┤
│   XServer (X11)    │   ALSA Audio Server  │
├────────────────────┼──────────────────────┤
│       Wine / Proton (Windows compat.)     │
├───────────────────────────────────────────┤
│  Box86/Box64  │  FEXCore  │  WOWBox64     │
│       (x86/x64 → ARM translation)        │
├───────────────────────────────────────────┤
│    virglrenderer   │   DXVK / D3D→VK     │
│       (OpenGL→Vulkan translation)         │
├───────────────────────────────────────────┤
│  Turnip / Qualcomm / Adreno GPU Drivers   │
└───────────────────────────────────────────┘
```

**Key emulation technologies:**
- **Wine / Proton**: Windows compatibility layer (supports Proton 10.0 ARM64ec).
- **Box86/Box64**: Real-time x86/x64 → ARM instruction translation for running Windows binaries on ARM Android devices.
- **FEXCore**: Alternative high-performance x86-64 CPU emulator.
- **WOWBox64**: 32-bit Windows application support within the 64-bit translation layer.
- **DXVK**: DirectX 9/10/11 → Vulkan translation (versions 1.10.3 through 2.7.1, including GPLASYNC variants).
- **virglrenderer**: OpenGL → Vulkan rendering bridge.
- **XServer**: Native X11 server implementation baked into the app for graphical output.
- **ALSA Server**: Low-latency audio bridge from the emulated environment to Android audio (AAudio).

## 🏗️ Architecture & Technology Stack

### Android Frontend (`app`)

| Layer | Technology |
|---|---|
| **Language** | [Kotlin](https://kotlinlang.org/) (JDK 17) |
| **UI Framework** | [Jetpack Compose](https://developer.android.com/compose) with Material Design |
| **Dependency Injection** | [Dagger Hilt](https://dagger.dev/hilt/) |
| **Local Database** | [Room](https://developer.android.com/training/data-storage/room) (10+ DAOs, type converters, migrations) |
| **Preferences** | [DataStore](https://developer.android.com/topic/libraries/architecture/datastore) |
| **Networking** | JavaSteam, Epic/GOG/Amazon OAuth (in-app WebView) |
| **Image Loading** | [Landscapist Coil](https://github.com/skydoves/landscapist) |
| **Serialization** | [Kotlinx Serialization](https://github.com/Kotlin/kotlinx.serialization) |
| **Logging** | [Timber](https://github.com/JakeWharton/timber) |
| **Analytics** | [PostHog](https://posthog.com/) (opt-in) |
| **Code Style** | [Kotlinter](https://github.com/jeremymailen/kotlinter-gradle) |

### Native Layer (`app/src/main/cpp/`)

The native layer is written in **C/C++** and compiled via CMake + NDK `22.1.7171670`:

| Component | Purpose |
|---|---|
| `winlator/` | Core native bridge — GPU rendering (`gpu_image.c`, `drawable.c`), X11 connector (`xconnector_epoll.c`), ALSA client (`alsa_client.c`), shared memory (`sysvshared_memory.c`), patchelf wrapper |
| `virglrenderer/` | OpenGL → Vulkan translation for the emulated GPU |
| `patchelf/` | Runtime ELF binary patching for library manipulation |

Linked against: `log`, `android`, `jnigraphics`, `aaudio`, `EGL`, `GLESv2`, `GLESv3`.

### Dynamic Feature Module (`ubuntufs`)

The [`ubuntufs`](ubuntufs/) module is delivered via [Android Dynamic Feature Delivery](https://developer.android.com/guide/playcore/feature-delivery), containing the Linux root filesystem required by the emulation layer. This keeps the base APK lean while delivering the full environment on-demand.

### Winlator Integration (`com.winlator.*`)

A comprehensive set of Java/Kotlin packages ported from [Winlator](https://github.com/brunodev85/winlator):

| Package | Responsibility |
|---|---|
| `container/` | Wine container lifecycle (creation, configuration, startup) |
| `core/` | Core emulation bridge and process management |
| `xserver/` | X11 window server implementation |
| `renderer/` | GPU rendering pipeline |
| `inputcontrols/` | Touch → gamepad/keyboard/mouse translation |
| `alsaserver/` | ALSA audio server bridge |
| `box86_64/` | Box86/Box64 configuration and management |
| `fexcore/` | FEXCore integration |
| `xenvironment/` | Linux user-space environment setup (proot) |
| `steampipeserver/` | Steam Pipe server for game compatibility |
| `winhandler/` | Windows message handling |

## 🛠️ Building from Source

### Prerequisites

- **Android Studio Koala** (2024.1+) or newer
- **Android NDK**: `22.1.7171670`
- **JDK 17**
- **CMake**: `3.22.1`

### Build Steps

1. **Clone the repository**:
   ```bash
   git clone https://github.com/utkarshdalal/GameNative.git
   cd GameNative
   ```

2. **Configure secrets** (optional):

   Create `local.properties` (or set environment variables) with any of:
   ```properties
   STEAMGRIDDB_API_KEY=your_key        # Enables game cover art
   POSTHOG_API_KEY=your_key             # Opt-in analytics
   POSTHOG_HOST=https://us.i.posthog.com
   CLOUD_PROJECT_NUMBER=your_number     # Google Play feature delivery
   ```

3. **Build**:

   Open in Android Studio → **Build > Make Project**, or from the command line:
   ```bash
   ./gradlew assembleDebug
   ```

### Build Variants

| Variant | Description |
|---|---|
| `debug` | Normal debug build |
| `release` | Minified with ProGuard, debug-signed |
| `release-signed` | Minified release with production signing |
| `release-gold` | "Gold" supporter edition with alternate icon and `app.gamenative.gold` application ID |

### CI/CD

The project uses **GitHub Actions** for:
- Pull request checks ([`pluvia-pr-check.yml`](.github/workflows/pluvia-pr-check.yml))
- Signed release builds ([`app-release-signed.yml`](.github/workflows/app-release-signed.yml))
- Tagged release automation ([`tagged-release.yml`](.github/workflows/tagged-release.yml))

## 📁 Project Structure

```
GameNative/
├── app/
│   ├── build.gradle.kts                 # App-level Gradle config
│   ├── src/main/
│   │   ├── AndroidManifest.xml
│   │   ├── java/
│   │   │   ├── app/gamenative/          # Main application code
│   │   │   │   ├── api/                 # REST API clients (GameNative, GameRun, Supporters)
│   │   │   │   ├── data/               # Data models (35+ classes)
│   │   │   │   ├── db/                  # Room database, DAOs, converters, migrations
│   │   │   │   ├── di/                  # Hilt dependency injection modules
│   │   │   │   ├── enums/              # App enums (OS, Arch, Language, Theme, etc.)
│   │   │   │   ├── events/             # Event bus / app events
│   │   │   │   ├── externaldisplay/    # External monitor support
│   │   │   │   ├── gamefixes/          # Per-game compatibility fixes registry
│   │   │   │   ├── service/            # Background services (Steam, GOG, Epic, Amazon)
│   │   │   │   ├── ui/                 # Jetpack Compose UI (screens, components, theme)
│   │   │   │   └── utils/             # Shared utilities
│   │   │   └── com/winlator/           # Winlator emulation engine (17 sub-packages)
│   │   ├── cpp/                         # Native C/C++ code
│   │   │   ├── winlator/              # Core native bridge
│   │   │   ├── virglrenderer/         # OpenGL→Vulkan
│   │   │   └── patchelf/             # ELF binary patching
│   │   ├── assets/                     # Runtime assets (drivers, containers, configs)
│   │   ├── jniLibs/                   # Pre-built native libraries
│   │   └── res/                       # Android resources & translations
├── ubuntufs/                           # Dynamic feature: Linux rootfs
├── manifest.json                       # Downloadable component catalog
├── media/                             # Branding & store assets
├── tools/                             # Build/dev utilities
└── keyvalues/                         # Steam KeyValues data
```

## 👥 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository and create a feature branch.
2. **Follow the code style** — the project uses [Kotlinter](https://github.com/jeremymailen/kotlinter-gradle) for enforced formatting:
   ```bash
   ./gradlew lintKotlin     # Check code style
   ./gradlew formatKotlin   # Auto-format
   ```
3. **Write tests** where applicable — the project supports JUnit, Robolectric, MockK, and Espresso.
4. **Open a Pull Request** — GitHub Actions will automatically run CI checks via [`pluvia-pr-check.yml`](.github/workflows/pluvia-pr-check.yml).

> For feature discussions and coordination, join the [Discord server](https://discord.gg/2hKv4VfZfE) first.

## 🙏 Acknowledgments

GameNative is built on the shoulders of outstanding open-source projects:

| Project | Role in GameNative |
|---|---|
| [Winlator](https://github.com/brunodev85/winlator) | Core emulation engine, container management, XServer, input controls |
| [Wine](https://www.winehq.org/) / [Proton](https://github.com/ValveSoftware/Proton) | Windows application compatibility layer |
| [Box86](https://github.com/ptitSeb/box86) / [Box64](https://github.com/ptitSeb/box64) | x86/x64 → ARM instruction translation |
| [FEX-Emu](https://github.com/FEX-Emu/FEX) | Alternative high-performance x86-64 emulator |
| [DXVK](https://github.com/doitsujin/dxvk) | DirectX → Vulkan translation |
| [Mesa / Turnip](https://www.mesa3d.org/) | Open-source Vulkan drivers for Adreno GPUs |
| [virglrenderer](https://gitlab.freedesktop.org/virgl/virglrenderer) | OpenGL → Vulkan rendering bridge |
| [JavaSteam](https://github.com/Longi94/JavaSteam) | Steam network protocol client for Java/Android |
| [SteamGridDB](https://www.steamgriddb.com/) | Game library artwork |
| [Kenney](https://kenney.nl/) | CC0 input prompt icons (gamepad, keyboard, mouse) |
| [Landscapist](https://github.com/skydoves/landscapist) | Compose-optimized image loading |
| [PostHog](https://posthog.com/) | Opt-in product analytics |

## 📜 License & Legal

- **License**: [GNU General Public License v3.0](LICENSE) — Copyright © 2024 Oxters Wyzgowski.
- **Privacy**: Read our [Privacy Policy](PrivacyPolicy/README.md).
- **Third-Party Notices**: See [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES) for attribution of bundled assets.

> **⚠️ Disclaimer**: GameNative is intended for playing games that you legally own. The maintainers do not condone or assume responsibility for piracy. Play responsibly.
