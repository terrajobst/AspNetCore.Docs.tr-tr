# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>ASP.NET Core yanıt örnek önbelleğe alma (ASP.NET Core 2.x)

Bu örnek ASP.NET Core kullanımını göstermektedir [yanıt önbelleğe alma Ara](xref:performance/caching/middleware) ASP.NET Core ile 2.x. ASP.NET Core 1.x örnek için bkz: [ASP.NET Core yanıt önbelleğe alma örnek (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

Uygulamanın gönderdiği bir `Hello World!` ileti ve geçerli saati ile birlikte bir `Cache-Control` önbelleğe alma davranışını yapılandırmak için üstbilgi. Uygulama ayrıca gönderir bir `Vary` yanıt eksikse hizmet önbelleğini yapılandırmak için üstbilgi `Accept-Encoding` sonraki istekleri üstbilgisinin, özgün istekteki veriye eşleşir.

Örnek çalıştırırken, bir yanıt önbellekten olası ve saklı 10 saniye boyunca sunulur.
