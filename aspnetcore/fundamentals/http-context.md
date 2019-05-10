---
title: ASP.NET core'da erişim HttpContext
author: coderandhiker
description: ASP.NET core'da HttpContext erişmeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 373c036e0839ce51259e23f8503fbe4691b48751
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900455"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="6533c-103">ASP.NET core'da erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="6533c-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="6533c-104">ASP.NET Core uygulamaları erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="6533c-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="6533c-105">Yalnızca kullanmak gerekli olan `IHttpContextAccessor` erişim gerektiğinde `HttpContext` bir hizmeti içinde.</span><span class="sxs-lookup"><span data-stu-id="6533c-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="6533c-106">Razor sayfaları'ndan HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="6533c-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="6533c-107">Razor sayfaları [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sunan [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="6533c-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="6533c-108">HttpContext Razor görünümünü kullanın</span><span class="sxs-lookup"><span data-stu-id="6533c-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="6533c-109">Razor görünümleri ile açığa `HttpContext` aracılığıyla doğrudan bir [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="6533c-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="6533c-110">Aşağıdaki örnek, Windows kimlik doğrulaması kullanarak bir Intranet uygulaması geçerli kullanıcı adını alır:</span><span class="sxs-lookup"><span data-stu-id="6533c-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="6533c-111">Bir denetleyiciden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="6533c-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="6533c-112">Denetleyicileri sunmaya [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="6533c-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="6533c-113">Ara yazılımı HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="6533c-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="6533c-114">Özel bir ara yazılım bileşenleri ile çalışırken `HttpContext` yöntemlere geçirilen `Invoke` veya `InvokeAsync` yöntemi ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="6533c-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="6533c-115">HttpContext özel bileşenlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6533c-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="6533c-116">Diğer framework ve erişim gerektiren özel bileşenler için `HttpContext`, yerleşik kullanarak System.Data.sqlclient'ı bağımlılık kaydetmek için önerilen yaklaşımdır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="6533c-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6533c-117">Bağımlılık ekleme kapsayıcısını kaynakları `IHttpContextAccessor` bağımlılık olarak oluşturucuları bildirme herhangi sınıflar.</span><span class="sxs-lookup"><span data-stu-id="6533c-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="6533c-118">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="6533c-118">In the following example:</span></span>

* <span data-ttu-id="6533c-119">`UserRepository` üzerinde bağımlılık bildirir `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="6533c-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="6533c-120">Bağımlılık ekleme bağımlılık zincirinden çözümler ve bir örneğini oluşturur, bağımlılık sağlanan `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="6533c-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="6533c-121">Arka plan iş parçacığından HttpContext erişim</span><span class="sxs-lookup"><span data-stu-id="6533c-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="6533c-122">`HttpContext` iş parçacığı açısından güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="6533c-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="6533c-123">Okuma veya yazma özelliklerinin `HttpContext` bir isteği işlerken dışında neden olabilir bir `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="6533c-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="6533c-124">Kullanarak `HttpContext` genellikle bir isteğin işlenmesinin dışında sonuçlanır bir `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="6533c-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="6533c-125">Uygulamanız kullanımın oluşturuyorsa `NullReferenceException`s, kodun arka plan işlemesi başlatın veya bir istek tamamlandıktan sonra işleme devam gözden geçirme bölümleri.</span><span class="sxs-lookup"><span data-stu-id="6533c-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="6533c-126">Bir denetleyici yöntemi olarak tanımlama gibi bir hataları arayın `async void`.</span><span class="sxs-lookup"><span data-stu-id="6533c-126">Look for a mistakes like defining a controller method as `async void`.</span></span>

<span data-ttu-id="6533c-127">Arka plan işleri ile güvenli bir şekilde gerçekleştirmek için `HttpContext` veri:</span><span class="sxs-lookup"><span data-stu-id="6533c-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="6533c-128">İstek işlenirken gerekli verileri kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="6533c-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="6533c-129">Kopyalanan veriler için bir arka plan görevi geçirin.</span><span class="sxs-lookup"><span data-stu-id="6533c-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="6533c-130">Güvenli olmayan kod önlemek için hiçbir zaman geçmesi `HttpContext` çalışma - arka planda bir yönteme ihtiyacınız bunun yerine verileri geçirin.</span><span class="sxs-lookup"><span data-stu-id="6533c-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
