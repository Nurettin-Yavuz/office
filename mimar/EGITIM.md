# Mimar - Egitim ve Mimari Standartlar

## 1. Evrensel Flutter Klasorleme Standardi
Projelerde dunya genelinde kabul gormus "Feature-First" (Ozellik Odakli) duzen uygulanir:

```text
lib/
├── core/                         # Tum uygulamanin ortak kullandigi yapilar
│   ├── constants/                # Renkler, boyutlar, API URL'leri, sabitler
│   ├── network/                  # HTTP istemcisi (Dio vb.), baglanti yonetimi
│   ├── errors/                   # Failure ve Result siniflari
│   ├── theme/                    # App temasi (Light & Dark), tipografi
│   └── utils/                    # Genel yardimci fonksiyonlar ve uzantilar (extensions)
│
└── features/                     # Uygulamanin her bir modulu bagimsiz bir klasordur
    └── <ozellik_adi>/            # Ornek: auth, bluetooth, dashboard, settings
        ├── data/                 # Veri katmani (API servisleri, veritabani, DTO modelleri)
        │   ├── models/           # JSON serilestirme modelleri
        │   └── repositories/     # Veri erisim implementasyonlari
        ├── domain/               # Saf is mantigi (Sadece karmasik kurallarda)
        │   └── repositories/     # Repository arayuzleri (abstract class)
        └── presentation/         # Kullanicinin gordugu arayuz ve durum
            ├── cubit/            # State yonetimi (veya bloc)
            ├── screens/          # Tam sayfa ekranlar
            └── widgets/          # Bu ozellige ozel tekrar kullanilabilir parcalar
```

## 2. Dosya Isimlendirme Kurallari (Net ve Anlasilir)
Sacma sapan, kafa karistirici veya asiri uzun isimler kesinlikle yasaktir. Tum dosyalar kucuk harf ve alt cizgi (`snake_case`) ile yazilir ve ne yaptigini dosya sonundaki ekten belli eder:

- **Ekran Dosyalari:** `<ozellik>_screen.dart` (Ornek: `login_screen.dart`, `device_settings_screen.dart`)
- **Parca / Bilesen Dosyalari:** `<bilesen>_widget.dart` veya `<bilesen>_card.dart` (Ornek: `user_avatar_widget.dart`, `battery_indicator_card.dart`)
- **State Yonetimi Dosyalari:** `<ozellik>_cubit.dart` ve `<ozellik>_state.dart` (Ornek: `bluetooth_cubit.dart`, `bluetooth_state.dart`)
- **Veri Modelleri:** `<nesne>_model.dart` (Ornek: `user_model.dart`, `device_info_model.dart`)
- **Servis / Baglanti Dosyalari:** `<islev>_service.dart` (Ornek: `auth_api_service.dart`, `ble_scanner_service.dart`)
- **Depo / Repository Dosyalari:** `<nesne>_repository.dart` (Ornek: `device_repository.dart`)

## 3. Ekran ve Arka Plan Ayriligi (Spagetti Koda Sifir Tolerans)
- Ekran widget'larinin (`_screen.dart`) icinde **ASLA**:
  - `http.get` veya `Dio.get` gibi dogrudan network istekleri bulunamaz.
  - SQLite, Hive veya SharedPreferences dogrudan cagrilamaz.
  - Bluetooth donanim tarama/baglanma kodlari yazilamaz.
- Ekran sadece durumu dinler (`BlocBuilder` vb.) ve kullanicinin etkilesimini Cubit/Bloc'a iletir. Tum is mantigi servislerde ve state katmaninda calisir.

## 4. Insa Sirasi Sozlesmesi (Build Sequence)
Mimar, Yazilimci'ya is devrederken her zaman su sirayi zorunlu kilar:
1. **Adim 1 - Veri Modelleri:** Once JSON/veritabani modelleri ve tip guvenligi olusturulur (`_model.dart`).
2. **Adim 2 - Servis ve Repository:** Veriyi getiren/kaydeden katman yazilir (`_service.dart`, `_repository.dart`).
3. **Adim 3 - State Yonetimi:** Veriyi yoneten ve ekran durumlarini belirleyen Cubit/Bloc kurulur (`_cubit.dart`, `_state.dart`).
4. **Adim 4 - UI Ekran ve Widget'lar:** En son hazir olan State yapisi ekrana baglanir (`_screen.dart`, `_widget.dart`).
*Hicbir zaman model olmadan ekran yazilmaya baslanamaz.*

## 5. Hata Yonetim Standardi (Result / Failure Modeli)
- Uygulamanin kirmizi/gri hata ekranina dusmesi kesinlikle engellenir.
- Ekrana ham hata (`throw Exception`, `SocketException`) firlatilmaz.
- Servis ve Repository katmaninda tum hatalar `try-catch` ile yakalanir, anlamli bir `Failure` nesnesine donusturulur.
- Tum servis fonksiyonlari `Result<Failure, T>` veya `Either<Failure, T>` dondurur. Ekran bu sonuca gore basarili veriyi gosterir veya kullanici dostu bir snackbar/dialog gosterir.

## 6. Pragmatik Yaklasim ve Burokrasi Yasagi (No-Boilerplate)
- Basit bir veri listeleme veya CRUD islemi icin gereksiz yere 10 ayri dosya (ayri DTO, Mapper, UseCase, Entity, Interface) acilmaz.
- Standart ozelliklerde dogrudan `Repository -> Cubit -> Screen` akisi yeterlidir.
- Sadece gercekten karmaşık bir hesaplama, coklu servis koordinasyonu veya karmasik bir is kurali varsa UseCase katmani eklenir. Proje yapay katmanlarla sisirilmez.
