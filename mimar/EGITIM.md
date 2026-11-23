# Mimar - Egitim ve Mimari Standartlar

## Mimari Prensipler
1. Bagimlilik Kurali: Ic katmanlar (Domain), dis katmanlari (Data, UI) bilmez.
2. Feature-First Klasor Yapisi:
   - `lib/core/` (sabitler, temalar, network servisi, utils)
   - `lib/features/<ozellik>/data/` (modeller, veri kaynaklari, repository implementasyonlari)
   - `lib/features/<ozellik>/domain/` (entitiler, use case'ler, repository arayuzleri)
   - `lib/features/<ozellik>/presentation/` (ekranlar, bilesenler, state yonetimi)
3. Immutability: Veri modelleri ve state nesneleri daima `immutable` (degistirilemez) olmalidir (`freezed` veya `@immutable`).
