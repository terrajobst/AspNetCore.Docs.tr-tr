---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları geliştirmek için temel oluşturabilen altyapı kavramları keşfedin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 30c456685ce26522faff9b58fbd2977ad2f2869a
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202633"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

ASP.NET Core uygulamasını bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Main` yöntemi:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Yöntemini çağıran `WebHost.CreateDefaultBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu deseni izler. Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır. IIS üzerinde çalıştırmak ASP.NET Core'nın web ana bilgisayarı varsa çalışır. Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha sonraki bölümde.

`IWebHostBuilder`, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar. Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve `UseContentRoot` kök içerik dizini belirtmek için. `Build` Ve `Run` yöntemleri yapı `IWebHost` nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Yöntemi kullanan `WebHostBuilder`, bir web uygulaması konağı oluşturmak için oluşturucu deseni izler. Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, `UseKestrel`) ve başlangıç sınıfı (`UseStartup`). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır. Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha sonraki bölümde.

`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar `UseIISIntegration` IIS ve IIS Express barındırmak ve `UseContentRoot` kök içerik dizini belirtmek için. `Build` Ve `Run` yöntemleri yapı `IWebHost` nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.

---

## <a name="startup"></a>Başlangıç

`UseStartup` Metodunda `WebHostBuilder` belirtir `Startup` uygulamanız için sınıf:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` Sınıfıdır istek işleme ardışık düzen tanımladığınız ve uygulama tarafından gereken diğer hizmetler yapılandırıldığı. `Startup` Sınıfı genel olmalıdır ve aşağıdaki yöntemleri içerir:

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

`ConfigureServices` tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Core kimlik) tarafından kullanılır. `Configure` tanımlar [ara yazılım](xref:fundamentals/middleware/index) istek ardışık düzenini için.

Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).

## <a name="content-root"></a>İçerik kök

Temel yol görünümleri gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfaları](xref:razor-pages/index)ve statik varlıklar. Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynıdır.

## <a name="web-root"></a>Web kökü

Bir uygulamanın web kök CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (Hizmetler)

