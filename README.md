# Office - Otonom Flutter Yazilim ve Muhendislik Ekibi

Bu calisma alani, Flutter projelerini dunya standartlarinda gelistirmek, test etmek, denetlemek ve yayina hazirlamak uzere ozel egitilmis 7 kisilik otonom yapay zeka ofisidir.

Her rol kendi klasoru altinda `ROL.md` (gorev sinirlari) ve `EGITIM.md` (teknik anayasa ve kurallar) dosyalariyla bagimsiz olarak egitilmistir.

---

## Ofis Rolleri ve Egitim Standartlari

### 1. [Yonetici](yonetici/) - Ofis ve Proje Yoneticisi
- **Dogrudan Otonom Icraat:** Kullanicidan gelen istegi onay beklemeden ekibe dagitir, calisan sonucu sunar.
- **Otonom Hata Dongusu:** Ara teknik hatalarla kullaniciyi mesgul etmez; Testci ve Denetci ile Yazilimci arasindaki donguyu arka planda cozer.
- **Kapsam Korumasi:** Calisan mevcut koda dokunmadan sadece istenen alana cerrahi mudahale yaptirir.
- **Standart Teslimat:** Is bittiginde (1) Yapilan Is, (2) Dosyalar, (3) flutter analyze ve test sonucu ile raporlar.

### 2. [Mimar](mimar/) - Sistem ve Cozum Mimari
- **Evrensel Klasorleme:** Feature-First (`lib/core/` + `lib/features/<ozellik>/`) duzenini zorunlu kilar.
- **Net Dosya Isimlendirme:** Amaci belli olan snake_case formatini uygular (`_screen`, `_widget`, `_cubit`, `_model`, `_service`).
- **Insa Sirasi (Build Sequence):** Yazilimciya kodlama sirasini verir: Once Model -> Servis/Repository -> Cubit/State -> En son Ekran.
- **Result / Failure Modeli:** Kirmizi ekran cokmelerini onlemek icin ham hata firlatmayi engeller.
- **Pragmatik Yaklasim:** Basit isler icin yapay katmanlar ve onlarca bos dosya acilmasini yasaklar.

### 3. [Tasarimci](tasarimci/) - Arayuz ve UI/UX Tasarimcisi
- **Yapay Zeka Kliselerini Reddetme:** Cirtlak mor/mavi gradyanlar, kart icine kart gommek ve goz boyayan gereksiz renk patlamalarini yasaklar.
- **60-30-10 Renk Orani:** %60 notr zemin, %30 destekleyici yuzeyler, %10 tek bir sofistike vurgu rengi.
- **Basparmak Bolgesi (Thumb-Zone):** Ana butonlari ekranin alt 1/3'luk rahat basilan alaninda toplar.
- **Esmerkezli Koseler (Concentric Radius):** Dis kose = Ic kose + padding formuluyle kusursuz optik geometri saglar.
- **Sayi Titremesi Korumasi:** Sayaclarda `FontFeature.tabularFigures()` zorunluluguyla metin titremesini onler.
- **Sifir Tasma Garantisi:** `SingleChildScrollView` ve `SafeArea` ile klavye acilsa dahi sifir `RenderFlex overflow`.

### 4. [Yazilimci](yazilimci/) - Flutter Yazilim Muhendisi
- **Mimarin Insa Sirasina Uyum:** Once Model -> Servis -> Cubit -> Ekran sirasiyla kodlar.
- **Asenkron Guvenlik:** Herhangi bir `await` sonrasinda `if (!context.mounted) return;` kontrolunu zorunlu kilar.
- **Modern Dart 3+:** State yapilarini `sealed class` ile kurar, kontrolleri `switch` ifadeleriyle yapar.
- **Bellek Hijyeni:** Tum controller, subscription ve timer nesnelerini `dispose()` icinde mutlak surette kapatir.
- **Performans:** Degismeyen widget'larda `const` zorunlulugu, dinamik listelerde `ListView.builder` kullanimi.
- **Kati Null Safety:** Zoraki unwrap (`!`) yasaktir; fallback (`??`) ve guard clause kullanir.

### 5. [Testci](testci/) - Kalite Guvence ve Test Muhendisi
- **flutter analyze Sifir Tolerans:** Tek bir linter uyarisi veya sari cizgi dahi varsa testi durdurur, kodu temizletir.
- **mocktail Standardi:** `build_runner` ve `mockito` hantalligina girmeden saf Dart ile hizli ve guvenilir unit testleri yazar.
- **Ikiz Test Agaci:** `test/` dizini `lib/` dizininin birebir kopyasidir (`_test.dart`).
- **Cokmesiz Widget Testleri:** Widget'lari `MaterialApp` ile sarar, `pumpAndSettle()` ile animasyonlarin bitmesini bekler.
- **Zorunlu Uc Senaryolar (Edge Cases):** Internet kopmasi, 500 hatasi, bos liste ve hatali form girislerini zorunlu test eder.

### 6. [Denetci](denetci/) - Bas Denetci ve Kod Inceleme Uzmani
- **Cerrahi Inceleme:** `git diff` ve `git diff --staged` uzerinden yalnizca degisen dosyalari denetler.
- **Uc Kademeli Karar:** `[ENGEL]`, `[UYARI]`, `[NOT]` olarak raporlar. Tek bir `[ENGEL]` varsa onay vermez.
- **Sessiz Hata Avciligi:** Bos `catch` bloklarini ve yutulan hatalari aninda yakalar ve reddeder.
- **_build*() Metot Yasagi:** Ekran icinde widget donduren fonksiyonlari ayri `StatelessWidget` sinifina boldurur.
- **Onay Sonrasi GitHub Gonderimi:** Kod onaylandiginda (`APPROVE`), anlamli bir Turkce commit mesajiyla Git commit ve push islemini yurutur.

### 7. [DevOps](devops/) - CI/CD ve Derleme Operasyon Sorumlusu
- **Cerrahi Mudahale:** Kodu veya mimariyi asla degistirmez; yalnizca derleme ayarlarini duzelterek build'i gecirir.
- **Android & Gradle Protokolu:** `gradlew clean`, Java 17 ve `minSdkVersion` uyumsuzluklarini onarir.
- **iOS & CocoaPods Protokolu:** Pod ve lock kilitlenmelerinde derin temizlik yapar (`rm -rf Pods Podfile.lock && pod install`).
- **Bagimlilik Cozumu:** Paket cakismalarini (`version solving failed`) `dependency_overrides` ve cache onarimiyla cozer.
- **Surumleme ve Paketleme:** SemVer ile Android APK/AAB ve iOS archive paketlerini derler, boyut ve yol raporu sunar.
