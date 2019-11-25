---
title: ASP.NET Core 'de HttpContext 'e erişme
author: coderandhiker
description: ASP.NET Core ' de HttpContext 'e erişme hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 0bf40f9cd2554f5ba01ccc06001fa4f1940d51a5
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289045"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="77053-103">ASP.NET Core 'de HttpContext 'e erişme</span><span class="sxs-lookup"><span data-stu-id="77053-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="77053-104">ASP.NET Core uygulamalar, [ıhttpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ve varsayılan uygulaması olan [httpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor)aracılığıyla `HttpContext` erişir.</span><span class="sxs-lookup"><span data-stu-id="77053-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="77053-105">Yalnızca bir hizmet içindeki `HttpContext` erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77053-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="77053-106">Razor Pages HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="77053-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="77053-107">Razor Pages [Pagemodel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="77053-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="77053-108">Razor görünümünden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="77053-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="77053-109">Razor görünümleri görünümdeki bir [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) özelliği aracılığıyla `HttpContext` doğrudan kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="77053-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="77053-110">Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir Intranet uygulamasındaki geçerli kullanıcı adını alır:</span><span class="sxs-lookup"><span data-stu-id="77053-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="77053-111">Bir denetleyiciden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="77053-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="77053-112">Denetleyiciler [ControllerBase. HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="77053-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="77053-113">Ara yazılım içinden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="77053-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="77053-114">Özel ara yazılım bileşenleriyle çalışırken, `HttpContext` `Invoke` veya `InvokeAsync` yöntemine geçirilir ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="77053-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="77053-115">Özel bileşenlerden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="77053-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="77053-116">`HttpContext`erişmesi gereken diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir.</span><span class="sxs-lookup"><span data-stu-id="77053-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="77053-117">Bağımlılık ekleme kapsayıcısı, `IHttpContextAccessor` oluşturucularına bağımlılık olarak bildiren her sınıfa sağlar.</span><span class="sxs-lookup"><span data-stu-id="77053-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="77053-118">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="77053-118">In the following example:</span></span>

* <span data-ttu-id="77053-119">`UserRepository` `IHttpContextAccessor`bağımlılığını bildirir.</span><span class="sxs-lookup"><span data-stu-id="77053-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="77053-120">Bağımlılık ekleme, bağımlılık zincirini çözdüğünde ve bir `UserRepository`örneği oluşturduğunda bağımlılık sağlanır.</span><span class="sxs-lookup"><span data-stu-id="77053-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="77053-121">Arka plan iş parçacığından HttpContext erişimi</span><span class="sxs-lookup"><span data-stu-id="77053-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="77053-122">`HttpContext`, iş parçacığı açısından güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="77053-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="77053-123">`HttpContext` bir isteği işlemenin dışında okuma veya yazma özellikleri `NullReferenceException`sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="77053-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="77053-124">Bir isteği işlemenin dışında `HttpContext` kullanmak genellikle `NullReferenceException`sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="77053-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="77053-125">Uygulamanız tek tek `NullReferenceException`s oluşturursa, arka plan işlemesini başlatan veya bir istek tamamlandıktan sonra işlemeye devam eden kodun bölümlerini gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="77053-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="77053-126">`async void`olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="77053-126">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="77053-127">`HttpContext` verilerle arka plan çalışmasını güvenle gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="77053-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="77053-128">İstek işleme sırasında gerekli verileri kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="77053-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="77053-129">Kopyalanmış verileri bir arka plan görevine geçirin.</span><span class="sxs-lookup"><span data-stu-id="77053-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="77053-130">Güvenli olmayan koddan kaçınmak için `HttpContext`, arka plan işi yapan bir yönteme hiçbir şekilde iletmeyin, bunun yerine ihtiyacınız olan verileri geçirin.</span><span class="sxs-lookup"><span data-stu-id="77053-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
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
