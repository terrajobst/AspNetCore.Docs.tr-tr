---
title: "IIS ile Windows ana ASP.NET Çekirdeği"
author: guardrex
description: "ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde konak öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="6126a-103">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="6126a-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="6126a-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6126a-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="6126a-105">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="6126a-105">Supported operating systems</span></span>

<span data-ttu-id="6126a-106">Aşağıdaki işletim sistemlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="6126a-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="6126a-107">Windows 7 ve daha yeni</span><span class="sxs-lookup"><span data-stu-id="6126a-107">Windows 7 and newer</span></span>
* <span data-ttu-id="6126a-108">Windows Server 2008 R2 ve daha yeni &#8224;</span><span class="sxs-lookup"><span data-stu-id="6126a-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="6126a-109">&#8224; Kavramsal olarak, bu belgede açıklanan IIS yapılandırmasını Nano Server IIS üzerinde ASP.NET Core uygulamaları barındırmak için de geçerlidir, ancak başvurmak [Nano Server IIS ile ASP.NET Core](xref:tutorials/nano-server) özel yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="6126a-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="6126a-110">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)) bir ters proxy yapılandırması IIS ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="6126a-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="6126a-111">Kullanım [Kestrel server](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6126a-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="6126a-112">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="6126a-112">IIS configuration</span></span>

<span data-ttu-id="6126a-113">Etkinleştirme **Web sunucusu (IIS)** rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6126a-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="6126a-114">Windows masaüstü işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="6126a-114">Windows desktop operating systems</span></span>

