---
title: ASP.NET Core ile Web API 'Leri oluşturma
author: scottaddie
description: ASP.NET Core ' de Web API 'SI oluşturmanın temellerini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: 6e1868690a2c384307a23c89467505d3ed8916db
ms.sourcegitcommit: 805f625d16d74e77f02f5f37326e5aceafcb78e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985469"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="f06df-103">ASP.NET Core ile Web API 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="f06df-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="f06df-104">[Scott Ade](https://github.com/scottaddie) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="f06df-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f06df-105">ASP.NET Core, kullanarak C#Web API 'leri olarak da bilinen, yeniden oluşturulan hizmetler oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="f06df-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="f06df-106">İstekleri işlemek için, bir Web API 'SI denetleyicileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="f06df-107">Bir Web API 'sindeki *denetleyiciler* öğesinden `ControllerBase`türetilen sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="f06df-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="f06df-108">Bu makalede, Web API isteklerini işlemek için denetleyicilerin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f06df-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="f06df-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="f06df-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="f06df-110">([İndirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f06df-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="f06df-111">ControllerBase sınıfı</span><span class="sxs-lookup"><span data-stu-id="f06df-111">ControllerBase class</span></span>

<span data-ttu-id="f06df-112">Bir Web API 'SI, öğesinden <xref:Microsoft.AspNetCore.Mvc.ControllerBase>türetilen bir veya daha fazla denetleyici sınıfından oluşur.</span><span class="sxs-lookup"><span data-stu-id="f06df-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="f06df-113">Web API proje şablonu bir başlatıcı denetleyicisi sağlar:</span><span class="sxs-lookup"><span data-stu-id="f06df-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="f06df-114"><xref:Microsoft.AspNetCore.Mvc.Controller> Sınıfından türeterek bir Web API denetleyicisi oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="f06df-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="f06df-115">`Controller`' dan `ControllerBase` türetilir ve görünümler için destek ekler, bu nedenle Web API istekleri için değil Web sayfalarını işlemeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="f06df-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="f06df-116">Bu kural için bir özel durum var: aynı denetleyiciyi hem görünümler hem de Web API 'Leri için kullanmayı planlıyorsanız, öğesinden `Controller`türetirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f06df-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="f06df-117">Sınıfı `ControllerBase` , http isteklerini işlemek için yararlı olan birçok özellik ve yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="f06df-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="f06df-118">Örneğin, `ControllerBase.CreatedAtAction` bir 201 durum kodu döndürür:</span><span class="sxs-lookup"><span data-stu-id="f06df-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="f06df-119">`ControllerBase` Sağladığı yöntemlere ilişkin bazı örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f06df-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="f06df-120">Yöntem</span><span class="sxs-lookup"><span data-stu-id="f06df-120">Method</span></span>   |<span data-ttu-id="f06df-121">Notlar</span><span class="sxs-lookup"><span data-stu-id="f06df-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="f06df-122">400 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f06df-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|<span data-ttu-id="f06df-123">404 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f06df-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="f06df-124">Bir dosya döndürür.</span><span class="sxs-lookup"><span data-stu-id="f06df-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="f06df-125">[Model bağlamasını](xref:mvc/models/model-binding)çağırır.</span><span class="sxs-lookup"><span data-stu-id="f06df-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="f06df-126">[Model doğrulamasını](xref:mvc/models/validation)çağırır.</span><span class="sxs-lookup"><span data-stu-id="f06df-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="f06df-127">Tüm kullanılabilir yöntemlerin ve özelliklerin listesi için bkz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="f06df-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="f06df-128">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="f06df-128">Attributes</span></span>

<span data-ttu-id="f06df-129">Ad <xref:Microsoft.AspNetCore.Mvc> alanı, Web API denetleyicileri ve eylem yöntemlerinin davranışını yapılandırmak için kullanılabilecek öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f06df-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="f06df-130">Aşağıdaki örnek, desteklenen HTTP eylem fiilini ve döndürülebilecek bilinen HTTP durum kodlarını belirtmek için özniteliklerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="f06df-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="f06df-131">Aşağıda, kullanılabilecek özniteliklerin daha fazla örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f06df-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="f06df-132">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="f06df-132">Attribute</span></span>|<span data-ttu-id="f06df-133">Notlar</span><span class="sxs-lookup"><span data-stu-id="f06df-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="f06df-134">[Yolu](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="f06df-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="f06df-135">Bir denetleyicinin veya eylemin URL modelini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f06df-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="f06df-136">[Bağladığınızda](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="f06df-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="f06df-137">Model bağlama için dahil edilecek öneki ve özellikleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="f06df-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="f06df-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="f06df-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="f06df-139">HTTP GET ACTION fiilini destekleyen bir eylemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f06df-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="f06df-140">[Harc](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="f06df-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="f06df-141">Bir eylemin kabul ettiği veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f06df-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="f06df-142">[Üretmez](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="f06df-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="f06df-143">Bir eylemin döndürdüğü veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f06df-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="f06df-144">Kullanılabilir öznitelikleri içeren bir liste için, bkz <xref:Microsoft.AspNetCore.Mvc> . ad alanı.</span><span class="sxs-lookup"><span data-stu-id="f06df-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="f06df-145">ApiController özniteliği</span><span class="sxs-lookup"><span data-stu-id="f06df-145">ApiController attribute</span></span>

<span data-ttu-id="f06df-146">[[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği bir denetleyici sınıfına uygulanabilir ve bu, API 'ye özgü aşağıdaki davranışları etkinleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f06df-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="f06df-147">Öznitelik yönlendirme gereksinimi</span><span class="sxs-lookup"><span data-stu-id="f06df-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="f06df-148">Otomatik HTTP 400 yanıtları</span><span class="sxs-lookup"><span data-stu-id="f06df-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="f06df-149">Bağlama kaynak parametresi çıkarımı</span><span class="sxs-lookup"><span data-stu-id="f06df-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="f06df-150">Multipart/form-veri isteği çıkarımı</span><span class="sxs-lookup"><span data-stu-id="f06df-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="f06df-151">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="f06df-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="f06df-152">Bu özellikler, 2,1 veya üzeri bir [Uyumluluk sürümü](xref:mvc/compatibility-version) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f06df-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="f06df-153">Belirli denetleyicilerde öznitelik</span><span class="sxs-lookup"><span data-stu-id="f06df-153">Attribute on specific controllers</span></span>

<span data-ttu-id="f06df-154">`[ApiController]` Özniteliği, proje şablonundan aşağıdaki örnekte olduğu gibi belirli denetleyicilere uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="f06df-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="f06df-155">Birden çok denetleyicilerde öznitelik</span><span class="sxs-lookup"><span data-stu-id="f06df-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="f06df-156">Özniteliği birden fazla denetleyicide kullanmanın bir yaklaşımı, `[ApiController]` özniteliğiyle birlikte açıklanan özel bir temel denetleyici sınıfı oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="f06df-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="f06df-157">Aşağıdaki örnekte, özel bir temel sınıf ve ondan türetilen bir denetleyici gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f06df-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="f06df-158">Bir derlemedeki öznitelik</span><span class="sxs-lookup"><span data-stu-id="f06df-158">Attribute on an assembly</span></span>

<span data-ttu-id="f06df-159">[Uyumluluk sürümü](xref:mvc/compatibility-version) 2,2 veya üzeri bir sürüme ayarlandıysa, `[ApiController]` öznitelik bir derlemeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="f06df-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="f06df-160">Bu şekilde ek açıklama, derlemedeki tüm denetleyicilere Web API davranışını uygular.</span><span class="sxs-lookup"><span data-stu-id="f06df-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="f06df-161">Tek tek denetleyiciler için geri alma yöntemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="f06df-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="f06df-162">Derleme düzeyi özniteliğini `Startup` sınıfı çevreleyen ad alanı bildirimine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f06df-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="f06df-163">Öznitelik yönlendirme gereksinimi</span><span class="sxs-lookup"><span data-stu-id="f06df-163">Attribute routing requirement</span></span>

<span data-ttu-id="f06df-164">Öznitelik `[ApiController]` , öznitelik yönlendirme bir gereksinim yapar.</span><span class="sxs-lookup"><span data-stu-id="f06df-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="f06df-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f06df-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="f06df-166">Eylemlere `UseEndpoints`, ,<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>veya içinde<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> [](xref:mvc/controllers/routing#conventional-routing) tanımlanangelenekselyollararacılığıyla`Startup.Configure`erişilemez.</span><span class="sxs-lookup"><span data-stu-id="f06df-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="f06df-167">Eylemlere, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde [](xref:mvc/controllers/routing#conventional-routing) tanımlanangelenekselyollararacılığıyla`Startup.Configure`erişilemez.</span><span class="sxs-lookup"><span data-stu-id="f06df-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="f06df-168">Otomatik HTTP 400 yanıtları</span><span class="sxs-lookup"><span data-stu-id="f06df-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="f06df-169">Öznitelik `[ApiController]` , model doğrulama hatalarının otomatik olarak bir HTTP 400 yanıtı tetiklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="f06df-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="f06df-170">Sonuç olarak, aşağıdaki kod bir eylem yönteminde gereksizdir:</span><span class="sxs-lookup"><span data-stu-id="f06df-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="f06df-171">ASP.NET Core MVC, önceki <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> denetimi yapmak için eylem filtresini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="f06df-172">Varsayılan BadRequest yanıtı</span><span class="sxs-lookup"><span data-stu-id="f06df-172">Default BadRequest response</span></span> 

<span data-ttu-id="f06df-173">Uyumluluk sürümü 2,1 ile, bir HTTP 400 yanıtı için varsayılan yanıt türü ' dir <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="f06df-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="f06df-174">Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="f06df-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f06df-175">2,2 veya üzeri bir uyumluluk sürümü ile, bir HTTP 400 yanıtı için varsayılan yanıt türü ' dir <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="f06df-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="f06df-176">Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="f06df-176">The following request body is an example of the serialized type:</span></span>

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

<span data-ttu-id="f06df-177">`ValidationProblemDetails` Tür:</span><span class="sxs-lookup"><span data-stu-id="f06df-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="f06df-178">Web API yanıtlarında hata belirtmek için makine tarafından okunabilen bir biçim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f06df-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="f06df-179">[RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807)uyar.</span><span class="sxs-lookup"><span data-stu-id="f06df-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="f06df-180">Varsayılan yanıt türünü olarak `SerializableError`değiştirmek için, vurgulanan değişiklikleri içine `Startup.ConfigureServices`uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f06df-180">To change the default response type to `SerializableError`, apply the highlighted changes in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

### <a name="customize-badrequest-response"></a><span data-ttu-id="f06df-181">BadRequest yanıtını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f06df-181">Customize BadRequest response</span></span>

<span data-ttu-id="f06df-182">Doğrulama hatasından kaynaklanan yanıtı özelleştirmek için kullanın <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="f06df-182">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="f06df-183">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f06df-183">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=4-20)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=5-21)]

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="f06df-184">Otomatik 400 yanıtlarını günlüğe kaydet</span><span class="sxs-lookup"><span data-stu-id="f06df-184">Log automatic 400 responses</span></span>

<span data-ttu-id="f06df-185">Bkz. [otomatik 400 yanıtlarını model doğrulama hatalarında günlüğe kaydetme (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="f06df-185">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="f06df-186">Otomatik 400 yanıtını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="f06df-186">Disable automatic 400 response</span></span>

<span data-ttu-id="f06df-187">Otomatik 400 davranışını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliğini olarak `true`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f06df-187">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="f06df-188">Aşağıdaki Vurgulanan kodu içine `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f06df-188">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="f06df-189">Bağlama kaynak parametresi çıkarımı</span><span class="sxs-lookup"><span data-stu-id="f06df-189">Binding source parameter inference</span></span>

<span data-ttu-id="f06df-190">Bağlama kaynak özniteliği, bir eylem parametresi değerinin bulunduğu konumu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f06df-190">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="f06df-191">Aşağıdaki bağlama kaynak öznitelikleri var:</span><span class="sxs-lookup"><span data-stu-id="f06df-191">The following binding source attributes exist:</span></span>

|<span data-ttu-id="f06df-192">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="f06df-192">Attribute</span></span>|<span data-ttu-id="f06df-193">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="f06df-193">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="f06df-194">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="f06df-194">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="f06df-195">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="f06df-195">Request body</span></span> |
|<span data-ttu-id="f06df-196">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="f06df-196">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="f06df-197">İstek gövdesinde form verileri</span><span class="sxs-lookup"><span data-stu-id="f06df-197">Form data in the request body</span></span> |
|<span data-ttu-id="f06df-198">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="f06df-198">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="f06df-199">İstek üst bilgisi</span><span class="sxs-lookup"><span data-stu-id="f06df-199">Request header</span></span> |
|<span data-ttu-id="f06df-200">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="f06df-200">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="f06df-201">İstek sorgusu dize parametresi</span><span class="sxs-lookup"><span data-stu-id="f06df-201">Request query string parameter</span></span> |
|<span data-ttu-id="f06df-202">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="f06df-202">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="f06df-203">Geçerli istekten veri yönlendir</span><span class="sxs-lookup"><span data-stu-id="f06df-203">Route data from the current request</span></span> |
|<span data-ttu-id="f06df-204">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="f06df-204">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="f06df-205">Eylem parametresi olarak eklenen istek hizmeti</span><span class="sxs-lookup"><span data-stu-id="f06df-205">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="f06df-206">Değerler ( `[FromRoute]` yani `%2f`)içerdiğindekullanmayın `/`.</span><span class="sxs-lookup"><span data-stu-id="f06df-206">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="f06df-207">`%2f`atlanmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="f06df-207">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="f06df-208">Değer `[FromQuery]` içermesi`%2f`gerekiyorsa kullanın.</span><span class="sxs-lookup"><span data-stu-id="f06df-208">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="f06df-209">Özniteliği veya gibi `[FromQuery]`bağlama kaynak öznitelikleri olmadan, ASP.NET Core çalışma zamanı karmaşık nesne modeli cildi kullanmaya çalışır. `[ApiController]`</span><span class="sxs-lookup"><span data-stu-id="f06df-209">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="f06df-210">Karmaşık nesne modeli Ciltçi, verileri değer sağlayıcılarından tanımlı bir düzende çeker.</span><span class="sxs-lookup"><span data-stu-id="f06df-210">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="f06df-211">Aşağıdaki örnekte `[FromQuery]` öznitelik, istek URL 'sinin sorgu dizesinde `discontinuedOnly` parametre değerinin sağlandığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="f06df-211">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="f06df-212">`[ApiController]` Öznitelik, eylem parametrelerinin varsayılan veri kaynakları için çıkarım kurallarını uygular.</span><span class="sxs-lookup"><span data-stu-id="f06df-212">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="f06df-213">Bu kurallar, eylem parametrelerine öznitelikleri uygulayarak bağlama kaynaklarını el ile tanımlamak zorunda kalmadan sizi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f06df-213">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="f06df-214">Bağlama kaynak çıkarımı kuralları aşağıdaki gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="f06df-214">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="f06df-215">`[FromBody]`karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-215">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="f06df-216">`[FromBody]` Çıkarım kuralı için bir özel durum, <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>gibi özel bir anlamı olan karmaşık, yerleşik bir türdür.</span><span class="sxs-lookup"><span data-stu-id="f06df-216">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="f06df-217">Bağlama kaynak çıkarımı kodu bu özel türleri yoksayar.</span><span class="sxs-lookup"><span data-stu-id="f06df-217">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="f06df-218">`[FromForm]`, ve <xref:Microsoft.AspNetCore.Http.IFormFile> <xref:Microsoft.AspNetCore.Http.IFormFileCollection>türündeki eylem parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-218">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="f06df-219">Bu, herhangi bir basit veya Kullanıcı tanımlı tür için çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="f06df-219">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="f06df-220">`[FromRoute]`yol şablonundaki bir parametreyle eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-220">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="f06df-221">Birden fazla yol bir eylem parametresiyle eşleştiğinde, herhangi bir rota değeri kabul `[FromRoute]`edilir.</span><span class="sxs-lookup"><span data-stu-id="f06df-221">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="f06df-222">`[FromQuery]`diğer eylem parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-222">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="f06df-223">FromBody çıkarım notları</span><span class="sxs-lookup"><span data-stu-id="f06df-223">FromBody inference notes</span></span>

<span data-ttu-id="f06df-224">`[FromBody]`, `string` veya`int`gibi basit türler için çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="f06df-224">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="f06df-225">Bu nedenle, `[FromBody]` bu işlev gerektiğinde basit türler için özniteliği kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f06df-225">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="f06df-226">Bir eylem, istek gövdesinden birden fazla parametre bağlamışsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f06df-226">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="f06df-227">Örneğin, aşağıdaki eylem yöntemi imzalarının tümü bir özel duruma neden olur:</span><span class="sxs-lookup"><span data-stu-id="f06df-227">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="f06df-228">`[FromBody]`karmaşık türler olduklarından her ikisi de üzerinde algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-228">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="f06df-229">`[FromBody]`karmaşık bir tür olduğundan, diğeri üzerinde olan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f06df-229">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="f06df-230">`[FromBody]`her ikisinde de özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f06df-230">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="f06df-231">ASP.NET Core 2,1 ' de, listeler ve diziler gibi koleksiyon türü parametreleri yanlış olarak `[FromQuery]`algılanır.</span><span class="sxs-lookup"><span data-stu-id="f06df-231">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="f06df-232">`[FromBody]` Öznitelik, istek gövdesinden bağlanmaları durumunda bu parametreler için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f06df-232">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="f06df-233">Bu davranış ASP.NET Core 2,2 veya sonraki bir sürümde düzeltilir. burada, koleksiyon türü parametrelerinin varsayılan olarak gövdeden bağlanacak şekilde çıkarsandır.</span><span class="sxs-lookup"><span data-stu-id="f06df-233">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="f06df-234">Çıkarım kurallarını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="f06df-234">Disable inference rules</span></span>

<span data-ttu-id="f06df-235">Bağlama kaynak çıkarımını devre dışı bırakmak <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> için `true`, olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f06df-235">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="f06df-236">Aşağıdaki kodu içine `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f06df-236">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="f06df-237">Multipart/form-veri isteği çıkarımı</span><span class="sxs-lookup"><span data-stu-id="f06df-237">Multipart/form-data request inference</span></span>

<span data-ttu-id="f06df-238">Öznitelik, [[fromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliğiyle bir eylem parametresine ek açıklama eklendiğinde bir çıkarım kuralı uygular. `[ApiController]`</span><span class="sxs-lookup"><span data-stu-id="f06df-238">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="f06df-239">`multipart/form-data` İstek içerik türü çıkarsandı.</span><span class="sxs-lookup"><span data-stu-id="f06df-239">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="f06df-240">Varsayılan davranışı devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> `true` özelliğini olarak `Startup.ConfigureServices`ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f06df-240">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="f06df-241">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="f06df-241">Problem details for error status codes</span></span>

<span data-ttu-id="f06df-242">Uyumluluk sürümü 2,2 veya üzeri olduğunda, MVC bir hata sonucunu (durum kodu 400 veya üzeri olan bir sonuç) ile <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>bir sonuçla dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f06df-242">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="f06df-243">Türü, bir http yanıtında makine tarafından okunabilen hata ayrıntılarını sağlamak için [RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807) dayalıdır. `ProblemDetails`</span><span class="sxs-lookup"><span data-stu-id="f06df-243">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="f06df-244">Bir denetleyici eyleminde aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="f06df-244">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="f06df-245">Yöntemi `NotFound` , `ProblemDetails` gövdesi olan bir HTTP 404 durum kodu üretir.</span><span class="sxs-lookup"><span data-stu-id="f06df-245">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="f06df-246">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f06df-246">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="f06df-247">ProblemDetails yanıtını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f06df-247">Customize ProblemDetails response</span></span>

<span data-ttu-id="f06df-248">Yanıtıniçeriğini`ProblemDetails` yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="f06df-248">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="f06df-249">Örneğin, aşağıdaki kod 404 yanıtlarının `type` özelliğini güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="f06df-249">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end

### <a name="disable-problemdetails-response"></a><span data-ttu-id="f06df-250">ProblemDetails yanıtını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="f06df-250">Disable ProblemDetails response</span></span>

<span data-ttu-id="f06df-251"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> Özelliği `ProblemDetails` olarak`true`ayarlandığında, bir örneğin otomatik olarak oluşturulması devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="f06df-251">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> property is set to `true`.</span></span> <span data-ttu-id="f06df-252">Aşağıdaki kodu içine `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f06df-252">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f06df-253">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f06df-253">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
