# Building Pondy for Android

Pondy's Android project builds **without Gradle, Android Studio, or GitHub Actions**.

## Requirements
- Linux development environment.
- Java 17 JDK.
- Android Build Tools 36: **aapt2**, **d8**, **zipalign**, **apksigner**.
- Android API 36 platform archive providing **android.jar**.
- Python 3 for assembling the standalone HTML editor.

Expected local archives:

~~~text
build-tools_r36_linux.zip
platform-36_r02.zip
OpenJDK17U-jdk_x64_linux_hotspot_17.0.20.1_1.tar.gz
~~~

The supplied direct build script uses /mnt/data by default; PONDWATER_ARCHIVES can point at a different archive directory.

## Build commands

~~~bash
python3 assemble.py
chmod +x build.sh
./build.sh
~~~

The build process compiles the Activity with javac, converts its classes into Dalvik bytecode with d8, packages the manifest, resources and HTML assets using aapt2, aligns the APK, signs it and verifies its signature.

## Verification

~~~bash
unzip -t PondWater-release.apk
aapt2 dump badging PondWater-release.apk
apksigner verify --verbose --print-certs PondWater-release.apk
unzip -l PondWater-release.apk | grep classes.dex
~~~

## Signing continuity

Keep the original private signing keystore if you want later builds to install over existing Android installations. **Never commit or upload a private keystore, password file, or signing-key backup.** The public repository does not include release-signing credentials.

The Android host source, asset assembly script and modular editor source are included in the full 1.1.0 source distribution. Physical Android testing is recommended before wider distribution.
