---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: Windows kimlik doğrulaması için IIS ve HTTP.sys içinde ASP.NET Core yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 900bbf5f14b1876ad537b2b77e4ba07d7aa168f2
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750159"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="6fe51-103">ASP.NET Core Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6fe51-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="6fe51-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6fe51-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6fe51-105">[Windows kimlik doğrulaması](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6fe51-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6fe51-106">Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="6fe51-107">Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fe51-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="6fe51-108">Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="6fe51-109">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="6fe51-109">IIS/IIS Express</span></span>

<span data-ttu-id="6fe51-110">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6fe51-110">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="6fe51-111">Başlatma ayarları (hata ayıklayıcı)</span><span class="sxs-lookup"><span data-stu-id="6fe51-111">Launch settings (debugger)</span></span>

<span data-ttu-id="6fe51-112">Başlatma ayarları yapılandırması yalnızca etkiler *Properties/launchSettings.json* dosya için IIS Express ve IIS için Windows kimlik doğrulaması yapılandırmaz.</span><span class="sxs-lookup"><span data-stu-id="6fe51-112">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="6fe51-113">Sunucu Yapılandırması içinde açıklanan [IIS](#iis) bölümü.</span><span class="sxs-lookup"><span data-stu-id="6fe51-113">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="6fe51-114">**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamasını güncelleştiren destekleyecek şekilde yapılandırılabilir *Properties/launchSettings.json* dosyası otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="6fe51-114">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fe51-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fe51-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fe51-116">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="6fe51-116">**New project**</span></span>

1. <span data-ttu-id="6fe51-117">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6fe51-117">Create a new project.</span></span>
1. <span data-ttu-id="6fe51-118">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6fe51-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-119">Select **Next**.</span></span>
1. <span data-ttu-id="6fe51-120">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="6fe51-120">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="6fe51-121">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="6fe51-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6fe51-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-122">Select **Create**.</span></span>
1. <span data-ttu-id="6fe51-123">Seçin **değişiklik** altında **kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-123">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="6fe51-124">İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-124">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="6fe51-125">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-125">Select **OK**.</span></span>
1. <span data-ttu-id="6fe51-126">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-126">Select **Web Application**.</span></span>
1. <span data-ttu-id="6fe51-127">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-127">Select **Create**.</span></span>

<span data-ttu-id="6fe51-128">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6fe51-128">Run the app.</span></span> <span data-ttu-id="6fe51-129">Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-129">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="6fe51-130">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="6fe51-130">**Existing project**</span></span>

<span data-ttu-id="6fe51-131">Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmek ve anonim kimlik doğrulamasını devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="6fe51-131">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="6fe51-132">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-132">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="6fe51-133">Seçin **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="6fe51-133">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="6fe51-134">Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-134">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="6fe51-135">Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-135">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="6fe51-136">Kaydet ve özellik sayfasını kapatın.</span><span class="sxs-lookup"><span data-stu-id="6fe51-136">Save and close the property page.</span></span>

<span data-ttu-id="6fe51-137">Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6fe51-137">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="6fe51-138">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6fe51-138">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="6fe51-139">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="6fe51-139">**New project**</span></span>

<span data-ttu-id="6fe51-140">Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:</span><span class="sxs-lookup"><span data-stu-id="6fe51-140">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="6fe51-141">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="6fe51-141">**Existing project**</span></span>

<span data-ttu-id="6fe51-142">Güncelleştirme `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6fe51-142">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="6fe51-143">Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="6fe51-143">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="6fe51-144">IIS</span><span class="sxs-lookup"><span data-stu-id="6fe51-144">IIS</span></span>

<span data-ttu-id="6fe51-145">IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="6fe51-145">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="6fe51-146">Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="6fe51-146">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="6fe51-147">Aşağıdaki bölümlerde show nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="6fe51-147">The following sections show how to:</span></span>

* <span data-ttu-id="6fe51-148">Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-148">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="6fe51-149">IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="6fe51-149">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="6fe51-150">Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-150">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="6fe51-151">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="6fe51-151">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="6fe51-152">Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-152">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="6fe51-153">Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="6fe51-153">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="6fe51-154">[IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) otomatik olarak isteklerinin kimliğini doğrulamak için varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-154">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="6fe51-155">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="6fe51-155">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="6fe51-156">ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-156">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="6fe51-157">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="6fe51-157">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="6fe51-158">Kullanım **ya da** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="6fe51-158">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="6fe51-159">**Projeyi dağıtma ve yayımlama önce** aşağıdaki *web.config* proje kök dosya:</span><span class="sxs-lookup"><span data-stu-id="6fe51-159">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="6fe51-160">Proje .NET Core SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="6fe51-160">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="6fe51-161">Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="6fe51-161">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="6fe51-162">**Sonra projeyi dağıtma ve yayımlama** sunucu tarafı yapılandırması IIS Yöneticisi ile gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="6fe51-162">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="6fe51-163">IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="6fe51-163">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="6fe51-164">Çift **kimlik doğrulaması** içinde **IIS** alan.</span><span class="sxs-lookup"><span data-stu-id="6fe51-164">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="6fe51-165">Seçin **anonim kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-165">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="6fe51-166">Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="6fe51-166">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="6fe51-167">Seçin **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="6fe51-167">Select **Windows Authentication**.</span></span> <span data-ttu-id="6fe51-168">Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="6fe51-168">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="6fe51-169">Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="6fe51-169">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="6fe51-170">A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="6fe51-170">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="6fe51-171">`<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü.</span><span class="sxs-lookup"><span data-stu-id="6fe51-171">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="6fe51-172">Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama.</span><span class="sxs-lookup"><span data-stu-id="6fe51-172">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="6fe51-173">Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` .NET Core SDK'sını sağlanan bölüm.</span><span class="sxs-lookup"><span data-stu-id="6fe51-173">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="6fe51-174">IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="6fe51-174">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="6fe51-175">Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="6fe51-175">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="6fe51-176">Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="6fe51-176">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="6fe51-177">Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="6fe51-177">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="6fe51-178">Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6fe51-178">Add a *web.config file* to the app locally with the settings.</span></span>

## <a name="httpsys"></a><span data-ttu-id="6fe51-179">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6fe51-179">HTTP.sys</span></span>

<span data-ttu-id="6fe51-180">Şirket içinde barındırılan senaryolarda [Kestrel](xref:fundamentals/servers/kestrel) kullanabilirsiniz değil Windows kimlik doğrulaması desteği, ancak [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6fe51-180">In self-hosted scenarios, [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, but you can use [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6fe51-181">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6fe51-181">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="6fe51-182">Uygulamanın web ana HTTP.sys Windows kimlik doğrulaması için yapılandırma (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="6fe51-182">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="6fe51-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> içinde <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="6fe51-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6fe51-184">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6fe51-184">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="6fe51-185">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="6fe51-185">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="6fe51-186">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-186">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="6fe51-187">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6fe51-187">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="6fe51-188">HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-188">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="6fe51-189">Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="6fe51-189">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="6fe51-190">Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="6fe51-190">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="6fe51-191">Kullanıcıları yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6fe51-191">Authorize users</span></span>

<span data-ttu-id="6fe51-192">Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-192">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="6fe51-193">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="6fe51-193">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="6fe51-194">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="6fe51-194">Disallow anonymous access</span></span>

<span data-ttu-id="6fe51-195">Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6fe51-195">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="6fe51-196">İstek, hiçbir zaman bir IIS sitesi anonim erişime izin vermeyecek şekilde yapılandırılmışsa, uygulama ulaşır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-196">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="6fe51-197">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="6fe51-197">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="6fe51-198">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="6fe51-198">Allow anonymous access</span></span>

<span data-ttu-id="6fe51-199">Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="6fe51-199">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="6fe51-200">`[Authorize]` Özniteliği güvenli kimlik doğrulaması gerektiren uygulama uç olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-200">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="6fe51-201">`[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamalarında öznitelik.</span><span class="sxs-lookup"><span data-stu-id="6fe51-201">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="6fe51-202">Öznitelik kullanım ayrıntıları için bkz. <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="6fe51-202">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="6fe51-203">Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="6fe51-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="6fe51-204">[StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="6fe51-204">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="6fe51-205">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="6fe51-205">Impersonation</span></span>

<span data-ttu-id="6fe51-206">ASP.NET Core, kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="6fe51-206">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="6fe51-207">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6fe51-207">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="6fe51-208">Uygulamanın kullanıcı adına bir eylem gerçekleştirmeniz gereken kullanırsanız [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6fe51-208">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="6fe51-209">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="6fe51-209">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="6fe51-210">`RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fe51-210">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="6fe51-211">Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="6fe51-211">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="6fe51-212">Talep dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="6fe51-212">Claims transformations</span></span>

<span data-ttu-id="6fe51-213">IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="6fe51-213">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6fe51-214">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="6fe51-214">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6fe51-215">Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="6fe51-215">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fe51-216">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6fe51-216">Additional resources</span></span>

* [<span data-ttu-id="6fe51-217">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="6fe51-217">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
