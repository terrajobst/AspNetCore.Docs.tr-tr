---
title: ASP.NET core'da uygulama başlatma
author: tdykstra
description: ASP.NET core'da başlangıç sınıfı, hizmet ve uygulamaların istek işlem hattı nasıl yapılandırdığını öğrenmek.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: 7e1741d2bed15f36a967713a2f9bd0d93801c8d0
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874947"
---
# <a name="app-startup-in-aspnet-core"></a>ASP.NET core'da uygulama başlatma

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com)

`Startup` Sınıfı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

ASP.NET Core uygulamaları kullanımı bir `Startup` adlı sınıfı `Startup` kural tarafından. `Startup` Sınıfı:

* İsteğe bağlı olarak içeren bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> uygulamanın yapılandırmak için yöntem *Hizmetleri*. Uygulama işlevselliği sağlayan bir yeniden kullanılabilir bileşen hizmetidir. Hizmetleri yapılandırılmış&mdash;olarak da açıklanan *kayıtlı*&mdash;içinde `ConfigureServices` ve uygulama boyunca tüketilen [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* İçeren bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> uygulamanın istek işleme ardışık düzenini oluşturmak için yöntemi.

`ConfigureServices` ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

`Startup` Sınıfı, uygulamaya belirtilirse, uygulamanın [konak](xref:fundamentals/index#host) oluşturulmuştur. Konak uygulamanın ne zaman oluşturulmuştur `Build` konak Oluşturucusu'nda üzerinde çağrılır `Program` sınıfı. `Startup` Sınıf genellikle belirtilen çağırarak [WebHostBuilderExtensions.UseStartup\<TStartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) konak Oluşturucu yöntemi:

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

Kullanılabilir hizmet ana bilgisayarının sağladığı `Startup` sınıf oluşturucusu. Ek hizmetler aracılığıyla uygulamanın eklediği `ConfigureServices`. Hem konak hem de uygulama hizmetleri ardından kullanılabilir `Configure` ve uygulama boyunca.

Yaygın [bağımlılık ekleme](xref:fundamentals/dependency-injection) içine `Startup` sınıftır eklemesine:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> ortamı tarafından hizmetleri yapılandırmak için.
* <xref:Microsoft.Extensions.Configuration.IConfiguration> yapılandırması okunamıyor.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> içinde bir Günlükçü oluşturmak için `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Ekleme alternatif `IHostingEnvironment` kurallarına dayalı bir yaklaşım kullanmaktır. Ne zaman uygulama tanımlar ayrı `Startup` sınıflar farklı ortamlar için (örneğin, `StartupDevelopment`), uygun `Startup` sınıfı çalışma zamanında seçilen. Geçerli ortamı olan adı sonekiyle sınıfı kurtarılmasına öncelik verilir. Uygulama geliştirme ortamında çalıştırılır ve her ikisi de içeren bir `Startup` sınıfı ve `StartupDevelopment` sınıfı `StartupDevelopment` sınıfı kullanılır. Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Konak hakkında daha fazla bilgi için bkz. [konak](xref:fundamentals/index#host). Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz: [başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Createservicereplicalisteners() yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Yöntemdir:

* İsteğe bağlı.
* Önce konak tarafından çağrılan `Configure` uygulamanın hizmetlerini yapılandırmak için yöntemi.
* Burada [yapılandırma seçenekleri](xref:fundamentals/configuration/index) kurala göre ayarlanır.

Tipik bir düzen tüm çağırmaktır `Add{Service}` yöntemleri ve tüm çağrı `services.Configure{Service}` yöntemleri. Örneğin, [yapılandırma kimlik Hizmetleri](xref:security/authentication/identity#pw).

Bazı hizmetler önce konak yapılandırabilirsiniz `Startup` yöntemi çağrılır. Daha fazla bilgi için [konak](xref:fundamentals/index#host).

Önemli kurulum gerektiren özellikler için vardır `Add{Service}` üzerinde genişletme yöntemleri <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Tipik bir ASP.NET Core uygulaması için Entity Framework, kimlik ve MVC Hizmetleri kaydeder:

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

Hizmet kapsayıcıya Hizmetleri ekleme kullanımınıza bunları uygulama içinde hem de `Configure` yöntemi. Hizmetleri aracılığıyla çözümlenir [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

Bkz: [SetCompatibilityVersion](xref:mvc/compatibility-version) hakkında daha fazla bilgi için `SetCompatibilityVersion`.

## <a name="the-configure-method"></a>Yapılandırma yöntemi

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Yöntemi, uygulamanın HTTP isteklerine nasıl yanıt verdiğini belirlemek için kullanılır. İstek ardışık düzenini ekleyerek yapılandırılır [ara yazılım](xref:fundamentals/middleware/index) bileşenleri bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneği. `IApplicationBuilder` kullanılabilir `Configure` yöntemi, ancak service kapsayıcısında kayıtlı değil. Barındırma oluşturur bir `IApplicationBuilder` ve doğrudan geçirir `Configure`.

[ASP.NET Core şablonları](/dotnet/core/tools/dotnet-new) desteği ile işlem hattı yapılandırın:

* [Geliştirici özel durumu sayfası](xref:fundamentals/error-handling#developer-exception-page)
* [Özel durum işleyicisi](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP katı aktarım güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS yeniden yönlendirmesi](xref:security/enforcing-ssl)
* [Statik dosyalar](xref:fundamentals/static-files)
* [Genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr)
* ASP.NET Core [MVC](xref:mvc/overview) ve [Razor sayfaları](xref:razor-pages/index)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Her `Use` genişletme yöntemi, bir veya daha fazla ara yazılımı bileşenleri istek ardışık düzene ekler. Örneğin, `UseMvc` genişletme yöntemi ekler [yönlendirme ara yazılım](xref:fundamentals/routing) istek ardışık düzenine ve yapılandırır [MVC](xref:mvc/overview) varsayılan işleç olarak eşleştirir.

Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur. Kısa devre ara yazılım zincirinde oluşmuyorsa, her bir ara yazılım istemciye gönderilmeden önce isteği işlemek için ikinci bir şans sahiptir.

Gibi ek hizmetleri `IHostingEnvironment` ve `ILoggerFactory`, içinde belirtilebilir `Configure` metodu imzası. Kullanılabilir iseler ek hizmetler belirtildiğinde, eklenmiş.

Nasıl kullanılacağı hakkında daha fazla bilgi için `IApplicationBuilder` ve ara yazılım işlenme sırasını <xref:fundamentals/middleware/index>.

## <a name="convenience-methods"></a>Yöntemler

Kullanmadan, hizmetler ve istek işleme ardışık düzenini yapılandırmak için bir `Startup` sınıfı, çağrı `ConfigureServices` ve `Configure` konak oluşturucu üzerinde kullanışlı yöntemler. Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok `Configure` yöntem çağrıları vardır, son `Configure` çağrısı kullanılır.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a>Başlangıç başlangıç filtreleri ile genişletme

Kullanım <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> uygulamanın başında veya sonunda ara yazılımını yapılandırma [yapılandırma](#the-configure-method) ara yazılım ardışık düzenini. `IStartupFilter` bir ara yazılım önce veya sonra Ara yazılım tarafından kitaplıkları başında veya uygulamanın istek işleme ardışık sonuna eklenen çalıştığından emin olmak kullanışlıdır.

`IStartupFilter` tek bir yöntem uygular <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, alır ve döndürür bir `Action<IApplicationBuilder>`. Bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> uygulamanın istek ardışık düzenini yapılandırmak için bir sınıf tanımlar. Daha fazla bilgi için [IApplicationBuilder ile bir ara yazılım ardışık düzenini oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Her `IStartupFilter` istek işlem hattı, bir veya daha fazla middlewares uygular. Filtreler, hizmet kapsayıcıya eklendikleri sırayla çağrılır. Ara yazılım önce filtreler ekleyebilir veya denetim sıradaki filtreye denetimini geçtikten sonra bu nedenle bunlar başına veya sonuna kadar uygulama ardışık ekleyin.

Aşağıdaki örnek bir ara yazılımla kaydettirmek gösterilmektedir `IStartupFilter`.

`RequestSetOptionsMiddleware` Ara yazılım, bir sorgu dizesi parametresi bir seçenek değeri ayarlar:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` Yapılandırılan `RequestSetOptionsStartupFilter` sınıfı:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Service kapsayıcısında kayıtlı <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> ve artırmaktadır `Startup` gelen dışında `Startup` sınıfı:

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

Bir sorgu dizesi parametresi için zaman `option` MVC ara yazılımın yanıt işlemeden önce ara değer atama işlemleri sağlanır:

![İşlenen dizin sayfasını gösteren tarayıcı penceresi. Bu seçeneğin değeri, 'Den ara yazılım' değerini 'Dan Ara' için ayarlanmış ve sorgu dizesi parametresi sayfa isteğinde bulunan tabanlı olarak işlenir.](startup/_static/index.png)

Yürütme sırası ara yazılım tarafından sırasına göre ayarlanmış `IStartupFilter` kayıtları:

* Birden çok `IStartupFilter` uygulamaları aynı nesnelerle etkileşim. Sıralama önemlidir, sipariş kendi `IStartupFilter` hizmet kendi middlewares çalışması gereken sıraya uyacak şekilde kayıtları.
* Kitaplıkları, bir veya daha fazla ile Ara yazılım ekleyebilir `IStartupFilter` önce veya sonra diğer uygulama ara yazılım kayıtlı çalışan uygulamaları `IStartupFilter`. Çağrılacak bir `IStartupFilter` ara yazılım bir kitaplık tarafından eklenen bir ara yazılım önce `IStartupFilter`, hizmet kaydı kitaplığı hizmet kapsayıcıya eklenmeden önce konumlandırın. Daha sonra çağırmak için kitaplık eklendikten sonra hizmet kaydı getirin.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Yapılandırma, başlatma sırasında dış bütünleştirilmiş koddan ekleyin.

Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
