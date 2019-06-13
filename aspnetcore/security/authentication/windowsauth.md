---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: Windows kimlik doğrulaması için IIS ve HTTP.sys içinde ASP.NET Core yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034957"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="8361c-103">ASP.NET Core Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8361c-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="8361c-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8361c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8361c-105">Windows kimlik doğrulaması (anlaşma, Kerberos veya NTLM kimlik olarak da bilinir) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), veya [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="8361c-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8361c-106">Windows kimlik doğrulaması (anlaşma, Kerberos veya NTLM kimlik olarak da bilinir) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="8361c-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="8361c-107">Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="8361c-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="8361c-108">Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8361c-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="8361c-109">Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.</span><span class="sxs-lookup"><span data-stu-id="8361c-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="8361c-110">Windows kimlik doğrulaması, HTTP/2 ile desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="8361c-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="8361c-111">Kimlik doğrulama sınaması, HTTP/2 yanıtları gönderilebilir, ancak önce kimlik doğrulaması, istemci HTTP/1.1 düşürme gerekir.</span><span class="sxs-lookup"><span data-stu-id="8361c-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="8361c-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="8361c-112">IIS/IIS Express</span></span>

<span data-ttu-id="8361c-113">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8361c-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="8361c-114">Başlatma ayarları (hata ayıklayıcı)</span><span class="sxs-lookup"><span data-stu-id="8361c-114">Launch settings (debugger)</span></span>

<span data-ttu-id="8361c-115">Başlatma ayarları yapılandırması yalnızca etkiler *Properties/launchSettings.json* dosya için IIS Express ve IIS için Windows kimlik doğrulaması yapılandırmaz.</span><span class="sxs-lookup"><span data-stu-id="8361c-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="8361c-116">Sunucu Yapılandırması içinde açıklanan [IIS](#iis) bölümü.</span><span class="sxs-lookup"><span data-stu-id="8361c-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="8361c-117">**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamasını güncelleştiren destekleyecek şekilde yapılandırılabilir *Properties/launchSettings.json* dosyası otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="8361c-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8361c-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8361c-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8361c-119">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="8361c-119">**New project**</span></span>

1. <span data-ttu-id="8361c-120">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8361c-120">Create a new project.</span></span>
1. <span data-ttu-id="8361c-121">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8361c-122">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="8361c-122">Select **Next**.</span></span>
1. <span data-ttu-id="8361c-123">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="8361c-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="8361c-124">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="8361c-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="8361c-125">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="8361c-125">Select **Create**.</span></span>
1. <span data-ttu-id="8361c-126">Seçin **değişiklik** altında **kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="8361c-127">İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="8361c-128">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="8361c-128">Select **OK**.</span></span>
1. <span data-ttu-id="8361c-129">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="8361c-130">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="8361c-130">Select **Create**.</span></span>

<span data-ttu-id="8361c-131">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8361c-131">Run the app.</span></span> <span data-ttu-id="8361c-132">Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8361c-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="8361c-133">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="8361c-133">**Existing project**</span></span>

<span data-ttu-id="8361c-134">Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmek ve anonim kimlik doğrulamasını devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="8361c-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="8361c-135">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="8361c-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="8361c-136">Seçin **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="8361c-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="8361c-137">Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="8361c-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="8361c-138">Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="8361c-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="8361c-139">Kaydet ve özellik sayfasını kapatın.</span><span class="sxs-lookup"><span data-stu-id="8361c-139">Save and close the property page.</span></span>

<span data-ttu-id="8361c-140">Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8361c-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="8361c-141">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8361c-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="8361c-142">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="8361c-142">**New project**</span></span>

<span data-ttu-id="8361c-143">Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:</span><span class="sxs-lookup"><span data-stu-id="8361c-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="8361c-144">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="8361c-144">**Existing project**</span></span>

