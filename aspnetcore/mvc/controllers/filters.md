---
title: ASP.NET Core filtreleri
author: ardalis
description: Filtreler nasıl çalışır ve bunları ASP.NET Core MVC'de nasıl kullanacağınızı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 4/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: edc2e9460eb68febe25e8dd60e3872e5ab28e9e9
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core filtreleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [zel Dykstra](https://github.com/tdykstra/), ve [Steve Smith](https://ardalis.com/)

*Filtreler* ASP.NET Core MVC'de, önce veya sonra istek işleme ardışık düzeninde belirli aşamaları kodu çalıştırmanızı sağlar.

> [!IMPORTANT]
> Bu konuda mu **değil** Razor sayfalarına uygulayın. ASP.NET Core 2.1 ve üzeri destekler [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor sayfalarının. Daha fazla bilgi için bkz: [yöntemleri için Razor sayfalarının filtre](xref:mvc/razor-pages/filter).

 Yerleşik filtreler görevler gibi işler:
 
 * Yetkilendirme (bir kullanıcı için yetkili değil kaynaklarına erişimini engelleme).
 * Tüm istekleri HTTPS kullanmak sağlama.
 * Yanıt (önbelleğe alınan yanıt döndürmek için istek ardışık düzenini kısa devre) önbelleğe alma. 

Özel Filtreler arası kesme sorunları işlemek için oluşturulabilir. Filtreler kodu eylemler arasında çoğaltma önleyebilirsiniz. Örneğin, bir hata özel durum filtresi işleme hata işleme birleştirebilir.

[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Filtreler nasıl çalışır?

Filtreler çalıştırmak içinde *MVC eylemi çağırma ardışık*ve bazen denir *filtre ardışık düzen*.  MVC yürütmek için eylemi seçer sonra filtre ardışık düzen çalışır.

![İstek diğer ara yazılım, yönlendirme ara yazılım, eylem seçimi ve MVC eylemi çağırma ardışık düzeni işlenir. İstek işleme istemciye gönderilen bir yanıt olmadan önce geri eylem seçimi, yönlendirme ara yazılımı ve çeşitli diğer ara yazılımı devam eder.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtre türleri

Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür.

* [Yetkilendirme filtreleri](#authorization-filters) ilk çalıştırma ve geçerli kullanıcının geçerli istek için yetki verilip verilmediğini belirlemek için kullanılır. Bir istek yetkisiz ise bunlar ardışık kısa devre oluşturur. 

* [Kaynak filtreleri](#resource-filters) yetkilendirme sonrasında bir isteği işlemek üzere ilk alan.  Filtre ardışık düzen ve ardışık düzen kalan tamamlandıktan sonra geri kalan önce kodu çalıştırabilirler. Önbelleğe alma nasıl uygulanacağını veya aksi halde filtre ardışık düzen performans nedenleriyle kısa devre oluşturur yararlı oldukları. Model bağlama etkileyebilir böylece bunlar model bağlama önce çalıştırın.

* [Eylem filtreleri](#action-filters) kodu hemen önce ve tek tek eylem yöntemi çağrıldıktan sonra çalıştırabilirsiniz. Bir eyleme geçirilen bağımsız değişkenler ve eylem döndürülen sonuç işlemek için kullanılabilir.

* [Özel durum filtreleri](#exception-filters) herhangi bir şey yanıt gövdesi yazılmadan önce gerçekleşen işlenmeyen özel durumlar genel ilkeleri uygulamak için kullanılır.

* [Sonuç filtreleri](#result-filters) kodu hemen önce ve sonra tek tek eylem sonuçlarını yürütülmesi çalıştırabilirsiniz. Yalnızca eylem yöntemi başarıyla yürütüldü, bunlar çalıştırın. Bunlar, görünümü veya biçimlendirici yürütme çevreleyen gerekir mantığı için kullanışlıdır.

Aşağıdaki diyagramda, bu filtre türleri filtre ardışık düzeninde nasıl etkileşim kurduğu gösterilmektedir.

![Talep yetkilendirme filtreleri, kaynak filtreleri, Model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütme işlenir. Yolda isteği yalnızca sonuç filtreleri ve kaynak filtreleri tarafından istemciye gönderilen bir yanıt olmadan önce işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Uygulama

Filtreler farklı arabirimi tanımları aracılığıyla zaman uyumlu ve uyumsuz uygulamaları destekler. 

Çalıştırabilirsiniz zaman uyumlu filtreleri kod, her ikisi de, önce ve bunların ardışık düzen aşaması tanımladıktan sonra*aşama*Executing ve*aşama*yöntemleri yürütülür. Örneğin, `OnActionExecuting` eylem yöntemi çağrılmadan önce çağrılır ve `OnActionExecuted` eylem yöntemine döndürür sonra çağrılır.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Zaman uyumsuz filtreleri üzerinde tek bir tanımlamak*aşama*ExecutionAsync yöntemi. Bu yöntem alır bir *FilterType*filtre ardışık düzen aşaması yürüten ExecutionDelegate temsilci. Örneğin, `ActionExecutionDelegate` ve eylem yöntemi yürütebilir çağrıları kod önce ve sonra onu çağırabilir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Tek bir sınıftaki birden çok filtre aşamaları için arabirimleri uygulayabilir. Örneğin, [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) uygulayan sınıf `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerine.

> [!NOTE]
> Uygulama **ya da** zaman uyumlu veya zaman uyumsuz bir filtre arabiriminin sürümünü, her ikisine değil. Framework ilk filtreyi zaman uyumsuz arabirimini uygulayan ve Öyleyse, çağıran bakar. Aksi durumda, zaman uyumlu arabiriminin yöntemleri çağırır. Her iki arabirimde üzerinde bir sınıf uygulama olsaydı, yalnızca zaman uyumsuz yöntem çağrılması. Soyut sınıflar gibi kullanırken [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) yalnızca zaman uyumlu yöntemleri veya zaman uyumsuz yöntem her filtre türü için geçersiz kılarsınız.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` uygulayan `IFilter`. Bu nedenle, bir `IFilterFactory` örneği olarak kullanılabilir bir `IFilter` filtre ardışık düzen başka bir yerindeki örneği. Framework filtre çağırmak hazırlarken, hangisine yayınlayacağınızı çalışır bir `IFilterFactory`. Bu atama başarılı olursa, `CreateInstance` yöntemi oluşturmak için çağrılır `IFilter` çağrılan örnek. Bu, kesin filtre ardışık düzen uygulama başlatıldığında açıkça ayarlanması gerekmez beri esnek bir tasarım sağlar.

Uygulayabileceğiniz `IFilterFactory` filtreleri oluşturma için başka bir yaklaşım olarak kendi özniteliği uygulamaları üzerinde:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Yerleşik filtre öznitelikleri

Bir alt kümesi için yerleşik öznitelik tabanlı filtreler framework içerir ve özelleştirin. Örneğin, aşağıdaki sonuç filtresi üstbilgi yanıta ekler.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Öznitelikler, yukarıdaki örnekte gösterildiği gibi bağımsız değişken kabul etmek filtreleri sağlar. Bu öznitelik bir denetleyici veya eylem yöntemine ekleyin ve adını ve HTTP üstbilgisinin değerini belirtmek:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Sonucu `Index` eylem aşağıda gösterilen - yanıt üstbilgilerini sağ alt görüntülenir.

![Microsoft Edge, geliştirici araçları gösteren Yazar Steve Smith dahil olmak üzere, yanıt üstbilgileri @ardalis](filters/_static/add-header.png)

Filtre arabirimlerinin çeşitli özel uygulamalar için temel sınıf olarak kullanılabilir karşılık gelen özniteliklere sahiptir.

Filtre öznitelikleri:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` ve `ServiceFilterAttribute` açıklanacak [bu makalenin ilerisinde yer](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Filtre kapsamı ve yürütme sırasını

Bir filtre ardışık düzen üç birinde eklenebilir *kapsamları*. Bir öznitelik kullanarak, belirli bir eylem yönteminin ya da bir denetleyici sınıfı bir filtre ekleyebilirsiniz. Veya genel olarak tüm denetleyicileri ve eylemleri için bir filtre kaydedebilirsiniz. Filtreler eklenir genel ekleyerek `MvcOptions.Filters` koleksiyonunda `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Varsayılan yürütme sırasını

Belirli bir ardışık düzen aşaması için birden çok filtre olduğunda, kapsam filtresi yürütme varsayılan sırasını belirler.  Genel filtrelerin sırayla yöntemi filtreleri çevreleyen sınıfı filtreleri koyun. Kapsamdaki her artış gibi önceki kapsamı paketlenir Bu bazen "Rusça doll" iç içe geçme olarak verilir bir [iç içe geçmiş doll](https://wikipedia.org/wiki/Matryoshka_doll). İstenen geçersiz kılma davranışı genellikle açık olarak sıralama belirlenemiyor gerek kalmadan alın.

Bu içe sonucunda *sonra* kodu filtreleri ters sırada çalışır *önce* kodu. Dizisi şöyle görünür:

* *Önce* genel olarak uygulanan filtreler kodu
  * *Önce* denetleyicilerine uygulanan filtreler kodu
    * *Önce* eylem yöntemleri için uygulanan filtreler kodu
    * *Sonra* eylem yöntemleri için uygulanan filtreler kodu
  * *Sonra* denetleyicilerine uygulanan filtreler kodu
* *Sonra* genel olarak uygulanan filtreler kodu
  
Filtre yöntemleri için zaman uyumlu eylem filtrelerini çağrılma sırası gösteren örnek aşağıda verilmiştir.

| Sırası | Filtre kapsamı | Filter yöntemi |
|:--------:|:------------:|:-------------:|
| 1. | Global | `OnActionExecuting` |
| 2 | Denetleyici | `OnActionExecuting` |
| 3 | Yöntem | `OnActionExecuting` |
| 4 | Yöntem | `OnActionExecuted` |
| 5 | Denetleyici | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Bu sırayı gösterir:

* Yöntem filtresi denetleyicisi filtre içinde yer alıyor.
* Denetleyici filtreyi genel filtre içinde yer alıyor. 

Zaman uyumsuz filtre içinde iseniz koymak istediğiniz başka bir yolu, kullanıcının*aşama*ExecutionAsync yöntemi, tüm filtreleri daha sıkı bir kapsamla çalıştırmak kodunuzu yığında olsa.

> [!NOTE]
> Öğesinden devralınan her denetleyici `Controller` temel sınıf içerir `OnActionExecuting` ve `OnActionExecuted` yöntemleri. Bu yöntemler çalıştıran belirli bir eylem filtrelerini kaydır: `OnActionExecuting` herhangi bir filtre önce çağrılır ve `OnActionExecuted` tüm filtreleri sonra çağrılır.

### <a name="overriding-the-default-order"></a>Varsayılan sırası geçersiz kılma

Uygulayarak yürütme varsayılan dizisini geçersiz kılabilirsiniz `IOrderedFilter`. Bu bir arabirimi kullanıma sunan bir `Order` yürütme sırasını belirlemek için kapsam önceliklidir özelliği. Bir alt sahip bir filtre `Order` değer olacaktır, *önce* , daha yüksek bir değeri olan bir filtre önce yürütülen kod `Order`. Bir alt sahip bir filtre `Order` değer olacaktır, *sonra* , daha yüksek olan bir filtre sonra yürütülen kod `Order` değeri. Ayarlayabileceğiniz `Order` Oluşturucu parametresini kullanarak özelliği:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Yukarıdaki örnek ancak kümesi gösterilen 3 eylem filtrelerini aynı varsa `Order` denetleyicisinin ve genel özelliği 1 ve 2'ye sırasıyla filtreleri, yürütme sırasını tersine.

| Sırası | Filtre kapsamı | `Order` Özelliği | Filter yöntemi |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1. | Yöntem | 0 | `OnActionExecuting` |
| 2 | Denetleyici | 1.  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Denetleyici | 1.  | `OnActionExecuted` |
| 6 | Yöntem | 0  | `OnActionExecuted` |

`Order` Filtreleri çalıştırılacağı sipariş belirlerken özelliği toplayın kapsamı. Filtreler önce sıraya göre sıralanır ve ardından kapsamı TIES ayırmak için kullanılır. Tüm yerleşik filtreleri uygulamak `IOrderedFilter` ve varsayılan `Order` 0 değeri. FIR yerleşik filtreleri, kapsamı belirler sipariş ayarladığınız sürece `Order` için sıfır olmayan bir değer.

## <a name="cancellation-and-short-circuiting"></a>İptal etme ve kestirmeler

Herhangi bir noktada filtre ardışık düzen ayarlayarak kısa devre oluşturur `Result` özellikte `context` filtre yönteme sağlanan parametre. Örneğin, aşağıdaki kaynak filtresi kalan ardışık düzenini yürütülmesini engeller.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` ve `AddHeader` filtre hedef `SomeResource` eylem yöntemi. `ShortCircuitingResourceFilter`:

* Kaynak Filtresi olduğu için ilk olarak çalışır ve `AddHeader` bir eylem filtresi.
* Kalan ardışık düzenini short-circuits.

Bu nedenle `AddHeader` hiçbir zaman filtresi çalıştıran `SomeResource` eylem. Her iki filtreleri sağlanan eylem yöntemi düzeyinde uygulandıysa, bu davranış aynı kalır `ShortCircuitingResourceFilter` ilk çalıştı. `ShortCircuitingResourceFilter` Filtre türü nedeniyle veya açık kullanımını tarafından ilk çalışır `Order` özelliği.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Bağımlılık ekleme

Filtre türü veya örnek tarafından eklenebilir. Bir örneği eklerseniz, bu örnek her istek için kullanılır. Bir türü eklerseniz, her istek için bir örneği oluşturulur ve Oluşturucusu bağımlılıkları tarafından doldurulmuş anlamına türü etkinleştirilen, olacaktır [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı). Türe göre bir filtre eklemeden eşdeğerdir `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Öznitelik olarak uygulanır ve doğrudan denetleyicisi sınıfları veya eylem yöntemlerine eklenen filtreleri tarafından sağlanan Oluşturucusu bağımlılıkları olamaz [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı). Bu durum, öznitelik burada uygulanan sağlanan Oluşturucusu parametrelerini olmalıdır çünkü. Bu öznitelikler nasıl işe bir kısıtlamadır.

Filtrelerinizi dı erişmesi gereken bağımlılıkları varsa, desteklenen birçok yaklaşım vardır. Aşağıdakilerden birini kullanarak bir sınıf veya eylem yöntemi için filtre uygulayabilirsiniz:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` özniteliğinizi üzerinde uygulanmadı

> [!NOTE]
> DI almak isteyebilirsiniz bir bağımlılık Günlükçü ' dir. Bununla birlikte, oluşturma ve filtreleri tamamen günlüğe kaydetme amacıyla beri kullanarak kaçının [yerleşik framework günlüğe kaydetme özelliklerini](xref:fundamentals/logging/index) gerekenleri zaten sağlayabilir. Günlüğe kaydetme, filtrelerinizi eklemek için kullanacaksanız, iş etki alanı sorunlarının veya davranışı, filtre yerine MVC eylemler veya diğer framework olayları özgü odaklanmanız gerekir.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` dı filtre örneğini alır. Kapsayıcıda filtre eklemek `ConfigureServices`ve içinde referans bir `ServiceFilter` özniteliği

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Kullanarak `ServiceFilter` bir özel durum filtresi türü sonuçları kaydetme olmadan:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` uygulayan `IFilterFactory`. `IFilterFactory` sunan `CreateInstance` yöntemi oluşturmak için bir `IFilter` örneği. `CreateInstance` Yöntemi (dı) Hizmetleri kapsayıcıdan belirtilen tür yükler.

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` benzer `ServiceFilterAttribute`, ancak türü doğrudan dı kapsayıcıdan çözülmüş değil. Kullanarak türü başlatır `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Bu farklılık nedeniyle:

* Kullanarak başvurulan türleri `TypeFilterAttribute` kapsayıcıyla ilk kayıtlı olması gerekmez.  Bunlar, kapsayıcı tarafından yerine kendi bağımlılıkları vardır. 
* `TypeFilterAttribute` İsteğe bağlı olarak Tür oluşturucu bağımsız değişkenleri kabul edebilir. 

Aşağıdaki örneği kullanarak bir tür bağımsız değişkenleri geçirmek gösterilmiştir `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Bir filtre varsa:

* Tüm bağımsız değişkenleri gerektirmez.
* DI tarafından doldurulması gerekir Oluşturucusu bağımlılıkları vardır.

Sınıflar ve yöntemler yerine adlandırılmış özniteliğinizi kullanabileceğinizi `[TypeFilter(typeof(FilterType))]`). Aşağıdaki filtre bu nasıl uygulanabilir gösterir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Bu filtre sınıfları veya yöntemlerini kullanarak uygulanabilir `[SampleActionFilter]` kullanmak zorunda olmak yerine sözdizimi, `[TypeFilter]` veya `[ServiceFilter]`.

## <a name="authorization-filters"></a>Yetkilendirme filtreleri

* Yetkilendirme filtreleri:
* Eylem yöntemlerine erişimi denetler.
* İçinde filtre ardışık düzeni yürütülecek ilk filtreleri şunlardır. 
* Sahip bir yöntem önce ancak Hayır yöntemi sonra. 

Yalnızca kendi yetkilendirme framework yazıyorsanız, bir özel yetkilendirme filtresi yazmanız gerekir. Yetkilendirme ilkelerini yapılandırma veya bir özel filtre yazma üzerinden özel yetkilendirme ilkesi yazma tercih eder. Yerleşik filtre uygulama yetkilendirme sistemi çağırmak için yalnızca sorumludur.

Hiçbir şey özel işleyecek beri yetkilendirme filtreleri içinde özel durumlar oluşturma döndürmemelidir (özel durum filtreleri çalışmaz işlemek bunları). Özel durum oluştuğunda bir sınama vermeyi düşünün.

Daha fazla bilgi edinmek [yetkilendirme](../../security/authorization/index.md).

## <a name="resource-filters"></a>Kaynak filtreleri

* Ya da uygulama `IResourceFilter` veya `IAsyncResourceFilter` arabirimi
* Kendi yürütme filtre ardışık düzen çoğunu sarmalar. 
* Yalnızca [yetkilendirme filtreleri](#authorization-filters) kaynak filtreleri önce çalışır.

Kaynak filtreleri bir istek yapıyor işlerin çoğunu kısa devre oluşturur faydalıdır. Örneğin, yanıt önbellekte ise bir önbelleğe alma filtre kalan ardışık düzenini önleyebilirsiniz.

[Kısa circuiting kaynak filtresi](#short-circuiting-resource-filter) daha önce gösterilen kaynak filtresi örneğidir. Başka bir örnek [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Model bağlama form verilerini erişmesini engeller. 
* Büyük dosya yüklemeleri için faydalı olur ve belleğe okuma formu önlemek isteyebilirsiniz.

## <a name="action-filters"></a>Eylem filtreleri

*Eylem filtreleri*:

* Ya da uygulama `IActionFilter` veya `IAsyncActionFilter` arabirimi.
* Kendi yürütme eylem yöntemleri yürütme çevreleyen.

Bir örnek eylem filtresi şöyledir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) aşağıdaki özellikleri sağlar:

* `ActionArguments` -Eylem girişleri işlemek olanak tanır.
* `Controller` -denetleyici örneği yönlendirme sağlar. 
* `Result` -Bu ayar, sonraki eylem filtrelerini ve eylem yönteminin yürütülmesi short-circuits. Bir özel durum atma ayrıca sonraki filtreleri ve eylem yönteminin yürütülmesi engeller, ancak başarılı sonuç yerine bir hata olarak kabul edilir.

[ActionExecutedContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) sağlar `Controller` ve `Result` aşağıdaki özellikleri artı:

* `Canceled` -Eylem yürütme başka bir filtre tarafından kısa devre yapılma ise true olur.
* `Exception` -Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa null olmayan olacaktır. Bu özellik etkili bir şekilde null olarak ayarlandığında 'handles' bir özel durum, ve `Result` eylem yönteminden normalde döndürülmedi sanki yürütülür.

İçin bir `IAsyncActionFilter`, çağrı `ActionExecutionDelegate`:

* Bir sonraki eylem filtrelerini ve eylem yöntemini yürütür.
* Döndürür `ActionExecutedContext`. 

Kısa devre oluşturur, Ata için `ActionExecutingContext.Result` bazı sonucu örneği ve çağrısı yok `ActionExecutionDelegate`.

Çerçeve bir Özet sağlar `ActionFilterAttribute` bir alt kümesi olabilir. 

Bir eylem filtresi durumu geçersiz herhangi bir hata döndürür ve model durumunu doğrulamak için kullanabilirsiniz:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Yöntemi çalışır eylem yöntemi ve can sonra görebilir ve eylem sonuçlarını işlemek `ActionExecutedContext.Result` özelliği. `ActionExecutedContext.Canceled` Eylem yürütme başka bir filtre tarafından kısa devre yapılma true değerine ayarlanır. `ActionExecutedContext.Exception` Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayarı `ActionExecutedContext.Exception` null:

* Etkili bir şekilde 'bir özel durum işleme'.
* `ActionExectedContext.Result` Eylem yönteminden normalde döndürülmedi sanki yürütülür.

## <a name="exception-filters"></a>Özel durum filtreleri

*Özel durum filtreleri* ya da uygulama `IExceptionFilter` veya `IAsyncExceptionFilter` arabirimi. Ortak hata işleme için bir uygulama ilkeleri uygulamak için kullanılabilir. 

Aşağıdaki örnek özel durum filtresi, uygulama geliştirme olduğunda oluşan özel durumlar hakkında ayrıntıları görüntülemek için bir özel Geliştirici hata görünümü kullanır:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Özel durum filtreleri:

* Önce ve sonra olayları yok. 
* Uygulama `OnException` veya `OnExceptionAsync`. 
* Denetleyici oluşturulmasında meydana gelen işlenmeyen özel durumları işleme [model bağlama](../models/model-binding.md), eylem filtrelerini ya da eylem yöntemleri. 
* Kaynak filtreleri, sonuç filtreleri veya MVC sonuç yürütme oluşan özel durumları yakalamak değil.

Özel bir durumu işlemek üzere ayarlanmış `ExceptionContext.ExceptionHandled` doğru veya bir yanıt yazma özelliği. Bu özel durumun yayma durdurur. Bir özel durum filtresi bir özel durum "başarılı" içine kapatamazsınız. Yalnızca bir eylem filtresi, bunu yapabilirsiniz.

> [!NOTE]
> Ayarlarsanız ASP.NET Core 1.1 içinde bir yanıt gönderdi değil `ExceptionHandled` true **ve** yanıt yazma. Bu senaryoda, ASP.NET Core 1.0 yanıtı gönder ve ASP.NET Core 1.1.2 1.0 davranışını döndürür. Daha fazla bilgi için bkz: [sorun #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub deposunda. 

Özel durum filtreleri:

* MVC Eylemler içinde gerçekleşir yakalama özel durumlar için iyidir.
* Ara yazılım işleme hatası olarak kadar esnek değildir. 

Ara yazılımı özel durum işleme için tercih edilir. Hata işleme yapmak için özel durum filtreleri yalnızca ihtiyaç duyacağınız kullanın *farklı* MVC eylemi seçildi tabanlı. Örneğin, uygulamanızın eylem yöntemleri görünümler/HTML ve her iki API uç noktaları için olabilir. Görünüm tabanlı eylemler HTML olarak hata sayfasını döndürebilirsiniz sırada API uç noktaları hata bilgisi, JSON olarak döndürebilir.

`ExceptionFilterAttribute` Sınıflandırma. 

## <a name="result-filters"></a>Sonuç filtreleri

* Ya da uygulama `IResultFilter` veya `IAsyncResultFilter` arabirimi.
* Kendi yürütme eylem sonuçlarını yürütülmesi çevreleyen. 

Bir HTTP üstbilgisi ekler bir sonuç filtresi bir örneği burada verilmiştir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Yürütülen sonuç türü, söz konusu eylem bağlıdır. Bir görünüm döndüren bir MVC eylem parçası olarak işleme tüm razor oluşmasıdır `ViewResult` yürütülmekte. Bir API yöntemi, sonuç yürütülmesini bir parçası olarak bazı serileştirme gerçekleştirebilir. Daha fazla bilgi edinmek [eylem sonuçlarını](actions.md)

Eylem veya eylem filtrelerini bir eylem sonucu, sonuç filtreleri yalnızca başarılı sonuç için - çalıştırılır. Özel durum filtreleri bir özel durum uyguluyorsanız sonuç filtreleri yürütülmedi.

`OnResultExecuting` Yöntemi kısa devre oluşturur sonraki sonuç filtreleri ve eylem sonucu yürütülmesi ayarlayarak `ResultExecutingContext.Cancel` true. Genellikle, boş bir yanıt oluşturmamak için kısa devre zaman yanıt nesnesine yazmanız gerekir. Bir özel durum üretiliyor:

* Sonraki filtreleri ve eylem sonucu olarak yürütülmesini engelliyor.
* Başarılı sonuç yerine bir hata olarak kabul edilecek.

Zaman `OnResultExecuted` yöntemi çalıştığında, yanıt istemciye bir olasılıkla gönderilen ve daha fazla (bir özel durum oluştu sürece) değiştirilemez. `ResultExecutedContext.Canceled` Eylem sonucu yürütme başka bir filtre tarafından kısa devre yapılma true değerine ayarlanır.

`ResultExecutedContext.Exception` Eylem sonucu veya bir sonraki sonuç filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayarı `Exception` için null etkili bir şekilde 'bir özel durum işleme' ve MVC tarafından ardışık düzeninde işlenemezse gelen özel durum engeller. Bir sonuç filtresi bir özel durum işlenirken herhangi bir veri yanıtı yazmak mümkün olmayabilir. Eylem sonucu kadar yürütülmesinin oluşturur ve üstbilgileri istemciye zaten atılmış olan, bir hata kodu göndermek için güvenilir bir mekanizma yoktur.

İçin bir `IAsyncResultFilter` yapılan bir çağrı `await next` üzerinde `ResultExecutionDelegate` sonraki sonuç filtreleri ve eylem sonucu yürütür. Kısa devre oluşturur, ayarlamak için `ResultExecutingContext.Cancel` için doğru ve çağrısı yok `ResultExectionDelegate`.

Çerçeve bir Özet sağlar `ResultFilterAttribute` bir alt kümesi olabilir. [AddHeaderAttribute](#add-header-attribute) daha önce gösterilen sınıf bir sonucu filtre özniteliği örneği verilmiştir.

## <a name="using-middleware-in-the-filter-pipeline"></a>Ara yazılım filtre ardışık düzeninde kullanma

Kaynak filtreleri çalışma gibi [ara yazılım](xref:fundamentals/middleware/index) ardışık düzeninde gelen yürütülmesi her şeyi çevreleyen olmasıdır. Ancak MVC bağlamı ve yapılarına erişime sahip oldukları anlamına gelir MVC parçası olup olmadıklarını bakımından filtreler Ara farklıdır.

' De ASP.NET Core 1.1, filtre ardışık düzeninde ara yazılımı kullanabilirsiniz. MVC rota verilerini ya da yalnızca belirli denetleyicileri veya Eylemler çalışması gereken bir erişmesi gereken bir ara yazılım bileşeni varsa, bunu yapmak isteyebilirsiniz.

Ara yazılım bir filtre olarak kullanılacak bir türüyle oluşturma bir `Configure` filtre ardışık düzenine eklemesine istediğiniz ara yazılım belirtir yöntemi. Yerelleştirme ara yazılım bir istek için geçerli kültürün kurmak için kullandığı örnek aşağıda verilmiştir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Daha sonra kullanabilirsiniz `MiddlewareFilterAttribute` seçilen denetleyici veya eylem için ara yazılımı çalıştırmak için veya genel olarak:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Çalışan ara yazılım aynı filtre ardışık düzen aşaması kaynağı olarak, model bağlama önce ve sonra kalan ardışık düzenini filtreleri.

## <a name="next-actions"></a>Sonraki Eylemler

Filtreleri ile denemek için [indirin, test ve örnek değiştirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
