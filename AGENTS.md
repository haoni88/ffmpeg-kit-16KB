# Repository Guidelines

## Project Structure & Module Organization
- `android/` contains the Gradle project for the Android wrapper plus JNI sources under `ffmpeg-kit-android-lib/src/main/cpp` and unit tests under `src/test`. `jni/` Makefiles control native builds.
- `apple/`, `linux/`, `macos.sh`, `tvos.sh`, and `ios.sh` handle Apple and desktop targets; platform-specific scripts live in `tools/` and shared helpers in `scripts/`.
- `flutter/` and `react-native/` house hybrid bindings; `docs/` holds platform guides and assets; `scripts/` and `tools/` are reused by all platform entrypoints (`android.sh`, `linux.sh`, `macos.sh`, `tvos.sh`, `apple.sh`).
- Keep additions within the relevant platform folder and reuse shared scripts instead of duplicating logic.

## Build, Test, and Development Commands
- Android: set `ANDROID_SDK_ROOT` and `ANDROID_NDK_ROOT` (use the 16KB-friendly r23/r25 NDK noted in README). Run `./android.sh --help` for flags; `./android.sh --enable-gpl --full` builds all external libs. Build output and logs go to `build.log` and `android/ffmpeg-kit-android-lib/build/`.
- Apple/macOS/tvOS: `./macos.sh`, `./tvos.sh`, or `./ios.sh` mirror the Android options (add `--enable-gpl` or `--full` as needed).
- Linux: `./linux.sh --help` lists architectures/libraries; prefer `--no-output-redirection` while debugging.
- Tests: `cd android && ./gradlew test` runs JVM tests. For end-to-end coverage, use the companion apps in the `ffmpeg-kit-test` repo that pair with this build.

## Coding Style & Naming Conventions
- C/C++ JNI code uses 4-space indentation, ALL_CAPS for macros, and snake_case for static helpers; follow existing header and license blocks. Java/Kotlin classes use UpperCamelCase types and lowerCamelCase methods/fields.
- Shell scripts assume bash; keep option handling consistent with existing `case` blocks and route noisy output to `build.log`.
- Avoid modifying upstream FFmpeg sources; prefer toggling features via build flags or library enable/disable helpers in `scripts/function-*.sh`.

## Testing Guidelines
- Add or update unit tests in `android/ffmpeg-kit-android-lib/src/test/java` alongside new functionality; name tests `*Test`.
- Before opening a PR, run the platform build plus `./gradlew test`; attach failing logs if something is platform-specific. Strive to test both LTS and main variants when touching build options.

## Commit & Pull Request Guidelines
- Use short, imperative commit messages (e.g., `Add arm64 neon tune`, `Fix muxer timeout`). Group platform-specific changes per commit when possible.
- Open PRs against the development branches (`development`, `development-react-native`, or `development-flutter`) rather than `main`. Describe what changed, which platform(s) were built, flags used, and test evidence. Link related issues and include screenshots/log excerpts when UI or build output changes. 
