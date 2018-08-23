---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41756287"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

ASP.NET Core uygulaması bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Main` yöntemi:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

`Main` Yöntemini çağıran [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web ana bilgisayarı oluşturma. Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır. IIS üzerinde çalıştırmak ASP.NET Core'nın web ana bilgisayarı varsa çalışır. Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha sonraki bölümde.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar. Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için. <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

`Main` Yöntemi kullanan <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web uygulama ana bilgisayarı oluşturma. Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır. Diğer web sunucuları gibi [WebListener](xref:fundamentals/servers/weblistener), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha sonraki bölümde.

`WebHostBuilder` dahil olmak üzere birçok isteğe bağlı yöntemler sağlar <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> IIS ve IIS Express barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için. <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.

::: moniker-end

## <a name="startup"></a>Başlangıç

`UseStartup` Metodunda `WebHostBuilder` belirtir `Startup` uygulamanız için sınıf:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

`Startup` Sınıfıdır istek işleme ardışık düzen tanımladığınız ve uygulama tarafından gereken diğer hizmetler yapılandırıldığı. `Startup` Sınıfı genel olmalıdır ve aşağıdaki yöntemleri içerir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> tanımlar [Hizmetleri](#dependency-injection-services) uygulamanız (örneğin, ASP.NET Core MVC, Entity Framework Core kimlik) tarafından kullanılır. <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> tanımlar [ara yazılım](xref:fundamentals/middleware/index) isteği işlem hattında çağrılır.

Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

## <a name="content-root"></a>İçerik kök

Temel yol gibi uygulama tarafından kullanılan herhangi bir içerik için içerik köküdür [Razor sayfaları](xref:razor-pages/index), MVC görünümleri ve statik varlıklar. Varsayılan olarak, uygulama barındırma yürütülebilir dosyası için uygulama temel yolu ile aynı konumda içerik kök dizinidir.

## <a name="web-root"></a>Web kökü

Bir uygulamanın web kök CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (Hizmetler)

A *hizmet* bir uygulamada ortak tüketim için hazırlanmış bir bileşendir. Hizmetleri aracılığıyla yapılan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). ASP.NET Core içeren destekleyen yerel bir denetimi tersine çevirme (IOC) kapsayıcı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) varsayılan olarak. İsterseniz, varsayılan kapsayıcı değiştirebilirsiniz. Ek olarak kendi [kaybolmasını avantajı eşlenmesiyle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI Hizmetleri gibi yapar [günlüğü](xref:fundamentals/logging/index), uygulamanız genelinde kullanılabilir.

Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Ara yazılım

ASP.NET Core, istek kullanarak işlem hattı oluşturma [ara yazılım](xref:fundamentals/middleware/index). ASP.NET Core ara yazılım üzerinde zaman uyumsuz işlemler gerçekleştiren bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.

Kural gereği, çağırarak "XYZ" adlı bir ara yazılım bileşeni ardışık düzenine eklenen bir `UseXYZ` uzantı yönteminde `Configure` yöntemi.

ASP.NET Core içeren zengin bir yerleşik ara yazılım ve kendi özel bir ara yazılım yazabilirsiniz. [.NET (OWIN) için açık Web arabirimi](xref:fundamentals/owin), web sunucuları, ölçeklendirilebilmeleri web apps sağlayan ASP.NET Core uygulamalarında desteklenir.

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index> ve <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>HTTP isteklerini başlatma

<xref:System.Net.Http.IHttpClientFactory> erişmek kullanılabilir <xref:System.Net.Http.HttpClient> HTTP isteğinde bulunmak için örnekleri.

Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Ortamlar

Ortamları gibi *geliştirme* ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan ve bir ortam değişkeni, ayarlar dosyası ve komut satırı bağımsız değişkeni kullanılarak ayarlanabilir.

Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

## <a name="hosting"></a>Barındırma

ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*, uygulama başlatma ve ömür yönetimi için sorumlu olduğu.

Daha fazla bilgi için bkz. <xref:fundamentals/host/index>.

## <a name="servers"></a>Sunucular

Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez. Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır. İletilen istek, arabirimler aracılığıyla erişilebilen bir özellik nesne olarak paketlenir. ASP.NET Core içeren olarak adlandırılan bir yönetilen, platformlar arası web sunucusuna [Kestrel](xref:fundamentals/servers/kestrel). Kestrel'i yaygın olarak çalıştığı bir üretim web sunucusu gibi [IIS](https://www.iis.net/) veya [Ngınx](http://nginx.org) ters Ara sunucu yapılandırması. Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerde kullanıma sunulan bir uç sunucusu olarak da çalıştırılabilir.

Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Yapılandırma

ASP.NET Core ad-değer çiftlerine göre bir yapılandırma modeli kullanır. Yapılandırma modeli temel almayan <xref:System.Configuration> veya *web.config*. Yapılandırma ayarları sıralı bir dizi yapılandırma sağlayıcıları alır. Yerleşik yapılandırma sağlayıcıları, çeşitli dosya biçimleri (XML, JSON, INI), ortam değişkenleri ve komut satırı bağımsız değişkenleri destekler. Ayrıca, kendi özel yapılandırma sağlayıcıları yazabilirsiniz.

Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET Core bir çeşitli günlük sağlayıcılar ile çalışan API'si günlük kaydını destekler. Yerleşik sağlayıcılar, bir veya daha fazla hedefe gönderen günlükleri destekler. Üçüncü taraf günlük altyapılarına kullanılabilir.

Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Hata işleme

ASP.NET Core uygulamalarında Geliştirici özel durum sayfasında, özel hata sayfaları, statik durumu kod sayfaları ve başlangıç özel durum işleme dahil olmak üzere, hataları işleme için yerleşik senaryolar vardır.

Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.

## <a name="routing"></a>Yönlendirme

ASP.NET Core, yol işleyicisi uygulama isteklerinin Yönlendirme senaryoları sunar.

Daha fazla bilgi için bkz. <xref:fundamentals/routing>.

## <a name="file-providers"></a>Dosya sağlayıcıları

ASP.NET Core, dosya sistemi erişimini kullanarak platformlar arasında dosyaları ile çalışma için ortak bir arabirim sunan dosya sağlayıcıları soyutlar.

Daha fazla bilgi için bkz. <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Statik dosyalar

Statik dosya ara yazılımı, HTML, CSS, görüntü ve JavaScript dosyaları gibi statik dosyalar işlevi görür.

Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Oturum ve uygulama durumu

ASP.NET Core, bir kullanıcı bir web uygulaması gözatar sırada oturum ve uygulama durumu korumak için çeşitli yaklaşımlar sunar.

Daha fazla bilgi için bkz. <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Genelleştirme ve yerelleştirme

ASP.NET Core ile çok dilli bir Web sitesi oluşturma sitenizin daha geniş kitlelere ulaşmasını sağlar. ASP.NET Core, içeriği yerelleştirmek için farklı dillere ve kültürlere Hizmetleri ve ara yazılım sağlar.

Daha fazla bilgi için bkz. <xref:fundamentals/localization>.

## <a name="request-features"></a>İstek özellikleri

Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır. Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.

Daha fazla bilgi için bkz. <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Arka plan görevleri

Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*. Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.

Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Erişim HttpContext

`HttpContext` Razor sayfaları ve MVC isteklerini işleme sırasında otomatik olarak kullanılabilir. Durumlarda burada `HttpContext` değil kullanıma hazır erişebileceğiniz `HttpContext` aracılığıyla <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve kendi varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür. Sohbet, yürütebilmektedir oyunlar gibi uygulamalar için kullanılır ve gerçek zamanlı bir web uygulaması işlevindeki istediğiniz herhangi bir yerde. ASP.NET Core web yuvası senaryolarını destekler.

Daha fazla bilgi için bkz. <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App metapackage

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage paket Yönetimi basitleştirir.

Daha fazla bilgi için bkz. <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:

* Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Core tarafından desteklenmiyor.
* ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.

Daha fazla bilgi için bkz. <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET core ve .NET Framework çalışma zamanı

ASP.NET Core uygulaması, .NET Core veya .NET Framework çalışma zamanı hedefleyebilirsiniz.

Daha fazla bilgi için [.NET Core ve .NET Framework arasında seçim yapma](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>ASP.NET Core ile ASP.NET arasında seçim yapma

ASP.NET Core ile ASP.NET arasında seçim yapma hakkında daha fazla bilgi için bkz: <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
