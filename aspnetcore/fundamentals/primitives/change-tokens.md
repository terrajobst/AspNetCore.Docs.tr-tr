---
title: "ASP.NET Core değişiklik belirteçleri değişikliklerle Algıla"
author: guardrex
description: "Değişiklik belirteçleri değişiklikleri izlemek için nasıl kullanılacağını öğrenin."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçleri değişikliklerle Algıla

Tarafından [Luke Latham](https://github.com/guardrex)

A *belirteci değiştirme* olan değişiklikleri izlemek için kullanılan genel amaçlı, alt düzey Yapı bloğu.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken arabirimi

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) gerçekleşen bir değişikliği bildirimleri yayar. `IChangeToken`bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasında NuGet paketi.

`IChangeToken`iki özelliklere sahiptir:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) belirteç proaktif olarak geri çağırmaları başlatır, belirtin. Varsa `ActiveChangedCallbacks` ayarlanır `false`, bir geri çağırma hiçbir zaman olarak adlandırılır ve uygulama yoklaması gerekir `HasChanged` değişiklikleri. Ayrıca, bir belirteç herhangi bir değişiklik meydana veya temel alınan değişiklik dinleyicisi atıldı ya da devre dışı hiçbir zaman iptal edilecek de mümkündür.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) bir değişiklik oluşup oluşmadığını belirten bir değer alır.

Bir yöntem arabirimi olan [RegisterChangeCallback (Eylem&lt;nesne&gt;, nesne)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), belirteç değiştiğinde çağrılan bir geri çağırma kaydeder. `HasChanged`geri çağırma çağrılmadan önce ayarlanmalıdır.

## <a name="changetoken-class"></a>ChangeToken sınıfı

`ChangeToken`statik sınıf gerçekleşen bir değişikliği bildirimleri yaymak için kullanılır. `ChangeToken`bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasında NuGet paketi.

`ChangeToken` [Değiştiğinde (Func&lt;IChangeToken&gt;, eylem)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) yöntemi yazmaçlar bir `Action` belirteci değiştiğinde çağırmak için:
* `Func<IChangeToken>`belirteç oluşturur.
* `Action`belirteç değiştiğinde çağrılır.

