---
title: ASP.NET Core temelleri
author: rick-anderson
description: "ASP.NET Core uygulamaları oluşturmak için temel kavramları bulur."
keywords: "ASP.NET Core, temelleri, genel bakış"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

Bir web sunucusu oluşturan bir konsol uygulaması bir ASP.NET Core uygulamadır kendi `Main` yöntemi:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Yöntemini çağırır `WebHost.CreateDefaultBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler. Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır. ASP.NET Core'nın web ana bilgisayarı, IIS'de çalışan varsa çalışır. Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun uzantı metodu çağırma tarafından kullanılabilir. `UseStartup`açıklanan daha sonraki bölümde.

`IWebHostBuilder`, dönüş türü `WebHost.CreateDefaultBuilder` çağırma, birçok isteğe bağlı yöntemler sağlar. Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırma ve `UseContentRoot` kök içerik dizinini belirtmek için. `Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Yöntemi kullanan `WebHostBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu düzenini izler. Oluşturucu web sunucusu tanımlayan yöntemleri vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır. Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun uzantı metodu çağırma tarafından kullanılabilir. `UseStartup`açıklanan daha sonraki bölümde.

`WebHostBuilder`dahil olmak üzere birçok isteğe bağlı yöntemler sağlar `UseIISIntegration` IIS ve IIS Express barındırmak için ve `UseContentRoot` kök içerik dizinini belirtmek için. `Build` Ve `Run` yöntemleri yapı `IWebHost` uygulamayı barındıran ve HTTP isteklerini dinlemeye başlar nesnesi.

---

## <a name="startup"></a>Başlangıç

`UseStartup` Yöntemi `WebHostBuilder` belirtir `Startup` sınıfı, uygulamanız için:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` Sınıfı olduğundan istek işleme ardışık düzen tanımladığınız ve burada uygulama tarafından gerekli tüm hizmetler yapılandırılmadı. `Startup` Sınıfı genel olmalı ve aşağıdaki yöntemlerden içermelidir:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices`tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Çekirdek, kimlik) tarafından kullanılır. `Configure`tanımlar [ara yazılım](xref:fundamentals/middleware) isteği ardışık düzeni için.

Daha fazla bilgi için bkz: [uygulama başlangıç](xref:fundamentals/startup).

## <a name="content-root"></a>İçerik kök

Taban yol görünümler gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfalarının](xref:mvc/razor-pages/index)ve statik varlıklar. Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynıdır.

## <a name="web-root"></a>Web kök

Web uygulama CSS, JavaScript ve görüntü dosyaları gibi ortak, statik kaynakları içeren proje dizininde köküdür.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (Hizmetleri)

Bir hizmeti bir uygulama içinde ortak tüketim yönelik bir bileşendir. Hizmetleri kullanılabilir hale getirilir aracılığıyla [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). ASP.NET Core içeren yerel **ı**nİşlevin **o**f **C**destekleyen Tim (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak. İsterseniz, varsayılan yerel kapsayıcı değiştirebilirsiniz. Kendi gevşek bağlantı avantajı yanı sıra dı Hizmetleri uygulamanız genelinde kullanılabilir hale getirir (örneğin, [günlüğü](xref:fundamentals/logging/index)).

Daha fazla bilgi için bkz: [bağımlılık ekleme](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Ara yazılım

ASP.NET çekirdek isteği kullanılarak ardışık düzeni oluşturma [Ara](xref:fundamentals/middleware). ASP.NET Core Ara gerçekleştirir zaman uyumsuz mantığı bir `HttpContext` ve sırayla sonraki ara yazılımı çağırır veya istek doğrudan sonlandırır. "XYZ" adlı bir ara yazılım bileşeni çağırarak eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.

ASP.NET Core zengin bir yerleşik ara yazılım kümesi ile birlikte gelir:

* [Statik dosyalar](xref:fundamentals/static-files)
* [Yönlendirme](xref:fundamentals/routing)
* [Kimlik doğrulaması](xref:security/authentication/index)
* [Yanıt sıkıştırma Ara](xref:performance/response-compression)
* [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)-tabanlı ara yazılım ASP.NET Core uygulamaları için kullanılabilir ve kendi özel ara yazabilirsiniz.

Daha fazla bilgi için bkz: [ara yazılımı](xref:fundamentals/middleware) ve [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).

## <a name="environments"></a>Ortamlar

"Geliştirme" ve "Üretim" gibi ortamları ASP.NET Core, birinci sınıf bir kavram ve ortam değişkenlerini kullanma ayarlayabilirsiniz.

Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).

## <a name="configuration"></a>Yapılandırma

ASP.NET Core üzerinde ad-değer çiftleri temel alan bir yapılandırma modeli kullanır. Yapılandırma modeli bağlı değil `System.Configuration` veya *web.config*. Yapılandırma ayarları yapılandırma sağlayıcılarının bir sıralanmış kümesinden alır. Yerleşik yapılandırma sağlayıcıları çeşitli dosya biçimleri (XML, JSON, INI) ve ortam tabanlı yapılandırmasını etkinleştirmek için ortam değişkenlerini desteklemez. Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.

Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET çekirdeği günlüğü sağlayıcıları çeşitli çalışır bir günlük API destekler. Yerleşik sağlayıcılar, bir veya daha fazla hedeflere gönderen günlükleri destekler. Üçüncü taraf günlük altyapıları kullanılabilir.

[Günlüğe kaydetme](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Hata işleme

ASP.NET Core Geliştirici özel durum sayfasında, özel hata sayfaları, statik durum kod sayfaları ve başlangıç özel durum işleme gibi uygulamalar, hataları işlemek için yerleşik özellikler vardır.

Daha fazla bilgi için bkz: [işleme hatası](xref:fundamentals/error-handling).

## <a name="routing"></a>Yönlendirme

ASP.NET Core rota işleyicileri için uygulama isteği yönlendirme için özellikleri sunar.

Daha fazla bilgi için bkz: [yönlendirme](xref:fundamentals/routing).

## <a name="file-providers"></a>Dosya sağlayıcıları

ASP.NET Core dosya sistemi erişimi platformlarında dosyalarıyla çalışmak için ortak bir arabirim sunar dosya sağlayıcıları kullanımıyla soyutlar.

Daha fazla bilgi için bkz: [dosya sağlayıcıları](xref:fundamentals/file-providers).

## <a name="static-files"></a>Statik dosyalar

Statik dosya ara yazılımı HTML, CSS, görüntü ve JavaScript gibi statik dosyaları sunar.

Daha fazla bilgi için bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files).

## <a name="hosting"></a>Barındırma

ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*, uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu.

Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Oturum ve uygulama durumu

Oturum durumunu kaydetmek ve kullanıcı web uygulamanızı gözatar sırasında kullanıcı verilerini depolamak için kullanabileceğiniz ASP.NET Core bir özelliğidir.

Daha fazla bilgi için bkz: [oturum ve uygulama durumu](xref:fundamentals/app-state).

## <a name="servers"></a>Sunucular

Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez. Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır. İletilen istek arabirimleri aracılığıyla erişilen özellik nesneleri kümesi olarak paketlenir. ASP.NET Core içerir olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel). Kestrel sık çalıştırıldığında bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [nginx](http://nginx.org). Kestrel bir uç sunucusu çalıştırabilirsiniz.

Daha fazla bilgi için bkz: [sunucuları](xref:fundamentals/servers/index) ve aşağıdaki konular:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [ASP.NET çekirdeği Modülü](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Genelleştirme ve yerelleştirme

ASP.NET Core ile çok dilli bir Web sitesi oluşturma, daha geniş bir kitleye ulaşmak sitenizin sağlar. ASP.NET Core farklı dillere ve kültürlere yerelleştirme için hizmetleri ve ara yazılım sağlar.

Daha fazla bilgi için bkz: [Genelleştirme ve Yerelleştirme](xref:fundamentals/localization).

## <a name="request-features"></a>İstek özellikleri

Web sunucusu uygulama ayrıntılarını ilgili HTTP istekleri ve yanıtları arabirimlerde tanımlanır. Bu arabirimleri oluşturmak ve uygulamanın barındırma ardışık düzen değiştirmek için sunucu uygulamaları ve ara yazılım tarafından kullanılır.

Daha fazla bilgi için bkz: [istek özellikleri](xref:fundamentals/request-features).

## <a name="open-web-interface-for-net-owin"></a>.NET (OWIN) için açık Web arabirimi

ASP.NET Core açık Web arabirimi .NET (OWIN) destekler. OWIN web uygulamalarının web sunucularından ayrılmış sağlar.

Daha fazla bilgi için bkz: [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) kalıcı iki yönlü iletişim kanalları üzerinden TCP bağlantıları sağlayan bir protokoldür. Bir web uygulamasında gerçek zamanlı işlevselliği işlemleriniz herhangi bir yere ve sohbet, yürütebilmektedir gibi uygulamalar için kullanılır. ASP.NET Core web yuva özelliklerini destekler.

Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:

* Tüm paketler ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Çekirdek tarafından desteklenir. 
* ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları.

Daha fazla bilgi için bkz: [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>.NET core ve .NET Framework çalışma zamanı

Bir ASP.NET Core uygulama .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.

Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>ASP.NET Core ve ASP.NET arasında seçim yapma

ASP.NET Core ve ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: [ASP.NET Core ve ASP.NET arasında seçim yapma](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
