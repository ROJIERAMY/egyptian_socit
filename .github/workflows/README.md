# GitHub Actions Workflows

This directory contains GitHub Actions workflows for building and deploying the Flutter Android application.

## Available Workflows

### 1. `build_apk.yml` - Basic APK Build
This workflow builds both APK and App Bundle (AAB) files automatically on:
- Push to `main` or `master` branch
- Pull requests to `main` or `master` branch
- Manual trigger (workflow_dispatch)

**Output:**
- APK file: `app-release.apk`
- App Bundle: `app-release.aab`

Both files are uploaded as artifacts and can be downloaded from the Actions tab.

### 2. `build_signed_apk.yml` - Signed APK Build (Production)
This workflow builds signed APK/AAB files for production releases. It triggers on:
- Version tags (e.g., `v1.0.0`, `v1.2.3`)
- Manual trigger with build type selection

**Setup for Signing (Optional):**
If you want to sign your APK/AAB, you need to:

1. Create a keystore file:
   ```bash
   keytool -genkey -v -keystore android/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
   ```

2. Add GitHub Secrets:
   - Go to Repository Settings → Secrets and variables → Actions
   - Add the following secrets:
     - `KEYSTORE_BASE64`: Base64 encoded keystore file (get it with: `base64 -i key.jks`)
     - `KEYSTORE_PASSWORD`: Keystore password
     - `KEY_ALIAS`: Key alias (usually "upload")
     - `KEY_PASSWORD`: Key password

3. Uncomment the "Setup signing key" section in `build_signed_apk.yml`

4. Update `android/app/build.gradle` to enable signing:
   ```gradle
   signingConfigs {
       release {
           keyAlias = keystoreProperties['keyAlias']
           keyPassword = keystoreProperties['keyPassword']
           storeFile = keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword = keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
   ```

## Usage

### Building APK Manually

1. Go to your repository on GitHub
2. Click on the "Actions" tab
3. Select "Build Android APK" or "Build Signed Android APK"
4. Click "Run workflow"
5. Select the branch and build type
6. Click "Run workflow" button
7. Wait for the build to complete
8. Download the APK from the artifacts section

### Building on Tag Push

To automatically build and create a release:

1. Create and push a tag:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. The workflow will automatically:
   - Build the APK and AAB
   - Create a GitHub Release
   - Attach the APK and AAB files to the release

## Configuration

### Update Flutter Version

If you're using a different Flutter version, update the `flutter-version` in the workflow files:

```yaml
flutter-version: '3.24.0' # Change to your version
```

To check your Flutter version:
```bash
flutter --version
```

### Update Java Version

If needed, you can change the Java version in the workflow:
```yaml
java-version: '17' # Options: '8', '11', '17', '21'
```

## Troubleshooting

### Build Fails
- Check Flutter version compatibility
- Ensure all dependencies are properly listed in `pubspec.yaml`
- Review the workflow logs in the Actions tab

### Signing Issues
- Verify all secrets are correctly set
- Ensure keystore file is properly base64 encoded
- Check that key.properties file paths are correct

### Artifact Not Found
- Artifacts are retained for 30-90 days depending on the workflow
- Ensure the build completed successfully
- Check the workflow logs for any errors

