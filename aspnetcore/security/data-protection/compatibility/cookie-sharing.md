---
title: "Tanımlama bilgilerini uygulamalar arasında paylaşma"
author: rick-anderson
description: "Bu belge kimlik doğrulaması tanımlama bilgileri ASP.NET arasında paylaşılmasını veri koruma yığını nasıl desteklediğini açıklar 4.x ve ASP.NET Core uygulamaları."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 0cbf5a3e9dfe8f99433800ac5c10ed36b4de6527
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="sharing-cookies-between-applications"></a>Tanımlama bilgilerini uygulamalar arasında paylaşma

Web sitelerine yaygın olarak birçok tek tek web uygulamalarının, tüm çalışma birlikte harmoniously oluşur. Bir uygulama geliştiricisi iyi bir çoklu oturum açma deneyimi sağlamak istiyorsa, bunlar genellikle tüm kimlik doğrulama biletlerini birbirine arasında paylaşmak için site içinde bulunan farklı web uygulamaları gerekir.

Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.

## <a name="sharing-authentication-cookies-between-applications"></a>Kimlik doğrulaması tanımlama bilgileri uygulamalar arasında paylaşma

Kimlik doğrulama tanımlama bilgisi iki farklı ASP.NET Core uygulamalar arasında paylaşmak için tanımlama bilgileri aşağıdaki gibi paylaşmalıdır her bir uygulama yapılandırın.

İçinde yöntemini yapılandırmak için veri koruma hizmeti tanımlama bilgileri ve AuthenticationScheme için ASP.NET eşleşecek şekilde ayarlamak için CookieAuthenticationOptions kullanın 4.x.

Kimlik kullanıyorsanız:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Tanımlama bilgilerini doğrudan kullanıyorsanız:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
`DataProtectionProvider` Gerektirir `Microsoft.AspNetCore.DataProtection.Extensions` NuGet paketi.

Bu şekilde kullanıldığında, DirectoryInfo özellikle kimlik doğrulaması tanımlama bilgileri için ayrılan bir anahtar depolama konumuna işaret etmelidir. Tanımlama bilgisi kimlik doğrulaması Ara artık DataProtectionProvider açıkça belirtilen uyarlamasını kullanacağı diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılır. Uygulama adı göz ardı edilir (isteyerek bunu, yüklerini paylaşmak için birden çok uygulama almak çalıştığınızdan).

>[!WARNING]
>DataProtectionProvider olarak şifrelenmiş anahtarlar, bekleyen güvenilecek şekilde yapılandırmayı dikkate almanız gereken örnek aşağıda.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları

Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaları ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir. Bu, büyük bir sitenin tek tek uygulamalar parça parça hala kesintisiz çoklu oturum açma deneyimi site genelinde sağlarken yükseltme olanağı sağlar.

>[!TIP]
> Mevcut uygulamanızda projenizin Startup.Auth.cs UseCookieAuthentication çağrısı varlığını tarafından Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanıp kullanmadığını anlayabilirsiniz. ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.

> [!NOTE]
> ASP.NET 4.x uygulamanız .NET Framework 4.5.1 hedeflemesi gerekir veya üzeri, aksi takdirde gerekli NuGet paketlerini yüklemek başarısız olur.

Kimlik doğrulaması tanımlama bilgileri, ASP.NET 4.x uygulamalarınızı ve ASP.NET Core uygulamalarınız arasında paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek, ASP.NET 4.x uygulamalarınızı yapılandırın.

1.  Paket Microsoft.Owin.Security.Interop her ASP.NET 4.x uygulamalarınızı yükleyin.

2.   Startup.Auth.cs içinde genellikle şuna benzer UseCookieAuthentication çağrısı bulun.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  UseCookieAuthentication çağrısı şu şekilde değiştirin, bir anahtar depolama konumuna başlatıldı DataProtectionProvider örneği sağlayarak ASP.NET Core tanımlama bilgisi kimlik doğrulaması Ara tarafından kullanılan adla eşleşecek şekilde Tanımlamabilgisiadı değiştirme ve Kümeleme biçimi uyumlu olacak şekilde CookieManager birlikte çalışma ChunkingCookieManager ayarlayın.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    ASP.NET Core uygulamanıza işaret ve aynı ayarları kullanarak yapılandırılmalıdır aynı depolama konumunu işaret edecek şekilde DirectoryInfo sahiptir.

ASP.NET 4.x ve ASP.NET Core uygulamaları artık kimlik doğrulaması tanımlama bilgileri paylaşmak için yapılandırılmışsa.

> [!NOTE]
> Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor emin olmak gerekir. Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgileri eşleşecek şekilde çalıştığında kimlik sistemi hataları çalışma zamanında oluşturur.
