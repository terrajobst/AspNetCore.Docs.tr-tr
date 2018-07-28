---
title: Belleğe yüklenmiş önbellek ASP.NET core'da
author: rick-anderson
description: ASP.NET Core bellekte önbelleğe öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2018
uid: performance/caching/memory
ms.openlocfilehash: b57e29965edc791ad4ecfe1b6b863a4a3dbe3f09
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332307"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Belleğe yüklenmiş önbellek ASP.NET core'da

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), ve [Steve Smith](https://ardalis.com/)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Temel bilgileri önbelleğe alma

Önbelleğe alma işlemi önemli ölçüde performans ve ölçeklenebilirlik, bir uygulamanın içeriği oluşturmak için gereken iş azaltarak artırabilir. Seyrek değişen verilerle iyi önbelleğe alma çalışır. Önbelleğe alma, çok döndürülen verilerin bir kopyasını yapar özgün kaynaktan daha hızlı. Yazın ve hiçbir zaman önbelleğe alınmış veri bağımlı uygulamanızı test etmek.

ASP.NET Core, birkaç farklı önbellek destekler. En basit önbellek dayanır [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), web sunucusu bellekte bir önbellek temsil eder. Bir sunucu grubu birden çok sunucu üzerinde çalışan uygulamalar oturumları bellek içi önbelleği kullanırken Yapışkan emin olmalısınız. Yapışkan oturumlar tüm istemciden gelen sonraki istekleri aynı sunucuya gittiğinden emin olun. Örneğin, Azure Web apps kullanımını [uygulama isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) sonraki isteklere aynı sunucuya yönlendirmek için (ARR).

Bir web grubunda olmayan Yapışkan oturumlar gerektiren bir [dağıtılmış önbellek](distributed.md) önbellek tutarlılığı sorunları önlemek için. Bazı uygulamalar için bir bellek içi önbelleğe göre daha yüksek ölçeği genişletilmiş dağıtılmış önbellek destekleyebilir. Bir paylaşılan önbellek kullanarak, bir dış işlem önbelleği üzerinizden alır.

`IMemoryCache` Sürece, önbelleği önbellek girişlerinin bellek baskısı altında Tahliye [önbelleğe öncelik](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ayarlanır `CacheItemPriority.NeverRemove`. Ayarlayabileceğiniz `CacheItemPriority` önbellek çıkarır bellek baskısı altında öğeleri önceliğini ayarlamak için.

Bellek içi önbellek, herhangi bir nesne kaydedebilir; Dağıtılmış önbellek arabirimi sınırlı olan `byte[]`.

### <a name="cache-guidelines"></a>Önbellek yönergeleri

* Kod, verileri getirmek için bir geri dönüş seçeneği her zaman olmalıdır ve **değil** kullanılabilir olan bir önbelleğe alınan değeri bağlıdır.
* Önbellek bellek scare kaynak kullanır. Önbellek büyüme sınırla:
  * Yapmak **değil** dış girdi önbellek anahtarları kullanın.
  * Süre sonu önbellek büyümesini sınırlamak için kullanın.
  * [SetSize, boyutu ve SizeLimit önbellek boyutunu sınırlamak için kullanın](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a>IMemoryCache kullanma

Bellek içi önbelleğe alma bir *hizmet* , uygulama kullanımından başvurduğu [bağımlılık ekleme](../../fundamentals/dependency-injection.md). Çağrı `AddMemoryCache` içinde `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

İstek `IMemoryCache` oluşturucu örneği:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

Aşağıdaki kod [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) bir süre önbellekte olup olmadığını denetlemek için. Bir süre önbelleğe, yeni bir girdi oluşturulur ve önbelleğe eklenen [ayarlamak](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Geçerli saat ve önbelleğe alınan zaman görüntülenir:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Önbelleğe alınan `DateTime` değeri, zaman aşımı süresi (ve bellek baskısı nedeniyle hiçbir çıkarma) içinde istekler varken önbellekte kalır. Aşağıdaki görüntüde, geçerli saat ve önbellekten daha eski bir zaman gösterilmektedir:

![Dizin görünümünün görüntülenen iki farklı saatler](memory/_static/time.png)

Aşağıdaki kod [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) verileri önbelleğe almak için. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Aşağıdaki kod çağrıları [alma](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) önbelleğe alınmış zaman getirilemedi:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Bkz: [IMemoryCache yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [CacheExtensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) önbellek yöntemleri açıklaması.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Aşağıdaki örnek:

- Mutlak zaman aşımı süresi ayarlar. Bu giriş önbelleğe alınacak en uzun süreyi ve öğe olmaadığını sürekli olarak yenilendiğinde çok eski duruma gelmesini engeller.
- Kayan bir sona erme saati ayarlar. Önbelleğe alınan bu öğenin erişim istekleri kayan sona erme saati sıfırlar.
- Önbellek önceliği ayarlar `CacheItemPriority.NeverRemove`.
- Ayarlar bir [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , çağrılacağı sonra giriş önbellekten çıkarılır. Geri çağırma öğeyi önbellekten kaldırır kodundan farklı bir iş parçacığı üzerinde çalıştırın.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>SetSize, boyutu ve SizeLimit önbellek boyutunu sınırlamak için kullanın

A `MemoryCache` örnek isteğe bağlı olarak belirtin ve boyut sınırını uygular. Önbellek giriş boyutu ölçmek için bir mekanizma bulunmadığından olduğundan bellek boyutu sınırı tanımlı ölçü yok. Önbellek boyutu sınırı ayarlarsanız, tüm girişleri boyutu belirtmeniz gerekir. Belirtilen birim Geliştirici seçer boyutudur.

Örneğin:

* Web uygulaması dizeleri öncelikle önbelleğe almayı her önbellek giriş boyutu dize uzunluğu olabilir.
* Uygulamanın tüm girişleri boyutu 1 belirtebilirsiniz ve boyut sınırını girişleri sayısıdır.

Aşağıdaki kod boyutu sabit bir unitless oluşturur [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) tarafından erişilebilen [bağımlılık ekleme](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) birim yok. Önbelleğe alınan girişlerin boyutu ne olursa olsun birimleri, önbellek boyutunu ayarlarsanız en uygun bulduğunuz de belirtmeniz gerekir. Bir önbellek örneği, tüm kullanıcılar, aynı birim sistemi kullanmanız gerekir. Önbelleğe alınan giriş boyutlarının toplamı tarafından belirtilen değeri aşıyorsa bir giriş önbelleğe alınmamış `SizeLimit`. Önbellek boyut sınırı olarak ayarlanırsa, önbellek boyutu girişinde ayarını yoksayılacak.

Aşağıdaki kod kayıtları `MyMemoryCache` ile [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` Bu boyutu sınırlı önbelleğini farkında olduğundan ve nasıl önbellek giriş boyutu uygun şekilde ayarlanacağını bilmek bileşenleri için bağımsız bir önbellek olarak oluşturulur.

Aşağıdaki kod `MyMemoryCache`:

[! kod-csharp [] (memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Önbellek giriş boyutu ayarlanabilir [boyutu](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) veya [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) genişletme yöntemi:

[! kod-csharp [] (memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2 & Vurgu 9, 10, 14, 15 =) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Önbellek bağımlılıkları

Aşağıdaki örnek, bir önbellek girdisi bağımlı giriş süresi dolarsa sona gösterilmiştir. A `CancellationChangeToken` önbelleğe eklenir. Zaman `Cancel` üzerinde çağrılır `CancellationTokenSource`, her iki önbellek girişlerinin çıkarılacak.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Kullanarak bir `CancellationTokenSource` grup olarak çıkarılacak birden fazla önbellek girişi sağlar. İle `using` önbellek girdisi Yukarıdaki kod modelinde oluşturulan içinde `using` blok, tetikleyiciler ve sona erme ayarları devralacaktır.

## <a name="additional-notes"></a>Ek Notlar

- Bir önbellek öğesi yeniden doldurmak için bir geri çağırma kullanırken:

  - Birden çok istek geri çağırma henüz tamamlanmadığından önbelleğe alınan anahtar değeri boş bulabilirsiniz.
  - Bu, önbelleğe alınan öğeyi yeniden birkaç iş parçacığı neden olabilir.

- Bir önbellek girdisi başka oluşturmak için kullanıldığında, alt üst girişin sona erme belirteçleri ve zamana bağlı süre sonu ayarları kopyalar. Alt tarafından el ile temizleme süresi dolmuş veya üst girişinin güncelleştirme yok.

- Kullanım [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) önbellek girdisi önbellekten çıkarıldığı sonra harekete geçirilmez geri çağırmaları ayarlamak için.

## <a name="additional-resources"></a>Ek kaynaklar

* [Dağıtılmış önbellekle çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri ile değişiklikleri algılama](xref:fundamentals/primitives/change-tokens)
* [Yanıtları önbelleğe alma](xref:performance/caching/response)
* [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)