---
title: ASP.NET core'da model bağlama
author: tdykstra
description: ASP.NET Core MVC, model bağlama HTTP isteklerinden alınan verileri olarak eylem metodu parametreleriyle nasıl eşlendiğini öğrenin.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 11/13/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 1dc9b41328ed78440622acc1865b6f088d394403
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898385"
---
# <a name="model-binding-in-aspnet-core"></a>ASP.NET core'da model bağlama

Tarafından [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Model bağlama için giriş

ASP.NET Core MVC, model bağlama, HTTP isteklerinden alınan verileri olarak eylem metodu parametreleriyle eşleştirir. Dize, tamsayı veya float gibi basit türler parametreleri olabilir veya karmaşık türler olması olabilir. Gelen veri karşılık gelen bir eşleme boyut ve karmaşıklıktaki verilerin ne olursa olsun, sık sık tekrarlanan bir senaryo MVC harika bir özelliği olmasıdır. MVC, geliştiricilerin biraz farklı bir sürümünü her uygulamada aynı kodun yeniden korumak zorunda kalmamak için bağlama hemen özetleyen tarafından bu sorunu çözer. Dönüştürücü kod yazmak için kendi metin yazma tedious ve hataya açık alanlardır.

## <a name="how-model-binding-works"></a>Model bağlama nasıl çalışır?

MVC bir HTTP isteği aldığında, bu etki alanı denetleyicisinin bir özel eylem yöntemine yönlendirir. Rota verileri nedir göre çalıştırmak için hangi eylem yöntemini belirler, ardından bu değerleri HTTP isteğinden, eylem yönteminin parametreleri bağlar. Örneğin, aşağıdaki URL'ye göz önünde bulundurun:

`http://contoso.com/movies/edit/2`

Rota şablonu şu şekilde baktığı `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` yönlendirir `Movies` denetleyicisi ve onun `Edit` eylem yöntemi. Ayrıca adlı isteğe bağlı bir parametreyi kabul eden `id`. Eylem yöntemi için kod aşağıdaki gibi görünmelidir:

```csharp
public IActionResult Edit(int? id)
   ```

Not: URL rota dizeler büyük/küçük harfe duyarlı değildir.

MVC istek verileri adına göre eylem parametrelerine bağlamak çalışacaktır. Parametre adı ve ortak, ayarlanabilir özelliklerin adlarını kullanarak her parametre için değer MVC arar. Yukarıdaki örnekte, tek bir eylem parametresinin adlı `id`, hangi MVC rota değerleri, aynı ada sahip bir değere bağlar. Ek olarak, rota değerleri MVC çeşitli bölümlerini bir istek veri bağlar ve bunu bir kümesi sırayla yapar. Model bağlama aracılığıyla görünen sırada veri kaynaklarının bir listesi aşağıdadır:

1. `Form values`: POST yöntemi kullanarak HTTP isteğinde Git form değerleri şunlardır. (jQuery POST istekleri dahil olmak üzere).

2. `Route values`: Rota değerleri tarafından sağlanan dizi [yönlendirme](xref:fundamentals/routing)

3. `Query strings`: Sorgu dizesi URI parçası.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Not: Form değerleri, rota verilerini ve sorgu dizeleri tüm ad-değer çiftleri olarak depolanır.

Model bağlama için adlı bir anahtar sorunuz `id` ve bir şey yok adlı `id` form değerleri, rota değerleri için anahtar aramak için taşınması. Bu örnekte, bir eşleşmedir. Bağlamadan ve 2 tamsayıya dönüştürülen değeri. Düzenle (dize kimliği) kullanarak aynı istekte, "2" dizeye dönüştürecektir.

Şu ana kadar örnek, basit türler kullanır. MVC'de basit herhangi bir .NET basit türü veya dize türü dönüştürücü türüyle türleridir. Eylem yönteminin parametresi gibi bir sınıf olsaydı `Movie` hem basit hem de karmaşık türleri gibi özellikleri, MVC'ın model bağlama devam eder, düzgün şekilde işleyen içeren türü. Karmaşık türler için eşleşme arama özelliklerini geçirmek için yansıma ve özyineleme kullanır. Model bağlama desenini arar *parameter_name.property_name* özellikleri değerlerini bağlamak için. Eşleşen değerleri bu formu bulamazsa, bağlama yalnızca özellik adı kullanılarak dener. Bu türleri gibi `Collection` türleri, model bağlama için bir eşleşme arar *parameter_name [dizin]* veya yalnızca *[dizin]*. Model bağlama davranır `Dictionary` benzer şekilde, sorarak türleri *parameter_name [anahtarı]* veya yalnızca *[anahtarı]*, basit türler anahtarları olduğu sürece. Desteklenen anahtarlar aynı model türü için oluşturulan etiket yardımcıları ve HTML alan adları aynı. Bu, form alanlarını, oluşturma veya düzenleme ilişkili verileri doğrulama başarılı olmadı olduğunda, kolaylık sağlamak için kullanıcının girişi ile doldurulmuş kalır. böylece gidiş dönüşü değerleri sağlar.

Model bağlama mümkün kılmak için sınıf, genel bir varsayılan oluşturucu ve bağlamak için genel yazılabilir özellikleri olmalıdır. Model bağlama oluştuğunda genel varsayılan oluşturucu kullanılarak sınıfının örneği ve özellikleri ayarlayabilirsiniz.

Bir parametre bağlandığında, model bağlama değerleri bu ada sahip mi arıyorsunuz durdurur ve bir sonraki parametreyi bağlamak taşır. Aksi takdirde, varsayılan model bağlama davranışı parametrelerini varsayılan değerlerine kendi türüne bağlı olarak ayarlar:

* `T[]`: Türü dizilerin dışında `byte[]`, bağlama türü parametreleri ayarlar `T[]` için `Array.Empty<T>()`. Tür dizilerini `byte[]` ayarlandığından `null`.

* Başvuru türleri: Bağlama, özellikleri ayarlamadan varsayılan oluşturucu ile bir sınıfın bir örneğini oluşturur. Ancak, bağlama kümeleri model `string` parametreleri `null`.

* Boş değer atanabilir türler: Boş değer atanabilir türler ayarlandığında `null`. Yukarıdaki örnekte, model bağlama kümeleri `id` için `null` türü olduğundan `int?`.

* Değer türleri: Türü olmayan boş değer atanabilir değer türlerinin `T` ayarlandığından `default(T)`. Örneğin, model bağlama parametresi ayarlayacak `int id` 0. Model doğrulama veya boş değer atanabilir türler kullanmak yerine varsayılan değerlerine bağlı olan göz önünde bulundurun.

Başarısız bağlanıyorsanız, MVC bir hata oluşturmaz. Kullanıcı girişi kabul eden her eylem denetlemelisiniz `ModelState.IsValid` özelliği.

Not: Denetleyicinin her giriş `ModelState` özelliği bir `ModelStateEntry` içeren bir `Errors` özelliği. Bu koleksiyonu kendiniz sorgulamak nadiren gereklidir. Bunun yerine `ModelState.IsValid` kullanın.

Buna ek olarak, MVC, model bağlama yapılırken dikkate almanız gereken bazı özel veri türleri vardır:

* `IFormFile`, `IEnumerable<IFormFile>`: HTTP isteği bir parçası olan bir veya daha fazla yüklenen dosyalar.

* `CancellationToken`: Zaman uyumsuz denetleyicileri etkinliğinde iptal etmek için kullanılır.

Bu tür bir sınıf türüne bağlı eylem parametrelerini veya özellikler olabilir.

Model bağlama tamamlandıktan sonra [doğrulama](validation.md) gerçekleşir. Varsayılan model bağlama geliştirme senaryoları büyük çoğunluğu için çok iyi çalışır. Benzersiz gereksinimleriniz varsa, yerleşik davranışına göre özelleştirme olanağı da genişletilebilir.

## <a name="customize-model-binding-behavior-with-attributes"></a>Model bağlama davranışı öznitelikleri ile özelleştirme

MVC, farklı bir kaynak için varsayılan model bağlama davranışını yönlendirmek için kullanabileceğiniz çeşitli özniteliklerini içerir. Örneğin, bağlama özelliği için gerekli olup olmadığını ya da, hiçbir zaman hiç kullanarak olacağını belirtebilirsiniz `[BindRequired]` veya `[BindNever]` öznitelikleri. Alternatif olarak, varsayılan veri kaynağı geçersiz kılar ve model Bağlayıcısı'nın veri kaynağını belirtin. Model bağlama özniteliklerin bir listesi aşağıdadır:

* `[BindRequired]`: Bu öznitelik, bağlama gerçekleşemez, model durumu hatası ekler.

* `[BindNever]`: Bu parametre için hiçbir zaman bağlamak için model bağlayıcı söyler.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Uygulamak istediğiniz tam bağlama kaynağı belirtme için bunları kullanın.

* `[FromServices]`: Bu özniteliği kullanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) hizmetlerinden parametleri bağlamak için.

