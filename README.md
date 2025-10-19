# tgpt-repo

**Agentic Termux → APK Pipeline with Model Plumbing**

This repository provides a repeatable, autonomous build system for creating Android APKs from a Termux environment or CI/CD pipeline. It integrates LLM model plumbing (on-device or hosted) and supports both Termux-native builds (python-for-android/Buildozer) and Docker-based cross-compilation with GitHub Actions.

## Features

- **Dual Build Paths**: Termux-native (Option A) or Docker/CI cross-build (Option B)
- **JDK Flexibility**: Supports OpenJDK 17 and 21 via build matrix
- **Model Integration**: On-device (ggml/llama.cpp) or hosted inference (Replicate, Hugging Face)
- **Agentic Workflow**: Autonomous agent loop for build, test, and release
- **CI/CD Ready**: GitHub Actions workflows for automated APK builds and releases
- **Mobile-First**: Optimized for Termux on Android devices

## Repository Structure

```
tgpt-repo/
├─ app/
│  └─ main.py            # Kivy/UI entry (vm1kivy_main)
├─ src/
│  └─ tgpt_core.py       # Agent core logic (vm2agent_core)
├─ models/
│  └─ llm_small.ggml     # On-device model (vm3model_small)
├─ scripts/
│  ├─ termux_setup.sh
│  ├─ build_termux_p4a.sh
│  ├─ docker_cross_build.sh
│  └─ sign_apk.sh
├─ android_build/
│  └─ buildozer.spec
├─ ci/
│  ├─ Dockerfile
│  ├─ ci_build.sh
│  └─ build_apk.yml      # GitHub Actions
├─ assets/
└─ README.md
```

## Quick Start

### Option A: Termux-Native Build

1. **Bootstrap Termux environment:**
   ```bash
   bash scripts/termux_setup.sh
   ```

2. **Build APK:**
   ```bash
   bash scripts/build_termux_p4a.sh
   ```

3. **Install and test:**
   ```bash
   adb install android_build/bin/*.apk
   ```

### Option B: Docker/CI Cross-Build

1. **Build with Docker:**
   ```bash
   bash scripts/docker_cross_build.sh
   ```

2. **Or trigger GitHub Actions:**
   - Push to `main` branch or manually trigger workflow
   - Download APK artifact from Actions tab

## Build Options

| Option | Environment | Pros | Cons |
|--------|-------------|------|------|
| **A** | Termux-native | Portable, offline, developer control | Slower on-device, fiddly NDK setup |
| **B** | Docker/CI | Reproducible, fast, automatable | Initial setup, requires network |

## Model Integration

### On-Device (Local, Offline)
- Bundle quantized ggml model in `models/`
- Compile llama.cpp for Android (via p4a or cross-compile)
- Use pyllamacpp or lightweight Python binding

### Hosted Inference (Cloud)
- Call HTTPS API from app
- Store API key in secure storage (Android Keystore or environment variable)
- Example integration in `src/tgpt_core.py`

## Agentic Workflow

The agent loop in `src/tgpt_core.py` autonomously:
1. Pulls latest repo changes
2. Runs tests/lint
3. Chooses build option based on environment
4. Builds APK
5. Runs smoke tests
6. Uploads artifact and opens PR if model updated

## GitHub Actions

The CI workflow (`.github/workflows/build_apk.yml`) builds APKs with JDK 17 and 21 in a matrix, signs them, and uploads as artifacts or releases.

## Requirements

- **Termux**: Python 3, git, clang, make, automake, autoconf, pkg-config, openssl, wget, unzip, zip
- **Java**: OpenJDK 17 or 21
- **Python packages**: cython, python-for-android, buildozer

## License

MIT License

## Credits

Built with Manus AI for autonomous mobile app development.
