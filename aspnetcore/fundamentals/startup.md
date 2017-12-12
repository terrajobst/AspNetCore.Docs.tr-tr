---
title: "Uygulama başlangıç ASP.NET Çekirdeği"
author: ardalis
description: "Hizmetler ve uygulamanın isteği ardışık düzeni ASP.NET Core başlangıç sınıfında nasıl yapılandırır bulur."
keywords: "ASP.NET Core, başlatma, yapılandırma yöntemi, ConfigureServices yöntemi"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 83b2647df8beec1feae33400224dacf9823be9b4
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="application-startup-in-aspnet-core"></a>Uygulama başlangıç ASP.NET Çekirdeği

Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)

`Startup` Sınıfı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamaları gerektiren bir `Startup` adlı sınıfı `Startup` kural tarafından. Başlangıç sınıfı adı belirtmeyin `Main` programın [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yöntemi. Bkz: [barındırma](xref:fundamentals/hosting) hakkında daha fazla bilgi için `WebHostBuilder`, çalıştığı önce `Startup`.

Ayrı tanımlayabilirsiniz `Startup` farklı ortamları ve uygun bir seçilecektir çalışma zamanında için sınıflar. Belirtirseniz `startupAssembly` içinde [WebHost yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) veya barındırma seçenekleri, bu başlangıç Derleme yüklenemedi ve arama bir `Startup` veya `Startup[Environment]` türü. Geçerli ortamı öncelik verilmesini, ad soneki eşleşmeleri, dolayısıyla uygulama çalıştırma sınıfı *geliştirme* ortamı ve her ikisi de içerir bir `Startup` ve `StartupDevelopment` sınıfı, `StartupDevelopment` sınıfı olacaktır kullanılır. Bkz: [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) içinde `StartupLoader` ve [birden çok ortamlarıyla çalışma](environments.md#startup-conventions).

Alternatif olarak, bir sabit tanımlayabilirsiniz `Startup` çağırarak ortam bağımsız olarak kullanılacak sınıfı `UseStartup<TStartup>`. Bu önerilen yaklaşımdır.

`Startup` Sınıfı oluşturucusu aracılığıyla sağlanan bağımlılıklar kabul edebileceği [bağımlılık ekleme](xref:fundamentals/dependency-injection). Ortak bir yaklaşım kullanmaktır `IHostingEnvironment` ayarlamak için [yapılandırma](xref:fundamentals/configuration/index) kaynakları.

`Startup` Sınıfı içermelidir bir `Configure` yöntemi ve istenirse bir `ConfigureServices` her ikisi de denir uygulama başladığında yöntemi. Sınıf da içerebilir [ortama özgü sürümleri bu yöntemlerin](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, varsa, önce çağrılır `Configure`.

Hakkında bilgi edinin [uygulama başlatma sırasında özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>ConfigureServices yöntemi

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yöntemdir isteğe bağlıdır; ancak kullandıysanız, önce çağrılır `Configure` web ana bilgisayarı tarafından yöntemi. Web ana bilgisayarı önce bazı hizmetler yapılandırabilirsiniz ``Startup`` yöntemleri çağrılmadan (bkz [barındırma](xref:fundamentals/hosting)). Kural tarafından [yapılandırma seçenekleri](xref:fundamentals/configuration/index) Bu yöntemde ayarlanır.

Önemli kurulum gerektiren özellikleri vardır `Add[Service]` genişletme yöntemleri [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). Bu örnekte varsayılan web sitesi şablondan Entity Framework, kimlik ve MVC hizmetleri kullanmak üzere uygulamayı yapılandırır:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Hizmetler için hizmet kapsayıcı ekleme kullanılabilir hale getirir bunları aracılığıyla uygulamanızı içinde [bağımlılık ekleme](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Başlangıç kullanılabilir hizmetler

ASP.NET Core bağımlılık ekleme uygulamanın başlatma sırasında hizmetleri sağlar. Üzerinde bir parametresi olarak uygun arabirimi ekleyerek bu hizmetleri isteyebilir, `Startup` sınıfının Oluşturucusu veya kendi `Configure` yöntemi. `ConfigureServices` Yöntemi yalnızca geçen bir `IServiceCollection` parametre (ancak ek parametreler gerekli olmayacak şekilde bu koleksiyondan alınması tüm kayıtlı hizmet olabilir).

Genellikle tarafından istenen Hizmetleri bazıları aşağıda verilmiştir `Startup` yöntemleri:

* Oluşturucuda: `IHostingEnvironment`,`ILogger<Startup>`
* İçinde `ConfigureServices`:`IServiceCollection`
* İçinde `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Tarafından eklenen tüm hizmetleri ``WebHostBuilder`` ``ConfigureServices`` yöntemi istenen tarafından ``Startup`` sınıfı oluşturucusu veya kendi ``Configure`` yöntemi. Kullanım `WebHostBuilder` sırasında tüm hizmetleri sağlamak için gereken `Startup` yöntemleri.

## <a name="the-configure-method"></a>Yapılandırma yöntemi

`Configure` Yöntemi, nasıl ASP.NET uygulaması HTTP isteklerine yanıt vereceğini belirtmek için kullanılır. İstek ardışık düzenini ekleyerek yapılandırılmış [ara yazılımı](middleware.md) bileşenleri için bir `IApplicationBuilder` bağımlılık ekleme tarafından sağlanan örneği.

Varsayılan web sitesi şablondan aşağıdaki örnekte, birkaç uzantı yöntemleri desteğiyle ardışık düzenini yapılandırmak için kullanılan [BrowserLink](http://vswebessentials.com/features/browserlink), hata sayfaları, statik dosyalar, ASP.NET MVC ve kimlik.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Her `Use` genişletme yöntemi ekler bir [ara yazılım](xref:fundamentals/middleware) istek ardışık düzenini bileşeni. Örneğin, `UseMvc` genişletme yöntemi ekler [yönlendirme](routing.md) Ara istek ardışık düzenine ve yapılandırır [MVC](xref:mvc/overview) varsayılan işleyici olarak.

Nasıl kullanılacağı hakkında daha fazla bilgi için `IApplicationBuilder`, bkz: [Ara](xref:fundamentals/middleware).

Ek hizmetler, ister `IHostingEnvironment` ve `ILoggerFactory` yöntem imzası da belirtilebilir bu hizmetleri; bu durumda olacaktır [eklenen](dependency-injection.md) kullanılabilir olmaları durumunda. 

## <a name="additional-resources"></a>Ek Kaynaklar

* [Birden çok ortamı ile çalışma](xref:fundamentals/environments)
* [Ara yazılım](xref:fundamentals/middleware)
* [Günlüğe kaydetme](xref:fundamentals/logging/index)
* [Yapılandırma](xref:fundamentals/configuration/index)
