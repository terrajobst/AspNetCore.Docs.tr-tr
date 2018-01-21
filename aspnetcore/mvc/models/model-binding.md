---
title: Model Binding
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 84b9c5dc3a87b739affaeaecaa180d1b01f49b8e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="model-binding"></a>Model Binding

Tarafından [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Model bağlama için giriş

Model bağlama ASP.NET Core mvc'de HTTP isteklerini verilerden eylem yöntemi parametrelerine eşler. Parametreleri dize, tamsayı veya float gibi basit türler olabilir veya karmaşık türler olabilir. Gelen veriler için karşılık gelen eşleme boyutu veya verilerin karmaşıklığına bağımsız olarak sık tekrarlanan bir senaryo harika bir özelliği olan MVC olmasıdır. MVC, geliştiricilerin her uygulamanın aynı kodda biraz farklı bir sürümünü yeniden yazma işlemi tutmak zorunda kalmamak için bağlama hemen özetleyen tarafından bu sorunu çözer. Tür dönüştürücü kodu için kendi metin yazma can sıkıcı ve hataya.

## <a name="how-model-binding-works"></a>Model bağlama nasıl çalışır?

MVC bir HTTP isteği aldığında, bir denetleyici belirli eylem yöntemine yönlendirir. Rota verileri nedir tabanlı çalıştırmak için hangi eylemini yöntemi belirler ve ardından bu değerleri HTTP isteğinden Bu eylem yönteminin parametreleri bağlar. Örneğin, aşağıdaki URL'yi göz önünde bulundurun:

`http://contoso.com/movies/edit/2`

Rota şablonu şu şekilde baktığı `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` yönlendirir `Movies` denetleyicisi ve onun `Edit` eylem yöntemi. Ayrıca adlı isteğe bağlı bir parametre kabul eden `id`. Eylem yöntemi için kod aşağıdakine benzer görünmelidir:

```csharp
public IActionResult Edit(int? id)
   ```

Not: URL rota dizelerde büyük küçük harfe duyarlı değildir.

MVC istek verileri için eylem parametrelerini adıyla bağlamak çalışacaktır. MVC parametre adı ve ortak ayarlanabilir özelliklerini adlarını kullanarak her parametre için değer arar. Yukarıdaki örnekte, yalnızca eylem parametresi adlı `id`, hangi MVC rota değerleri aynı adla değeri için bağlar. Rota değerleri yanı sıra MVC istek çeşitli parçalarını veri bağlar ve bunu bir kümesini sırayla yapar. Model bağlama aralarında arar sırada veri kaynaklarının listesi aşağıdadır:

1. `Form values`: POST yöntemini kullanan HTTP isteğinde Git form değerleri şunlardır. (jQuery POST istekleri dahil olmak üzere).

2. `Route values`: Tarafından sağlanan rota değerleri kümesi [yönlendirme](../../fundamentals/routing.md)

3. `Query strings`: URI sorgu dizesi bölümü.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Not: Form değerleri, rota verilerini ve sorgu dizeleri tüm ad-değer çiftleri olarak depolanır.

Model bağlama adlı bir anahtar için sorunuz `id` ve hiçbir şey adlı `id` form değerleri için bu anahtar arayan rota değerleri için taşındıktan. Bizim örneğimizde, onu bir eşleşmedir. Bağlama olur ve değer 2 tamsayıya dönüştürülür. Düzenleme (dize kimliği) kullanarak aynı istekte "2" dizeye dönüştürme.

Şu ana kadar örnek basit türler kullanır. MVC'de basit herhangi bir .NET ilkel tür veya türü bir dize türü dönüştürücü ile türleridir. Eylem yönteminin parametre gibi bir sınıf olsaydı `Movie` özellikleri, MVC'ın model bağlama hala bu sorunsuz şekilde işlemek gibi basit ve karmaşık türler içeren türü. Karmaşık türler için eşleşen arama özelliklerini gezinmesine yansıma ve özyineleme kullanır. Model bağlama arar desenini *parameter_name.property_name* özelliklerine değerler bağlamak için. Bu formun eşleşen değerleri bulamazsa, yalnızca özellik adı kullanılarak bağlama dener. Bu türleri gibi `Collection` türleri, model bağlama eşleşmeler arar *parametre_adý [dizin]* veya yalnızca *[dizin]*. Model bağlama davranır `Dictionary` benzer şekilde, sorarak türleri *parametre_adý [anahtarı]* veya yalnızca *[anahtarı]*, basit türler anahtarları olduğu sürece. Desteklenen anahtarlarının aynı model türü için oluşturulan etiket yardımcıları ve alan adları HTML eşleşmesi. Böylece form alanlarını Örneğin, oluşturma veya düzenleme ilişkili verileri doğrulamayı geçemedi zaman kendi kolaylık sağlamak için kullanıcının girdisi ile doldurulmuş kalır. Bu gidiş değerleri sağlar.

Olmasını bağlama için sırayla sınıfı ortak varsayılan bir oluşturucu olmalıdır ve ortak yazılabilir özelliklerini bağlanması için üye olmanız gerekir. Model bağlama sınıfı yalnızca ortak varsayılan oluşturucu kullanılarak oluşturulacak gerçekleştiğinde, özellikler ayarlanabilir.

Bir parametre bağlandığında, bu adı taşıyan değerleri arayan model bağlama durdurur ve sonraki parametrenin bağlamak taşır. Aksi takdirde, varsayılan model bağlama davranışı parametrelerini varsayılan değerlerine kendi türüne bağlı olarak ayarlar:

* `T[]`: Sistem, türlerinden diziler `byte[]`, bağlama türü parametrelerinin ayarlar `T[]` için `Array.Empty<T>()`. Dizi türü `byte[]` ayarlanır `null`.

* Başvuru türleri: Bağlama bir sınıfının bir örneğini varsayılan kurucu ile özellikleri ayarlamadan oluşturur. Ancak, bağlama ayarlar'ı model `string` parametreleri `null`.

* Boş değer atanabilir türler: Boş değer atanabilir türler ayarlamak `null`. Yukarıdaki örnekte, model bağlama kümeleri `id` için `null` türü olduğundan `int?`.

* Değer türleri: Atanamayan değer türleri türü `T` ayarlanır `default(T)`. Örneğin, model bağlama parametresi ayarlar `int id` 0. Model doğrulama veya boş değer atanabilir türler yerine varsayılan değerlerine bağlı göz önünde bulundurun.

Bağlama başarısız olur, MVC bir hata durum değil. Kullanıcı girişi kabul eden her eylem denetlemelisiniz `ModelState.IsValid` özelliği.

Not: Her giriş denetleyicinin `ModelState` özelliği bir `ModelStateEntry` içeren bir `Errors` özelliği. Bu koleksiyon kendiniz sorgu nadiren gereklidir. Bunun yerine `ModelState.IsValid` kullanın.

Ayrıca, MVC, model bağlama gerçekleştirirken dikkate almanız gereken bazı özel veri türü vardır:

* `IFormFile`, `IEnumerable<IFormFile>`: HTTP isteğinin bir parçası olan bir veya daha fazla karşıya yüklenen dosyalar.

* `CancellationToken`: Zaman uyumsuz denetleyicileri etkinliğinde iptal etmek için kullanılır.

Bu tür bir sınıf türü üzerinde eylem parametrelerini veya özellikler bağlanabilir.

Model bağlama tamamlandıktan sonra [doğrulama](validation.md) oluşur. Varsayılan model bağlama geliştirme senaryolarını çoğunluğu harika çalışır. Benzersiz gereksinimlere sahip değilse, yerleşik davranışını özelleştirebilirsiniz şekilde da genişletilebilir.

## <a name="customize-model-binding-behavior-with-attributes"></a>Model bağlama davranışı öznitelikleri olan özelleştirme

MVC kendi varsayılan model bağlama davranışı farklı bir kaynak için doğrudan için kullanabileceğiniz birkaç öznitelik içerir. Örneğin, bağlama için bir özellik gerekli olup olmadığını ya da onu hiç kullanarak asla olmamalıdır belirtebilirsiniz `[BindRequired]` veya `[BindNever]` öznitelikleri. Alternatif olarak, varsayılan veri kaynağı geçersiz kılmak ve model Bağlayıcısı'nın veri kaynağını belirtin. Model bağlama öznitelikler listesi aşağıdadır:

* `[BindRequired]`: Bağlama gerçekleşemez, model durum hatası Bu öznitelik ekler.

* `[BindNever]`: Hiçbir zaman bu parametreye bağlamak için model bağlayıcı söyler.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Bunlar, uygulamak istediğiniz tam bağlama kaynağı belirtmek için kullanın.

* `[FromServices]`: Bu özniteliği kullanıyor [bağımlılık ekleme](../../fundamentals/dependency-injection.md) Hizmetleri'nden parametreleri bağlamak için.

* `[FromBody]`: Veri isteği gövdesinden bağlamak için yapılandırılmış biçimlendiricileri kullanın. Biçimlendiriciyi isteğin içerik türüne göre seçilir.

* `[ModelBinder]`: Varsayılan model bağlayıcısını ve bağlama kaynağı adı geçersiz kılmak için kullanılır.

Model bağlama varsayılan davranışını geçersiz kılmak gerektiğinde yararlı Araçlar öznitelikleridir.

## <a name="binding-formatted-data-from-the-request-body"></a>İstek gövdesini biçimlendirilmiş bağlama verileri

İstek veri biçimleri JSON, XML ve diğer birçok dahil olmak üzere çeşitli gelebilir. Veri istek gövdesindeki bir parametre bağlamak istediğiniz belirtmek için [FromBody] özniteliği kullandığınızda, MVC biçimlendiricileri yapılandırılmış bir dizi içerik türüne göre istek verileri işlemek için kullanır. Varsayılan olarak MVC içeren bir `JsonInputFormatter` JSON verilerini, ancak işleme ek biçimlendiricileri XML ve diğer özel biçimler işlemek için ekleyebilirsiniz için sınıf.

> [!NOTE]
> Olabilir en çok bir parametre ile donatılmış eylem başına `[FromBody]`. ASP.NET Core MVC çalışma zamanı biçimlendirici için istek akışı okuma sorumluluğunu atar. İstek akışı için bir parametre okuma sonra genellikle diğer bağlama için tekrar istek akışı okunamıyor yoktur `[FromBody]` parametreleri.

> [!NOTE]
> `JsonInputFormatter` Temel alır ve varsayılan biçimlendiricidir [Json.NET](https://www.newtonsoft.com/json).

ASP.NET seçer göre giriş biçimlendiricileri [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) üstbilgi ve parametrenin türü olmadığı sürece, aksi takdirde belirtme uygulanan bir öznitelik. XML kullanmak istediğiniz ya da başka bir biçime içinde yapılandırmalısınız *haline* dosyası, ancak öncelikle sahip bir başvuru almak `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet kullanma. Başlangıç kodu aşağıdakine benzer görünmelidir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Kod *haline* dosyasını içeren bir `ConfigureServices` yöntemi ile bir `services` bağımsız değişkeni ASP.NET uygulamanız için hizmetleri oluşturmak için kullanabilirsiniz. Örnekte, MVC için bu uygulamayı sağlayacak bir hizmet olarak bir XML biçimlendiricisi ekliyoruz. `options` Bağımsız değişken geçirildi `AddMvc` yöntemi ekleyin ve filtreleri, biçimlendiricileri ve diğer sistem seçenekleri MVC uygulama başlatma sırasında yönetmenize olanak sağlar. Daha sonra uygulamanız `Consumes` özniteliği denetleyicisi sınıfları veya istediğiniz biçimi ile çalışmak için eylem yöntemleri.

### <a name="custom-model-binding"></a>Özel Model bağlama

Model bağlama kendi özel model bağlayıcıları yazarak genişletebilirsiniz. Daha fazla bilgi edinmek [özel model bağlama](../advanced/custom-model-binding.md).
