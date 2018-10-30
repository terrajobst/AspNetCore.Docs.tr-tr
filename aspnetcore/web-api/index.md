---
title: Web API ASP.NET Core ile oluşturma
author: scottaddie
description: Web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 950f4e8afa13bf297ea8658ef1c1bea0c9b62936
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234598"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="8fbcf-103">Web API ASP.NET Core ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="8fbcf-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="8fbcf-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="8fbcf-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8fbcf-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fbcf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8fbcf-106">Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="8fbcf-107">Sınıf ControllerBase türetin.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="8fbcf-108">Devralınan <xref:Microsoft.AspNetCore.Mvc.ControllerBase> web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="8fbcf-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="8fbcf-110">`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="8fbcf-111">Önceki kodda; örnekler <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="8fbcf-112">HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="8fbcf-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Ayrıca tarafından sağlanan özellik, `ControllerBase`, model doğrulama isteği işlemek için erişilir.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="8fbcf-114">Sınıf ApiControllerAttribute ile Not Ekle</span><span class="sxs-lookup"><span data-stu-id="8fbcf-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="8fbcf-115">ASP.NET Core 2.1 tanıtır [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) bir web API denetleyici sınıfı belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="8fbcf-116">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="8fbcf-117">2.1 veya üzeri uyumluluk sürümüyle kümesi aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, bu öznitelik kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="8fbcf-118">Örneğin, vurgulanan kodu *Startup.ConfigureServices* 2.2 Uyumluluk bayrağını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="8fbcf-119">Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="8fbcf-120">`[ApiController]` Özniteliği ile birlikte sık `ControllerBase` denetleyicileri için REST özgü davranışı etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="8fbcf-121">`ControllerBase` yöntemleri erişim gibi sağlar <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="8fbcf-122">İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="8fbcf-123">Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="8fbcf-124">Hata durum kodları için yanıtları sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="8fbcf-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="8fbcf-125">ASP.NET Core 2.1 ve üzeri içerir [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), bir tür temel alarak [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="8fbcf-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="8fbcf-126">`ProblemDetails` Türü makine okunabilir bir HTTP yanıtında hata ayrıntılarını açıklamak için kullanılan standart bir biçim sağlar.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="8fbcf-127">ASP.NET Core 2.2 ve sonraki sürümlerinde, MVC ile bir sonuç için hata durumu kodu sonuçları (durum kodu 400 ve üzeri) dönüştüren `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="8fbcf-128">Aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="8fbcf-129">HTTP yanıtı `NotFound` sonucu olan bir 404 durum kodu ile bir `ProblemDetails` gövdesi aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="8fbcf-130">Sorun ayrıntıları özelliği 2.2 veya üzeri uyumluluk bayrak gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="8fbcf-131">Varsayılan davranışı devre dışı olduğunda [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="8fbcf-132">Aşağıdaki vurgulanmış koddan `Startup.ConfigureServices` sorun ayrıntıları devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="8fbcf-133">Kullanım [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> içeriğini yapılandırmak için özellik `ProblemDetails` yanıt.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="8fbcf-134">Örneğin, aşağıdaki güncelleştirmeleri kod `type` geçmesine karşın 404 yanıtlarını özelliği:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="8fbcf-135">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="8fbcf-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="8fbcf-136">Doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="8fbcf-137">Aşağıdaki kod, Eylemler gereksiz olur:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="8fbcf-138">Kullanım <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> çıktısını özelleştirmek için verilen yanıtları.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="8fbcf-139">Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="8fbcf-140">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="8fbcf-141">2.2 veya üzeri uyumluluk bayrağı ile 400 yanıtlar için döndürülen varsayılan yanıt türü olan bir <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="8fbcf-142">Kullanma [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> özelliği ASP.NET Core 2.1 hata biçimini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-142">Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="8fbcf-143">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="8fbcf-143">Binding source parameter inference</span></span>

<span data-ttu-id="8fbcf-144">Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="8fbcf-145">Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="8fbcf-146">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="8fbcf-146">Attribute</span></span>|<span data-ttu-id="8fbcf-147">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="8fbcf-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="8fbcf-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="8fbcf-149">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="8fbcf-149">Request body</span></span> |
|<span data-ttu-id="8fbcf-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="8fbcf-151">İstek gövdesini form verilerini</span><span class="sxs-lookup"><span data-stu-id="8fbcf-151">Form data in the request body</span></span> |
|<span data-ttu-id="8fbcf-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="8fbcf-153">İstek üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="8fbcf-153">Request header</span></span> |
|<span data-ttu-id="8fbcf-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="8fbcf-155">İstek sorgu dizesi parametresi</span><span class="sxs-lookup"><span data-stu-id="8fbcf-155">Request query string parameter</span></span> |
|<span data-ttu-id="8fbcf-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="8fbcf-157">Geçerli istek için rota verilerini</span><span class="sxs-lookup"><span data-stu-id="8fbcf-157">Route data from the current request</span></span> |
|<span data-ttu-id="8fbcf-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="8fbcf-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="8fbcf-159">Bir eylem parametresi olarak eklenen isteği hizmeti</span><span class="sxs-lookup"><span data-stu-id="8fbcf-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="8fbcf-160">Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`).</span><span class="sxs-lookup"><span data-stu-id="8fbcf-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="8fbcf-161">`%2f` unescaped olmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="8fbcf-162">Kullanım `[FromQuery]` değer içerebilir, `%2f`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="8fbcf-163">Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="8fbcf-164">Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="8fbcf-165">Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="8fbcf-166">Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="8fbcf-167">Bağlama kaynağı öznitelikleri gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="8fbcf-168">**[FromBody]**  karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="8fbcf-169">Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="8fbcf-170">Bağlama kaynağı çıkarımı kod türlerine özel yok sayar.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="8fbcf-171">`[FromBody]` basit türleri için gibi çıkarılan değil `string` veya `int`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="8fbcf-172">Bu nedenle, `[FromBody]` özniteliği işlevselliği istendiğinde basit türleri için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="8fbcf-173">Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="8fbcf-174">Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="8fbcf-175">**[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="8fbcf-176">Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="8fbcf-177">**[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="8fbcf-178">Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="8fbcf-179">**[FromQuery]**  için başka bir eylem parametrelerini algılanır.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="8fbcf-180">Varsayılan çıkarım kuralları devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="8fbcf-181">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="8fbcf-182">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="8fbcf-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="8fbcf-183">Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="8fbcf-184">Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="8fbcf-185">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="8fbcf-186">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="8fbcf-186">Attribute routing requirement</span></span>

<span data-ttu-id="8fbcf-187">Öznitelik yönlendirme, bir gereksinim haline gelir.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="8fbcf-188">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8fbcf-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="8fbcf-189">Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ya da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="8fbcf-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8fbcf-190">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8fbcf-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
