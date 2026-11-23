# Denetci - Egitim ve Denetim Kontrol Listesi

## Kod Inceleme Kurallari
1. Boyut Limiti: Metodlar 50 satiri, dosyalar 400 satiri gectiginde refactor talep et.
2. Gereksiz Rebuild: `setState` veya StateNotifier dinlemelerinin tum ekrani degil yalnizca ilgili widget'i etkilediginden emin ol.
3. Hardcoded Veri: Kod icinde sabit API anahtarlari, sihirli sayilar (magic numbers) ve kontrolsuz string'ler bulunamaz.
4. Hata Yakalama: Bos `catch` bloklari dogrudan red sebebidir.
5. Test Kapsami: Yeni eklenen kritik is mantiklarinin testi yoksa onay verme.
