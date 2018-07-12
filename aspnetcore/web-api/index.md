---
title: Web API ASP.NET Core ile oluşturma
author: scottaddie
description: Web API ASP.NET Core ve her özelliği kullanmak, uygun olduğunda oluşturmak için kullanılabilen özellikler hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 0ff0bbc629930666d46247d6c1257fac8bfaf7c2
ms.sourcegitcommit: 260abb706ed17f07a53288d8a0c3e69fc13e7468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38966815"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="72393-103">Web API ASP.NET Core ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="72393-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="72393-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="72393-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="72393-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72393-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="72393-106">Bu belgede, bir web API ASP.NET Core ve her özelliği kullanmak en uygun olduğunda oluşturma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="72393-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="72393-107">Sınıf ControllerBase türetin.</span><span class="sxs-lookup"><span data-stu-id="72393-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="72393-108">Devralınan [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) web API'si hizmet vermek için hedeflenen bir denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="72393-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="72393-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72393-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="72393-110">`ControllerBase` Sınıfı çeşitli özelliklere ve yöntemlere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="72393-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="72393-111">Önceki kodda; örnekler [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) ve [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="72393-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="72393-112">HTTP 400 ve 201 durum kodları, sırasıyla döndürülecek eylem yöntemlerinde bu yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="72393-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="72393-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) ayrıca tarafından sağlanan özellik, `ControllerBase`, model doğrulama isteği işlemek için erişilir.</span><span class="sxs-lookup"><span data-stu-id="72393-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="72393-114">Sınıf ApiControllerAttribute ile Not Ekle</span><span class="sxs-lookup"><span data-stu-id="72393-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="72393-115">ASP.NET Core 2.1 tanıtır [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) bir web API denetleyici sınıfı belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="72393-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="72393-116">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72393-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="72393-117">2.1 veya üzeri uyumluluk sürümüyle kümesi aracılığıyla [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), bu öznitelik kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="72393-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="72393-118">Örneğin, vurgulanan kodu *Startup.ConfigureServices* 2.1 Uyumluluk bayrağını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="72393-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="72393-119">`[ApiController]` Özniteliği ile birlikte sık `ControllerBase` denetleyicileri için REST özgü davranışı etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="72393-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="72393-120">`ControllerBase` yöntemleri erişim gibi sağlar [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) ve [dosya](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="72393-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="72393-121">İle ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için başka bir yaklaşımdır `[ApiController]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="72393-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="72393-122">Aşağıdaki bölümlerde öznitelik tarafından eklenen kullanışlı özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="72393-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="72393-123">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="72393-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="72393-124">Doğrulama hataları, HTTP 400 yanıt otomatik olarak tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="72393-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="72393-125">Aşağıdaki kod, Eylemler gereksiz olur:</span><span class="sxs-lookup"><span data-stu-id="72393-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="72393-126">Varsayılan davranışı devre dışı olduğunda [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="72393-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="72393-127">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="72393-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="72393-128">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="72393-128">Binding source parameter inference</span></span>

<span data-ttu-id="72393-129">Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar.</span><span class="sxs-lookup"><span data-stu-id="72393-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="72393-130">Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="72393-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="72393-131">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="72393-131">Attribute</span></span>|<span data-ttu-id="72393-132">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="72393-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="72393-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="72393-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="72393-134">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="72393-134">Request body</span></span> |
|<span data-ttu-id="72393-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="72393-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="72393-136">İstek gövdesini form verilerini</span><span class="sxs-lookup"><span data-stu-id="72393-136">Form data in the request body</span></span> |
|<span data-ttu-id="72393-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="72393-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="72393-138">İstek üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="72393-138">Request header</span></span> |
|<span data-ttu-id="72393-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="72393-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="72393-140">İstek sorgu dizesi parametresi</span><span class="sxs-lookup"><span data-stu-id="72393-140">Request query string parameter</span></span> |
|<span data-ttu-id="72393-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="72393-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="72393-142">Geçerli istek için rota verilerini</span><span class="sxs-lookup"><span data-stu-id="72393-142">Route data from the current request</span></span> |
|<span data-ttu-id="72393-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="72393-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="72393-144">Bir eylem parametresi olarak eklenen isteği hizmeti</span><span class="sxs-lookup"><span data-stu-id="72393-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="72393-145">Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`).</span><span class="sxs-lookup"><span data-stu-id="72393-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="72393-146">`%2f` unescaped olmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="72393-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="72393-147">Kullanım `[FromQuery]` değer içerebilir, `%2f`.</span><span class="sxs-lookup"><span data-stu-id="72393-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="72393-148">Olmadan `[ApiController]` öznitelikler açıkça tanımlanmış kaynağını bağlama özniteliği.</span><span class="sxs-lookup"><span data-stu-id="72393-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="72393-149">Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="72393-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="72393-150">Eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="72393-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="72393-151">Bu kurallar, aksi takdirde el ile eylem parametrelerine uygulamak olası kaynakları bağlama yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72393-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="72393-152">Bağlama kaynağı öznitelikleri gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="72393-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="72393-153">**[FromBody]**  karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="72393-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="72393-154">Bu kural için bir özel herhangi bir karmaşık, yerleşik türü özel bir anlama sahip olduğu gibi [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) ve [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="72393-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="72393-155">Bağlama kaynağı çıkarımı kod türlerine özel yok sayar.</span><span class="sxs-lookup"><span data-stu-id="72393-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="72393-156">Açıkça belirtilen birden fazla parametre olduğunda bir eylem (aracılığıyla `[FromBody]`) veya istek gövdesinden bağlı olarak çıkarılan, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72393-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="72393-157">Örneğin, aşağıdaki eylemi imzaları bir özel duruma neden:</span><span class="sxs-lookup"><span data-stu-id="72393-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="72393-158">**[FromForm]**  için eylem parametrelerini tür çıkarımı yapılan [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="72393-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="72393-159">Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.</span><span class="sxs-lookup"><span data-stu-id="72393-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="72393-160">**[FromRoute]**  rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="72393-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="72393-161">Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="72393-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="72393-162">**[FromQuery]**  için başka bir eylem parametrelerini algılanır.</span><span class="sxs-lookup"><span data-stu-id="72393-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="72393-163">Varsayılan çıkarım kuralları devre dışı olduğunda [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="72393-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="72393-164">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="72393-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="72393-165">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="72393-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="72393-166">Ne zaman bir eylem parametresinin açıklama ile [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) özniteliği `multipart/form-data` içerik türü çıkarılan isteyin.</span><span class="sxs-lookup"><span data-stu-id="72393-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="72393-167">Varsayılan davranışı devre dışı olduğunda [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="72393-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="72393-168">Aşağıdaki kodu ekleyin *Startup.ConfigureServices* sonra `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="72393-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="72393-169">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="72393-169">Attribute routing requirement</span></span>

<span data-ttu-id="72393-170">Öznitelik yönlendirme, bir gereksinim haline gelir.</span><span class="sxs-lookup"><span data-stu-id="72393-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="72393-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72393-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="72393-172">Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tanımlanan [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ya da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) içinde *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="72393-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="72393-173">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="72393-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