<span data-ttu-id="6126a-115">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="6126a-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="6126a-116">Grup için açık **Internet Information Services** ve **Web yönetimi araçları**.</span><span class="sxs-lookup"><span data-stu-id="6126a-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="6126a-117">Onay kutusunu için **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="6126a-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="6126a-118">Onay kutusunu için **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="6126a-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="6126a-119">Varsayılan özellikler için kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6126a-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![IIS Yönetim Konsolu ve World Wide Web Hizmetleri içinde Windows özellikleri seçilir.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="6126a-121">Windows Server işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="6126a-121">Windows Server operating systems</span></span>

<span data-ttu-id="6126a-122">Sunucu işletim sistemleri için **rol ve Özellik Ekle** Sihirbazı aracılığıyla **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**.</span><span class="sxs-lookup"><span data-stu-id="6126a-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="6126a-123">Üzerinde **sunucu rolleri** adım, onay kutusunu için **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="6126a-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="6126a-125">Üzerinde **rol hizmetlerini** adım, istenen IIS rol hizmetlerini seçin veya varsayılan rol hizmetlerini sağlanan kabul edin.</span><span class="sxs-lookup"><span data-stu-id="6126a-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

<span data-ttu-id="6126a-127">Üzerinden devam **onay** web sunucusu rolü ve hizmetlerini yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="6126a-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="6126a-128">Web sunucusu (IIS) rolünü yükledikten sonra sunucu/IIS yeniden başlatma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6126a-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="6126a-129">.NET Core Windows Server barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="6126a-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="6126a-130">Yükleme [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting) barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="6126a-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="6126a-131">.NET çekirdeği çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6126a-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="6126a-132">Modül IIS Kestrel sunucusu arasında ters proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6126a-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="6126a-133">Sistem Internet bağlantısı yoksa, edinme ve yükleme [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core Windows Server barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="6126a-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="6126a-134">Sistemi yeniden başlatın veya yürütme **net stop edildi /y** arkasından **net start w3svc** sistem yolu değişiklik seçmek için bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="6126a-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="6126a-135">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="6126a-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="6126a-136">Visual Studio ile yayımlama sırasında Web dağıtımı yükleyin</span><span class="sxs-lookup"><span data-stu-id="6126a-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="6126a-137">Uygulamaları sahip sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6126a-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="6126a-138">Web dağıtımı yüklemek için kullandığınız [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyici elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="6126a-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="6126a-139">Tercih edilen yöntem Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="6126a-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="6126a-140">Webpı, tek başına Kurulum ve barındırma sağlayıcıları için bir yapılandırma sunar.</span><span class="sxs-lookup"><span data-stu-id="6126a-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="6126a-141">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="6126a-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="6126a-142">IISIntegration bileşenleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6126a-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6126a-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6126a-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6126a-144">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlamak için.</span><span class="sxs-lookup"><span data-stu-id="6126a-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="6126a-145">`CreateDefaultBuilder`yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) için bağlantı noktası ve temel yolunu yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="6126a-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6126a-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6126a-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6126a-147">Bir bağımlılık içerir [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıkları paketinde.</span><span class="sxs-lookup"><span data-stu-id="6126a-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="6126a-148">Ekleyerek uygulamayı yazarken IIS tümleştirme ara yazılımı eklemenize *UseIISIntegration* genişletme yöntemi *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="6126a-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="6126a-149">Her ikisi de `UseKestrel` ve `UseIISIntegration` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6126a-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="6126a-150">Kod arama `UseIISIntegration` kod taşınabilirlik etkilemez.</span><span class="sxs-lookup"><span data-stu-id="6126a-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="6126a-151">Uygulama IIS çalıştırırsanız değil (örneğin, uygulama doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` ops yok.</span><span class="sxs-lookup"><span data-stu-id="6126a-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="6126a-152">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="6126a-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="6126a-153">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="6126a-153">IIS options</span></span>

<span data-ttu-id="6126a-154">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil `IISOptions` içinde `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6126a-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="6126a-155">Seçenek</span><span class="sxs-lookup"><span data-stu-id="6126a-155">Option</span></span>                         | <span data-ttu-id="6126a-156">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="6126a-156">Default</span></span> | <span data-ttu-id="6126a-157">Ayar</span><span class="sxs-lookup"><span data-stu-id="6126a-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="6126a-158">Varsa `true`, kimlik doğrulaması ara yazılımı kümeleri `HttpContext.User` ve genel zorluklar yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="6126a-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="6126a-159">Varsa `false`, kimlik doğrulaması ara yazılımı yalnızca bir kimlik sağlar (`HttpContext.User`) ve açıkça tarafından istendiğinde zorluklar yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="6126a-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="6126a-160">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="6126a-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="6126a-161">Oturum açma sayfalarındaki kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6126a-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="6126a-162">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="6126a-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="6126a-163">Web.config</span><span class="sxs-lookup"><span data-stu-id="6126a-163">web.config</span></span>

<span data-ttu-id="6126a-164">*Web.config* dosyanın birincil amaçtır yapılandırmak için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6126a-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="6126a-165">İsteğe bağlı olarak, ek IIS yapılandırması ayarları de sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="6126a-166">Oluşturma, dönüştürme ve yayımlama *web.config* .NET Core Web SDK tarafından yapılır (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="6126a-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="6126a-167">SDK proje dosyasının üst kısmında ayarlanır `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="6126a-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="6126a-168">SDK desenlerin dönüştürülmesini engellemek için *web.config* dosya, ekleme  **\<IsTransformWebConfigDisabled >** ayarı olan proje dosyası özelliğine `true`:</span><span class="sxs-lookup"><span data-stu-id="6126a-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="6126a-169">Varsa bir *web.config* dosyası projeye, doğru ile dönüştürüldükten *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [çıkış yayımlanan](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="6126a-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="6126a-170">Dönüşüm dosyasında IIS yapılandırma ayarlarını değiştirme değil.</span><span class="sxs-lookup"><span data-stu-id="6126a-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="6126a-171">Web.config konumu</span><span class="sxs-lookup"><span data-stu-id="6126a-171">web.config location</span></span>

<span data-ttu-id="6126a-172">.NET core uygulamaları IIS Kestrel sunucusu arasında ters sunucu aracılığıyla barındırılır.</span><span class="sxs-lookup"><span data-stu-id="6126a-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="6126a-173">Ters proxy oluşturmak için *web.config* dosyası için IIS tarafından sağlanan Web sitesi fiziksel yolu dağıtılan uygulamanın (genellikle uygulama temel yol) içerik kök yolundaki mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6126a-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="6126a-174">*Web.config* Web dağıtımı kullanarak birden çok uygulamaları yayımlamayı etkinleştirmek için uygulama kökü dosyası gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6126a-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="6126a-175">Hassas dosyaları var gibi alt klasörler dahil olmak üzere uygulamanın fiziksel yola  *\<assembly_name >. runtimeconfig.json*,  *\<assembly_name > .xml* (XML Belge açıklamaları için) ve  *\<assembly_name >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="6126a-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="6126a-176">Zaman *web.config* dosya varsa ve siteyi yapılandırır, IIS bu hassas dosyalar sunulmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="6126a-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="6126a-177">**Bu nedenle önemlidir, *web.config* dağıtımından kaldırılmış veya yeniden adlandırılmış dosya yanlışlıkla değil.**</span><span class="sxs-lookup"><span data-stu-id="6126a-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="6126a-178">IIS Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6126a-178">Create the IIS Website</span></span>

1. <span data-ttu-id="6126a-179">Hedef IIS sistem, uygulamanın yayımlanan klasörleri ve açıklanan dosyaları içermesi için bir klasör oluşturun [dizin yapısını](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="6126a-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="6126a-180">Klasörü içinde bir *günlükleri* stdout günlük kaydı etkinleştirildiğinde stdout günlükleri tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="6126a-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="6126a-181">Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="6126a-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="6126a-182">MSBuild oluşturma yapma hakkında yönergeler için *günlükleri* klasörü, bkz: [dizin yapısını](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="6126a-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="6126a-183">İçinde **IIS Yöneticisi'ni**, yeni bir Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6126a-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="6126a-184">Sağlayan bir **Site adı** ve **fiziksel yolu** uygulamanın dağıtım klasörü için.</span><span class="sxs-lookup"><span data-stu-id="6126a-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="6126a-185">Sağlamak **bağlama** yapılandırma ve Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6126a-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="6126a-186">Uygulama havuzunda ayarlanan **yönetilen kod yok**.</span><span class="sxs-lookup"><span data-stu-id="6126a-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="6126a-187">ASP.NET Core ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="6126a-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="6126a-188">Açık **Web sitesi Ekle** penceresi.</span><span class="sxs-lookup"><span data-stu-id="6126a-188">Open the **Add Website** window.</span></span>

   !['Web sitesi Ekle' siteler bağlam menüsünden seçin.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="6126a-190">Web sitesini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6126a-190">Configure the website.</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="6126a-192">İçinde **uygulama havuzları** paneli, açık **uygulama havuzunu Düzenle** Web sitesinin uygulama havuzu üzerinde sağ tıklatıp seçerek penceresi **temel ayarlar...**  açılan menüsünden.</span><span class="sxs-lookup"><span data-stu-id="6126a-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Temel ayarlar uygulama havuzunun bağlamsal menüsünden seçin.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="6126a-194">Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.</span><span class="sxs-lookup"><span data-stu-id="6126a-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Yönetilen kod için .NET CLR sürümü ayarlayın.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="6126a-196">Not: Ayarlama **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6126a-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="6126a-197">ASP.NET Core Masaüstü CLR Yükleme kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="6126a-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="6126a-198">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6126a-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="6126a-199">Uygulama havuzu varsayılan kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimliğini doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve gerekli diğer kaynaklarına erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="6126a-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="6126a-200">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="6126a-200">Deploy the app</span></span>

<span data-ttu-id="6126a-201">IIS sistem hedef sunucuda oluşturduğunuz klasöre uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="6126a-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="6126a-202">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="6126a-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="6126a-203">Yayımlanan uygulama dağıtımı için çalışmayan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="6126a-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="6126a-204">Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="6126a-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="6126a-205">Kilitli dosyaları üzerine yazılamıyor.</span><span class="sxs-lookup"><span data-stu-id="6126a-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="6126a-206">Bir dağıtımı kilitli dosyalarında serbest bırakmak için uygulama havuzunu durdur:</span><span class="sxs-lookup"><span data-stu-id="6126a-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="6126a-207">El ile IIS Yöneticisi'nde sunucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="6126a-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="6126a-208">Web dağıtımı kullanarak ve başvuran `Microsoft.NET.Sdk.Web` proje dosyasında.</span><span class="sxs-lookup"><span data-stu-id="6126a-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="6126a-209">Bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="6126a-210">Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="6126a-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="6126a-211">Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="6126a-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="6126a-212">Durdurup (PowerShell 5 veya sonraki sürümünü gerektirir) uygulama havuzu yeniden başlatmak için PowerShell kullanın:</span><span class="sxs-lookup"><span data-stu-id="6126a-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
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

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="6126a-213">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="6126a-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="6126a-214">Bkz: [ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="6126a-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="6126a-215">Barındırma sağlayıcısı, bir yayımlama profili veya bir oluşturma desteği sağlarsa, kendi profili indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="6126a-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımla iletişim sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="6126a-217">Web dağıtımı dışında Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6126a-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="6126a-218">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dışında Visual Studio komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="6126a-219">Daha fazla bilgi için bkz: [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="6126a-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="6126a-220">Web alternatifleri dağıtma</span><span class="sxs-lookup"><span data-stu-id="6126a-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="6126a-221">Uygulama barındırma sisteminin Xcopy, Robocopy veya PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6126a-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="6126a-222">Visual Studio kullanıcılar kullanabilir [yayımlama örnekleri](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="6126a-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="6126a-223">Web sitesi Gözat</span><span class="sxs-lookup"><span data-stu-id="6126a-223">Browse the website</span></span>

![Microsoft Edge tarayıcısının IIS başlangıç sayfası yükledi.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="6126a-225">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="6126a-225">Data protection</span></span>

<span data-ttu-id="6126a-226">Veri koruma, kimlik doğrulamasında kullanılan dahil olmak üzere birkaç ASP.NET middlewares tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6126a-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="6126a-227">Veri Koruma API'ları kullanıcının koddan çağrılan olmayan olsa bile, veri koruma kalıcı bir anahtar deposu oluşturmak için kullanıcı kodu veya bir dağıtım komut dosyası ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6126a-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="6126a-228">Veri koruma yapılandırılmamışsa, anahtarları bellekte tutulması ve uygulama yeniden başlatıldığında atılır.</span><span class="sxs-lookup"><span data-stu-id="6126a-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="6126a-229">Uygulama yeniden başlatıldığında keyring bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="6126a-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="6126a-230">Tüm form kimlik doğrulama belirteçleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="6126a-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="6126a-231">Kullanıcıların kendi bir sonraki istekte yeniden oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6126a-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="6126a-232">Keyring ile korunan tüm verileri artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="6126a-233">IIS altında veri korumayı yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan:</span><span class="sxs-lookup"><span data-stu-id="6126a-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="6126a-234">Bir veri koruma kayıt defteri kovanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6126a-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="6126a-235">ASP.NET uygulamaları tarafından kullanılan veri koruma anahtarları kayıt defteri kovanları için uygulamaları dış depolanır.</span><span class="sxs-lookup"><span data-stu-id="6126a-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="6126a-236">Belirli bir uygulamanın tuşları kalıcı hale getirmek için uygulama havuzu için bir kayıt defteri kovanını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6126a-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="6126a-237">Tek başına, webfarm olmayan IIS yüklemeleri için [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) bir ASP.NET Core uygulamayla kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="6126a-238">Bu komut dosyası, kayıt defterinde yalnızca çalışan işlem hesabı ACLed olan HKLM özel kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6126a-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="6126a-239">Anahtarları bir makineye özel anahtarla DPAPI kullanarak REST şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="6126a-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="6126a-240">Web grubu senaryolarda, bir uygulama, veri koruma keyring depolamak için bir UNC yolu kullanmak üzere yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="6126a-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="6126a-241">Varsayılan olarak, veri koruma anahtarları şifrelenmez.</span><span class="sxs-lookup"><span data-stu-id="6126a-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="6126a-242">Bu tür bir paylaşım dosya izinlerini uygulama olarak çalışan Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6126a-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="6126a-243">Ayrıca, sertifika REST anahtarlarınızı korumak için kullanılan bir X509.</span><span class="sxs-lookup"><span data-stu-id="6126a-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="6126a-244">Sertifikaları karşıya yüklemesine izin vermek için bir mekanizma göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="6126a-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="6126a-245">Bkz: [yapılandırma veri koruması](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="6126a-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="6126a-246">Kullanıcı profilini yüklemek için IIS uygulama havuzu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6126a-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="6126a-247">Bu ayar **işlem modeli** altında bölümünde **Gelişmiş ayarları** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="6126a-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="6126a-248">Kümesine kullanıcı profilini Yükle `True`.</span><span class="sxs-lookup"><span data-stu-id="6126a-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="6126a-249">Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI uygulama havuzu için kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.</span><span class="sxs-lookup"><span data-stu-id="6126a-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="6126a-250">Dosya sistemi anahtar halkası deposu olarak kullanın</span><span class="sxs-lookup"><span data-stu-id="6126a-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="6126a-251">Uygulama kodu ayarlamak [bir anahtar halkası deposu olarak dosya sistemini kullanmak](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="6126a-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="6126a-252">Kullanım X509 bir sertifika keyring korumak ve sertifikanın güvenilir bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6126a-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="6126a-253">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilir kök deposuna yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="6126a-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="6126a-254">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="6126a-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="6126a-255">Tüm makineler erişebileceği bir dosya paylaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="6126a-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="6126a-256">Bir X509 dağıtmak her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="6126a-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="6126a-257">Yapılandırma [veri koruması kodda](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="6126a-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="6126a-258">Veri koruması için bir makine genelinde İlkesi ayarlama</span><span class="sxs-lookup"><span data-stu-id="6126a-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="6126a-259">Veri koruma sisteminde sınırlı bir varsayılan ayarı için destek [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'ları kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="6126a-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="6126a-260">Bkz: [veri koruması](xref:security/data-protection/index) daha fazla ayrıntı için belgeleri.</span><span class="sxs-lookup"><span data-stu-id="6126a-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="6126a-261">Alt uygulama yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6126a-261">Configuration of sub-applications</span></span>

<span data-ttu-id="6126a-262">Kök uygulama altında eklenen alt uygulamaları ASP.NET Core modülü işleyici olarak içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="6126a-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="6126a-263">Modül bir alt uygulamasının işleyici olarak eklediyseniz *web.config* dosya, bir 500.19 (Dahili Sunucu hatası) başvuran alt uygulama Gözat çalışılırken hatalı yapılandırma dosyası aldı.</span><span class="sxs-lookup"><span data-stu-id="6126a-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="6126a-264">Aşağıdaki örnek, bir yayımlanan içeriğini gösterir *web.config* dosya bir ASP.NET Core alt-uygulama için:</span><span class="sxs-lookup"><span data-stu-id="6126a-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="6126a-265">Bir ASP.NET Core uygulama altında ASP.NET Core alt-app barındırdığında devralınan işleyici alt uygulamada açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6126a-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="6126a-266">ASP.NET Core Modülü'nü yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) konu ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6126a-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="6126a-267">IIS yapılandırma web.config ile</span><span class="sxs-lookup"><span data-stu-id="6126a-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="6126a-268">IIS yapılandırmasını etkileyen tarafından  **\<system.webServer >** bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="6126a-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="6126a-269">IIS dinamik sıkıştırma kullanmak için sistem düzeyinde yapılandırdıysanız, bu ayarı ile bir uygulama için devre dışı bırakılabilir  **\<urlCompression >** uygulamanın öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="6126a-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="6126a-270">Daha fazla bilgi için bkz: [yapılandırma başvurusunu \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET temel modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module) ve [kullanarak IIS modüllerini ASP. NET çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="6126a-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="6126a-271">Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bir gereksinimi varsa bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.</span><span class="sxs-lookup"><span data-stu-id="6126a-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="6126a-272">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="6126a-272">Configuration sections of web.config</span></span>

<span data-ttu-id="6126a-273">ASP.NET Framework uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma ASP.NET Core uygulamaları tarafından kullanılan değil:</span><span class="sxs-lookup"><span data-stu-id="6126a-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="6126a-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="6126a-274">**\<system.web>**</span></span>
* <span data-ttu-id="6126a-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="6126a-275">**\<appSettings>**</span></span>
* <span data-ttu-id="6126a-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="6126a-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="6126a-277">**\<Konum >**</span><span class="sxs-lookup"><span data-stu-id="6126a-277">**\<location>**</span></span>

<span data-ttu-id="6126a-278">ASP.NET Core uygulamaları başka bir yapılandırma sağlayıcısı kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6126a-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="6126a-279">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6126a-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="6126a-280">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="6126a-280">Application Pools</span></span>

<span data-ttu-id="6126a-281">Tek bir sistemde birden çok Web sitesi barındırma, kendi uygulama havuzuna her uygulamayı çalıştırarak uygulamaları birbirinden yalıtmak.</span><span class="sxs-lookup"><span data-stu-id="6126a-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="6126a-282">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="6126a-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="6126a-283">Zaman **Site adı** metni için otomatik olarak aktarılır sağlanmış **uygulama havuzu** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="6126a-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="6126a-284">Yeni bir uygulama havuzu, Web sitesi eklendiğinde, site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6126a-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="6126a-285">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="6126a-285">Application Pool Identity</span></span>

<span data-ttu-id="6126a-286">Bir uygulama havuzu kimliği hesabı oluşturmak ve etki alanı veya yerel hesaplar yönetmek zorunda kalmadan benzersiz bir hesap altında çalıştırmak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="6126a-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="6126a-287">IIS 8.0 +, IIS Yönetici çalışan işlem (WAS), yeni uygulama havuzu adı ile sanal hesap oluşturur ve uygulama havuzunun çalışan işlemi bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6126a-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="6126a-288">IIS yönetim konsolundaki altında **Gelişmiş ayarları** emin olun uygulama havuzu için **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="6126a-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="6126a-290">IIS yönetim işlemi güvenli bir tanımlayıcı Windows güvenlik sisteminde uygulama havuzunun adı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6126a-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="6126a-291">Kaynakların bu kimliği kullanılarak güvenli hale; Ancak, bu kimlik gerçek kullanıcı hesabı değilse ve Windows kullanıcı Yönetim Konsolu'nda görünmez.</span><span class="sxs-lookup"><span data-stu-id="6126a-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="6126a-292">IIS çalışan işlemi uygulamaya yükseltilmiş erişimi gerektiriyorsa, uygulamayı içeren dizin için erişim denetim listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6126a-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="6126a-293">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="6126a-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="6126a-294">Sağ tıklatın ve directory **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="6126a-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="6126a-295">Altında **güvenlik** sekmesine **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="6126a-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="6126a-296">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="6126a-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="6126a-297">ENTER **IIS AppPool\DefaultAppPool** içinde **Seçilecek nesne adlarını girin** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="6126a-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü seçin](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="6126a-299">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="6126a-299">Select the **Check Names** button.</span></span> <span data-ttu-id="6126a-300">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6126a-300">Select **OK**.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü seçin](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="6126a-302">Bu da bir komut istemini kullanarak aracılığıyla gerçekleştirilebilir **ICACLS** aracı:</span><span class="sxs-lookup"><span data-stu-id="6126a-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="6126a-303">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6126a-303">Additional resources</span></span>

* [<span data-ttu-id="6126a-304">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="6126a-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="6126a-305">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="6126a-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="6126a-306">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="6126a-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="6126a-307">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="6126a-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="6126a-308">ASP.NET çekirdeği ile IIS modüllerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6126a-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="6126a-309">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="6126a-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="6126a-310">The Official Microsoft IIS Site</span><span class="sxs-lookup"><span data-stu-id="6126a-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="6126a-311">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="6126a-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
