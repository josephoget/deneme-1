# Piyasa Masası

GitHub Pages giriş dosyası `index.html` dosyasıdır. CSS, JS ve veri dosyaları aynı kök dizinde kalmalıdır. Eski `aselsan-teknik-analiz.html` adresi ana sayfaya yönlenir.

## Kapsam

- 2026 üçüncü çeyrek BIST 30 listesindeki 30 pay; Arçelik ve ALTIN.S1 ayrıca izlenir.
- USD/TRY, EUR/TRY, EUR/USD; vadeli altın ons, teorik gram altın, Brent ve WTI grafikleri.
- Günlük fiyat ve hacim, SMA20/50/200, RSI14, MACD12/26/9, Bollinger bantları, ATR14, geçmiş getiri ve destek/direnç eşikleri.
- BIST 30 izleme sırası: eğilim (25), momentum (20), MACD (15), hacim (10), 20/60 gün performansı (15), ATR oynaklığı (15). Puanın toplamı 100'dür. Eksik veya eski veri puan sırasının altına alınır. Puan yatırım önerisi değildir.

BIST 30 bileşenleri [Borsa İstanbul'un 1 Temmuz–30 Eylül 2026 duyurusuna](https://www.borsaistanbul.com/duyuru/15483/bist-pay-endeksleri-donemsel-degisiklikleri) ve ikinci çeyrek listesine göre sabitlendi. **1 Ekim 2026'da üyelik listesini yeni resmi duyuruyla güncelleyin.** Fiyat güncellemesi üyelik listesini otomatik değiştirmez.

## GitHub Pages ve veri yenileme

1. Depoyu GitHub'a `main` dalında gönderin. **Settings → Pages → Build and deployment → Source → GitHub Actions** ayarını seçin. İlk push statik siteyi yayınlar.
2. **Actions → Refresh market data and deploy → Run workflow** ile ilk veri yenilemesini başlatın. Gerekirse **Settings → Actions → General → Workflow permissions** bölümünde **Read and write permissions** seçin.
3. Workflow iş günleri 06:00–16:30 UTC aralığında 30 dakikada bir Yahoo Finance'ın gecikmeli günlük veri serilerini çeker. Fiyat değişmişse `market-live.json` ve `market-live.js` dosyalarını commit eder; aynı çalışmada Pages'i yeniden yayınlar. GitHub'ın zamanlanmış işleri gecikebilir veya devre dışı kalabilir.
4. Site her açılışta ve sekmeye geri dönüldüğünde `market-live.json` dosyasını önbelleği atlayarak yeniden okur. Ekran veri tarihini gösterir. Bu mekanizma **anlık borsa verisi değildir**; yeni veri ancak workflow başarılı olunca görünür. İşlem öncesi aracı kurum fiyatını kullanın.

`python3 scripts/refresh_market.py` komutu aynı veri dosyalarını yerel olarak da yeniler. Python standart kütüphanesi yeterlidir. Kaynak başarısız olursa son başarılı seri korunur ve `market-live.json` içindeki `errors` alanına hata yazılır. Sayfa eski veriyi işaretler.

ALTIN.S1 Yahoo Finance'da bulunmadığı için 29 Eylül 2026 tarihli tarihsel kesit korunur. Otomatik yenileme bu üründe çalışmaz; detay ekranı veri tarihini ve kaynak başarısızlığını gösterir. Teorik gram altın grafiği **vadeli ons altın × USD/TRY ÷ 31,1034768** formülüdür; kuyumcu veya spot gram fiyatı değildir. Brent ve WTI grafikleri de vadeli sözleşme referans fiyatlarıdır. Senaryo hesaplayıcı gerçek vadeli sözleşme büyüklüklerini içermez.
