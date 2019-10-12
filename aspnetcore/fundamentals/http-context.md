---
title: ASP.NET Core 'de HttpContext 'e erişme
author: coderandhiker
description: ASP.NET Core ' de HttpContext 'e erişme hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 888adf6d61e6968127385952e65f942e86b7eb63
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288972"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="fff23-103">ASP.NET Core 'de HttpContext 'e erişme</span><span class="sxs-lookup"><span data-stu-id="fff23-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="fff23-104">ASP.NET Core uygulamalar [ıhttpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ve varsayılan uygulama [httpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor)aracılığıyla `HttpContext` ' a erişir.</span><span class="sxs-lookup"><span data-stu-id="fff23-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="fff23-105">Yalnızca bir hizmet içindeki `HttpContext` ' e erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fff23-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="fff23-106">Razor Pages HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fff23-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="fff23-107">Razor Pages [Pagemodel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="fff23-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="fff23-108">Razor görünümünden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="fff23-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="fff23-109">Razor görünümleri, `HttpContext` ' yı doğrudan görünümdeki bir [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) özelliği aracılığıyla kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fff23-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="fff23-110">Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir Intranet uygulamasındaki geçerli kullanıcı adını alır:</span><span class="sxs-lookup"><span data-stu-id="fff23-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="fff23-111">Bir denetleyiciden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="fff23-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="fff23-112">Denetleyiciler [ControllerBase. HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="fff23-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="fff23-113">Ara yazılım içinden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fff23-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="fff23-114">Özel ara yazılım bileşenleriyle çalışırken, `HttpContext` `Invoke` veya `InvokeAsync` yöntemine geçirilir ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="fff23-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="fff23-115">Özel bileşenlerden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fff23-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="fff23-116">@No__t-0 ' a erişimi gerektiren diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir.</span><span class="sxs-lookup"><span data-stu-id="fff23-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fff23-117">Bağımlılık ekleme kapsayıcısı, @no__t oluşturucularını bir bağımlılık olarak bildiren tüm sınıflara sağlar.</span><span class="sxs-lookup"><span data-stu-id="fff23-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="fff23-118">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="fff23-118">In the following example:</span></span>

* <span data-ttu-id="fff23-119">`UserRepository` `IHttpContextAccessor` ' deki bağımlılığını bildirir.</span><span class="sxs-lookup"><span data-stu-id="fff23-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="fff23-120">Bağımlılık ekleme, bağımlılık zincirini çözümlediğinde ve bir @no__t örneği oluşturduğunda bağımlılık sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fff23-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="fff23-121">Arka plan iş parçacığından HttpContext erişimi</span><span class="sxs-lookup"><span data-stu-id="fff23-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="fff23-122">`HttpContext`, iş parçacığı açısından güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="fff23-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="fff23-123">@No__t bir isteği işlemenin dışında @no__t okuma veya yazma özellikleri, 1 ile sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fff23-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="fff23-124">Bir isteği işlemenin dışında `HttpContext` kullanmak genellikle `NullReferenceException` ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="fff23-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="fff23-125">Uygulamanız tek biçimli `NullReferenceException`s oluşturursa, arka plan işlemesini Başlatan kodun bölümlerini gözden geçirin veya istek tamamlandıktan sonra işlemeye devam edin.</span><span class="sxs-lookup"><span data-stu-id="fff23-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="fff23-126">@No__t-0 olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="fff23-126">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="fff23-127">@No__t-0 verileriyle arka plan çalışmasını güvenle gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="fff23-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="fff23-128">İstek işleme sırasında gerekli verileri kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="fff23-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="fff23-129">Kopyalanmış verileri bir arka plan görevine geçirin.</span><span class="sxs-lookup"><span data-stu-id="fff23-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="fff23-130">Güvenli olmayan koddan kaçınmak için, `HttpContext` ' ı hiçbir şekilde arka plan iş yapan bir yönteme iletmeyin, bunun yerine ihtiyacınız olan verileri geçirin.</span><span class="sxs-lookup"><span data-stu-id="fff23-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

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
