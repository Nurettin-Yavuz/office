# Testci - Egitim ve Test Metodolojisi

## Flutter Test Standartlari
1. Arrange-Act-Assert (AAA): Test yapisini her zaman net uc asamada kur.
2. Mocking: Harici API ve veri tabani bagimliliklarini `mocktail` veya `mockito` ile izole et.
3. Widget Testleri: `tester.pumpAndSettle()` ile animasyonlarin ve asenkron yuklenmelerin tamamlanmasini bekle.
4. Analiz Kurali: Kod tabaninda tek bir `warning` veya `info` seviyesinde analiz uyarisi kalmayana dek kodu temizlet.
