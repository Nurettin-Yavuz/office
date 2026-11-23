# Tasarimci - Egitim ve UI/UX Standartlari

## 1. "Yapay Zeka Kliselerini" Reddetme Rehberi
Bir tasarimin "yapay zeka sablonu" gibi gorunmesini engelleyen kesin kurallar:
- **Renk Patlamasi Yasagi:** Ekranda ayni anda 4-5 canli renk, cirtlak neon gradyanlar veya rastgele mor-mavi arka planlar kesinlikle kullanilamaz.
- **Tekrar Eden Kutu Sendromu Yasagi:** Ekrandaki her veriyi ayni yuvarlak beyaz kartin icine koyup alt alta siralamak yasaktir. Bilgi tipine gore ritim kurulur:
  - Ana aksiyonlar net, zarif kartlarda.
  - Listeler kutusuz, aralarinda hafif bosluk ve ince ayraclar bulunan temiz satirlarda.
  - Detay metinleri arka planla uyumlu, sessiz tipografik bloklarda.
- **Agir Golgeler Yasagi:** `blurRadius: 20` gibi camurlu buyuk golgeler yasaktir. Yuzeyler arasi gecis hafif ton farkiyla (`surfaceContainer`) veya `0.5 - 1 dp` cok ince seffaf cercevelerle saglanir.

## 2. Renk ve Yuzey Standardi (60-30-10 Kurali)
Tasarimci renkleri su orana gore dengeler:
- **%60 Ana Arka Plan:** Koyu modda derin arduvaz/grafit (`#0F141C`, `#121824`), acik modda sicak kirik beyaz/tas tonlari (`#F8F9FA`, `#F3F4F6`).
- **%30 Destekleyici Yuzeyler:** Kartlar, alt barlar, modal sayfalar icin ana zeminden sadece 1-2 ton acik/koyu yuzeyler.
- **%10 Tek Vurgu Rengi (Accent):** Yalnizca kullanicinin tiklamasi gereken butonlar, secili sekmeler veya kritik durumlar icin tek bir sofistike renk (ornegin: sicak kiremit, zeytin yesili, saf kobalt veya endustriyel kehribar).

## 3. Tum Telefon Boyutlarina Gercek Responsive Uyum
Tasarimlarin tum ekranlarda (iPhone SE'den buyuk Pro Max'lere ve tabletlere) sorunsuz calisma kurallari:
- **Sabit Yukseklik Yasagi:** Ekrani kaplayan bilesenlere asla `Container(height: 380)` gibi korlemesine sabit piksel verilmez.
- **Tasma Korumasi:** Form ekranlari ve dikey bilesenler her zaman `SingleChildScrollView` ve `SafeArea` ile sarilir; klavye acildiginda tek bir piksel dahi tasma (`RenderFlex overflow`) yapamaz.
- **Esnek Boyutlandirma:** Esnek alanlarda `LayoutBuilder`, `Flexible` ve `Expanded` dogru hiyerarsiyle kullanilir.
- **Maksimum Genislik Siniri:** Genis ekranlarda veya tabletlerde kartlarin ucube gibi saga sola esnemesini engellemek icin `ConstrainedBox(constraints: BoxConstraints(maxWidth: 600))` kullanilir.

## 4. Tipografi ve 8-Point Izgara (Grid) Sistemi
- Tum bosluklar ve padding degerleri 8'in katlari veya 4'un ara degerleridir (`4, 8, 12, 16, 24, 32 dp`). Rastgele sayilar (`13 dp`, `19 dp`) yasaktir.
- Tum font boyutlari `Theme.of(context).textTheme` uzerinden alinir:
  - Basliklar: `titleLarge` / `headlineSmall` (net, kisa, guclu).
  - Govde: `bodyMedium` (yuksek okunabilirlik, ferah satir araligi).
  - Ikincil aciklamalar: `bodySmall` / `labelMedium` (hafif kisik renk kontrastinda).

## 5. Dokunma Ergonomisi ve Mikro Geri Bildirim
- **Hit Area:** Tiklanabilir her bilesen parmakla kolay basilabilmesi icin en az `48x48 dp` alana sahip olmalidir.
- **Dokunma Hissi:** Tiklanan butonlarda abartili ziplamalar yerine akici, Apple standartlarinda hafif opaklik veya zarif `InkWell` ripple dalgalanmasi kullanilir.
- **Bos ve Yuklenme Durumu:** Veri beklenirken cirkin yuklenme donusu yerine ekrana ozel zarif iskelet (`Shimmer`) cizilir. Veri yoksa ozenle hazirlanmis minimal bir `EmptyStateWidget` gosterilir.
