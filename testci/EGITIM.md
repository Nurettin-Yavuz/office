# Testci - Egitim ve Test Standartlari

## 1. Statik Kod Analizi Kapisi (flutter analyze)
Testci, testleri kosmadan once kod kalitesini dogrular:
- Komut: `flutter analyze`
- Kural: Tek bir `warning` veya `error` bulunamaz. Uyarilar varsa Testci testi durdurur, raporu Yonetici ve Yazilimci'ya ileterek kodu temizletir.

## 2. mocktail ile Birim Testi (Unit Test) Standardi
Kod uretimi (`build_runner`) gerektirmeyen `mocktail` kutuphanesi varsayilan mocklama aracidir:
- **Mock Sinifi:**
```dart
import 'package:mocktail/mocktail.dart';

class MockAuthRepository extends Mock implements AuthRepository {}
```
- **Fallback Kaydi (Gerektiginde):** Ozel nesneler parametre olarak geciliyorsa `setUpAll()` icinde `registerFallbackValue(FakeUserModel())` tanimlanir.
- **Arrange - Act - Assert (AAA) Kalibi:**
```dart
test('kullanici girisi basarili oldugunda AuthSuccess state donmeli', () async {
  // Arrange
  when(() => mockAuthRepository.login('test@test.com', '123456'))
      .thenAnswer((_) async => Result.success(testUser));

  // Act
  await authCubit.login('test@test.com', '123456');

  // Assert
  expect(authCubit.state, isA<AuthSuccess>());
  verify(() => mockAuthRepository.login('test@test.com', '123456')).called(1);
});
```

## 3. Ikiz Klasor Standardi (Mirror Directory Tree)
`test/` dizini `lib/` dizininin birebir ayna kopyasi olmak zorundadir:
- Kaynak: `lib/features/auth/data/repositories/auth_repository.dart`
- Test: `test/features/auth/data/repositories/auth_repository_test.dart`
- Tum test dosyalari istisnasiz `_test.dart` son ekiyle biter.

## 4. Widget Testlerinde Cokme Korumasi
Widget'larin `No Directionality widget found` veya `No MediaQuery found` hatalariyla patlamamasi icin:
- **MaterialApp Sarmalama:**
```dart
testWidgets('Giris butonu tiklandiginda yukleniyor gosterilmeli', (tester) async {
  await tester.pumpWidget(
    MaterialApp(
      home: Scaffold(
        body: LoginScreen(),
      ),
    ),
  );

  // Animasyonlarin ve gecislerin bitmesini bekle
  await tester.pumpAndSettle();

  // Semantik Finder kullanimi
  expect(find.byType(ElevatedButton), findsOneWidget);
});
```

## 5. Zorunlu Uc Senaryolar (Edge Cases) Kontrol Listesi
Testci bir ozelligi test ederken asagidaki negatif durumlari test etmeden onay vermez:
1. **Network Kesintisi:** Servis `NetworkFailure` dondugunde Cubit ekrana dogru hata mesajini aktariyor mu?
2. **Bos Liste Durumu:** API bos liste dondugunde ekranda `EmptyStateWidget` beliriyor mu?
3. **Gecersiz / Null Veri:** Eksik veya hatali veri geldiginde uygulama cökmek yerine guvenli fallback gosteriyor mu?
4. **Form Dogrulama:** Gecersiz e-posta veya eksik sifre girildiginde buton kilitleniyor veya uyari veriyor mu?
