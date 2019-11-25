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
# <a name="access-httpcontext-in-aspnet-core"></a>ASP.NET Core 'de HttpContext 'e erişme

ASP.NET Core uygulamalar, [ıhttpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ve varsayılan uygulaması olan [httpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor)aracılığıyla `HttpContext` erişir. Yalnızca bir hizmet içindeki `HttpContext` erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Razor Pages HttpContext kullanın

Razor Pages [Pagemodel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliğini kullanıma sunar:

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

## <a name="use-httpcontext-from-a-razor-view"></a>Razor görünümünden HttpContext kullanma

Razor görünümleri görünümdeki bir [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) özelliği aracılığıyla `HttpContext` doğrudan kullanıma sunar. Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir Intranet uygulamasındaki geçerli kullanıcı adını alır:

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>Bir denetleyiciden HttpContext kullanma

Denetleyiciler [ControllerBase. HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliğini kullanıma sunar:

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

## <a name="use-httpcontext-from-middleware"></a>Ara yazılım içinden HttpContext kullanın

Özel ara yazılım bileşenleriyle çalışırken, `HttpContext` `Invoke` veya `InvokeAsync` yöntemine geçirilir ve ara yazılım yapılandırıldığında erişilebilir:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Özel bileşenlerden HttpContext kullanın

`HttpContext`erişmesi gereken diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir. Bağımlılık ekleme kapsayıcısı, `IHttpContextAccessor` oluşturucularına bağımlılık olarak bildiren her sınıfa sağlar.

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

Aşağıdaki örnekte:

* `UserRepository` `IHttpContextAccessor`bağımlılığını bildirir.
* Bağımlılık ekleme, bağımlılık zincirini çözdüğünde ve bir `UserRepository`örneği oluşturduğunda bağımlılık sağlanır.

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

## <a name="httpcontext-access-from-a-background-thread"></a>Arka plan iş parçacığından HttpContext erişimi

`HttpContext`, iş parçacığı açısından güvenli değildir. `HttpContext` bir isteği işlemenin dışında okuma veya yazma özellikleri `NullReferenceException`sonuçlanabilir.

> [!NOTE]
> Bir isteği işlemenin dışında `HttpContext` kullanmak genellikle `NullReferenceException`sonuçlanır. Uygulamanız tek tek `NullReferenceException`s oluşturursa, arka plan işlemesini başlatan veya bir istek tamamlandıktan sonra işlemeye devam eden kodun bölümlerini gözden geçirin. `async void`olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.

`HttpContext` verilerle arka plan çalışmasını güvenle gerçekleştirmek için:

* İstek işleme sırasında gerekli verileri kopyalayın.
* Kopyalanmış verileri bir arka plan görevine geçirin.

Güvenli olmayan koddan kaçınmak için `HttpContext`, arka plan işi yapan bir yönteme hiçbir şekilde iletmeyin, bunun yerine ihtiyacınız olan verileri geçirin.

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
