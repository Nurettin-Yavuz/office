# Yazilimci - Egitim ve Kodlama Standartlari

## 1. Mimarin Insa Sirasi (Build Sequence) Kurali
Yazilimci, gelistirme yaparken asagidaki siralama kuralina tavizsiz uyar:
1. **Adim 1 - Model Katmani:** `features/<ozellik>/data/models/<nesne>_model.dart`
   - `fromJson` / `toJson` donusumleri tip guvenli yapilir.
   - Alanlar mumkunse `final` tutulur (Immutability).
2. **Adim 2 - Servis & Repository Katmani:** `features/<ozellik>/data/repositories/<nesne>_repository.dart`
   - API istekleri veya yerel veritabani (Hive, SQLite) kodlanir.
   - Hatalar `try-catch` icinde yakalanip `Failure` dondurur, asla UI'a ham hata sizdirilmaz.
3. **Adim 3 - State Yonetimi:** `features/<ozellik>/presentation/cubit/<ozellik>_cubit.dart`
   - Modern `sealed class` ile State yapisi kurulur.
   - Servisten gelen `Result` tipine gore ekran durumu `Loading`, `Success` veya `Error` yapilir.
4. **Adim 4 - UI Ekranlari:** `features/<ozellik>/presentation/screens/<ozellik>_screen.dart`
   - Hazir olan Cubit, `BlocBuilder` veya `BlocConsumer` ile arayuze baglanir.

## 2. Asenkron Guvenlik Kurali (Async Gap Protection)
- Herhangi bir `await` cagrildiktan sonra `context` uzerinden `Navigator`, `ScaffoldMessenger` veya `Theme.of(context)` cagrilacaksa:
```dart
// YANLIS - Uygulamayi cokertir:
await authRepository.login(email, password);
Navigator.pushNamed(context, '/home');

// DOGRU - Standart koruma:
await authRepository.login(email, password);
if (!context.mounted) return;
Navigator.pushNamed(context, '/home');
```

## 3. Modern Dart 3+ Standartlari
- State siniflari her zaman `sealed class` ile tanimlanir:
```dart
sealed class AuthState {}
final class AuthInitial extends AuthState {}
final class AuthLoading extends AuthState {}
final class AuthSuccess extends AuthState {
  final UserModel user;
  AuthSuccess(this.user);
}
final class AuthFailure extends AuthState {
  final String message;
  AuthFailure(this.message);
}
```
- UI icinde durum kontrolleri modern `switch` ile yapilir:
```dart
return switch (state) {
  AuthInitial() => const SizedBox.shrink(),
  AuthLoading() => const Center(child: CircularProgressIndicator()),
  AuthSuccess(:final user) => UserProfileWidget(user: user),
  AuthFailure(:final message) => ErrorMessageWidget(message: message),
};
```

## 4. Bellek Hijyeni ve Dispose Disiplini
- Acik birakilan her kaynak bellek sizintisina (memory leak) yol acar.
- Asagidaki nesneler kullanildigi anda ilgili `StatefulWidget`'in `dispose()` metodunda kapatilmalidir:
  - `TextEditingController.dispose()`
  - `AnimationController.dispose()`
  - `ScrollController.dispose()`
  - `FocusNode.dispose()`
  - `StreamSubscription.cancel()`
  - `Timer.cancel()`

## 5. Performans ve Render Optimizasyonu
- **Const Constructor:** `const Text(...)`, `const SizedBox(...)`, `const Padding(...)` gibi degismeyen bilesenlerde `const` kullanimi zorunludur.
- **Liste Performansi:** 5'ten fazla eleman veya dinamik liste iceren yerlerde asla `Column(children: ...)` kullanilmaz; daima `ListView.builder` veya `CustomScrollView` + `SliverList` kullanilir.

## 6. Null Safety ve Hata Koruma Kurali
- Kodda `user!.name` veya `list.first!` gibi `!` (force unwrap) isaretleri kesinlikle yasaktir.
- Fallback operasyonu (`user?.name ?? 'Bilinmiyor'`) veya erken donus (`guard clause`):
```dart
final user = currentUser;
if (user == null) {
  return const EmptyStateWidget();
}
```
kullanilmalidir.
