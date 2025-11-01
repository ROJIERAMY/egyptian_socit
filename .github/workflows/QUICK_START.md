# Quick Start Guide - Build APK with GitHub Actions

## Step 1: Push the workflow to your repository

The workflow file is already created at `.github/workflows/build_apk.yml`. Just commit and push:

```bash
git add .github/workflows/build_apk.yml
git commit -m "Add GitHub Actions workflow for building APK"
git push origin main  # or master, or your branch name
```

## Step 2: Run the workflow

### Option A: Manual Trigger (Recommended for first time)

1. Go to your repository: https://github.com/ROJIERAMY/egyptian_socit
2. Click on the **"Actions"** tab at the top
3. In the left sidebar, click **"Build Android APK"**
4. Click the **"Run workflow"** button (top right)
5. Select your branch (usually `main` or `master`)
6. Click the green **"Run workflow"** button
7. Wait 5-10 minutes for the build to complete

### Option B: Automatic Trigger

The workflow will automatically run when you:
- Push code to any branch
- Create a pull request

## Step 3: Download your APK

1. Go back to the **"Actions"** tab
2. Click on the completed workflow run (the one with a green checkmark ✓)
3. Scroll down to the **"Artifacts"** section
4. Download:
   - **app-release-apk** - The APK file for installation
   - **app-release-bundle** - The AAB file for Google Play Store

## Troubleshooting

### Workflow doesn't appear?
- Make sure you've pushed the `.github/workflows/build_apk.yml` file
- Refresh the Actions page
- The workflow file must be in `.github/workflows/` directory

### Build fails?
- Check the workflow logs by clicking on the failed run
- Common issues:
  - Flutter version mismatch (update `flutter-version` in the workflow)
  - Missing dependencies (ensure `pubspec.yaml` is correct)
  - Android SDK issues (usually auto-resolved)

### Can't find the APK?
- Artifacts are only available for completed runs
- Check that the build step completed successfully
- Artifacts expire after 30 days (you can extend this)

## Need help?

- Check the full documentation in `README.md`
- View workflow logs for detailed error messages
- Ensure your Flutter project builds locally first: `flutter build apk --release`

