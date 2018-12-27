---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/index
ms.openlocfilehash: 11dc6336ae7667038983c967f28232bef325f5bb
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637776"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

ASP.NET Core uygulaması bir web sunucusunu oluşturan bir konsol uygulaması olan kendi `Program.Main` yöntemi. `Main` Yöntemdir uygulamanın *yönetilen giriş noktasını*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

.NET Core ana bilgisayarı:

* Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).
* İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.

`Main` Yöntemini çağıran [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web ana bilgisayarı oluşturma. Oluşturucu bir web sunucusu tanımlayan yöntemleri vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu otomatik olarak ayrılır. ASP.NET Core'nın web ana bilgisayarı denemeleri çalıştırmak [Internet Information Services (IIS)](https://www.iis.net/)varsa. Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha ayrıntılı olarak [başlangıç](#startup) bölümü.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, dönüş türünü `WebHost.CreateDefaultBuilder` çağrısı birçok isteğe bağlı yöntemler sağlar. Bu yöntemlerin bazıları `UseHttpSys` HTTP.sys uygulamada barındırmak ve <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> kök içerik dizini belirtmek için. <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> Ve <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> yöntemleri yapı <xref:Microsoft.AspNetCore.Hosting.IWebHost> nesnesini uygulamasını barındıran ve HTTP isteklerini dinlemeye başlar.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

.NET Core ana bilgisayarı:

* Yükleri [.NET Core çalışma zamanı](https://github.com/dotnet/coreclr).
* İlk komut satırı bağımsız değişkeni olarak giriş noktasını içeren yönetilen ikili dosya yolunu kullanır (`Main`) ve kod yürütmeyi başlatır.

`Main` Yöntemi kullanan <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) bir web uygulama ana bilgisayarı oluşturma. Web sunucusu tanımlayan yöntemleri Oluşturucusu vardır (örneğin, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) ve başlangıç sınıfı (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Önceki örnekte [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu kullanılır. Diğer web sunucuları gibi [HTTP.sys](xref:fundamentals/servers/httpsys), uygun bir genişletme yöntemi çağrılarak kullanılabilir. `UseStartup` açıklanan daha ayrıntılı olarak [başlangıç](#startup) bölümü.

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

## <a name="web-root-webroot"></a>Web kökü (webroot)

Bir uygulamanın webroot CSS, JavaScript ve görüntü dosyaları gibi genel, statik kaynakları içeren proje dizinindedir. Varsayılan olarak, *wwwroot* webroot olduğu.

Razor için (*.cshtml*) dosyaları, eğik çizgi tilde `~/` webroot için işaret eder. İle başlayan yollar `~/` sanal yol adlandırılır.

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

Barındırma modeli ASP.NET Core doğrudan isteklerini dinlemez. Uygulama isteği iletmek için bir HTTP sunucusu uygulamasını barındırma modeli kullanır.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:

* [Kestrel'i](xref:fundamentals/servers/kestrel) yönetilen, platformlar arası web sunucusu sunucusudur. Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/). Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.
* IIS HTTP sunucusu (`IISHttpServer`) olan bir [işlem sunucusu](xref:fundamentals/servers/index#in-process-hosting-model) IIS için.
* [HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu, Windows üzerinde ASP.NET Core bir web sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması. Kestrel'i yönetilen, platformlar arası web sunucusudur. Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması. Kestrel'i yönetilen, platformlar arası web sunucusudur. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/). Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:

* [Kestrel'i](xref:fundamentals/servers/kestrel) yönetilen, platformlar arası web sunucusu sunucusudur. Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/). Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.
* [HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu, Windows üzerinde ASP.NET Core bir web sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması. Kestrel'i yönetilen, platformlar arası web sunucusudur. Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core kullanan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu uygulaması. Kestrel'i yönetilen, platformlar arası web sunucusudur. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/). Kestrel'i doğrudan Internet'e ASP.NET Core 2.0 veya sonraki sürümlerinde sunulan genel kullanıma yönelik bir uç sunucusu olarak da çalıştırılabilir.

---

::: moniker-end

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

## <a name="background-tasks"></a>Arka plan görevleri

Arka plan görevleri olarak gerçekleştirilen *barındırılan hizmetleri*. Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.

Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Erişim HttpContext

`HttpContext` Razor sayfaları ve MVC isteklerini işleme sırasında otomatik olarak kullanılabilir. Durumlarda burada `HttpContext` değil kullanıma hazır erişebileceğiniz `HttpContext` aracılığıyla <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve kendi varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Daha fazla bilgi için bkz. <xref:fundamentals/httpcontext>.
