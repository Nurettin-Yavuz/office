---
name: magaza-ve-build-otomasyonu
description: Android APK/AAB ve iOS ipa/archive uretim recetesi. Keystore yonetimi, ProGuard kurallari, CocoaPods onarimlari ve SemVer surumleme otomasyonu.
---

# Magaza ve Build Otomasyonu Becerisi

Bu beceri; Flutter uygulamasinin derleme hatalarina takilmadan Google Play Store ve Apple App Store standartlarinda uretim paketlerine (production builds) donusturulmesi icin kullanilir.

## 1. Android Release Hazirligi
- **Keystore Yapilandirmasi:**
  - `key.properties` dosyasi `android/` altinda tanimlanir ve kesinlikle `.gitignore`'a eklenir.
  - `android/app/build.gradle` icinde `signingConfigs` blogu kurulur.
- **R8 / ProGuard Korumasi:**
  - Kod kucultme (`minifyEnabled true`) ve kaynak optimizasyonu (`shrinkResources true`) acilir.
  - Bluetooth ve JSON serialization paketlerinin isimlerinin karismamasi icin `proguard-rules.pro` kurallari eklenir.
- **Derleme Komutlari:**
  - Test APK: `flutter build apk --release`
  - Play Store Paketi: `flutter build appbundle --release`

## 2. iOS Release Hazirligi (macOS)
- **Deployment Target:** `ios/Podfile` icinde `platform :ios, '13.0'` ayarlanir.
- **Derleme Komutu:**
  - Arşivleme: `flutter build ipa --release --no-codesign`
  - TestFlight / App Store hazirligi Xcode Archive uzerinden veya Fastlane ile yurutulur.

## 3. Paket Kontrol Listesi (Yayin Oncesi)
- [ ] Uygulama surum numarasi `pubspec.yaml` icinde yukseltildi mi? (SemVer)
- [ ] Uygulama ikonlari (`flutter_launcher_icons`) tum boyutlar icin uretildi mi?
- [ ] Acilis ekrani (`flutter_native_splash`) hazir mi?
- [ ] Gereksiz loglar (`print`) ve test API URL'leri kaldirildi mi?
- [ ] Paket boyutu optimize edildi mi? (Hedef: < 25 MB)
