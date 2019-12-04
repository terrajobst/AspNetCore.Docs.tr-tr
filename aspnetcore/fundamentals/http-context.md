---
title: ASP.NET Core 'de HttpContext 'e erişme
author: coderandhiker
description: ASP.NET Core ' de HttpContext 'e erişme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: fundamentals/httpcontext
ms.openlocfilehash: 8a7ee180380c42ea745c91b8e6a18c1baa820220
ms.sourcegitcommit: 5974e3e66dab3398ecf2324fbb82a9c5636f70de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74778745"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="b6227-103">ASP.NET Core 'de HttpContext 'e erişme</span><span class="sxs-lookup"><span data-stu-id="b6227-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="b6227-104"><xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>ASP.NET Core uygulamalara erişim `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="b6227-104">ASP.NET Core apps access `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span> <span data-ttu-id="b6227-105">Yalnızca bir hizmet içindeki `HttpContext` erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b6227-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="b6227-106">Razor Pages HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="b6227-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="b6227-107">Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="b6227-107">The Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> exposes the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="b6227-108">Razor görünümünden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="b6227-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="b6227-109">Razor görünümleri görünümdeki bir [RazorPage. Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) özelliği aracılığıyla `HttpContext` doğrudan kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="b6227-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) property on the view.</span></span> <span data-ttu-id="b6227-110">Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir intranet uygulamasındaki geçerli kullanıcı adını alır:</span><span class="sxs-lookup"><span data-stu-id="b6227-110">The following example retrieves the current username in an intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="b6227-111">Bir denetleyiciden HttpContext kullanma</span><span class="sxs-lookup"><span data-stu-id="b6227-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="b6227-112">Denetleyiciler [ControllerBase. HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) özelliğini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="b6227-112">Controllers expose the [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;

        ...

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="b6227-113">Ara yazılım içinden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="b6227-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="b6227-114">Özel ara yazılım bileşenleriyle çalışırken, `HttpContext` `Invoke` veya `InvokeAsync` yöntemine geçirilir ve ara yazılım yapılandırıldığında erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="b6227-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="b6227-115">Özel bileşenlerden HttpContext kullanın</span><span class="sxs-lookup"><span data-stu-id="b6227-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="b6227-116">`HttpContext`erişmesi gereken diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir.</span><span class="sxs-lookup"><span data-stu-id="b6227-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b6227-117">Bağımlılık ekleme kapsayıcısı, `IHttpContextAccessor` oluşturuculara bir bağımlılık olarak bildiren tüm sınıflara sağlar:</span><span class="sxs-lookup"><span data-stu-id="b6227-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddControllersWithViews();
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="b6227-118">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="b6227-118">In the following example:</span></span>

* <span data-ttu-id="b6227-119">`UserRepository` `IHttpContextAccessor`bağımlılığını bildirir.</span><span class="sxs-lookup"><span data-stu-id="b6227-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="b6227-120">Bağımlılık ekleme, bağımlılık zincirini çözdüğünde ve bir `UserRepository`örneği oluşturduğunda bağımlılık sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b6227-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="b6227-121">Arka plan iş parçacığından HttpContext erişimi</span><span class="sxs-lookup"><span data-stu-id="b6227-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="b6227-122">`HttpContext` iş parçacığı açısından güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="b6227-122">`HttpContext` isn't thread-safe.</span></span> <span data-ttu-id="b6227-123">`HttpContext` bir isteği işlemenin dışında okuma veya yazma özellikleri <xref:System.NullReferenceException>sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b6227-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a <xref:System.NullReferenceException>.</span></span>

> [!NOTE]
> <span data-ttu-id="b6227-124">Uygulamanız tek tek `NullReferenceException` hatalar oluşturursa, arka plan işlemeyi başlatan veya bir istek tamamlandıktan sonra işlemeye devam eden kodun bölümlerini gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="b6227-124">If your app generates sporadic `NullReferenceException` errors, review parts of the code that start background processing or that continue processing after a request completes.</span></span> <span data-ttu-id="b6227-125">`async void`olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="b6227-125">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="b6227-126">`HttpContext` verilerle arka plan çalışmasını güvenle gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="b6227-126">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="b6227-127">İstek işleme sırasında gerekli verileri kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="b6227-127">Copy the required data during request processing.</span></span>
* <span data-ttu-id="b6227-128">Kopyalanmış verileri bir arka plan görevine geçirin.</span><span class="sxs-lookup"><span data-stu-id="b6227-128">Pass the copied data to a background task.</span></span>

<span data-ttu-id="b6227-129">Güvenli olmayan koddan kaçınmak için `HttpContext`, arka plan çalışması gerçekleştiren bir yönteme hiçbir şekilde iletmeyin.</span><span class="sxs-lookup"><span data-stu-id="b6227-129">To avoid unsafe code, never pass the `HttpContext` into a method that performs background work.</span></span> <span data-ttu-id="b6227-130">Bunun yerine gerekli verileri geçirin.</span><span class="sxs-lookup"><span data-stu-id="b6227-130">Pass the required data instead.</span></span> <span data-ttu-id="b6227-131">Aşağıdaki örnekte, bir e-posta göndermeye başlamak için `SendEmailCore` çağırılır.</span><span class="sxs-lookup"><span data-stu-id="b6227-131">In the following example, `SendEmailCore` is called to start sending an email.</span></span> <span data-ttu-id="b6227-132">`correlationId`, `HttpContext`değil `SendEmailCore`geçirilir.</span><span class="sxs-lookup"><span data-stu-id="b6227-132">The `correlationId` is passed to `SendEmailCore`, not the `HttpContext`.</span></span> <span data-ttu-id="b6227-133">Kod yürütme `SendEmailCore` tamamlanmasını beklemez:</span><span class="sxs-lookup"><span data-stu-id="b6227-133">Code execution doesn't wait for `SendEmailCore` to complete:</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        _ = SendEmailCore(correlationId);

        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        ...
    }
}
