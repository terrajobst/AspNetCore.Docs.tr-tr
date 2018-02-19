---
title: "IIS ile Windows ana ASP.NET Çekirdeği"
author: guardrex
description: "ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde konak öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 620bfefa625f4b39cb2731b4f553caaa4526c71b
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="86beb-103">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="86beb-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="86beb-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86beb-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="86beb-105">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="86beb-105">Supported operating systems</span></span>

<span data-ttu-id="86beb-106">Aşağıdaki işletim sistemlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="86beb-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="86beb-107">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="86beb-107">Windows 7 or later</span></span>
* <span data-ttu-id="86beb-108">Windows Server 2008 R2 veya daha sonra &#8224;</span><span class="sxs-lookup"><span data-stu-id="86beb-108">Windows Server 2008 R2 or later&#8224;</span></span>

<span data-ttu-id="86beb-109">&#8224; Kavramsal olarak, bu belgede açıklanan IIS yapılandırmasını da Nano Server IIS üzerinde ASP.NET Core uygulamaları barındırmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="86beb-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS.</span></span> <span data-ttu-id="86beb-110">Nano Server için özel yönergeler için bkz: [Nano Server IIS ile ASP.NET Core](xref:tutorials/nano-server) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="86beb-110">For instructions specific to Nano Server, see the [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) tutorial.</span></span>

<span data-ttu-id="86beb-111">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)) bir ters proxy yapılandırması IIS ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="86beb-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="86beb-112">Kullanım [Kestrel server](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="86beb-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="86beb-113">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="86beb-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="86beb-114">IISIntegration bileşenleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="86beb-114">Enable the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86beb-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="86beb-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="86beb-116">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlamak için.</span><span class="sxs-lookup"><span data-stu-id="86beb-116">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="86beb-117">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) için bağlantı noktası ve temel yolunu yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="86beb-117">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86beb-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="86beb-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="86beb-119">Bir bağımlılık içerir [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıkları paketinde.</span><span class="sxs-lookup"><span data-stu-id="86beb-119">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="86beb-120">IIS tümleştirme Ara ekleyerek kullanmak [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) genişletme yöntemi [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="86beb-120">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="86beb-121">Her ikisi de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) ve [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="86beb-121">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="86beb-122">Kod arama `UseIISIntegration` kod taşınabilirlik etkilemez.</span><span class="sxs-lookup"><span data-stu-id="86beb-122">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="86beb-123">Uygulama IIS çalıştırırsanız değil (örneğin, uygulama doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="86beb-123">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

---

<span data-ttu-id="86beb-124">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="86beb-124">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="86beb-125">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="86beb-125">IIS options</span></span>

