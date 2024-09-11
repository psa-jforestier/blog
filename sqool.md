# Install anything on a SQOOL tablet

## Enable "USB Debugging"

- Go to the tablet settings
- Scroll down to "About the tablet"
- Tap several time in the "Build number" entry, until a message "You are now in developer mode" appears
- Go back to the settings menu
- Go into the "System" entry
- Go into the "Advanced preference" entry
- Go into the "Developers options" entry
- Check "USB debugging"

## Install ADB
- For your computer, install "adb.exe" from Google
- Connect the tablet to your computer via USB
- On the tablet, confirm that you want to debug the tablet
- Verify the CPU architecure and the pixel density of the tablet
  - Do the following ADB command : `adb shell wm density ; uname -m`
  - It will reply something like `Physical density: 160 ; aarch64`
    
## Download an APK
- Use a trusted APK source to download the APK you want to install
  - For example, go to https://apkcombo.com/fr/downloader/#package=com.dragonbox.School&device=tablet&sdk=29&arches=arm64-v8a&dpi=160&lang=fr to download the APK of "Login Access: DB Les Noums" for the aarch64 with 160dpi in French.
- If the APK is in XAPK format, unpack every APK that are inside the XAPK
  - With the previous example, you will find inside the XAPK, the following files : "com.dragonbox.School.apk", "config.arm64_v8a.apk" and "UnityDataAssetPack.apk"
    
## Push the APK  
- Use ADB to push the APK by using the `adb install` or `adb install-multiple` command.
  - For example, with the previous APKs, use the following command : `adb install-multiple com.dragonbox.School.apk config.arm64_v8a.apk UnityDataAssetPack.apk`
- If the APKs is large (several MB), wait a few minute.

## Profit !

The application is now installed. By default, it is not on the main desktop, you will find it under all the existing applications.
