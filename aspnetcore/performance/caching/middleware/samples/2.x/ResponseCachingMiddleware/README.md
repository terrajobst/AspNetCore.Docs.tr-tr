# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core yanıt önbelleğe alma örneği

Bu örnek ASP.NET Core kullanımını [yanıt önbelleğe alma ara yazılımı](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Uygulama, dizin sayfasıyla yanıt dahil olmak üzere bir `Cache-Control` önbelleğe alma davranışını yapılandırmak için üst bilgi. Uygulama ayrıca ayarlar `Vary` yanıt yalnızca şu durumlarda hizmet önbelleğini yapılandırmak için üst bilgi `Accept-Encoding` sonraki istekleri üstbilgisinin özgün istekteki eşleşen.

Örnek çalışırken, dizin sayfası depolandığında ve 10 saniye boyunca önbelleğe önbelleğinden sunulur.
