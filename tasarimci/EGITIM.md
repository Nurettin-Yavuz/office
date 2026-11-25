# Tasarimci - Egitim ve Craft Standartlari

## 1. Yapay Zeka Klişelerini Yok Eden Tasarım Manifestosu

Bir Flutter ekranına bakıldığında "bunu yapay zeka yazmış" dedirten 4 temel günah ve bunların zanaatkar çözümleri:

| Yapay Zeka Klişesi (Yasak) | Zanaatkar / İnsan Eli Çözümü (Zorunlu) |
| :--- | :--- |
| Tailwind İndigo (`0xFF4F46E5`) ve jenerik morlar | Obsidyen Siyahı (`#0A0A0C`), Titanyum Grisi ve projeye özel tek imza renk (Hermès Turuncusu, Zümrüt, Amber). |
| Her satırın solunda renkli daire içinde bir ikon | Tipografik hiyerarşi, mikro durum noktaları (2-4 dp dot), sade ve hafif ikonlar veya saf editoryal metin. |
| Her yeri kaplayan yuvarlak kutular ve çamurlu gölgeler | Kutusuz ferah satırlar, 0.5 dp saç teli inceliğinde kenarlıklar (`Border.all(color: Colors.white.withOpacity(0.08))`). |
| Kart içine ikinci bir kart gömmek (`Nested Cards`) | Tek yüzey üzerinde ince ayraç (hairline divider) veya mikro boşluk hiyerarşisi. |

---

## 2. Renk ve Yüzey Mimarisi

### Koyu Mod (Obsidian & Titanium)
- **Ana Zemin:** `Color(0xFF0A0A0C)` (Derin mat obsidyen, asla jenerik lacivert-gri değil).
- **Yüzey / Kart Katmanı:** `Color(0xFF141418)` veya `Color(0xFF1A1A20)`.
- **Saç Teli Çerçeveler:** `Border.all(color: Colors.white.withOpacity(0.07), width: 0.7)`.
- **Vurgu Rengi:** Tek ve net (Örn: `Color(0xFFF97316)` Hermès Turuncusu veya `Color(0xFF10B981)` Zümrüt).

### Açık Mod (Swiss Minimal / Paper & Bone)
- **Ana Zemin:** `Color(0xFFF7F7F9)` (Sıcak taş/kemik tonu).
- **Yüzey / Kart Katmanı:** `Color(0xFFFFFFFF)`.
- **Saç Teli Çerçeveler:** `Border.all(color: Colors.black.withOpacity(0.06), width: 0.7)`.

---

## 3. Tipografik Ustalık ve Sayısal Hiyerarşi

### Başlıklar ve Etiketler
* **Büyük Başlıklar:** `letterSpacing: -0.7`, `fontWeight: FontWeight.w700`.
* **Kategori / Üst Başlık (Eyebrow):** `fontSize: 11`, `letterSpacing: 1.2`, `fontWeight: FontWeight.w600`, tamamı BÜYÜK HARF ve ikincil soluk renk.
* **Gövde Metinleri:** Yüksek okunurluk, `letterSpacing: -0.2`.

### Finansal ve Sayısal Gösterimler (Craft Typography)
Para tutarlarında asla dümdüz `Text('₺12.450,50')` yazılmaz. Parçalı ve usta işi format uygulanır:
```dart
RichText(
  text: TextSpan(
    style: TextStyle(
      fontFeatures: const [FontFeature.tabularFigures()],
      color: textColor,
    ),
    children: [
      TextSpan(
        text: '₺',
        style: TextStyle(fontSize: 16, fontWeight: FontWeight.w400, color: textColor.withOpacity(0.6)),
      ),
      const TextSpan(
        text: '12.450',
        style: TextStyle(fontSize: 28, fontWeight: FontWeight.w700, letterSpacing: -0.8),
      ),
      TextSpan(
        text: ',50',
        style: TextStyle(fontSize: 16, fontWeight: FontWeight.w500, color: textColor.withOpacity(0.6)),
      ),
    ],
  ),
)
```

---

## 4. Dokunsal Detaylar ve Mikro Öğeler
- **Durum Göstergeleri:** Büyük etiket kutuları yerine mini noktalar:
  ```dart
  Container(
    width: 6,
    height: 6,
    decoration: BoxDecoration(shape: BoxShape.circle, color: accentColor),
  )
  ```
- **Seçiciler (Segment Controls):** Pofuduk butonlar yerine Apple tarzı gömülü, kayar zeminli minimalist seçiciler.
- **Liste Düzeni:** Kutulu haplar yerine Things 3 tarzı temiz, açık ve editoryal satırlar.

---

## 5. Responsive ve Taşma Koruması
- Sabit yükseklik (`height: 350`) kesinlikle yasaktır.
- Klavye ve küçük ekranlar için her dikey akış `SingleChildScrollView` ve `SafeArea` ile korunur.
- Tablet ve geniş ekranlarda `ConstrainedBox(constraints: BoxConstraints(maxWidth: 600))` ile optimum okuma genişliği korunur.
