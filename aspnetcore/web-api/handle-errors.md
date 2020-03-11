---
title: ASP.NET Core Web API 'Lerinde hataları işleme
author: pranavkm
description: ASP.NET Core Web API 'Leri ile hata işleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/10/2019
uid: web-api/handle-errors
ms.openlocfilehash: e445fb3d50973643c9cea60395d1ed02c2f5f675
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660890"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="84d20-103">ASP.NET Core Web API 'Lerinde hataları işleme</span><span class="sxs-lookup"><span data-stu-id="84d20-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="84d20-104">Bu makalede, ASP.NET Core Web API 'Leriyle hata işlemenin nasıl işleneceği ve özelleştirileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="84d20-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="84d20-105">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84d20-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="84d20-106">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="84d20-106">Developer Exception Page</span></span>

<span data-ttu-id="84d20-107">[Geliştirici özel durum sayfası](xref:fundamentals/error-handling) , sunucu hataları için ayrıntılı yığın izlemeleri almak için kullanışlı bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="84d20-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span> <span data-ttu-id="84d20-108">HTTP ardışık düzeninde zaman uyumlu ve zaman uyumsuz özel durumları yakalamak ve hata yanıtları oluşturmak için <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> kullanır.</span><span class="sxs-lookup"><span data-stu-id="84d20-108">It uses <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> to capture synchronous and asynchronous exceptions from the HTTP pipeline and to generate error responses.</span></span> <span data-ttu-id="84d20-109">Göstermek için aşağıdaki denetleyici eylemini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="84d20-109">To illustrate, consider the following controller action:</span></span>

[!code-csharp[](handle-errors/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_GetByCity)]

<span data-ttu-id="84d20-110">Önceki eylemi test etmek için aşağıdaki `curl` komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="84d20-110">Run the following `curl` command to test the preceding action:</span></span>

```bash
curl -i https://localhost:5001/weatherforecast/chicago
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84d20-111">ASP.NET Core 3,0 ve üzeri sürümlerde geliştirici özel durum sayfasında, istemci HTML biçimli çıkış isteğinde yoksa bir düz metin yanıtı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84d20-111">In ASP.NET Core 3.0 and later, the Developer Exception Page displays a plain-text response if the client doesn't request HTML-formatted output.</span></span> <span data-ttu-id="84d20-112">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="84d20-112">The following output appears:</span></span>

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/plain
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:13:16 GMT

System.ArgumentException: We don't offer a weather forecast for chicago. (Parameter 'city')
   at WebApiSample.Controllers.WeatherForecastController.Get(String city) in C:\working_folder\aspnet\AspNetCore.Docs\aspnetcore\web-api\handle-errors\samples\3.x\Controllers\WeatherForecastController.cs:line 34
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.SyncObjectResultExecutor.Execute(IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] arguments)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Logged|12_1(ControllerActionInvoker invoker)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.InvokeInnerFilterAsync()
--- End of stack trace from previous location where exception was thrown ---
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeFilterPipelineAsync>g__Awaited|19_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Logged|17_1(ResourceInvoker invoker)
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>g__AwaitRequestTask|6_0(Endpoint endpoint, Task requestTask, ILogger logger)
   at Microsoft.AspNetCore.Authorization.AuthorizationMiddleware.Invoke(HttpContext context)
   at Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware.Invoke(HttpContext context)

HEADERS
=======
Accept: */*
Host: localhost:44312
User-Agent: curl/7.55.1
```

<span data-ttu-id="84d20-113">Bunun yerine HTML biçimli bir yanıt göstermek için `Accept` HTTP istek üst bilgisini `text/html` medya türüne ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="84d20-113">To display an HTML-formatted response instead, set the `Accept` HTTP request header to the `text/html` media type.</span></span> <span data-ttu-id="84d20-114">Örnek:</span><span class="sxs-lookup"><span data-stu-id="84d20-114">For example:</span></span>

```bash
curl -i -H "Accept: text/html" https://localhost:5001/weatherforecast/chicago
```

