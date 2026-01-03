# Setup Instructions for Android 2D Game

This guide will walk you through setting up your development environment to build and run the Android 2D Game project locally.

## Prerequisites

- **Windows 10/11** (64-bit) or **macOS** or **Linux**
- At least **8 GB of RAM** (16 GB recommended)
- **10 GB of free disk space** for Visual Studio, Android SDK, and project files
- Internet connection for downloading components

## Step 1: Install Visual Studio

### For Windows:
1. Download **Visual Studio Community** (free) or Professional/Enterprise from [Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/)
2. Run the installer
3. During installation, **do not close the installer** - proceed to the next step while it's open

### For macOS:
1. Download **Visual Studio for Mac** from [Visual Studio for Mac Downloads](https://visualstudio.microsoft.com/vs/mac/)
2. Run the installer and follow the prompts

### For Linux:
Consider using **Visual Studio Code** with appropriate extensions or use Android Studio for Android development.

## Step 2: Install Required Workloads

The following workloads are essential for Android game development:

### For Windows (Visual Studio Installer):
1. Open **Visual Studio Installer** (search for it in Start menu)
2. Click **Modify** on your Visual Studio installation
3. Select the **Workloads** tab
4. Install the following workloads:
   - ☑️ **Mobile development with .NET** (includes Xamarin)
   - ☑️ **Mobile development with C++** (for native code if needed)
   - ☑️ **Universal Windows Platform development** (optional but recommended)

### For macOS (Visual Studio for Mac):
1. Open **Visual Studio for Mac**
2. Go to **Visual Studio > Preferences**
3. Navigate to **Projects > SDKs & Toolsets**
4. Install:
   - ☑️ **Android SDK** (latest version)
   - ☑️ **.NET SDK** (if not already included)

## Step 3: Install Android SDK and Tools

### For Windows (via Visual Studio):
1. In Visual Studio Installer, go to **Individual components** tab
2. Search for and install:
   - ☑️ **Android SDK setup (API level 31)** or higher
   - ☑️ **Android NDK** (if working with native code)
   - ☑️ **Google Android Emulator**
   - ☑️ **Intel HAXM** (for faster emulation on Windows)

### For macOS:
Follow the **Visual Studio for Mac** setup above, which includes Android SDK installation.

### Verify Android SDK Installation:
```bash
# The Android SDK should be located at:
# Windows: %LOCALAPPDATA%\Android\sdk
# macOS: ~/Library/Android/sdk
```

## Step 4: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/soquarky/android-2d-game.git

# Navigate to the project directory
cd android-2d-game
```

## Step 5: Open and Build the Project

### Using Visual Studio (Windows/macOS):
1. Open Visual Studio
2. Click **File > Open > Project/Solution**
3. Navigate to the cloned repository and open the solution file (`.sln`)
4. Wait for the project to load and dependencies to restore
5. Build the project:
   - **Visual Studio**: Press `Ctrl+Shift+B` (Windows) or `Cmd+Shift+B` (macOS)
   - Or: **Build > Build Solution** from the menu

### Building via Command Line:
```bash
# Navigate to the project directory
cd android-2d-game

# Restore dependencies
dotnet restore

# Build the project
dotnet build

# Build for Android (Release)
dotnet build -c Release
```

## Step 6: Run the Project

### On Android Emulator:
1. Open **Android Device Manager** (Tools > Android Device Manager in Visual Studio)
2. Create or select a virtual device
3. Click **Start** to launch the emulator
4. In Visual Studio, select the emulator from the target device dropdown
5. Press `F5` or click **Debug > Start Debugging**

### On Physical Android Device:
1. Enable **Developer Mode** on your Android device:
   - Go to **Settings > About Phone**
   - Tap **Build Number** 7 times
   - Go back to **Settings > Developer Options**
   - Enable **USB Debugging**
2. Connect your device via USB
3. Accept the USB debugging prompt on your device
4. In Visual Studio, select your device from the target device dropdown
5. Press `F5` or click **Debug > Start Debugging**

## Troubleshooting

### Issue: Android SDK not found
- **Solution**: Go to Visual Studio > Tools > Options > Xamarin > Android Settings
- Verify the Android SDK Location path is correct
- Reinstall Android SDK components if necessary

### Issue: Emulator is slow
- **Solution**: 
  - Ensure **Intel HAXM** is installed (Windows)
  - Use **Hyper-V** instead of HAXM if available
  - Allocate more RAM to the virtual device
  - Use an ARM-based emulator image instead of x86

### Issue: Build fails with dependency errors
- **Solution**: 
  - Run `dotnet restore` to restore NuGet packages
  - Check your internet connection
  - Delete the `bin` and `obj` folders and rebuild

### Issue: Visual Studio can't find the project
- **Solution**: Ensure you've cloned the repository correctly
- Verify the file paths don't contain special characters
- Try opening the solution file directly

## Additional Resources

- [Visual Studio Documentation](https://docs.microsoft.com/en-us/visualstudio/)
- [Xamarin Android Documentation](https://docs.microsoft.com/en-us/xamarin/android/)
- [Android Studio Setup Guide](https://developer.android.com/studio/install)
- [.NET Documentation](https://docs.microsoft.com/en-us/dotnet/)

## Need Help?

If you encounter issues:
1. Check this SETUP.md file again
2. Review the troubleshooting section
3. Consult the official documentation links above
4. Open an issue on the GitHub repository

---

**Last Updated**: January 2026
