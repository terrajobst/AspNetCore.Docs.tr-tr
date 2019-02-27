---
title: ASP.NET core'da filtreleri
author: ardalis
description: Filtreleri nasıl çalıştığını ve ASP.NET Core MVC nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: a9081a9938d56b7612bba13937eba384ff02455b
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833741"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET core'da filtreleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), ve [Steve Smith](https://ardalis.com/)

*Filtreler* ASP.NET Core MVC önce veya sonra istek işleme ardışık düzeninde belirli aşamalara kod çalıştırmanıza olanak tanır.

 Yerleşik filtreler gibi görevleri işler:

 * Yetkilendirme (bir kullanıcı için yetkili olmayan kaynaklara erişimini engelleme).
 * Tüm istekler için HTTPS kullanmanızı sağlar.
 * Yanıt önbelleğe alma (önbelleğe alınan yanıt döndürmek için istek ardışık düzenini kısa devre). 

Özel Filtreler, geniş kapsamlı kritik konular işlemek için oluşturulabilir. Filtreleri, eylemleri arasında kod çoğaltma önleyebilirsiniz. Örneğin, bir hata özel durum filtresi işleme hata işleme birleştirebilir.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-filters-work"></a>Filtreler nasıl çalışır

Filtre çalıştırma içinde *MVC eylemi çağırma işlem hattı*bazen denilen *filtre ardışık düzen*.  Filtre ardışık düzen, yürütülecek eylemi MVC seçtikten sonra çalışır.

