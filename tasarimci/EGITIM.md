# Tasarimci - Egitim ve UI/UX Standartlari

## Flutter Arayuz Kurallari
1. Asla buyuk monolitik `build` metodlari yazma; en fazla 50 satirda bilesenlere bol.
2. Responsive tasarim icin `MediaQuery`, `LayoutBuilder` veya flex bilesenlerini olculu ve dogru kullan.
3. Tema destegi: Hem Light hem Dark tema renk token'larini `Theme.of(context)` uzerinden dinamik al.
4. Hit Area: Tiklanabilir tum alanlar en az 48x48 dp dokunma alanina sahip olmalidir.
5. Tipografi ve hiyerarsi: Basliklar, govde metinleri ve etiketler arasinda net kontrast sagla.
