---
title: ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama
author: guardrex
description: Değişiklikleri izlemek için değişiklik belirteçleri kullanmayı öğrenin.
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/change-tokens
ms.openlocfilehash: 1cf3693764919a8fd064584ab7b7ad237e8465b3
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391452"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama

Tarafından [Luke Latham](https://github.com/guardrex)

A *belirteç değiştirme* bir genel amaçlı, alt düzey yapı değişiklikleri izlemek için kullanılan blok.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/change-tokens/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken arabirimi

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) gerçekleşen bir değişikliği bildirimleri yayar. `IChangeToken` bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri), başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasındaki NuGet paketi.

`IChangeToken` iki özelliğe sahiptir:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) belirteç proaktif bir şekilde geri çağırmaları harekete geçirirse gösterir. Varsa `ActiveChangedCallbacks` ayarlanır `false`, bir geri çağırma hiçbir zaman çağrılır ve uygulama yoklama yapmalıdır `HasChanged` değişiklikler. Ayrıca, herhangi bir değişiklik meydana veya temel alınan değişiklik dinleyicisi elden ya da devre dışı hiçbir zaman iptal edilmesi için bir belirteç de mümkündür.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) bir değişiklik meydana geldiğini gösteren bir değer alır.

Bir yöntem arabirimine sahiptir [RegisterChangeCallback (Eylem&lt;nesne&gt;, nesne)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), belirteç değiştiğinde çağrılan bir geri çağırma kaydeder. `HasChanged` geri çağırma çağrılmadan önce ayarlanmalıdır.

## <a name="changetoken-class"></a>ChangeToken sınıfı

`ChangeToken` statik sınıf gerçekleşen bir değişikliği bildirimleri yaymak için kullanılır. `ChangeToken` bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasındaki NuGet paketi.

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, eylem)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) yöntemi kayıtları bir `Action` belirteç değiştiğinde çağrılacak:

* `Func<IChangeToken>` belirteci oluşturur.
* `Action` belirteç değiştiğinde çağrılır.

`ChangeToken` sahip bir [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, eylem&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) ek bir alan aşırı yüklemesini `TState` belirteç tüketici geçirilen parametre `Action`.

`OnChange` döndürür bir [IDisposable](/dotnet/api/system.idisposable). Çağırma [Dispose](/dotnet/api/system.idisposable.dispose) ilerideki değişiklikler için dinleme gelen belirteç durdurur ve belirtecin kaynakları serbest bırakır.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Örnek ASP.NET Core değişiklik belirteçler kullanır

ASP.NET Core nesneleri için değişiklik izleme tanınmış alanlarda değişiklik belirteçleri kullanılır:

* Dosya değişiklikleri izleme [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi oluşturur bir `IChangeToken` belirtilen dosyaların veya klasörlerin izlemek için.
* `IChangeToken` belirteçleri önbelleğe çıkarmaları değişiklik tetiklemek için önbellek girişlerinin eklenebilir.
* İçin `TOptions` değiştirir, varsayılan [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) uygulaması [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bir veya daha fazla kabul eden aşırı yüklenmiş [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)örnekleri. Her bir örnek döndürür bir `IChangeToken` değişiklikleri izleme seçenekleri için değişiklik bildirimi geri araması kaydetme.

## <a name="monitoring-for-configuration-changes"></a>Yapılandırma değişikliklerini izleme

Varsayılan olarak, ASP.NET Core şablonları kullanın [JSON yapılandırma dosyaları](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, ve *appsettings. Production.JSON*) uygulama yapılandırma ayarları yüklenemedi.

Bu dosya kullanılarak yapılandırılır [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) genişletme yöntemini [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) kabul eden bir `reloadOnChange` parametre (ASP.NET Core 1.1 ve üzeri). `reloadOnChange` yapılandırma dosya değişikliklerinde yüklenmesi durumunda gösterir. Bu ayarda bkz [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) kolaylık yöntemi [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Dosya tabanlı yapılandırma tarafından temsil edilen [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` kullanan [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) dosyaları izlemek için.

Varsayılan olarak, `IFileMonitor` tarafından sağlanan bir [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), kullanan [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) yapılandırma dosya değişiklikleri izlemek için.

Örnek uygulamayı yapılandırma değişiklikleri izlemek için iki uygulamaları gösterir. Ya da *appsettings.json* dosya değişikliklerini veya dosyanın ortam sürümü değiştirir, her uygulama özel kodu yürütür. Örnek uygulama bir ileti konsola yazar.

Bir yapılandırma dosyasının `FileSystemWatcher` tek bir yapılandırma dosyası değişikliği için birden fazla belirteç geri çağırmaları tetikleyebilirsiniz. Örnek'ın uygulama yapılandırma dosyaları dosya karmalarının kontrol ederek bu soruna karşı korur. Dosya karmalarını denetimi yapılandırma dosyalarını en az biri özel kod çalıştırmadan önce değişti sağlar. Dosya SHA1 karma örnek kullanır (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Bir üstel geri alma ile yeniden uygulanır. Yeniden deneyin, çünkü dosya kilitleme geçici olarak dosyalardan birinde yeni bir karma hesaplaması engelleyen oluşabilir mevcuttur.

### <a name="simple-startup-change-token"></a>Basit başlangıç belirteci Değiştir

Bir belirteç tüketici kaydetme `Action` geri çağırma için değişiklik bildirimleri yapılandırma yeniden yükleme belirteci (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` belirteç sağlar. Aramasıdır `InvokeChanged` yöntemi:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Geri çağırma içinde iletmek için kullanılan `IHostingEnvironment`. Bu doğru belirlemek yararlıdır *appsettings* izlemek için yapılandırma JSON dosyası *appsettings.&lt; Ortam&gt;.json*. Dosya karmalarını önlemek için kullanılan `WriteConsole` yapılandırma dosyasının yalnızca bir kez değiştirdiği zaman birden çok kez birden fazla belirteç geri çağırmaları nedeniyle çalışmasını deyimi.

Bu sistem, uygulamayı çalıştıran ve kullanıcı tarafından devre dışı bırakılamaz sürece çalıştırır.

### <a name="monitoring-configuration-changes-as-a-service"></a>Hizmet olarak yapılandırma değişikliklerini izleme

Örnek uygular:

* Temel başlangıç belirteci izleme.
* Hizmet olarak izleme.
* Enable ve disable izleme için bir mekanizma.

Örnek kurar bir `IConfigurationMonitor` arabirimi (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Uygulanan sınıfının oluşturucusu, `ConfigurationMonitor`, değişiklik bildirimlerinin bir geri çağırma kaydeder:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` belirteç sağlar. `InvokeChanged` geri çağırma yöntemidir. `state` Bu örnekte bir başvurudur `IConfigurationMonitor` izleme durumuna erişmek için kullanılan bir örnek. İki özellikleri kullanılır:

* `MonitoringEnabled` geri çağırma kendi özel kod çalışması gerekip gerekmediğini gösterir.
* `CurrentState` kullanıcı arabirimini kullanmak için geçerli izleme durumunu açıklar.

`InvokeChanged` Yöntemi, önceki yaklaşımı, BT'nin dışında benzerdir:

* Kendi kod sürece çalışmaz `MonitoringEnabled` olduğu `true`.
* Geçerli notları `state` içinde kendi `WriteConsole` çıktı.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Bir örneği `ConfigurationMonitor` bir hizmet olarak kayıtlı `ConfigureServices` , *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

Dizin Sayfası izleme yapılandırması üzerinde kullanıcı denetimi sunar. Örneğini `IConfigurationMonitor` içine eklenen `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Bir düğme etkinleştirir ve izlemeyi devre dışı bırakır:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Zaman `OnPostStartMonitoring` olan tetiklenen, izleme etkin olduğundan ve geçerli durum temizlenir. Zaman `OnPostStopMonitoring` olan tetiklenen, izleme devre dışıdır ve durum izleme gerçekleşen olmayan yansıtacak şekilde ayarlanır.

## <a name="monitoring-cached-file-changes"></a>Önbelleğe alınan dosya değişikliklerini izleme

Dosya içeriği, önbelleğe alınan bellek içi kullanarak olabilir [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Bellek içi önbelleğe alma açıklanan [bellek içi önbelleğe alma](xref:performance/caching/memory) konu. Uygulama, aşağıda açıklandığı gibi ek adımları almadan *eski* (eski) veri kaynak veriler değiştiğinde olursa bir önbellekten döndürülür.

Hesaba bir önbelleğe alınan kaynak dosyası durumunu yenilenirken katılarak değil bir [kayan zaman aşımı](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) süresi müşteri adayları getirse verileri önbelleğe almak için. Kayan zaman aşımı süresi her istek için verileri yeniler, ancak dosya hiçbir zaman önbelleğine yüklenir. Önbelleğe alınan dosyanın içeriğini kullanan herhangi bir uygulama özelliği, büyük olasılıkla eski bir içerik alma tabi ' dir.

Senaryo önbelleğe alma bir dosyada değişiklik belirteçleri kullanarak eski dosya önbelleğindeki içeriği engeller. Örnek uygulamayı yaklaşımın bir uygulamasını gösterir.

Örnek kullanır `GetFileContent` için:

* Dosya içeriğini döndürür.
* Bir üstel geri alma burada dosya kilitleme geçici olarak bir dosya okunmasını engelliyor kapak çalışmaları için yeniden deneme algoritmasıyla uygulayın.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A `FileService` önbelleğe alınmış dosya aramaları işlenecek oluşturulur. `GetFileContent` Yöntem çağrısının hizmetin çalışır çağırana döndürmesi ve bellek içi Önbelleği'ndeki dosya içeriğini almak (*Services/FileService.cs*).

Önbellek anahtarını kullanarak önbelleğe alınmış içerikleri bulunamazsa, şu işlemler uygulanır:

1. Dosya içeriğini kullanarak elde edilen `GetFileContent`.
1. İle dosya sağlayıcısından alınan bir değişiklik belirteci [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Belirtecin geri çağırma dosya değiştirildiğinde tetiklenir.
1. Dosya içeriği önbelleğe alınmış bir [kayan zaman aşımı](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) süresi. Belirteci Değiştir iliştirilir [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) önbelleğe alınır ancak dosyayı değiştirirse, önbellek girişi çıkarmak için.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Hizmet kapsayıcı hizmeti önbelleğe alma bellek birlikte kaydedilir (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Sayfa modeli hizmetini kullanarak dosyanın içeriğini yükler (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken sınıfı

Bir veya daha fazla temsil eden `IChangeToken` tek bir nesne örneğini kullanın [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) sınıfı.

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

`HasChanged` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `HasChanged` olduğu `true`. `ActiveChangeCallbacks` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `ActiveChangeCallbacks` olduğu `true`. Birden çok eş zamanlı değişikliği olayları meydana gelirse, bileşik bir değişikliği geri çağırma tam olarak bir kez çağrılır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Belleğe yüklenmiş önbellek](xref:performance/caching/memory)
* [Dağıtılmış önbellekle çalışma](xref:performance/caching/distributed)
* [Yanıtları önbelleğe alma](xref:performance/caching/response)
* [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
