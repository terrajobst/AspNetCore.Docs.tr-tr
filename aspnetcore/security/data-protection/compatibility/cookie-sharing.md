---
title: "Tanımlama bilgilerini uygulamalar arasında paylaşma"
author: rick-anderson
description: "Bu belge kimlik doğrulaması tanımlama bilgileri ASP.NET arasında paylaşılmasını veri koruma yığını nasıl desteklediğini açıklar 4.x ve ASP.NET Core uygulamaları."
keywords: "ASP.NET paylaşımı Core,ASP.NET,cookies,Interop,cookie"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="1550a-104">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="1550a-104">Sharing cookies between applications</span></span>

<span data-ttu-id="1550a-105">Web sitelerine yaygın olarak birçok tek tek web uygulamalarının, tüm çalışma birlikte harmoniously oluşur.</span><span class="sxs-lookup"><span data-stu-id="1550a-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="1550a-106">Bir uygulama geliştiricisi iyi bir çoklu oturum açma deneyimi sağlamak istiyorsa, bunlar genellikle tüm kimlik doğrulama biletlerini birbirine arasında paylaşmak için site içinde bulunan farklı web uygulamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="1550a-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="1550a-107">Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1550a-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="1550a-108">Kimlik doğrulaması tanımlama bilgileri uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="1550a-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="1550a-109">Kimlik doğrulama tanımlama bilgisi iki farklı ASP.NET Core uygulamalar arasında paylaşmak için tanımlama bilgileri aşağıdaki gibi paylaşmalıdır her bir uygulama yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1550a-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="1550a-110">İçinde yöntemini yapılandırmak için veri koruma hizmeti tanımlama bilgileri ve AuthenticationScheme için ASP.NET eşleşecek şekilde ayarlamak için CookieAuthenticationOptions kullanın 4.x.</span><span class="sxs-lookup"><span data-stu-id="1550a-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="1550a-111">Kimlik kullanıyorsanız:</span><span class="sxs-lookup"><span data-stu-id="1550a-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="1550a-112">Tanımlama bilgilerini doğrudan kullanıyorsanız:</span><span class="sxs-lookup"><span data-stu-id="1550a-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="1550a-113">`DataProtectionProvider` Gerektirir `Microsoft.AspNetCore.DataProtection.Extensions` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="1550a-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="1550a-114">Bu şekilde kullanıldığında, DirectoryInfo özellikle kimlik doğrulaması tanımlama bilgileri için ayrılan bir anahtar depolama konumuna işaret etmelidir.</span><span class="sxs-lookup"><span data-stu-id="1550a-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="1550a-115">Tanımlama bilgisi kimlik doğrulaması Ara artık DataProtectionProvider açıkça belirtilen uyarlamasını kullanacağı diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılır.</span><span class="sxs-lookup"><span data-stu-id="1550a-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="1550a-116">Uygulama adı göz ardı edilir (isteyerek bunu, yüklerini paylaşmak için birden çok uygulama almak çalıştığınızdan).</span><span class="sxs-lookup"><span data-stu-id="1550a-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="1550a-117">DataProtectionProvider olarak şifrelenmiş anahtarlar, bekleyen güvenilecek şekilde yapılandırmayı dikkate almanız gereken örnek aşağıda.</span><span class="sxs-lookup"><span data-stu-id="1550a-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="1550a-118">ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="1550a-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="1550a-119">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaları ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1550a-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1550a-120">Bu, büyük bir sitenin tek tek uygulamalar parça parça hala kesintisiz çoklu oturum açma deneyimi site genelinde sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1550a-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="1550a-121">Mevcut uygulamanızda projenizin Startup.Auth.cs UseCookieAuthentication çağrısı varlığını tarafından Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanıp kullanmadığını anlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1550a-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="1550a-122">ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="1550a-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="1550a-123">ASP.NET 4.x uygulamanız .NET Framework 4.5.1 hedeflemesi gerekir veya üzeri, aksi takdirde gerekli NuGet paketlerini yüklemek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1550a-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="1550a-124">Kimlik doğrulaması tanımlama bilgileri, ASP.NET 4.x uygulamalarınızı ve ASP.NET Core uygulamalarınız arasında paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek, ASP.NET 4.x uygulamalarınızı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1550a-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="1550a-125">Paket Microsoft.Owin.Security.Interop her ASP.NET 4.x uygulamalarınızı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1550a-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="1550a-126">Startup.Auth.cs içinde genellikle şuna benzer UseCookieAuthentication çağrısı bulun.</span><span class="sxs-lookup"><span data-stu-id="1550a-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="1550a-127">UseCookieAuthentication çağrısı şu şekilde değiştirin, bir anahtar depolama konumuna başlatıldı DataProtectionProvider örneği sağlayarak ASP.NET Core tanımlama bilgisi kimlik doğrulaması Ara tarafından kullanılan adla eşleşecek şekilde Tanımlamabilgisiadı değiştirme ve Kümeleme biçimi uyumlu olacak şekilde CookieManager birlikte çalışma ChunkingCookieManager ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1550a-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

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
    <span data-ttu-id="1550a-128">ASP.NET Core uygulamanıza işaret ve aynı ayarları kullanarak yapılandırılmalıdır aynı depolama konumunu işaret edecek şekilde DirectoryInfo sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1550a-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="1550a-129">ASP.NET 4.x ve ASP.NET Core uygulamaları artık kimlik doğrulaması tanımlama bilgileri paylaşmak için yapılandırılmışsa.</span><span class="sxs-lookup"><span data-stu-id="1550a-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="1550a-130">Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="1550a-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="1550a-131">Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgileri eşleşecek şekilde çalıştığında kimlik sistemi hataları çalışma zamanında oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1550a-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
