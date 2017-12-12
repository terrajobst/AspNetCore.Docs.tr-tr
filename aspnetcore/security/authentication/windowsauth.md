---
title: "ASP.NET Core Windows kimlik doğrulamasını yapılandırma"
author: ardalis
description: "Bu makalede, ASP.NET çekirdek IIS Express, IIS, HTTP.sys ve WebListener kullanarak, Windows kimlik doğrulamasını yapılandırmak açıklar."
keywords: "ASP.NET Core, Windows kimlik doğrulaması, yetkilendirme özniteliği, AllowAnonymous özniteliği"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="fd5e9-104">Bir ASP.NET Core uygulamada Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fd5e9-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="fd5e9-105">Tarafından [Steve Smith](https://ardalis.com) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="fd5e9-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="fd5e9-106">Windows kimlik doğrulaması, IIS ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [HTTP.sys](xref:fundamentals/servers/httpsys), veya [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="fd5e9-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="fd5e9-107">Windows kimlik doğrulaması nedir?</span><span class="sxs-lookup"><span data-stu-id="fd5e9-107">What is Windows authentication?</span></span>

<span data-ttu-id="fd5e9-108">Windows kimlik doğrulaması ASP.NET Core uygulamaları kullanıcıların kimliklerini işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="fd5e9-109">Sunucunuzu kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya diğer Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="fd5e9-110">Windows kimlik doğrulaması kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için uygundur.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="fd5e9-111">[Windows kimlik doğrulaması ve IIS için yükleme hakkında daha fazla bilgi](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="fd5e9-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="fd5e9-112">Bir ASP.NET Core uygulamasında Windows kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="fd5e9-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="fd5e9-113">Visual Studio Web uygulaması şablonu, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="fd5e9-114">Windows kimlik doğrulaması uygulama şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="fd5e9-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="fd5e9-115">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-115">In Visual Studio:</span></span>
1. <span data-ttu-id="fd5e9-116">Yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="fd5e9-117">Web uygulaması şablonları listesinden seçin.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="fd5e9-118">Seçin **kimlik doğrulamayı Değiştir** düğmesine tıklayın ve ardından **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="fd5e9-119">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-119">Run the app.</span></span> <span data-ttu-id="fd5e9-120">Kullanıcı adı görünür uygulama sağında.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-120">The username appears in the top right of the app.</span></span>

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="fd5e9-122">IIS Express kullanarak geliştirme iş için Windows kimlik doğrulaması kullanmak için gerekli tüm yapılandırma şablonu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="fd5e9-123">Aşağıdaki bölümde el ile bir ASP.NET Core uygulama Windows kimlik doğrulaması için nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="fd5e9-124">Windows ve anonim kimlik doğrulaması için Visual Studio ayarları</span><span class="sxs-lookup"><span data-stu-id="fd5e9-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="fd5e9-125">Visual Studio projesi **özellikleri** sayfanın **hata ayıklama** sekmesi, Windows kimlik doğrulaması ve anonim kimlik doğrulaması için onay kutularını sağlar.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="fd5e9-127">Alternatif olarak, bu iki özellik de yapılandırılabilir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="fd5e9-128">IIS Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="fd5e9-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="fd5e9-129">IIS kullanan [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) (ASP.NET Core uygulamaları barındırmak için ANCM).</span><span class="sxs-lookup"><span data-stu-id="fd5e9-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="fd5e9-130">ANCM akışları için Windows kimlik doğrulamasını IIS varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="fd5e9-131">Windows kimlik doğrulaması yapılandırmasını, IIS, uygulama projesi içinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="fd5e9-132">Aşağıdaki bölümlerde ASP.NET Core uygulamayı Windows kimlik doğrulaması kullanacak şekilde yapılandırmak için IIS Yöneticisi'ni kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="fd5e9-133">Yeni bir IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="fd5e9-133">Create a new IIS site</span></span>

<span data-ttu-id="fd5e9-134">Bir ad ve klasörü belirtin ve yeni bir uygulama havuzu oluşturmak izin verin.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="fd5e9-135">Kimlik doğrulama özelleştirme</span><span class="sxs-lookup"><span data-stu-id="fd5e9-135">Customize authentication</span></span>

<span data-ttu-id="fd5e9-136">Site için kimlik doğrulama menüsünü açın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-136">Open the Authentication menu for the site.</span></span>

![IIS kimlik doğrulaması menüsü](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="fd5e9-138">Anonim kimlik doğrulamasını devre dışı bırakın ve Windows kimlik doğrulamasını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS kimlik doğrulama ayarları](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="fd5e9-140">IIS site klasörüne projenizin Yayımla</span><span class="sxs-lookup"><span data-stu-id="fd5e9-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="fd5e9-141">Visual Studio veya .NET Core CLI kullanarak, hedef klasöre uygulamayı yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio yayımlama iletişim kutusu](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="fd5e9-143">Daha fazla bilgi edinmek [IIS yayımlama](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="fd5e9-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="fd5e9-144">Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="fd5e9-145">HTTP.sys veya WebListener Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="fd5e9-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd5e9-146">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="fd5e9-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd5e9-147">Kestrel Windows kimlik doğrulamasını desteklemez, ancak kullanabilirsiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows kendini barındıran senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="fd5e9-148">Aşağıdaki örnek HTTP.sys Windows kimlik doğrulaması ile kullanmak için uygulamanın web ana bilgisayarı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd5e9-149">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="fd5e9-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd5e9-150">Kestrel Windows kimlik doğrulamasını desteklemez, ancak kullanabilirsiniz [WebListener](xref:fundamentals/servers/weblistener) Windows kendini barındıran senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="fd5e9-151">Aşağıdaki örnek WebListener Windows kimlik doğrulaması ile kullanmak için uygulamanın web ana bilgisayarı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="fd5e9-152">Windows kimlik doğrulaması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="fd5e9-152">Work with Windows authentication</span></span>

<span data-ttu-id="fd5e9-153">Anonim erişim yapılandırma durumunu şekilde belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="fd5e9-154">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumlarını nasıl ele alınacağını açıklamaktadır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="fd5e9-155">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="fd5e9-155">Disallow anonymous access</span></span>

<span data-ttu-id="fd5e9-156">Windows kimlik doğrulaması etkinleştirildi ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` öznitelikleri etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="fd5e9-157">IIS sitesi (veya HTTP.sys veya WebListener sunucusu) anonim erişime izin vermeyecek şekilde yapılandırılmışsa, istek uygulamanıza hiçbir zaman ulaşır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="fd5e9-158">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="fd5e9-159">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="fd5e9-159">Allow anonymous access</span></span>

<span data-ttu-id="fd5e9-160">Windows kimlik doğrulaması ve anonim erişim etkin olduğunda, kullanın `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="fd5e9-161">`[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="fd5e9-162">`[AllowAnonymous]` Özniteliği geçersiz kılmaları `[Authorize]` özniteliği anonim erişime izin veren uygulamaların içindeki kullanım.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="fd5e9-163">Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="fd5e9-164">ASP.NET Core içinde 2.x `[Authorize]` öznitelik, bir ek yapılandırma gerektirir *haline* Windows kimlik doğrulaması için anonim isteklere sınama için.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="fd5e9-165">Önerilen yapılandırma kullanılan web sunucusu göre biraz değişir.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="fd5e9-166">IIS</span><span class="sxs-lookup"><span data-stu-id="fd5e9-166">IIS</span></span>

<span data-ttu-id="fd5e9-167">IIS kullanıyorsanız, aşağıdaki ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="fd5e9-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd5e9-168">HTTP.sys</span></span>

<span data-ttu-id="fd5e9-169">HTTP.sys kullanıyorsanız, aşağıdaki ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fd5e9-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="fd5e9-170">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="fd5e9-170">Impersonation</span></span>

<span data-ttu-id="fd5e9-171">ASP.NET Core kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="fd5e9-172">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekleri için uygulama kimliği ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="fd5e9-173">Açıkça bir kullanıcı adına bir eylem gerçekleştirmeniz gerekirse, kullanmak `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="fd5e9-174">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="fd5e9-175">Unutmayın `RunImpersonated` zaman uyumsuz işlemleri desteklemez ve karmaşık senaryolar için kullanılmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="fd5e9-176">Örneğin, tüm istekleri ya da Ara yazılım zincirleri kaydırma desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="fd5e9-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