* `[FromBody]`: Gövdeden veri bağlamak için yapılandırılan biçimlendiricileri kullanın. Biçimlendiriciyi isteğin içerik türüne göre seçilir.

* `[ModelBinder]`: Kaynak ve ad bağlama varsayılan model bağlayıcısını geçersiz kılmak için kullanılır.

Model bağlama'nın varsayılan davranışını geçersiz kılmanız gerekiyorsa öznitelikleri çok yararlı araçlardır.

## <a name="customize-model-binding-and-validation-globally"></a>Model bağlama ve doğrulama genel özelleştirme

Model bağlama ve doğrulama sistemin davranış tarafından yönlendirilen [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) açıklayan:

* Nasıl bir model bağlı olmasını sağlamaktır.
* Ne tür ve özelliklerini doğrulama gerçekleşir.

Sistemin davranış yönlerini yapılandırılabilir genel ayrıntıları sağlayıcıya ekleyerek [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders). MVC, model bağlama veya doğrulama belirli devre dışı bırakma gibi böyle davranışını yapılandırma türleri sağlayan birkaç yerleşik ayrıntıları sağlayıcıları vardır.

Belirli bir türdeki tüm modeller üzerinde model bağlama devre dışı bırakmak için ekleme bir [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider) içinde `Startup.ConfigureServices`. Örneğin, devre dışı bırakma türü tüm modeller üzerinde model bağlama için `System.Version`:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

