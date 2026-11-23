---
name: 120fps-akici-ui
description: 60/120 FPS ultra akici Flutter arayuzleri, CustomPainter optimizasyonu, RepaintBoundary kullanimi ve Apple seviyesinde mikro hareket tasarimi rehberi.
---

# 120 FPS Akici UI ve Hareket Becerisi

Bu beceri; Flutter uygulamalarinda sifir takilma (zero jank), 60/120 FPS akici kaydirma ve Apple kalitesinde mikro etkilesimler olusturmak icin kullanilir.

## 1. Render Optimizasyonu ve Yeniden Cizim Izolasyonu
- **RepaintBoundary:** Canli grafikler, animasyonlu gostergeler ve saat kadranlari gibi surekli cizilen alanlar `RepaintBoundary` ile sarilir. Boylece sadece o kucuk bolge yeniden boyanir, ekranin geri kalani islemciyi yormaz.
- **CustomPainter shouldRepaint Kurali:** `CustomPainter` icinde `shouldRepaint` metodu asla `return true;` yapilamaz. Yalnizca cizim verisi (eski deger != yeni deger) degistiginde `true` dondurulmelidir.
- **Const Constructor:** Degismeyen tum widget'larda `const` zorunludur.

## 2. Mikro Etkilesimler ve Sivi Hareketler
- **Fizik Tabanli Gecisler:** Yapay, sert animasyonlar yerine `Curves.easeOutCubic` veya `Curves.easeInOutCubic` gibi dogal ivmelenmeler kullanilir.
- **Dokunma Tepkisi (Touch Feedback):** Buton tiklandiginda hafif `scale(0.97)` basma hissi veya zarif `InkWell` ripple efekti verilir.
- **Gecikmesiz Gostergeler:** Nabiz dalgasi, adim sayaci veya batarya seviyesi gibi canli metrikler degisirken sayilar `FontFeature.tabularFigures()` ile sabit tutulur.

## 3. Liste ve Ekran Performansi
- **Tembel Yukleme (Lazy Loading):** Tum listeler `ListView.builder` veya `SliverList` ile tembelce render edilir.
- **Gorsel Onbellegi:** Donanim ikonlari ve gorseller `precacheImage()` ile hafizaya onden yuklenir; ekranda ilk acilista beyaz parlama engellenir.
