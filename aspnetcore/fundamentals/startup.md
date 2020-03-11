---
title: ASP.NET Core 'de uygulama başlatma
author: rick-anderson
description: ASP.NET Core ' deki başlangıç sınıfının Hizmetleri ve uygulamanın istek ardışık düzenini nasıl yapılandırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: fundamentals/startup
ms.openlocfilehash: e3249df4b7388beeff13fe4b4e0ff481c35725c5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667659"
---
# <a name="app-startup-in-aspnet-core"></a>ASP.NET Core 'de uygulama başlatma

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)ve [Steve Smith](https://ardalis.com)

`Startup` sınıfı Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

::: moniker range=">= aspnetcore-3.0"

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamalar, kuralına göre `Startup` adlı bir `Startup` sınıfını kullanır. `Startup` Sınıfı:

* İsteğe bağlı olarak, uygulamanın *hizmetlerini*yapılandırmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi içerir. Hizmet, uygulama işlevselliği sağlayan yeniden kullanılabilir bir bileşendir. Hizmetler `ConfigureServices` *kaydedilir* ve [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>aracılığıyla uygulama genelinde tüketilebilir.
* Uygulamanın istek işleme ardışık düzenini oluşturmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi içerir.

`ConfigureServices` ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.


`Startup` sınıfı, uygulamanın [ana bilgisayarı](xref:fundamentals/index#host) yapılandırıldığında belirtilir. `Startup` sınıfı genellikle konak Oluşturucu 'da [Webhostbuilderextensions. UseStartup\<tstartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) yöntemi çağırarak belirtilir:

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

Ana bilgisayar `Startup` sınıfı Oluşturucusu tarafından kullanılabilen hizmetleri sağlar. Uygulama `ConfigureServices`aracılığıyla ek hizmetler ekler. Hem konak hem de uygulama hizmetleri `Configure` ve uygulama genelinde kullanılabilir.

[Genel ana bilgisayar](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) kullanılırken `Startup` oluşturucusuna yalnızca aşağıdaki hizmet türleri eklenebilir:

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

`Configure` yöntemi çağrılana kadar çoğu hizmet kullanılamaz.

### <a name="multiple-startup"></a>Çoklu başlangıç

Uygulama farklı ortamlarda ayrı `Startup` sınıfları tanımladığında (örneğin, `StartupDevelopment`), çalışma zamanında uygun `Startup` sınıfı seçilidir. Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir. Uygulama geliştirme ortamında çalıştırıldıysanız ve hem bir `Startup` sınıfını hem de bir `StartupDevelopment` sınıfını içeriyorsa `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Konak hakkında daha fazla bilgi için [konağa](xref:fundamentals/index#host) bakın. Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz. [Başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi:

* İsteğe bağlı.
* Uygulamanın hizmetlerini yapılandırmak için `Configure` yönteminden önce ana bilgisayar tarafından çağırılır.
* [Yapılandırma seçeneklerinin](xref:fundamentals/configuration/index) kurala göre ayarlandığı yer.

Konak `Startup` Yöntemler çağrılmadan önce bazı hizmetleri yapılandırabilir. Daha fazla bilgi için bkz. [ana bilgisayar](xref:fundamentals/index#host).

Önemli kurulum gerektiren özellikler için <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>`Add{Service}` uzantı yöntemleri vardır. Örneğin, DbContext **ekleyin** **, defaultıdentity ekleyin,** entityframeworkmağazalarını **ekleyin ve**RazorPages **ekleyin**:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

Hizmet kapsayıcısına hizmetleri eklemek, uygulama içinde ve `Configure` yönteminde kullanılabilir hale getirir. Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>üzerinden çözümlenir.

## <a name="the-configure-method"></a>Configure yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi, uygulamanın HTTP isteklerine nasıl yanıt verdiğini belirtmek için kullanılır. İstek ardışık düzeni, bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneğine [Ara yazılım](xref:fundamentals/middleware/index) bileşenleri eklenerek yapılandırılır. `IApplicationBuilder` `Configure` yöntemi tarafından kullanılabilir, ancak hizmet kapsayıcısında kayıtlı değildir. Barındırma bir `IApplicationBuilder` oluşturur ve doğrudan `Configure`geçirir.

[ASP.NET Core şablonlar](/dotnet/core/tools/dotnet-new) işlem hattını desteğiyle birlikte yapılandırır:

* [Geliştirici özel durum sayfası](xref:fundamentals/error-handling#developer-exception-page)
* [Özel durum işleyicisi](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP katı taşıma güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS yönlendirmesi](xref:security/enforcing-ssl)
* [Statik dosyalar](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) ve [Razor Pages](xref:razor-pages/index)


[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.

Her `Use` uzantısı yöntemi, istek ardışık düzenine bir veya daha fazla ara yazılım bileşeni ekler. Örneğin, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>, [ara yazılımı](xref:fundamentals/middleware/index) [statik dosyaları](xref:fundamentals/static-files)sunacak şekilde yapılandırır.

İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, uygun olduğunda, zincirdeki bir sonraki bileşeni çağırmaktan veya zincirde kısa bir süre sonra sağlanmasından sorumludur.

`IWebHostEnvironment`, `ILoggerFactory`veya `ConfigureServices`tanımlı herhangi bir şey gibi ek hizmetler `Configure` yöntemi imzasında belirtilebilir. Bu hizmetler varsa eklenir.

`IApplicationBuilder` kullanımı ve ara yazılım işleme sırası hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Hizmetleri başlatmadan yapılandırma

`Startup` sınıf kullanmadan Hizmetleri ve istek işleme işlem hattını yapılandırmak için, `ConfigureServices` çağırın ve konak Oluşturucu üzerinde `Configure` kullanışlı yöntemler kullanın. `ConfigureServices` birden çok çağrısı birbirine eklenir. Birden çok `Configure` yöntemi varsa, son `Configure` çağrısı kullanılır.

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

## <a name="extend-startup-with-startup-filters"></a>Başlangıç filtreleriyle başlatmayı Genişlet

<xref:Microsoft.AspNetCore.Hosting.IStartupFilter>kullanın:

* Ara yazılımı, bir uygulamanın başlangıcında veya sonunda `Use{Middleware}`açık bir çağrı olmadan [Ara yazılım ardışık düzeni yapılandırma.](#the-configure-method) `IStartupFilter`, ASP.NET Core tarafından, uygulama yazarının varsayılan ara yazılımı açıkça kaydetmesini sağlamak zorunda kalmadan, işlem hattının başlangıcına varsayılanlar eklemek için kullanılır. `IStartupFilter`, uygulama yazarı adına farklı bir bileşen çağrısının `Use{Middleware}` izin verir.
* `Configure` yöntemlerinin bir işlem hattı oluşturmak için. [Itartupfilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) , bir ara yazılımı kitaplıklar tarafından eklenen bir veya daha sonra çalışacak şekilde ayarlayabilir.

`IStartupFilter`, `Action<IApplicationBuilder>`alan ve döndüren <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>uygular. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, bir uygulamanın istek işlem hattını yapılandırmak için bir sınıfı tanımlar. Daha fazla bilgi için bkz. [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter`, istek ardışık düzeninde bir veya daha fazla middlewares ekleyebilir. Filtreler, hizmet kapsayıcısına eklendikleri sırada çağrılır. Filtreler bir sonraki filtreye denetimi geçirmeden önce veya sonra bir ara yazılım ekleyebilir, böylece uygulama işlem hattının başına veya sonuna eklenir.

Aşağıdaki örnek, `IStartupFilter`bir ara yazılımı nasıl kaydedeceğinizi gösterir. `RequestSetOptionsMiddleware` ara yazılım bir sorgu dizesi parametresinden bir seçenek değeri ayarlar:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` sınıfında yapılandırılır:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter`, <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>hizmet kapsayıcısına kaydedilir.

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

`option` için bir sorgu dizesi parametresi sağlandığında, ASP.NET Core ara yazılımı yanıtı oluşturmadan önce, ara yazılım değer atamasını işler.

Ara yazılım yürütme sırası `IStartupFilter` kayıt sırasıyla ayarlanır:

* Birden çok `IStartupFilter` uygulaması aynı nesnelerle etkileşime geçebilir. Sıralama önemliyse, kendi `IStartupFilter` hizmet kayıtlarını, middlewares çalıştırmaları sırasıyla eşleşecek şekilde sıralayın.
* Kitaplıklar, `IStartupFilter`kayıtlı olan diğer uygulama ara yazılımı ile önce veya sonra çalışan bir veya daha fazla `IStartupFilter` uygulaması olan ara yazılım ekleyebilir. Bir `IStartupFilter` ara yazılım bir kitaplık `IStartupFilter`tarafından eklenmeden önce çağırmak için:

  * Kitaplık hizmet kapsayıcısına eklenmeden önce hizmet kaydını konumlandırın.
  * Daha sonra çağırmak için, kitaplık eklendikten sonra hizmet kaydını konumlandırın.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Başlangıçta bir dış derlemeden yapılandırma Ekle

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamalar, kuralına göre `Startup` adlı bir `Startup` sınıfını kullanır. `Startup` Sınıfı:

* İsteğe bağlı olarak, uygulamanın *hizmetlerini*yapılandırmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi içerir. Hizmet, uygulama işlevselliği sağlayan yeniden kullanılabilir bir bileşendir. Hizmetler `ConfigureServices` *kaydedilir* ve [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>aracılığıyla uygulama genelinde tüketilebilir.
* Uygulamanın istek işleme ardışık düzenini oluşturmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi içerir.

`ConfigureServices` ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

`Startup` sınıfı, uygulamanın [ana bilgisayarı](xref:fundamentals/index#host) yapılandırıldığında belirtilir. `Startup` sınıfı genellikle konak Oluşturucu 'da [Webhostbuilderextensions. UseStartup\<tstartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) yöntemi çağırarak belirtilir:

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

Ana bilgisayar `Startup` sınıfı Oluşturucusu tarafından kullanılabilen hizmetleri sağlar. Uygulama `ConfigureServices`aracılığıyla ek hizmetler ekler. Hem konak hem de uygulama hizmetleri `Configure` ve uygulama genelinde kullanılabilir.

`Startup` sınıfa [bağımlılık ekleme](xref:fundamentals/dependency-injection) 'nin yaygın bir kullanımı, şu ekleme işlemini kullanmaktır:

* Hizmetleri ortama göre yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>.
* yapılandırmayı okumak için <xref:Microsoft.Extensions.Configuration.IConfiguration>.
* `Startup.ConfigureServices`bir günlükçü oluşturmak için <xref:Microsoft.Extensions.Logging.ILoggerFactory>.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

`Configure` yöntemi çağrılana kadar çoğu hizmet kullanılamaz.

### <a name="multiple-startup"></a>Çoklu başlangıç

Uygulama farklı ortamlarda ayrı `Startup` sınıfları tanımladığında (örneğin, `StartupDevelopment`), çalışma zamanında uygun `Startup` sınıfı seçilidir. Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir. Uygulama geliştirme ortamında çalıştırıldıysanız ve hem bir `Startup` sınıfını hem de bir `StartupDevelopment` sınıfını içeriyorsa `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Konak hakkında daha fazla bilgi için [konağa](xref:fundamentals/index#host) bakın. Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz. [Başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi:

* İsteğe bağlı.
* Uygulamanın hizmetlerini yapılandırmak için `Configure` yönteminden önce ana bilgisayar tarafından çağırılır.
* [Yapılandırma seçeneklerinin](xref:fundamentals/configuration/index) kurala göre ayarlandığı yer.

Konak `Startup` Yöntemler çağrılmadan önce bazı hizmetleri yapılandırabilir. Daha fazla bilgi için bkz. [ana bilgisayar](xref:fundamentals/index#host).

Önemli kurulum gerektiren özellikler için <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>`Add{Service}` uzantı yöntemleri vardır. Örneğin, DbContext **ekleyin** **, defaultıdentity ekleyin,** entityframeworkmağazalarını **ekleyin ve**RazorPages **ekleyin**:

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

Hizmet kapsayıcısına hizmetleri eklemek, uygulama içinde ve `Configure` yönteminde kullanılabilir hale getirir. Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>üzerinden çözümlenir.

`SetCompatibilityVersion`hakkında daha fazla bilgi için bkz. [Setcompatibilityversion](xref:mvc/compatibility-version) .

## <a name="the-configure-method"></a>Configure yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi, uygulamanın HTTP isteklerine nasıl yanıt verdiğini belirtmek için kullanılır. İstek ardışık düzeni, bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneğine [Ara yazılım](xref:fundamentals/middleware/index) bileşenleri eklenerek yapılandırılır. `IApplicationBuilder` `Configure` yöntemi tarafından kullanılabilir, ancak hizmet kapsayıcısında kayıtlı değildir. Barındırma bir `IApplicationBuilder` oluşturur ve doğrudan `Configure`geçirir.

[ASP.NET Core şablonlar](/dotnet/core/tools/dotnet-new) işlem hattını desteğiyle birlikte yapılandırır:

* [Geliştirici özel durum sayfası](xref:fundamentals/error-handling#developer-exception-page)
* [Özel durum işleyicisi](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP katı taşıma güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS yönlendirmesi](xref:security/enforcing-ssl)
* [Statik dosyalar](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) ve [Razor Pages](xref:razor-pages/index)
* [Genel Veri Koruma Yönetmeliği (GDPR)](xref:security/gdpr)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Her `Use` uzantısı yöntemi, istek ardışık düzenine bir veya daha fazla ara yazılım bileşeni ekler. Örneğin, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>, [ara yazılımı](xref:fundamentals/middleware/index) [statik dosyaları](xref:fundamentals/static-files)sunacak şekilde yapılandırır.

İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, uygun olduğunda, zincirdeki bir sonraki bileşeni çağırmaktan veya zincirde kısa bir süre sonra sağlanmasından sorumludur.

`IHostingEnvironment` ve `ILoggerFactory`gibi ek hizmetler veya `ConfigureServices`tanımlı herhangi bir şey `Configure` yöntemi imzasında belirtilebilir. Bu hizmetler varsa eklenir.

`IApplicationBuilder` kullanımı ve ara yazılım işleme sırası hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Hizmetleri başlatmadan yapılandırma

`Startup` sınıf kullanmadan Hizmetleri ve istek işleme işlem hattını yapılandırmak için, `ConfigureServices` çağırın ve konak Oluşturucu üzerinde `Configure` kullanışlı yöntemler kullanın. `ConfigureServices` birden çok çağrısı birbirine eklenir. Birden çok `Configure` yöntemi varsa, son `Configure` çağrısı kullanılır.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a>Başlangıç filtreleriyle başlatmayı Genişlet

<xref:Microsoft.AspNetCore.Hosting.IStartupFilter>kullanın:

* Ara yazılımı, bir uygulamanın başlangıcında veya sonunda `Use{Middleware}`açık bir çağrı olmadan [Ara yazılım ardışık düzeni yapılandırma.](#the-configure-method) `IStartupFilter`, ASP.NET Core tarafından, uygulama yazarının varsayılan ara yazılımı açıkça kaydetmesini sağlamak zorunda kalmadan, işlem hattının başlangıcına varsayılanlar eklemek için kullanılır. `IStartupFilter`, uygulama yazarı adına farklı bir bileşen çağrısının `Use{Middleware}` izin verir.
* `Configure` yöntemlerinin bir işlem hattı oluşturmak için. [Itartupfilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) , bir ara yazılımı kitaplıklar tarafından eklenen bir veya daha sonra çalışacak şekilde ayarlayabilir.

`IStartupFilter`, `Action<IApplicationBuilder>`alan ve döndüren <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>uygular. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, bir uygulamanın istek işlem hattını yapılandırmak için bir sınıfı tanımlar. Daha fazla bilgi için bkz. [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter`, istek ardışık düzeninde bir veya daha fazla middlewares ekleyebilir. Filtreler, hizmet kapsayıcısına eklendikleri sırada çağrılır. Filtreler bir sonraki filtreye denetimi geçirmeden önce veya sonra bir ara yazılım ekleyebilir, böylece uygulama işlem hattının başına veya sonuna eklenir.

Aşağıdaki örnek, `IStartupFilter`bir ara yazılımı nasıl kaydedeceğinizi gösterir. `RequestSetOptionsMiddleware` ara yazılım bir sorgu dizesi parametresinden bir seçenek değeri ayarlar:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` sınıfında yapılandırılır:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter`, <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>hizmet kapsayıcısına kaydedilir.

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

`option` için bir sorgu dizesi parametresi sağlandığında, ASP.NET Core ara yazılımı yanıtı oluşturmadan önce, ara yazılım değer atamasını işler.

Ara yazılım yürütme sırası `IStartupFilter` kayıt sırasıyla ayarlanır:

* Birden çok `IStartupFilter` uygulaması aynı nesnelerle etkileşime geçebilir. Sıralama önemliyse, kendi `IStartupFilter` hizmet kayıtlarını, middlewares çalıştırmaları sırasıyla eşleşecek şekilde sıralayın.
* Kitaplıklar, `IStartupFilter`kayıtlı olan diğer uygulama ara yazılımı ile önce veya sonra çalışan bir veya daha fazla `IStartupFilter` uygulaması olan ara yazılım ekleyebilir. Bir `IStartupFilter` ara yazılım bir kitaplık `IStartupFilter`tarafından eklenmeden önce çağırmak için:

  * Kitaplık hizmet kapsayıcısına eklenmeden önce hizmet kaydını konumlandırın.
  * Daha sonra çağırmak için, kitaplık eklendikten sonra hizmet kaydını konumlandırın.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Başlangıçta bir dış derlemeden yapılandırma Ekle

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end