Bir hizmeti bir uygulamada ortak tüketim için hazırlanmış bir bileşenidir. Hizmetleri aracılığıyla yapılan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). ASP.NET Core içeren yerel **miyim**nİşlevin **o**f **C**destekleyen Tim (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak. İsterseniz, varsayılan yerel kapsayıcı değiştirebilirsiniz. Kendi gevşek bağ modelini avantajı yanı sıra DI Hizmetleri uygulamanız genelinde kullanılabilir yapar (örneğin, [günlüğü](xref:fundamentals/logging/index)).

Daha fazla bilgi için [bağımlılık ekleme](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Ara yazılım

ASP.NET Core, istek kullanarak işlem hattı oluşturma [ara yazılım](xref:fundamentals/middleware/index). ASP.NET Core ara yazılım üzerinde zaman uyumsuz mantık gerçekleştiren bir `HttpContext` ve dizideki sonraki ara yazılımı çağırır veya istek doğrudan sonlandırır. "XYZ" adlı bir ara yazılım bileşeni çağırarak eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.

ASP.NET Core, zengin bir yerleşik ara yazılım içerir:

* [Statik dosyalar](xref:fundamentals/static-files)
* [Yönlendirme](xref:fundamentals/routing)
* [Kimlik Doğrulaması](xref:security/authentication/index)
* [Yanıt Sıkıştırma Ara Yazılımı](xref:performance/response-compression)
* [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)-tabanlı ara yazılım, ASP.NET Core uygulamaları için kullanılabilir ve kendi özel bir ara yazılım yazabilirsiniz.

Daha fazla bilgi için [ara yazılım](xref:fundamentals/middleware/index) ve [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>HTTP isteklerini başlatma

Kullanma hakkında bilgi için `IHttpClientFactory` erişimi `HttpClient` HTTP isteğinde bulunmak için örnekleri görmek [başlatmak HTTP istekleri](xref:fundamentals/http-requests).

::: moniker-end

## <a name="environments"></a>Ortamlar

Ortamlar, "Geliştirme" ve "Üretim" gibi ASP.NET core'da birinci sınıf bir kavram ve ortam değişkenlerini kullanarak ayarlayabilirsiniz.

Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).

## <a name="configuration"></a>Yapılandırma

ASP.NET Core ad-değer çiftlerine göre bir yapılandırma modeli kullanır. Yapılandırma modeli temel almayan `System.Configuration` veya *web.config*. Yapılandırma ayarları sıralı bir dizi yapılandırma sağlayıcıları alır. Yerleşik yapılandırma sağlayıcıları, çeşitli dosya biçimleri (XML, JSON, INI) ve ortama dayalı yapılandırma etkinleştirmek için ortam değişkenlerini desteklemez. Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.

Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET Core çeşitli günlük sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler. Yerleşik sağlayıcılar, bir veya daha fazla hedefe gönderen günlükleri destekler. Üçüncü taraf günlük altyapılarına kullanılabilir.

Daha fazla bilgi için [günlüğe kaydetme](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Hata işleme

ASP.NET Core uygulamalarında Geliştirici özel durum sayfasında, özel hata sayfaları, statik durumu kod sayfaları ve başlangıç özel durum işleme dahil olmak üzere, hataları işleme için yerleşik özelliklere sahiptir.

Daha fazla bilgi için [hatalarının nasıl işleneceğini](xref:fundamentals/error-handling).

## <a name="routing"></a>Yönlendirme

ASP.NET Core, yol işleyicisi uygulama isteklerinin yönlendirme özellikleri sunar.

Daha fazla bilgi için [yönlendirme](xref:fundamentals/routing).

## <a name="file-providers"></a>Dosya sağlayıcıları

ASP.NET Core, dosya sistemi erişimini kullanarak platformlar arasında dosyaları ile çalışma için ortak bir arabirim sunan dosya sağlayıcıları soyutlar.

Daha fazla bilgi için [dosya sağlayıcıları](xref:fundamentals/file-providers).

## <a name="static-files"></a>Statik dosyalar

Statik dosya ara yazılımı statik dosyaları, HTML, CSS, görüntü ve JavaScript gibi işlev görür.

Daha fazla bilgi için [statik dosyalar](xref:fundamentals/static-files).

## <a name="hosting"></a>Barındırma

ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*, uygulama başlatma ve ömür yönetimi için sorumlu olduğu.

Daha fazla bilgi için [ASP.NET Core ana](xref:fundamentals/host/index).

## <a name="session-and-app-state"></a>Oturum ve uygulama durumu

ASP.NET Core, kullanıcının bir web uygulaması gözatar sırada oturum ve uygulama durumu korumak için çeşitli yaklaşımlar sunar.

Daha fazla bilgi için [oturum ve uygulama durumu](xref:fundamentals/app-state).

## <a name="servers"></a>Sunucular

Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez. Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır. İletilen istek, arabirimler aracılığıyla erişilebilen bir özellik nesne olarak paketlenir. ASP.NET Core içeren olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel). Kestrel'i genellikle çalıştığı bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [Ngınx](http://nginx.org). Kestrel'i bir uç sunucusu çalıştırılabilir.

Daha fazla bilgi için [sunucuları](xref:fundamentals/servers/index) ve aşağıdaki konular:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Genelleştirme ve yerelleştirme

ASP.NET Core ile çok dilli bir Web sitesi oluşturma sitenizin daha geniş kitlelere ulaşmasını sağlar. ASP.NET Core, farklı dillere ve kültürlere yerelleştirme için Hizmetler ve ara yazılım sağlar.

Daha fazla bilgi için [Genelleştirme ve Yerelleştirme](xref:fundamentals/localization).

## <a name="request-features"></a>İstek özellikleri

Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır. Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.

Daha fazla bilgi için [istek özellikleri](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Arka plan görevleri

Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*. Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.

Daha fazla bilgi için [görevleri barındırılan hizmetler ile arka plan](xref:fundamentals/host/hosted-services).

## <a name="access-httpcontext"></a>Erişim HttpContext

Erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.

## <a name="open-web-interface-for-net-owin"></a>.NET (OWIN) için açık Web arabirimi

ASP.NET Core, .NET (OWIN) için açık Web arabirimi destekler. OWIN web uygulamalarının web sunucularından ölçeklendirilebilmeleri sağlar.

Daha fazla bilgi için [açık Web arabirimi için .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür. Sohbet, yürütebilmektedir oyunlar gibi uygulamalar için kullanılır ve gerçek zamanlı bir web uygulaması işlevindeki istediğiniz herhangi bir yerde. ASP.NET Core web yuvası özellikleri destekler.

Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App metapackage

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage paket Yönetimi basitleştirir. Daha fazla bilgi için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:

* Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Core tarafından desteklenmiyor.
* ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.

Daha fazla bilgi için [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET core ve .NET Framework çalışma zamanı

ASP.NET Core uygulaması, .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.

Daha fazla bilgi için [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>ASP.NET Core ile ASP.NET arasında seçim yapma

ASP.NET Core ile ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: [ASP.NET Core ve ASP.NET arasında seçim yapma](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
