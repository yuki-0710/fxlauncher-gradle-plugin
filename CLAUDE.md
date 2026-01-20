# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

FXLauncher Gradle Plugin automates packaging and deployment of JavaFX applications using FXLauncher. It provides Gradle tasks for assembling dependencies, generating manifests, and deploying applications.

- **Plugin ID**: `com.github.yuki-0710.fxlauncher`
- **Language**: Groovy
- **Java Target**: 1.8
- **Gradle Version**: 6.4.1

## Build Commands

```bash
# Build the plugin
./gradlew build

# Run tests
./gradlew test

# Publish to GitHub Packages (requires GITHUB_USERNAME, GITHUB_TOKEN)
./gradlew publish
```

## Architecture

### Task Dependency Chain

```
jar → copyAppDependencies → generateApplicationManifest → embedApplicationManifest → deployApp
                                                                                  → generateNativeInstaller
```

### Core Components (src/main/groovy/no/tornado/fxlauncher/gradle/)

- **FXLauncherPlugin.groovy**: Main plugin entry point, registers extension and tasks
- **FXLauncherExtension.groovy**: Configuration DSL (applicationMainClass, deployTarget, etc.)
- **CopyAppDependenciesTask.groovy**: Copies runtime dependencies to build/fxlauncher
- **GenerateApplicationManifestTask.groovy**: Creates app.xml using FXLauncher's CreateManifest (loads via custom ClassLoader)
- **EmbedApplicationManifestTask.groovy**: Embeds app.xml into fxlauncher.jar via ZIP manipulation
- **DeployAppTask.groovy**: SCP deployment to remote/local targets
- **GenerateNativeInstallerTask.groovy**: Generates native installers via javapackager

### Testing

Tests use Spock Framework with Gradle's ProjectBuilder fixture. Test file is at `src/test/groovy/org/kordamp/gradle/stats/FXLauncherPluginSpec.groovy`.
