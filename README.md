<div align="center">

# Android App 4 (Practical 4)

**Modern Android (Java 11) application workspace configured with Android Gradle Plugin 9.0, Gradle Version Catalogs, Material Day/Night theming, and Android 12+ backup rules.**

[![Platform](https://img.shields.io/badge/Platform-Android%20(API%2024%E2%80%9336)-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com/)
[![Language](https://img.shields.io/badge/Java-11-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Gradle](https://img.shields.io/badge/Gradle-9.1.0-02303A?style=for-the-badge&logo=gradle&logoColor=white)](https://gradle.org/)
[![AGP](https://img.shields.io/badge/AGP-9.0.0-4285F4?style=for-the-badge&logo=androidstudio&logoColor=white)](https://developer.android.com/build)
[![Material](https://img.shields.io/badge/Material_Components-1.10.0-757575?style=for-the-badge&logo=materialdesign&logoColor=white)](https://m2.material.io/develop/android)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

</div>

---

## System & Build Architecture

The repository provides a clean, Gradle-managed Android application scaffold (`com.example.practical_4`) structured around centralized dependency management (`gradle/libs.versions.toml`), non-transitive R classes, Light/Dark theme resource qualifiers, and dual-tier unit and instrumentation test harnesses.

```mermaid
flowchart LR
    subgraph BuildConfig["Gradle Build System"]
        Catalog["Version Catalog<br/>libs.versions.toml"]
        RootGradle["Root Config<br/>settings.gradle and build.gradle"]
        AppGradle["Module Config<br/>app/build.gradle"]
    end

    subgraph AppModule["Android App Module"]
        Manifest["App Manifest<br/>AndroidManifest.xml"]
        Resources["Resources and Themes<br/>Day/Night, Adaptive Icons, XML Rules"]
        UnitTests["JVM Unit Tests<br/>JUnit 4"]
        AndroidTests["Instrumented Tests<br/>AndroidX Test and Espresso"]
    end

    subgraph Outputs["Build and Verification Artifacts"]
        Lint["Android Lint Report<br/>Static Analysis"]
        APK["Debug and Release APK<br/>API 24 to API 36"]
    end

    Catalog -->|Resolve Plugin and Library Versions| RootGradle
    RootGradle -->|Include App Module| AppGradle
    AppGradle -->|Compile Java 11 and Package| Manifest
    AppGradle -->|Merge Resource Qualifiers| Resources
    AppGradle -->|Execute Local Suite| UnitTests
    AppGradle -->|Execute Device Suite| AndroidTests
    Manifest -->|Validate and Analyze| Lint
    Resources -->|Package Resources| APK
    Manifest -->|Assemble Application| APK
```

---

## Key Engineering Highlights

- **Centralized Version Catalog (`libs.versions.toml`):** Declares plugins (`AGP 9.0.0`) and dependencies (`AppCompat`, `Material Components`, `JUnit`, `Espresso`) in one strongly typed TOML catalog for reproducible builds.
- **Modern Android SDK Targeting:** Configured for `compileSdk 36` and `targetSdk 36` while maintaining broad backwards compatibility down to `minSdk 24` (Android 7.0 Nougat) with Java 11 source/target bytecode compatibility.
- **Day/Night Material Theming:** Separates default (`values/themes.xml`) and dark mode (`values-night/themes.xml`) palettes inheriting from `Theme.MaterialComponents.DayNight.DarkActionBar`.
- **Adaptive Vector Launcher Assets:** Supplies `mipmap-anydpi-v26` adaptive icon definitions backed by vector foreground/background drawables alongside density-specific WebP launcher fallbacks (`mdpi` through `xxxhdpi`).
- **Android 12+ Privacy & Backup Rules:** Pre-wired with `xml/data_extraction_rules.xml` (cloud backup and device-to-device transfer) and legacy `xml/backup_rules.xml`.
- **Non-Transitive R Classes:** Enables `android.nonTransitiveRClass=true` in `gradle.properties` to keep resource symbols isolated per module and accelerate incremental compilation.

---

## Repository Structure

```text
Android_App_4/
├── app/
│   ├── build.gradle                              # App module config (SDK 24–36, Java 11)
│   ├── proguard-rules.pro                        # R8 / ProGuard shrinking rules
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml               # Application manifest & theme/backup bindings
│       │   └── res/
│       │       ├── drawable/                     # Adaptive launcher foreground & background vectors
│       │       ├── mipmap-*/                     # Adaptive XML & WebP launcher icons
│       │       ├── values/                       # Strings, colors, and Light theme definitions
│       │       ├── values-night/                 # Dark mode theme definitions
│       │       └── xml/                          # Cloud backup & data extraction rules
│       ├── test/java/com/example/practical_4/
│       │   └── ExampleUnitTest.java              # Local JVM JUnit 4 test suite
│       └── androidTest/java/com/example/practical_4/
│           └── ExampleInstrumentedTest.java      # On-device AndroidJUnit4 / Espresso test
├── gradle/
│   ├── libs.versions.toml                        # Centralized Gradle Version Catalog
│   └── wrapper/                                  # Pinned Gradle 9.1.0 wrapper distribution
├── build.gradle                                  # Top-level plugin declaration
├── settings.gradle                               # Repository management & module inclusion
├── gradle.properties                             # JVM heap, AndroidX & non-transitive R flags
├── gradlew / gradlew.bat                         # Cross-platform Gradle wrapper scripts
├── LICENSE                                       # MIT License
└── README.md                                     # Project documentation
```

---

## Getting Started & Local Execution

### Prerequisites

- **JDK 17 or JDK 21** (required to run Gradle 9.1.0; compiles app bytecode to Java 11)
- **Android SDK** with Platform **API 36** installed
- **Android Studio** (optional, for IDE editing and emulator execution)

### 1. Clone the Repository

```bash
git clone https://github.com/theekshana-sudeepa/Android_App_4.git
cd Android_App_4
```

### 2. Configure the Android SDK Path

Ensure `ANDROID_HOME` is exported in your environment, or create a local `local.properties` file at the repository root (already excluded by `.gitignore`):

```properties
sdk.dir=/path/to/your/Android/sdk
```

### 3. Build, Test, and Run Lint

```bash
# Run local JVM unit tests
./gradlew testDebugUnitTest

# Run Android Lint static analysis
./gradlew lintDebug

# Assemble the debug APK
./gradlew assembleDebug

# Run instrumented tests on a connected device or emulator
./gradlew connectedDebugAndroidTest
```

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for full details.

---

<div align="center">

**Authored by Theekshana Sudeepa**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Theekshana_Sudeepa-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/theekshana-sudeepa/)
[![GitHub](https://img.shields.io/badge/GitHub-theekshana--sudeepa-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/theekshana-sudeepa)
[![Website](https://img.shields.io/badge/LexoraTech-lexoratech.com-0F172A?style=flat-square&logo=googlechrome&logoColor=white)](https://lexoratech.com)

</div>
