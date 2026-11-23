---
name: ble-donanim-iletisimi
description: Bluetooth Low Energy (BLE) ile akilli saat, medikal cihaz ve sensor haberlesmesi rehberi. Tarama, baglanma, MTU muzakeresi, karakteristik yonetimi, otomatik yeniden baglanma ve bayt ayristirma.
---

# BLE Donanim Iletisimi Becerisi

Bu beceri; akilli saat, medikal cihazlar, nabiz/EKG sensorleri ve ozel BLE donanimlariyla kararli, kesintisiz ve yuksek performansli iletisim kurmak icin kullanilir.

## 1. BLE Baglanti Yasam Dongusu (Connection Lifecycle)
1. **Izin Kontrolleri:** Bluetooth Scan, Connect ve Location izinlerinin calisma zamaninda (runtime permission) dogrulanmasi.
2. **Cihaz Tarama (Scanning):** Surekli tarama pili tuketir; maksimum 10-15 saniyelik zaman asimi (`timeout`) ile filtrelenmis (Service UUID ile) tarama yapilir.
3. **Baglanti & MTU Muzakeresi:** Baglanti kurulduktan hemen sonra yuksek paket transferi icin Android'de MTU boyutu genisletilir (`requestMtu(512)`).
4. **Servis ve Karakteristik Kesfi:** `discoverServices()` calistirilarak okuma (`read`), yazma (`write`) ve bildirim (`notify`/`indicate`) kanallari bulunur.
5. **Bildirim Dinleme (Notify/Stream):** Karakteristik degisimleri `StreamSubscription` ile dinlenir.

## 2. Baglanti Kopmasi ve Otomatik Yeniden Baglanma (Auto-Reconnect)
- Donanim kapsama alani disina ciktiginda veya sinyal zayifladiginda:
- **Ustel Geri Cekilme (Exponential Backoff):** Aninda 100 kez baglanmaya calismak yerine (1s -> 2s -> 4s -> 8s -> 16s) araliklarla yeniden baglanma denenir.
- Baglanti koptugunda UI State aninda `DeviceDisconnected` durumuna gecirilir, kullaniciya net bilgi verilir.

## 3. Bayt Seviyesinde Veri Paketleme ve Ayristirma (Parsing)
- Donanimdan gelen ham bayt dizisi (`List<int>`) dogrudan parse edilmez; su guvenlik kontrollerinden gecer:
  1. **Paket Basligi (Header Check):** Paketin baslangic baytlari (ornegin `0xAA, 0x55`) dogrulanir.
  2. **Paket Boyutu (Length Check):** Veri uzunlugu beklenen boyuttan kisa ise paket islenmeden elenir.
  3. **Checksum / CRC Dogrulama:** Ham baytlarin toplami veya CRC kontrolu tutmuyorsa paket cope atilir, uygulama cokertilmez.
  4. **Modelleme:** Dogrulanan baytlar tip guvenli bir modele (ornegin `HeartRateMeasurementModel`) donusturulur.
