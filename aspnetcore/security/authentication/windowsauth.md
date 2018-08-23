---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: ardalis
description: Bu makalede, IIS Express, IIS, HTTP.sys ve WebListener kullanarak ASP.NET Core Windows kimlik doğrulaması yapılandırma açıklanır.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 93b1a1de74ef6554d48709b04870f7e23738846b
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41753911"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="aa54c-103">ASP.NET Core Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="aa54c-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="aa54c-104">Tarafından [Steve Smith](https://ardalis.com) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="aa54c-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="aa54c-105">Windows kimlik doğrulaması, IIS ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [HTTP.sys](xref:fundamentals/servers/httpsys), veya [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="aa54c-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="aa54c-106">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="aa54c-106">Windows Authentication</span></span>

<span data-ttu-id="aa54c-107">Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="aa54c-108">Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya diğer Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aa54c-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="aa54c-109">Windows kimlik doğrulaması kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="aa54c-110">[Windows kimlik doğrulaması hakkında daha fazla bilgi edinin ve IIS yükleniyor](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="aa54c-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="aa54c-111">ASP.NET Core uygulaması Windows kimlik doğrulamasını etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="aa54c-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="aa54c-112">Visual Studio Web uygulama şablonu, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="aa54c-113">Windows kimlik doğrulama uygulaması şablonunu kullanma</span><span class="sxs-lookup"><span data-stu-id="aa54c-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="aa54c-114">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="aa54c-114">In Visual Studio:</span></span>

1. <span data-ttu-id="aa54c-115">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="aa54c-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="aa54c-116">Web uygulaması şablonları listesinden seçin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="aa54c-117">Seçin **kimlik doğrulamayı Değiştir** düğmesini tıklatın ve seçin **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="aa54c-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="aa54c-118">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="aa54c-118">Run the app.</span></span> <span data-ttu-id="aa54c-119">Kullanıcı adı görünür uygulamasının sağ.</span><span class="sxs-lookup"><span data-stu-id="aa54c-119">The username appears in the top right of the app.</span></span>

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="aa54c-121">IIS Express kullanarak, geliştirme iş için Windows kimlik doğrulamasını kullanmak gerekli tüm yapılandırma şablon sağlar.</span><span class="sxs-lookup"><span data-stu-id="aa54c-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="aa54c-122">Aşağıdaki bölümde el ile ASP.NET Core uygulaması Windows kimlik doğrulaması için nasıl yapılandırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="aa54c-123">Windows ve anonim kimlik doğrulaması için Visual Studio ayarları</span><span class="sxs-lookup"><span data-stu-id="aa54c-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="aa54c-124">Visual Studio projesini **özellikleri** sayfanın **hata ayıklama** sekmesi, Windows kimlik doğrulaması ve anonim kimlik doğrulaması için onay kutularını sağlar.</span><span class="sxs-lookup"><span data-stu-id="aa54c-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Windows kimlik doğrulaması tarayıcı ekran görüntüsü](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="aa54c-126">Alternatif olarak, bu iki özellik de yapılandırılabilir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="aa54c-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="aa54c-127">IIS ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="aa54c-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="aa54c-128">IIS kullanan [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konak ASP.NET Core uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="aa54c-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="aa54c-129">Windows kimlik doğrulaması, IIS, uygulama yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="aa54c-130">Aşağıdaki bölümlerde, ASP.NET Core uygulaması Windows kimlik doğrulaması kullanacak şekilde yapılandırmak için IIS Yöneticisi'ni kullanmayı göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="aa54c-131">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aa54c-131">IIS configuration</span></span>

<span data-ttu-id="aa54c-132">Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="aa54c-133">Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="aa54c-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="aa54c-134">IIS tümleştirme ara yazılımı varsayılan olarak, varsayılan olarak otomatik olarak isteklerinde kimlik doğrulamak üzere yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="aa54c-135">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçenekleri (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="aa54c-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="aa54c-136">ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="aa54c-137">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="aa54c-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="aa54c-138">Yeni bir IIS sitesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="aa54c-138">Create a new IIS site</span></span>

<span data-ttu-id="aa54c-139">Bir ad ve klasör belirtin ve yeni bir uygulama havuzu oluşturmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="aa54c-140">Kimlik doğrulaması'nı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="aa54c-140">Customize authentication</span></span>

<span data-ttu-id="aa54c-141">Sitesi için kimlik doğrulama özelliklerini açın.</span><span class="sxs-lookup"><span data-stu-id="aa54c-141">Open the Authentication features for the site.</span></span>

![IIS kimlik doğrulaması menüsü](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="aa54c-143">Anonim kimlik doğrulamasını devre dışı bırakın ve Windows kimlik doğrulamasını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS kimlik doğrulama ayarları](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="aa54c-145">Projenizi yayımlamak için IIS site klasörü</span><span class="sxs-lookup"><span data-stu-id="aa54c-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="aa54c-146">Visual Studio veya .NET Core CLI'yı kullanarak uygulamayı hedef klasöre yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="aa54c-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio yayımlama iletişim kutusu](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="aa54c-148">Daha fazla bilgi edinin [IIS yayımlama](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="aa54c-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="aa54c-149">Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="aa54c-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="aa54c-150">HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="aa54c-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="aa54c-151">Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="aa54c-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="aa54c-152">Aşağıdaki örnek, HTTP.sys ile Windows kimlik doğrulaması kullanmak için uygulamanın web ana bilgisayarı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="aa54c-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="aa54c-153">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="aa54c-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="aa54c-154">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="aa54c-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="aa54c-155">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="aa54c-156">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="aa54c-157">WebListener ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="aa54c-157">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="aa54c-158">Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [WebListener](xref:fundamentals/servers/weblistener) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="aa54c-158">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="aa54c-159">Aşağıdaki örnekte, uygulamanın web ana bilgisayarı WebListener Windows kimlik doğrulaması ile kullanılacak yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="aa54c-159">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="aa54c-160">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü WebListener temsil eder.</span><span class="sxs-lookup"><span data-stu-id="aa54c-160">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="aa54c-161">Kullanıcı modu kimlik doğrulaması, Kerberos ve WebListener ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="aa54c-161">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="aa54c-162">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-162">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="aa54c-163">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="aa54c-163">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="aa54c-164">Windows kimlik doğrulaması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="aa54c-164">Work with Windows Authentication</span></span>

<span data-ttu-id="aa54c-165">Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-165">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="aa54c-166">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="aa54c-166">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="aa54c-167">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="aa54c-167">Disallow anonymous access</span></span>

<span data-ttu-id="aa54c-168">Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="aa54c-168">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="aa54c-169">IIS sitesi (veya HTTP.sys veya WebListener sunucusu), anonim erişime izin vermeyecek şekilde yapılandırıldığından, istek, uygulamanızı hiçbir zaman ulaşır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-169">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="aa54c-170">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="aa54c-170">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="aa54c-171">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="aa54c-171">Allow anonymous access</span></span>

<span data-ttu-id="aa54c-172">Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="aa54c-172">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="aa54c-173">`[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-173">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="aa54c-174">`[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamaların içindeki kullanım özniteliği.</span><span class="sxs-lookup"><span data-stu-id="aa54c-174">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="aa54c-175">Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.</span><span class="sxs-lookup"><span data-stu-id="aa54c-175">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="aa54c-176">ASP.NET core'da 2.x `[Authorize]` öznitelik ek yapılandırma gerektirir *Startup.cs* anonim istekler için Windows kimlik doğrulaması meydan okuyun.</span><span class="sxs-lookup"><span data-stu-id="aa54c-176">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="aa54c-177">Önerilen yapılandırma kullanılan web sunucusu göre biraz farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-177">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="aa54c-178">Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="aa54c-178">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="aa54c-179">[StatusCodePages ara yazılım](xref:fundamentals/error-handling#configuring-status-code-pages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="aa54c-179">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="aa54c-180">IIS</span><span class="sxs-lookup"><span data-stu-id="aa54c-180">IIS</span></span>

<span data-ttu-id="aa54c-181">IIS kullanarak aşağıdakileri ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aa54c-181">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="aa54c-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="aa54c-182">HTTP.sys</span></span>

<span data-ttu-id="aa54c-183">HTTP.sys kullanıyorsanız, ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aa54c-183">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="aa54c-184">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="aa54c-184">Impersonation</span></span>

<span data-ttu-id="aa54c-185">ASP.NET Core, kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="aa54c-185">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="aa54c-186">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulama kimliği ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-186">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="aa54c-187">Açıkça bir kullanıcı adına eylem gerçekleştirmek ihtiyacınız varsa `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="aa54c-187">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="aa54c-188">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="aa54c-188">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="aa54c-189">Unutmayın `RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="aa54c-189">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="aa54c-190">Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="aa54c-190">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
