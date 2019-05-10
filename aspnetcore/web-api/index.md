---
title: ASP.NET Core ile Web API'leri oluşturma
author: scottaddie
description: ASP.NET Core web API'si oluşturma hakkındaki temel bilgileri öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/11/2019
uid: web-api/index
ms.openlocfilehash: d804a7f1b4f0e89f433a3674116c97804705f7cc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902582"
---
# <a name="create-web-apis-with-aspnet-core"></a>ASP.NET Core ile Web API'leri oluşturma

Tarafından [Scott Addie](https://github.com/scottaddie) ve [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core web API'leri kullanarak, olarak da bilinir oluşturma RESTful hizmetleri destekleyen C#. İsteklerini işlemek için bir web API denetleyicisi kullanır. *Denetleyicileri* bir web API'SİNDE türetilen sınıflardır `ControllerBase`. Bu makalede, API istekleri işlemek için denetleyicileri kullanmayı gösterir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Nasıl indirileceğini](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>ControllerBase sınıfı

Bir web API öğesinden türetilen en az bir denetleyici sınıflarına sahip <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Örneğin, web API proje şablonunu değerleri denetleyici oluşturur:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

Bir web API denetleyicisi türeterek oluşturmayın <xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfı. `Controller` öğesinden türetilen `ControllerBase` ve web API istekleri işleme web sayfaları için bu nedenle görünümleri için destek ekler.  Bu kural için bir istisna vardır: görünümler ve API'leri için aynı denetleyici kullanmayı planlıyorsanız, bu sınıftan türetilen `Controller`.

`ControllerBase` Sınıfı, birçok özellik ve HTTP isteklerini işlemek için kullanışlı yöntemler sağlar. Örneğin, `ControllerBase.CreatedAtAction` 201 durum kodunu döndürür:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 Yöntemlerden daha fazla bazı örnekleri aşağıda verilmiştir, `ControllerBase` sağlar.

|Yöntem  |Notlar  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| 400 durum kodu döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |404 durum kodu döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|Bir dosyayı döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|Çağıran [model bağlama](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|Çağıran [model doğrulama](xref:mvc/models/validation).|

Tüm kullanılabilir yöntemlerin ve özelliklerin listesi için bkz. <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Öznitelikler

<xref:Microsoft.AspNetCore.Mvc> Ad alanı, web API'si denetleyiciler ve eylem yöntemlerine davranışını yapılandırmak için kullanılan öznitelikleri sağlar. Aşağıdaki örnek, HTTP yöntemini kabul edilir ve döndürülen durum kodları belirtmek için öznitelikleri kullanır:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Kullanılabilir öznitelikler daha fazla bazı örnekleri aşağıda verilmiştir.

|Öznitelik|Notlar|
|---------|-----|
|[[Yol]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Bir denetleyici veya eylem için URL deseni belirtir.|
|[[Bağlama]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Önek ve dahil edilecek model bağlama için özellik belirtir.|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |HTTP GET yöntemini destekleyen bir eylem belirtir.|
|[[Tüketir]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Bir eylem kabul eden veri türlerini belirtir.|
|[[Oluşturur]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Bir eylem döndürdüğü veri türlerini belirtir.|

Kullanılabilir öznitelikler içeren bir liste için bkz. <xref:Microsoft.AspNetCore.Mvc> ad alanı.

## <a name="apicontroller-attribute"></a>ApiController özniteliği

[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) API özgü davranışları etkinleştirmek için bir denetleyici sınıfı özniteliği uygulanabilir:

* [Öznitelik yönlendirme gereksinimleri](#attribute-routing-requirement)
* [HTTP 400 otomatik yanıtlar](#automatic-http-400-responses)
* [Kaynak parametre çıkarımı bağlama](#binding-source-parameter-inference)
* [Çıkarım multipart/form-data iste](#multipartform-data-request-inference)
* [Hata durum kodları için sorun ayrıntıları](#problem-details-for-error-status-codes)

Bu özellikleri gerektiren bir [uyumluluk sürümü](<xref:mvc/compatibility-version>) 2.1 veya üzeri.

### <a name="apicontroller-on-specific-controllers"></a>Belirli denetleyicilerinde ApiController

`[ApiController]` Özniteliği, proje şablonundan aşağıdaki örnekteki gibi belirli denetleyicilerine uygulanabilir:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a>Birden çok denetleyicisinde ApiController

Birden fazla denetleyicisinde özniteliğini kullanarak bir yaklaşım olan ile ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için `[ApiController]` özniteliği. Özel bir temel sınıf ve ondan türetilen denetleyicisi gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a>Bir derleme üzerinde ApiController

Varsa [uyumluluk sürümü](<xref:mvc/compatibility-version>) 2.2 veya sonraki sürümleri ayarlanır `[ApiController]` özniteliği bir derlemeye uygulanabilir. Bu şekilde ek açıklama derleme içindeki tüm denetleyicilere web API davranış uygulanır. Tek tek denetleyicileri için geri çevirmek için hiçbir yolu yoktur. Derleme düzeyi özniteliği için geçerli `Startup` aşağıdaki örnekte gösterildiği gibi sınıf:

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a>Öznitelik yönlendirme gereksinimleri

`ApiController` Özniteliği öznitelik gereksinim yönlendirme sağlar. Örneğin:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tarafından tanımlanan <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde `Startup.Configure`.

## <a name="automatic-http-400-responses"></a>HTTP 400 otomatik yanıtlar

`ApiController` Öznitelik yapar model doğrulama hataları otomatik olarak bir HTTP 400 tetikleyici yanıtı. Sonuç olarak, aşağıdaki kod bir eylem yöntemi gerekli değildir:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a>Varsayılan BadRequest yanıt 

2.2 veya üzeri uyumluluk sürümü ile varsayılan yanıt için HTTP 400 yanıtları türdür <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. `ValidationProblemDetails` Türü uyumlu ile [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807).

Varsayılan yanıt olarak değiştirmek için <xref:Microsoft.AspNetCore.Mvc.SerializableError>ayarlayın `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` özelliğini `true` içinde `Startup.ConfigureServices`aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a>BadRequest yanıt özelleştirme

Doğrulama hatasıyla sonuçlanan yanıt özelleştirmek için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>. Sonra aşağıdaki vurgulanmış kodu ekleyin `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="disable-automatic-400"></a>Otomatik 400 devre dışı bırak

Otomatik 400 davranışı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliğini `true`. Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a>Kaynak parametre çıkarımı bağlama

Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar. Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:

|Öznitelik|Bağlama kaynağı |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | İstek gövdesi |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | İstek gövdesini form verilerini |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | İstek üstbilgisi |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | İstek sorgu dizesi parametresi |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Geçerli istek için rota verilerini |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Bir eylem parametresi olarak eklenen isteği hizmeti |

> [!WARNING]
> Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`). `%2f` unescaped olmaz `/`. Kullanım `[FromQuery]` değer içerebilir, `%2f`.

Olmadan `[ApiController]` özniteliği veya bağlama kaynağı öznitelikleri ister `[FromQuery]`, ASP.NET Core çalışma zamanı, karmaşık nesne model bağlayıcısını kullanmayı dener. Karmaşık nesne model bağlayıcı, tanımlanmış bir sıralama değeri sağlayıcılardan veri çeker.

Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

`[ApiController]` Özniteliği eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygular. Bu kurallar kaynakları bağlama eylem parametreleri öznitelikleri uygulayarak el ile tanımlamak zorunda kalmaktan kaydedin. Bağlama kaynağı çıkarım kuralları gibi davranır:

* `[FromBody]` Karmaşık tür parametreleri için algılanır. Bir özel durum `[FromBody]` çıkarım kuralı olan herhangi bir karmaşık, yerleşik türü özel bir anlamı olan gibi <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>. Bağlama kaynağı çıkarımı kod türlerine özel yok sayar. 
* `[FromForm]` için eylem parametrelerini tür çıkarımı yapılan <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.
* `[FromRoute]` Rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır. Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.
* `[FromQuery]` herhangi bir eylem parametreleri için algılanır.

### <a name="frombody-inference-notes"></a>FromBody çıkarımı notları

`[FromBody]` basit türleri için gibi çıkarılan değil `string` veya `int`. Bu nedenle, `[FromBody]` özniteliği işlevselliği gerektiğinde basit türleri için kullanılmalıdır.

Bir eylem gövdeden bağlı birden fazla parametre varsa, bir özel durum oluşturulur. Örneğin, aşağıdaki eylemi yöntem imzaları tümünün bir özel duruma neden:

* `[FromBody]` karmaşık türler için hem de sonuçlandı.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* `[FromBody]` öznitelik, bir karmaşık bir tür olduğundan diğer sonuçlandı.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* `[FromBody]` Her iki öznitelik.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> ASP.NET Core 2.1 içinde koleksiyonu tür parametreleri listeler ve diziler gibi yanlış olarak algılanır `[FromQuery]`. `[FromBody]` Özniteliği gövdeden bağlı olmaları durumunda bu parametreler için kullanılmalıdır. Bu davranış ASP.NET Core 2.2 veya üzeri, toplama türü parametrelerini gövdesinden varsayılan olarak bağlı olmasını olduğu algılanır düzeltilir.

### <a name="disable-inference-rules"></a>Çıkarım kuralları devre dışı bırak

Bağlama kaynağı çıkarımı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> için `true`. Aşağıdaki kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a>Çıkarım multipart/form-data iste

`[ApiController]` Özniteliğini uygular çıkarım kuralı ile bir eylem parametresinin açıklandığında [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliği: `multipart/form-data` içerik türü çıkarılan isteyin.

Varsayılan davranışı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> için `true` içinde `Startup.ConfigureServices`aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a>Hata durum kodları için sorun ayrıntıları

2.2 veya üzeri uyumluluk sürümü olduğunda, MVC için bir sonuç ile bir hata sonucu (durum kodu 400 veya üzeri bir sonuç) dönüştüren <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. `ProblemDetails` Türü temel [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807) bir HTTP yanıtı makine tarafından okunabilir hata ayrıntılarında sağlamak için.

Aşağıdaki kodda bir denetleyici eylemi göz önünde bulundurun:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

HTTP yanıtı `NotFound` bir 404 durum kodu ile sahip bir `ProblemDetails` gövdesi. Örneğin:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a>ProblemDetails yanıt özelleştirme

Kullanım `ClientErrorMapping` içeriğini yapılandırmak için özellik `ProblemDetails` yanıt. Örneğin, aşağıdaki güncelleştirmeleri kod `type` geçmesine karşın 404 yanıtlarını özelliği:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a>ProblemDetails yanıt devre dışı bırak

Otomatik olarak oluşturulmasını `ProblemDetails` ne zaman devre dışı `SuppressMapClientErrors` özelliği `true`. Aşağıdaki kodu ekleyin `Startup.ConfigureServices`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a>Ek kaynaklar 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
