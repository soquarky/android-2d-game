# Google Play Console Publishing Guide

This guide provides step-by-step instructions for compiling the Android 2D Game, creating signing keys, generating Android App Bundle (AAB) files, and uploading your game to Google Play Console.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Compiling the Game](#compiling-the-game)
3. [Creating Signing Keys](#creating-signing-keys)
4. [Generating AAB Files](#generating-aab-files)
5. [Uploading to Google Play Console](#uploading-to-google-play-console)
6. [Post-Upload Steps](#post-upload-steps)
7. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before you begin, ensure you have:

- **Android Studio** (latest version recommended)
- **Java Development Kit (JDK)** version 11 or higher
- **Android SDK** with API level 34 or higher
- **Google Play Developer Account** ($25 one-time fee)
- **Google Play Console access**
- At least 500 MB of free disk space
- Command-line tools (`keytool`, `jarsigner`)

### Install Required Tools

```bash
# macOS (using Homebrew)
brew install openjdk@11

# Ubuntu/Debian
sudo apt-get install openjdk-11-jdk

# Windows
# Download from https://www.oracle.com/java/technologies/downloads/
```

---

## Compiling the Game

### Step 1: Clone and Prepare the Repository

```bash
git clone https://github.com/soquarky/android-2d-game.git
cd android-2d-game
```

### Step 2: Open in Android Studio

1. Open **Android Studio**
2. Click **File** → **Open**
3. Navigate to the `android-2d-game` directory
4. Click **Open**
5. Wait for Gradle to sync (this may take several minutes on first load)

### Step 3: Verify Build Configuration

1. Open `build.gradle` (Module: app)
2. Ensure the `targetSdkVersion` is at least 34
3. Verify `minSdkVersion` is 21 or higher (for broader device compatibility)
4. Update version codes if needed:

```gradle
android {
    compileSdkVersion 34
    
    defaultConfig {
        applicationId "com.soquarky.game2d"
        minSdkVersion 21
        targetSdkVersion 34
        versionCode 1
        versionName "1.0.0"
    }
}
```

### Step 4: Build APK/AAB (Local Testing)

To build a debug version for testing:

```bash
./gradlew assembleDebug
```

The APK will be located at: `app/build/outputs/apk/debug/app-debug.apk`

---

## Creating Signing Keys

### Step 1: Generate Keystore File

A keystore file is required to sign your app for production release.

#### Using Android Studio (Recommended):

1. Go to **Build** → **Generate Signed Bundle / APK**
2. Select **Android App Bundle** (for Google Play)
3. Click **Create new** under the Keystore path
4. Fill in the following information:

```
Key Store Path: ~/android-keystore/my-release-key.jks
Password: [Create a strong password - write it down securely!]
Key Alias: my-key-alias
Key Password: [Can be same as keystore password]
Validity (years): 25 (or higher)

Certificate Information:
- First and Last Name: Your Name
- Organizational Unit: Your Company/Organization
- Organization: Your Company
- City or Locality: Your City
- State or Province: Your State
- Country Code: US (or your country)
```

5. Click **OK**

#### Using Command Line:

```bash
mkdir -p ~/.android-keystore

keytool -genkey -v -keystore ~/.android-keystore/my-release-key.jks \
  -keyalg RSA -keysize 2048 -validity 9125 -alias my-key-alias
```

You'll be prompted for:
- Keystore password
- Key password
- Certificate details (name, organization, etc.)

### Step 2: Secure Your Keystore

**IMPORTANT**: Your keystore file is crucial for future app updates.

```bash
# Backup your keystore to multiple secure locations
cp ~/.android-keystore/my-release-key.jks /path/to/secure/backup/

# Set restrictive permissions (Linux/macOS)
chmod 600 ~/.android-keystore/my-release-key.jks

# Never commit keystore to version control
echo "*.jks" >> .gitignore
```

---

## Generating AAB Files

Android App Bundle (AAB) is the required format for Google Play Console submissions.

### Method 1: Using Android Studio (Recommended)

1. Go to **Build** → **Generate Signed Bundle / APK**
2. Select **Android App Bundle**
3. Click **Next**
4. Select your Keystore:
   - **Keystore path**: Browse to your `.jks` file
   - **Password**: Enter your keystore password
   - **Key alias**: Select your alias
   - **Key password**: Enter your key password
5. Click **Next**
6. Select **Release** build variant
7. Click **Finish**

The signed AAB will be at: `app/build/outputs/bundle/release/app-release.aab`

### Method 2: Using Command Line

```bash
./gradlew bundleRelease \
  -Pandroid.injected.signing.store.file=/path/to/my-release-key.jks \
  -Pandroid.injected.signing.store.password=your_keystore_password \
  -Pandroid.injected.signing.key.alias=my-key-alias \
  -Pandroid.injected.signing.key.password=your_key_password
```

### Verify Your AAB

```bash
# Check AAB integrity (optional)
bundletool validate --bundle-path=app/build/outputs/bundle/release/app-release.aab
```

---

## Uploading to Google Play Console

### Step 1: Set Up Google Play Developer Account

1. Visit [Google Play Console](https://play.google.com/console)
2. Sign in with your Google account
3. Accept the terms and pay the $25 one-time developer fee
4. Complete your account setup:
   - Add payment method
   - Provide developer identity information
   - Accept developer agreement

### Step 2: Create an App

1. In Google Play Console, click **Create app**
2. Enter app details:
   - **App name**: "Android 2D Game" (or your preferred name)
   - **Default language**: English
   - **App or game**: Select **Game**
   - **Free or paid**: Select **Free** (unless you're selling)
3. Click **Create app**

### Step 3: Complete App Details

Navigate through each section to complete:

#### Dashboard
- Review all requirements

#### App details
- **Short description** (80 characters max)
- **Full description** (4000 characters max)
- **Developer contact information**

Example descriptions:
```
Short: "An action-packed 2D platformer with exciting gameplay!"

Full: "Experience fast-paced action in our 2D game. Features smooth 
controls, challenging levels, and engaging gameplay. Perfect for 
casual gaming on the go. Play offline anytime, anywhere!"
```

#### Graphics
Upload the following:
- **App icon**: 512x512 PNG (must not have transparency outside icon)
- **Featured graphic**: 1024x500 PNG
- **Screenshots**: At least 2 (up to 8) screenshots (1080x1920 or similar)
- **Video preview** (optional): YouTube video link

### Step 4: Content Rating Questionnaire

1. Click **Content rating**
2. Complete the questionnaire about your game's content
3. Submit for review
4. Receive your content rating

### Step 5: Upload AAB File

1. Click **Testing** → **Internal testing** (for initial testing)
   - Or go directly to **Production** to release publicly
2. Click **Create new release**
3. Click **Browse files** and select your signed AAB file:
   ```
   app/build/outputs/bundle/release/app-release.aab
   ```
4. Review the following auto-generated information:
   - App version
   - Supported devices
   - Supported features
5. Add **Release notes** (optional):
   ```
   Version 1.0.0 - Initial Release
   - Launch day features
   - Optimized for all devices
   ```
6. Click **Review release**

### Step 6: Review and Submit

1. Review all app details one final time
2. Accept the declaration:
   - "I understand that Google Play requires apps to target the latest Android API level"
3. Click **Submit to Play Store**

Your app enters **review queue** (typically 1-3 hours for processing).

---

## Post-Upload Steps

### Monitor Review Status

1. In Google Play Console, go to your app
2. Check **All apps** → Your app name → **Release** → **Production**
3. View status:
   - **In review**: Your app is being reviewed
   - **In rollout**: Your app is being rolled out to users
   - **Live**: Your app is published

### Set Up Rollout (Optional but Recommended)

Instead of releasing to 100% immediately:

1. Click your release
2. Set **Rollout percentage** (e.g., start with 20%, increase gradually)
3. Monitor crash reports and reviews
4. Increase rollout percentage once stable

### Monitor Performance

After publication:

1. Go to **Analytics** to view:
   - Downloads
   - Uninstalls
   - Active installs
   - Crash reports
   - ANR (Application Not Responding) reports
2. Check **Reviews and ratings** regularly
3. Respond to user feedback

---

## Troubleshooting

### Issue: "Build fails with Gradle error"

**Solution**:
```bash
# Clean and rebuild
./gradlew clean build

# Or in Android Studio:
# Build → Clean Project
# Build → Rebuild Project
```

### Issue: "Keystore file not found"

**Solution**:
```bash
# Verify keystore path
ls -la ~/.android-keystore/my-release-key.jks

# Check file permissions
chmod 600 ~/.android-keystore/my-release-key.jks
```

### Issue: "Invalid keystore password"

**Solution**:
- Verify you're using the correct password (case-sensitive)
- Regenerate the keystore if password is lost
- **Note**: Lost keystores require creating a new signing key and cannot update existing apps

### Issue: "App signature does not match"

**Solution**:
- Ensure you're using the same keystore for all releases
- Verify key alias and password match your stored credentials
- Check that targeting SDK matches Play Store requirements

### Issue: "Upload fails with permission error"

**Solution**:
1. Verify your Google account has admin access in Google Play Console
2. Check that your Google Developer Account is in good standing
3. Ensure payment method is valid on file

### Issue: "Content rating rejected"

**Solution**:
- Re-review the content rating questionnaire
- Be honest about app content
- Submit again for re-rating

### Issue: "App rejected during review"

**Solution**:
1. Check rejection message in Google Play Console
2. Address the specific policy violation
3. Common reasons:
   - App crashes or ANR issues → Test more thoroughly
   - Inappropriate content → Review content guidelines
   - Privacy policy missing → Add privacy policy
   - Requires certain permissions → Justify or remove
4. Build a new AAB and resubmit

---

## Additional Resources

- [Google Play Console Help](https://support.google.com/googleplay/android-developer)
- [Android App Bundle Documentation](https://developer.android.com/guide/app-bundle)
- [App Signing Overview](https://developer.android.com/studio/publish/app-signing)
- [Google Play Policies](https://play.google.com/about/developer-content-policy/)
- [Android Developers Guide](https://developer.android.com/docs)

---

## Quick Reference Checklist

- [ ] Java Development Kit 11+ installed
- [ ] Android Studio updated to latest version
- [ ] Project builds successfully locally
- [ ] Signing keystore created and backed up securely
- [ ] `.gitignore` updated to exclude keystore
- [ ] AAB file generated successfully
- [ ] Google Play Developer Account created
- [ ] App created in Google Play Console
- [ ] All graphics assets uploaded (icon, screenshots)
- [ ] Content rating questionnaire completed
- [ ] App details filled in completely
- [ ] Privacy policy added (if required)
- [ ] AAB uploaded to internal testing
- [ ] App tested on multiple devices
- [ ] Submitted to production for review
- [ ] Rollout percentage monitored
- [ ] Analytics and reviews monitored

---

**Last Updated**: January 3, 2026

For questions or contributions, please open an issue in the repository.
