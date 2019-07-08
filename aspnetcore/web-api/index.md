---
title: ASP.NET Core ile Web API'leri oluşturma
author: scottaddie
description: ASP.NET Core web API'si oluşturma hakkındaki temel bilgileri öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/08/2019
uid: web-api/index
ms.openlocfilehash: 4f9c334f74dd2a8b7c31c7a42703fa361ccf9139
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622793"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="70da9-103">ASP.NET Core ile Web API'leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="70da9-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="70da9-104">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="70da9-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="70da9-105">ASP.NET Core web API'leri kullanarak, olarak da bilinir oluşturma RESTful hizmetleri destekleyen C#.</span><span class="sxs-lookup"><span data-stu-id="70da9-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="70da9-106">İsteklerini işlemek için bir web API denetleyicisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="70da9-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="70da9-107">*Denetleyicileri* bir web API'SİNDE türetilen sınıflardır `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="70da9-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="70da9-108">Bu makalede, API istekleri işlemek için denetleyicileri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="70da9-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="70da9-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="70da9-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="70da9-110">([Nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="70da9-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="70da9-111">ControllerBase sınıfı</span><span class="sxs-lookup"><span data-stu-id="70da9-111">ControllerBase class</span></span>

<span data-ttu-id="70da9-112">Bir web API öğesinden türetilen en az bir denetleyici sınıflarına sahip <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="70da9-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="70da9-113">Örneğin, web API proje şablonunu değerleri denetleyici oluşturur:</span><span class="sxs-lookup"><span data-stu-id="70da9-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="70da9-114">Bir web API denetleyicisi türeterek oluşturmayın <xref:Microsoft.AspNetCore.Mvc.Controller> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="70da9-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="70da9-115">`Controller` öğesinden türetilen `ControllerBase` ve web API istekleri işleme web sayfaları için bu nedenle görünümleri için destek ekler.</span><span class="sxs-lookup"><span data-stu-id="70da9-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="70da9-116">Bu kural için bir istisna vardır: görünümler ve API'leri için aynı denetleyici kullanmayı planlıyorsanız, bu sınıftan türetilen `Controller`.</span><span class="sxs-lookup"><span data-stu-id="70da9-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="70da9-117">`ControllerBase` Sınıfı, birçok özellik ve HTTP isteklerini işlemek için kullanışlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="70da9-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="70da9-118">Örneğin, `ControllerBase.CreatedAtAction` 201 durum kodunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="70da9-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="70da9-119">Yöntemlerden daha fazla bazı örnekleri aşağıda verilmiştir, `ControllerBase` sağlar.</span><span class="sxs-lookup"><span data-stu-id="70da9-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="70da9-120">Yöntem</span><span class="sxs-lookup"><span data-stu-id="70da9-120">Method</span></span>  |<span data-ttu-id="70da9-121">Notlar</span><span class="sxs-lookup"><span data-stu-id="70da9-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="70da9-122">400 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="70da9-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="70da9-123">404 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="70da9-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="70da9-124">Bir dosyayı döndürür.</span><span class="sxs-lookup"><span data-stu-id="70da9-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="70da9-125">Çağıran [model bağlama](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="70da9-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="70da9-126">Çağıran [model doğrulama](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="70da9-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="70da9-127">Tüm kullanılabilir yöntemlerin ve özelliklerin listesi için bkz. <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="70da9-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="70da9-128">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="70da9-128">Attributes</span></span>

