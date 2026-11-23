---
name: offline-first-veritabani
description: Cevrimdisi oncelikli (offline-first) yerel veri saklama rehberi. Hive, Isar ve SQLite sifreli veritabani yonetimi, TypeAdapter adaptorleri ve arka plan senkronizasyonu.
---

# Offline-First Yerel Veritabani Becerisi

Bu beceri; internet veya Bluetooth baglantisi olmadigi anlarda dahi uygulamanin kesintisiz calismasini, verilerin yerelde guvenle saklanmasini ve baglanti geldiginde senkronize edilmesini saglar.

## 1. Veritabani Secim Kriterleri
- **Hive / Isar (NoSQL):** Sensor kayitlari, saglik/nabiz loglari, hizli okuma-yazma ve cache gereksinimleri icin (hafif ve asiri hizli).
- **SQLite / Drift (Relational):** Iliskisel karmasik veriler, filtreleme ve raporlama gereksinimleri icin.
- **flutter_secure_storage:** Yalnizca JWT token, sifreleme anahtarlari ve hassas kullanici credential'lari icin.

## 2. Guvenli ve Sifreli Kutu (Encrypted Storage)
- Hassas saglik ve kullanici verileri duz metin olarak diske yazilamaz:
- 256-bit sifreleme anahtari `flutter_secure_storage` icinde saklanir.
- Hive kutusu acilirken bu anahtar ile sifreli acilir (`Hive.openBox('secureBox', encryptionCipher: HiveAesCipher(key))`).

## 3. Offline-First Senkronizasyon Akisi
1. **Veri Uretimi:** Cihazdan veya kullanicidan gelen veri aninda yerel veritabanina kaydedilir (`status: pending`).
2. **Kullanici Deneyimi:** Ekran bekletilmez, yerel veriden beslenerek aninda guncellenir.
3. **Arka Plan Senk:** Baglanti saglandiginda arka planda `pending` kayitlar sunucuya gonderilir ve basarili olanlar `synced` olarak isaretlenir.
