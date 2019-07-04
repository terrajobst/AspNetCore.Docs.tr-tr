---
title: ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama
author: guardrex
description: Değişiklikleri izlemek için değişiklik belirteçleri kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/03/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 8b73b72d093b33edeb91bc78080e05aa312579ec
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561656"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama

Tarafından [Luke Latham](https://github.com/guardrex)

A *belirteç değiştirme* bir genel amaçlı, alt düzey oluşturma durumu değişiklikleri izlemek için kullanılan blok.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken arabirimi

<xref:Microsoft.Extensions.Primitives.IChangeToken> gerçekleşen bir değişikliği bildirimleri yayar. `IChangeToken` bulunan <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bir paket başvurusu için [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi.

`IChangeToken` iki özelliğe sahiptir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirteç proaktif bir şekilde geri çağırmaları harekete geçirirse gösterir. Varsa `ActiveChangedCallbacks` ayarlanır `false`, bir geri çağırma hiçbir zaman çağrılır ve uygulama yoklama yapmalıdır `HasChanged` değişiklikler. Ayrıca, herhangi bir değişiklik meydana veya temel alınan değişiklik dinleyicisi elden ya da devre dışı hiçbir zaman iptal edilmesi için bir belirteç de mümkündür.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişiklik meydana geldiğini gösteren bir değer alır.

`IChangeToken` Arabirimi içeren [RegisterChangeCallback (Eylem\<Nesne >, nesne)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemi belirteç değiştiğinde çağrılan bir geri çağırma kaydeder. `HasChanged` geri çağırma çağrılmadan önce ayarlanmalıdır.

## <a name="changetoken-class"></a>ChangeToken sınıfı

<xref:Microsoft.Extensions.Primitives.ChangeToken> statik sınıf gerçekleşen bir değişikliği bildirimleri yaymak için kullanılır. `ChangeToken` bulunan <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanı. Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bir paket başvurusu için [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi.

[ChangeToken.OnChange (Func\<IChangeToken >, eylem)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi kayıtları bir `Action` belirteç değiştiğinde çağrılacak:

* `Func<IChangeToken>` belirteci oluşturur.
* `Action` belirteç değiştiğinde çağrılır.

[ChangeToken.OnChange\<TState > (Func\<IChangeToken >, eylem\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı ek bir alan `TState` belirtece geçirilen parametre Tüketici `Action`.

`OnChange` döndürür bir <xref:System.IDisposable>. Çağırma <xref:System.IDisposable.Dispose*> ilerideki değişiklikler için dinleme gelen belirteç durdurur ve belirtecin kaynakları serbest bırakır.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Örnek ASP.NET Core değişiklik belirteçler kullanır

Değişiklik belirteçleri, ASP.NET Core tanınmış alanlarında nesnelere değişiklikleri izlemek için kullanılır:

* Dosya değişiklikleri izleme <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi oluşturur bir `IChangeToken` belirtilen dosyaların veya klasörlerin izlemek için.
* `IChangeToken` belirteçleri önbelleğe çıkarmaları değişiklik tetiklemek için önbellek girişlerinin eklenebilir.
* İçin `TOptions` değiştirir, varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulaması <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> bir veya daha fazla kabul eden aşırı yüklenmiş <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örnekleri. Her bir örnek döndürür bir `IChangeToken` değişiklikleri izleme seçenekleri için değişiklik bildirimi geri araması kaydetme.

## <a name="monitor-for-configuration-changes"></a>Yapılandırma değişiklikleri izleyin

Varsayılan olarak, ASP.NET Core şablonları kullanın [JSON yapılandırma dosyaları](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, ve *appsettings. Production.JSON*) uygulama yapılandırma ayarları yüklenemedi.

Bu dosya kullanılarak yapılandırılır [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> kabul eden bir `reloadOnChange` parametresi. `reloadOnChange` yapılandırma dosya değişikliklerinde yüklenmesi durumunda gösterir. Bu ayar görünür <xref:Microsoft.AspNetCore.WebHost> kolaylık yöntemi <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Dosya tabanlı yapılandırma tarafından temsil edilen <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>. `FileConfigurationSource` kullanan <xref:Microsoft.Extensions.FileProviders.IFileProvider> dosyaları izlemek için.

Varsayılan olarak, `IFileMonitor` tarafından sağlanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, kullanan <xref:System.IO.FileSystemWatcher> yapılandırma dosya değişiklikleri izlemek için.

Örnek uygulamayı yapılandırma değişiklikleri izlemek için iki uygulamaları gösterir. Varsa *appsettings* dosyaları değiştirmek, uygulamaları izleme dosyasının hem de özel kod yürütme&mdash;örnek uygulama bir ileti konsola yazar.

Bir yapılandırma dosyasının `FileSystemWatcher` tek bir yapılandırma dosyası değişikliği için birden fazla belirteç geri çağırmaları tetikleyebilirsiniz. Birden çok belirteç geri çağırmaları harekete geçirildiğinde sonra özel kod yalnızca çalıştığından emin olmak için dosya karmaları örnek'ın uygulama denetler. Örnek dosya SHA1 karma kullanır. Bir üstel geri alma ile yeniden uygulanır. Yeniden deneme mevcut olduğundan dosya kilitleme geçici olarak bir dosya çubuğunda yeni karma hesaplaması engelleyen ortaya çıkabilir.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Basit başlangıç belirteci Değiştir

Bir belirteç tüketici kaydetme `Action` geri çağırma için değişiklik bildirimleri yapılandırma yeniden yükleme belirteci.

İçinde `Startup.Configure`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` belirteç sağlar. Aramasıdır `InvokeChanged` yöntemi:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

`state` Geri çağırma içinde iletmek için kullanılan `IHostingEnvironment`, doğru olarak belirtmek için yararlı olan *appsettings* izlemek için yapılandırma dosyası (örneğin, *appsettings. Development.JSON* zaman geliştirme ortamında). Dosya karmalarını önlemek için kullanılan `WriteConsole` yapılandırma dosyasının yalnızca bir kez değiştirdiği zaman birden çok kez birden fazla belirteç geri çağırmaları nedeniyle çalışmasını deyimi.

Bu sistem, uygulamayı çalıştıran ve kullanıcı tarafından devre dışı bırakılamaz sürece çalıştırır.

### <a name="monitor-configuration-changes-as-a-service"></a>Hizmet olarak yapılandırma değişikliklerini izleyin

Örnek uygular:

* Temel başlangıç belirteci izleme.
* Hizmet olarak izleme.
* Enable ve disable izleme için bir mekanizma.

Örnek kurar bir `IConfigurationMonitor` arabirimi.

*Extensions/ConfigurationMonitor.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Uygulanan sınıfının oluşturucusu, `ConfigurationMonitor`, değişiklik bildirimlerinin bir geri çağırma kaydeder:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` belirteç sağlar. `InvokeChanged` geri çağırma yöntemidir. `state` Bu örnekte bir başvurudur `IConfigurationMonitor` izleme durumuna erişmek için kullanılan bir örnek. İki özellikleri kullanılır:

* `MonitoringEnabled` &ndash; geri çağırma kendi özel kod çalışması gerekip gerekmediğini gösterir.
* `CurrentState` &ndash; kullanıcı arabirimini kullanmak için geçerli izleme durumunu açıklar.

`InvokeChanged` Yöntemi, önceki yaklaşımı, BT'nin dışında benzerdir:

* Kendi kod sürece çalışmaz `MonitoringEnabled` olduğu `true`.
* Geçerli çıkarır `state` içinde kendi `WriteConsole` çıktı.

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Bir örneği `ConfigurationMonitor` bir hizmet olarak kayıtlı `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Dizin Sayfası izleme yapılandırması üzerinde kullanıcı denetimi sunar. Örneğini `IConfigurationMonitor` içine eklenen `IndexModel`.

*Pages/Index.cshtml.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Yapılandırma İzleyicisi (`_monitor`) etkinleştirmek veya izlemeyi devre dışı bırakın ve UI geri bildirim için geçerli durumunu ayarlamak için kullanılır:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

Zaman `OnPostStartMonitoring` olan tetiklenen, izleme etkin olduğundan ve geçerli durum temizlenir. Zaman `OnPostStopMonitoring` olan tetiklenen, izleme devre dışıdır ve durum izleme gerçekleşen olmayan yansıtacak şekilde ayarlanır.

Kullanıcı arabiriminde düğmeleri etkinleştirin ve izlemeyi devre dışı bırakın.

*Pages/Index.cshtml*:

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Önbelleğe alınan dosya değişikliklerini izleme

Dosya içeriği, önbelleğe alınan bellek içi kullanarak olabilir <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Bellek içi önbelleğe alma açıklanan [bellek içi önbelleğe alma](xref:performance/caching/memory) konu. Uygulama, aşağıda açıklandığı gibi ek adımları almadan *eski* (eski) veri kaynak veriler değiştiğinde olursa bir önbellekten döndürülür.

Örneğin, hesaba önbelleğe alınmış kaynak dosyası durumunu yenilenirken katılarak değil bir [kayan zaman aşımı](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süre eski önbelleğe alınmış dosya verileri için yol açar. Kayan zaman aşımı süresi her istek için verileri yeniler, ancak dosya hiçbir zaman önbelleğine yüklenir. Önbelleğe alınan dosyanın içeriğini kullanan herhangi bir uygulama özelliği, büyük olasılıkla eski bir içerik alma tabi ' dir.

Senaryo önbelleğe alma bir dosyada değişiklik belirteçleri kullanarak eski dosya içeriği önbelleğinde varlığını engeller. Örnek uygulamayı yaklaşımın bir uygulamasını gösterir.

Örnek kullanır `GetFileContent` için:

* Dosya içeriğini döndürür.
* Bir üstel geri alma burada dosya kilit geçici olarak bir dosyayı okuma engeller kapak çalışmaları için yeniden deneme algoritmasıyla uygulayın.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

A `FileService` önbelleğe alınmış dosya aramaları işlenecek oluşturulur. `GetFileContent` Yöntem çağrısının hizmetin çalışır çağırana döndürmesi ve bellek içi Önbelleği'ndeki dosya içeriğini almak (*Services/FileService.cs*).

Önbellek anahtarını kullanarak önbelleğe alınmış içerikleri bulunamazsa, şu işlemler uygulanır:

1. Dosya içeriğini kullanarak elde edilen `GetFileContent`.
1. İle dosya sağlayıcısından alınan bir değişiklik belirteci [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*). Belirtecin geri çağırma dosya değiştirildiğinde tetiklenir.
1. Dosya içeriği önbelleğe alınmış bir [kayan zaman aşımı](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresi. Belirteci Değiştir iliştirilir [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) önbelleğe alınır ancak dosyayı değiştirirse, önbellek girişi çıkarmak için.

Aşağıdaki örnekte, dosyaları uygulamanın içerik kök dizininde depolanır. [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) elde etmek için kullanılan bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> uygulamanın işaret eden <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>. `filePath` İle elde edilen [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` Hizmet kapsayıcı hizmeti önbelleğe alma bellek birlikte kaydedilir.

İçinde `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

Sayfa modeli hizmetini kullanarak dosyanın içeriğini yükler.

Dizin sayfanın içinde `OnGet` yöntemi (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken sınıfı

Bir veya daha fazla temsil eden `IChangeToken` tek bir nesne örneğini kullanın <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfı.

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

`HasChanged` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `HasChanged` olduğu `true`. `ActiveChangeCallbacks` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `ActiveChangeCallbacks` olduğu `true`. Birden çok eş zamanlı değişikliği olayları meydana gelirse, bileşik bir değişikliği geri çağırma bir kez çağrılır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
