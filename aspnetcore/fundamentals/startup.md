---
title: ASP.NET Core uygulama başlangıç
author: ardalis
description: ASP.NET Core başlangıç sınıfında Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır nasıl bulur.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: f0b907e4322809dfe2bcd287bb064f35f5ebe150
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314126"
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core uygulama başlangıç

Tarafından [Steve Smith](https://ardalis.com), [zel Dykstra](https://github.com/tdykstra), ve [Luke Latham](https://github.com/guardrex)

`Startup` Sınıfı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamaları kullanımı bir `Startup` adlı sınıfı `Startup` kural tarafından. `Startup` Sınıfı:

* İsteğe bağlı olarak dahil edebileceğiniz bir [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) yöntemi uygulamanın Hizmetleri'ni yapılandırmak için.
* İçermelidir bir [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) uygulamanın istek işleme ardışık düzenini oluşturmak için yöntemi.

`ConfigureServices` ve `Configure` uygulama başlatıldığında çalışma zamanı tarafından adı verilir:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Belirtin `Startup` ile sınıf [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yöntemi:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` Sınıf oluşturucu ana bilgisayar tarafından tanımlanan bağımlılıklar kabul eder. Yaygın kullanımı [bağımlılık ekleme](xref:fundamentals/dependency-injection) içine `Startup` sınıftır eklemesine:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) Hizmetleri ortamı tarafından yapılandırılır.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulama başlatma sırasında yapılandırılır.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

İnjecting alternatif `IHostingEnvironment` kuralları tabanlı bir yaklaşım kullanmaktır. Uygulama ayrı tanımlayabilirsiniz `Startup` farklı ortamlar için sınıflar (örneğin, `StartupDevelopment`), ve uygun başlangıç sınıfı çalışma zamanında seçilir. Geçerli ortamda, bir ad soneki eşleşen sınıfı öncelik. Uygulama geliştirme ortamında çalıştırın ve her ikisi de içeriyorsa bir `Startup` sınıfı ve bir `StartupDevelopment` sınıfı, `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Daha fazla bilgi edinmek için `WebHostBuilder`, bkz: [barındırma](xref:fundamentals/host/index) konu. Başlatma sırasında hata işleme hakkında daha fazla bilgi için bkz: [başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices yöntemi

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) yöntemi:

* İsteğe Bağlı
* Önce web ana bilgisayarı tarafından çağrılan `Configure` yöntemi uygulamanın Hizmetleri'ni yapılandırmak için.
* Burada [yapılandırma seçenekleri](xref:fundamentals/configuration/index) kurala göre ayarlanır.

Hizmetler için hizmet kapsayıcı ekleme yapar bunları uygulama içinde ve kullanılabilir `Configure` yöntemi. Hizmetler aracılığıyla çözümlenmiş [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Web ana bilgisayarı önce bazı hizmetler yapılandırabilirsiniz `Startup` yöntemleri çağrılır. Ayrıntılar kullanılabilir [ASP.NET Core ana](xref:fundamentals/host/index) konu.

Önemli kurulum gerektiren özellikleri vardır `Add[Service]` genişletme yöntemleri [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Tipik web uygulaması için Entity Framework, kimlik ve MVC Hizmetleri kaydeder:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>ASP.NET Core MVC SetCompatibilityVersion 

`SetCompatibilityVersion` Yöntemi katılımı veya potansiyel olarak yeni ASP.NET MVC çekirdek 2.1 + sunulan davranışı değişiklikler çevirin bir uygulamanın olanak tanır. Büyük olasılıkla yeni davranış değişiklikler genellikle, MVC alt sistemi davranır nasıl ve ne bunlar **kodunuzu** çalışma zamanı tarafından çağrılır. Seçim tarafından son davranışı ve ASP.NET Core uzun vadeli davranışını alır.

Aşağıdaki kod, ASP.NET Core 2.1 uyumluluk modu ayarlar:

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

En son sürümünü kullanarak uygulamanızı test öneririz (`CompatibilityVersion.Version_2_1`). Çoğu uygulama en son sürümünü kullanarak davranışı değişiklikler olmaz beklenir. 

Çağıran uygulamalar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` davranış değişiklikleri ASP.NET Core 2.1 MVC ve sonraki 2.x sürümlerinde sunulan potansiyel olarak sonlandırmasını korunur. Bu koruma:

* Uygulanmaz 2.1 ve üzeri yapılan tüm değişiklikler, potansiyel olarak ASP.NET Core çalışma zamanı davranışı MVC alt sisteminde önemli değişiklikler için görünür duruma yöneliktir.
* Sonraki ana sürüm kapsamaz.

ASP.NET Core 2.1 ve yapmak sonraki 2.x uygulamaları için varsayılan uyumluluk **değil** çağrısı `SetCompatibilityVersion` 2.0 uyumluluğa yöneliktir. Diğer bir deyişle, değil çağırma `SetCompatibilityVersion` arama aynı `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Aşağıdaki kod uyumluluk modu dışında aşağıdaki davranışları ASP.NET Core 2.1 için ayarlar:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

Uygulamalar için uygun uyumluluk anahtarları kullanarak sonu davranış değişiklikleri karşılaşırsınız:

* En son sürümü kullanma ve belirli sonu davranış değişiklikleri dışında opt izin verir.
* Böylece son değişikliklerle birlikte çalışır uygulamanızı güncellemek için zaman verir.

[MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) sınıfı kaynak görüşlerinizi bildirmek iyi bir açıklama nelerin değiştiğini ve çoğu kullanıcı için bir geliştirme neden değişir.

Bazı ileriki bir tarihte olacaktır bir [ASP.NET Core 3.0 sürümü](https://github.com/aspnet/Home/wiki/Roadmap). Uyumluluk anahtarları tarafından desteklenen eski davranışları 3.0 sürümünde kaldırılacak. Neredeyse tüm kullanıcılar teknolojisinden yararlanan pozitif değişiklikler bunlar eşitleyerek. Bu değişiklikleri şimdi sunarız tarafından çoğu uygulamalar artık yararlanabilir ve diğerlerinin uygulamalarını güncellemek için gerekir.

::: moniker-end

## <a name="services-available-in-startup"></a>Başlangıç kullanılabilir hizmetler

Web ana bilgisayarı tarafından kullanılabilen bazı hizmetler sağlar `Startup` sınıfı oluşturucusu. Ek hizmetler aracılığıyla uygulama ekler `ConfigureServices`. Hem konak hem de uygulama hizmetleri sonra kullanılabilir olan `Configure` ve uygulama boyunca.

## <a name="the-configure-method"></a>Yapılandırma yöntemi

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) yöntemi uygulama HTTP isteklerine nasıl yanıt vereceğini belirtmek için kullanılır. İstek ardışık düzenini ekleyerek yapılandırılmış [ara yazılımı](xref:fundamentals/middleware/index) bileşenleri için bir [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) örneği. `IApplicationBuilder` kullanılabilir `Configure` yöntemi, ancak hizmet kapsayıcısında kayıtlı değil. Barındırma oluşturur bir `IApplicationBuilder` ve doğrudan geçirir `Configure` ([başvuru kaynağı](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[ASP.NET Core şablonları](/dotnet/core/tools/dotnet-new) bir geliştirici özel durum sayfası desteğiyle ardışık düzenini yapılandırmak [BrowserLink](http://vswebessentials.com/features/browserlink), hata sayfaları, statik dosyalar ve ASP.NET MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Her `Use` genişletme yöntemi, istek ardışık düzenine bir ara yazılım bileşeni ekler. Örneğin, `UseMvc` genişletme yöntemi ekler [yönlendirme Ara](xref:fundamentals/routing) istek ardışık düzenine ve yapılandırır [MVC](xref:mvc/overview) varsayılan işleyici olarak.

Her ara yazılım bileşeni istek kanalında, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur. Kısa devre ara yazılım zinciri gerçekleşmezse, her ara istemciye gönderilmeden önce isteğini işlemek için ikinci bir fırsat sahiptir.

Gibi ek hizmetleri `IHostingEnvironment` ve `ILoggerFactory`, yöntem imzası da belirtilebilir. Kullanılabilir ise ek hizmetler belirtildiğinde, eklenmiş.

Nasıl kullanılacağı hakkında daha fazla bilgi için `IApplicationBuilder` ve ara yazılım işleme sırasını [Ara](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Kullanışlı yöntemler

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) ve [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) belirtmek yerine kullanışlı yöntemler kullanılabilir bir `Startup` sınıfı. Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok çağrılar `Configure` son yöntem çağrısı kullanın.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Başlangıç filtreleri

Kullanım [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) bir uygulamanın başında veya sonunda ara yazılımını yapılandırma [yapılandırma](#the-configure-method) ara yazılım ardışık düzenini. `IStartupFilter` bir ara yazılım önce veya sonra Başlat veya uygulamanın istek işleme ardışık düzen sonunda kitaplıkları tarafından eklenen Ara çalıştığından emin olmak kullanışlıdır.

`IStartupFilter` tek bir yöntem uygulayan [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), alır ve döndüren bir `Action<IApplicationBuilder>`. Bir [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) bir uygulamanın istek ardışık düzenini yapılandırmak için bir sınıf tanımlar. Daha fazla bilgi için bkz: [IApplicationBuilder ile Ara yazılım ardışık düzenini oluşturmak](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter` istek ardışık düzeninde bir veya daha fazla middlewares uygular. Filtreler, hizmet kapsayıcısı eklendikleri sırayla çağrılır. Filtreleri önce ara ekleyebilir veya sonraki filtre denetimini geçtikten sonra bu nedenle bunlar başına veya uygulama ardışık sonuna ekleyin.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) bir ara yazılımı ile kaydetmek gösterilmiştir `IStartupFilter`. Örnek uygulaması bir sorgu dizesi parametresi bir seçenek değeri ayarlar bir ara yazılımı içerir:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Yapılandırılan `RequestSetOptionsStartupFilter` sınıfı:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Hizmet kapsayıcısında kayıtlı `ConfigureServices`:

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Bir sorgu dizesi parametresi için zaman `option` MVC ara yazılımın yanıt işlemeden önce ara değer atama işler sağlanır:

![İşlenen dizin sayfasını gösteren bir tarayıcı penceresi. 'Den ara yazılımı' seçenek kümesi 'Nden Ara' değerini ve sorgu dizesi parametresi sayfasıyla isteyen tabanlı olarak seçeneğinin değerini işlenir.](startup/_static/index.png)

Ara yazılım yürütme sırası olarak ayarlanmış tarafından sırasına göre `IStartupFilter` kayıtlar:

* Birden çok `IStartupFilter` uygulamaları aynı nesnelerle etkileşim. Sıralama önemliyse, sipariş kendi `IStartupFilter` hizmet kendi middlewares çalışması gerektiğini sıraya uyacak şekilde kayıtlar.
* Kitaplık, bir veya daha fazla ile Ara yazılım ekleyin `IStartupFilter` önce veya diğer uygulama ara yazılımı ile kaydedildikten sonra çalışan uygulamaları `IStartupFilter`. Çağrılacak bir `IStartupFilter` Ara kitaplığın tarafından eklenen bir ara yazılım önce `IStartupFilter`, kitaplık hizmet kapsayıcısı eklenmeden önce hizmet kaydı getirin. Daha sonra çağırmak için kitaplık eklendikten sonra hizmet kaydı getirin.

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>Dış bir derlemeye ait başlangıçta yapılandırması ekleniyor

Bir [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışında bir dış derlemesinden başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı. Daha fazla bilgi için bkz: [dış bütünleştirilmiş uygulama geliştirmek](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Ek kaynaklar

* [Barındırma](xref:fundamentals/host/index)
* [Birden çok ortam kullanma](xref:fundamentals/environments)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Günlüğe kaydetme](xref:fundamentals/logging/index)
* [Yapılandırma](xref:fundamentals/configuration/index)
* [StartupLoader sınıfı: FindStartupType yöntemi (başvuru kaynağı)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
