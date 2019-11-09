---
title: ASP.NET Core ile Web API 'Leri oluşturma
author: scottaddie
description: ASP.NET Core ' de Web API 'SI oluşturmanın temellerini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: 122de0a225668a7523eec900e2ad8fdac56d7886
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73897017"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="607a0-103">ASP.NET Core ile Web API 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="607a0-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="607a0-104">[Scott Ade](https://github.com/scottaddie) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="607a0-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="607a0-105">ASP.NET Core, kullanarak C#Web API 'leri olarak da bilinen, yeniden oluşturulan hizmetler oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="607a0-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="607a0-106">İstekleri işlemek için, bir Web API 'SI denetleyicileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="607a0-107">Bir Web API 'sindeki *denetleyiciler* `ControllerBase`türetilen sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="607a0-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="607a0-108">Bu makalede, Web API isteklerini işlemek için denetleyicilerin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="607a0-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="607a0-109">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="607a0-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="607a0-110">([İndirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="607a0-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="607a0-111">ControllerBase sınıfı</span><span class="sxs-lookup"><span data-stu-id="607a0-111">ControllerBase class</span></span>

<span data-ttu-id="607a0-112">Bir Web API 'SI <xref:Microsoft.AspNetCore.Mvc.ControllerBase>türetilen bir veya daha fazla denetleyici sınıfından oluşur.</span><span class="sxs-lookup"><span data-stu-id="607a0-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="607a0-113">Web API proje şablonu bir başlatıcı denetleyicisi sağlar:</span><span class="sxs-lookup"><span data-stu-id="607a0-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="607a0-114"><xref:Microsoft.AspNetCore.Mvc.Controller> sınıfından türeterek bir Web API denetleyicisi oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="607a0-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="607a0-115">`Controller` `ControllerBase` türetilir ve görünümler için destek ekler, bu nedenle Web API istekleri için değil Web sayfalarını işlemeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="607a0-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="607a0-116">Bu kural için bir özel durum var: aynı denetleyiciyi hem görünümler hem de Web API 'Leri için kullanmayı planlıyorsanız, `Controller`türetirsiniz.</span><span class="sxs-lookup"><span data-stu-id="607a0-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="607a0-117">`ControllerBase` sınıfı, HTTP isteklerini işlemek için yararlı olan çok sayıda özellik ve yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="607a0-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="607a0-118">Örneğin, `ControllerBase.CreatedAtAction` 201 durum kodu döndürür:</span><span class="sxs-lookup"><span data-stu-id="607a0-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="607a0-119">`ControllerBase` sağladığı yöntemlere ilişkin bazı örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="607a0-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="607a0-120">Yöntem</span><span class="sxs-lookup"><span data-stu-id="607a0-120">Method</span></span>   |<span data-ttu-id="607a0-121">Notlar</span><span class="sxs-lookup"><span data-stu-id="607a0-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="607a0-122">400 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="607a0-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|<span data-ttu-id="607a0-123">404 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="607a0-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="607a0-124">Bir dosya döndürür.</span><span class="sxs-lookup"><span data-stu-id="607a0-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="607a0-125">[Model bağlamasını](xref:mvc/models/model-binding)çağırır.</span><span class="sxs-lookup"><span data-stu-id="607a0-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="607a0-126">[Model doğrulamasını](xref:mvc/models/validation)çağırır.</span><span class="sxs-lookup"><span data-stu-id="607a0-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="607a0-127">Tüm kullanılabilir yöntemlerin ve özelliklerin listesi için bkz. <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="607a0-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="607a0-128">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="607a0-128">Attributes</span></span>

<span data-ttu-id="607a0-129"><xref:Microsoft.AspNetCore.Mvc> ad alanı, Web API denetleyicileri ve eylem yöntemlerinin davranışını yapılandırmak için kullanılabilen öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="607a0-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="607a0-130">Aşağıdaki örnek, desteklenen HTTP eylem fiilini ve döndürülebilecek bilinen HTTP durum kodlarını belirtmek için özniteliklerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="607a0-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="607a0-131">Aşağıda, kullanılabilecek özniteliklerin daha fazla örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="607a0-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="607a0-132">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="607a0-132">Attribute</span></span>|<span data-ttu-id="607a0-133">Notlar</span><span class="sxs-lookup"><span data-stu-id="607a0-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="607a0-134">[Yolu](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="607a0-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="607a0-135">Bir denetleyicinin veya eylemin URL modelini belirtir.</span><span class="sxs-lookup"><span data-stu-id="607a0-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="607a0-136">[Bağladığınızda](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="607a0-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="607a0-137">Model bağlama için dahil edilecek öneki ve özellikleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="607a0-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="607a0-138">[HttpGet](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="607a0-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="607a0-139">HTTP GET ACTION fiilini destekleyen bir eylemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="607a0-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="607a0-140">[Harc](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="607a0-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="607a0-141">Bir eylemin kabul ettiği veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="607a0-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="607a0-142">[Üretmez](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="607a0-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="607a0-143">Bir eylemin döndürdüğü veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="607a0-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="607a0-144">Kullanılabilir öznitelikleri içeren bir liste için <xref:Microsoft.AspNetCore.Mvc> ad alanına bakın.</span><span class="sxs-lookup"><span data-stu-id="607a0-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="607a0-145">ApiController özniteliği</span><span class="sxs-lookup"><span data-stu-id="607a0-145">ApiController attribute</span></span>

<span data-ttu-id="607a0-146">[[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği bir denetleyici sınıfına uygulanabilir ve bu, API 'ye özgü aşağıdaki davranışları etkinleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="607a0-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="607a0-147">Öznitelik yönlendirme gereksinimi</span><span class="sxs-lookup"><span data-stu-id="607a0-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="607a0-148">Otomatik HTTP 400 yanıtları</span><span class="sxs-lookup"><span data-stu-id="607a0-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="607a0-149">Bağlama kaynak parametresi çıkarımı</span><span class="sxs-lookup"><span data-stu-id="607a0-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="607a0-150">Multipart/form-veri isteği çıkarımı</span><span class="sxs-lookup"><span data-stu-id="607a0-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="607a0-151">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="607a0-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="607a0-152">Bu özellikler, 2,1 veya üzeri bir [Uyumluluk sürümü](xref:mvc/compatibility-version) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="607a0-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="607a0-153">Belirli denetleyicilerde öznitelik</span><span class="sxs-lookup"><span data-stu-id="607a0-153">Attribute on specific controllers</span></span>

<span data-ttu-id="607a0-154">`[ApiController]` özniteliği, proje şablonundan aşağıdaki örnekte olduğu gibi belirli denetleyicilere uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="607a0-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="607a0-155">Birden çok denetleyicilerde öznitelik</span><span class="sxs-lookup"><span data-stu-id="607a0-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="607a0-156">Özniteliği birden fazla denetleyicide kullanmanın bir yaklaşımı, `[ApiController]` özniteliğiyle açıklanmış bir özel temel denetleyici sınıfı oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="607a0-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="607a0-157">Aşağıdaki örnekte, özel bir temel sınıf ve ondan türetilen bir denetleyici gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="607a0-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="607a0-158">Bir derlemedeki öznitelik</span><span class="sxs-lookup"><span data-stu-id="607a0-158">Attribute on an assembly</span></span>

<span data-ttu-id="607a0-159">[Uyumluluk sürümü](xref:mvc/compatibility-version) 2,2 veya üzeri bir sürüme ayarlandıysa, `[ApiController]` özniteliği bir derlemeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="607a0-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="607a0-160">Bu şekilde ek açıklama, derlemedeki tüm denetleyicilere Web API davranışını uygular.</span><span class="sxs-lookup"><span data-stu-id="607a0-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="607a0-161">Tek tek denetleyiciler için geri alma yöntemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="607a0-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="607a0-162">Derleme düzeyi özniteliğini `Startup` sınıfını çevreleyen ad alanı bildirimine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="607a0-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="607a0-163">Öznitelik yönlendirme gereksinimi</span><span class="sxs-lookup"><span data-stu-id="607a0-163">Attribute routing requirement</span></span>

<span data-ttu-id="607a0-164">`[ApiController]` özniteliği öznitelik yönlendirme bir gereksinim yapar.</span><span class="sxs-lookup"><span data-stu-id="607a0-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="607a0-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="607a0-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="607a0-166">Eylemler `Startup.Configure`içinde `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> tarafından tanımlanan [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) aracılığıyla erişilemez.</span><span class="sxs-lookup"><span data-stu-id="607a0-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="607a0-167">Eylemler `Startup.Configure`<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> tarafından tanımlanan [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) aracılığıyla erişilemez.</span><span class="sxs-lookup"><span data-stu-id="607a0-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="607a0-168">Otomatik HTTP 400 yanıtları</span><span class="sxs-lookup"><span data-stu-id="607a0-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="607a0-169">`[ApiController]` özniteliği, model doğrulama hatalarının otomatik olarak bir HTTP 400 yanıtı tetiklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="607a0-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="607a0-170">Sonuç olarak, aşağıdaki kod bir eylem yönteminde gereksizdir:</span><span class="sxs-lookup"><span data-stu-id="607a0-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="607a0-171">ASP.NET Core MVC, önceki denetimi yapmak için <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> eylem filtresini kullanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="607a0-172">Varsayılan BadRequest yanıtı</span><span class="sxs-lookup"><span data-stu-id="607a0-172">Default BadRequest response</span></span>

<span data-ttu-id="607a0-173">2,1 uyumluluk sürümü ile bir HTTP 400 yanıtı için varsayılan yanıt türü <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="607a0-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="607a0-174">Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="607a0-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="607a0-175">2,2 veya üzeri bir uyumluluk sürümü ile, bir HTTP 400 yanıtı için varsayılan yanıt türü <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="607a0-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="607a0-176">Aşağıdaki istek gövdesi, serileştirilmiş türün bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="607a0-176">The following request body is an example of the serialized type:</span></span>

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

<span data-ttu-id="607a0-177">`ValidationProblemDetails` türü:</span><span class="sxs-lookup"><span data-stu-id="607a0-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="607a0-178">Web API yanıtlarında hata belirtmek için makine tarafından okunabilen bir biçim sağlar.</span><span class="sxs-lookup"><span data-stu-id="607a0-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="607a0-179">[RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807)uyar.</span><span class="sxs-lookup"><span data-stu-id="607a0-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="607a0-180">Otomatik 400 yanıtlarını günlüğe kaydet</span><span class="sxs-lookup"><span data-stu-id="607a0-180">Log automatic 400 responses</span></span>

<span data-ttu-id="607a0-181">Bkz. [otomatik 400 yanıtlarını model doğrulama hatalarında günlüğe kaydetme (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="607a0-181">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="607a0-182">Otomatik 400 yanıtını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="607a0-182">Disable automatic 400 response</span></span>

<span data-ttu-id="607a0-183">Otomatik 400 davranışını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliğini `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="607a0-183">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="607a0-184">Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="607a0-184">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="607a0-185">Bağlama kaynak parametresi çıkarımı</span><span class="sxs-lookup"><span data-stu-id="607a0-185">Binding source parameter inference</span></span>

<span data-ttu-id="607a0-186">Bağlama kaynak özniteliği, bir eylem parametresi değerinin bulunduğu konumu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="607a0-186">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="607a0-187">Aşağıdaki bağlama kaynak öznitelikleri var:</span><span class="sxs-lookup"><span data-stu-id="607a0-187">The following binding source attributes exist:</span></span>

|<span data-ttu-id="607a0-188">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="607a0-188">Attribute</span></span>|<span data-ttu-id="607a0-189">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="607a0-189">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="607a0-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="607a0-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="607a0-191">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="607a0-191">Request body</span></span> |
|<span data-ttu-id="607a0-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="607a0-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="607a0-193">İstek gövdesinde form verileri</span><span class="sxs-lookup"><span data-stu-id="607a0-193">Form data in the request body</span></span> |
|<span data-ttu-id="607a0-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="607a0-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="607a0-195">İstek üst bilgisi</span><span class="sxs-lookup"><span data-stu-id="607a0-195">Request header</span></span> |
|<span data-ttu-id="607a0-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="607a0-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="607a0-197">İstek sorgusu dize parametresi</span><span class="sxs-lookup"><span data-stu-id="607a0-197">Request query string parameter</span></span> |
|<span data-ttu-id="607a0-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="607a0-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="607a0-199">Geçerli istekten veri yönlendir</span><span class="sxs-lookup"><span data-stu-id="607a0-199">Route data from the current request</span></span> |
|<span data-ttu-id="607a0-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="607a0-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="607a0-201">Eylem parametresi olarak eklenen istek hizmeti</span><span class="sxs-lookup"><span data-stu-id="607a0-201">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="607a0-202">Değerler `%2f` (`/`) içerdiğinde `[FromRoute]` kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="607a0-202">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="607a0-203">`%2f` `/`atlanmaz.</span><span class="sxs-lookup"><span data-stu-id="607a0-203">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="607a0-204">Değer `%2f`içeriyorsa `[FromQuery]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="607a0-204">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="607a0-205">`[FromQuery]`gibi `[ApiController]` özniteliği veya bağlama kaynağı öznitelikleri olmadan ASP.NET Core çalışma zamanı karmaşık nesne modeli Ciltçi kullanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="607a0-205">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="607a0-206">Karmaşık nesne modeli Ciltçi, verileri değer sağlayıcılarından tanımlı bir düzende çeker.</span><span class="sxs-lookup"><span data-stu-id="607a0-206">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="607a0-207">Aşağıdaki örnekte `[FromQuery]` özniteliği, istek URL 'sinin sorgu dizesinde `discontinuedOnly` parametre değerinin sağlandığını belirtir:</span><span class="sxs-lookup"><span data-stu-id="607a0-207">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="607a0-208">`[ApiController]` özniteliği, eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygular.</span><span class="sxs-lookup"><span data-stu-id="607a0-208">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="607a0-209">Bu kurallar, eylem parametrelerine öznitelikleri uygulayarak bağlama kaynaklarını el ile tanımlamak zorunda kalmadan sizi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="607a0-209">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="607a0-210">Bağlama kaynak çıkarımı kuralları aşağıdaki gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="607a0-210">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="607a0-211">karmaşık tür parametreleri için `[FromBody]` algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-211">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="607a0-212">`[FromBody]` çıkarım kuralı için bir özel durum, <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>gibi özel bir anlamı olan karmaşık, yerleşik bir türdür.</span><span class="sxs-lookup"><span data-stu-id="607a0-212">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="607a0-213">Bağlama kaynak çıkarımı kodu bu özel türleri yoksayar.</span><span class="sxs-lookup"><span data-stu-id="607a0-213">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="607a0-214"><xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>türündeki eylem parametreleri için `[FromForm]` algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-214">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="607a0-215">Bu, herhangi bir basit veya Kullanıcı tanımlı tür için çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="607a0-215">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="607a0-216">yol şablonundaki bir parametreyle eşleşen herhangi bir eylem parametresi adı için `[FromRoute]` algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-216">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="607a0-217">Birden fazla yol bir eylem parametresiyle eşleştiğinde, herhangi bir rota değeri `[FromRoute]`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="607a0-217">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="607a0-218">`[FromQuery]` diğer eylem parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-218">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="607a0-219">FromBody çıkarım notları</span><span class="sxs-lookup"><span data-stu-id="607a0-219">FromBody inference notes</span></span>

<span data-ttu-id="607a0-220">`string` veya `int`gibi basit türler için `[FromBody]` çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="607a0-220">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="607a0-221">Bu nedenle, bu işlev gerektiğinde basit türler için `[FromBody]` özniteliği kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="607a0-221">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="607a0-222">Bir eylem, istek gövdesinden birden fazla parametre bağlamışsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="607a0-222">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="607a0-223">Örneğin, aşağıdaki eylem yöntemi imzalarının tümü bir özel duruma neden olur:</span><span class="sxs-lookup"><span data-stu-id="607a0-223">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="607a0-224">karmaşık türler olduklarından her ikisi de `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="607a0-224">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="607a0-225">karmaşık bir tür olduğundan, başka bir üzerinde `[FromBody]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="607a0-225">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="607a0-226">her ikisinde de `[FromBody]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="607a0-226">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="607a0-227">ASP.NET Core 2,1 ' de, listeler ve diziler gibi koleksiyon türü parametreleri `[FromQuery]`olarak yanlış şekilde algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-227">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="607a0-228">`[FromBody]` özniteliği, istek gövdesinden bağlanmaları durumunda bu parametreler için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="607a0-228">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="607a0-229">Bu davranış ASP.NET Core 2,2 veya sonraki bir sürümde düzeltilir. burada, koleksiyon türü parametrelerinin varsayılan olarak gövdeden bağlanacak şekilde çıkarsandır.</span><span class="sxs-lookup"><span data-stu-id="607a0-229">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="607a0-230">Çıkarım kurallarını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="607a0-230">Disable inference rules</span></span>

<span data-ttu-id="607a0-231">Bağlama kaynak çıkarımını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="607a0-231">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="607a0-232">Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="607a0-232">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="607a0-233">Multipart/form-veri isteği çıkarımı</span><span class="sxs-lookup"><span data-stu-id="607a0-233">Multipart/form-data request inference</span></span>

<span data-ttu-id="607a0-234">Bir eylem parametresine [[fromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliğiyle açıklama eklendiğinde `[ApiController]` özniteliği bir çıkarım kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="607a0-234">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="607a0-235">`multipart/form-data` isteği içerik türü algılanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-235">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="607a0-236">Varsayılan davranışı devre dışı bırakmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> özelliğini `Startup.ConfigureServices``true` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="607a0-236">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="607a0-237">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="607a0-237">Problem details for error status codes</span></span>

<span data-ttu-id="607a0-238">Uyumluluk sürümü 2,2 veya üzeri olduğunda, MVC bir hata sonucunu (durum kodu 400 veya üzeri olan bir sonuç) <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>bir sonuçla dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="607a0-238">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="607a0-239">`ProblemDetails` türü, bir HTTP yanıtında makine tarafından okunabilen hata ayrıntılarını sağlamak için [RFC 7807 belirtimine](https://tools.ietf.org/html/rfc7807) dayanır.</span><span class="sxs-lookup"><span data-stu-id="607a0-239">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="607a0-240">Bir denetleyici eyleminde aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="607a0-240">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="607a0-241">`NotFound` yöntemi `ProblemDetails` gövde içeren bir HTTP 404 durum kodu üretir.</span><span class="sxs-lookup"><span data-stu-id="607a0-241">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="607a0-242">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="607a0-242">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="607a0-243">ProblemDetails yanıtını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="607a0-243">Disable ProblemDetails response</span></span>

<span data-ttu-id="607a0-244"><xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> özelliği `true`olarak ayarlandığında `ProblemDetails` örneğinin otomatik olarak oluşturulması devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="607a0-244">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> property is set to `true`.</span></span> <span data-ttu-id="607a0-245">Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="607a0-245">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="607a0-246">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="607a0-246">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
