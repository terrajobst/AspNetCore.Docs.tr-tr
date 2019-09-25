---
title: ASP.NET Core 'de uygulama başlatma
author: rick-anderson
description: ASP.NET Core ' deki başlangıç sınıfının Hizmetleri ve uygulamanın istek ardışık düzenini nasıl yapılandırdığını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 47194f786b2d32fb343e8f1078a4400d6db37293
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248329"
---
# <a name="app-startup-in-aspnet-core"></a>ASP.NET Core 'de uygulama başlatma

[Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)ve [Steve Smith](https://ardalis.com)

`Startup` Sınıfı Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamalar, kural `Startup` tarafından adlandırılan `Startup` bir sınıfı kullanır. `Startup` Sınıf:

* İsteğe bağlı olarak <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> , uygulamanın *hizmetlerini*yapılandırmak için bir yöntem içerir. Hizmet, uygulama işlevselliği sağlayan yeniden kullanılabilir bir bileşendir. &mdash; `ConfigureServices` <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> [](xref:fundamentals/dependency-injection) Hizmetler Ayrıca, bağımlılık ekleme (dı) veya aracılığıyla uygulama genelinde kullanılan ve tüketilen şekilde de açıklanır.&mdash;
* Uygulamanın istek <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> işleme ardışık düzenini oluşturmak için bir yöntem içerir.

`ConfigureServices`ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

Sınıf, uygulamanın [ana makinesi](xref:fundamentals/index#host) yapılandırıldığında belirtilir. `Startup` Sınıfı, genellikle [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) yöntemi ana bilgisayar Oluşturucu üzerinde çağırarak belirtilir: `Startup`

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

Ana bilgisayar, `Startup` sınıf oluşturucusunun kullanabildiği hizmetleri sağlar. Uygulama aracılığıyla `ConfigureServices`ek hizmetler ekler. Hem konak hem de uygulama Hizmetleri uygulama içinde ve `Configure` üzerinde kullanılabilir.

Şu kullanıldığında `Startup` oluşturucuya <xref:Microsoft.Extensions.Hosting.IHostBuilder>yalnızca aşağıdaki hizmet türleri eklenebilir:

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

Çoğu hizmet, `Configure` Yöntem çağrılana kadar kullanılabilir değildir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ana bilgisayar, `Startup` sınıf oluşturucusunun kullanabildiği hizmetleri sağlar. Uygulama aracılığıyla `ConfigureServices`ek hizmetler ekler. Hem konak hem de uygulama Hizmetleri uygulama içinde ve üzerinde `Configure` kullanılabilir.

`Startup` Sınıfa [bağımlılık ekleme](xref:fundamentals/dependency-injection) 'nin yaygın bir kullanımı, şu ekleme yapmak için kullanılır:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>Hizmetleri ortama göre yapılandırmak için.
* <xref:Microsoft.Extensions.Configuration.IConfiguration>yapılandırmasını okuyun.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory>içinde `Startup.ConfigureServices`bir günlükçü oluşturmak için.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Çoğu hizmet, `Configure` Yöntem çağrılana kadar kullanılabilir değildir.

::: moniker-end

### <a name="multiple-startup"></a>Çoklu başlangıç

Uygulama farklı ortamlar için ayrı `Startup` sınıflar tanımladığında (örneğin, `StartupDevelopment`), çalışma zamanında uygun `Startup` sınıf seçilir. Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir. Uygulama geliştirme ortamında çalıştırıldıysanız ve hem `Startup` sınıf `StartupDevelopment` hem de sınıf içeriyorsa, `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Konak hakkında daha fazla bilgi için [konağa](xref:fundamentals/index#host) bakın. Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz. [Başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Yöntemi:

* İsteğe bağlı.
* Uygulamanın hizmetlerini yapılandırma `Configure` yönteminden önce ana bilgisayar tarafından çağırılır.
* [Yapılandırma seçeneklerinin](xref:fundamentals/configuration/index) kurala göre ayarlandığı yer.

Konak, Yöntemler çağrılmadan önce `Startup` bazı hizmetleri yapılandırabilir. Daha fazla bilgi için bkz. [ana bilgisayar](xref:fundamentals/index#host).

Önemli kurulum `Add{Service}` gerektiren özellikler için üzerinde <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>uzantı yöntemleri vardır. Örneğin, DbContext **ekleyin** **, defaultıdentity ekleyin,** entityframeworkmağazalarını **ekleyin ve**RazorPages **ekleyin**:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

Hizmet kapsayıcısına hizmet eklemek, bunları uygulama içinde ve `Configure` yönteminde kullanılabilir hale getirir. Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya konumundan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>çözümlenir.

::: moniker range="< aspnetcore-3.0"

Hakkında`SetCompatibilityVersion`daha fazla bilgi Için bkz. [setcompatibilityversion](xref:mvc/compatibility-version) .

::: moniker-end

## <a name="the-configure-method"></a>Configure yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Yöntemi, uygulamanın http isteklerine nasıl yanıt verdiğini belirtmek için kullanılır. İstek ardışık düzeni, bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneğe [Ara yazılım](xref:fundamentals/middleware/index) bileşenleri eklenerek yapılandırılır. `IApplicationBuilder`, `Configure` yöntemi için kullanılabilir, ancak hizmet kapsayıcısına kayıtlı değildir. Barındırma bir `IApplicationBuilder` oluşturur ve doğrudan öğesine `Configure`geçirir.

[ASP.NET Core şablonlar](/dotnet/core/tools/dotnet-new) işlem hattını desteğiyle birlikte yapılandırır:

* [Geliştirici özel durum sayfası](xref:fundamentals/error-handling#developer-exception-page)
* [Özel durum işleyicisi](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP katı taşıma güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS yönlendirmesi](xref:security/enforcing-ssl)
* [Statik dosyalar](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) ve [Razor Pages](xref:razor-pages/index)

::: moniker range="< aspnetcore-3.0"

* [Genel Veri Koruma Yönetmeliği (GDPR)](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

Her `Use` genişletme yöntemi, istek ardışık düzenine bir veya daha fazla ara yazılım bileşeni ekler. Örneğin, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> [ara yazılımı](xref:fundamentals/middleware/index) [statik dosyaları](xref:fundamentals/static-files)sunacak şekilde yapılandırır.

İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, uygun olduğunda, zincirdeki bir sonraki bileşeni çağırmaktan veya zincirde kısa bir süre sonra sağlanmasından sorumludur.

::: moniker range=">= aspnetcore-3.0"

`IWebHostEnvironment`, Veya `ILoggerFactory` `Configure` gibi ek hizmetler, yöntem imzasında belirtilebilir. `ConfigureServices` Bu hizmetler varsa eklenir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`IHostingEnvironment` Ve `ConfigureServices` `Configure` gibi ek hizmetler veya ' de tanımlı herhangi bir şey, yöntem imzasında belirtilebilir. `ILoggerFactory` Bu hizmetler varsa eklenir.

::: moniker-end

Kullanımı `IApplicationBuilder` ve ara yazılım işleme sırası hakkında daha fazla bilgi için bkz <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Hizmetleri başlatmadan yapılandırma

Hizmetleri ve istek işleme işlem hattını, ana bilgisayar Oluşturucu üzerinde `Startup` bir sınıf, `ConfigureServices` çağrı `Configure` ve kullanışlı yöntemler kullanmadan yapılandırmak için. Birbirine eklenecek birden `ConfigureServices` çok çağrı. Birden çok `Configure` yöntem varsa, son `Configure` çağrı kullanılır.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a>Başlangıç filtreleriyle başlatmayı Genişlet

Bir <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> uygulamanın, ara yazılım [yapılandırma](#the-configure-method) işlem hattının başlangıcında veya sonunda ara yazılımı yapılandırmak için kullanın. `IStartupFilter``Configure` Yöntem işlem hattı oluşturmak için kullanılır. [Itartupfilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) , bir ara yazılımı kitaplıklar tarafından eklenen bir veya daha sonra çalışacak şekilde ayarlayabilir.

`IStartupFilter`öğesini alır ve `Action<IApplicationBuilder>`döndürür. <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Bir uygulamanın istek ardışık düzenini yapılandırmak için bir sınıf tanımlar.<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Daha fazla bilgi için bkz. [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter` biri, istek ardışık düzeninde bir veya daha fazla middlewares ekleyebilir. Filtreler, hizmet kapsayıcısına eklendikleri sırada çağrılır. Filtreler bir sonraki filtreye denetimi geçirmeden önce veya sonra bir ara yazılım ekleyebilir, böylece uygulama işlem hattının başına veya sonuna eklenir.

Aşağıdaki örnek, ile `IStartupFilter`bir ara yazılımın nasıl kaydettirildiğini gösterir. `RequestSetOptionsMiddleware` Ara yazılım bir sorgu dizesi parametresinden bir seçenek değeri ayarlar:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

, `RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` Sınıfında yapılandırılır:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

, `RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` Sınıfında yapılandırılır:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

, `IStartupFilter` İçindeki<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>hizmet kapsayıcısına kaydedilir.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

İçin `option` bir sorgu dizesi parametresi sağlandığında, ara yazılım ASP.NET Core ara yazılım tarafından yanıt oluşturmadan önce değer atamasını işler.

Ara yazılım yürütme sırası, `IStartupFilter` kayıt sırasıyla ayarlanır:

* Birden `IStartupFilter` çok uygulama aynı nesnelerle etkileşime geçebilir. Sıralama önemliyse, `IStartupFilter` hizmet kayıtlarını middlewares 'in çalıştırması gereken sırayla eşleşecek şekilde sıralayın.
* Kitaplıklar, ile `IStartupFilter`kaydolmadan önce veya sonra `IStartupFilter` çalışan bir veya daha fazla uygulamayla bir ara yazılım ekleyebilir. Bir ara yazılımı `IStartupFilter` , bir `IStartupFilter`kitaplık tarafından eklenen bir ara yazılımı çağırmak için:

  * Kitaplık hizmet kapsayıcısına eklenmeden önce hizmet kaydını konumlandırın.
  * Daha sonra çağırmak için, kitaplık eklendikten sonra hizmet kaydını konumlandırın.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Başlangıçta bir dış derlemeden yapılandırma Ekle

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Uygulama ,`Startup` uygulamanın sınıfı dışında bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler eklenmesine olanak sağlar. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
