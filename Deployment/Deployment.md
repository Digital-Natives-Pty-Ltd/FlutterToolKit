
# **Step-by-Step Guide for App Deployment Pipeline**

---

## **1. Preparing for Deployment**

### **1.1. Prerequisites**
1. Ensure your Flutter app is thoroughly tested.
2. Remove unnecessary code, debug logs, and comments.
3. Update the `pubspec.yaml` file with the correct app name, version, and description.
4. Configure the app icon and splash screen:
   - Use [flutter_launcher_icons](https://pub.dev/packages/flutter_launcher_icons) for app icons.
   - Use [flutter_native_splash](https://pub.dev/packages/flutter_native_splash) for splash screens.
5. Verify all dependencies and assets are properly configured:
   ```bash
   flutter pub get
   ```
6. Ensure the app runs correctly on all targeted platforms:
   ```bash
   flutter run
   ```

---

## **2. Android Deployment Pipeline**

### **2.1. Configure Android App**
1. Update `android/app/build.gradle`:
   - Set `applicationId` to your app’s unique identifier (e.g., `com.example.myapp`).
   - Set `minSdkVersion` and `targetSdkVersion` to align with your app’s requirements.

2. Update the version in `android/app/build.gradle`:
   ```groovy
   versionCode 1
   versionName "1.0.0"
   ```
   Align this with the `pubspec.yaml` version.

3. Add signing configurations:
   - Generate a keystore:
     ```bash
     keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
     ```
   - Place the `my-release-key.jks` file in `android/app/`.
   - Update `android/app/build.gradle` with signing information:
     ```groovy
     signingConfigs {
         release {
             keyAlias 'my-key-alias'
             keyPassword 'your-key-password'
             storeFile file('my-release-key.jks')
             storePassword 'your-store-password'
         }
     }
     buildTypes {
         release {
             signingConfig signingConfigs.release
         }
     }
     ```

### **2.2. Build the APK and AAB**
1. Clean the project:
   ```bash
   flutter clean
   ```
2. Build the APK:
   ```bash
   flutter build apk --release
   ```
   Locate it at `build/app/outputs/flutter-apk/app-release.apk`.

3. Build the AAB:
   ```bash
   flutter build appbundle --release
   ```
   Locate it at `build/app/outputs/bundle/release/app-release.aab`.

### **2.3. Deploy to Google Play Store**
1. Register as a developer at the [Google Play Console](https://play.google.com/console/).
2. Create a new app and provide required details (name, description, category, etc.).
3. Upload the AAB file to the "Production" track.
4. Complete content rating and review steps.
5. Publish the app.

---

## **3. iOS Deployment Pipeline**

### **3.1. Configure iOS App**
1. Open `ios/Runner.xcworkspace` in Xcode.
2. Set the app’s bundle identifier:
   - Navigate to "General" → "Bundle Identifier" (e.g., `com.example.myapp`).

3. Configure signing and capabilities:
   - Select your Apple Developer Team.
   - Enable "Automatically manage signing."

4. Update the version and build number in Xcode’s "General" tab.

### **3.2. Build the iOS App**
1. Run the following command to create a release build:
   ```bash
   flutter build ios --release
   ```
2. Test the build on a physical device or simulator.

### **3.3. Upload to App Store**
1. Archive the app:
   - In Xcode, select "Product" → "Archive."
   - Ensure the correct scheme (`Runner`) is active.
2. Validate the archive:
   - Use the "Distribute App" option and select "App Store Connect."
3. Upload the app:
   - Use Xcode or the Transporter app.
4. Configure the app in App Store Connect:
   - Add screenshots, metadata, and app-specific information.
5. Submit for review and wait for approval.

---

## **4. Automating Deployment with GitHub Actions**

### **4.1. Setting Up GitHub Actions**
1. Create a `.github/workflows` directory in your repository.
2. Add a workflow YAML file, such as `deploy.yml`:
   ```yaml
   name: Deploy Flutter App

   on:
     push:
       branches:
         - main

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
       - name: Checkout code
         uses: actions/checkout@v3

       - name: Setup Flutter
         uses: subosito/flutter-action@v2
         with:
           flutter-version: 'stable'

       - name: Install dependencies
         run: flutter pub get

       - name: Build APK
         run: flutter build apk --release

       - name: Build AAB
         run: flutter build appbundle --release

       - name: Upload to Google Play
         uses: r0adkll/upload-google-play@v1
         with:
           serviceAccountJson: ${{ secrets.GOOGLE_PLAY_JSON }}
           packageName: com.example.myapp
           releaseFiles: build/app/outputs/bundle/release/app-release.aab
   ```
3. Save the workflow file and commit it to your repository.

### **4.2. Setting Up Secrets**
1. In the GitHub repository, go to "Settings" → "Secrets and variables."
2. Add the following secrets:
   - `GOOGLE_PLAY_JSON`: The service account JSON key for Google Play Console.
   - `APPLE_CERTIFICATE`: Your Apple distribution certificate (for iOS).
   - `APPLE_PRIVATE_KEY`: The associated private key.

---

## **5. Post-Deployment Checklist**
1. Monitor analytics using Firebase or App Center.
2. Collect user feedback via reviews or in-app surveys.
3. Address any issues reported by users promptly.
4. Plan for regular updates and feature enhancements.

---
