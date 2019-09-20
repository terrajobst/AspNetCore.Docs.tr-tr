---
title: ASP.NET Core Web API 'Lerinde hataları işleme
author: pranavkm
description: ASP.NET Core Web API 'Leri ile hata işleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/18/2019
uid: web-api/handle-errors
ms.openlocfilehash: bf8229d37ef15c42b5d7bb4a12737ac65d7b753c
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150910"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="c7059-103">ASP.NET Core Web API 'Lerinde hataları işleme</span><span class="sxs-lookup"><span data-stu-id="c7059-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="c7059-104">Bu makalede, ASP.NET Core Web API 'Leriyle hata işlemenin nasıl işleneceği ve özelleştirileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="c7059-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="c7059-105">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="c7059-105">Developer Exception Page</span></span>

<span data-ttu-id="c7059-106">[Geliştirici özel durum sayfası](xref:fundamentals/error-handling) , sunucu hataları için ayrıntılı yığın izlemeleri almak için kullanışlı bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="c7059-106">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7059-107">İstemci HTML biçimli çıktıyı kabul etmezse, geliştirici özel durum sayfasında düz metin yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c7059-107">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="c7059-108">Geliştirici özel durum sayfasını **yalnızca uygulama geliştirme ortamında çalışırken**etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="c7059-108">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="c7059-109">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık bir şekilde paylaşmak istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7059-109">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="c7059-110">Ortamları yapılandırma hakkında daha fazla bilgi için bkz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c7059-110">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="c7059-111">Özel durum işleyicisi</span><span class="sxs-lookup"><span data-stu-id="c7059-111">Exception handler</span></span>

<span data-ttu-id="c7059-112">Geliştirme dışı ortamlarda, [özel durum Işleme ara yazılımı](xref:fundamentals/error-handling) bir hata yükü oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7059-112">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="c7059-113">' `Startup.Configure`De, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ara yazılımı kullanmak için çağırın:</span><span class="sxs-lookup"><span data-stu-id="c7059-113">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

1. <span data-ttu-id="c7059-114">`/error` Yola yanıt vermek için bir denetleyici eylemi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="c7059-114">Configure a controller action to respond to the `/error` route:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