<span data-ttu-id="8361c-145">Güncelleştirme `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8361c-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="8361c-146">Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8361c-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="8361c-147">IIS</span><span class="sxs-lookup"><span data-stu-id="8361c-147">IIS</span></span>

<span data-ttu-id="8361c-148">IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="8361c-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="8361c-149">Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="8361c-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="8361c-150">Aşağıdaki bölümlerde show nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="8361c-150">The following sections show how to:</span></span>

* <span data-ttu-id="8361c-151">Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="8361c-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="8361c-152">IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="8361c-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="8361c-153">Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8361c-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="8361c-154">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="8361c-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="8361c-155">Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8361c-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="8361c-156">Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="8361c-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="8361c-157">[IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) otomatik olarak isteklerinin kimliğini doğrulamak için varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="8361c-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="8361c-158">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="8361c-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="8361c-159">ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="8361c-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="8361c-160">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="8361c-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="8361c-161">Kullanım **ya da** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="8361c-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="8361c-162">**Projeyi dağıtma ve yayımlama önce** aşağıdaki *web.config* proje kök dosya:</span><span class="sxs-lookup"><span data-stu-id="8361c-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="8361c-163">Proje .NET Core SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="8361c-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="8361c-164">Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="8361c-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="8361c-165">**Sonra projeyi dağıtma ve yayımlama** sunucu tarafı yapılandırması IIS Yöneticisi ile gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="8361c-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="8361c-166">IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="8361c-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="8361c-167">Çift **kimlik doğrulaması** içinde **IIS** alan.</span><span class="sxs-lookup"><span data-stu-id="8361c-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="8361c-168">Seçin **anonim kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="8361c-169">Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="8361c-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="8361c-170">Seçin **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="8361c-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="8361c-171">Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="8361c-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="8361c-172">Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="8361c-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="8361c-173">A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="8361c-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="8361c-174">`<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü.</span><span class="sxs-lookup"><span data-stu-id="8361c-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="8361c-175">Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama.</span><span class="sxs-lookup"><span data-stu-id="8361c-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="8361c-176">Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` .NET Core SDK'sını sağlanan bölüm.</span><span class="sxs-lookup"><span data-stu-id="8361c-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="8361c-177">IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="8361c-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="8361c-178">Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="8361c-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="8361c-179">Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="8361c-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="8361c-180">Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="8361c-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="8361c-181">Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8361c-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="8361c-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8361c-182">Kestrel</span></span>

 <span data-ttu-id="8361c-183">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet paketi ile kullanılabilir [Kestrel](xref:fundamentals/servers/kestrel) Windows Windows, Linux ve Macos'ta anlaşma, Kerberos ve NTLM kullanarak kimlik doğrulamasını desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="8361c-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="8361c-184">Kimlik bilgileri bağlantı istekleri arasında kalıcı.</span><span class="sxs-lookup"><span data-stu-id="8361c-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="8361c-185">*Anlaşma kimlik doğrulaması değil kullanılmalıdır proxy'leriyle sürece proxy Kestrel ile 1:1 bağlantı benzeşimi (kalıcı bir bağlantı) korur.*</span><span class="sxs-lookup"><span data-stu-id="8361c-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span> <span data-ttu-id="8361c-186">Anlaşma kimlik doğrulamasını Kestrel arkasında IIS ile kullanılmamalıdır, yani [ASP.NET Core Modülü (ANCM) giden işlem](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="8361c-186">This means that Negotiate authentication must not be used with Kestrel behind the IIS [ASP.NET Core Module (ANCM) out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span></span>

 <span data-ttu-id="8361c-187">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` ad alanı) ve `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8361c-187">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="8361c-188">Kimlik doğrulaması ara yazılımı çağırarak ekleme <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8361c-188">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="8361c-189">Ara yazılım hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="8361c-189">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="8361c-190">Anonim isteklere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="8361c-190">Anonymous requests are allowed.</span></span> <span data-ttu-id="8361c-191">Kullanım [ASP.NET Core yetkilendirme](xref:security/authorization/introduction) kimlik doğrulaması için anonim isteklere meydan okuyun.</span><span class="sxs-lookup"><span data-stu-id="8361c-191">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="8361c-192">Windows ortamını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8361c-192">Windows environment configuration</span></span>

<span data-ttu-id="8361c-193">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) bileşeni kullanıcı modu kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="8361c-193">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="8361c-194">Hizmet asıl adları (SPN) makine hesabı değil hizmetini çalıştıran kullanıcı hesabına eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8361c-194">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="8361c-195">Yürütme `setspn -S HTTP/mysrevername.mydomain.com myuser` bir yönetim komut kabuğu'nda.</span><span class="sxs-lookup"><span data-stu-id="8361c-195">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="8361c-196">Linux ve Macos'ta ortamı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8361c-196">Linux and macOS environment configuration</span></span>

<span data-ttu-id="8361c-197">Bir Linux veya Macos'ta makine bir Windows etki alanına katmak için yönergeler kullanılabilir [Azure veri Studio Windows kimlik doğrulaması - Kerberos kullanarak SQL sunucunuza bağlanmak](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) makalesi.</span><span class="sxs-lookup"><span data-stu-id="8361c-197">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="8361c-198">Yönergeleri Linux makinesi için bir makine hesabı etki alanında oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8361c-198">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="8361c-199">Bu makine hesabı için SPN eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8361c-199">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="8361c-200">' Deki yönergeleri takip ederken [Azure veri Studio Windows kimlik doğrulaması - Kerberos kullanarak SQL sunucunuza bağlanmak](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) makalesi, yerine `python-software-properties` ile `python3-software-properties` gerekirse.</span><span class="sxs-lookup"><span data-stu-id="8361c-200">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="8361c-201">Linux veya Macos'ta makine etki alanına katılmış sonra sağlamak için ek adımlar gereklidir bir [anahtar tablosu dosya](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) SPN'ler ile:</span><span class="sxs-lookup"><span data-stu-id="8361c-201">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="8361c-202">Etki alanı denetleyicisinde makine hesabı için yeni bir web hizmeti SPN'ler ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8361c-202">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="8361c-203">Kullanım [ktpass](/windows-server/administration/windows-commands/ktpass) anahtar tablosu dosyası oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="8361c-203">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="8361c-204">Bazı alanları belirtilmelidir gösterildiği gibi büyük.</span><span class="sxs-lookup"><span data-stu-id="8361c-204">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="8361c-205">Anahtar tablosu dosyasını Linux veya Macos'ta makineye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="8361c-205">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="8361c-206">Bir ortam değişkeni aracılığıyla anahtar tablosu dosyayı seçin: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="8361c-206">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="8361c-207">Çağırma `klist` şu anda kullanılabilir SPN'ler gösterilecek.</span><span class="sxs-lookup"><span data-stu-id="8361c-207">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="8361c-208">Bir anahtar tablosu dosyası, etki alanı erişim kimlik bilgileri içeriyor ve uygun şekilde korunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8361c-208">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="8361c-209">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="8361c-209">HTTP.sys</span></span>

<span data-ttu-id="8361c-210">[HTTP.sys](xref:fundamentals/servers/httpsys) çekirdek modu Windows Negotiate, NTLM veya temel kimlik doğrulamasını kullanarak kimlik doğrulaması destekler.</span><span class="sxs-lookup"><span data-stu-id="8361c-210">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="8361c-211">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8361c-211">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="8361c-212">Uygulamanın web ana HTTP.sys Windows kimlik doğrulaması için yapılandırma (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="8361c-212">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="8361c-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> içinde <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="8361c-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="8361c-214">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8361c-214">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="8361c-215">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8361c-215">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="8361c-216">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="8361c-216">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="8361c-217">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8361c-217">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="8361c-218">HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="8361c-218">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="8361c-219">Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="8361c-219">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="8361c-220">Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="8361c-220">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="8361c-221">Kullanıcıları yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="8361c-221">Authorize users</span></span>

<span data-ttu-id="8361c-222">Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8361c-222">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="8361c-223">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="8361c-223">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="8361c-224">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="8361c-224">Disallow anonymous access</span></span>

<span data-ttu-id="8361c-225">Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="8361c-225">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="8361c-226">İstek, hiçbir zaman bir IIS sitesi anonim erişime izin vermeyecek şekilde yapılandırılmışsa, uygulama ulaşır.</span><span class="sxs-lookup"><span data-stu-id="8361c-226">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="8361c-227">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="8361c-227">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="8361c-228">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="8361c-228">Allow anonymous access</span></span>

<span data-ttu-id="8361c-229">Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="8361c-229">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="8361c-230">`[Authorize]` Özniteliği güvenli kimlik doğrulaması gerektiren uygulama uç olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8361c-230">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="8361c-231">`[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamalarında öznitelik.</span><span class="sxs-lookup"><span data-stu-id="8361c-231">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="8361c-232">Öznitelik kullanım ayrıntıları için bkz. <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="8361c-232">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="8361c-233">Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="8361c-233">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="8361c-234">[StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8361c-234">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="8361c-235">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="8361c-235">Impersonation</span></span>

<span data-ttu-id="8361c-236">ASP.NET Core, kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="8361c-236">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="8361c-237">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8361c-237">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="8361c-238">Uygulamanın kullanıcı adına bir eylem gerçekleştirmeniz gereken kullanırsanız [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8361c-238">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="8361c-239">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="8361c-239">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="8361c-240">`RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="8361c-240">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="8361c-241">Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="8361c-241">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8361c-242">Sırada [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) paket, Windows kimlik doğrulamasını etkinleştirir, Linux ve Macos'ta kimliğe bürünme yalnızca Windows üzerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8361c-242">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="8361c-243">Talep dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="8361c-243">Claims transformations</span></span>

<span data-ttu-id="8361c-244">IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="8361c-244">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="8361c-245">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="8361c-245">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="8361c-246">Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="8361c-246">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8361c-247">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8361c-247">Additional resources</span></span>

* [<span data-ttu-id="8361c-248">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="8361c-248">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
