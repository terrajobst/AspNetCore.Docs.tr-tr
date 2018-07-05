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
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="4a5d5-103">Web API ASP.NET Core ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="4a5d5-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="4a5d5-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="4a5d5-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="4a5d5-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a5d5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4a5d5-106">Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="4a5d5-107">Sınıf ControllerBase türetin.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="4a5d5-108">Devralınan [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="4a5d5-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="4a5d5-110">`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="4a5d5-111">Önceki örnekte, böyle bir yöntemleri dahil [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ve [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="4a5d5-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="4a5d5-112">Bu yöntemler, HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="4a5d5-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) ayrıca tarafından sağlanan özellik, `ControllerBase`, istek modeli doğrulama gerçekleştirmek için erişilir.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="4a5d5-114">Sınıf ApiControllerAttribute ile Not Ekle</span><span class="sxs-lookup"><span data-stu-id="4a5d5-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="4a5d5-115">ASP.NET Core 2.1 tanıtır [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) bir web API denetleyici sınıfı belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="4a5d5-116">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="4a5d5-117">Bu öznitelik genellikle ile birlikte `ControllerBase` yararlı yöntemlere ve özelliklere erişim kazanmak için.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="4a5d5-118">`ControllerBase` yöntemleri erişim gibi sağlar [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ve [dosya](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="4a5d5-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="4a5d5-119">İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="4a5d5-120">Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="4a5d5-121">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="4a5d5-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="4a5d5-122">Doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="4a5d5-123">Aşağıdaki kod, Eylemler gereksiz olur:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="4a5d5-124">Aşağıdaki kod ile bu varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="4a5d5-125">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="4a5d5-125">Binding source parameter inference</span></span>

<span data-ttu-id="4a5d5-126">Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="4a5d5-127">Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="4a5d5-128">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="4a5d5-128">Attribute</span></span>|<span data-ttu-id="4a5d5-129">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="4a5d5-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="4a5d5-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="4a5d5-131">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="4a5d5-131">Request body</span></span> |
|<span data-ttu-id="4a5d5-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="4a5d5-133">İstek gövdesini form verilerini</span><span class="sxs-lookup"><span data-stu-id="4a5d5-133">Form data in the request body</span></span> |
|<span data-ttu-id="4a5d5-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="4a5d5-135">İstek üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="4a5d5-135">Request header</span></span> |
|<span data-ttu-id="4a5d5-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="4a5d5-137">İstek sorgu dizesi parametresi</span><span class="sxs-lookup"><span data-stu-id="4a5d5-137">Request query string parameter</span></span> |
|<span data-ttu-id="4a5d5-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="4a5d5-139">Geçerli istek için rota verilerini</span><span class="sxs-lookup"><span data-stu-id="4a5d5-139">Route data from the current request</span></span> |
|<span data-ttu-id="4a5d5-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="4a5d5-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="4a5d5-141">Bir eylem parametresi olarak eklenen isteği hizmeti</span><span class="sxs-lookup"><span data-stu-id="4a5d5-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="4a5d5-142">Yapmak **değil** kullanın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`) çünkü `%2f` unescaped olmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="4a5d5-143">Kullanım `[FromQuery]` değer içerebilir, `%2f`.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="4a5d5-144">Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="4a5d5-145">Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="4a5d5-146">Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="4a5d5-147">Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="4a5d5-148">Bağlama kaynağı öznitelikleri gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="4a5d5-149">**[FromBody]**  karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="4a5d5-150">Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) ve [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="4a5d5-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="4a5d5-151">Bağlama kaynağı çıkarımı kod türlerine özel yok sayar.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="4a5d5-152">Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="4a5d5-153">Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="4a5d5-154">**[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="4a5d5-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="4a5d5-155">Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="4a5d5-156">**[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="4a5d5-157">Birden çok yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="4a5d5-158">**[FromQuery]**  için başka bir eylem parametrelerini algılanır.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="4a5d5-159">Aşağıdaki kod ile varsayılan çıkarım kuralları devre dışı *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="4a5d5-160">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="4a5d5-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="4a5d5-161">Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="4a5d5-162">Aşağıdaki kod ile varsayılan davranışı devre dışı bırakılır *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="4a5d5-163">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="4a5d5-163">Attribute routing requirement</span></span>

<span data-ttu-id="4a5d5-164">Öznitelik yönlendirme, bir gereksinim haline gelir.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="4a5d5-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4a5d5-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="4a5d5-166">Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ya da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) içinde *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="4a5d5-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4a5d5-167">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4a5d5-167">Additional resources</span></span>

* [<span data-ttu-id="4a5d5-168">Denetleyici eylemi dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="4a5d5-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="4a5d5-169">Özel biçimlendiriciler</span><span class="sxs-lookup"><span data-stu-id="4a5d5-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="4a5d5-170">Yanıt verilerini biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="4a5d5-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="4a5d5-171">Swagger kullanan yardım sayfaları</span><span class="sxs-lookup"><span data-stu-id="4a5d5-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="4a5d5-172">Denetleyici eylemlerine yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4a5d5-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
