# Yazilimci - Egitim ve Kodlama Standartlari

## Flutter & Dart Gelistirme Prensipleri
1. Null Safety: Force unwrapping (`!`) kullanmaktan kacin; null kontrolleri veya fallback (`??`) kullan.
2. Hata Yonetimi: Servis ve repository katmaninda tum hatalari `Result` veya `Either` tipleri ile yakala; asla sessizce yutma.
3. Bellek Yonetimi: Controller, StreamSubscription, AnimationController ve FocusNode nesnelerini mutlaka `dispose()` et.
4. Veri Donusumu: API yanitlarini dogrudan dinamik tipte degil, tip guvenli DTO / Model siniflarina cevirerek kullan.
