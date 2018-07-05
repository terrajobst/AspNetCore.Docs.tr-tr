---
title: Web API ASP.NET Core ile oluşturma
author: scottaddie
description: Web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274972"
---
# <a name="build-web-apis-with-aspnet-core"></a>Web API ASP.NET Core ile oluşturma

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.

## <a name="derive-class-from-controllerbase"></a>Sınıf ControllerBase türetin.

Devralınan [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı. Örneğin:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar. Önceki örnekte, böyle bir yöntemleri dahil [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ve [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Bu yöntemler, HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde çağrılır. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) ayrıca tarafından sağlanan özellik, `ControllerBase`, istek modeli doğrulama gerçekleştirmek için erişilir.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>Sınıf ApiControllerAttribute ile Not Ekle

ASP.NET Core 2.1 tanıtır [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) bir web API denetleyici sınıfı belirtmek için özniteliği. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Bu öznitelik genellikle ile birlikte `ControllerBase` yararlı yöntemlere ve özelliklere erişim kazanmak için. `ControllerBase` yöntemleri erişim gibi sağlar [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ve [dosya](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.

### <a name="automatic-http-400-responses"></a>HTTP 400 otomatik yanıtlar

Doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin. Aşağıdaki kod, Eylemler gereksiz olur:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Aşağıdaki kod ile bu varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Kaynak parametre çıkarımı bağlama

Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar. Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:

|Öznitelik|Bağlama kaynağı |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | İstek gövdesi |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | İstek gövdesini form verilerini |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | İstek üstbilgisi |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | İstek sorgu dizesi parametresi |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Geçerli istek için rota verilerini |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Bir eylem parametresi olarak eklenen isteği hizmeti |

> [!NOTE]
> Yapmak **değil** kullanın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`) çünkü `%2f` unescaped olmaz `/`. Kullanım `[FromQuery]` değer içerebilir, `%2f`.

Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği. Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır. Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın. Bağlama kaynağı öznitelikleri gibi davranır:

* **[FromBody]**  karmaşık tür parametreleri için algılanır. Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) ve [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Bağlama kaynağı çıkarımı kod türlerine özel yok sayar. Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur. Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.
* **[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır. Birden çok yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.
* **[FromQuery]**  için başka bir eylem parametrelerini algılanır.

Aşağıdaki kod ile varsayılan çıkarım kuralları devre dışı *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Çıkarım multipart/form-data iste

Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.

Aşağıdaki kod ile varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Öznitelik yönlendirme gereksinimleri

Öznitelik yönlendirme, bir gereksinim haline gelir. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ya da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) içinde *Startup.Configure*.
::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Denetleyici eylemi dönüş türleri](xref:web-api/action-return-types)
* [Özel biçimlendiriciler](xref:web-api/advanced/custom-formatters)
* [Yanıt verilerini biçimlendirme](xref:web-api/advanced/formatting)
* [Swagger kullanan yardım sayfaları](xref:tutorials/web-api-help-pages-using-swagger)
* [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing)