`ChangeToken`sahip bir [değiştiğinde&lt;TState&gt;(Func&lt;IChangeToken&gt;, eylem&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) ek alanaşırı`TState`belirteci tüketici geçirilen parametre `Action`.

`OnChange`döndüren bir [IDisposable](/dotnet/api/system.idisposable). Çağırma [Dispose](/dotnet/api/system.idisposable.dispose) ilerideki değişiklikler için dinleme gelen belirtecin durdurur ve belirtecin kaynakları serbest bırakır.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Örnek ASP.NET Core değişiklik belirteçleri kullanır

Değişiklik belirteçleri ASP.NET çekirdek nesnelerdeki değişiklikleri izleme belirgin alanlarında kullanılır:

* Dosyaları, yapılan değişiklikleri izleme için [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [izleme](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi oluşturur bir `IChangeToken` belirli dosyaları veya klasörleri izlemek için.
* `IChangeToken`belirteçleri önbellek çıkarmaları değişiklik tetiklemek için önbellek girişlerinin eklenebilir.
* İçin `TOptions` değişikliği, varsayılan [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) uyarlamasını [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bir veya daha fazla kabul eden bir aşırı sahip [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)örnekleri. Her oluşumu döndüren bir `IChangeToken` değişiklikleri izleme seçenekleri için değişiklik bildirimi geri araması kaydetme.

## <a name="monitoring-for-configuration-changes"></a>Yapılandırma değişikliklerini izleme

Varsayılan olarak, ASP.NET Core şablonlarını kullanma [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings. Development.JSON*, ve *appsettings. Production.JSON*) uygulama yapılandırma ayarlarını yüklenemiyor.

Bu dosyaları kullanılarak yapılandırılmış olan [AddJsonFile (IConfigurationBuilder, dize, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) genişletme yöntemi [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) kabul eden bir `reloadOnChange` parametresi (ASP.NET Çekirdek 1.1 ve üzeri). `reloadOnChange`yapılandırma dosyası değişiklikleri yeniden olmadığını gösterir. Bu ayarda bkz [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) kolaylık yöntemi [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([başvuru kaynağı](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Dosya tabanlı yapılandırma ile temsil edilir [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`kullanan [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([başvuru kaynağı](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) dosyaları izlemek için.

Varsayılan olarak, `IFileMonitor` tarafından sağlanan bir [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([başvuru kaynağı](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), kullanan [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) için yapılandırma dosyası izlemek için değiştirir.

Örnek uygulamayı yapılandırma değişikliklerini izleme için iki uygulamaları gösterir. Her iki *appsettings.json* dosya değişiklikleri veya dosyanın ortamı sürümü değişiklikler, her uygulama özel kodu yürütür. Örnek uygulaması bir ileti konsola yazar.

Bir yapılandırma dosyasının `FileSystemWatcher` tek yapılandırma dosyası değişikliği için birden çok belirteç geri aramalar tetikleyebilir. Örnek 's uygulama yapılandırma dosyaları dosya karmalarını denetleyerek bu soruna karşı korur. Dosya karmalarını denetimi yapılandırma dosyalarını en az biri özel kod çalıştırmadan önce değişti sağlar. Örnek dosya SHA1 karma kullanır (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Yeniden deneme bir üstel geri alma ile uygulanır. Dosya kilitleme dosyalardan birinde yeni bir karma hesaplama geçici olarak engelleyen oluşabilir olduğundan yeniden deneyin mevcuttur.

### <a name="simple-startup-change-token"></a>Basit başlangıç değişiklik simgesi

Bir belirteç tüketici kaydetmek `Action` için geri çağırma değişiklik bildirimleri yapılandırma yeniden yükleme belirtece (*haline*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`belirteç sağlar. Geri çağırma olduğu `InvokeChanged` yöntemi:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Geri çağırma içinde geçirmek için kullanılan `IHostingEnvironment`. Bu doğru belirlemek kullanışlıdır *appsettings* izlemek için yapılandırma JSON dosyasını *appsettings.&lt; Ortam&gt;.json*. Dosya karmalarını önlemek için kullanılan `WriteConsole` yapılandırma dosyasının yalnızca bir kez değiştiğinde birden çok kez birden çok belirteç geri aramalar nedeniyle çalışmasını deyimi.

Bu sistem uygulaması çalıştıran ve kullanıcı tarafından devre dışı bırakılamaz sürece çalıştırır.

### <a name="monitoring-configuration-changes-as-a-service"></a>Bir hizmet olarak yapılandırma değişikliklerini izleme

Örnek uygular:

* Temel başlangıç belirteci izleme.
* Bir hizmet olarak izleme.
* Etkinleştirip izleme devre dışı bırakmak için bir mekanizma.

Örnek kurar bir `IConfigurationMonitor` arabirimi (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Uygulanan sınıf `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`belirteç sağlar. `InvokeChanged`geri çağırma yöntemidir. `state` Bu örnekte, izleme durumu açıklayan bir dizedir. İki özellik kullanılır:

* `MonitoringEnabled`geri arama özel kod çalışması gerekip gerekmediğini gösterir.
* `CurrentState`kullanıcı arabirimini kullanmak için geçerli izleme durumu açıklar.

`InvokeChanged` Yöntemi, önceki yaklaşım, BT'nin dışında benzerdir:

* Kendi kod sürece çalışmaz `MonitoringEnabled` olan `true`.
* Ayarlar `CurrentState` kodu çalıştırdığınızda kayıtları açıklayıcı bir ileti için özellik dizesi.
* Geçerli Notlar `state` içinde kendi `WriteConsole` çıktı.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Bir örneği `ConfigurationMonitor` bir hizmet olarak kayıtlı `ConfigureServices` , *haline*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

Dizin Sayfası yapılandırma izleme üzerinden kullanıcı denetimi sunar. Örneğinin `IConfigurationMonitor` içine eklenen `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Bir düğme sağlar ve izlemeyi devre dışı bırakır:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Zaman `OnPostStartMonitoring` olan tetiklenen, izleme etkinleştirilmişse ve geçerli durumu temizlenir. Zaman `OnPostStopMonitoring` olan tetiklenen, izleme devre dışı bırakılır ve durumunu izleme gerçekleşen değil yansıtacak şekilde ayarlayın.

## <a name="monitoring-cached-file-changes"></a>Önbelleğe alınmış dosya değişiklikleri izleme

Dosya içeriği, önbelleğe alınan bellek içi kullanarak olabilir [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Bellek içi önbelleğe alma açıklanan [bellek içi önbelleğe alma](xref:performance/caching/memory) konu. Uygulama, aşağıda açıklandığı gibi ek adımlar alma olmadan *eski* kaynak veriler değişirse (güncel olmayan) verileri bir önbellekten döndürülür.

Hesaba bir önbelleğe alınan kaynak dosyası durumunu yenilenirken katarak olmayan bir [kayan bitiş tarihinin](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) önbellek verilerini eski dönemi müşteri adayları. Kayan sona erme süresi her istek için verileri yeniler, ancak dosyası hiçbir zaman önbelleğe yeniden. Dosyanın önbelleğe alınmış içeriği kullanan tüm uygulama büyük olasılıkla eski içerik alma tabi özellikleridir.

Senaryo önbelleğe alma bir dosyada değişiklik belirteçleri kullanarak eski dosya içeriği önbelleğinde engeller. Örnek uygulaması, bir uygulama yaklaşımı ortaya koyar.

Örnek kullanır `GetFileContent` için:

* Dosya içeriğini döndürür.
* Üstel geri alma burada bir dosya kilidi geçici olarak bir dosya okunmaya karşı engelliyor kapak çalışmaları için bir yeniden deneme algoritmasıyla uygulayın.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A `FileService` önbelleğe alınmış dosya aramalarını işlemek için oluşturulur. `GetFileContent` Yöntem çağrısı hizmetinin çalışır bellek içi önbellekten dosya içeriği almak ve çağırana dönmek (*Services/FileService.cs*).

Önbelleğe alınmış içeriği önbellek anahtarını kullanarak bulunamazsa, şu işlemler uygulanır:

1. Dosya içeriği kullanılarak elde edilen `GetFileContent`.
1. Bir değişiklik belirteci ile dosya sağlayıcısından alınan [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Dosya değiştirildiğinde belirtecin geri çağırma tetiklenir.
1. Dosya içeriği ile önbelleğe alınmış bir [kayan bitiş tarihinin](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) süresi. Değişiklik simgesi ile bağlı [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) önbelleğe sırada dosyası değişirse önbellek girişi çıkarmak için.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Hizmet önbelleğe alma bellek birlikte hizmet kapsayıcısında kayıtlı (*haline*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

Hizmetini kullanarak dosyanın içeriğini sayfa modelini yükler (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken sınıfı

Bir veya daha fazla temsil etmek için `IChangeToken` tek bir nesne durumlarda kullanmak [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) sınıfı ([başvuru kaynağı](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

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

`HasChanged`Birleşik belirteci raporlarda `true` herhangi bir belirteç temsil varsa `HasChanged` olan `true`. `ActiveChangeCallbacks`Birleşik belirteci raporlarda `true` herhangi bir belirteç temsil varsa `ActiveChangeCallbacks` olan `true`. Birden çok eşzamanlı değişikliği olayları meydana gelirse, bileşik değişikliği geri çağırma tam olarak bir kez çağrılır.

## <a name="see-also"></a>Ayrıca bkz.

* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıtları önbelleğe alma](xref:performance/caching/response)
* [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
