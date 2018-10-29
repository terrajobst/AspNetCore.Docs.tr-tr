---
title: Web API ASP.NET Core ile oluşturma
author: scottaddie
description: Web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: e4615e5d416ba2433d55309b25ee3643c6c636ac
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207010"
---
# <a name="build-web-apis-with-aspnet-core"></a>Web API ASP.NET Core ile oluşturma

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.

## <a name="derive-class-from-controllerbase"></a>Sınıf ControllerBase türetin.

Devralınan <xref:Microsoft.AspNetCore.Mvc.ControllerBase> web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı. Örneğin:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar. Önceki kodda; örnekler <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde bu yöntem çağrılır. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Ayrıca tarafından sağlanan özellik, `ControllerBase`, model doğrulama isteği işlemek için erişilir.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Sınıf ApiControllerAttribute ile Not Ekle

ASP.NET Core 2.1 tanıtır [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) bir web API denetleyici sınıfı belirtmek için özniteliği. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

2.1 veya üzeri uyumluluk sürümüyle kümesi aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, bu öznitelik kullanmak için gereklidir. Örneğin, vurgulanan kodu *Startup.ConfigureServices* 2.2 Uyumluluk bayrağını ayarlar:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.

`[ApiController]` Özniteliği ile birlikte sık `ControllerBase` denetleyicileri için REST özgü davranışı etkinleştirmek için. `ControllerBase` yöntemleri erişim gibi sağlar <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.

### <a name="problem-details-responses-for-error-status-codes"></a>Hata durum kodları için yanıtları sorun ayrıntıları

ASP.NET Core 2.1 ve üzeri içerir [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), bir tür temel alarak [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807). `ProblemDetails` Türü makine okunabilir bir HTTP yanıtında hata ayrıntılarını açıklamak için kullanılan standart bir biçim sağlar.

ASP.NET Core 2.2 ve sonraki sürümlerinde, MVC ile bir sonuç için hata durumu kodu sonuçları (durum kodu 400 ve üzeri) dönüştüren `ProblemDetails`. Aşağıdaki kodu göz önünde bulundurun:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

HTTP yanıtı `NotFound` sonucu olan bir 404 durum kodu ile bir `ProblemDetails` gövdesi aşağıdakine benzer:

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

Sorun ayrıntıları özelliği 2.2 veya üzeri uyumluluk bayrak gerektirir. Varsayılan davranışı devre dışı olduğunda [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> özelliği `true`. Aşağıdaki vurgulanmış koddan `Startup.ConfigureServices` sorun ayrıntıları devre dışı bırakır:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

Kullanım [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> içeriğini yapılandırmak için özellik `ProblemDetails` yanıt. Örneğin, aşağıdaki güncelleştirmeleri kod `type` geçmesine karşın 404 yanıtlarını özelliği:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a>HTTP 400 otomatik yanıtlar

Doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin. Aşağıdaki kod, Eylemler gereksiz olur:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Kullanım <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> çıktısını özelleştirmek için verilen yanıtları.

Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliği `true`. Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

2.2 veya üzeri uyumluluk bayrağı ile 400 yanıtlar için döndürülen varsayılan yanıt türü olan bir <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Kullanma [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> özelliği ASP.NET Core 2.1 hata biçimini kullanın.

### <a name="binding-source-parameter-inference"></a>Kaynak parametre çıkarımı bağlama

Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar. Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:

|Öznitelik|Bağlama kaynağı |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | İstek gövdesi |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | İstek gövdesini form verilerini |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | İstek üstbilgisi |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | İstek sorgu dizesi parametresi |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Geçerli istek için rota verilerini |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Bir eylem parametresi olarak eklenen isteği hizmeti |

> [!WARNING]
> Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`). `%2f` unescaped olmaz `/`. Kullanım `[FromQuery]` değer içerebilir, `%2f`.

Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği. Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır. Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın. Bağlama kaynağı öznitelikleri gibi davranır:

* **[FromBody]**  karmaşık tür parametreleri için algılanır. Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>. Bağlama kaynağı çıkarımı kod türlerine özel yok sayar. `[FromBody]` basit türleri için gibi çıkarılan değil `string` veya `int`. Bu nedenle, `[FromBody]` özniteliği işlevselliği istendiğinde basit türleri için kullanılmalıdır. Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur. Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.
* **[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır. Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.
* **[FromQuery]**  için başka bir eylem parametrelerini algılanır.

Varsayılan çıkarım kuralları devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> özelliği `true`. Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Çıkarım multipart/form-data iste

Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.

Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> özelliği `true`. Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Öznitelik yönlendirme gereksinimleri

Öznitelik yönlendirme, bir gereksinim haline gelir. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ya da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
