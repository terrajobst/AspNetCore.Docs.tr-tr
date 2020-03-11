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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658748"
---
# <a name="access-httpcontext-in-aspnet-core"></a>ASP.NET Core 'de HttpContext 'e erişme

<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> arabirimi ve varsayılan uygulama <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>ASP.NET Core uygulamalara erişim `HttpContext`. Yalnızca bir hizmet içindeki `HttpContext` erişmeniz gerektiğinde `IHttpContextAccessor` kullanılması gerekir.

## <a name="use-httpcontext-from-razor-pages"></a>Razor Pages HttpContext kullanın

Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> özelliğini kullanıma sunar:

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

## <a name="use-httpcontext-from-a-razor-view"></a>Razor görünümünden HttpContext kullanma

Razor görünümleri görünümdeki bir [RazorPage. Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) özelliği aracılığıyla `HttpContext` doğrudan kullanıma sunar. Aşağıdaki örnek, Windows kimlik doğrulamasını kullanarak bir intranet uygulamasındaki geçerli kullanıcı adını alır:

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a>Bir denetleyiciden HttpContext kullanma

Denetleyiciler [ControllerBase. HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) özelliğini kullanıma sunar:

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

## <a name="use-httpcontext-from-middleware"></a>Ara yazılım içinden HttpContext kullanın

Özel ara yazılım bileşenleriyle çalışırken, `HttpContext` `Invoke` veya `InvokeAsync` yöntemine geçirilir ve ara yazılım yapılandırıldığında erişilebilir:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Özel bileşenlerden HttpContext kullanın

`HttpContext`erişmesi gereken diğer Framework ve özel bileşenler için önerilen yaklaşım, yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısını kullanarak bir bağımlılığı kaydetmesidir. Bağımlılık ekleme kapsayıcısı, `IHttpContextAccessor` oluşturuculara bir bağımlılık olarak bildiren tüm sınıflara sağlar:

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

`HttpContext` iş parçacığı açısından güvenli değildir. `HttpContext` bir isteği işlemenin dışında okuma veya yazma özellikleri <xref:System.NullReferenceException>sonuçlanabilir.

> [!NOTE]
> Uygulamanız tek tek `NullReferenceException` hatalar oluşturursa, arka plan işlemeyi başlatan veya bir istek tamamlandıktan sonra işlemeye devam eden kodun bölümlerini gözden geçirin. `async void`olarak bir denetleyici yöntemi tanımlama gibi hataları arayın.

`HttpContext` verilerle arka plan çalışmasını güvenle gerçekleştirmek için:

* İstek işleme sırasında gerekli verileri kopyalayın.
* Kopyalanmış verileri bir arka plan görevine geçirin.

Güvenli olmayan koddan kaçınmak için `HttpContext`, arka plan çalışması gerçekleştiren bir yönteme hiçbir şekilde iletmeyin. Bunun yerine gerekli verileri geçirin. Aşağıdaki örnekte, bir e-posta göndermeye başlamak için `SendEmailCore` çağırılır. `correlationId`, `HttpContext`değil `SendEmailCore`geçirilir. Kod yürütme `SendEmailCore` tamamlanmasını beklemez:

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
