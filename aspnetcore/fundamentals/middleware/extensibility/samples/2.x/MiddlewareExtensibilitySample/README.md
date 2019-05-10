# <a name="aspnet-core-middleware-extensibility-sample"></a>ASP.NET Core ara yazılım genişletilebilirlik örneği

Bu örnek, açıklanan senaryoları gösterir [ASP.NET Core Fabrika tabanlı ara yazılım etkinleştirme](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

Örnek uygulama tarafından etkinleştirilen bir ara yazılım gösterir:

* Kuralı. Geleneksel bir ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz. [ara yazılım](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) konu.
* Bir [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması. Varsayılan [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara yazılım sınıfı etkinleştirir.

Ara yazılım uygulamaları aynı şekilde çalışır ve bir sorgu dizesi parametresi tarafından sağlanan değerini kaydedin (`key`). Middlewares, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için bir eklenen veritabanı bağlamı (kapsamlı bir hizmet) kullanın.
