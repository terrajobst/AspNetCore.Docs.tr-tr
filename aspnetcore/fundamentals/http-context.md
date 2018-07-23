---
title: ASP.NET core'da erişim HttpContext
author: coderandhiker
description: ASP.NET core'da HttpContext erişmeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202722"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="d0725-103">ASP.NET core'da erişim HttpContext</span><span class="sxs-lookup"><span data-stu-id="d0725-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="d0725-104">ASP.NET Core uygulamaları erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="d0725-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="d0725-105">Razor sayfaları'ndan HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="d0725-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="d0725-106">Razor sayfaları [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sunan [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="d0725-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="d0725-107">Bir denetleyiciden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="d0725-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="d0725-108">Denetleyicileri sunmaya [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliği:</span><span class="sxs-lookup"><span data-stu-id="d0725-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="d0725-109">Ara yazılımı HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="d0725-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="d0725-110">Özel bir ara yazılım bileşenleri ile çalışırken `HttpContext` yöntemlere geçirilen `Invoke` veya `InvokeAsync` yöntemi ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="d0725-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="d0725-111">HttpContext özel bileşenlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="d0725-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="d0725-112">Diğer framework ve erişim gerektiren özel bileşenler için `HttpContext`, yerleşik kullanarak System.Data.sqlclient'ı bağımlılık kaydetmek için önerilen yaklaşımdır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="d0725-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d0725-113">Bağımlılık ekleme kapsayıcısını kaynakları `IHttpContextAccessor` bağımlılık olarak oluşturucuları bildirme herhangi sınıflar.</span><span class="sxs-lookup"><span data-stu-id="d0725-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="d0725-114">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d0725-114">In the preceding example:</span></span>

* <span data-ttu-id="d0725-115">`UserRepository` üzerinde bağımlılık bildirir `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="d0725-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="d0725-116">Bağımlılık ekleme bağımlılık zincirinden çözümler ve bir örneğini oluşturur, bağımlılık sağlanan `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="d0725-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
