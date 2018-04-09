---
title: ASP.NET Core filtreleri
author: ardalis
description: Filtreler nasıl çalışır ve bunları ASP.NET Core MVC'de nasıl kullanacağınızı öğrenin.
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 7a857bc60500985c9b0547dc0d7e372e987a9099
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core filtreleri

Tarafından [zel Dykstra](https://github.com/tdykstra/) ve [Steve Smith](https://ardalis.com/)

*Filtreler* ASP.NET Core MVC'de, önce veya sonra istek işleme ardışık düzeninde belirli aşamaları kodu çalıştırmanızı sağlar.

> [!IMPORTANT]
> Bu konuda mu **değil** Razor sayfalarına uygulayın. ASP.NET Core 2.1 önizleme ve sonraki destekler `IPageFilter` ve `IAsyncPageFilter` Razor sayfalarının.

 (Bir kullanıcı için yetkili değil kaynaklarına erişimini engelleme) yetkilendirme gibi yerleşik filtreler tanıtıcı görevleri, tüm istekleri HTTPS ve yanıt (önbelleğe alınan yanıt döndürmek için istek ardışık düzenini kısa devre) önbelleğe almayı kullanmak sağlama. 

Uygulamanız için çapraz kesme sorunları işlemek için özel filtreler oluşturabilirsiniz. Kod eylemler arasında çoğaltılmasını önlemek istediğiniz zaman filtreleri çözümdür. Örneğin, bir özel durum filtresi kodda işleme hatası birleştirebilir.

[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Filtreler nasıl çalışır?

Filtreler çalıştırmak içinde *MVC eylemi çağırma ardışık*ve bazen denir *filtre ardışık düzen*.  MVC yürütmek için eylemi seçer sonra filtre ardışık düzen çalışır.

![İstek diğer ara yazılım, yönlendirme ara yazılım, eylem seçimi ve MVC eylemi çağırma ardışık düzeni işlenir. İstek işleme istemciye gönderilen bir yanıt olmadan önce geri eylem seçimi, yönlendirme ara yazılımı ve çeşitli diğer ara yazılımı devam eder.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtre türleri

Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür.

* [Yetkilendirme filtreleri](#authorization-filters) ilk çalıştırma ve geçerli kullanıcının geçerli istek için yetki verilip verilmediğini belirlemek için kullanılır. Bir istek yetkisiz ise bunlar ardışık kısa devre oluşturur. 

* [Kaynak filtreleri](#resource-filters) yetkilendirme sonrasında bir isteği işlemek üzere ilk alan.  Filtre ardışık düzen ve ardışık düzen kalan tamamlandıktan sonra geri kalan önce kodu çalıştırabilirler. Önbelleğe alma nasıl uygulanacağını veya aksi halde filtre ardışık düzen performans nedenleriyle kısa devre oluşturur yararlı oldukları. Model bağlama işleminden önce çalıştırdıkları olduğundan, model bağlama etkilemek için gereken her şey için yararlıdır.

* [Eylem filtreleri](#action-filters) kodu hemen önce ve tek tek eylem yöntemi çağrıldıktan sonra çalıştırabilirsiniz. Bir eyleme geçirilen bağımsız değişkenler ve eylem döndürülen sonuç işlemek için kullanılabilir.

* [Özel durum filtreleri](#exception-filters) herhangi bir şey yanıt gövdesi yazılmadan önce gerçekleşen işlenmeyen özel durumlar genel ilkeleri uygulamak için kullanılır.

* [Sonuç filtreleri](#result-filters) kodu hemen önce ve sonra tek tek eylem sonuçlarını yürütülmesi çalıştırabilirsiniz. Bunlar yalnızca eylem yöntemi başarıyla çalıştırıldığında ve görünüm veya biçimlendirici yürütme çevreleyen gerekir mantığı için kullanışlıdır.

Aşağıdaki diyagramda, bu filtre türleri filtre ardışık düzeninde nasıl etkileşim kurduğu gösterilmektedir.

![Talep yetkilendirme filtreleri, kaynak filtreleri, Model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütme işlenir. Yolda isteği yalnızca sonuç filtreleri ve kaynak filtreleri tarafından istemciye gönderilen bir yanıt olmadan önce işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Uygulama

Filtreler farklı arabirimi tanımları aracılığıyla zaman uyumlu ve uyumsuz uygulamaları destekler. Tür bir görev gerçekleştirmeniz gereken bağlı olarak eşitleme veya zaman uyumsuz VARIANT'ı seçin. 

Çalıştırabilirsiniz zaman uyumlu filtreleri kod, her ikisi de, önce ve bunların ardışık düzen aşaması tanımladıktan sonra*aşama*Executing ve*aşama*yöntemleri yürütülür. Örneğin, `OnActionExecuting` eylem yöntemi çağrılmadan önce çağrılır ve `OnActionExecuted` eylem yöntemine döndürür sonra çağrılır.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?&name=snippet1)]

Zaman uyumsuz filtreleri üzerinde tek bir tanımlamak*aşama*ExecutionAsync yöntemi. Bu yöntem alır bir *FilterType*filtre ardışık düzen aşaması yürüten ExecutionDelegate temsilci. Örneğin, `ActionExecutionDelegate` ve eylem yöntemi yürütebilir çağrıları kod önce ve sonra onu çağırabilir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Tek bir sınıftaki birden çok filtre aşamaları için arabirimleri uygulayabilir. Örneğin, [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) soyut sınıf uygulayan her ikisi de `IActionFilter` ve `IResultFilter`, zaman uyumsuz eşdeğerlerine yanı sıra.

> [!NOTE]
> Uygulama **ya da** zaman uyumlu veya zaman uyumsuz bir filtre arabiriminin sürümünü, her ikisine değil. Framework ilk filtreyi zaman uyumsuz arabirimini uygulayan ve Öyleyse, çağıran bakar. Aksi durumda, zaman uyumlu arabiriminin yöntemleri çağırır. Her iki arabirimde üzerinde bir sınıf uygulama olsaydı, yalnızca zaman uyumsuz yöntem çağrılması. Soyut sınıflar gibi kullanırken [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) yalnızca zaman uyumlu yöntemleri veya zaman uyumsuz yöntem her filtre türü için geçersiz kılarsınız.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` uygulayan `IFilter`. Bu nedenle, bir `IFilterFactory` örneği olarak kullanılabilir bir `IFilter` filtre ardışık düzen başka bir yerindeki örneği. Framework filtre çağırmak hazırlarken, hangisine yayınlayacağınızı çalışır bir `IFilterFactory`. Bu atama başarılı olursa, `CreateInstance` yöntemi oluşturmak için çağrılır `IFilter` çağrılan örnek. Bu, kesin filtre ardışık düzen uygulama başladığında açıkça ayarlanması gerekmez bu yana çok esnek bir tasarım sağlar.

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

Bir filtre ardışık düzen üç birinde eklenebilir *kapsamları*. Bir öznitelik kullanarak, belirli bir eylem yönteminin ya da bir denetleyici sınıfı bir filtre ekleyebilirsiniz. Veya ekleyerek genel (tüm denetleyicileri ve eylemleri) için bir filtre kaydedebilirsiniz `MvcOptions.Filters` koleksiyonunda `ConfigureServices` yönteminde `Startup` sınıfı:

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

Bu yöntem filtresi denetleyicisi filtre içinde iç içe geçmiş ve denetleyici filtreyi genel filtre içinde iç içe gösterilir. Zaman uyumsuz filtre içinde iseniz koymak istediğiniz başka bir yolu, kullanıcının*aşama*ExecutionAsync yöntemi, tüm filtreleri daha sıkı bir kapsamla çalıştırmak kodunuzu yığında olsa.

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

`Order` Filtreleri çalıştırılacağı sipariş belirlerken özelliği toplayın kapsamı. Filtreler önce sıraya göre sıralanır ve ardından kapsamı TIES ayırmak için kullanılır. Tüm yerleşik filtre uygulamak `IOrderedFilter` ve varsayılan `Order` ayarladığınız sürece kapsam sırasını belirler. böylece 0 değerine `Order` için sıfır olmayan bir değer.

## <a name="cancellation-and-short-circuiting"></a>İptal etme ve kestirmeler

Herhangi bir noktada filtre ardışık düzen ayarlayarak kısa devre oluşturur `Result` özellikte `context` filtre yönteme sağlanan parametre. Örneğin, aşağıdaki kaynak filtresi kalan ardışık düzenini yürütülmesini engeller.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` ve `AddHeader` filtre hedef `SomeResource` eylem yöntemi. Ancak, çünkü `ShortCircuitingResourceFilter` ilk çalışır (kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi) ve kalan ardışık düzenini short-circuits `AddHeader` hiçbir zaman filtresi çalıştıran `SomeResource` eylem. Her iki filtreleri sağlanan eylem yöntemi düzeyinde uygulandıysa, bu davranış aynı kalır `ShortCircuitingResourceFilter` ilk çalışan (kendi filtre türü nedeniyle veya açık kullanımını `Order` özelliği örneği için).

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

`ServiceFilterAttribute` uygulayan `IFilterFactory`, oluşturmak için tek bir yöntem kullanıma sunan bir `IFilter` örneği. Durumunda `ServiceFilterAttribute`, `IFilterFactory` arabiriminin `CreateInstance` yöntemi, belirtilen tür hizmet kapsayıcı (dı) yüklemek için uygulanır.

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` çok benzer `ServiceFilterAttribute` (ve ayrıca uygulayan `IFilterFactory`), ancak türü doğrudan dı kapsayıcıdan çözülmüş değil. Bunun yerine, kullanarak türü başlatır `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Kullanarak başvurulan türleri bu farklılık nedeniyle `TypeFilterAttribute` kapsayıcıyla ilk kayıtlı olması gerekmez (ancak kapsayıcı yerine bağımlılıklarını çözümlenmedi). Ayrıca, `TypeFilterAttribute` isteğe bağlı olarak söz konusu türü için oluşturucu bağımsız değişkenleri kabul edebilir. Aşağıdaki örneği kullanarak bir tür bağımsız değişkenleri geçirmek gösterilmiştir `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Herhangi bir bağımsız değişken gerektirmeyen bir filtreye sahip ancak dı tarafından doldurulması gerekir Oluşturucusu bağımlılıkları olan, sınıflar ve yöntemler yerine adlandırılmış özniteliğinizi kullanabilirsiniz `[TypeFilter(typeof(FilterType))]`). Aşağıdaki filtre bu nasıl uygulanabilir gösterir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Bu filtre sınıfları veya yöntemlerini kullanarak uygulanabilir `[SampleActionFilter]` kullanmak zorunda olmak yerine sözdizimi, `[TypeFilter]` veya `[ServiceFilter]`.

## <a name="authorization-filters"></a>Yetkilendirme filtreleri

*Yetkilendirme filtreleri* eylem yöntemlerine erişimi denetlemek ve içinde filtre ardışık düzeni yürütülecek ilk filtreler. Yalnızca sahip oldukları bir yöntemi, önce ve sonra yöntemleri destek filtrelerin çoğu aksine önce. Yalnızca kendi yetkilendirme framework yazıyorsanız, bir özel yetkilendirme filtresi yazmanız gerekir. Yetkilendirme ilkelerini yapılandırma veya bir özel filtre yazma üzerinden özel yetkilendirme ilkesi yazma tercih eder. Yerleşik filtre uygulama yetkilendirme sistemi çağırmak için yalnızca sorumludur.

Hiçbir şey özel işleyecek beri yetkilendirme filtreleri içinde özel durumlar oluşturma döndürmemelidir unutmayın (özel durum filtreleri çalışmaz işlemek bunları). Bunun yerine, bir sınama vermek veya başka bir yolu bulunamıyor.

Daha fazla bilgi edinmek [yetkilendirme](../../security/authorization/index.md).

## <a name="resource-filters"></a>Kaynak filtreleri

*Kaynak filtreleri* ya da uygulama `IResourceFilter` veya `IAsyncResourceFilter` arabirimi ve bunların yürütme filtre ardışık düzen çoğunu sarmalar. (Yalnızca [yetkilendirme filtreleri](#authorization-filters) daha önce çalıştırın.) Kaynak filtreleri bir istek yapıyor işlerin çoğunu kısa devre oluşturur gerekiyorsa özellikle yararlı olur. Örneğin, yanıt önbellekte ise bir önbelleğe alma filtre kalan ardışık düzenini önleyebilirsiniz.

[Kısa circuiting kaynak filtresi](#short-circuiting-resource-filter) daha önce gösterilen kaynak filtresi örneğidir. Başka bir örnek [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), model bağlama form verilerini erişmesini engeller. Burada büyük dosya yüklemeleri almak ve belleğe okuma formu engellemek istediğiniz için duyacağınızı biliyorsanız durumlarda yararlıdır.

## <a name="action-filters"></a>Eylem filtreleri

*Eylem filtreleri* ya da uygulama `IActionFilter` veya `IAsyncActionFilter` arabirimi ve bunların yürütme çevreleyen eylem yöntemleri yürütme.

Bir örnek eylem filtresi şöyledir:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) aşağıdaki özellikleri sağlar:

* `ActionArguments` -Eylem girişleri işlemek olanak tanır.
* `Controller` -denetleyici örneği yönlendirme sağlar. 
* `Result` -Bu ayar, sonraki eylem filtrelerini ve eylem yönteminin yürütülmesi short-circuits. Bir özel durum atma ayrıca sonraki filtreleri ve eylem yönteminin yürütülmesi engeller, ancak başarılı sonuç yerine bir hata olarak kabul edilir.

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) sağlar `Controller` ve `Result` aşağıdaki özellikleri artı:

* `Canceled` -Eylem yürütme başka bir filtre tarafından kısa devre yapılma ise true olur.
* `Exception` -Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa null olmayan olacaktır. Bu özellik etkili bir şekilde null olarak ayarlandığında 'handles' bir özel durum, ve `Result` eylem yönteminden normalde döndürülmedi sanki yürütülür.

İçin bir `IAsyncActionFilter`, çağrı `ActionExecutionDelegate` sonraki eylem filtreleri ve döndüren eylem yöntemini yürütür bir `ActionExecutedContext`. Kısa devre oluşturur, Ata için `ActionExecutingContext.Result` bazı sonucu örneği ve çağrısı yok `ActionExecutionDelegate`.

Çerçeve bir Özet sağlar `ActionFilterAttribute` bir alt kümesi olabilir. 

Otomatik olarak model durumunu doğrulamak ve herhangi bir hata durumu geçersizse dönmek için bir eylem filtresi kullanabilirsiniz:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Yöntemi çalışır eylem yöntemi ve can sonra görebilir ve eylem sonuçlarını işlemek `ActionExecutedContext.Result` özelliği. `ActionExecutedContext.Canceled` Eylem yürütme başka bir filtre tarafından kısa devre yapılma true değerine ayarlanır. `ActionExecutedContext.Exception` Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayarı `ActionExecutedContext.Exception` etkili bir şekilde null olarak 'bir özel durum işleme' ve `ActionExectedContext.Result` eylem yönteminden normalde döndürülmedi gibi daha sonra yürütülür.

## <a name="exception-filters"></a>Özel durum filtreleri

*Özel durum filtreleri* ya da uygulama `IExceptionFilter` veya `IAsyncExceptionFilter` arabirimi. Ortak hata işleme için bir uygulama ilkeleri uygulamak için kullanılabilir. 

Aşağıdaki örnek özel durum filtresi, uygulama geliştirme olduğunda oluşan özel durumlar hakkında ayrıntıları görüntülemek için bir özel Geliştirici hata görünümü kullanır:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

(Önce ve sonra), özel durum filtreleri iki olay sahip değilseniz - yalnızca uyguladıkları `OnException` (veya `OnExceptionAsync`). 

Özel durum filtreleri denetleyicisi oluşturulmasında meydana gelen işlenmeyen özel durumları işleme [model bağlama](../models/model-binding.md), eylem filtrelerini ya da eylem yöntemleri. Bunlar, kaynak filtreleri, sonuç filtreleri veya MVC sonuç yürütme oluşan özel durumları yakalamak olmaz.

Özel bir durumu işlemek üzere ayarlanmış `ExceptionContext.ExceptionHandled` doğru veya bir yanıt yazma özelliği. Bu özel durumun yayma durdurur. Bir özel durum filtresi bir özel durum "başarılı" içine etkinleştiremiyor unutmayın. Yalnızca bir eylem filtresi, bunu yapabilirsiniz.

> [!NOTE]
> Ayarlarsanız ASP.NET Core 1.1 içinde bir yanıt gönderdi değil `ExceptionHandled` true **ve** yanıt yazma. Bu senaryoda, ASP.NET Core 1.0 yanıtı gönder ve ASP.NET Core 1.1.2 1.0 davranışını döndürür. Daha fazla bilgi için bkz: [sorun #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub deposunda. 

Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir. Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı. Örneğin, uygulamanızın eylem yöntemleri görünümler/HTML ve her iki API uç noktaları için olabilir. Görünüm tabanlı eylemler HTML olarak hata sayfasını döndürebilirsiniz sırada API uç noktaları hata bilgisi, JSON olarak döndürebilir.

Çerçeve bir Özet sağlar `ExceptionFilterAttribute` bir alt kümesi olabilir. 

## <a name="result-filters"></a>Sonuç filtreleri

*Sonuç filtreleri* ya da uygulama `IResultFilter` veya `IAsyncResultFilter` arabirimi ve bunların yürütme çevreleyen eylem sonuçlarını yürütme. 

Bir HTTP üstbilgisi ekler bir sonuç filtresi bir örneği burada verilmiştir.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Yürütülen sonuç türü, söz konusu eylem bağlıdır. Bir görünüm döndüren bir MVC eylem parçası olarak işleme tüm razor oluşmasıdır `ViewResult` yürütülmekte. Bir API yöntemi, sonuç yürütülmesini bir parçası olarak bazı serileştirme gerçekleştirebilir. Daha fazla bilgi edinmek [eylem sonuçlarını](actions.md)

Eylem veya eylem filtrelerini bir eylem sonucu, sonuç filtreleri yalnızca başarılı sonuç için - çalıştırılır. Özel durum filtreleri bir özel durum uyguluyorsanız sonuç filtreleri yürütülmedi.

`OnResultExecuting` Yöntemi kısa devre oluşturur sonraki sonuç filtreleri ve eylem sonucu yürütülmesi ayarlayarak `ResultExecutingContext.Cancel` true. Genellikle, boş bir yanıt oluşturmamak için kısa devre zaman yanıt nesnesine yazmanız gerekir. Bir özel durum atma sonraki filtreleri ve eylem sonucu yürütülmesi da engeller, ancak başarılı sonuç yerine bir hata olarak kabul edilir.

Zaman `OnResultExecuted` yöntemi çalıştığında, yanıt istemciye bir olasılıkla gönderilen ve daha fazla (bir özel durum oluştu sürece) değiştirilemez. `ResultExecutedContext.Canceled` Eylem sonucu yürütme başka bir filtre tarafından kısa devre yapılma true değerine ayarlanır.

`ResultExecutedContext.Exception` Eylem sonucu veya bir sonraki sonuç filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır. Ayarı `Exception` için null etkili bir şekilde 'bir özel durum işleme' ve MVC tarafından ardışık düzeninde işlenemezse gelen özel durum engeller. Bir sonuç filtresi bir özel durum işlenirken herhangi bir veri yanıtı yazmak mümkün olmayabilir. Eylem sonucu kadar yürütülmesinin oluşturur ve üstbilgileri istemciye zaten atılmış olan, bir hata kodu göndermek için güvenilir bir mekanizma yoktur.

İçin bir `IAsyncResultFilter` yapılan bir çağrı `await next()` üzerinde `ResultExecutionDelegate` sonraki sonuç filtreleri ve eylem sonucu yürütür. Kısa devre oluşturur, ayarlamak için `ResultExecutingContext.Cancel` için doğru ve çağrısı yok `ResultExectionDelegate`.

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