<span data-ttu-id="70da9-129"><xref:Microsoft.AspNetCore.Mvc> Ad alanı, web API'si denetleyiciler ve eylem yöntemlerine davranışını yapılandırmak için kullanılan öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="70da9-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="70da9-130">Aşağıdaki örnek, HTTP yöntemini kabul edilir ve döndürülen durum kodları belirtmek için öznitelikleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="70da9-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="70da9-131">Kullanılabilir öznitelikler daha fazla bazı örnekleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70da9-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="70da9-132">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="70da9-132">Attribute</span></span>|<span data-ttu-id="70da9-133">Notlar</span><span class="sxs-lookup"><span data-stu-id="70da9-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="70da9-134">[[Yol]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="70da9-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="70da9-135">Bir denetleyici veya eylem için URL deseni belirtir.</span><span class="sxs-lookup"><span data-stu-id="70da9-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="70da9-136">[[Bağlama]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="70da9-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="70da9-137">Önek ve dahil edilecek model bağlama için özellik belirtir.</span><span class="sxs-lookup"><span data-stu-id="70da9-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="70da9-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="70da9-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="70da9-139">HTTP GET yöntemini destekleyen bir eylem belirtir.</span><span class="sxs-lookup"><span data-stu-id="70da9-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="70da9-140">[[Tüketir]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="70da9-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="70da9-141">Bir eylem kabul eden veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="70da9-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="70da9-142">[[Oluşturur]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="70da9-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="70da9-143">Bir eylem döndürdüğü veri türlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="70da9-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="70da9-144">Kullanılabilir öznitelikler içeren bir liste için bkz. <xref:Microsoft.AspNetCore.Mvc> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="70da9-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="70da9-145">ApiController özniteliği</span><span class="sxs-lookup"><span data-stu-id="70da9-145">ApiController attribute</span></span>

<span data-ttu-id="70da9-146">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) API özgü davranışları etkinleştirmek için bir denetleyici sınıfı özniteliği uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="70da9-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="70da9-147">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="70da9-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="70da9-148">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="70da9-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="70da9-149">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="70da9-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="70da9-150">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="70da9-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="70da9-151">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="70da9-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="70da9-152">Bu özellikleri gerektiren bir [uyumluluk sürümü](<xref:mvc/compatibility-version>) 2.1 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="70da9-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="70da9-153">Belirli denetleyicilerinde ApiController</span><span class="sxs-lookup"><span data-stu-id="70da9-153">ApiController on specific controllers</span></span>

<span data-ttu-id="70da9-154">`[ApiController]` Özniteliği, proje şablonundan aşağıdaki örnekteki gibi belirli denetleyicilerine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="70da9-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="70da9-155">Birden çok denetleyicisinde ApiController</span><span class="sxs-lookup"><span data-stu-id="70da9-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="70da9-156">Birden fazla denetleyicisinde özniteliğini kullanarak bir yaklaşım olan ile ek açıklamalı bir özel temel denetleyici sınıfını oluşturmak için `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="70da9-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="70da9-157">Özel bir temel sınıf ve ondan türetilen denetleyicisi gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="70da9-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="70da9-158">Bir derleme üzerinde ApiController</span><span class="sxs-lookup"><span data-stu-id="70da9-158">ApiController on an assembly</span></span>

<span data-ttu-id="70da9-159">Varsa [uyumluluk sürümü](<xref:mvc/compatibility-version>) 2.2 veya sonraki sürümleri ayarlanır `[ApiController]` özniteliği bir derlemeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="70da9-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="70da9-160">Bu şekilde ek açıklama derleme içindeki tüm denetleyicilere web API davranış uygulanır.</span><span class="sxs-lookup"><span data-stu-id="70da9-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="70da9-161">Tek tek denetleyicileri için geri çevirmek için hiçbir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="70da9-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="70da9-162">Derleme düzeyi özniteliği için geçerli `Startup` aşağıdaki örnekte gösterildiği gibi sınıf:</span><span class="sxs-lookup"><span data-stu-id="70da9-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="70da9-163">Öznitelik yönlendirme gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="70da9-163">Attribute routing requirement</span></span>

<span data-ttu-id="70da9-164">`ApiController` Özniteliği öznitelik gereksinim yönlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="70da9-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="70da9-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="70da9-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="70da9-166">Eylemleri aracılığıyla erişilemez [geleneksel yollar](xref:mvc/controllers/routing#conventional-routing) tarafından tanımlanan <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> veya <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="70da9-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="70da9-167">HTTP 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="70da9-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="70da9-168">`ApiController` Öznitelik yapar model doğrulama hataları otomatik olarak bir HTTP 400 tetikleyici yanıtı.</span><span class="sxs-lookup"><span data-stu-id="70da9-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="70da9-169">Sonuç olarak, aşağıdaki kod bir eylem yöntemi gerekli değildir:</span><span class="sxs-lookup"><span data-stu-id="70da9-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="70da9-170">Varsayılan BadRequest yanıt</span><span class="sxs-lookup"><span data-stu-id="70da9-170">Default BadRequest response</span></span> 

<span data-ttu-id="70da9-171">2\.2 veya üzeri uyumluluk sürümü ile varsayılan yanıt için HTTP 400 yanıtları türdür <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="70da9-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="70da9-172">`ValidationProblemDetails` Türü uyumlu ile [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="70da9-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="70da9-173">Varsayılan yanıt olarak değiştirmek için <xref:Microsoft.AspNetCore.Mvc.SerializableError>ayarlayın `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` özelliğini `true` içinde `Startup.ConfigureServices`aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="70da9-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="70da9-174">BadRequest yanıt özelleştirme</span><span class="sxs-lookup"><span data-stu-id="70da9-174">Customize BadRequest response</span></span>

<span data-ttu-id="70da9-175">Doğrulama hatasıyla sonuçlanan yanıt özelleştirmek için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="70da9-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="70da9-176">Sonra aşağıdaki vurgulanmış kodu ekleyin `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="70da9-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a><span data-ttu-id="70da9-177">Günlük 400 otomatik yanıtlar</span><span class="sxs-lookup"><span data-stu-id="70da9-177">Log automatic 400 responses</span></span>

