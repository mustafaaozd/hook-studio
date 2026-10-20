# Hook Studio

Markalara özel kanca ve reklam koleksiyonları. React arayüzü GitHub Pages üzerinde, özel dosyalar ve API Supabase üzerinde çalışır.

## Kullanım

Yönetici mevcut uygulama hesabıyla giriş yapar; marka adı/logo, içerik yükleme, kanca kesme, performans seçimi, sıralama ve silme işlemlerini yönetir. Müşterilere marka panelindeki bağlantı gönderilir. Müşteriler giriş yapmadan yalnızca ilgili koleksiyonu görebilir.

Yerel HTML önizlemesindeki IndexedDB kayıtları otomatik taşınmaz. Bu kayıtlar kullanıldıkları tarayıcıda kalır; canlı sisteme henüz aktarılmadılar.

## Derleme ve yayın

Güncel kaynaklar `Hook-Studio-GitHub-Kaynak.zip` içindedir. Arşivi bir klasöre çıkarın. Eski `Hook-Studio-v3-Kaynak.zip` yerel prototip arşividir. Node.js 22 veya üstü: `npm ci`, ardından `npm run build`. Üretilen `index.html` depoda tutulur; GitHub Pages `main` dalının kökünden yayınlanır. `src/` değişikliklerinden sonra derlemeyi yenileyin.

## Sunucu

`backend/index.ts`, `hook-studio-api` adlı Supabase Edge Function kaynağıdır. JWT gateway kontrolü kapalıdır çünkü fonksiyon hem Supabase Auth kullanıcılarını doğrular hem de markaya özel bağlantı anahtarlarını kontrol eder. Yönetici yetkisi sunucu tarafındaki `hook_studio_admins` tablosuyla belirlenir. İstemciden gelen kimlik e-posta başlıklarına güvenilmez.

`backend/schema.sql` yalnızca yeni kurulum içindir; mevcut kurulumda tekrar çalıştırmayın. `hook_studio_*` tabloları RLS ile ve tarayıcı rollerinin tüm doğrudan hakları kaldırılarak korunur. Servis anahtarı yalnızca Edge Function ortamındadır. Tarayıcıdaki publishable key gizli değildir ve yönetici yetkisi sağlamaz.

`hook-studio-media` özel depodur. Görüntüleme için verilen imzalı dosya adresleri 10 dakika geçerlidir; sayfa bunları 5 dakikada bir yeniler. Marka bağlantısı yenilendiğinde eski bağlantı yeni içerik alamaz; önceden verilmiş dosya adresleri süreleri dolana kadar çalışabilir.

Mevcut Meta uygulamasının tabloları, politikaları ve giriş ayarları değiştirilmemiştir. Projenin depolama/işlem/trafik kotası ortaktır. Çıkış işlemi yalnızca Hook Studio oturumunu kapatır.

## Doğrulama

TypeScript ve üretim derlemesi kontrol edilir. Canlı API'de oturumsuz erişim, sahte kimlik başlıkları, geçersiz bağlantı, müşteri yazma ve bağlantı yönetme isteklerinin reddedildiği test edildi. Geçerli müşteri bağlantısının yalnızca ilgili markayı döndürdüğü kontrol edildi. Gerçek yönetici oturumu ile video yükleme/kesme akışı ayrıca tarayıcıda doğrulanmalıdır.
