---
title: ASP.NET Core değişiklik belirteçleriyle değişiklikleri Algıla
author: guardrex
description: Değişiklikleri izlemek için değişiklik belirteçlerini nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007213"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçleriyle değişiklikleri Algıla

Tarafından [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Ichangetoken arabirimi

<xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar. `IChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur. [Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.

`IChangeToken` iki özelliğe sahiptir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirtecin, geri çağırmaları etkin bir şekilde harekete geçirmediğini belirtir. `ActiveChangedCallbacks` `false`olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` yoklamalıdır. Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.

`IChangeToken` arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action\<Object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir. `HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.

## <a name="changetoken-class"></a>ChangeToken sınıfı

<xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır. `ChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur. [Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.

[ChangeToken. OnChange (Func\<IChannel>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:

* `Func<IChangeToken>` belirteci üretir.
* belirteç değiştiğinde `Action` çağrılır.

[ChangeToken. OnChange\<tstate > (Func\<ıhangetoken >, Action\<tstate >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `TState` geçirilen ek bir `Action`parametresini alır.

`OnChange` <xref:System.IDisposable>döndürür. <xref:System.IDisposable.Dispose*> çağırmak, belirteci daha fazla değişiklik için dinlemeyi durdurup belirtecin kaynaklarını serbest bırakır.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçlerinin örnek kullanımları

Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:

* Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için izlemek üzere bir `IChangeToken` oluşturur.
* değişiklik üzerine önbellek çıkarmaları tetiklemek için, `IChangeToken` belirteçleri önbellek girişlerine eklenebilir.
* `TOptions` değişiklikler için <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır. Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir `IChangeToken` döndürür.

## <a name="monitor-for-configuration-changes"></a>Yapılandırma değişikliklerini izle

Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.

Bu dosyalar, bir `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> için [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır. `reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir. Bu ayar <xref:Microsoft.Extensions.Hosting.Host> kullanışlı yöntemde görüntülenir <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>tarafından temsil edilir. `FileConfigurationSource` dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.

Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>tarafından sağlanır.

Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir. *AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür&mdash;örnek uygulama konsola bir ileti yazar.

Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir. Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler. Örnek, SHA1 dosya karma kullanır. Bir yeniden deneme, üstel geri dönme ile uygulanır. Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.

*Yardımcı programlar/yardımcı programlar. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Basit başlangıç değiştirme belirteci

Değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri çağırma işlemini yapılandırma yeniden yükleme belirtecine kaydedin.

`Startup.Configure`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` belirteci sağlar. Geri çağırma `InvokeChanged` yöntemidir:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

Geri aramanın `state`, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IWebHostEnvironment`geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ). Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` deyimin birden çok kez çalışmasını engellemek için kullanılır.

Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.

### <a name="monitor-configuration-changes-as-a-service"></a>Yapılandırma değişikliklerini hizmet olarak izle

Örnek şunları uygular:

* Temel başlangıç belirteci izleme.
* Hizmet olarak izleme.
* İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.

Örnek bir `IConfigurationMonitor` arabirimi oluşturur.

*Uzantılar/ConfigurationMonitor. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` belirteci sağlar. `InvokeChanged` geri çağırma yöntemidir. Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur. İki özellik kullanılır:

* `MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırıp çalıştıramayacağını gösterir.
* `CurrentState` &ndash;, Kullanıcı arabirimindeki kullanım için geçerli izleme durumunu açıklar.

`InvokeChanged` yöntemi önceki yaklaşımla benzerdir, bunun dışında:

* `MonitoringEnabled` `true`olmadığı müddetçe kodunu çalıştırmaz.
* Geçerli `state` `WriteConsole` çıktısındaki çıktısını verir.

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices`bir hizmet olarak kaydedilir:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar. `IConfigurationMonitor` örneği `IndexModel`eklenmiş.

*Pages/Index. cshtml. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak için kullanılır:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

`OnPostStartMonitoring` tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir. `OnPostStopMonitoring` tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.

Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.

*Sayfa/dizin. cshtml*:

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Önbelleğe alınmış dosya değişikliklerini izle

Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>kullanarak bellek içi önbelleğe alınabilir. Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır. Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.

Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar. Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez. Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.

Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller. Örnek uygulama, yaklaşımın bir uygulamasını gösterir.

Örnek, `GetFileContent` için şunu kullanır:

* Dosya içeriğini döndürür.
* Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.

*Yardımcı programlar/yardımcı programlar. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur. Hizmet `GetFileContent` yöntemi çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.

Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:

1. Dosya içeriği `GetFileContent`kullanılarak elde edilir.
1. Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir. Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.
1. Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır. Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.

Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır. `IWebHostEnvironment.ContentRootFileProvider`, uygulamanın `IWebHostEnvironment.ContentRootPath`işaret eden bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde etmek için kullanılır. `filePath`, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService`, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.

`Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.

Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken sınıfı

Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged`, temsil edilen herhangi bir belirteç `HasChanged` `true`ise bileşik belirteç raporlarında `true`. `ActiveChangeCallbacks`, temsil edilen herhangi bir belirteç `ActiveChangeCallbacks` `true`ise bileşik belirteç raporlarında `true`. Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Ichangetoken arabirimi

<xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar. `IChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur. [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.

`IChangeToken` iki özelliğe sahiptir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirtecin, geri çağırmaları etkin bir şekilde harekete geçirmediğini belirtir. `ActiveChangedCallbacks` `false`olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` yoklamalıdır. Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.

`IChangeToken` arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action\<Object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir. `HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.

## <a name="changetoken-class"></a>ChangeToken sınıfı

<xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır. `ChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur. [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.

[ChangeToken. OnChange (Func\<IChannel>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:

* `Func<IChangeToken>` belirteci üretir.
* belirteç değiştiğinde `Action` çağrılır.

[ChangeToken. OnChange\<tstate > (Func\<ıhangetoken >, Action\<tstate >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `TState` geçirilen ek bir `Action`parametresini alır.

`OnChange` <xref:System.IDisposable>döndürür. <xref:System.IDisposable.Dispose*> çağırmak, belirteci daha fazla değişiklik için dinlemeyi durdurup belirtecin kaynaklarını serbest bırakır.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçlerinin örnek kullanımları

Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:

* Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için izlemek üzere bir `IChangeToken` oluşturur.
* değişiklik üzerine önbellek çıkarmaları tetiklemek için, `IChangeToken` belirteçleri önbellek girişlerine eklenebilir.
* `TOptions` değişiklikler için <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır. Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir `IChangeToken` döndürür.

## <a name="monitor-for-configuration-changes"></a>Yapılandırma değişikliklerini izle

Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.

Bu dosyalar, bir `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> için [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır. `reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir. Bu ayar <xref:Microsoft.AspNetCore.WebHost> kullanışlı yöntemde görüntülenir <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>tarafından temsil edilir. `FileConfigurationSource` dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.

Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>tarafından sağlanır.

Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir. *AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür&mdash;örnek uygulama konsola bir ileti yazar.

Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir. Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler. Örnek, SHA1 dosya karma kullanır. Bir yeniden deneme, üstel geri dönme ile uygulanır. Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.

*Yardımcı programlar/yardımcı programlar. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Basit başlangıç değiştirme belirteci

Değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri çağırma işlemini yapılandırma yeniden yükleme belirtecine kaydedin.

`Startup.Configure`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` belirteci sağlar. Geri çağırma `InvokeChanged` yöntemidir:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

Geri aramanın `state`, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IHostingEnvironment`geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ). Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` deyimin birden çok kez çalışmasını engellemek için kullanılır.

Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.

### <a name="monitor-configuration-changes-as-a-service"></a>Yapılandırma değişikliklerini hizmet olarak izle

Örnek şunları uygular:

* Temel başlangıç belirteci izleme.
* Hizmet olarak izleme.
* İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.

Örnek bir `IConfigurationMonitor` arabirimi oluşturur.

*Uzantılar/ConfigurationMonitor. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` belirteci sağlar. `InvokeChanged` geri çağırma yöntemidir. Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur. İki özellik kullanılır:

* `MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırıp çalıştıramayacağını gösterir.
* `CurrentState` &ndash;, Kullanıcı arabirimindeki kullanım için geçerli izleme durumunu açıklar.

`InvokeChanged` yöntemi önceki yaklaşımla benzerdir, bunun dışında:

* `MonitoringEnabled` `true`olmadığı müddetçe kodunu çalıştırmaz.
* Geçerli `state` `WriteConsole` çıktısındaki çıktısını verir.

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices`bir hizmet olarak kaydedilir:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar. `IConfigurationMonitor` örneği `IndexModel`eklenmiş.

*Pages/Index. cshtml. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak için kullanılır:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

`OnPostStartMonitoring` tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir. `OnPostStopMonitoring` tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.

Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.

*Sayfa/dizin. cshtml*:

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Önbelleğe alınmış dosya değişikliklerini izle

Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>kullanarak bellek içi önbelleğe alınabilir. Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır. Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.

Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar. Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez. Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.

Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller. Örnek uygulama, yaklaşımın bir uygulamasını gösterir.

Örnek, `GetFileContent` için şunu kullanır:

* Dosya içeriğini döndürür.
* Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.

*Yardımcı programlar/yardımcı programlar. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur. Hizmet `GetFileContent` yöntemi çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.

Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:

1. Dosya içeriği `GetFileContent`kullanılarak elde edilir.
1. Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir. Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.
1. Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır. Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.

Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır. [Ihostingenvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) , uygulamanın <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>işaret eden bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde etmek için kullanılır. `filePath`, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService`, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.

`Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.

Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken sınıfı

Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged`, temsil edilen herhangi bir belirteç `HasChanged` `true`ise bileşik belirteç raporlarında `true`. `ActiveChangeCallbacks`, temsil edilen herhangi bir belirteç `ActiveChangeCallbacks` `true`ise bileşik belirteç raporlarında `true`. Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