<span data-ttu-id="86beb-126">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) içinde [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="86beb-126">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="86beb-127">Aşağıdaki örnekte, istemci sertifikalarını doldurmak için uygulamasına iletme `HttpContext.Connection.ClientCertificate` devre dışı bırakılır:</span><span class="sxs-lookup"><span data-stu-id="86beb-127">In the following example, forwarding client certificates to the app to populate `HttpContext.Connection.ClientCertificate` is disabled:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="86beb-128">Seçenek</span><span class="sxs-lookup"><span data-stu-id="86beb-128">Option</span></span>                         | <span data-ttu-id="86beb-129">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="86beb-129">Default</span></span> | <span data-ttu-id="86beb-130">Ayar</span><span class="sxs-lookup"><span data-stu-id="86beb-130">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="86beb-131">Varsa `true`, IIS tümleştirme Ara ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="86beb-131">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="86beb-132">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorluklar yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="86beb-132">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="86beb-133">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="86beb-133">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="86beb-134">Daha fazla bilgi için bkz: [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-134">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="86beb-135">Oturum açma sayfalarındaki kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="86beb-135">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="86beb-136">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="86beb-136">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig-file"></a><span data-ttu-id="86beb-137">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="86beb-137">web.config file</span></span>

<span data-ttu-id="86beb-138">*Web.config* dosyası yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="86beb-138">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="86beb-139">Oluşturma, dönüştürme ve yayımlama *web.config* .NET Core Web SDK tarafından yapılır (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="86beb-139">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="86beb-140">SDK proje dosyasının üst kısmında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="86beb-140">The SDK is set  at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="86beb-141">Varsa bir *web.config* dosyası mevcut değil proje dosyası doğru ile oluşturulan *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [çıkış yayımlanan](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="86beb-141">If a *web.config* file isn't present the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="86beb-142">Varsa bir *web.config* dosyası projede mevcut, dosyanın doğru ile dönüştürüldüğünde *processPath* ve *bağımsız değişkenleri* ASP.NET Core Modülü'nü yapılandırmak için ve taşınabilir yayımlanan çıktı.</span><span class="sxs-lookup"><span data-stu-id="86beb-142">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="86beb-143">Dönüşüm dosyasında IIS yapılandırma ayarlarını değiştirme değil.</span><span class="sxs-lookup"><span data-stu-id="86beb-143">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="86beb-144">*Web.config* dosya etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-144">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="86beb-145">ASP.NET Core uygulamaları istekleriyle işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [kullanarak IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-145">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [Using IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="86beb-146">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** proje dosyasında özellik:</span><span class="sxs-lookup"><span data-stu-id="86beb-146">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="86beb-147">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="86beb-147">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="86beb-148">Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="86beb-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="86beb-149">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="86beb-149">web.config file location</span></span>

<span data-ttu-id="86beb-150">ASP.NET Core uygulamaları IIS Kestrel sunucusu arasında ters proxy barındırılır.</span><span class="sxs-lookup"><span data-stu-id="86beb-150">ASP.NET Core apps are hosted in a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="86beb-151">Ters proxy oluşturmak için *web.config* dosya dağıtılan uygulama içerik kök yolda (genellikle uygulama temel yol) var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="86beb-151">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="86beb-152">IIS için sağlanan Web sitesi fiziksel yolu ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="86beb-152">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="86beb-153">*Web.config* Web dağıtımı kullanarak birden çok uygulamaları yayımlamayı etkinleştirmek için uygulama kökü dosyası gereklidir.</span><span class="sxs-lookup"><span data-stu-id="86beb-153">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="86beb-154">Gizli dosyaların mevcut uygulamanın fiziksel yola gibi  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="86beb-154">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="86beb-155">Zaman *web.config* dosya varsa ve ve site normalde başlatır ve bunların istenmesi halinde, IIS bu hassas dosyalar hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="86beb-155">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="86beb-156">Varsa *web.config* dosyası eksik, yanlış olarak adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet.</span><span class="sxs-lookup"><span data-stu-id="86beb-156">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="86beb-157">***Web.config* dosyası düzgün olarak adlandırılan dağıtımdaki her zaman, mevcut ve yukarı site normal başlangıç için yapılandırılamıyor olması gerekir. Hiçbir zaman kaldırmak *web.config* Üretim dağıtımı dosyasından.**</span><span class="sxs-lookup"><span data-stu-id="86beb-157">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="86beb-158">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="86beb-158">IIS configuration</span></span>

<span data-ttu-id="86beb-159">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="86beb-159">**Windows Server operating systems**</span></span>

<span data-ttu-id="86beb-160">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86beb-160">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="86beb-161">Kullanım **rol ve Özellik Ekle** gelen Sihirbazı **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**.</span><span class="sxs-lookup"><span data-stu-id="86beb-161">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="86beb-162">Üzerinde **sunucu rolleri** adım, onay kutusunu için **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="86beb-162">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="86beb-164">Sonra **özellikleri** adım, **rol hizmetlerini** adım Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="86beb-164">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="86beb-165">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="86beb-165">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="86beb-167">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="86beb-167">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="86beb-168">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="86beb-168">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="86beb-169">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="86beb-169">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="86beb-170">Daha fazla bilgi için bkz: [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="86beb-170">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="86beb-171">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="86beb-171">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="86beb-172">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="86beb-172">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="86beb-173">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="86beb-173">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="86beb-174">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="86beb-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="86beb-175">Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="86beb-175">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="86beb-176">Üzerinden devam **onay** web sunucusu rolü ve hizmetlerini yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="86beb-176">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="86beb-177">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değildir **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="86beb-177">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="86beb-178">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="86beb-178">**Windows desktop operating systems**</span></span>

<span data-ttu-id="86beb-179">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="86beb-179">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="86beb-180">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="86beb-180">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="86beb-181">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="86beb-181">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="86beb-182">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="86beb-182">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="86beb-183">Onay kutusunu için **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="86beb-183">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="86beb-184">Onay kutusunu için **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="86beb-184">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="86beb-185">Varsayılan özellikler için kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="86beb-185">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="86beb-186">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="86beb-186">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="86beb-187">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="86beb-187">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="86beb-188">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="86beb-188">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="86beb-189">Daha fazla bilgi için bkz: [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="86beb-189">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="86beb-190">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="86beb-190">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="86beb-191">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="86beb-191">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="86beb-192">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="86beb-192">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="86beb-193">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="86beb-193">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="86beb-194">Daha fazla bilgi için bkz: [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="86beb-194">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="86beb-195">IIS yüklemesi bir yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="86beb-195">If the IIS installation requires a restart, restart the system.</span></span>

![IIS Yönetim Konsolu ve World Wide Web Hizmetleri içinde Windows özellikleri seçilir.](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="86beb-197">.NET Core Windows Server barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="86beb-197">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="86beb-198">Yükleme [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting) barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="86beb-198">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="86beb-199">.NET çekirdeği çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="86beb-199">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="86beb-200">Modül IIS Kestrel sunucusu arasında ters proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="86beb-200">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="86beb-201">Sistem Internet bağlantısı yoksa, edinme ve yükleme [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core Windows Server barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="86beb-201">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

   <span data-ttu-id="86beb-202">**Önemli!**</span><span class="sxs-lookup"><span data-stu-id="86beb-202">**Important!**</span></span> <span data-ttu-id="86beb-203">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="86beb-203">If the hosting bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="86beb-204">IIS yeniden yükledikten sonra barındırma paket yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86beb-204">Run the hosting bundle installer again after installing IIS.</span></span>

1. <span data-ttu-id="86beb-205">Sistemi yeniden başlatın veya yürütme **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="86beb-205">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="86beb-206">IIS yeniden başlatma sistem yolu yükleyici tarafından yapılan bir değişiklik seçer.</span><span class="sxs-lookup"><span data-stu-id="86beb-206">Restarting IIS picks up a change to the system PATH made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="86beb-207">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="86beb-207">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="86beb-208">Visual Studio ile yayımlama sırasında Web dağıtımı yükleyin</span><span class="sxs-lookup"><span data-stu-id="86beb-208">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="86beb-209">Uygulamaları sahip sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="86beb-209">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="86beb-210">Web dağıtımı yüklemek için kullandığınız [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyici elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="86beb-210">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="86beb-211">Tercih edilen yöntem Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="86beb-211">The preferred method is to use WebPI.</span></span> <span data-ttu-id="86beb-212">Webpı, tek başına Kurulum ve barındırma sağlayıcıları için bir yapılandırma sunar.</span><span class="sxs-lookup"><span data-stu-id="86beb-212">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="86beb-213">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="86beb-213">Create the IIS site</span></span>

1. <span data-ttu-id="86beb-214">Barındıran sistemde uygulamanın yayımlanan klasörleri ve dosyaları içermesi için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86beb-214">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="86beb-215">Bir uygulamanın dağıtım düzeni açıklanan [dizin yapısını](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-215">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="86beb-216">Yeni bir klasör içinde oluşturun bir *günlükleri* stdout günlük kaydı etkinleştirildiğinde ASP.NET Core modül stdout günlüklerini tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="86beb-216">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="86beb-217">Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="86beb-217">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="86beb-218">Oluşturmaya ilişkin yönergeler için MSBuild etkinleştirme *günlükleri* otomatik olarak proje yerel olarak yapılandırıldığında klasörü [dizin yapısını](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-218">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   <span data-ttu-id="86beb-219">**Önemli!**</span><span class="sxs-lookup"><span data-stu-id="86beb-219">**Important!**</span></span> <span data-ttu-id="86beb-220">Yalnızca uygulama başlatma hataları gidermek için stdout günlük kullanın.</span><span class="sxs-lookup"><span data-stu-id="86beb-220">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="86beb-221">Hiçbir zaman stdout günlük rutin uygulama günlüğü için kullanın.</span><span class="sxs-lookup"><span data-stu-id="86beb-221">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="86beb-222">Günlük dosyası boyutu bir sınırlama yoktur veya oluşturulan günlük dosyalarını sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="86beb-222">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="86beb-223">Stdout günlüğü hakkında daha fazla bilgi için bkz: [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="86beb-223">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="86beb-224">Bir ASP.NET Core uygulamasında günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlüğü](xref:fundamentals/logging/index) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-224">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="86beb-225">İçinde **IIS Yöneticisi'ni**, sunucunun düğümünde açmak **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="86beb-225">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="86beb-226">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="86beb-226">Right-click the **Sites** folder.</span></span> <span data-ttu-id="86beb-227">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="86beb-227">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="86beb-228">Sağlayan bir **Site adı** ve **fiziksel yolu** uygulamanın dağıtım klasörü için.</span><span class="sxs-lookup"><span data-stu-id="86beb-228">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="86beb-229">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="86beb-229">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

1. <span data-ttu-id="86beb-231">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="86beb-231">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="86beb-232">Sitenin uygulama havuzu sağ tıklatıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="86beb-232">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="86beb-233">İçinde **uygulama havuzunu Düzenle** penceresindeki ayarlayın **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="86beb-233">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="86beb-235">ASP.NET Core ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="86beb-235">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="86beb-236">ASP.NET Core Masaüstü CLR Yükleme kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="86beb-236">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="86beb-237">Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="86beb-237">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="86beb-238">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="86beb-238">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="86beb-239">Uygulama havuzu varsayılan kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimliğini doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve gerekli diğer kaynaklarına erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="86beb-239">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="86beb-240">Örneğin, uygulama havuzu okuma ve yazma erişimi burada uygulama okur ve dosyaları yazma klasörlere gerektirir.</span><span class="sxs-lookup"><span data-stu-id="86beb-240">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="86beb-241">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="86beb-241">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="86beb-242">Daha fazla bilgi için bkz: [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="86beb-242">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="86beb-243">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="86beb-243">Deploy the app</span></span>

<span data-ttu-id="86beb-244">Barındıran sistemde oluşturulan klasöre uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="86beb-244">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="86beb-245">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="86beb-245">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="86beb-246">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="86beb-246">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="86beb-247">Bkz: [ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="86beb-247">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="86beb-248">Barındırma sağlayıcısı bir yayımlama profili veya destek bir oluşturmak için sağlıyorsa, kendi profili indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="86beb-248">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımla iletişim sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="86beb-250">Web dağıtımı dışında Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86beb-250">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="86beb-251">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dışında Visual Studio komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-251">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="86beb-252">Daha fazla bilgi için bkz: [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="86beb-252">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="86beb-253">Web alternatifleri dağıtma</span><span class="sxs-lookup"><span data-stu-id="86beb-253">Alternatives to Web Deploy</span></span>

<span data-ttu-id="86beb-254">Uygulama barındırma sisteminin el ile kopyalama, Xcopy, Robocopy veya PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="86beb-254">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="86beb-255">Web sitesi Gözat</span><span class="sxs-lookup"><span data-stu-id="86beb-255">Browse the website</span></span>

![Microsoft Edge tarayıcısının IIS başlangıç sayfası yükledi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="86beb-257">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="86beb-257">Locked deployment files</span></span>

<span data-ttu-id="86beb-258">Uygulama çalışırken dağıtım klasöründeki dosyaları kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="86beb-258">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="86beb-259">Dağıtım sırasında kilitli dosyaları üzerine yazılamıyor.</span><span class="sxs-lookup"><span data-stu-id="86beb-259">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="86beb-260">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyaları serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan:</span><span class="sxs-lookup"><span data-stu-id="86beb-260">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="86beb-261">Web dağıtımı ve başvuru kullanmak `Microsoft.NET.Sdk.Web` proje dosyasında.</span><span class="sxs-lookup"><span data-stu-id="86beb-261">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="86beb-262">Bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-262">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="86beb-263">Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="86beb-263">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="86beb-264">Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="86beb-264">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="86beb-265">El ile IIS Yöneticisi'nde uygulama havuzu sunucuda durdurun.</span><span class="sxs-lookup"><span data-stu-id="86beb-265">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="86beb-266">Durdurup (PowerShell 5 veya sonraki sürümünü gerektirir) uygulama havuzu yeniden başlatmak için PowerShell kullanın:</span><span class="sxs-lookup"><span data-stu-id="86beb-266">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a><span data-ttu-id="86beb-267">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="86beb-267">Data protection</span></span>

<span data-ttu-id="86beb-268">[ASP.NET Core veri koruması yığın](xref:security/data-protection/index) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="86beb-268">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="86beb-269">Kullanıcı kodu tarafından veri koruma API çağrısı olmayan olsa bile, veri koruma bir kalıcı oluşturmak için kullanıcı kodu veya bir dağıtım komut dosyası ile yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="86beb-269">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="86beb-270">Veri koruma yapılandırılmamışsa, anahtarları bellekte tutulması ve uygulama yeniden başlatıldığında atılır.</span><span class="sxs-lookup"><span data-stu-id="86beb-270">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="86beb-271">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="86beb-271">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="86beb-272">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="86beb-272">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="86beb-273">Kullanıcıların kendi bir sonraki istekte yeniden oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="86beb-273">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="86beb-274">Anahtar halkası ile korunan tüm verileri artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-274">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="86beb-275">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf) ve [ASP.NET Core MVC tempdata tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="86beb-275">This may include [CSRF tokens](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf) and [ASP.NET Core MVC tempdata cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="86beb-276">Veri koruma anahtarı halkası kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan:</span><span class="sxs-lookup"><span data-stu-id="86beb-276">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="86beb-277">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="86beb-277">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="86beb-278">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları uygulamalara dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="86beb-278">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="86beb-279">Belirli bir uygulamanın tuşları kalıcı hale getirmek için kayıt defteri anahtarları için uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86beb-279">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="86beb-280">Tek başına, webfarm olmayan IIS yüklemeleri için [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) bir ASP.NET Core uygulamayla kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-280">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="86beb-281">Bu komut dosyası, kayıt defterinde yalnızca çalışan işlem hesabına uygulamanın uygulama havuzunun erişilebilen HKLM bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="86beb-281">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="86beb-282">Anahtarları bir makineye özel anahtarla DPAPI kullanarak REST şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="86beb-282">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="86beb-283">Web grubu senaryolarda, bir uygulama, veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak üzere yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-283">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="86beb-284">Varsayılan olarak, veri koruma anahtarlarını şifreli değil.</span><span class="sxs-lookup"><span data-stu-id="86beb-284">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="86beb-285">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="86beb-285">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="86beb-286">Sertifika, REST anahtarlarınızı korumak için kullanılabilir bir X509.</span><span class="sxs-lookup"><span data-stu-id="86beb-286">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="86beb-287">Sertifikaları karşıya yüklemesine izin vermek için bir mekanizma göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="86beb-287">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="86beb-288">Bkz: [yapılandırma veri koruması](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="86beb-288">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="86beb-289">**Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="86beb-289">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="86beb-290">Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="86beb-290">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="86beb-291">Kümesine kullanıcı profilini Yükle `True`.</span><span class="sxs-lookup"><span data-stu-id="86beb-291">Set Load User Profile to `True`.</span></span> <span data-ttu-id="86beb-292">Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu tarafından kullanılan kullanıcı hesabının belirli bir anahtar kullanarak.</span><span class="sxs-lookup"><span data-stu-id="86beb-292">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="86beb-293">**Dosya sistemi anahtar halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="86beb-293">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="86beb-294">Uygulama kodu ayarlamak [bir anahtar halkası deposu olarak dosya sistemini kullanmak](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="86beb-294">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="86beb-295">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifikanın güvenilir bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="86beb-295">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="86beb-296">Sertifika otomatik olarak imzalanan ise, sertifikayı güvenilir kök deposuna yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="86beb-296">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="86beb-297">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="86beb-297">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="86beb-298">Tüm makineler erişebileceği bir dosya paylaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="86beb-298">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="86beb-299">Bir X509 dağıtmak her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="86beb-299">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="86beb-300">Yapılandırma [veri koruması kodda](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="86beb-300">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="86beb-301">**Veri koruması için bir makine genelinde İlkesi ayarlama**</span><span class="sxs-lookup"><span data-stu-id="86beb-301">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="86beb-302">Veri koruma sisteminde sınırlı bir varsayılan ayarı için destek [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'ları kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="86beb-302">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="86beb-303">Bkz: [veri koruma belgelerine](xref:security/data-protection/index) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="86beb-303">See the [data protection documentation](xref:security/data-protection/index) for details.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="86beb-304">Alt uygulama yapılandırma</span><span class="sxs-lookup"><span data-stu-id="86beb-304">Sub-application configuration</span></span>

<span data-ttu-id="86beb-305">Kök uygulama altında eklenen alt uygulamaları ASP.NET Core modülü işleyici olarak içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="86beb-305">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="86beb-306">Modül bir alt uygulamasının işleyici olarak eklediyseniz *web.config* dosya, bir *500.19 iç sunucu hatası* hatalı yapılandırma dosyası başvuran alındığında alt uygulama Gözat çalışılırken.</span><span class="sxs-lookup"><span data-stu-id="86beb-306">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="86beb-307">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosya bir ASP.NET Core alt-uygulama için:</span><span class="sxs-lookup"><span data-stu-id="86beb-307">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="86beb-308">Bir ASP.NET Core uygulama altında ASP.NET Core alt-app barındırdığında devralınan işleyici alt uygulamada açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="86beb-308">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="86beb-309">ASP.NET Core Modülü'nü yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) konu ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="86beb-309">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="86beb-310">IIS yapılandırma web.config ile</span><span class="sxs-lookup"><span data-stu-id="86beb-310">Configuration of IIS with web.config</span></span>

<span data-ttu-id="86beb-311">IIS yapılandırmasını etkileyen tarafından  **\<system.webServer >** bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="86beb-311">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="86beb-312">IIS sunucu düzeyinde dinamik sıkıştırma kullanmak üzere yapılandırılmışsa,  **\<urlCompression >** uygulamanın öğesinde *web.config* dosya devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86beb-312">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="86beb-313">Daha fazla bilgi için bkz: [yapılandırma başvurusunu \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET temel modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [kullanarak IIS modüllerini ASP. NET çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="86beb-313">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="86beb-314">(Veya sonraki sürümler için IIS 10.0 desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ortam değişkenlerini ayarlama Bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.</span><span class="sxs-lookup"><span data-stu-id="86beb-314">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="86beb-315">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="86beb-315">Configuration sections of web.config</span></span>

<span data-ttu-id="86beb-316">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma ASP.NET Core uygulamaları tarafından kullanılan değil:</span><span class="sxs-lookup"><span data-stu-id="86beb-316">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="86beb-317">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="86beb-317">**\<system.web>**</span></span>
* <span data-ttu-id="86beb-318">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="86beb-318">**\<appSettings>**</span></span>
* <span data-ttu-id="86beb-319">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="86beb-319">**\<connectionStrings>**</span></span>
* <span data-ttu-id="86beb-320">**\<Konum >**</span><span class="sxs-lookup"><span data-stu-id="86beb-320">**\<location>**</span></span>

<span data-ttu-id="86beb-321">ASP.NET Core uygulamaları başka bir yapılandırma sağlayıcısı kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="86beb-321">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="86beb-322">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="86beb-322">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="86beb-323">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="86beb-323">Application Pools</span></span>

<span data-ttu-id="86beb-324">Bir sunucuda birden çok Web sitesi barındırma, kendi uygulama havuzuna her uygulamayı çalıştırarak uygulamaları birbirinden yalıtmak.</span><span class="sxs-lookup"><span data-stu-id="86beb-324">When hosting multiple websites on a server, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="86beb-325">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="86beb-325">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="86beb-326">Zaman **Site adı** metni için otomatik olarak aktarılır sağlanmış **uygulama havuzu** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="86beb-326">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="86beb-327">Yeni bir uygulama havuzu site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="86beb-327">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="86beb-328">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="86beb-328">Application Pool Identity</span></span>

<span data-ttu-id="86beb-329">Bir uygulama havuzu kimliği hesabı oluşturmak ve etki alanı veya yerel hesaplar yönetmek zorunda kalmadan benzersiz bir hesap altında çalıştırmak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="86beb-329">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="86beb-330">IIS 8.0 veya sonraki sürümlerde, IIS Yönetim çalışan işlem (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemi bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="86beb-330">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="86beb-331">IIS yönetim konsolundaki altında **Gelişmiş ayarları** emin olun uygulama havuzu için **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="86beb-331">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="86beb-333">IIS yönetim işlemi güvenli bir tanımlayıcı Windows güvenlik sisteminde uygulama havuzunun adı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="86beb-333">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="86beb-334">Kaynakların bu kimliği kullanılarak güvenli hale getirilebilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-334">Resources can be secured using this identity.</span></span> <span data-ttu-id="86beb-335">Ancak, bu kimlik gerçek kullanıcı hesabı değilse ve Windows kullanıcı yönetim konsolunda göstermez.</span><span class="sxs-lookup"><span data-stu-id="86beb-335">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="86beb-336">IIS çalışan işlemi uygulamaya yükseltilmiş erişimi gerektiriyorsa, uygulamayı içeren dizin için erişim denetim listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="86beb-336">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="86beb-337">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="86beb-337">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="86beb-338">Sağ tıklatın ve directory **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="86beb-338">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="86beb-339">Altında **güvenlik** sekmesine **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="86beb-339">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="86beb-340">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="86beb-340">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="86beb-341">ENTER **IIS AppPool\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alanı.</span><span class="sxs-lookup"><span data-stu-id="86beb-341">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="86beb-342">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="86beb-342">Select the **Check Names** button.</span></span> <span data-ttu-id="86beb-343">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="86beb-343">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="86beb-344">Zaman **Adları Denetle** düğmesi seçilirse, değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="86beb-344">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="86beb-345">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="86beb-345">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="86beb-346">Kullanım **IIS AppPool\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="86beb-346">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Select kullanıcılar veya gruplar iletişim kutusu için uygulama klasörü: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="86beb-348">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="86beb-348">Select **OK**.</span></span>

   ![Select kullanıcılar veya gruplar iletişim kutusu için uygulama klasörü: "Adları denetle" seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="86beb-350">Okuma &amp; Yürütme izinleri varsayılan olarak verilmesi.</span><span class="sxs-lookup"><span data-stu-id="86beb-350">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="86beb-351">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="86beb-351">Provide additional permissions as needed.</span></span>

<span data-ttu-id="86beb-352">Erişim olanağı da verilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="86beb-352">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="86beb-353">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="86beb-353">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="86beb-354">Daha fazla bilgi için bkz: [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="86beb-354">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86beb-355">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="86beb-355">Additional resources</span></span>

* [<span data-ttu-id="86beb-356">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="86beb-356">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="86beb-357">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="86beb-357">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="86beb-358">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="86beb-358">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="86beb-359">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="86beb-359">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="86beb-360">ASP.NET çekirdeği ile IIS modüllerini kullanma</span><span class="sxs-lookup"><span data-stu-id="86beb-360">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="86beb-361">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="86beb-361">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="86beb-362">The Official Microsoft IIS Site</span><span class="sxs-lookup"><span data-stu-id="86beb-362">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="86beb-363">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="86beb-363">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
