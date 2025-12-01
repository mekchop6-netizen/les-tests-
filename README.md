name: Build Android APK

on:
  push:
    branches: [ main, master ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Install Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: 'stable'

      - name: Flutter pub get
        run: flutter pub get

      - name: Generate Android folders (if missing)
        run: flutter create .

      - name: Run flutter analyze (optional)
        run: flutter analyze || true

      - name: Build APK (release)
        run: flutter build apk --release

      - name: Upload release APK
        uses: actions/upload-artifact@v4
        with:
          name: app-release
          path: build/app/outputs/flutter-apk/app-release.apk
