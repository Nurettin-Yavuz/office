# DevOps - Egitim ve Derleme Standartlari

## 1. Cerrahi Mudahale ve Hata Cozum Kurali
- DevOps'un gorevi kod yazmak degil, derleme yolunu acmaktir.
- Bir derleme hatasinda (`flutter build apk` vb.) asla is mantigi veya mimari degistirilmez.
- Yalnizca ilgili konfigurasyon (`build.gradle`, `Podfile`, `pubspec.yaml`) cerrahi olarak duzeltilir.

## 2. Android & Gradle Onarim Protokolu
Android derleme hatalarinda su sira isletilir:
1. **Gradle Onbellegi Temizleme:**
   ```bash
   cd android && ./gradlew clean && cd ..
   ```
2. **Java ve Gradle Uyumu:** Java 17 ortam degiskenlerinin tanimli oldugunu dogrula.
3. **minSdkVersion / NDK Hatalari:** Eger bir eklenti daha yuksek bir SDK istiyorsa, `android/app/build.gradle` icinde `minSdkVersion` degeri guvenle guncellenir.
4. **AndroidX ve Namespace Kontrolu:** Eski Android projelerinde eksik `namespace` veya AndroidX ayarlarini tamamla.

## 3. iOS & CocoaPods Onarim Protokolu (macOS)
iOS derleme ve pod kilitlenmelerinde su sira isletilir:
1. **Derin Pod Temizligi ve Yeniden Kurulum:**
   ```bash
   cd ios && rm -rf Pods Podfile.lock && pod repo update && pod install && cd ..
   ```
2. **Deployment Target Ayari:** Eger bir pod daha yeni iOS surumu istiyorsa, `ios/Podfile` icindeki `platform :ios, '13.0'` (veya ustu) satirini guncelle.

## 4. Bagimlilik Cakismasi Cozumu (pubspec.yaml)
Paket surum uyumsuzluklarinda (`Because X depends on Y and Z depends on...`):
1. Bagimlilik agacini tara:
   ```bash
   flutter pub deps --style=compact | grep <sorunlu_paket>
   ```
2. Gecici uyum icin gerekiyorsa `dependency_overrides` kullan.
3. Bozuk yerel pub paket onbellegini onar:
   ```bash
   flutter clean && flutter pub cache repair && flutter pub get
   ```

## 5. Surumleme ve Uretim Paketleme
- Sürüm Numarasi: `pubspec.yaml` icinde `version: 1.0.0+1` seklinde SemVer (`major.minor.patch+buildNumber`) formatinda ilerletilir.
- **Android Paketleri:**
  - Test/Cihaza Yukleme: `flutter build apk --release`
  - Google Play Store Yayini: `flutter build appbundle --release`
- **iOS Paketi:**
  - Arşivleme: `flutter build ipa --release --no-codesign`
- **Paket Raporu:** Cikan dosyanin boyutu (MB) ve tam disk yolu Yoneticiye tek satirda bildirilir.