<span data-ttu-id="84d20-115">HTTP yanıtından aşağıdaki alıntıyı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="84d20-115">Consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="84d20-116">ASP.NET Core 2,2 ve önceki sürümlerde, geliştirici özel durum sayfasında HTML biçimli bir yanıt görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84d20-116">In ASP.NET Core 2.2 and earlier, the Developer Exception Page displays an HTML-formatted response.</span></span> <span data-ttu-id="84d20-117">Örneğin, HTTP yanıtından aşağıdaki alıntıyı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="84d20-117">For example, consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/html; charset=utf-8
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:55:37 GMT

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="utf-8" />
        <title>Internal Server Error</title>
        <style>
            body {
    font-family: 'Segoe UI', Tahoma, Arial, Helvetica, sans-serif;
    font-size: .813em;
    color: #222;
    background-color: #fff;
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84d20-118">HTML biçimli yanıt Postman gibi araçlar aracılığıyla test edilirken yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="84d20-118">The HTML-formatted response becomes useful when testing via tools like Postman.</span></span> <span data-ttu-id="84d20-119">Aşağıdaki ekran yakalama, Postman 'daki düz metin ve HTML biçimli yanıtları gösterir:</span><span class="sxs-lookup"><span data-stu-id="84d20-119">The following screen capture shows both the plain-text and the HTML-formatted responses in Postman:</span></span>

![Geliştirici özel durum sayfası Postman 'da test ediliyor](handle-errors/_static/developer-exception-page-postman.gif)

::: moniker-end

> [!WARNING]
> <span data-ttu-id="84d20-121">Geliştirici özel durum sayfasını **yalnızca uygulama geliştirme ortamında çalışırken**etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="84d20-121">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="84d20-122">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık bir şekilde paylaşmak istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="84d20-122">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="84d20-123">Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="84d20-123">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="84d20-124">Özel durum işleyicisi</span><span class="sxs-lookup"><span data-stu-id="84d20-124">Exception handler</span></span>

<span data-ttu-id="84d20-125">Geliştirme dışı ortamlarda, [özel durum Işleme ara yazılımı](xref:fundamentals/error-handling) bir hata yükü oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="84d20-125">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="84d20-126">`Startup.Configure`' de, ara yazılımı kullanmak için <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A> çağırın:</span><span class="sxs-lookup"><span data-stu-id="84d20-126">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="84d20-127">`/error` yoluna yanıt vermek için bir denetleyici eylemi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="84d20-127">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="84d20-128">Yukarıdaki `Error` eylemi istemciye [RFC 7807](https://tools.ietf.org/html/rfc7807)ile uyumlu bir yük gönderir.</span><span class="sxs-lookup"><span data-stu-id="84d20-128">The preceding `Error` action sends an [RFC 7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="84d20-129">Özel durum Işleme ara yazılımı, yerel geliştirme ortamında daha ayrıntılı içerik üzerinde anlaşılan çıkış de sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="84d20-129">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="84d20-130">Geliştirme ve üretim ortamları genelinde tutarlı bir yük biçimi oluşturmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="84d20-130">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="84d20-131">`Startup.Configure`, ortama özgü özel durum Işleme ara yazılım örneklerini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="84d20-131">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    <span data-ttu-id="84d20-132">Yukarıdaki kodda, ara yazılım ile kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="84d20-132">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="84d20-133">Geliştirme ortamında `/error-local-development` yolu.</span><span class="sxs-lookup"><span data-stu-id="84d20-133">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="84d20-134">Geliştirmeyen ortamlarda `/error` yolu.</span><span class="sxs-lookup"><span data-stu-id="84d20-134">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="84d20-135">Denetleyici eylemlerine öznitelik yönlendirmeyi Uygula:</span><span class="sxs-lookup"><span data-stu-id="84d20-135">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="84d20-136">Yanıtı değiştirmek için özel durumları kullanın</span><span class="sxs-lookup"><span data-stu-id="84d20-136">Use exceptions to modify the response</span></span>

<span data-ttu-id="84d20-137">Yanıtın içeriği, denetleyicinin dışından değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="84d20-137">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="84d20-138">ASP.NET 4. x Web API 'sinde, bunu yapmanın bir yolu <xref:System.Web.Http.HttpResponseException> türünü kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="84d20-138">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="84d20-139">ASP.NET Core eşdeğer bir tür içermez.</span><span class="sxs-lookup"><span data-stu-id="84d20-139">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="84d20-140">`HttpResponseException` için destek aşağıdaki adımlarla eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="84d20-140">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="84d20-141">`HttpResponseException`adlı iyi bilinen bir özel durum türü oluşturun:</span><span class="sxs-lookup"><span data-stu-id="84d20-141">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="84d20-142">`HttpResponseExceptionFilter`adlı bir eylem filtresi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="84d20-142">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="84d20-143">`Startup.ConfigureServices`, filtre koleksiyonuna eylem filtresini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="84d20-143">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="84d20-144">Doğrulama hatası hata yanıtı</span><span class="sxs-lookup"><span data-stu-id="84d20-144">Validation failure error response</span></span>

<span data-ttu-id="84d20-145">Web API denetleyicileri için,, model doğrulama başarısız olduğunda MVC bir <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> yanıt türüyle yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="84d20-145">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="84d20-146">MVC, bir doğrulama hatasına yönelik hata yanıtını oluşturmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> sonuçlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="84d20-146">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="84d20-147">Aşağıdaki örnek, varsayılan yanıt türünü `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.SerializableError> olarak değiştirmek için fabrikası kullanır:</span><span class="sxs-lookup"><span data-stu-id="84d20-147">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="84d20-148">İstemci hata yanıtı</span><span class="sxs-lookup"><span data-stu-id="84d20-148">Client error response</span></span>

<span data-ttu-id="84d20-149">Bir *hata sonucu* , 400 veya ÜZERI bir http durum kodu ile sonuç olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="84d20-149">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="84d20-150">Web API denetleyicileri için, MVC bir hata sonucunu <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>bir sonuçla dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="84d20-150">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range="= aspnetcore-2.1"

> [!IMPORTANT]
> <span data-ttu-id="84d20-151">ASP.NET Core 2,1, neredeyse RFC 7807 uyumlu olan bir sorun ayrıntıları yanıtı üretir.</span><span class="sxs-lookup"><span data-stu-id="84d20-151">ASP.NET Core 2.1 generates a problem details response that's nearly RFC 7807-compliant.</span></span> <span data-ttu-id="84d20-152">Yüzde 100 uyumluluğu önemliyse, projeyi ASP.NET Core 2,2 veya üzeri bir sürüme yükseltin.</span><span class="sxs-lookup"><span data-stu-id="84d20-152">If 100 percent compliance is important, upgrade the project to ASP.NET Core 2.2 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84d20-153">Hata yanıtı aşağıdaki yollarla yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="84d20-153">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="84d20-154">Problemayrıntılar Fabrikası Uygulama</span><span class="sxs-lookup"><span data-stu-id="84d20-154">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="84d20-155">ApiBehaviorOptions. ClientErrorMapping kullanın</span><span class="sxs-lookup"><span data-stu-id="84d20-155">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="84d20-156">Problemayrıntılar Fabrikası Uygulama</span><span class="sxs-lookup"><span data-stu-id="84d20-156">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="84d20-157">MVC, tüm <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> ve <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>örneklerini üretmek için `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` kullanır.</span><span class="sxs-lookup"><span data-stu-id="84d20-157">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="84d20-158">Buna istemci hata yanıtları, doğrulama hatası hata yanıtları ve `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> yardımcı yöntemler dahildir.</span><span class="sxs-lookup"><span data-stu-id="84d20-158">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="84d20-159">Sorun ayrıntıları yanıtını özelleştirmek için `Startup.ConfigureServices``ProblemDetailsFactory` özel bir uygulamasını kaydedin:</span><span class="sxs-lookup"><span data-stu-id="84d20-159">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="84d20-160">Hata yanıtı, [Use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) bölümünde özetlenen şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="84d20-160">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="84d20-161">ApiBehaviorOptions. ClientErrorMapping kullanın</span><span class="sxs-lookup"><span data-stu-id="84d20-161">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="84d20-162">`ProblemDetails` yanıtının içeriğini yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping%2A> özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="84d20-162">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping%2A> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="84d20-163">Örneğin, `Startup.ConfigureServices` aşağıdaki kod 404 yanıtları için `type` özelliğini güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="84d20-163">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
