# Tasarimci - Egitim ve UI/UX Standartlari

## 1. "Yapay Zeka Kliselerini" Reddetme Rehberi
Bir tasarimin "yapay zeka sablonu" gibi gorunmesini engelleyen kesin kurallar:
- **Renk Patlamasi Yasagi:** Ekranda ayni anda 4-5 canli renk, cirtlak neon gradyanlar veya rastgele mor-mavi arka planlar kesinlikle kullanilamaz.
- **Tekrar Eden Kutu Sendromu Yasagi:** Ekrandaki her veriyi ayni yuvarlak beyaz kartin icine koyup alt alta siralamak yasaktir. Bilgi tipine gore ritim kurulur:
  - Ana aksiyonlar net, zarif kartlarda.
  - Listeler kutusuz, aralarinda hafif bosluk ve ince ayraclar bulunan temiz satirlarda.
  - Detay metinleri arka planla uyumlu, sessiz tipografik bloklarda.
- **Kart Icinde Kart Yasagi (No Nested Cards):** Bir kartin icine asla ikinci bir kart gomulmez (`Card -> Card` yasaktir). Ic bolumler sadece ince bir `divider` veya boslukla ayrilir.
- **Agir Golgeler Yasagi:** `blurRadius: 20` gibi camurlu buyuk golgeler yasaktir. Yuzeyler arasi gecis hafif ton farkiyla (`surfaceContainer`) veya `0.5 - 1 dp` cok ince seffaf cercevelerle saglanir.

## 2. Renk ve Yuzey Standardi (60-30-10 Kurali)
Tasarimci renkleri su orana gore dengeler:
- **%60 Ana Arka Plan:** Koyu modda derin arduvaz/grafit (`#0F141C`, `#121824`), acik modda sicak kirik beyaz/tas tonlari (`#F8F9FA`, `#F3F4F6`).
- **%30 Destekleyici Yuzeyler:** Kartlar, alt barlar, modal sayfalar icin ana zeminden sadece 1-2 ton acik/koyu yuzeyler.
- **%10 Tek Vurgu Rengi (Accent):** Yalnizca kullanicinin tiklamasi gereken butonlar, secili sekmeler veya kritik durumlar icin tek bir sofistike renk (ornegin: sicak kiremit, zeytin yesili, saf kobalt veya endustriyel kehribar).

## 3. Basparmak Bolgesi (Thumb-Zone) ve Ergonomi
- **Tek Elle Kullanim:** Kullanicinin tek elle telefonu tutarken rahat ulasabilecegi alan ekranin alt 1/3'luk dilimidir.
- **Ana Aksiyonlar (CTA):** Form onaylama, devam etme, kaydetme gibi birincil butonlar her zaman alt kisimda (Bottom Action Bar veya alt bolgede) yer almalidir.
- **Yikici / Tehlikeli Aksiyonlar:** "Sil", "Hesabi Kapat", "Cikis Yap" gibi geri donussuz butonlar asla alt basparmak hizasina konulamaz; yukari kosede veya onay pencerelerinde guvenli tutulur.

## 4. Esmerkezli Kose Geometrisi (Concentric Radius)
Ic ice yuvarlatilmis yuzeylerde optik carpmayi onleyen kural:
```text
Dis Kose Yaricapi = Ic Kose Yaricapi + Aradaki Bosluk (Padding)
```
- Ornek: Dis kartin padding'i 8dp ve ic butonun `borderRadius`'u 8dp ise, dis kartin `borderRadius`'u 16dp olmalidir. Boylece koseler birbirini kusursuz takip eder.

## 5. Sayi Titremesi Korumasi (Tabular Figures)
- Kronometre, sayac, nabiz, fiyatta sayilar surekli degisirken ekranin saga sola titremesini onlemek icin su kural zorunludur:
```dart
style: TextStyle(
  fontFeatures: const [FontFeature.tabularFigures()],
)
```

## 6. Tum Telefon Boyutlarina Gercek Responsive Uyum
Tasarimlarin tum ekranlarda (iPhone SE'den buyuk Pro Max'lere ve tabletlere) sorunsuz calisma kurallari:
- **Sabit Yukseklik Yasagi:** Ekrani kaplayan bilesenlere asla `Container(height: 380)` gibi korlemesine sabit piksel verilmez.
- **Tasma Korumasi:** Form ekranlari ve dikey bilesenler her zaman `SingleChildScrollView` ve `SafeArea` ile sarilir; klavye acildiginda tek bir piksel dahi tasma (`RenderFlex overflow`) yapamaz.
- **Esnek Boyutlandirma:** Esnek alanlarda `LayoutBuilder`, `Flexible` ve `Expanded` dogru hiyerarsiyle kullanilir.
- **Maksimum Genislik Siniri:** Genis ekranlarda veya tabletlerde kartlarin ucube gibi saga sola esnemesini engellemek icin `ConstrainedBox(constraints: BoxConstraints(maxWidth: 600))` kullanilir.

## 7. Tipografi ve 8-Point Izgara (Grid) Sistemi
- Tum bosluklar ve padding degerleri 8'in katlari veya 4'un ara degerleridir (`4, 8, 12, 16, 24, 32 dp`).
- Tum font boyutlari `Theme.of(context).textTheme` uzerinden alinir.
- Tiklanabilir her bilesen parmakla kolay basilabilmesi icin en az `48x48 dp` alana sahip olmalidir.
