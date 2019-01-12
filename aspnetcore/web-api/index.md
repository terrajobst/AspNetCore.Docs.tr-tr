---
title: Web API ASP.NET Core ile oluşturma
author: scottaddie
description: Web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: a826bdecdd3a25eb23597123166695c169ba4229
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249444"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="fab3b-103">Web API ASP.NET Core ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="fab3b-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="fab3b-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="fab3b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="fab3b-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fab3b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fab3b-106">Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="fab3b-107">Sınıf ControllerBase türetin.</span><span class="sxs-lookup"><span data-stu-id="fab3b-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="fab3b-108">Devralınan <xref:Microsoft.AspNetCore.Mvc.ControllerBase> web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fab3b-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="fab3b-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fab3b-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="fab3b-110">`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="fab3b-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="fab3b-111">Önceki kodda; örnekler <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="fab3b-112">HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="fab3b-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Ayrıca tarafından sağlanan özellik, `ControllerBase`, model doğrulama isteği işlemek için erişilir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="fab3b-114">Ek açıklama ApiController özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="fab3b-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="fab3b-115">ASP.NET Core 2.1 tanıtır [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) bir web API denetleyici sınıfı belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="fab3b-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="fab3b-116">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fab3b-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="fab3b-117">2.1 veya üzeri uyumluluk sürümüyle kümesi aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, bu öznitelik denetleyici düzeyinde kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="fab3b-118">Örneğin, vurgulanan kodu `Startup.ConfigureServices` 2.1 Uyumluluk bayrağını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="fab3b-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="fab3b-119">Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fab3b-120">ASP.NET Core 2.2 veya sonraki sürümlerde, `[ApiController]` özniteliği bir derlemeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="fab3b-121">Bu şekilde ek açıklama derleme içindeki tüm denetleyicilere web API davranış uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="fab3b-122">Tek tek denetleyicileri için geri çevirmek mümkün olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="fab3b-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="fab3b-123">Bir öneri olarak, derleme düzeyinde öznitelikler uygulanması gereken `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="fab3b-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="fab3b-124">2.2 veya üzeri uyumluluk sürümüyle kümesi aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, bu öznitelik derleme düzeyinde kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fab3b-125">`[ApiController]` Özniteliği ile birlikte sık `ControllerBase` denetleyicileri için REST özgü davranışı etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="fab3b-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="fab3b-126">`ControllerBase` yöntemleri erişim gibi sağlar <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="fab3b-127">İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="fab3b-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="fab3b-128">Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="fab3b-129">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="fab3b-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="fab3b-130">Model doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="fab3b-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="fab3b-131">Sonuç olarak, aşağıdaki kod, Eylemler gereksiz olur:</span><span class="sxs-lookup"><span data-stu-id="fab3b-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="fab3b-132">Kullanım <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> elde edilen yanıt çıktısı özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="fab3b-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="fab3b-133">Varsayılan davranışı devre dışı bırakma eylem bir model doğrulama hatadan kurtarılması gerektiğinde faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="fab3b-134">Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="fab3b-135">Aşağıdaki kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fab3b-136">2.2 veya üzeri uyumluluk bayrağı ile varsayılan yanıt için HTTP 400 yanıtları türdür <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="fab3b-137">`ValidationProblemDetails` Türü uyumlu ile [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="fab3b-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="fab3b-138">Ayarlama `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` özelliğini `true` ASP.NET Core 2.1 hata biçimi yerine döndürülecek <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="fab3b-139">Aşağıdaki kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-139">Add the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="fab3b-140">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="fab3b-140">Binding source parameter inference</span></span>

