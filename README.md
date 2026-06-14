# HomeCare

Android app for managing apartment maintenance tasks, bills, and grocery lists. The runnable project lives in `4-application/`.

This guide covers running the app from **Cursor’s terminal** (no Android Studio required).

## Prerequisites

- **JDK 17+** (Homebrew: `brew install openjdk@17`)
- **Android SDK** at `~/Library/Android/sdk` (platform-tools, emulator, API 34)
- **An Android emulator AVD** (e.g. `Pixel_6a`) or a physical device with USB debugging enabled

## One-time setup

### 1. Shell environment

Add these lines to `~/.zshrc` so Gradle, `adb`, and the emulator are available in every terminal:

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
export ANDROID_HOME="$HOME/Library/Android/sdk"
export PATH="$JAVA_HOME/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator:$PATH"
```

Reload your shell:

```bash
source ~/.zshrc
```

Verify:

```bash
java -version
adb version
emulator -list-avds
```

### 2. SDK path for Gradle

Create `4-application/local.properties` (this file is gitignored and must be created locally):

```properties
sdk.dir=/Users/YOUR_USERNAME/Library/Android/sdk
```

Replace `YOUR_USERNAME` with your macOS username, or use the full path to wherever your Android SDK is installed.

## Run the app

Use **two Cursor terminal tabs**.

### Terminal 1 — start the emulator

```bash
emulator -avd Pixel_6a
```

Wait until the emulator finishes booting. Use your own AVD name if different (`emulator -list-avds` to list them).

### Terminal 2 — build, install, and launch

```bash
cd 4-application
./gradlew installDebug
adb shell am start -n com.example.homecare/.MainActivity
```

The first `./gradlew` run downloads Gradle and dependencies and may take a few minutes. Later builds are much faster.

## Useful commands

| Goal | Command |
|------|---------|
| Build APK only | `cd 4-application && ./gradlew assembleDebug` |
| Check connected devices | `adb devices` |
| View app logs | `adb logcat \| grep -i homecare` |
| Uninstall from device | `adb uninstall com.example.homecare` |

Debug APK output: `4-application/app/build/outputs/apk/debug/app-debug.apk`

## Troubleshooting

**`SDK location not found`** — Create or fix `4-application/local.properties` with the correct `sdk.dir` path.

**`command not found: emulator` or `adb`** — Your shell env is not set up. Run the `export` lines above or add them to `~/.zshrc`.

**`No connected devices`** — Start the emulator first and confirm it appears with `adb devices`.

**Physical phone instead of emulator** — Enable USB debugging, connect the phone, run `adb devices`, then use the same `./gradlew installDebug` command.

## Project docs

- `1-overview.md` — app overview
- `2-requirements.md` — requirements
- `3-api.md` — API notes
- `5-retrospective.md` — project retrospective
