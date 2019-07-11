---
title: ASP.NET core'da filtreleri
author: ardalis
description: Filtreleri nasıl çalıştığını ve ASP.NET Core nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: df6f144f23f36d8009a5638859846e3cfb768b37
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815441"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET core'da filtreleri

Tarafından [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), ve [Steve Smith](https://ardalis.com/)

*Filtreler* önce veya sonra istek işleme ardışık düzeninde belirli aşamalara çalıştırmak için kod içinde ASP.NET Core izin verin.

Yerleşik filtreler gibi görevleri işler:

* Yetkilendirme (bir kullanıcı için yetkili olmayan kaynaklara erişimini engelleme).
* Yanıt önbelleğe alma (önbelleğe alınan yanıt döndürmek için istek ardışık düzenini kısa devre).

Özel Filtreler, geniş kapsamlı kritik konular işlemek için oluşturulabilir. Hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme geniş kapsamlı kritik konular örnekleridir.  Filtreler, kod yinelemekten kaçının. Örneğin, bir hata özel durum filtresi işleme hata işleme birleştirebilir.

Bu belge, Razor sayfaları, API denetleyicileri ve görünümleri denetleyicileriyle geçerlidir.

[Görüntüleme veya indirme örnek](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

## <a name="how-filters-work"></a>Filtreler nasıl çalışır

Filtre çalıştırma içinde *ASP.NET Core eylem çağrısı işlem hattı*bazen denilen *filtre ardışık düzen*.  ASP.NET Core yürütülecek eylemi seçtikten sonra filtre ardışık düzen çalıştırır.

![İstek, diğer bir ara yazılım, yönlendirme ara yazılım, eylem seçimi ve ASP.NET Core eylem çağrısı işlem hattı işlenir. İstek işleme geri eylem seçimi, yönlendirme ara yazılım ve diğer çeşitli ara yazılımı istemciye gönderilen bir yanıt olmadan önce devam eder.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtre türleri

Her filtre türü, farklı bir filtre ardışık düzen aşamasında yürütülen:

* [Yetkilendirme filtreleri](#authorization-filters) ilk önce çalışır, kullanıcının istek için yetki verilip verilmediğini belirlemek için kullanılır. İstek yetkisiz ise yetkilendirme filtrelerini ardışık kısa devre oluşturur.

* [Kaynak filtreleri](#resource-filters):

  * Yetkilendirmeden sonra çalıştırın.  
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> Kalan filtre ardışık düzenini önce kod çalıştırabilir. Örneğin, `OnResourceExecuting` kod model bağlama önce çalıştırabilirsiniz.
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> Kalan ardışık düzenini tamamlandıktan sonra kodu çalıştırabilirsiniz.

* [Eylem filtreleri](#action-filters) kod hemen önce ve tek bir eylem yöntemi çağrıldıktan sonra çalıştırabilirsiniz. Bir eyleme geçirilen bağımsız değişkenleri ve döndürülen eylem sonucu işlemek için kullanılabilir. Eylem filtreleri **değil** Razor sayfaları desteklenir.

* [Özel durum filtreleri](#exception-filters) yanıt gövdesi için herhangi bir şey yazılmadan önce gerçekleşen işlenmeyen özel durumlar genel ilkeleri uygulamak için kullanılır.

* [Sonuç filtreleri](#result-filters) kod hemen önce ve sonra tek tek eylem sonuçlarını yürütülmesini çalıştırabilirsiniz. Yalnızca eylem yöntemi başarıyla yürütüldü, bunlar çalıştırın. Bunlar, görünümü veya biçimlendirici yürütme sarmanız gerekir mantığı için kullanışlıdır.

Aşağıdaki diyagramda, filtre türleri filtre işlem hattında nasıl etkileşim kurduğu gösterilmektedir.

![İstek, yetkilendirme filtreleri, kaynak filtreleri, Model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütme işlenir. Biçimi üzerinde istek yalnızca sonuç filtreleri ve kaynak filtreler tarafından istemciye gönderilen bir yanıt olmadan önce işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Uygulama

Filtreleri farklı arabirimi tanımları aracılığıyla zaman uyumlu ve uyumsuz uygulamaları destekler.

Zaman uyumlu filtreleri önce kodu çalıştırabilirsiniz (`On-Stage-Executing`) ve sonra (`On-Stage-Executed`), ardışık düzen aşaması. Örneğin, `OnActionExecuting` eylem yönteminden önce çağrılır. `OnActionExecuted` Eylem yöntemi döndükten sonra çağrılır.

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

Zaman uyumsuz filtre tanımlayan bir `On-Stage-ExecutionAsync` yöntemi:

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

Önceki kodda, `SampleAsyncActionFilter` sahip bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), eylem yöntemini yürütür.  Her biri `On-Stage-ExecutionAsync` yöntemleri alır bir `FilterType-ExecutionDelegate` , filtre ardışık düzen aşamasını yürütür.

### <a name="multiple-filter-stages"></a>Birden çok filtre aşamaları

Tek bir sınıfta birden çok filtre aşamalar için arabirimleri uygulanabilir. Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfının Implements `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerine.

Uygulama **ya da** zaman uyumlu veya zaman uyumsuz bir filtre arabirimi sürümü **değil** hem de. Çalışma zamanı ilk filtre zaman uyumsuz arabirimini uygulayan ve çağrı yaptığı bu durumda olup olmadığını denetler. Aksi durumda, zaman uyumlu arabirim yöntemleri çağırır. Bir sınıf içinde zaman uyumsuz ve zaman uyumlu arabirimleri olmasa da, yalnızca zaman uyumsuz yöntem çağrılır. Soyut sınıflar gibi kullanırken <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> yalnızca zaman uyumlu metotları veya zaman uyumsuz yöntem her filtre türü için geçersiz kılın.

### <a name="built-in-filter-attributes"></a>Yerleşik filtre öznitelikleri

ASP.NET Core, Sınıflandırma ve özelleştirilebilir öznitelik tabanlı yerleşik filtreleri içerir. Örneğin, aşağıdaki sonucu filtre üstbilgi yanıta ekler:

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

Öznitelikleri, yukarıdaki örnekte gösterildiği gibi bağımsız değişken kabul etmek filtreler sağlar. Uygulama `AddHeaderAttribute` bir denetleyici veya eylem yöntemi için adı ve HTTP üstbilgisinin değerini belirtin:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

Filtre arabirimlerinin birkaç özel uygulamalar için temel sınıf olarak kullanılabilecek ilgili özniteliklere sahiptir.

Filtre öznitelikleri:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a>Filtre kapsamları ve Yürütme sırası

Bir filtre işlem hattının üç birinde eklenebilir *kapsamları*:

* Öznitelik, bir eylem kullanarak.
* Öznitelik, bir denetleyicisinde kullanma.
* Genel olarak tüm denetleyicileri ve aşağıdaki kodda gösterildiği gibi eylemleri için:

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

Yukarıdaki kod genel kullanarak üç filtreler ekler [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonu.

### <a name="default-order-of-execution"></a>Varsayılan yürütme sırası

Belirli bir ardışık düzen aşaması için birden çok filtre olduğunda, filtre yürütme varsayılan sıra kapsamı belirler.  Genel filtrelerin sırayla yöntemi filtreleri çevreleyen sınıf filtreleri alın.

Filtre iç içe geçme sonucunda *sonra* kodu filtre ters sırada çalışır *önce* kod. Filtre sırası:

* *Önce* kod genel filtre.
  * *Önce* denetleyici filtreleri kodu.
    * *Önce* kod eylem yöntemi filtreler.
    * *Sonra* kod eylem yöntemi filtreler.
  * *Sonra* denetleyici filtreleri kodu.
* *Sonra* kod genel filtre.
  
Aşağıdaki örnek filtre yöntemleri zaman uyumlu eylem filtreleri için çağrılma sırasını göstermektedir.

| Sequence | Filtre kapsamı | Filter yöntemi |
|:--------:|:------------:|:-------------:|
| 1\. | Global | `OnActionExecuting` |
| 2 | Denetleyici | `OnActionExecuting` |
| 3 | Yöntem | `OnActionExecuting` |
| 4 | Yöntem | `OnActionExecuted` |
| 5 | Denetleyici | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Bu dizisini gösterir:

* Yöntem filtresi denetleyici filtresi içinde yer alıyor.
* Denetleyici filtreyi genel filtre içinde yer alıyor.

### <a name="controller-and-razor-page-level-filters"></a>Denetleyici ve Razor sayfa düzeyi filtreleri

Devralınan denetleyicilerin <xref:Microsoft.AspNetCore.Mvc.Controller> taban sınıfı içeren [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), ve [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*) 
 `OnActionExecuted` yöntemleri. Bu yöntemler:

* Çalıştıran belirli bir eylem filtrelerini kaydır.
* `OnActionExecuting` herhangi bir eylem filtreleri önce çağrılır.
* `OnActionExecuted` Tüm eylem filtrelerini sonra çağrılır.
* `OnActionExecutionAsync` herhangi bir eylem filtreleri önce çağrılır. Kod sonra filtre `next` sonra eylem yönteminin çalıştırır.

Örneğin, yükleme örnekteki `MySampleActionFilter` başlangıç genel olarak uygulanır.

`TestController`:

* Geçerli `SampleActionFilterAttribute` (`[SampleActionFilter]`) için `FilterTest2` eylem:
* Geçersiz kılmalar `OnActionExecuting` ve `OnActionExecuted`.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

Gezinme `https://localhost:5001/Test/FilterTest2` aşağıdaki kodu çalıştırır:

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

Razor sayfaları için bkz: [Filtreleri Uygula Razor sayfası filtre yöntemi geçersiz kılarak](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).

### <a name="overriding-the-default-order"></a>Varsayılan sıra geçersiz kılma

Varsayılan sıralı yürütme uygulama tarafından geçersiz kılınabilir <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>. `IOrderedFilter` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> yürütme sırasını belirlemek için kapsam üzerinde önceliklidir özelliği. Daha düşük bir filtre `Order` değeri:

* Çalıştırmaları *önce* kodu daha yüksek bir değere sahip bir filtre, önce `Order`.
* Çalıştırmaları *sonra* kodun hemen ardından, daha yüksek bir filtre `Order` değeri.

`Order` Özelliği ile bir oluşturucu parametresi ayarlanabilir:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Önceki örnekte gösterilen aynı 3 eylem filtreleri göz önünde bulundurun. Varsa `Order` denetleyici ve genel filtre özelliği ayarlandığında 1 ve 2'ye sırasıyla, yürütme sıra ters çevrilir.

| Sequence | Filtre kapsamı | `Order` Özelliği | Filter yöntemi |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1\. | Yöntem | 0 | `OnActionExecuting` |
| 2 | Denetleyici | 1\.  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Denetleyici | 1\.  | `OnActionExecuted` |
| 6 | Yöntem | 0  | `OnActionExecuted` |

`Order` Özelliğini çalıştırma hangi filtreleri sırayla belirlerken kapsamı geçersiz kılar. Filtreleri ilk sıraya göre sıralanır ve kapsam TIES ayırmak için kullanılır. Tüm yerleşik filtreleri uygulamak `IOrderedFilter` ve varsayılan `Order` 0 değeri. Yerleşik filtreleri için sürece kapsam sırasını belirleyen `Order` sıfır olmayan bir değere ayarlanır.

## <a name="cancellation-and-short-circuiting"></a>İptal ve kısa devre

Filtre ardışık düzen ayarlayarak short-circuited <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> filter yöntemi için sağlanan parametre. Örneğin, aşağıdaki kaynak filtre rest işlem hattının yürütülmesini engeller:

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` ve `AddHeader` filtre hedef `SomeResource` eylem yöntemi. `ShortCircuitingResourceFilter`:

* Kaynak Filtresi olduğu için ilk olarak çalışır ve `AddHeader` bir eylem filtresi.
* Kalan ardışık düzenini short-circuits.

Bu nedenle `AddHeader` filtresi hiç çalıştığı için `SomeResource` eylem. Belirtilen eylem yöntemini düzeyinde hem de bir filtre uygulansaydı, bu davranışı aynı olacaktır `ShortCircuitingResourceFilter` ilk çalıştı. `ShortCircuitingResourceFilter` Filtre türü nedeniyle ya da açık kullanılarak ilk çalıştıran `Order` özelliği.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Bağımlılık ekleme

Filtre türü veya örnek tarafından eklenebilir. Bir örneği eklenirse, bu örneği her istek için kullanılır. Bir tür eklenirse, türü etkinleştirildi. Türü tarafından etkinleştirilen bir filtre anlamına gelir:

* Her istek için bir örneği oluşturulur.
* Oluşturucu herhangi bir bağımlılığın tarafından doldurulur [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı).

Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler tarafından sağlanan Oluşturucusu bağımlılıkları olamaz [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı). Oluşturucu bağımlılıkları olduğundan DI tarafından sağlanamaz:

* Öznitelik oluşturucu parametrelerinin nereye uygulanan sağlanan olmalıdır. 
* Bu öznitelikler nasıl işe sınırlamasıdır.

Aşağıdaki filtreler DI sağlanan Oluşturucu bağımlılıkları destekler:

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> öznitelikteki uygulanır.

Bir denetleyici veya eylem yöntemi için yukarıdaki filtreleri uygulanabilir:

Günlükçüleri DI ' kullanılabilir. Bununla birlikte, oluşturma ve filtreler kullanılarak yalnızca günlüğe kaydetme amacıyla kaçının. [Yerleşik framework günlük kaydını](xref:fundamentals/logging/index) günlüğü için ihtiyacınız olan şey genellikle sağlar. Günlüğe kaydetme filtreleri eklendi:

* İşletme etki alanı sorunlarını veya filtre belirli davranış odaklanmanız gerekir.
* Gereken **değil** eylemler veya diğer framework olayları oturum. Yerleşik filtreleri, eylemleri ve framework olayları günlüğe.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Hizmet uygulaması türlerini filtreleme kaydedilen `ConfigureServices`. A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> DI filtre örneğini alır.

Aşağıdaki kodda gösterildiği `AddHeaderResultServiceFilter`:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Aşağıdaki kodda, `AddHeaderResultServiceFilter` DI kapsayıcıya eklenir:

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

Aşağıdaki kodda, `ServiceFilter` özniteliği bir örneğini alır `AddHeaderResultServiceFilter` filtresinden dı:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):

* Bir ipucu sağlayan filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir. ASP.NET Core çalışma zamanı garantisi vermez:

  * Filtre tek bir örneğini oluşturulmasına.
  * Filtre, belirli bir sonraki noktada DI kapsayıcısından yeniden istenen olmayacaktır.

* Singleton dışında bir yaşam süresi hizmetleriyle bağımlı bir filtre ile kullanılmamalıdır.

 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>. `IFilterFactory` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği. `CreateInstance` Belirtilen tür DI yükler.

### <a name="typefilterattribute"></a>TypeFilterAttribute

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> benzer <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ancak türü doğrudan DI kapsayıcısından çözülmüş değildir. Kullanarak türün örneğini oluşturduğunda <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.

Çünkü `TypeFilterAttribute` türleri olmayan çözümlenen doğrudan DI kapsayıcısından:

* Kullanılarak başvurulan türleri `TypeFilterAttribute` DI kapsayıcı ile kayıtlı olması gerekmez.  DI kapsayıcı tarafından yerine bunların bağımlılıklarını sahiptirler.
* `TypeFilterAttribute` İsteğe bağlı olarak tür için oluşturucu bağımsız değişkenleri kabul edebilir.

Kullanırken `TypeFilterAttribute`ayarını `IsReusable` bir ipucudur, filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir. ASP.NET Core çalışma zamanı filtre tek bir örneği oluşturulur tutarlılık garantisi sağlar. `IsReusable` Singleton dışında bir yaşam süresi hizmetleriyle bağımlı bir filtre ile kullanılmamalıdır.

Aşağıdaki örneği kullanarak bir tür bağımsız değişkenleri geçirmek gösterilmektedir `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a>Yetkilendirme filtreleri

Yetkilendirme filtreleri:

* İlk filtreler, filtre ardışık çalıştırılır.
* Eylem yöntemlerine erişimi denetler.
* Sahip bir yöntemi önce ancak bundan sonra yöntemi.

Özel yetkilendirme filtrelerini özel yetkilendirme framework gerektirir. Yetkilendirme ilkeleri yapılandırarak veya özel yetkilendirme ilkesi özel filtre yazma üzerine yazma seçeneğini tercih eder. Yerleşik yetkilendirme Filtresi:

* Yetkilendirme sistem çağırır.
* İstekleri yetkisi yok.

Yapmak **değil** yetkilendirme filtrelerini içinde özel durumlar:

* Özel durumun işlenip değil.
* Özel durum filtreleri özel durum işleme yok.

Bir yetkilendirme filtresi bir özel durum oluştuğunda bir sınama vermeyi düşünün.

Daha fazla bilgi edinin [yetkilendirme](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Kaynak filtreleri

Kaynak filtreleri:

* Ya da uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimi.
* Yürütme, filtre ardışık düzen çoğunu sarmalar.
* Yalnızca [yetkilendirme filtrelerini](#authorization-filters) kaynak filtreleri önce çalışır.

Kaynak filtreleri çoğu işlem hattı iki şekilde yararlıdır. Örneğin, bir önbelleğe alma filtre isabetli önbellek okuması işlem hattını geri kalanını önleyebilirsiniz.

Kaynak Filtresi örnekleri:

* [Short-circuiting kaynak filtresi](#short-circuiting-resource-filter) daha önce gösterilen.
* [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

  * Model bağlama form verilerini erişmesini engeller.
  * Form verileri belleğe okuma önlemek için büyük dosya yüklemeleri için kullanılır.

## <a name="action-filters"></a>Eylem filtreleri

> [!IMPORTANT]
> Eylem filtreleri yapmak **değil** Razor sayfaları için geçerlidir. Razor sayfaları destekler <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> . Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).

Eylem filtreleri:

* Ya da uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimi.
* Eylem yöntemleri yürütülmesini yürütülmesi çevreler.

Aşağıdaki kod örneği eylem filtresi gösterir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Aşağıdaki özellikleri sağlar:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> -bir eylem yöntemine girişleri etkinleştirir okuyun.
* <xref:Microsoft.AspNetCore.Mvc.Controller> -denetleyici örneği düzenleme sağlar.
* <xref:System.Web.Mvc.ActionExecutingContext.Result> -ayarlama `Result` sonraki eylem filtreleri eylem yöntemi ve yürütme short-circuits.

Bir eylem yöntemi bir özel durum:

* Sonraki filtrelerin çalışmasını engeller.
* Ayar aksine `Result`, başarılı sonuç yerine bir hata olarak kabul edilir.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Sağlar `Controller` ve `Result` ayrıca aşağıdaki özellikleri:

* <xref:System.Web.Mvc.ActionExecutedContext.Canceled> -Eylem yürütme başka bir filtre tarafından kısa devre yapılma gerekiyorsa true.
* <xref:System.Web.Mvc.ActionExecutedContext.Exception> Eylem ya da daha önce çalışan eylem filtresi bir özel durum oluşturduysa null olmayan. Bu özelliği null olarak ayarlama:

  * Etkili bir şekilde özel durumu işler.
  * `Result` Bu eylem yönteminden döndürülen yokmuş gibi yürütülür.

İçin bir `IAsyncActionFilter`, çağrı <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:

* Herhangi bir sonraki eylem filtrelerini ve eylem yöntemini yürütür.
* Döndürür `ActionExecutedContext`.

Kısa devre oluşturur, Ata <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> bir sonuç örneği ve Remove() çağırmayın `next` ( `ActionExecutionDelegate`).

Bir soyut bir çerçeve sağlar <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> , sınıflandırma.

`OnActionExecuting` Eylem filtresi için kullanılabilir:

* Model durumu doğrulayın.
* Durumu geçersiz bir hata döndürür.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

`OnActionExecuted` Yöntemi çalıştıktan sonra eylem yöntemi:

* Ve görebilir ve eylemin sonuçlarını işlemek <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği.
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> Eylem yürütme başka bir filtre tarafından kısa devre yapılma gerekiyorsa true olarak ayarlanır.
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayar `Exception` null:

  * Bir özel durum etkili bir şekilde işler.
  * `ActionExecutedContext.Result` Bu normalde eylem yönteminden döndürülen yokmuş gibi yürütülür.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri:

* Uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>. 
* Genel hata işleme ilkeleri uygulamak için kullanılabilir.

Aşağıdaki örnek özel durum filtresi, uygulama geliştirme olduğunda oluşan özel durumları hakkında ayrıntıları görüntülemek için bir özel hata görünümü kullanır:

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

Özel durum filtreleri:

* Önceki ve sonraki olaylar yok.
* Uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.
* Razor sayfası veya denetleyici oluşturma, meydana gelen işlenmeyen özel durumları işlemek [model bağlama](xref:mvc/models/model-binding), eylem filtreleri ya da eylem yöntemleri.
* Yapmak **değil** kaynak filtreleri, sonuç filtrelerini veya MVC sonucu yürütme oluşan özel durumları yakalama.

Bir özel durumu işlemek üzere ayarlanmış <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya yanıt yazın. Bu özel durumun yayılmasını durdurur. Özel Durum Filtresi, bir özel durum "başarılı" içine kapatamazsınız. Yalnızca bir eyleme eylem filtresi, bunu yapabilirsiniz.

Özel durum filtreleri:

* Eylemler içinde gerçekleşen Yakalama özel durumlar için uygundur.
* Ara yazılım işleme hata olarak kadar esnek değildir.

Özel durum işleme için bir ara yazılım tercih eder. Kullanım özel durum filtreleri yalnızca nereye hata işleme *farklı* göre hangi eylem yöntemi çağrılır. Örneğin, bir uygulama, görünümler/HTML ve her iki API uç noktaları için eylem yöntemleri olabilir. API uç noktaları, HTML olarak bir hata sayfası görüntüleme tabanlı eylemleri döndürebilir sırasında hata bilgisi, JSON olarak döndürebilir.

## <a name="result-filters"></a>Sonuç filtreleri

Sonuç filtreleri:

* Bir arabirim uygular:
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter>
* Yürütülmesi, eylem sonuçlarını yürütülmesini çevreler.

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter ve IAsyncResultFilter

Aşağıdaki kod, bir HTTP üstbilgisi ekler bir sonuç filtresi gösterir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Yürütülmekte olan sonuç türünü eylemini bağlıdır. Bir parçası olarak işleme tüm razor görünüm döndüren bir eylem verilebilir <xref:Microsoft.AspNetCore.Mvc.ViewResult> yürütülmekte. Bir API yöntemi, bazı serileştirme yürütme sonucu bir parçası olarak gerçekleştirebilir. Daha fazla bilgi edinin [eylem sonuçları](xref:mvc/controllers/actions)

Eylem veya eylem filtreleri eylem sonucu üretir, sonuç filtreleri için başarılı sonuçları - yalnızca çalıştırılır. Özel durum filtreleri, bir özel durumu işlediğinizde sonuç filtrelerini yürütülmedi.

<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> Yöntemi iki yürütme sonraki sonuç filtreleri ve eylem sonucu ayarlayarak <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> için `true`. Yanıt nesnesi için boş bir yanıt oluşturmaktan kaçınmak için kısa devre olduğunda yazın. Bir özel durum `IResultFilter.OnResultExecuting` olur:

* Sonraki filtrelerin ve eylem sonucu yürütülmesini engeller.
* Başarılı sonuç yerine bir hata olarak kabul edilecek.

Zaman <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalışır:

* Yanıtı istemciye büyük olasılıkla gönderildi ve değiştirilemez.
* Bir özel durum oluştu, yanıt gövdesi gönderilmez.

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

`ResultExecutedContext.Canceled` ayarlanır `true` yürütülen eylem sonucu başka bir filtre tarafından kısa devre yapılma durumunda.

`ResultExecutedContext.Exception` Eylem sonucu veya bir sonraki sonuç filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayar `Exception` için null etkili bir şekilde bir özel durum işleme ve daha sonra işlem hattı, ASP.NET Core tarafından işlenemezse gelen özel durumu önler. Bir sonuç filtresi bir özel durum işlenirken bir yanıtı veri yazmak için güvenilir bir yolu yoktur. Eylem sonucu bir özel durum oluşturduğunda üstbilgileri istemciye temizlenmiş, bir hata kodu göndermek için güvenilir bir mekanizma yoktur.

İçin bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, çağrı `await next` üzerinde <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> herhangi bir sonraki sonuç filtre ve eylem sonucu yürütür. Kısa devre oluşturur, ayarlayın [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) için `true` ve Remove() çağırmayın `ResultExecutionDelegate`:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

Bir soyut bir çerçeve sağlar `ResultFilterAttribute` , sınıflandırma. [AddHeaderAttribute](#add-header-attribute) gösterilen sınıfı daha önce bir sonuç filtre özniteliği örneği verilmiştir.

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter ve IAsyncAlwaysRunResultFilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> Ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri bildirmek bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> için tüm eylem sonuçlarına çalışan uygulama. Filtre sürece tüm eylem sonuçlarına uygulanır:

* Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ve yanıt short-circuits uygular.
* Özel Durum Filtresi, eylem sonucunu üreten tarafından bir özel durumu işler.

Filtreler dışında `IExceptionFilter` ve `IAuthorizationFilter` iki yoksa `IAlwaysRunResultFilter` ve `IAsyncAlwaysRunResultFilter`.

Örneğin, aşağıdaki filtre her zaman çalışır ve eylem sonucunu ayarlar (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ile bir *422 işlenemeyen* içerik anlaşması başarısız olduğunda, durum kodu:

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a>IFilterFactory

<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>. Bu nedenle, bir `IFilterFactory` örneği olarak kullanılabilir bir `IFilterMetadata` filtre işlem hattının herhangi bir yerindeki örneği. Çalışma zamanı, filtre çağırmak hazırlanırken yayınlayacağınızı çalışır bir `IFilterFactory`. Bu tür dönüştürme başarılı olursa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için çağrılır `IFilterMetadata` çağrılan örnek. Bu, kesin filtre ardışık düzen uygulama başlatıldığında açıkça ayarlanması gerekmez bu yana esnek bir tasarım sağlar.

`IFilterFactory` özel öznitelik uygulamaları filtreleri oluşturma başka bir yaklaşım kullanarak uygulanabilir:

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

Yukarıdaki kod çalıştırarak test edilebilir [örneği indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):

* F12 Geliştirici araçlarıyla çağırın.
* Gidin `https://localhost:5001/Sample/HeaderWithFactory`

F12 Geliştirici araçlarıyla örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüleme:

* **Yazar:** `Joe Smith`
* **globaladdheader:** `Result filter added to MvcOptions.Filters`
* **İç:** `My header`

Yukarıdaki kod oluşturur **iç:** `My header` yanıtı üstbilgisi.

### <a name="ifilterfactory-implemented-on-an-attribute"></a>Özniteliğin uygulandığı IFilterFactory

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

Filtreler uygulayan `IFilterFactory` için yararlıdır, filtreler:

* Parametreleri geçirme gerektirmez.
* DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>. `IFilterFactory` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği. `CreateInstance` Belirtilen tür Hizmetleri kapsayıcısı (dı) yükler.

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Aşağıdaki kodu uygulamak için üç koyulmaktadır `[SampleActionFilter]`:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

Önceki kodda yöntemiyle dekorasyon `[SampleActionFilter]` uygulamak için tercih edilen yaklaşım `SampleActionFilter`.

## <a name="using-middleware-in-the-filter-pipeline"></a>Filtre işlem hattı, ara yazılımın kullanılması

İş kaynağı filtreler gibi [ara yazılım](xref:fundamentals/middleware/index) , bunlar daha sonra işlem hattı, gelen yürütme her şeyin çevreleyen. Ancak, ASP.NET Core çalışma zamanı, ASP.NET Core bağlam ve yapılar için erişime sahip oldukları anlamına gelir parçası oldukları filtreleri ara yazılım farklı.

Ara yazılım bir filtre olarak kullanılacak bir türle oluşturma bir `Configure` filtre ardışık düzende eklemesine ara yazılım belirten yöntemi. Aşağıdaki örnek, bir isteğin geçerli kültürünü yerleştirmeye yerelleştirme ara yazılım kullanır:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Kullanım <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> ara yazılım çalıştırmak için:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Çalışan bir ara yazılım aynı filtre ardışık düzen aşamasını kaynak olarak, model bağlama önce ve sonra kalan ardışık düzenini filtreleri.

## <a name="next-actions"></a>Sonraki Eylemler

* Bkz: [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter)
* Filtrelerle denemeler için [indirin, test etme ve GitHub örneği değiştirirseniz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
