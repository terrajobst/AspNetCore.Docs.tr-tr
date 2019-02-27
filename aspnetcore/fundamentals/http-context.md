---
title: ASP.NET core'da erişim HttpContext
author: coderandhiker
description: ASP.NET core'da HttpContext erişmeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 446882297524af3cbaed3ba7f941935debf5e7f4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899209"
---
# <a name="access-httpcontext-in-aspnet-core"></a>ASP.NET core'da erişim HttpContext

ASP.NET Core uygulamaları erişim `HttpContext` aracılığıyla [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) arabirimi ile varsayılan uygulaması [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). Yalnızca kullanmak gerekli olan `IHttpContextAccessor` erişim gerektiğinde `HttpContext` bir hizmeti içinde.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Razor sayfaları'ndan HttpContext kullanın

Razor sayfaları [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sunan [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) özelliği:

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

## <a name="use-httpcontext-from-a-razor-view"></a>HttpContext Razor görünümünü kullanın

Razor görünümleri ile açığa `HttpContext` aracılığıyla doğrudan bir [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) görünümü özelliği. Aşağıdaki örnek, Windows kimlik doğrulaması kullanarak bir Intranet uygulaması geçerli kullanıcı adını alır:

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>Bir denetleyiciden HttpContext kullanın

Denetleyicileri sunmaya [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) özelliği:

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

## <a name="use-httpcontext-from-middleware"></a>Ara yazılımı HttpContext kullanın

Özel bir ara yazılım bileşenleri ile çalışırken `HttpContext` yöntemlere geçirilen `Invoke` veya `InvokeAsync` yöntemi ve ara yazılım yapılandırıldığında erişilebilir:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>HttpContext özel bileşenlerini kullanma

Diğer framework ve erişim gerektiren özel bileşenler için `HttpContext`, yerleşik kullanarak System.Data.sqlclient'ı bağımlılık kaydetmek için önerilen yaklaşımdır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. Bağımlılık ekleme kapsayıcısını kaynakları `IHttpContextAccessor` bağımlılık olarak oluşturucuları bildirme herhangi sınıflar.

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

* `UserRepository` üzerinde bağımlılık bildirir `IHttpContextAccessor`.
* Bağımlılık ekleme bağımlılık zincirinden çözümler ve bir örneğini oluşturur, bağımlılık sağlanan `UserRepository`.

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

## <a name="httpcontext-access-from-a-background-thread"></a>Arka plan iş parçacığından HttpContext erişim

`HttpContext` iş parçacığı açısından güvenli değildir. Okuma veya yazma özelliklerinin `HttpContext` bir isteği işlerken dışında neden olabilir bir `NullReferenceException`.

> [!NOTE]
> Kullanarak `HttpContext` genellikle bir isteğin işlenmesinin dışında sonuçlanır bir `NullReferenceException`. Uygulamanız kullanımın oluşturuyorsa `NullReferenceException`s, kodun arka plan işlemesi başlatın veya bir istek tamamlandıktan sonra işleme devam gözden geçirme bölümleri. Bir denetleyici yöntemi olarak tanımlama gibi bir hataları arayın `async void`.

Arka plan işleri ile güvenli bir şekilde gerçekleştirmek için `HttpContext` veri:

* İstek işlenirken gerekli verileri kopyalayın.
* Kopyalanan veriler için bir arka plan görevi geçirin.

Güvenli olmayan kod önlemek için hiçbir zaman geçmesi `HttpContext` çalışma - arka planda bir yönteme ihtiyacınız bunun yerine verileri geçirin.

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