<span data-ttu-id="c7059-115">Önceki `Error` eylem istemciye [RFC7807](https://tools.ietf.org/html/rfc7807)uyumlu bir yük gönderir.</span><span class="sxs-lookup"><span data-stu-id="c7059-115">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="c7059-116">Özel durum Işleme ara yazılımı, yerel geliştirme ortamında daha ayrıntılı içerik üzerinde anlaşılan çıkış de sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c7059-116">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="c7059-117">Geliştirme ve üretim ortamları genelinde tutarlı bir yük biçimi oluşturmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="c7059-117">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="c7059-118">' `Startup.Configure`De, ortama özgü özel durum işleme ara yazılım örneklerini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="c7059-118">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="c7059-119">Yukarıdaki kodda, ara yazılım ile kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="c7059-119">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="c7059-120">Geliştirme ortamında bir `/error-local-development` yol.</span><span class="sxs-lookup"><span data-stu-id="c7059-120">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="c7059-121">Geliştirme olmayan ortamlarda `/error` bir yolu.</span><span class="sxs-lookup"><span data-stu-id="c7059-121">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="c7059-122">Denetleyici eylemlerine öznitelik yönlendirmeyi Uygula:</span><span class="sxs-lookup"><span data-stu-id="c7059-122">Apply attribute routing to controller actions:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error-local-development")]
        public IActionResult ErrorLocalDevelopment(
            [FromServices] IWebHostEnvironment webHostEnvironment)
        {
            if (!webHostEnvironment.IsDevelopment())
            {
                throw new InvalidOperationException(
                    "This shouldn't be invoked in non-development environments.");
            }
    
            var context = HttpContext.Features.Get<IExceptionHandlerFeature>();

            return Problem(
                detail: context.Error.StackTrace,
                title: context.Error.Message);
        }
    
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="c7059-123">Yanıtı değiştirmek için özel durumları kullanın</span><span class="sxs-lookup"><span data-stu-id="c7059-123">Use exceptions to modify the response</span></span>

<span data-ttu-id="c7059-124">Yanıtın içeriği, denetleyicinin dışından değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c7059-124">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="c7059-125">ASP.NET 4. x Web API 'sinde, bunu yapmanın bir yolu <xref:System.Web.Http.HttpResponseException> türünü kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c7059-125">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="c7059-126">ASP.NET Core eşdeğer bir tür içermez.</span><span class="sxs-lookup"><span data-stu-id="c7059-126">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="c7059-127">İçin `HttpResponseException` destek aşağıdaki adımlarla eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="c7059-127">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="c7059-128">Adında `HttpResponseException`iyi bilinen bir özel durum türü oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c7059-128">Create a well-known exception type named `HttpResponseException`:</span></span>

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. <span data-ttu-id="c7059-129">Adlı `HttpResponseExceptionFilter`bir eylem filtresi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c7059-129">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    ```csharp
    public class HttpResponseExceptionFilter : IActionFilter, IOrderedFilter
    {
        public int Order { get; set; } = int.MaxValue - 10;
    
        public void OnActionExecuting(ActionExecutingContext context) {}
    
        public void OnActionExecuted(ActionExecutedContext context)
        {
            if (context.Exception is HttpResponseException exception)
            {
                context.Result = new ObjectResult(exception.Value)
                {
                    Status = exception.Status,
                };
                context.ExceptionHandled = true;
            }
        }
    }
    ```

1. <span data-ttu-id="c7059-130">İçinde `Startup.ConfigureServices`, filtre koleksiyonuna eylem filtresini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c7059-130">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a><span data-ttu-id="c7059-131">Doğrulama hatası hata yanıtı</span><span class="sxs-lookup"><span data-stu-id="c7059-131">Validation failure error response</span></span>

<span data-ttu-id="c7059-132">Web API denetleyicileri için, bir model doğrulaması başarısız <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> olduğunda MVC bir yanıt türüyle yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="c7059-132">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="c7059-133">MVC, doğrulama hatasına yönelik <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> hata yanıtını oluşturmak için sonuçlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="c7059-133">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="c7059-134">Aşağıdaki örnek, varsayılan yanıt türünü <xref:Microsoft.AspNetCore.Mvc.SerializableError> içinde `Startup.ConfigureServices`olarak değiştirmek için fabrikası kullanır:</span><span class="sxs-lookup"><span data-stu-id="c7059-134">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

services.Configure<ApiBehaviorOptions>(options =>
{
    options.InvalidModelStateResponseFactory = context =>
    {
        var result = new BadRequestObjectResult(context.ModelState);

        // TODO: add `using using System.Net.Mime;` to resolve MediaTypeNames
        result.ContentTypes.Add(MediaTypeNames.Application.Json);
        result.ContentTypes.Add(MediaTypeNames.Application.Xml);

        return result;
    };
});
```

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="c7059-135">İstemci hata yanıtı</span><span class="sxs-lookup"><span data-stu-id="c7059-135">Client error response</span></span>

<span data-ttu-id="c7059-136">Bir *hata sonucu* , 400 veya ÜZERI bir http durum kodu ile sonuç olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c7059-136">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="c7059-137">Web API denetleyicileri için, MVC bir hata sonucunu ile <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>sonucuyla dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c7059-137">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7059-138">Hata yanıtı aşağıdaki yollarla yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7059-138">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="c7059-139">Problemayrıntılar Fabrikası Uygulama</span><span class="sxs-lookup"><span data-stu-id="c7059-139">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="c7059-140">ApiBehaviorOptions. ClientErrorMapping kullanın</span><span class="sxs-lookup"><span data-stu-id="c7059-140">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="c7059-141">Problemayrıntılar Fabrikası Uygulama</span><span class="sxs-lookup"><span data-stu-id="c7059-141">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="c7059-142">MVC, `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` ve <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> öğesinin<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>tüm örneklerini üretmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="c7059-142">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="c7059-143">Buna istemci hata yanıtları, doğrulama hatası hata yanıtları ve `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> yardımcı yöntemler dahildir.</span><span class="sxs-lookup"><span data-stu-id="c7059-143">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="c7059-144">Sorun ayrıntıları yanıtını özelleştirmek için, `ProblemDetailsFactory` içinde `Startup.ConfigureServices`özel bir uygulamasını kaydedin:</span><span class="sxs-lookup"><span data-stu-id="c7059-144">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c7059-145">Hata yanıtı, [Use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) bölümünde özetlenen şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c7059-145">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="c7059-146">ApiBehaviorOptions. ClientErrorMapping kullanın</span><span class="sxs-lookup"><span data-stu-id="c7059-146">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="c7059-147">Yanıtıniçeriğini`ProblemDetails` yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c7059-147">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="c7059-148">Örneğin, aşağıdaki kod 404 yanıtlarının `type` özelliğini güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="c7059-148">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
