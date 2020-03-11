---
title: ASP.NET Core 2,1 MVC SameSite tanımlama bilgisi örneği
author: rick-anderson
description: ASP.NET Core 2,1 MVC SameSite tanımlama bilgisi örneği
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite/mvc21
ms.openlocfilehash: 14f3d3d27d5740519e1ba57529d5694b4cdeb9ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667750"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a>ASP.NET Core 2,1 MVC SameSite tanımlama bilgisi örneği

ASP.NET Core 2,1, [SameSite](https://www.owasp.org/index.php/SameSite) özniteliği için yerleşik desteğe sahiptir, ancak özgün standart olarak yazılmıştır. [Düzeltme eki uygulanmış davranış](https://github.com/dotnet/aspnetcore/issues/8212) , `SameSite.None` değerini, değeri hiçbir şekilde yaymaması yerine, bir `None`değeri ile birlikte göstermek için anlamını değiştirdi. Değeri göstermek istiyorsanız, bir tanımlama bilgisinde `SameSite` özelliğini-1 olarak ayarlayabilirsiniz.

## <a name="sampleCode"></a>SameSite özniteliği yazılıyor

Bir tanımlama bilgisine bir SameSite özniteliği yazma örneği aşağıda verilmiştir:

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a>Tanımlama bilgisi kimlik doğrulaması ve oturum durumu tanımlama bilgilerini ayarlama

Tanımlama bilgisi kimlik doğrulaması, oturum durumu ve [diğer diğer bileşenler, farklı](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) site seçeneklerini tanımlama bilgisi seçenekleri aracılığıyla ayarlar, örneğin

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

Yukarıdaki kodda, tanımlama bilgisi kimlik doğrulaması ve oturum durumu `None`olarak ayarlanır, özniteliği bir `None` değeriyle yayın ve ayrıca güvenli özniteliğini true olarak ayarlayın.

### <a name="run-the-sample"></a>Örneği çalıştırma

[Örnek projeyi](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)çalıştırırsanız, tarayıcı hata ayıklayıcıyı ilk sayfaya yükleyin ve site için tanımlama bilgisi toplamasını görüntülemek için kullanın. Bunu kenar ve Chrome 'a bas `F12` için `Application` sekmesini seçin ve `Storage` bölümündeki `Cookies` seçeneğinin altındaki site URL 'sine tıklayın.

![Tarayıcı hata ayıklayıcısı tanımlama bilgisi listesi](BrowserDebugger.png)

"SameSite tanımlama bilgisi oluştur" düğmesine tıkladığınızda örnek tarafından oluşturulan tanımlama bilgisinin, [örnek kodda](#sampleCode)ayarlanan değerle eşleşen `Lax`bir SameSite özniteliği değeri olduğunu görürsünüz.

## <a name="interception"></a>Tanımlama bilgilerini kesintiye

Tanımlama bilgilerini ele almak için, kullanıcının tarayıcı aracısındaki desteğine göre None değerini ayarlamak üzere `CookiePolicy` ara yazılımını kullanmanız gerekir. Bu, tanımlama bilgilerini yazan ve `ConfigureServices()`içinde yapılandırılan bileşenlerden **önce** http istek ardışık düzenine yerleştirilmelidir.

İşlem hattına eklemek için [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs)içindeki `Configure(IApplicationBuilder, IHostingEnvironment)` yönteminde `app.UseCookiePolicy()` kullanın. Örnek:

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Ardından `ConfigureServices(IServiceCollection services)` tanımlama bilgisi ilkesini, tanımlama bilgileri eklendiği veya silindiği bir yardımcı sınıfa çağırmak üzere yapılandırın. Örnek:

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

Yardımcı işlevi `CheckSameSite(HttpContext, CookieOptions)`:

* , İstek için tanımlama bilgilerinin eklendiği veya istekten silindiği zaman çağrılır.
* `SameSite` özelliğinin `None`olarak ayarlanmış olup olmadığını denetler.
* `SameSite` `None` olarak ayarlanırsa ve geçerli kullanıcı aracısının None öznitelik değerini desteklemediği bilinmektedir. Denetim, [Samesitesupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) sınıfı kullanılarak yapılır:
  * Özelliği `(SameSiteMode)(-1)` olarak ayarlayarak değeri yaymayan `SameSite` ayarlar

## <a name="targeting-net-framework"></a>Hedefleme .NET Framework

ASP.NET Core ve System. Web (ASP.NET Classic), SameSite 'nin bağımsız uygulamalarına sahiptir. ASP.NET Core kullanılıyorsa veya System. Web SameSite en düşük çerçeve sürümü gereksinimi (.NET 4.7.2) ASP.NET Core için .NET Framework, için SameSite KB yamaları gerekli değildir.

.NET üzerindeki ASP.NET Core, uygun düzeltmeleri almak için NuGet paket bağımlılıklarının güncelleştirilmesini gerektirir.

.NET Framework ASP.NET Core değişiklikleri almak için, düzeltme eki uygulanmış paketlere ve sürümlere (2.1.14 veya üzeri 2,1 sürümler) doğrudan başvurtığınızdan emin olun.

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a>Daha Fazla Bilgi
 
[Chrome güncelleştirmeleri](https://www.chromium.org/updates/same-site)
[ASP.NET Core samesite belgeleri](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2,1 SameSite değişiklik duyurusu](https://github.com/dotnet/aspnetcore/issues/8212)