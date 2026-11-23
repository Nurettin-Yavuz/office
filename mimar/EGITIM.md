# Mimar - Egitim ve Mimari Standartlar

## 1. Evrensel Flutter Klasorleme Standardi
Projelerde dunya genelinde kabul gormus "Feature-First" (Ozellik Odakli) duzen uygulanir:

```text
lib/
├── core/                         # Tum uygulamanin ortak kullandigi yapilar
│   ├── constants/                # Renkler, boyutlar, API URL'leri, sabitler
│   ├── network/                  # HTTP istemcisi (Dio vb.), baglanti yonetimi
│   ├── theme/                    # App temasi (Light & Dark), tipografi
│   └── utils/                    # Genel yardimci fonksiyonlar ve uzantilar (extensions)
│
└── features/                     # Uygulamanin her bir modulu bagimsiz bir klasordur
    └── <ozellik_adi>/            # Ornek: auth, bluetooth, dashboard, settings
        ├── data/                 # Veri katmani (API servisleri, veritabani, DTO modelleri)
        │   ├── models/           # JSON serilestirme modelleri
        │   └── repositories/     # Veri erisim implementasyonlari
        ├── domain/               # Saf is mantigi (Opsiyonel / ihtiyaca gore)
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

## 4. Sadelesme Kurali (Asiri Muhendislikten Kacinma)
- Ihtiyac yokken 5 katmanli yapay soyutlamalar yapilmaz.
- Basit bir ozellik icin gereksiz onlarca bos dosya olusturulmaz.
- Kod okunabilir, anlasilir ve diger muhendislerin kolayca gelistirebilecegi sadelikte tutulur.