Belirli bir tür özelliklerini doğrulamasını devre dışı bırakmak için ekleme bir [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider) içinde `Startup.ConfigureServices`. Örneğin, türün özelliklerini doğrulama devre dışı bırakmak için `System.Guid`:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a>Gövdeden biçimlendirilmiş veri bağlama

İstek verileri bir çeşitli biçimlerde JSON, XML ve diğer birçok gibi gelebilir. İstek gövdesinde veriye parametre bağlama istediğinizi belirtmek için [FromBody] özniteliği kullandığınızda, MVC biçimlendiricileri yapılandırılmış bir dizi kendi içerik türüne göre istek verileri işlemek için kullanır. Varsayılan olarak, MVC içeren bir `JsonInputFormatter` JSON verilerini, ancak işleme ek biçimlendiricileri XML ve diğer özel biçimler işlemek için ekleyebilirsiniz için sınıf.

> [!NOTE]
> En fazla olabilir bir parametre ile donatılmış eylem başına `[FromBody]`. ASP.NET Core MVC çalışma zamanı, biçimlendirici için istek akışı okuma sorumluluğunu atar. İstek akışı için bir parametre okunduktan sonra genellikle diğer bağlama için yeniden isteği akışını okumak mümkün değildir `[FromBody]` parametreleri.

> [!NOTE]
> `JsonInputFormatter` Bağlıdır ve varsayılan biçimlendiricidir [Json.NET](https://www.newtonsoft.com/json).

ASP.NET Core giriş biçimlendiricileri göre seçer [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) üstbilgi ve parametrenin türü olmadığı sürece, aksi takdirde belirtme uygulanan bir öznitelik. XML kullanmak ister misiniz veya başka bir biçimde, bunu yapılandırmalısınız *Startup.cs* dosyası, ancak öncelikle sahip bir başvuru almak `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet kullanma. Başlangıç kodunuzun aşağıdakine benzer görünmelidir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Kod *Startup.cs* dosyasını içeren bir `ConfigureServices` yöntemi ile bir `services` bağımsız değişkeni için ASP.NET Core uygulamanızı hizmetlerini oluşturmak için kullanabilirsiniz. Aşağıdaki örnekte, bu uygulama için MVC sunan hizmet olarak bir XML biçimlendiricisi ekliyoruz. `options` Bağımsız değişken geçirildi `AddMvc` yöntemi ekleyin ve filtreler, biçimlendiricileri ve diğer sistem seçenekleri MVC uygulama başlatma sırasında yönetmenize olanak sağlar. Daha sonra uygulamanızı `Consumes` özniteliği denetleyici sınıflarına veya istediğiniz biçimi ile çalışması için eylem yöntemleri.

### <a name="custom-model-binding"></a>Özel Model bağlama

Model bağlama, kendi özel model bağlayıcıları yazarak genişletebilirsiniz. Daha fazla bilgi edinin [özel model bağlama](../advanced/custom-model-binding.md).
