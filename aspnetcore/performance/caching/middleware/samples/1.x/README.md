# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>ASP.NET Core yanıt örnek önbelleğe alma (ASP.NET 1.x çekirdek)

Bu örnek ASP.NET Core kullanımını göstermektedir [yanıt önbelleğe alma Ara](xref:performance/caching/middleware) ASP.NET Core ile 1.x. ASP.NET Core 2.x örnek için bkz: [ASP.NET Core yanıt önbelleğe alma örnek (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x).

Uygulamanın gönderdiği bir `Hello World!` ileti ve geçerli saati ile birlikte bir `Cache-Control` önbelleğe alma davranışını yapılandırmak için üstbilgi. Uygulama ayrıca gönderir bir `Vary` yanıt eksikse hizmet önbelleğini yapılandırmak için üstbilgi `Accept-Encoding` sonraki istekleri üstbilgisinin, özgün istekteki veriye eşleşir.

Örnek çalıştırırken, bir yanıt önbellekten olası ve saklı 10 saniye boyunca sunulur.
