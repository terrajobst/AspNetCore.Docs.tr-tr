---
title: Web API ile ASP.NET çekirdek oluşturma
author: scottaddie
description: Bir web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/index
ms.openlocfilehash: 6afc02c1a966b62d0fcead0349c5f0803309dcbb
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33972793"
---
# <a name="build-web-apis-with-aspnet-core"></a>Web API ile ASP.NET çekirdek oluşturma

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu belge, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.

## <a name="derive-class-from-controllerbase"></a>ControllerBase sınıf türetin

Devralınan [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) web API'si hizmet vermek için amaçlanan bir denetleyicide sınıfı. Örneğin:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

`ControllerBase` Sınıfı çok sayıda özellikleri ve yöntemleri erişim sağlar. Önceki örnekte, bu tür bir yöntemleri dahil [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ve [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Bu yöntemler HTTP 400 ve 201 durum kodları, sırasıyla döndürmek için eylem yöntemlerinde çağrılır. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) de tarafından sağlanan özellik, `ControllerBase`, istek model doğrulama gerçekleştirmek için erişilir.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>ApiControllerAttribute sınıfıyla açıklama ekleme

ASP.NET Core 2.1 tanıtır [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) web API denetleyicisi sınıfı belirtmek için öznitelik. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Bu öznitelik genellikle ile birlikte `ControllerBase` yararlı yöntemlere ve özelliklere erişmek için. `ControllerBase` yöntemleri erişim gibi sağlar [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ve [dosya](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

İle açıklanan özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Aşağıdaki bölümlerde özniteliği tarafından eklenen kullanışlı özellikler açıklanmaktadır.

### <a name="automatic-http-400-responses"></a>Otomatik HTTP 400 yanıtlar

Doğrulama hatalarını otomatik olarak bir HTTP 400 yanıt tetikler. Aşağıdaki kod, Eylemler gereksiz olur:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Aşağıdaki kod ile bu varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Kaynak parametresi çıkarım bağlama

Bağlama kaynak özniteliği, bir eylem parametresinin değeri bulunan konumu tanımlar. Aşağıdaki bağlama kaynak öznitelikleri mevcuttur:

|Öznitelik|Bağlama kaynağı |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | İstek gövdesi |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | İstek gövdesini form verilerini |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | İstek üstbilgisi |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Sorgu dizesi parametresi isteği |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Geçerli isteğin rota verileri |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Bir eylem parametresi olarak eklenen isteği hizmeti |

> [!NOTE]
> Yapmak **değil** kullanmak `[FromRoute]` zaman değerleri içeriyor olabilir `%2f` (diğer bir deyişle `/`) çünkü `%2f` için unescaped olmaz `/`. Kullanım `[FromQuery]` değeri içerme olasılığı varsa `%2f`.

Olmadan `[ApiController]` öznitelikleri açıkça tanımlanmış kaynak bağlama özniteliği. Aşağıdaki örnekte, `[FromQuery]` öznitelik gösterir `discontinuedOnly` istek URL'SİNDEKİ sorgu dizesinde sağlanan parametre değeri:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Çıkarım kuralları eylem parametrelerini varsayılan veri kaynakları için uygulanır. Bu kurallar, aksi takdirde eylem parametrelerini el ile uygulamak büyük olasılıkla bağlama kaynaklarını yapılandırın. Bağlama kaynak öznitelikleri gibi davranır:

* **[FromBody]**  karmaşık tür parametreleri için algılanır. Bu kural için bir özel gibi karmaşık, yerleşik ile herhangi bir türü özel bir anlamı olan [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) ve [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Bağlama kaynak çıkarım kodu, bu özel türleri yok sayar. Birden fazla parametre açıkça belirtilen bir eylem sahip olduğunda (aracılığıyla `[FromBody]`) veya isteği gövdesinden bağlı olarak çıkarımı yapılan, bir özel durum oluşur. Örneğin, aşağıdaki eylemi imzaları bir özel durum neden:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Tüm basit veya kullanıcı tanımlı türler için olayla değil.
* **[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adını algılanır. Birden çok yol bir eylem parametresinin eşleştiğinde, herhangi bir rota değeri olarak kabul edilir `[FromRoute]`.
* **[FromQuery]**  için başka bir eylem parametrelerini algılanır.

Aşağıdaki kod ile varsayılan çıkarım kuralları devre dışı *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Multipart/form-data isteği çıkarımı

Ne zaman bir eylem parametresine açıklama ile [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) özniteliği `multipart/form-data` içerik türü çıkarımı yapılan istek.

Aşağıdaki kod ile varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Yönlendirme gereksinim özniteliği

Öznitelik yönlendirme gereksinimi olur. Örneğin:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Eylemler aracılığıyla erişilemez [geleneksel yolları](xref:mvc/controllers/routing#conventional-routing) tanımlanan [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) veya [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) içinde *Startup.Configure*.
::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Denetleyici eylemi dönüş türleri](xref:web-api/action-return-types)
* [Özel biçimlendiriciler](xref:web-api/advanced/custom-formatters)
* [Yanıt verilerini biçimlendirme](xref:web-api/advanced/formatting)
* [Swagger kullanan yardım sayfaları](xref:tutorials/web-api-help-pages-using-swagger)
* [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing)
