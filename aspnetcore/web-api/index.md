---
title: ASP.NET Core ile Web API 'Leri oluşturma
author: scottaddie
description: ASP.NET Core ' de Web API 'SI oluşturmanın temellerini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/02/2020
uid: web-api/index
ms.openlocfilehash: 3dca07db3d6be4ab219a2e05e3adcf1b24ee5c40
ms.sourcegitcommit: 80286715afb93c4d13c931b008016d6086c0312b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074516"
---
# <a name="create-web-apis-with-aspnet-core"></a>ASP.NET Core ile Web API 'Leri oluşturma

[Scott Ade](https://github.com/scottaddie) ve [Tom Dykstra](https://github.com/tdykstra) tarafından

ASP.NET Core, kullanarak C#Web API 'leri olarak da bilinen, yeniden oluşturulan hizmetler oluşturmayı destekler. İstekleri işlemek için, bir Web API 'SI denetleyicileri kullanır. Bir Web API 'sindeki *denetleyiciler* `ControllerBase`türetilen sınıflardır. Bu makalede, Web API isteklerini işlemek için denetleyicilerin nasıl kullanılacağı gösterilmektedir.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([İndirme](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>ControllerBase sınıfı

Bir Web API 'SI <xref:Microsoft.AspNetCore.Mvc.ControllerBase>türetilen bir veya daha fazla denetleyici sınıfından oluşur. Web API proje şablonu bir başlatıcı denetleyicisi sağlar:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<xref:Microsoft.AspNetCore.Mvc.Controller> sınıfından türeterek bir Web API denetleyicisi oluşturmayın. `Controller` `ControllerBase` türetilir ve görünümler için destek ekler, bu nedenle Web API istekleri için değil Web sayfalarını işlemeye yöneliktir. Bu kural için bir özel durum var: aynı denetleyiciyi hem görünümler hem de Web API 'Leri için kullanmayı planlıyorsanız, `Controller`türetirsiniz.

`ControllerBase` sınıfı, HTTP isteklerini işlemek için yararlı olan çok sayıda özellik ve yöntem sağlar. Örneğin, `ControllerBase.CreatedAtAction` 201 durum kodu döndürür:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

`ControllerBase` sağladığı yöntemlere ilişkin bazı örnekler aşağıda verilmiştir.

|Yöntem   |Notlar    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| 400 durum kodunu döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|404 durum kodunu döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|Bir dosya döndürür.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|[Model bağlamasını](xref:mvc/models/model-binding)çağırır.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|[Model doğrulamasını](xref:mvc/models/validation)çağırır.|

Tüm kullanılabilir yöntemlerin ve özelliklerin listesi için bkz. <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Öznitelikler

<xref:Microsoft.AspNetCore.Mvc> ad alanı, Web API denetleyicileri ve eylem yöntemlerinin davranışını yapılandırmak için kullanılabilen öznitelikleri sağlar. Aşağıdaki örnek, desteklenen HTTP eylem fiilini ve döndürülebilecek bilinen HTTP durum kodlarını belirtmek için özniteliklerini kullanır:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Aşağıda, kullanılabilecek özniteliklerin daha fazla örneği verilmiştir.

|Öznitelik|Notlar|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Bir denetleyicinin veya eylemin URL modelini belirtir.|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Model bağlama için dahil edilecek öneki ve özellikleri belirtir.|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |HTTP GET ACTION fiilini destekleyen bir eylemi tanımlar.|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Bir eylemin kabul ettiği veri türlerini belirtir.|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Bir eylemin döndürdüğü veri türlerini belirtir.|

Kullanılabilir öznitelikleri içeren bir liste için <xref:Microsoft.AspNetCore.Mvc> ad alanına bakın.

## <a name="apicontroller-attribute"></a>ApiController özniteliği

[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği, AŞAĞıDAKI, API 'ye özgü davranışları etkinleştirmek üzere bir denetleyici sınıfına uygulanabilir:

::: moniker range=">= aspnetcore-2.2"

* [Öznitelik yönlendirme gereksinimi](#attribute-routing-requirement)
* [Otomatik HTTP 400 yanıtları](#automatic-http-400-responses)
* [Bağlama kaynak parametresi çıkarımı](#binding-source-parameter-inference)
* [Multipart/form-veri isteği çıkarımı](#multipartform-data-request-inference)
* [Hata durum kodları için sorun ayrıntıları](#problem-details-for-error-status-codes)

*Hata durum kodları özelliği Için sorun ayrıntıları* , 2,2 veya üzeri bir [Uyumluluk sürümü](xref:mvc/compatibility-version) gerektirir. Diğer özellikler, 2,1 veya üzeri bir uyumluluk sürümü gerektirir.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

* [Öznitelik yönlendirme gereksinimi](#attribute-routing-requirement)
* [Otomatik HTTP 400 yanıtları](#automatic-http-400-responses)
* [Bağlama kaynak parametresi çıkarımı](#binding-source-parameter-inference)
* [Multipart/form-veri isteği çıkarımı](#multipartform-data-request-inference)

Bu özellikler, 2,1 veya üzeri bir [Uyumluluk sürümü](xref:mvc/compatibility-version) gerektirir.

::: moniker-end

### <a name="attribute-on-specific-controllers"></a>Belirli denetleyicilerde öznitelik

`[ApiController]` özniteliği, proje şablonundan aşağıdaki örnekte olduğu gibi belirli denetleyicilere uygulanabilir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a>Birden çok denetleyicilerde öznitelik

Özniteliği birden fazla denetleyicide kullanmanın bir yaklaşımı, `[ApiController]` özniteliğiyle açıklanmış bir özel temel denetleyici sınıfı oluşturmaktır. Aşağıdaki örnekte, özel bir temel sınıf ve ondan türetilen bir denetleyici gösterilmektedir:

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a>Bir derlemedeki öznitelik

[Uyumluluk sürümü](xref:mvc/compatibility-version) 2,2 veya üzeri bir sürüme ayarlandıysa, `[ApiController]` özniteliği bir derlemeye uygulanabilir. Bu şekilde ek açıklama, derlemedeki tüm denetleyicilere Web API davranışını uygular. Tek tek denetleyiciler için geri alma yöntemi yoktur. Derleme düzeyi özniteliğini `Startup` sınıfını çevreleyen ad alanı bildirimine uygulayın:

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

::: moniker-end

## <a name="attribute-routing-requirement"></a>Öznitelik yönlendirme gereksinimi

`[ApiController]` özniteliği öznitelik yönlendirme bir gereksinim yapar. Örneğin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

Eylemler `Startup.Configure`içinde `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> tarafından tanımlanan [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) aracılığıyla erişilemez.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

Eylemler `Startup.Configure`<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> tarafından tanımlanan [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) aracılığıyla erişilemez.

::: moniker-end

## <a name="automatic-http-400-responses"></a>Otomatik HTTP 400 yanıtları

`[ApiController]` özniteliği, model doğrulama hatalarının otomatik olarak bir HTTP 400 yanıtı tetiklenmesine neden olur. Sonuç olarak, aşağıdaki kod bir eylem yönteminde gereksizdir:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

ASP.NET Core MVC, önceki denetimi yapmak için <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> eylem filtresini kullanır.

### <a name="default-badrequest-response"></a>Varsayılan BadRequest yanıtı

2,1 uyumluluk sürümü ile bir HTTP 400 yanıtı için varsayılan yanıt türü <xref:Microsoft.AspNetCore.Mvc.SerializableError>. Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

2,2 veya üzeri bir uyumluluk sürümü ile, bir HTTP 400 yanıtı için varsayılan yanıt türü <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

`ValidationProblemDetails` türü:

* Web API yanıtlarında hata belirtmek için makine tarafından okunabilen bir biçim sağlar.
* [RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807)uyar.

::: moniker-end

### <a name="log-automatic-400-responses"></a>Otomatik 400 yanıtlarını günlüğe kaydet

Bkz. [otomatik 400 yanıtlarını model doğrulama hatalarında günlüğe kaydetme (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).

### <a name="disable-automatic-400-response"></a>Otomatik 400 yanıtını devre dışı bırak

Otomatik 400 davranışını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliğini `true`olarak ayarlayın. Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a>Bağlama kaynak parametresi çıkarımı

Bağlama kaynak özniteliği, bir eylem parametresi değerinin bulunduğu konumu tanımlar. Aşağıdaki bağlama kaynak öznitelikleri var:

|Öznitelik|Bağlama kaynağı |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | İstek gövdesi |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | İstek gövdesinde form verileri |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | İstek üst bilgisi |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | İstek sorgusu dize parametresi |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Geçerli istekten veri yönlendir |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Eylem parametresi olarak eklenen istek hizmeti |

> [!WARNING]
> Değerler `%2f` (`/`) içerdiğinde `[FromRoute]` kullanmayın. `%2f` `/`atlanmaz. Değer `%2f`içeriyorsa `[FromQuery]` kullanın.

`[FromQuery]`gibi `[ApiController]` özniteliği veya bağlama kaynağı öznitelikleri olmadan ASP.NET Core çalışma zamanı karmaşık nesne modeli Ciltçi kullanmaya çalışır. Karmaşık nesne modeli Ciltçi, verileri değer sağlayıcılarından tanımlı bir düzende çeker.

Aşağıdaki örnekte `[FromQuery]` özniteliği, istek URL 'sinin sorgu dizesinde `discontinuedOnly` parametre değerinin sağlandığını belirtir:

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

`[ApiController]` özniteliği, eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygular. Bu kurallar, eylem parametrelerine öznitelikleri uygulayarak bağlama kaynaklarını el ile tanımlamak zorunda kalmadan sizi kaydeder. Bağlama kaynak çıkarımı kuralları aşağıdaki gibi davranır:

* karmaşık tür parametreleri için `[FromBody]` algılanır. `[FromBody]` çıkarım kuralı için bir özel durum, <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>gibi özel bir anlamı olan karmaşık, yerleşik bir türdür. Bağlama kaynak çıkarımı kodu bu özel türleri yoksayar.
* <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>türündeki eylem parametreleri için `[FromForm]` algılanır. Bu, herhangi bir basit veya Kullanıcı tanımlı tür için çıkarsanamıyor.
* yol şablonundaki bir parametreyle eşleşen herhangi bir eylem parametresi adı için `[FromRoute]` algılanır. Birden fazla yol bir eylem parametresiyle eşleştiğinde, herhangi bir rota değeri `[FromRoute]`olarak değerlendirilir.
* `[FromQuery]` diğer eylem parametreleri için algılanır.

### <a name="frombody-inference-notes"></a>FromBody çıkarım notları

`string` veya `int`gibi basit türler için `[FromBody]` çıkarsanamıyor. Bu nedenle, bu işlev gerektiğinde basit türler için `[FromBody]` özniteliği kullanılmalıdır.

Bir eylem, istek gövdesinden birden fazla parametre bağlamışsa, bir özel durum oluşturulur. Örneğin, aşağıdaki eylem yöntemi imzalarının tümü bir özel duruma neden olur:

* karmaşık türler olduklarından her ikisi de `[FromBody]`.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* karmaşık bir tür olduğundan, başka bir üzerinde `[FromBody]` özniteliği.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* her ikisinde de `[FromBody]` özniteliği.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> ASP.NET Core 2,1 ' de, listeler ve diziler gibi koleksiyon türü parametreleri `[FromQuery]`olarak yanlış şekilde algılanır. `[FromBody]` özniteliği, istek gövdesinden bağlanmaları durumunda bu parametreler için kullanılmalıdır. Bu davranış ASP.NET Core 2,2 veya sonraki bir sürümde düzeltilir. burada, koleksiyon türü parametrelerinin varsayılan olarak gövdeden bağlanacak şekilde çıkarsandır.

::: moniker-end

### <a name="disable-inference-rules"></a>Çıkarım kurallarını devre dışı bırak

Bağlama kaynak çıkarımını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> `true`olarak ayarlayın. Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a>Multipart/form-veri isteği çıkarımı

[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliğiyle bir eylem parametresine ek açıklama eklendiğinde `[ApiController]` özniteliği bir çıkarım kuralı uygular. `multipart/form-data` isteği içerik türü algılanır.

Varsayılan davranışı devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> özelliğini `Startup.ConfigureServices``true` olarak ayarlayın:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a>Hata durum kodları için sorun ayrıntıları

Uyumluluk sürümü 2,2 veya üzeri olduğunda, MVC bir hata sonucunu (durum kodu 400 veya üzeri olan bir sonuç) <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>bir sonuçla dönüştürür. `ProblemDetails` türü, bir HTTP yanıtında makine tarafından okunabilen hata ayrıntılarını sağlamak için [RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807) dayanır.

Bir denetleyici eyleminde aşağıdaki kodu göz önünde bulundurun:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

`NotFound` yöntemi `ProblemDetails` gövde içeren bir HTTP 404 durum kodu üretir. Örneğin:

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a>ProblemDetails yanıtını devre dışı bırak

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> özelliği `true`olarak ayarlandığında `ProblemDetails` örneğinin otomatik olarak oluşturulması devre dışıdır. Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

<a name="consumes"></a>

## <a name="define-supported-request-content-types-with-the-consumes-attribute"></a>Desteklenen istek içerik türlerini [tüketir] özniteliğiyle tanımlayın

Varsayılan olarak, bir eylem tüm kullanılabilir istek içerik türlerini destekler. Örneğin, bir uygulama hem JSON hem de XML [giriş formatlarını](xref:mvc/models/model-binding#input-formatters)destekleyecek şekilde yapılandırıldıysa, bir eylem `application/json` ve `application/xml`dahil olmak üzere birden çok içerik türünü destekler.

[[Tüketir]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) özniteliği, desteklenen istek içerik türlerini sınırlama eylemi sağlar. Bir veya daha fazla içerik türü belirterek `[Consumes]` özniteliğini bir eyleme veya denetleyiciye uygulayın:

```csharp
[HttpPost]
[Consumes("application/xml")]
public IActionResult CreateProduct(Product product)
```

Yukarıdaki kodda `CreateProduct` eylemi `application/xml`içerik türünü belirtir. Bu eyleme yönlendirilen isteklerin `application/xml`bir `Content-Type` üst bilgisi belirtmesi gerekir. `application/xml` `Content-Type` üst bilgisi belirtmeyen istekler [415 desteklenmeyen medya türü](https://developer.mozilla.org/docs/Web/HTTP/Status/415) yanıtına neden olacak.

`[Consumes]` özniteliği Ayrıca bir eylemin bir tür kısıtlaması uygulayarak bir gelen isteğin içerik türüne göre seçimini etkilemesini sağlar. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](index/samples/3.x/Controllers/ConsumesController.cs?name=snippet_Class)]

Yukarıdaki kodda `ConsumesController`, `https://localhost:5001/api/Consumes` URL 'sine gönderilen istekleri işleyecek şekilde yapılandırılmıştır. Denetleyicinin eylemleri, `PostJson` ve `PostForm`aynı URL 'ye sahip POST isteklerini işler. Bir tür kısıtlaması uygulamadan `[Consumes]` özniteliği olmadan belirsiz eşleşme özel durumu oluşur.

`[Consumes]` özniteliği her iki eyleme de uygulanır. `PostJson` eylemi, `application/json``Content-Type` üstbilgisiyle gönderilen istekleri işler. `PostForm` eylemi, `application/x-www-form-urlencoded``Content-Type` üstbilgisiyle gönderilen istekleri işler. 

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
