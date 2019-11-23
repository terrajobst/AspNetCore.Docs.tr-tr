---
title: ASP.NET Core filtreler
author: ardalis
description: Filtrelerin nasıl çalıştığını ve ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 6a83b8e85b68a9b8796aeed2fd39108dbeed3266
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190528"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core filtreler

[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından

ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.

Yerleşik Filtreler şunları gibi görevleri işler:

* Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).
* Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).

Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir. Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.  Filtreler kodu çoğaltmaktan kaçının. Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.

Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.

Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) .

## <a name="how-filters-work"></a>Filtreler nasıl çalışır?

Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.  Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve ASP.NET Core eylemi çağırma Işlem hattı aracılığıyla işlenir. İstek işleme, istemciye gönderilen bir yanıt olmadan önce eylem seçimi, yönlendirme ara yazılımı ve diğer diğer ara yazılım aracılığıyla yeniden devam eder.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtre türleri

Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:

* İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır. Yetkilendirme filtreleri, istek yetkisiz ise işlem hattı kısa devre dışı.

* [Kaynak filtreleri](#resource-filters):

  * Yetkilendirmeden sonra çalıştırın.  
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre işlem hattının geri kalanının önüne kod çalıştırabilir. Örneğin `OnResourceExecuting`, model bağlamadan önce kodu çalıştırabilir.
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırabilir.

* [Eylem filtreleri](#action-filters) , tek bir eylem yöntemi çağrıldıktan hemen önce ve sonra kodu çalıştırabilir. Bir eyleme geçirilen bağımsız değişkenleri ve eylemden döndürülen sonucu işlemek için kullanılabilirler. Eylem filtreleri Razor Pages **desteklenmez.**

* [Özel durum filtreleri](#exception-filters) , yanıt gövdesine hiçbir şey yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygulamak için kullanılır.

* [Sonuç filtreleri](#result-filters) , tek tek eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırabilir. Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır. Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.

Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir. Bu şekilde, istek yalnızca istemciye gönderilen yanıt haline gelmeden önce sonuç filtreleri ve kaynak filtreleri tarafından işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Uygulama

Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.

Zaman uyumlu filtreler, ardışık düzen aşamasından önce (`On-Stage-Executing`) ve sonra kodu çalıştırabilir (`On-Stage-Executed`). Örneğin, `OnActionExecuting` eylem yöntemi çağrılmadan önce çağrılır. `OnActionExecuted`, eylem yöntemi döndüğünde çağrılır.

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

Zaman uyumsuz filtreler bir `On-Stage-ExecutionAsync` yöntemi tanımlar:

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

Yukarıdaki kodda `SampleAsyncActionFilter`, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) sahiptir.  `On-Stage-ExecutionAsync` yöntemlerinin her biri, filtrenin ardışık düzen aşamasını yürüten bir `FilterType-ExecutionDelegate` alır.

### <a name="multiple-filter-stages"></a>Birden çok filtre aşaması

Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir. Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerini uygular.

Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.** Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır. Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır. Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır. <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> gibi soyut sınıflar kullanılırken, yalnızca zaman uyumlu yöntemleri veya her bir filtre türü için zaman uyumsuz yöntemi geçersiz kılın.

### <a name="built-in-filter-attributes"></a>Yerleşik filtre öznitelikleri

ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir. Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir. `AddHeaderAttribute` bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.

Filtre öznitelikleri:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a>Kapsamları ve yürütme sırasını filtrele

Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:

* Bir eylem üzerinde bir öznitelik kullanma.
* Bir denetleyicide bir özniteliği kullanma.
* Aşağıdaki kodda gösterildiği gibi tüm denetleyiciler ve eylemler için genel olarak:

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

Yukarıdaki kod, [Mvcoptions. Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonunu kullanarak genel olarak üç filtre ekler.

### <a name="default-order-of-execution"></a>Varsayılan yürütme sırası

İşlem hattının belirli bir aşamasına ilişkin birden çok filtre olduğunda, kapsam varsayılan filtre yürütme sırasını belirler.  Genel filtreler kapsayan sınıf filtreleri, bu da kapsayan Yöntem filtreleri.

Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır. Filtre sırası:

* Genel filtrelerin *önceki* kodu.
  * Denetleyici filtrelerinden *önceki* kod.
    * Eylem yöntemi filtrelerinden *önceki* kod.
    * Eylem yöntemi filtrelerinden *sonraki* kod.
  * Denetleyici filtrelerinden *sonraki* kod.
* Genel filtrelerin *sonraki* kodu.
  
Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.

| Sequence | Filtre kapsamı | Filter yöntemi |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Denetleyici | `OnActionExecuting` |
| 3 | Yöntem | `OnActionExecuting` |
| 4 | Yöntem | `OnActionExecuted` |
| 5 | Denetleyici | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Bu sıra şunları gösterir:

* Yöntem filtresi, denetleyici filtresi içinde iç içe geçmiş.
* Denetleyici filtresi, genel filtrenin içinde iç içe geçmiş.

### <a name="controller-and-razor-page-level-filters"></a>Denetleyici ve Razor sayfa düzeyi filtreleri

<xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` yöntemlerini içerir. Bu Yöntemler:

* Belirli bir eylem için çalışan filtreleri sarın.
* `OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.
* `OnActionExecuted` tüm eylem filtrelerinden sonra çağrılır.
* `OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır. `next` sonra filtre içindeki kod eylem yönteminden sonra çalışır.

Örneğin, indirme örneğinde `MySampleActionFilter`, başlangıçta genel olarak uygulanır.

`TestController`:

* `FilterTest2` eyleme `SampleActionFilterAttribute` (`[SampleActionFilter]`) uygular.
* `OnActionExecuting` ve `OnActionExecuted`geçersiz kılar.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

`https://localhost:5001/Test/FilterTest2` gitme aşağıdaki kodu çalıştırır:

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).

### <a name="overriding-the-default-order"></a>Varsayılan sırayı geçersiz kılma

Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>uygulayarak geçersiz kılınabilir. `IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar. Daha düşük bir `Order` değerine sahip bir filtre:

* Daha yüksek bir `Order`bir filtrenin *önüne kodundan önce* kodu çalıştırır.
* Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.

`Order` özelliği bir Oluşturucu parametresiyle ayarlanabilir:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Yukarıdaki örnekte gösterilen 3 eylem filtresini göz önünde bulundurun. Denetleyicinin ve genel filtrelerin `Order` özelliği sırasıyla 1 ve 2 ' ye ayarlandıysa, yürütme sırası tersine çevrilir.

| Sequence | Filtre kapsamı | `Order` özelliği | Filter yöntemi |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Yöntem | 0 | `OnActionExecuting` |
| 2 | Denetleyici | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Denetleyici | 1  | `OnActionExecuted` |
| 6 | Yöntem | 0  | `OnActionExecuted` |

`Order` özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar. Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır. Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar. Yerleşik filtreler için, `Order` sıfır olmayan bir değere ayarlanmadığı takdirde kapsam sıralamayı belirler.

## <a name="cancellation-and-short-circuiting"></a>İptal ve kısa devre dışı

Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresindeki <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir. Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` Action metodunu hedefleyin. `ShortCircuitingResourceFilter`:

* Önce bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.
* Kısa süreli işlem hattının geri kalanı.

Bu nedenle `AddHeader` filtresi `SomeResource` eylemi için hiçbir şekilde çalışmamayacaktır. Her iki filtre de eylem yöntemi düzeyinde uygulanırsa, `ShortCircuitingResourceFilter` ilk kez çalıştırıldıysa bu davranış aynı olur. `ShortCircuitingResourceFilter`, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Bağımlılık ekleme

Filtreler türe veya örneğe göre eklenebilir. Bir örnek eklenirse, bu örnek her istek için kullanılır. Bir tür eklenirse, türü etkinleştirilmiş olur. Tür etkinleştirilmiş bir filtre şu anlama gelir:

* Her istek için bir örnek oluşturulur.
* Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.

Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz. Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:

* Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır. 
* Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.

Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğe uygulandı.

Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:

Günlükçüler, DI 'den alınabilir. Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının. [Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar. Filtrelere günlük eklendi:

* , İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.
* Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** . Yerleşik filtreler günlük eylemleri ve çerçeve olayları.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Hizmet filtresi uygulama türleri `ConfigureServices`kaydedilir. <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, bir filtrenin bir örneğini dı öğesinden alır.

Aşağıdaki kod `AddHeaderResultServiceFilter`gösterir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Aşağıdaki kodda, `AddHeaderResultServiceFilter` dı kapsayıcısına eklenir:

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

Aşağıdaki kodda, `ServiceFilter` özniteliği dı öğesinden `AddHeaderResultServiceFilter` filtrenin bir örneğini alır:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

`ServiceFilterAttribute`kullanırken, [Servicefilterattribute. ısyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:

* Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar. ASP.NET Core çalışma zamanı garanti etmez:

  * Filtrenin tek bir örneğinin oluşturulması gerekir.
  * Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.

* Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.

 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular. `IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar. `CreateInstance`, belirtilen türü DI 'dan yükler.

### <a name="typefilterattribute"></a>TypeFilterAttribute

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez. <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>kullanarak türü başlatır.

`TypeFilterAttribute` türler doğrudan dı kapsayıcısından çözümlenmediğinden:

* `TypeFilterAttribute` kullanılarak başvurulan türlerin dı kapsayıcısına kaydedilmesi gerekmez.  Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.
* `TypeFilterAttribute`, isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.

`TypeFilterAttribute`kullanırken [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:
* Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar. ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.

* Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.

Aşağıdaki örnek `TypeFilterAttribute`kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a>Yetkilendirme filtreleri

Yetkilendirme filtreleri:

* , Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.
* Eylem yöntemlerine erişimi denetleyin.
* Bir Before yöntemi, ancak After yönteminden hiçbiri.

Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir. Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin. Yerleşik yetkilendirme filtresi:

* Yetkilendirme sistemini çağırır.
* İstekleri yetkilendirmez.

Yetkilendirme filtreleri içinde özel **durumlar atamayın:**

* Özel durum işlenmeyecektir.
* Özel durum filtreleri özel durumu işleymeyecektir.

Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.

[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.

## <a name="resource-filters"></a>Kaynak filtreleri

Kaynak filtreleri:

* <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.
* Yürütme, filtre işlem hattının çoğunu sarmalar.
* Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.

Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır. Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.

Kaynak filtresi örnekleri:

* Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .
* [Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

  * Model bağlamanın form verilerine erişimini engeller.
  * Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.

## <a name="action-filters"></a>Eylem filtreleri

> [!IMPORTANT]
> Eylem filtreleri Razor Pages **için uygulanmaz.** Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> destekler. Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).

Eylem filtreleri:

* <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.
* Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.

Aşağıdaki kod bir örnek eylem filtresi gösterir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> aşağıdaki özellikleri sağlar:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-bir eylem yöntemine yönelik girişlerin okunmalarını sağlar.
* <xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.
* <xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.

Eylem yönteminde özel durum oluşturma:

* Sonraki filtrelerin çalıştırılmasını önler.
* `Result`ayarının aksine, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> `Controller` ve `Result` ek olarak aşağıdaki özellikleri sağlar:

* <xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.
* <xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan eylem filtresi bir özel durum oluşturdu, null değil. Bu özellik null olarak ayarlanıyor:

  * Özel durumu etkin bir şekilde işler.
  * `Result`, eylem yönteminden döndürülmüş gibi yürütülür.

Bir `IAsyncActionFilter`için <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>çağrısı:

* Sonraki eylem filtrelerini ve eylem yöntemini yürütür.
* `ActionExecutedContext`döndürür.

Kısa devre dışı, bir sonuç örneğine <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> atayın ve `next` çağırmayın (`ActionExecutionDelegate`).

Framework, alt sınıflı olabilecek bir soyut <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sağlar.

`OnActionExecuting` eylem filtresi şu şekilde kullanılabilir:

* Model durumunu doğrulayın.
* Durum geçersizse bir hata döndürür.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

`OnActionExecuted` yöntemi eylem yönteminden sonra çalışır:

* Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.
* eylem yürütmesi başka bir filtre tarafından kabul edilse, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> true olarak ayarlanır.
* eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır. `Exception` null olarak ayarlanıyor:

  * Bir özel durumu etkin bir şekilde işler.
  * `ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri:

* <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>uygulayın. 
* , Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.

Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

Özel durum filtreleri:

* Etkinlikden önceki ve sonraki olaylar yok.
* <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>uygulayın.
* Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.
* Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .

Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya bir yanıt yazın. Bu, özel durumun yayılmasını engeller. Özel durum filtresi bir özel durumu "başarılı" olarak açamaz. Yalnızca bir eylem filtresi bunu yapabilir.

Özel durum filtreleri:

* Eylemler içinde oluşan özel durumları yakalamaya uygundur.
* , Hata işleme ara yazılımı olarak esnek değildir.

Özel durum işleme için ara yazılımı tercih edin. Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın. Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir. API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.

## <a name="result-filters"></a>Sonuç filtreleri

Sonuç filtreleri:

* Arabirim uygulama:
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter>
* Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter ve ıasyncresultfilter

Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Yürütülen sonuç türü eyleme göre değişir. Bir görünüm döndüren bir eylem, yürütülen <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir parçası olarak tüm Razor işlemlerini içerir. Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir. [Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.

Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür. Şu durumlarda sonuç filtreleri yürütülmez:

* Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.
* Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.

<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> yöntemi, eylem sonucunun ve sonraki sonuç filtrelerinin `true`<xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ayarlanarak kısa devre yürütülmesine neden olabilir. Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın. `IResultFilter.OnResultExecuting` bir özel durum oluşturma şu şekilde yapılır:

* Eylem sonucunun ve sonraki filtrelerin yürütülmesini önleyin.
* Başarılı bir sonuç yerine hata olarak kabul edilir.

<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalıştırıldığında, yanıt istemciye zaten gönderilir. Yanıt istemciye zaten gönderildiyse, daha fazla değiştirilemez.

`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre ise `true` olarak ayarlanır.

eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi, `ResultExecutedContext.Exception` null olmayan bir değere ayarlanır. `Exception` null olarak ayarlanması, bir özel durumu etkili bir şekilde işler ve özel durumun, ardışık düzendeki ASP.NET Core daha sonra yeniden oluşturulmasını önler. Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur. Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.

Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> bir `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür. Kısa devre dışı, [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` ve `ResultExecutionDelegate`çağırmayın:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

Framework, alt sınıflı olabilecek bir soyut `ResultFilterAttribute` sağlar. Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri, tüm eylem sonuçları için çalışan bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> uygulamasını bildirir. Bu, tarafından oluşturulan eylem sonuçlarını içerir:

* Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.
* Özel durum filtreleri.

Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a>IFilterFactory

<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>uygular. Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` bir örnek olarak kullanılabilir. Çalışma zamanı filtreyi çağırmayı hazırlarken, bir `IFilterFactory`dönüştürmeyi dener. Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır. Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.

`IFilterFactory`, filtre oluşturmaya yönelik başka bir yaklaşım olarak özel öznitelik uygulamaları kullanılarak uygulanabilir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

Önceki kod, [indirme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)çalıştırılarak test edilebilir:

* F12 geliştirici araçlarını çağırın.
* [https://test-cors.org](`https://localhost:5001/Sample/HeaderWithFactory`) sayfasına gidin.

F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:

* **Yazar:** `Joe Smith`
* **globaladdheader:** `Result filter added to MvcOptions.Filters`
* **iç:** `My header`

Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.

### <a name="ifilterfactory-implemented-on-an-attribute"></a>Öznitelik üzerinde IFilterFactory uygulandı

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

`IFilterFactory` uygulayan filtreler şu filtreler için yararlıdır:

* Parametre geçirme gerekmez.
* DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular. `IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar. `CreateInstance`, belirtilen türü hizmetler kapsayıcısından (DI) yükler.

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Aşağıdaki kodda `[SampleActionFilter]`uygulamak için üç yaklaşım gösterilmektedir:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

Yukarıdaki kodda, yöntemi `[SampleActionFilter]` olarak dekorasyon, `SampleActionFilter`uygulamak için tercih edilen yaklaşımdır.

## <a name="using-middleware-in-the-filter-pipeline"></a>Filtre ardışık düzeninde ara yazılım kullanma

Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır. Ancak filtreler, ASP.NET Core çalışma zamanının bir parçası olan ara yazılımlar dışında farklılık gösterir, bu da ASP.NET Core bağlamına ve yapılarına erişime sahip oldukları anlamına gelir.

Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemi ile bir tür oluşturun. Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.

## <a name="next-actions"></a>Sonraki eylemler

* [Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.
* Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
