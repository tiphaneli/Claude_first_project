# Nesne Tarayıcı

Telefonun kamerasını açar, gördüğü nesneleri tanır ve her birini bir **veri tablosuna** satır olarak yazar.
Tabloyu Excel dosyası olarak dışa aktarabilirsin.

Uygulama tek bir dosyadan ibarettir (`index.html`) ve her şeyi telefonun içinde yapar —
görüntü hiçbir sunucuya gönderilmez.

---

## Telefonda nasıl açılır?

1. GitHub'da bu deponun sayfasını aç.
2. Üstteki **Settings** (Ayarlar) sekmesine gir.
3. Sol menüden **Pages**'e tıkla.
4. **Source** kısmında **Deploy from a branch** seçili olsun.
5. Branch olarak `claude/codex-vs-claude-comparison-d4wve3`, klasör olarak `/ (root)` seç ve **Save**'e bas.
6. Bir iki dakika bekle, sayfayı yenile. Yukarıda bir adres çıkacak:
   `https://tiphaneli.github.io/Claude_first_project/`
7. Bu adresi iPhone'da **Safari** ile aç.

Ana ekrana kısayol koymak için: Safari'de **Paylaş** düğmesi → **Ana Ekrana Ekle**.
Artık normal bir uygulama gibi ikonuyla açılır.

> Kameranın çalışması için adresin **https://** ile başlaması şart. GitHub Pages bunu kendiliğinden sağlar.
> Dosyayı bilgisayarda çift tıklayarak açarsan (`file://`) kamera açılmaz — bu bir hata değil, tarayıcının güvenlik kuralı.

---

## Ne kaydediyor?

Her tanınan nesne için bir satır:

| Sütun | Anlamı |
|---|---|
| Zaman | Kaydın alındığı tarih ve saat |
| Nesne / Nesne (EN) | Türkçe ve İngilizce adı |
| Tür | insan, hayvan, bitki, yiyecek, eşya |
| Güven % | Modelin ne kadar emin olduğu |
| Detay tahmin | İkinci modelin daha ince tahmini (ör. cins) — İngilizce |
| Merkez X / Y (oran) | Nesnenin karedeki yeri, 0–1 arası |
| Genişlik / Yükseklik (oran) | Nesnenin karedeki boyutu, 0–1 arası |
| X / Y / Genişlik / Yükseklik (piksel) | Aynı bilgiler ham piksel olarak |
| Alan % | Karenin yüzde kaçını kapladığı |
| En/Boy oranı | Yatık mı dik mi |
| Renk / Renk kodu | Baskın rengin adı ve HEX kodu |
| Parlaklık 0–100 | Bölgenin aydınlığı |
| Karedeki nesne sayısı | O kayıtta kaç nesne vardı |
| Kare genişlik / yükseklik / Yön | Kameranın çözünürlüğü ve yönü |

Ölçüler **santimetre değildir**; ekrana göre orandır. Tek bir kameradan gerçek boyut çıkarılamaz —
bunun için derinlik sensörü (LiDAR) ya da bilinen boyutta bir referans nesne gerekir.

---

## Ne tanır?

- **Yer bulma (kutu çizme):** 80 çeşit — insan, kedi, köpek, kuş, at, inek, koyun, fil, ayı, zebra,
  zürafa, araba, bisiklet, otobüs, sandalye, masa, yatak, televizyon, telefon, kitap, şişe, bardak,
  muz, elma, pizza ve benzerleri.
- **Detay tahmini:** 1000 çeşit — köpek/kedi cinsleri, kuş türleri, birçok bitki ve eşya.

Bu listelerin dışındaki bir şeye çevirirsen tabloya bir şey yazılmaz. Model, bilmediği bir şeye
tahmin yürütmek yerine susar.

---

## Teknik notlar

- `index.html` — uygulamanın tamamı (arayüz + mantık, tek dosya)
- `manifest.webmanifest`, `icon-*.png`, `apple-touch-icon.png` — ana ekran kısayolu için
- Nesne bulma: TensorFlow.js + COCO-SSD (`lite_mobilenet_v2`)
- Detay sınıflandırma: MobileNet v2 (ilk kayıtta tembel yüklenir)
- Modeller ilk açılışta internetten iner (~10 MB), sonra tarayıcı önbelleğinde kalır
- Tablo `localStorage`'da saklanır, son 5000 satır tutulur
- Dışa aktarma: noktalı virgülle ayrılmış CSV + UTF-8 BOM (Türkçe Excel'de doğru açılsın diye)