![İstek, diğer bir ara yazılım, yönlendirme ara yazılım, eylem seçimi ve MVC eylemi çağırma ardışık düzeni işlenir. İstek işleme geri eylem seçimi, yönlendirme ara yazılım ve diğer çeşitli ara yazılımı istemciye gönderilen bir yanıt olmadan önce devam eder.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtre türleri

Her filtre türü, farklı bir aşamada filtre ardışık düzende yürütülür.

* [Yetkilendirme filtreleri](#authorization-filters) ilk çalıştırma ve geçerli kullanıcının geçerli istek için yetki verilip verilmediğini belirlemek için kullanılır. Bir istek yetkisiz ise bunlar işlem hattı iki. 

* [Kaynak filtreleri](#resource-filters) yetkilendirme sonrasında bir isteği işlemek için ilk olan.  Kod ve filtre işlem hattının işlem hattı geri kalanını tamamlandıktan sonra geri kalan önce çalışabilirler. Bunlar yararlı önbelleğe almayı uygulayın ya da aksi takdirde performansı artırmak için filtre ardışık düzenini kısa devre oluşturur. Model bağlama üzerinde etki oluşturabilirsiniz böylece bunlar model bağlama önce çalıştırın.

* [Eylem filtreleri](#action-filters) kod hemen önce ve tek bir eylem yöntemi çağrıldıktan sonra çalıştırabilirsiniz. Bir eyleme geçirilen bağımsız değişkenleri ve döndürülen eylem sonucu işlemek için kullanılabilir. Eylem filtreleri Razor sayfalarında desteklenmiyor.

* [Özel durum filtreleri](#exception-filters) yanıt gövdesi için herhangi bir şey yazılmadan önce gerçekleşen işlenmeyen özel durumlar genel ilkeleri uygulamak için kullanılır.

* [Sonuç filtreleri](#result-filters) kod hemen önce ve sonra tek tek eylem sonuçlarını yürütülmesini çalıştırabilirsiniz. Yalnızca eylem yöntemi başarıyla yürütüldü, bunlar çalıştırın. Bunlar, görünümü veya biçimlendirici yürütme sarmanız gerekir mantığı için kullanışlıdır.

Aşağıdaki diyagramda şu filtre türlerini filtre işlem hattında nasıl etkileşim kurduklarını gösterir.

![İstek, yetkilendirme filtreleri, kaynak filtreleri, Model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütme işlenir. Biçimi üzerinde istek yalnızca sonuç filtreleri ve kaynak filtreler tarafından istemciye gönderilen bir yanıt olmadan önce işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Uygulama

Filtreleri farklı arabirimi tanımları aracılığıyla zaman uyumlu ve uyumsuz uygulamaları destekler. 

Çalışan zaman uyumlu filtreleri kod, her ikisi de, önce ve sonra bunların ardışık düzen aşamasını tanımlamak üzerinde*aşama*Executing ve*aşama*yöntemleri yürütülür. Örneğin, `OnActionExecuting` eylem yönteminden önce çağrılır ve `OnActionExecuted` eylem yöntemi döndükten sonra çağrılır.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Zaman uyumsuz filtre üzerinde tek bir tanımlama*aşama*ExecutionAsync yöntemi. Bu yöntem bir *FilterType*ExecutionDelegate temsilci, bir filtre ardışık düzen aşamasını yürütür. Örneğin, `ActionExecutionDelegate` çağrıları eylem yöntemi ve bir sonraki eylem filtresi ve yürütebilir kod önce ve sonra onu çağırabilir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Tek bir sınıftaki birden çok filtre aşamalar için arabirim uygulayabilir. Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfının Implements `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerine.

> [!NOTE]
> Uygulama **ya da** zaman uyumlu veya zaman uyumsuz sürümü filtre arabirimi, her ikisini birden değil. Framework ilk filtre zaman uyumsuz arabirimini uygulayan ve çağrı yaptığı bu durumda olup olmadığını denetler. Aksi durumda, zaman uyumlu arabirim yöntemleri çağırır. Her iki arabirimde üzerinde bir sınıf uygulamak için olsaydı, yalnızca zaman uyumsuz yöntem çağrılır. Soyut sınıflar gibi kullanırken <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> yalnızca zaman uyumlu metotları veya zaman uyumsuz yöntem her filtre türü için geçersiz kılarsınız.

### <a name="ifilterfactory"></a>IFilterFactory

[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>. Bu nedenle, bir `IFilterFactory` örneği olarak kullanılabilir bir `IFilterMetadata` filtre işlem hattının herhangi bir yerindeki örneği. Framework filtre çağırmak hazırlanırken yayınlayacağınızı çalışır bir `IFilterFactory`. Bu tür dönüştürme başarılı olursa [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) yöntemi oluşturmak için çağrılır `IFilterMetadata` çağrılan örnek. Bu, kesin filtre ardışık düzen uygulama başlatıldığında açıkça ayarlanması gerekmez bu yana esnek bir tasarım sağlar.

Uygulayabileceğiniz `IFilterFactory` filtreleri oluşturma başka bir yaklaşım olarak kendi öznitelik uygulamaları üzerinde:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Yerleşik filtre öznitelikleri

Öğesinin alt sınıfı için yerleşik öznitelik tabanlı filtre framework içerir ve özelleştirin. Örneğin, aşağıdaki sonucu filtre üstbilgi yanıta ekler.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Öznitelikleri, yukarıdaki örnekte gösterildiği gibi bağımsız değişken kabul edecek şekilde filtreleri izin verin. Bu öznitelik bir denetleyici veya eylem yöntemine ekleyin ve HTTP üstbilgisinin değerini ve adını belirtmek:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Sonucu `Index` eylem aşağıda gösterilen - yanıt üstbilgilerini sağ alt görüntülenir.

![Microsoft Edge, geliştirici araçları Yazar Steve Smith dahil olmak üzere yanıt üst bilgilerini gösterme @ardalis](filters/_static/add-header.png)

Filtre arabirimlerinin birkaç özel uygulamalar için temel sınıf olarak kullanılabilecek ilgili özniteliklere sahiptir.

Filtre öznitelikleri:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` ve `ServiceFilterAttribute` açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Filtre kapsamları ve Yürütme sırası

Bir filtre işlem hattının üç birinde eklenebilir *kapsamları*. Bir öznitelik kullanarak, belirli bir eylem yönteminin veya bir denetleyici sınıfı için bir filtre ekleyebilirsiniz. Veya tüm denetleyicilere ve eylemlere için genel filtre kaydedebilirsiniz. Filtreleri eklenir genel ekleyerek `MvcOptions.Filters` koleksiyonda `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Varsayılan yürütme sırası

Belirli bir ardışık düzen aşaması için birden çok filtre olduğunda, filtre yürütme varsayılan sıra kapsamı belirler.  Genel filtrelerin sırayla yöntemi filtreleri çevreleyen sınıf filtreleri alın. Kapsamdaki her artış gibi önceki kapsam sarmalandıktan gibi bu bazen "Rusça doll" iç içe geçme olarak adlandırılır bir [iç içe geçme doll](https://wikipedia.org/wiki/Matryoshka_doll). Geçersiz kılma istenen davranışı genellikle sıralama açıkça belirlemek zorunda kalmadan alın.

Bu iç içe geçme sonucunda *sonra* kodu filtre ters sırada çalışır *önce* kod. Sıra şöyle görünür:

* *Önce* kod genel uygulanan filtreler
  * *Önce* kod denetleyicilerine uygulanan filtreler
    * *Önce* kod eylem yöntemleri için uygulanan filtreler
    * *Sonra* kod eylem yöntemleri için uygulanan filtreler
  * *Sonra* kod denetleyicilerine uygulanan filtreler
* *Sonra* kod genel uygulanan filtreler
  
Filtre yöntemleri için zaman uyumlu eylem filtrelerini çağrılma sırası gösteren bir örnek aşağıda verilmiştir.

| Dizisi | Filtre kapsamı | Filter yöntemi |
|:--------:|:------------:|:-------------:|
| 1. | Global | `OnActionExecuting` |
| 2 | Denetleyici | `OnActionExecuting` |
| 3 | Yöntem | `OnActionExecuting` |
| 4 | Yöntem | `OnActionExecuted` |
| 5 | Denetleyici | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Bu dizisini gösterir:

* Yöntem filtresi denetleyici filtresi içinde yer alıyor.
* Denetleyici filtreyi genel filtre içinde yer alıyor. 

İçinde bir zaman uyumsuz filtre kullanıyorsanız koymak istediğiniz başka bir şekilde kullanıcının*aşama*sıkı bir kapsamla filtrelerinin tümüne run ExecutionAsync yöntemi kodunuzu yığında olsa da.

> [!NOTE]
> Devralınan denetleyicilerin `Controller` taban sınıfı içeren `OnActionExecuting` ve `OnActionExecuted` yöntemleri. Bu yöntemler çalıştıran belirli bir eylem filtrelerini kaydır: `OnActionExecuting` herhangi filtreleri önce çağrılır ve `OnActionExecuted` filtrelerinin tümüne sonra çağrılır.

### <a name="overriding-the-default-order"></a>Varsayılan sıra geçersiz kılma

Uygulayarak varsayılan yürütme sırası geçersiz kılabilirsiniz `IOrderedFilter`. Bu arabirimi kullanıma sunan bir `Order` yürütme sırasını belirlemek için kapsam üzerinde önceliklidir özelliği. Daha düşük bir filtre `Order` değer olacaktır, *önce* daha yüksek bir değere sahip bir filtre, önce yürütülen kod `Order`. Daha düşük bir filtre `Order` değer olacaktır, *sonra* , daha yüksek bir filtre sonra yürütülen kod `Order` değeri. Ayarlayabileceğiniz `Order` Oluşturucu parametresi kullanarak özelliği:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Önceki örnek ancak kümesi gösterilen 3 eylem filtrelerini aynı varsa `Order` yürütme sırasını tersine, denetleyici ve genel özelliği 1 ve 2'ye sırasıyla filtreler.

| Dizisi | Filtre kapsamı | `Order` Özelliği | Filter yöntemi |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1. | Yöntem | 0 | `OnActionExecuting` |
| 2 | Denetleyici | 1.  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Denetleyici | 1.  | `OnActionExecuted` |
| 6 | Yöntem | 0  | `OnActionExecuted` |

`Order` Filtreleri çalışacağı sırayı belirlerken özelliği toplayın kapsam. Filtreleri ilk sıraya göre sıralanır ve kapsam TIES ayırmak için kullanılır. Tüm yerleşik filtreleri uygulamak `IOrderedFilter` ve varsayılan `Order` 0 değeri. Ayarlamadığınız sürece yerleşik filtreleri için kapsam sırasını belirler. `Order` için sıfır olmayan bir değer.

## <a name="cancellation-and-short-circuiting"></a>İptal ve kestirmeler

Filtre ardışık düzen herhangi bir noktada ayarlayarak iki `Result` özelliği `context` filter yöntemi için sağlanan parametre. Örneğin, aşağıdaki kaynak filtre rest işlem hattının yürütülmesini engeller.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` ve `AddHeader` filtre hedef `SomeResource` eylem yöntemi. `ShortCircuitingResourceFilter`:

* Kaynak Filtresi olduğu için ilk olarak çalışır ve `AddHeader` bir eylem filtresi.
* Kalan ardışık düzenini short-circuits.

Bu nedenle `AddHeader` filtresi hiç çalıştığı için `SomeResource` eylem. Belirtilen eylem yöntemini düzeyinde hem de bir filtre uygulansaydı, bu davranışı aynı olacaktır `ShortCircuitingResourceFilter` ilk çalıştı. `ShortCircuitingResourceFilter` Filtre türü nedeniyle ya da açık kullanılarak ilk çalıştıran `Order` özelliği.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Bağımlılık ekleme

Filtre türü veya örnek tarafından eklenebilir. Bir örnek ekler, bu örnek her istek için kullanılacaktır. Bir türü eklerseniz, her istek için bir örneği oluşturulur ve herhangi bir oluşturucu bağımlılığın tarafından doldurulur türü etkinleştirildi, olacaktır [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı). Türe göre filtre ekleme eşdeğerdir `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler tarafından sağlanan Oluşturucusu bağımlılıkları olamaz [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı). Öznitelik oluşturucu parametrelerinin nereye uygulanan sağlanan olmalıdır olmasıdır. Bu öznitelikler nasıl işe sınırlamasıdır.

Filtrelerinizi DI erişmek için gereken bağımlılıkları varsa, desteklenen birçok yaklaşım vardır. Bir sınıf ya da eylem yöntemine aşağıdakilerden birini kullanarak, filtre uygulayabilirsiniz:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` üzerinde öznitelik uygulanmadı

> [!NOTE]
> DI almak isteyebileceğiniz bir Günlükçü bağımlılığıdır. Ancak, oluşturma ve filtreler yalnızca günlüğe kaydetme amacıyla beri kullanarak önlemek [yerleşik bir çerçeve günlüğe kaydetme özelliklerini](xref:fundamentals/logging/index) gerekenler zaten sağlayabilir. Günlüğe kaydetme, filtrelere Ekle kullanacaksanız, işletme etki alanı sorunlarını veya filtreniz, yerine MVC eylemler veya diğer framework olayları özgü davranış odaklanmanız gerekir.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Hizmet uygulaması türlerini filtreleme DI kaydedilir. A `ServiceFilterAttribute` DI filtre örneğini alır. Ekleme `ServiceFilterAttribute` kapsayıcısı `Startup.ConfigureServices`ve onu referans bir `[ServiceFilter]` özniteliği:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Kullanırken `ServiceFilterAttribute`ayarını `IsReusable` bir ipucudur, filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir. Framework filtre tek bir örneği oluşturulur veya filtre daha sonraki bir noktada DI kapsayıcısından yeniden istenen olmayacaktır garanti sağlar. Kullanmaktan kaçının `IsReusable` Hizmetleri tekil dışında bir yaşam süresi ile bağlı olduğu bir filtre kullanırken.

Kullanarak `ServiceFilterAttribute` kaydetmeden filtre türü sonuçları bir özel durum:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` uygulayan `IFilterFactory`. `IFilterFactory` kullanıma sunan `CreateInstance` yöntemi oluşturmak için bir `IFilterMetadata` örneği. `CreateInstance` Yöntemi, belirtilen türe Hizmetleri kapsayıcısı (dı) yükler.

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` benzer `ServiceFilterAttribute`, ancak türü doğrudan DI kapsayıcısından çözülmüş değildir. Kullanarak türün örneğini oluşturduğunda `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Bu farklılık nedeniyle:

* Kullanılarak başvurulan türleri `TypeFilterAttribute` kapsayıcı ile ilk kaydedilmesi gerekmez.  Kapsayıcı tarafından yerine bunların bağımlılıklarını sahiptirler. 
* `TypeFilterAttribute` İsteğe bağlı olarak tür için oluşturucu bağımsız değişkenleri kabul edebilir.

Kullanırken `TypeFilterAttribute`ayarını `IsReusable` bir ipucudur, filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir. Framework filtre tek bir örneği oluşturulur tutarlılık garantisi sağlar. Kullanmaktan kaçının `IsReusable` Hizmetleri tekil dışında bir yaşam süresi ile bağlı olduğu bir filtre kullanırken.

Aşağıdaki örneği kullanarak bir tür bağımsız değişkenleri geçirmek gösterilmiştir `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>IFilterFactory, özniteliği uygulandı

Bir filtre varsa:

* Tüm bağımsız değişkenleri gerektirmez.
* DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.

Sınıflar ve yöntemler yerine kendi adlandırılmış öznitelik kullanabileceğinizi `[TypeFilter(typeof(FilterType))]`). Aşağıdaki filtre, bunu nasıl uygulanabileceği gösterilmektedir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Bu filtre uygulanabilir, sınıfları veya yöntemleri kullanarak `[SampleActionFilter]` kullanmak zorunda olmak yerine söz dizimini `[TypeFilter]` veya `[ServiceFilter]`.

## <a name="authorization-filters"></a>Yetkilendirme filtreleri

*Yetkilendirme filtreleri*:

* Eylem yöntemlerine erişimi denetler.
* Filtre ardışık yürütülecek ilk filtrelerdir. 
* Sahip bir yöntemi önce ancak bundan sonra yöntemi. 

Yalnızca kendi yetkilendirme framework yazıyorsanız özel yetkilendirme filtresi yazmanız gerekir. Yetkilendirme İlkelerini yapılandırarak veya özel yetkilendirme ilkesi özel filtre yazma üzerine yazma seçeneğini tercih eder. Yetkilendirme sistemi çağırmak için yalnızca sorumlu yerleşik filtre uygulamasıdır.

Hiçbir özel durum işleyecek bu yana yetkilendirme filtrelerini içinde özel durumlar olmamalıdır (özel durum filtreleri olmaz onları işleyebilirsiniz). Bir challenge, bir özel durum oluştuğunda vermeyi düşünün.

Daha fazla bilgi edinin [yetkilendirme](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Kaynak filtreleri

* Ya da uygulama `IResourceFilter` veya `IAsyncResourceFilter` arabirimi
* Filtre ardışık düzen çoğunu yürütülmesi sarmalar. 
* Yalnızca [yetkilendirme filtrelerini](#authorization-filters) kaynak filtreleri önce çalışır.

Kaynak filtreleri, bir istek yapıyor çoğunu iki faydalıdır. Örneğin, yanıt önbellekte ise önbelleğe alma bir filtre işlem hattının rest önleyebilirsiniz.

[Kısa circuiting kaynak filtresi](#short-circuiting-resource-filter) daha önce gösterilen bir kaynak filtresi örneğidir. Başka bir örnek [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Model bağlama form verilerini erişmesini engeller. 
* Bu, büyük dosyaların karşıya yüklenmesi için yararlıdır ve belleğe okuma formu engellemek istiyorsunuz.

## <a name="action-filters"></a>Eylem filtreleri

> [!IMPORTANT]
> Eylem filtreleri yapmak **değil** Razor sayfaları için geçerlidir. Razor sayfaları destekler <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> . Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).

*Eylem filtreleri*:

* Ya da uygulama `IActionFilter` veya `IAsyncActionFilter` arabirimi.
* Eylem yöntemleri yürütülmesini yürütülmesi çevreler.

Örnek eylem filtresi şu şekildedir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Aşağıdaki özellikleri sağlar:

* `ActionArguments` -eyleme yönelik girişleri işleme sağlar.
* `Controller` -denetleyici örneği yönlendirme sağlar. 
* `Result` -Bu ayarı short-circuits sonraki eylem filtreleri eylem yöntemi ve yürütme. Bir özel durum da eylem yöntemi ve sonraki filtrelerin yürütülmesini engeller ancak başarılı sonuç yerine bir hata olarak kabul edilir.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Sağlar `Controller` ve `Result` ayrıca aşağıdaki özellikleri:

* `Canceled` -Eylem yürütme başka bir filtre tarafından kısa devre yapılma durumlarda true olur.
* `Exception` -Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa null olmayan olacaktır. Bu özelliği etkili bir şekilde null olarak ayarlamak 'işler' bir özel durum, ve `Result` eylem yönteminden normalde döndürülmedi yokmuş gibi yürütülür.

İçin bir `IAsyncActionFilter`, çağrı `ActionExecutionDelegate`:

* Herhangi bir sonraki eylem filtrelerini ve eylem yöntemini yürütür.
* Döndürür `ActionExecutedContext`. 

Kısa devre oluşturur, Ata `ActionExecutingContext.Result` bazı sonuç örneği ve Remove() çağırmayın `ActionExecutionDelegate`.

Bir soyut bir çerçeve sağlar `ActionFilterAttribute` alt olabilir. 

Model durumu doğrulamak ve durumu geçersiz olduğunda herhangi bir hata döndürmek için bir eyleme eylem filtresi kullanabilirsiniz:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Yöntemi çalıştırıldıktan sonra eylem yöntemi ve can görebilir ve eylemin sonuçlarını işlemek `ActionExecutedContext.Result` özelliği. `ActionExecutedContext.Canceled` Eylem yürütme başka bir filtre tarafından kısa devre yapılma gerekiyorsa true olarak ayarlanır. `ActionExecutedContext.Exception` Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayar `ActionExecutedContext.Exception` null:

* Etkili bir şekilde 'bir özel durum işleme'.
* `ActionExecutedContext.Result` Bu normalde eylem yönteminden döndürülen yokmuş gibi yürütülür.

## <a name="exception-filters"></a>Özel durum filtreleri

*Özel durum filtreleri* ya da uygulama `IExceptionFilter` veya `IAsyncExceptionFilter` arabirimi. Ortak hata işleme için bir uygulama ilkeleri uygulamak için kullanılabilir. 

Aşağıdaki örnek özel durum filtresi, uygulama geliştirme olduğunda oluşan özel durumları hakkında ayrıntıları görüntülemek için bir özel Geliştirici hata görünümü kullanır:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Özel durum filtreleri:

* Önceki ve sonraki olaylar yok. 
* Uygulama `OnException` veya `OnExceptionAsync`. 
* Denetleyici oluşturulmasında meydana gelen işlenmeyen özel durumları işlemek [model bağlama](../models/model-binding.md), eylem filtreleri ya da eylem yöntemleri. 
* Kaynak filtreleri, sonuç filtrelerini veya yürütme MVC sonucu oluşan özel durumlarının yakalamayın.

Bir özel durumu işlemek üzere ayarlanmış `ExceptionContext.ExceptionHandled` özelliği doğru veya yanıt yazın. Bu özel durumun yayılmasını durdurur. Özel Durum Filtresi, bir özel durum "başarılı" içine kapatamazsınız. Yalnızca bir eyleme eylem filtresi, bunu yapabilirsiniz.

> [!NOTE]
> Ayarlarsanız ASP.NET Core 1.1 yanıtı gönderilmez `ExceptionHandled` true **ve** yanıt yazın. Bu senaryoda, ASP.NET Core 1.0 yanıtı göndermek ve 1.1.2 ASP.NET Core 1.0 uygulamasına döndürür. Daha fazla bilgi için [sorun #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub deposunda. 

Özel durum filtreleri:

* MVC Eylemler içinde gerçekleşen Yakalama özel durumlar için uygundur.
* Ara yazılım işleme hata olarak kadar esnek değildir. 

Özel durum işleme için bir ara yazılım tercih eder. Hata işleme yapmak için özel durum filtreleri yalnızca gerek duyduğunuz kullanın *farklı* göre MVC eylemi seçildi. Örneğin, uygulamanızın eylem yöntemleri görünümler/HTML ve her iki API uç noktaları için olabilir. API uç noktaları, HTML olarak bir hata sayfası görüntüleme tabanlı eylemleri döndürebilir sırasında hata bilgisi, JSON olarak döndürebilir.

`ExceptionFilterAttribute` Sınıflandırılacak. 

## <a name="result-filters"></a>Sonuç filtreleri

* Bir arabirim uygular:
  * `IResultFilter` veya `IAsyncResultFilter`.
  * `IAlwaysRunResultFilter` veya `IAsyncAlwaysRunResultFilter`
* Yürütülmesi, eylem sonuçlarını yürütülmesini çevreler. 

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter ve IAsyncResultFilter

Bir HTTP üstbilgisi ekler bir sonuç filtresi örneği aşağıda verilmiştir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Söz konusu eylemini yürütülmekte olan sonuç türüne bağlıdır. Bir parçası olarak işleme tüm razor görünüm döndüren bir MVC eylemi verilebilir `ViewResult` yürütülmekte. Bir API yöntemi, bazı serileştirme yürütme sonucu bir parçası olarak gerçekleştirebilir. Daha fazla bilgi edinin [eylem sonuçları](actions.md)

Eylem veya eylem filtreleri eylem sonucu üretir, sonuç filtreleri için başarılı sonuçları - yalnızca çalıştırılır. Özel durum filtreleri, bir özel durumu işlediğinizde sonuç filtrelerini yürütülmedi.

`OnResultExecuting` Yöntemi iki yürütme sonraki sonuç filtreleri ve eylem sonucu ayarlayarak `ResultExecutingContext.Cancel` true. Genellikle, boş bir yanıt oluşturmaktan kaçınmak için kısa devre zaman yanıt nesnesine yazmanız gerekir. Bir özel durum olacak durum:

* Sonraki filtrelerin ve eylem sonucu yürütülmesini engeller.
* Başarılı sonuç yerine bir hata olarak kabul edilecek.

Zaman `OnResultExecuted` yöntemi çalıştığında, yanıtı istemciye büyük olasılıkla gönderildi ve daha fazla (bir özel durum oluştu sürece) değiştirilemez. `ResultExecutedContext.Canceled` yürütülen eylem sonucu başka bir filtre tarafından kısa devre yapılma gerekiyorsa true olarak ayarlanır.

`ResultExecutedContext.Exception` Eylem sonucu veya bir sonraki sonuç filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayar `Exception` için null etkili bir şekilde 'bir özel durum işleme' ve daha sonra işlem hattı, MVC tarafından işlenemezse gelen özel durumu önler. Bir sonuç filtresi bir özel durum işlenirken herhangi bir veri yazma yanıtını mümkün olmayabilir. Eylem sonucu kadar yürütme oluşturur ve üstbilgileri istemciye zaten temizlenmiş, bir hata kodu göndermek için güvenilir bir mekanizma yoktur.

İçin bir `IAsyncResultFilter` çağrısı `await next` üzerinde `ResultExecutionDelegate` herhangi bir sonraki sonuç filtre ve eylem sonucu yürütür. Kısa devre oluşturur, ayarlayın `ResultExecutingContext.Cancel` için doğru ve Remove() çağırmayın `ResultExectionDelegate`.

Bir soyut bir çerçeve sağlar `ResultFilterAttribute` alt olabilir. [AddHeaderAttribute](#add-header-attribute) daha önce gösterilen sınıfı bir sonuç filtre özniteliği örneği verilmiştir.

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter ve IAsyncAlwaysRunResultFilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> Ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri bildirmek bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> ilişkin eylem sonuçlarıyla çalışan uygulama. Filtre için eylem sonucunu sürece uygulanır bir <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ve yanıt short-circuits uygular.

Her zaman çalıştır, bir özel durum veya yetkilendirme filtresi bunları ne zaman short-circuits dışında başka bir deyişle, bu "her zaman çalıştır" filtreler. Filtreler dışında `IExceptionFilter` ve `IAuthorizationFilter` bunları kısa devre yok.

Örneğin, aşağıdaki filtre her zaman çalışır ve eylem sonucunu ayarlar (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ile bir *422 işlenemeyen* içerik anlaşması başarısız olduğunda, durum kodu:

```csharp
public class UnprocessableResultFilter : Attribute, IAlwaysRunResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
    {
        if (context.Result is StatusCodeResult statusCodeResult &&
            statusCodeResult.StatusCode == 415)
        {
            context.Result = new ObjectResult("Can't process this!")
            {
                StatusCode = 422,
            };
        }
    }

    public void OnResultExecuted(ResultExecutedContext context)
    {
    }
}
```

## <a name="using-middleware-in-the-filter-pipeline"></a>Filtre işlem hattı, ara yazılımın kullanılması

İş kaynağı filtreler gibi [ara yazılım](xref:fundamentals/middleware/index) , bunlar daha sonra işlem hattı, gelen yürütme her şeyin çevreleyen. Ancak, MVC, MVC bağlam ve yapılar için erişime sahip oldukları anlamına gelir parçası oldukları filtreleri ara yazılım farklı.

ASP.NET Core 1.1 içinde filtre işlem hattı, ara yazılım kullanabilirsiniz. MVC rota verilerini veya yalnızca belirli denetleyicileri veya Eylemler çalışması gereken bir erişim gerektiren bir ara yazılım bileşeni varsa bunu yapmak isteyebilirsiniz.

Ara yazılım bir filtre olarak kullanılacak bir türle oluşturma bir `Configure` filtre ardışık düzende eklemek istediğiniz bir ara yazılım belirten yöntemi. Bir isteğin geçerli kültürünü yerleştirmeye yerelleştirme ara yazılımı kullanan bir örnek aşağıda verilmiştir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Ardından `MiddlewareFilterAttribute` seçili denetleyici veya eylem için bir ara yazılım çalıştırmak için veya genel olarak:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Çalışan bir ara yazılım aynı filtre ardışık düzen aşamasını kaynak olarak, model bağlama önce ve sonra kalan ardışık düzenini filtreleri.

## <a name="next-actions"></a>Sonraki Eylemler

* Bkz: [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter)
* Filtrelerle denemeler için [indirin, test ve Github örneği değiştirirseniz](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
