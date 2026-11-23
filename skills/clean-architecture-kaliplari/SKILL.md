---
name: clean-architecture-kaliplari
description: Flutter Feature-First Clean Architecture pratikleri, Model-Repository-Cubit sablonlari, Result/Failure hata yonetimi ve Dependency Injection standartlari.
---

# Clean Architecture ve Kod Kaliplari Becerisi

Bu beceri; spagetti koddan uzak, genisletilebilir, test edilebilir ve surdurulebilir bir mimari insa etmek icin hazir kod sablonlari sunar.

## 1. Sonuc ve Hata Kalibi (Result / Failure)
Ekrana ham hata firlatmayi engelleyen temel yapi:

```dart
sealed class Failure {
  final String message;
  const Failure(this.message);
}

class NetworkFailure extends Failure {
  const NetworkFailure([super.message = 'Ag baglantisi saglanamadi']);
}

class DeviceFailure extends Failure {
  const DeviceFailure([super.message = 'Donanim yanit vermiyor']);
}

sealed class Result<S, E extends Failure> {
  const Result();
  factory Result.success(S data) = Success<S, E>;
  factory Result.failure(E failure) = Error<S, E>;
}

class Success<S, E extends Failure> extends Result<S, E> {
  final S data;
  const Success(this.data);
}

class Error<S, E extends Failure> extends Result<S, E> {
  final E failure;
  const Error(this.failure);
}
```

## 2. State Yonetimi (Sealed Class Cubit)
```dart
sealed class DeviceState {}

final class DeviceInitial extends DeviceState {}
final class DeviceScanning extends DeviceState {}
final class DeviceConnected extends DeviceState {
  final DeviceInfoModel device;
  DeviceConnected(this.device);
}
final class DeviceError extends DeviceState {
  final String message;
  DeviceError(this.message);
}
```

## 3. Bagimlilik Enjeksiyonu (get_it Standarti)
- Servisler ve repository'ler `lib/core/di/injection.dart` dosyasinda tek bir noktadan kaydedilir:
```dart
final getIt = GetIt.instance;

void setupDependencies() {
  // Services
  getIt.registerLazySingleton<BleService>(() => BleServiceImpl());
  
  // Repositories
  getIt.registerLazySingleton<DeviceRepository>(
    () => DeviceRepositoryImpl(getIt<BleService>()),
  );
  
  // Cubits / Blocs (Factory)
  getIt.registerFactory(() => DeviceCubit(getIt<DeviceRepository>()));
}
```