<span data-ttu-id="70da9-178">Bkz: [nasıl model doğrulama hataları (#12157 aspnet/AspNetCore.Docs) 400 otomatik yanıtları oturum](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="70da9-178">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400"></a><span data-ttu-id="70da9-179">Otomatik 400 devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="70da9-179">Disable automatic 400</span></span>

<span data-ttu-id="70da9-180">Otomatik 400 davranışı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="70da9-180">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="70da9-181">Aşağıdaki vurgulanmış kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="70da9-181">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="70da9-182">Kaynak parametre çıkarımı bağlama</span><span class="sxs-lookup"><span data-stu-id="70da9-182">Binding source parameter inference</span></span>

<span data-ttu-id="70da9-183">Bir bağlama kaynak özniteliği bir eylem parametresinin değeri bulunduğu konumun tanımlar.</span><span class="sxs-lookup"><span data-stu-id="70da9-183">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="70da9-184">Aşağıdaki bağlama kaynak özniteliklerini mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="70da9-184">The following binding source attributes exist:</span></span>

|<span data-ttu-id="70da9-185">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="70da9-185">Attribute</span></span>|<span data-ttu-id="70da9-186">Bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="70da9-186">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="70da9-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="70da9-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="70da9-188">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="70da9-188">Request body</span></span> |
|<span data-ttu-id="70da9-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="70da9-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="70da9-190">İstek gövdesini form verilerini</span><span class="sxs-lookup"><span data-stu-id="70da9-190">Form data in the request body</span></span> |
|<span data-ttu-id="70da9-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="70da9-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="70da9-192">İstek üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="70da9-192">Request header</span></span> |
|<span data-ttu-id="70da9-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="70da9-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="70da9-194">İstek sorgu dizesi parametresi</span><span class="sxs-lookup"><span data-stu-id="70da9-194">Request query string parameter</span></span> |
|<span data-ttu-id="70da9-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="70da9-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="70da9-196">Geçerli istek için rota verilerini</span><span class="sxs-lookup"><span data-stu-id="70da9-196">Route data from the current request</span></span> |
|<span data-ttu-id="70da9-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="70da9-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="70da9-198">Bir eylem parametresi olarak eklenen isteği hizmeti</span><span class="sxs-lookup"><span data-stu-id="70da9-198">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="70da9-199">Kullanmayın `[FromRoute]` değerleri içerebilir zaman `%2f` (diğer bir deyişle `/`).</span><span class="sxs-lookup"><span data-stu-id="70da9-199">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="70da9-200">`%2f` unescaped olmaz `/`.</span><span class="sxs-lookup"><span data-stu-id="70da9-200">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="70da9-201">Kullanım `[FromQuery]` değer içerebilir, `%2f`.</span><span class="sxs-lookup"><span data-stu-id="70da9-201">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="70da9-202">Olmadan `[ApiController]` özniteliği veya bağlama kaynağı öznitelikleri ister `[FromQuery]`, ASP.NET Core çalışma zamanı, karmaşık nesne model bağlayıcısını kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="70da9-202">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="70da9-203">Karmaşık nesne model bağlayıcı, tanımlanmış bir sıralama değeri sağlayıcılardan veri çeker.</span><span class="sxs-lookup"><span data-stu-id="70da9-203">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="70da9-204">Aşağıdaki örnekte, `[FromQuery]` özniteliği gösterir `discontinuedOnly` parametre değeri, istek URL'SİNİN sorgu dizesinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="70da9-204">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="70da9-205">`[ApiController]` Özniteliği eylem parametrelerinin varsayılan veri kaynakları için çıkarım kuralları uygular.</span><span class="sxs-lookup"><span data-stu-id="70da9-205">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="70da9-206">Bu kurallar kaynakları bağlama eylem parametreleri öznitelikleri uygulayarak el ile tanımlamak zorunda kalmaktan kaydedin.</span><span class="sxs-lookup"><span data-stu-id="70da9-206">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="70da9-207">Bağlama kaynağı çıkarım kuralları gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="70da9-207">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="70da9-208">`[FromBody]` Karmaşık tür parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="70da9-208">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="70da9-209">Bir özel durum `[FromBody]` çıkarım kuralı olan herhangi bir karmaşık, yerleşik türü özel bir anlamı olan gibi <xref:Microsoft.AspNetCore.Http.IFormCollection> ve <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="70da9-209">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="70da9-210">Bağlama kaynağı çıkarımı kod türlerine özel yok sayar.</span><span class="sxs-lookup"><span data-stu-id="70da9-210">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="70da9-211">`[FromForm]` için eylem parametrelerini tür çıkarımı yapılan <xref:Microsoft.AspNetCore.Http.IFormFile> ve <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="70da9-211">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="70da9-212">Herhangi bir basit veya kullanıcı tanımlı türleri için çıkarsanan değil.</span><span class="sxs-lookup"><span data-stu-id="70da9-212">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="70da9-213">`[FromRoute]` Rota şablonu içindeki bir parametre eşleşen herhangi bir eylem parametresi adı için algılanır.</span><span class="sxs-lookup"><span data-stu-id="70da9-213">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="70da9-214">Birden fazla yol bir eylem parametresinin eşleştiğinde, herhangi bir yol değer olarak kabul edilir `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="70da9-214">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="70da9-215">`[FromQuery]` herhangi bir eylem parametreleri için algılanır.</span><span class="sxs-lookup"><span data-stu-id="70da9-215">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="70da9-216">FromBody çıkarımı notları</span><span class="sxs-lookup"><span data-stu-id="70da9-216">FromBody inference notes</span></span>

<span data-ttu-id="70da9-217">`[FromBody]` basit türleri için gibi çıkarılan değil `string` veya `int`.</span><span class="sxs-lookup"><span data-stu-id="70da9-217">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="70da9-218">Bu nedenle, `[FromBody]` özniteliği işlevselliği gerektiğinde basit türleri için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="70da9-218">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="70da9-219">Bir eylem gövdeden bağlı birden fazla parametre varsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="70da9-219">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="70da9-220">Örneğin, aşağıdaki eylemi yöntem imzaları tümünün bir özel duruma neden:</span><span class="sxs-lookup"><span data-stu-id="70da9-220">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="70da9-221">`[FromBody]` karmaşık türler için hem de sonuçlandı.</span><span class="sxs-lookup"><span data-stu-id="70da9-221">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="70da9-222">`[FromBody]` öznitelik, bir karmaşık bir tür olduğundan diğer sonuçlandı.</span><span class="sxs-lookup"><span data-stu-id="70da9-222">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="70da9-223">`[FromBody]` Her iki öznitelik.</span><span class="sxs-lookup"><span data-stu-id="70da9-223">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="70da9-224">ASP.NET Core 2.1 içinde koleksiyonu tür parametreleri listeler ve diziler gibi yanlış olarak algılanır `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="70da9-224">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="70da9-225">`[FromBody]` Özniteliği gövdeden bağlı olmaları durumunda bu parametreler için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="70da9-225">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="70da9-226">Bu davranış ASP.NET Core 2.2 veya üzeri, toplama türü parametrelerini gövdesinden varsayılan olarak bağlı olmasını olduğu algılanır düzeltilir.</span><span class="sxs-lookup"><span data-stu-id="70da9-226">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="70da9-227">Çıkarım kuralları devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="70da9-227">Disable inference rules</span></span>

<span data-ttu-id="70da9-228">Bağlama kaynağı çıkarımı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> için `true`.</span><span class="sxs-lookup"><span data-stu-id="70da9-228">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="70da9-229">Aşağıdaki kodu ekleyin `Startup.ConfigureServices` sonra `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="70da9-229">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="70da9-230">Çıkarım multipart/form-data iste</span><span class="sxs-lookup"><span data-stu-id="70da9-230">Multipart/form-data request inference</span></span>

<span data-ttu-id="70da9-231">`[ApiController]` Özniteliğini uygular çıkarım kuralı ile bir eylem parametresinin açıklandığında [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) özniteliği: `multipart/form-data` içerik türü çıkarılan isteyin.</span><span class="sxs-lookup"><span data-stu-id="70da9-231">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="70da9-232">Varsayılan davranışı devre dışı bırakmak için ayarlanmış <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> için `true` içinde `Startup.ConfigureServices`aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="70da9-232">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="70da9-233">Hata durum kodları için sorun ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="70da9-233">Problem details for error status codes</span></span>

<span data-ttu-id="70da9-234">2\.2 veya üzeri uyumluluk sürümü olduğunda, MVC için bir sonuç ile bir hata sonucu (durum kodu 400 veya üzeri bir sonuç) dönüştüren <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="70da9-234">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="70da9-235">`ProblemDetails` Türü temel [RFC 7807 belirtimi](https://tools.ietf.org/html/rfc7807) bir HTTP yanıtı makine tarafından okunabilir hata ayrıntılarında sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="70da9-235">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="70da9-236">Aşağıdaki kodda bir denetleyici eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="70da9-236">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="70da9-237">HTTP yanıtı `NotFound` bir 404 durum kodu ile sahip bir `ProblemDetails` gövdesi.</span><span class="sxs-lookup"><span data-stu-id="70da9-237">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="70da9-238">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="70da9-238">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="70da9-239">ProblemDetails yanıt özelleştirme</span><span class="sxs-lookup"><span data-stu-id="70da9-239">Customize ProblemDetails response</span></span>

<span data-ttu-id="70da9-240">Kullanım `ClientErrorMapping` içeriğini yapılandırmak için özellik `ProblemDetails` yanıt.</span><span class="sxs-lookup"><span data-stu-id="70da9-240">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="70da9-241">Örneğin, aşağıdaki güncelleştirmeleri kod `type` geçmesine karşın 404 yanıtlarını özelliği:</span><span class="sxs-lookup"><span data-stu-id="70da9-241">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="70da9-242">ProblemDetails yanıt devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="70da9-242">Disable ProblemDetails response</span></span>

<span data-ttu-id="70da9-243">Otomatik olarak oluşturulmasını `ProblemDetails` ne zaman devre dışı `SuppressMapClientErrors` özelliği `true`.</span><span class="sxs-lookup"><span data-stu-id="70da9-243">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="70da9-244">Aşağıdaki kodu ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="70da9-244">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="70da9-245">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70da9-245">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
