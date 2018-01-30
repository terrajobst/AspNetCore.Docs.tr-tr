# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core yanıt önbelleğe alma örneği

Bu örnek ASP.NET Core kullanımını göstermektedir [yanıt önbelleğe alma Ara](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Uygulama, dizin sayfasıyla yanıt dahil olmak üzere bir `Cache-Control` önbelleğe alma davranışını yapılandırmak için üstbilgi. Uygulama ayrıca ayarlar `Vary` yanıt eksikse hizmet önbelleğini yapılandırmak için üstbilgi `Accept-Encoding` sonraki istekleri üstbilgisinin eşleşir, özgün istekteki veriye.

Örnek çalıştırırken, dizin sayfası depolandığında ve 10 saniye için önbelleğe önbelleğinden sunulur.
