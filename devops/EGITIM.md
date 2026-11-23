# DevOps - Egitim ve Derleme Standartlari

## Derleme ve Surum Kurallari
1. SemVer (Semantic Versioning): Surum numaralandirmasini `major.minor.patch+buildNumber` standardinda tut.
2. Bagimlilik Guvenligi: `pubspec.yaml` icinde guvensiz veya terk edilmis paketleri tespit edip alternatiflerini oner.
3. Platform Temizligi: Gerekli durumlarda `flutter clean` sonrasi `flutter pub get` ve `pod install` adimlarini sirayla islet.
4. Gizli Bilgi: Anahtar dosyalari (`keystore`, `provisioning profile`) ve API token'lari asla kaynak kod reposuna commit ettirme.
