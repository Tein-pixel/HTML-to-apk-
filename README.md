# HTML-to-apk-

This repository builds the TEIN Android project from a checked-in source archive using GitHub Actions.

## Run a build

1. Open **Actions** in this repository.
2. Select **Build TEIN APK**.
3. Select **Run workflow**, or push a change to one of the configured source archive/workflow paths.
4. Open the completed run and download the **TEIN-apk** artifact.

## Accepted source layouts

The workflow first checks whether `extracted/TEIN/settings.gradle` and `extracted/TEIN/app/build.gradle` already exist. If not, it looks for the first available archive in this order:

1. `TEIN.zip`
2. `TEIN_LOCAL_GRADLE_ENGINE_NDK_FIXED.zip`
3. `TEIN_LOCAL_GRADLE_ENGINE_RECHECKED.zip`
4. `TEIN_LOCAL_GRADLE_ENGINE_RESEARCHED.zip`

The selected ZIP is integrity-tested before extraction. Its contents must provide a `TEIN/` directory with the Gradle settings file and Android app module at the expected paths.

## Build environment

The current workflow configures:

- Ubuntu GitHub-hosted runner.
- Eclipse Temurin Java 17.
- Android SDK platform 33.
- Android Build Tools 34.0.0.
- Android NDK 28.2.13676358.
- Gradle 8.5.
- Build task: `:app:assembleDebug`.

The workflow verifies the NDK compiler executable before starting the Gradle build and writes the runner's SDK path to `local.properties`.

## Output

The workflow uploads debug APK files from:

```text
extracted/TEIN/app/build/outputs/apk/debug/
```

The artifact is named `TEIN-apk` and is retained for 7 days. The workflow does not publish a signed production release.

## Troubleshooting

**No source archive found**  
Upload one of the accepted ZIP names to the repository root, or commit the extracted project at `extracted/TEIN`.

**ZIP validation or extraction fails**  
Check that the archive is a valid ZIP and that it contains the expected `TEIN/` project directory.

**NDK compiler verification fails**  
Inspect the SDK installation step in the Actions log. The build requires NDK 28.2.13676358 and its Linux x86_64 Clang executable.

**Gradle build fails**  
Inspect the first compiler or dependency error in the Gradle log. Confirm the project is compatible with Java 17, Gradle 8.5, compile SDK 33, and the configured NDK.

## Security and maintenance

Do not commit API keys, signing passwords, or other secrets to this repository. Use GitHub Actions secrets for credentials required by CI. Keep source archives limited to project files needed for the build.
