---
title: ASP.NET core'da erişim HttpContext
author: coderandhiker
description: ASP.NET core'da HttpContext erişmeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: ee185cd30af51fa6ee9a4d23ea60a56ec1b76c8d
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332294"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="fd940-103">ASP.NET core'da erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="fd940-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="fd940-104">ASP.NET Core uygulamaları erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="fd940-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="fd940-105">Yalnızca kullanmak gerekli olan `IHttpContextAccessor` erişim gerektiğinde `HttpContext` bir hizmeti içinde.</span><span class="sxs-lookup"><span data-stu-id="fd940-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="fd940-106">Razor sayfaları'ndan HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fd940-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="fd940-107">Razor sayfaları [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sunan [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="fd940-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="fd940-108">HttpContext Razor görünümünü kullanın</span><span class="sxs-lookup"><span data-stu-id="fd940-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="fd940-109">Razor görünümleri ile açığa `HttpContext` aracılığıyla doğrudan bir [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="fd940-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="fd940-110">Aşağıdaki örnek, Windows kimlik doğrulaması kullanarak bir Intranet uygulaması geçerli kullanıcı adını alır:</span><span class="sxs-lookup"><span data-stu-id="fd940-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="fd940-111">Bir denetleyiciden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fd940-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="fd940-112">Denetleyicileri sunmaya [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="fd940-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="fd940-113">Ara yazılımı HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="fd940-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="fd940-114">Özel bir ara yazılım bileşenleri ile çalışırken `HttpContext` yöntemlere geçirilen `Invoke` veya `InvokeAsync` yöntemi ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="fd940-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="fd940-115">HttpContext özel bileşenlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="fd940-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="fd940-116">Diğer framework ve erişim gerektiren özel bileşenler için `HttpContext`, yerleşik kullanarak System.Data.sqlclient'ı bağımlılık kaydetmek için önerilen yaklaşımdır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="fd940-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fd940-117">Bağımlılık ekleme kapsayıcısını kaynakları `IHttpContextAccessor` bağımlılık olarak oluşturucuları bildirme herhangi sınıflar.</span><span class="sxs-lookup"><span data-stu-id="fd940-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="fd940-118">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="fd940-118">In the preceding example:</span></span>

* <span data-ttu-id="fd940-119">`UserRepository` üzerinde bağımlılık bildirir `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="fd940-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="fd940-120">Bağımlılık ekleme bağımlılık zincirinden çözümler ve bir örneğini oluşturur, bağımlılık sağlanan `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="fd940-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