<span data-ttu-id="fab3b-141">Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fab3b-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="fab3b-142">Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="fab3b-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="fab3b-143">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="fab3b-143">Attribute</span></span>|<span data-ttu-id="fab3b-144">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="fab3b-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="fab3b-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="fab3b-146">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="fab3b-146">Request body</span></span> |
|<span data-ttu-id="fab3b-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="fab3b-148">İstek gövdesini form verilerini</span><span class="sxs-lookup"><span data-stu-id="fab3b-148">Form data in the request body</span></span> |
|<span data-ttu-id="fab3b-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="fab3b-150">İstek üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="fab3b-150">Request header</span></span> |
|<span data-ttu-id="fab3b-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="fab3b-152">İstek sorgu dizesi parametresi</span><span class="sxs-lookup"><span data-stu-id="fab3b-152">Request query string parameter</span></span> |
|<span data-ttu-id="fab3b-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="fab3b-154">Geçerli istek için rota verilerini</span><span class="sxs-lookup"><span data-stu-id="fab3b-154">Route data from the current request</span></span> |
|<span data-ttu-id="fab3b-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="fab3b-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="fab3b-156">Bir eylem parametresi olarak eklenen isteği hizmeti</span><span class="sxs-lookup"><span data-stu-id="fab3b-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="fab3b-157">Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`).</span><span class="sxs-lookup"><span data-stu-id="fab3b-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="fab3b-158">`%2f` unescaped olmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="fab3b-159">Kullanım `[FromQuery]` değer içerebilir, `%2f`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="fab3b-160">Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği.</span><span class="sxs-lookup"><span data-stu-id="fab3b-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="fab3b-161">Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="fab3b-161">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="fab3b-162">Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-162">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="fab3b-163">Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fab3b-163">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="fab3b-164">Bağlama kaynağı öznitelikleri gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="fab3b-164">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="fab3b-165">**[FromBody]**  karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-165">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="fab3b-166">Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-166">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="fab3b-167">Bağlama kaynağı çıkarımı kod türlerine özel yok sayar.</span><span class="sxs-lookup"><span data-stu-id="fab3b-167">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="fab3b-168">`[FromBody]` basit türleri için gibi çıkarılan değil `string` veya `int`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-168">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="fab3b-169">Bu nedenle, `[FromBody]` özniteliği işlevselliği gerektiğinde basit türleri için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-169">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="fab3b-170">Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fab3b-170">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="fab3b-171">Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:</span><span class="sxs-lookup"><span data-stu-id="fab3b-171">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="fab3b-172">ASP.NET Core 2.1 içinde koleksiyonu tür parametreleri listeler ve diziler gibi yanlış olarak algılanır [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span><span class="sxs-lookup"><span data-stu-id="fab3b-172">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="fab3b-173">[[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) gövdeden bağlı olmaları durumunda bu parametreler için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-173">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="fab3b-174">Bu davranış ASP.NET Core 2.2 veya sonraki sürümlerde, toplama türü parametrelerini gövdesinden varsayılan olarak bağlı olmasını olduğu algılanır sabittir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-174">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="fab3b-175">**[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="fab3b-176">Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.</span><span class="sxs-lookup"><span data-stu-id="fab3b-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="fab3b-177">**[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="fab3b-178">Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="fab3b-179">**[FromQuery]**  için başka bir eylem parametrelerini algılanır.</span><span class="sxs-lookup"><span data-stu-id="fab3b-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="fab3b-180">Varsayılan çıkarım kuralları devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="fab3b-181">Aşağıdaki kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-181">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="fab3b-182">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="fab3b-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="fab3b-183">Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.</span><span class="sxs-lookup"><span data-stu-id="fab3b-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="fab3b-184">Varsayılan davranışı devre dışı olduğunda <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fab3b-185">Aşağıdaki kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-185">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fab3b-186">Aşağıdaki kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-186">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="fab3b-187">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="fab3b-187">Attribute routing requirement</span></span>

<span data-ttu-id="fab3b-188">Öznitelik yönlendirme, bir gereksinim haline gelir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-188">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="fab3b-189">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fab3b-189">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="fab3b-190">Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ya da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-190">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="fab3b-191">Hata durum kodları için yanıtları sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="fab3b-191">Problem details responses for error status codes</span></span>

<span data-ttu-id="fab3b-192">ASP.NET Core 2.2 veya sonraki sürümlerde, MVC için bir sonuç ile bir hata sonucu (durum kodu 400 veya üzeri bir sonuç) dönüştüren <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="fab3b-192">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="fab3b-193">`ProblemDetails` aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="fab3b-193">`ProblemDetails` is:</span></span>

* <span data-ttu-id="fab3b-194">Bir tür temel alarak [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="fab3b-194">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="fab3b-195">Bir HTTP yanıtında makine tarafından okunabilir hata ayrıntılarını belirtmek için kullanılan standart bir biçim.</span><span class="sxs-lookup"><span data-stu-id="fab3b-195">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="fab3b-196">Aşağıdaki kodda bir denetleyici eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fab3b-196">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="fab3b-197">HTTP yanıtı `NotFound` bir 404 durum kodu ile sahip bir `ProblemDetails` gövdesi.</span><span class="sxs-lookup"><span data-stu-id="fab3b-197">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="fab3b-198">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fab3b-198">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="fab3b-199">Sorun ayrıntıları özelliği 2.2 veya üzeri uyumluluk bayrak gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fab3b-199">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="fab3b-200">Varsayılan davranışı devre dışı olduğunda `SuppressMapClientErrors` özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="fab3b-200">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="fab3b-201">Aşağıdaki kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fab3b-201">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="fab3b-202">Kullanım `ClientErrorMapping` içeriğini yapılandırmak için özellik `ProblemDetails` yanıt.</span><span class="sxs-lookup"><span data-stu-id="fab3b-202">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="fab3b-203">Örneğin, aşağıdaki güncelleştirmeleri kod `type` geçmesine karşın 404 yanıtlarını özelliği:</span><span class="sxs-lookup"><span data-stu-id="fab3b-203">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fab3b-204">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fab3b-204">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
