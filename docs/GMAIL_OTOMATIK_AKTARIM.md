# İlan Asistanı — Gmail → CRM otomatik aktarım

## Amaç
Gmail'deki sahibinden.com **Favori Arama için Sonuçlar** bildirimlerindeki herkese açık ilan bilgilerini saatlik kontrolle `data/favori-ilanlar.json` dosyasına aktarmak. CRM'nin `ilan-asistani.html` ekranı, açıldığında ve "Şimdi Yenile" düğmesiyle bu dosyayı okur.

## İşleyiş
1. Yetkilendirilmiş ChatGPT otomasyonu Gmail'den `info@sahibinden.com` göndericili favori arama bildirimlerini okur.
2. Yalnızca ilan numarası, başlık, fiyat, `satilik`/`kiralik` ayrımı ve herkese açık `www.sahibinden.com/ilan/` URL'si alınır. İlan kimliğiyle kayıt tekilleştirilir.
3. GitHub'da `main:data/favori-ilanlar.json` dosyası yalnızca yeni veya değişen ilan olduğunda güncellenir.
4. CRM dosyayı kendi origin'inden indirip yeni ilanları yerel kayıtlarına ekler. Daha önceki "gönderildi", "olumlu", "olumsuz", "engelli", "manuel" durumlarını **değiştirmez**.
5. Kullanıcı ilanı açar, açıklamasını kendi kontrol eder, mesajı manuel gönderir; sahibinden.com üzerinde otomatik mesaj gönderme işlemi yapılmaz.

## Güvenlik ve görünürlük
- `crm` deposu **public**. GitHub Pages üzerindeki JSON dosyası herkese açık olabilir; yalnızca sahibinden.com'da zaten herkese açık **ilan metaverisi** saklanır.
- E-posta göndereni/alıcı adresi, Gmail içerikleri, iletişim cevapları, kullanıcı notları, tokenlar ve şifreler **GitHub'a yazılmaz**.
- ChatGPT'de Gmail/GitHub bağlantıları kesilirse veya otomasyon durursa yeni ilanlar gelmez; mevcut ilanlar kaybolmaz.
- İlk kaynak dosyası 29 Ekim 2026'da okunan 11 bildirimden 17 ilan içerir.
- Otomasyonun varlığı, otomasyon ekranından ayrıca doğrulanmalı; programlanmış kontrolün yalnızca kaynak dosyaya yeni commit atması, GitHub Pages'in ne zaman güncellenmesini sağlayacağını etkileyebilir.

## Manuel test
1. CRM'de `ilan-asistani.html` açılır. `İlan kaynağı` bölümündeki durum kontrol edilir.
2. `Şimdi Yenile` seçilir; yeni dosya yüklendiyse yalnızca yeni ilanlar eklenir.
3. Aynı kaynağı tekrar yenilemek tekrar ilan oluşturmaz.
4. Gönderilmiş olarak işaretlenen ilanlar yenileme sonrasında da gönderilmiş kalır.

## Hata durumunda
- Eğer `HTTP 404` görünürse GitHub Pages yayın kaynağının `main` + kök dizin olduğu doğrulanmalı.
- Eğer bir ilan listede yoksa ilgili Gmail bildirimindeki bağlantı formatı kontrol edilmeli; giriş bağlantıları ve özel e-posta bağlantıları dosyaya eklenmemeli.
- Platform izinleri ve sahibinden.com mesajlaşma politikası bu aktarımdan etkilenmez.
