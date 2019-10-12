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
# <a name="access-httpcontext-in-aspnet-core"></a>ASP.NET Core 'de HttpContext 'e erişme

ASP.NET Core uygulamalar [ıhttpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ve varsayılan uygulama [httpcontextaccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor)aracılığıyla `HttpContext` ' a erişir. Yalnızca bir hizmet içindeki `HttpContext` ' e erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.

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

Razor görünümleri, `HttpContext` ' yı doğrudan görünümdeki bir [RazorPage. Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) özelliği aracılığıyla kullanıma sunar. Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir Intranet uygulamasındaki geçerli kullanıcı adını alır:

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

@No__t-0 ' a erişimi gerektiren diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir. Bağımlılık ekleme kapsayıcısı, @no__t oluşturucularını bir bağımlılık olarak bildiren tüm sınıflara sağlar.

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

* `UserRepository` `IHttpContextAccessor` ' deki bağımlılığını bildirir.
* Bağımlılık ekleme, bağımlılık zincirini çözümlediğinde ve bir @no__t örneği oluşturduğunda bağımlılık sağlanır.

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

`HttpContext`, iş parçacığı açısından güvenli değildir. @No__t bir isteği işlemenin dışında @no__t okuma veya yazma özellikleri, 1 ile sonuçlanabilir.

> [!NOTE]
> Bir isteği işlemenin dışında `HttpContext` kullanmak genellikle `NullReferenceException` ile sonuçlanır. Uygulamanız tek biçimli `NullReferenceException`s oluşturursa, arka plan işlemesini Başlatan kodun bölümlerini gözden geçirin veya istek tamamlandıktan sonra işlemeye devam edin. @No__t-0 olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.

@No__t-0 verileriyle arka plan çalışmasını güvenle gerçekleştirmek için:

* İstek işleme sırasında gerekli verileri kopyalayın.
* Kopyalanmış verileri bir arka plan görevine geçirin.

Güvenli olmayan koddan kaçınmak için, `HttpContext` ' ı hiçbir şekilde arka plan iş yapan bir yönteme iletmeyin, bunun yerine ihtiyacınız olan verileri geçirin.

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
