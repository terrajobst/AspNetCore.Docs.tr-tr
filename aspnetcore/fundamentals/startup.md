---
title: ASP.NET Core uygulaması başlangıç
author: ardalis
description: ASP.NET core'da başlangıç sınıfı, hizmet ve uygulamaların istek işlem hattı nasıl yapılandırdığını keşfedin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 465d33cc1f19428d5189b3a6fa7088ac402a9751
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927977"
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core uygulaması başlangıç

Tarafından [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), ve [Luke Latham](https://github.com/guardrex)

`Startup` Sınıfı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamaları kullanımı bir `Startup` adlı sınıfı `Startup` kural tarafından. `Startup` Sınıfı:

* İsteğe bağlı olarak dahil edebilirsiniz bir [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) uygulamanın hizmetlerini yapılandırmak için yöntemi.
* İçermelidir bir [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) uygulamanın istek işleme ardışık düzenini oluşturmak için yöntemi.

`ConfigureServices` ve `Configure` uygulama başlatıldığında çalışma zamanı tarafından çağrılır:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Belirtin `Startup` sınıfıyla [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yöntemi:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Web ana bilgisayarı için kullanılabilen bazı hizmetleri sağlayan `Startup` sınıf oluşturucusu. Ek hizmetler aracılığıyla uygulamanın eklediği `ConfigureServices`. Hem konak hem de uygulama hizmetleri ardından kullanılabilir `Configure` ve uygulama boyunca.

Yaygın [bağımlılık ekleme](xref:fundamentals/dependency-injection) içine `Startup` sınıftır eklemesine:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) ortamı tarafından hizmetleri yapılandırmak için.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) yapılandırması okunamıyor.
* [System.Diagnostics.tracesorce](/dotnet/api/microsoft.extensions.logging.iloggerfactory) bir Günlükçü içinde oluşturulacağı `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Ekleme alternatif `IHostingEnvironment` kurallarına dayalı bir yaklaşım kullanmaktır. Uygulamayı ayrı tanımlayabilirsiniz `Startup` sınıflar farklı ortamlar için (örneğin, `StartupDevelopment`) ve uygun `Startup` sınıfı, çalışma zamanında seçilidir. Geçerli ortamı olan adı sonekiyle sınıfı kurtarılmasına öncelik verilir. Uygulama geliştirme ortamında çalıştırılır ve her ikisi de içeren bir `Startup` sınıfı ve `StartupDevelopment` sınıfı `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Hakkında daha fazla bilgi edinmek için `WebHostBuilder`, bkz: [barındırma](xref:fundamentals/host/index) konu. Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz: [başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Createservicereplicalisteners() yöntemi

[Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) yöntemdir:

* İsteğe Bağlı
* Önce web ana bilgisayarı tarafından çağrılan `Configure` uygulamanın hizmetlerini yapılandırmak için yöntemi.
* Burada [yapılandırma seçenekleri](xref:fundamentals/configuration/index) kurala göre ayarlanır.

Hizmet kapsayıcıya Hizmetleri ekleme kullanımınıza bunları uygulama içinde hem de `Configure` yöntemi. Hizmetleri aracılığıyla çözümlenir [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Bazı hizmetler önce web ana bilgisayarı yapılandırabilirsiniz `Startup` yöntemi çağrılır. Ayrıntılar kullanılabilir [ASP.NET Core ana](xref:fundamentals/host/index) konu.

Önemli kurulum gerektiren özellikler için vardır `Add[Service]` üzerinde genişletme yöntemleri [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Tipik bir web uygulaması için Entity Framework, kimlik ve MVC Hizmetleri kaydeder:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Yapılandırma yöntemi

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) yöntemi, uygulamanın HTTP isteklerine nasıl yanıt verdiğini belirlemek için kullanılır. İstek ardışık düzenini ekleyerek yapılandırılır [ara yazılım](xref:fundamentals/middleware/index) bileşenleri bir [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) örneği. `IApplicationBuilder` kullanılabilir `Configure` yöntemi, ancak service kapsayıcısında kayıtlı değil. Barındırma oluşturur bir `IApplicationBuilder` ve doğrudan geçirir `Configure` ([başvuru kaynağı](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[ASP.NET Core şablonları](/dotnet/core/tools/dotnet-new) desteğiyle bir geliştirici özel durum sayfasında, işlem hattını yapılandırmanız [BrowserLink](http://vswebessentials.com/features/browserlink), hata sayfaları, statik dosyalar ve ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Her `Use` genişletme yöntemi, talep ardışık düzenine bir ara yazılım bileşeni ekler. Örneğin, `UseMvc` genişletme yöntemi ekler [yönlendirme ara yazılım](xref:fundamentals/routing) istek ardışık düzenine ve yapılandırır [MVC](xref:mvc/overview) varsayılan işleç olarak eşleştirir.

Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur. Kısa devre ara yazılım zincirinde oluşmuyorsa, her bir ara yazılım istemciye gönderilmeden önce isteği işlemek için ikinci bir şans sahiptir.

Gibi ek hizmetleri `IHostingEnvironment` ve `ILoggerFactory`, Yöntem imzasında de belirtilebilir. Kullanılabilir iseler ek hizmetler belirtildiğinde, eklenmiş.

Nasıl kullanılacağı hakkında daha fazla bilgi için `IApplicationBuilder` ve ara yazılım işlenme sırasını [ara yazılım](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Yöntemler

[Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) ve [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) kullanışlı yöntemler belirtmek yerine kullanılabileceği bir `Startup` sınıfı. Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok çağrılar `Configure` son yöntem çağrısı kullanın.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Başlangıç filtreleri

Kullanım [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) uygulamanın başında veya sonunda ara yazılımını yapılandırma [yapılandırma](#the-configure-method) ara yazılım ardışık düzenini. `IStartupFilter` bir ara yazılım önce veya sonra Ara yazılım tarafından kitaplıkları başında veya uygulamanın istek işleme ardışık sonuna eklenen çalıştığından emin olmak kullanışlıdır.

`IStartupFilter` tek bir yöntem uygular [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), alır ve döndürür bir `Action<IApplicationBuilder>`. Bir [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) uygulamanın istek ardışık düzenini yapılandırmak için bir sınıf tanımlar. Daha fazla bilgi için [IApplicationBuilder ile bir ara yazılım ardışık düzenini oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter` istek işlem hattı, bir veya daha fazla middlewares uygular. Filtreler, hizmet kapsayıcıya eklendikleri sırayla çağrılır. Ara yazılım önce filtreler ekleyebilir veya denetim sıradaki filtreye denetimini geçtikten sonra bu nedenle bunlar başına veya sonuna kadar uygulama ardışık ekleyin.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) ile bir ara yazılım kaydettirmek gösterilmektedir `IStartupFilter`. Örnek uygulama, bir ara yazılım seçenekleri değerini ayarlayan bir sorgu dizesi parametresi içerir:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Yapılandırılan `RequestSetOptionsStartupFilter` sınıfı:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Service kapsayıcısında kayıtlı `ConfigureServices`:

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Bir sorgu dizesi parametresi için zaman `option` MVC ara yazılımın yanıt işlemeden önce ara değer atama işlemleri sağlanır:

![İşlenen dizin sayfasını gösteren tarayıcı penceresi. Bu seçeneğin değeri, 'Den ara yazılım' değerini 'Dan Ara' için ayarlanmış ve sorgu dizesi parametresi sayfa isteğinde bulunan tabanlı olarak işlenir.](startup/_static/index.png)

Yürütme sırası ara yazılım tarafından sırasına göre ayarlanmış `IStartupFilter` kayıtları:

* Birden çok `IStartupFilter` uygulamaları aynı nesnelerle etkileşim. Sıralama önemlidir, sipariş kendi `IStartupFilter` hizmet kendi middlewares çalışması gereken sıraya uyacak şekilde kayıtları.
* Kitaplıkları, bir veya daha fazla ile Ara yazılım ekleyebilir `IStartupFilter` önce veya sonra diğer uygulama ara yazılım kayıtlı çalışan uygulamaları `IStartupFilter`. Çağrılacak bir `IStartupFilter` ara yazılım bir kitaplık tarafından eklenen bir ara yazılım önce `IStartupFilter`, hizmet kaydı kitaplığı hizmet kapsayıcıya eklenmeden önce konumlandırın. Daha sonra çağırmak için kitaplık eklendikten sonra hizmet kaydı getirin.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Yapılandırma, başlatma sırasında dış bütünleştirilmiş koddan ekleyin.

Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı. Daha fazla bilgi için [dış bütünleştirilmiş koddan uygulama geliştiren](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
* [StartupLoader sınıfı: FindStartupType yöntemi (başvuru kaynağı)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
