<div align="center">
  <img src="app/src/main/ic_launcher-playstore.png" width="128" alt="GameNative Logo" />
  <h1>GameNative</h1>
  <p><strong>Play your PC games from Steam, Epic, and GOG directly on your Android device with seamless cloud saves.</strong></p>
</div>

**GameNative** is a game launcher and compatibility layer for Android that provides access to PC game libraries from Steam, Epic Games, and GOG. The application integrates Winlator as an emulation backend to execute Windows binaries natively on ARM architectures.

*(Note: This project is a fork of [Pluvia](https://github.com/oxters168/Pluvia), originally a Steam client for Android, and has been expanded to support Epic Games and GOG.)*

<div align="center">

[Playing Stray on Poco F6](https://github.com/user-attachments/assets/1870fd14-7de9-4054-ba92-d3a5c73686b5)

</div>

---

## Capabilities

- **Store Integration**: Built-in authentication and library management for Steam, Epic Games, and GOG storefronts.
- **Cloud Saves**: Automatic synchronization of remote save data across linked platforms.
- **Native UI**: Application frontend is developed utilizing native Android UI toolkits (`Jetpack Compose`).
- **Emulation Backend**: Relies on a bundled Winlator runtime environment utilizing established compatibility layers:
  - **Proton**: A tool for use with the Steam client, which allows games exclusive to Windows to run on the Linux operating system. *It relies on **Wine** a compatibility layer that translates Windows API calls into POSIX calls on-the-fly, eliminating the performance and memory penalties of traditional virtual machines to facilitate this.*
  - **FEX-emu**: An advanced binary recompiler that allows you to run x86/x64 applications on ARM64 Linux devices. It offers broad compatibility and generates optimized code via a custom IR, minimizing in-game stuttering. 
  - **Box64**: Enables running x86_64 Linux programs (including games) on non-x86_64 Linux systems (like ARM64). It leverages native system libraries (libc, libm, SDL, OpenGL) and features a DynaRec for a 5-10x speed boost.
  - **DXVK**: Provides a Vulkan-based translation layer for D3D8, D3D9, D3D10 and D3D11.
  - **VKD3D**: Specifically *vkd3d-proton*, a fork of VKD3D which aims to implement the full Direct3D 12 API on top of Vulkan.

## Quick Start

> **Note:** GameNative is in early development. Compatibility is not guaranteed for all titles and extensive configuration may be necessary for stable execution.

### Prerequisites
- An Android device running Android 8.0 (API Level 26) or newer.
- Games legally purchased on a supported digital storefront.

### Installation
1. Download the latest release APK from the [Downloads Page](https://github.com/utkarshdalal/GameNative/releases).
2. Install the APK on your device. Ensure that installation from unknown sources is permitted in your security settings.

### Basic Usage
1. Launch the application.
2. Authenticate with a supported digital storefront (Steam, Epic Games, or GOG).
3. Download and install a title from your library.
4. Launch the executable from the interface.

---

## Community and Support

- **Discord**: For troubleshooting and compatibility reports, join the [Discord server](https://discord.gg/2hKv4VfZfE).
- **Donations**: Project development can be supported via [Ko-fi](https://ko-fi.com/gamenative).

> **Important:** GitHub Issues are reserved exclusively for codebase development tracking. End-user support requests submitted via GitHub Issues will be closed automatically.

## Building from Source

1. Clone the repository and import the project into **Android Studio**. The application builds via a standard Gradle execution sequence.
2. **SteamGridDB API Key (Optional):** To enable automatic asset fetching via the SteamGridDB API for custom games, append your token to `local.properties`:
   ```properties
   STEAMGRIDDB_API_KEY=your_api_key_here
   ```
   *If a valid token is not supplied, the application will compile normally but will fallback to default placeholder assets.*

## License

- **License:** [GPL 3.0](https://github.com/utkarshdalal/GameNative/blob/master/LICENSE)
- **Privacy Policy:** [Read our Privacy Policy](https://github.com/utkarshdalal/GameNative/blob/master/PrivacyPolicy/README.md)

> **Disclaimer:** This software is intended strictly for playing games that you **legally own**. Do not use this software for piracy or any other illegal purposes. The maintainer of this fork assumes no responsibility for misuse.
