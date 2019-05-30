---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: Windows kimlik doğrulaması için IIS ve HTTP.sys içinde ASP.NET Core yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/29/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9dfff5dcba409ddca7e05c771b864ab121e0ea85
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395923"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="08a36-103">ASP.NET Core Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="08a36-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="08a36-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="08a36-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="08a36-105">[Windows kimlik doğrulaması](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="08a36-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="08a36-106">Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="08a36-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="08a36-107">Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="08a36-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="08a36-108">Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.</span><span class="sxs-lookup"><span data-stu-id="08a36-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="launch-settings-debugger"></a><span data-ttu-id="08a36-109">Başlatma ayarları (hata ayıklayıcı)</span><span class="sxs-lookup"><span data-stu-id="08a36-109">Launch settings (debugger)</span></span>

<span data-ttu-id="08a36-110">Başlatma ayarları yapılandırması yalnızca etkiler *Properties/launchSettings.json* dosyası ve Windows kimlik doğrulaması için IIS veya HTTP.sys sunucu yapılandırmaz.</span><span class="sxs-lookup"><span data-stu-id="08a36-110">Configuration for launch settings only affects the *Properties/launchSettings.json* file and doesn't configure the IIS or HTTP.sys server for Windows Authentication.</span></span> <span data-ttu-id="08a36-111">Yapılandırma sunucusunun içinde açıklanan [etkinleştirmek için IIS veya HTTP.sys kimlik doğrulama hizmetleri](#authentication-services-for-iis-or-httpsys) bölümü.</span><span class="sxs-lookup"><span data-stu-id="08a36-111">Configuration of the server is explained in the [Enable authentication services for IIS or HTTP.sys](#authentication-services-for-iis-or-httpsys) section.</span></span>

<span data-ttu-id="08a36-112">**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamasını güncelleştiren destekleyecek şekilde yapılandırılabilir *Properties/launchSettings.json* dosyası otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="08a36-112">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08a36-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08a36-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="08a36-114">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="08a36-114">**New project**</span></span>

1. <span data-ttu-id="08a36-115">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="08a36-115">Create a new project.</span></span>
1. <span data-ttu-id="08a36-116">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="08a36-117">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="08a36-117">Select **Next**.</span></span>
1. <span data-ttu-id="08a36-118">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="08a36-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="08a36-119">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="08a36-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="08a36-120">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="08a36-120">Select **Create**.</span></span>
1. <span data-ttu-id="08a36-121">Seçin **değişiklik** altında **kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-121">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="08a36-122">İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-122">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="08a36-123">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="08a36-123">Select **OK**.</span></span>
1. <span data-ttu-id="08a36-124">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-124">Select **Web Application**.</span></span>
1. <span data-ttu-id="08a36-125">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="08a36-125">Select **Create**.</span></span>

<span data-ttu-id="08a36-126">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="08a36-126">Run the app.</span></span> <span data-ttu-id="08a36-127">Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="08a36-127">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="08a36-128">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="08a36-128">**Existing project**</span></span>

<span data-ttu-id="08a36-129">Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmek ve anonim kimlik doğrulamasını devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="08a36-129">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="08a36-130">Projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="08a36-130">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="08a36-131">Seçin **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="08a36-131">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="08a36-132">Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="08a36-132">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="08a36-133">Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="08a36-133">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="08a36-134">Kaydet ve özellik sayfasını kapatın.</span><span class="sxs-lookup"><span data-stu-id="08a36-134">Save and close the property page.</span></span>

<span data-ttu-id="08a36-135">Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="08a36-135">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="08a36-136">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="08a36-136">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="08a36-137">**Yeni Proje**</span><span class="sxs-lookup"><span data-stu-id="08a36-137">**New project**</span></span>

<span data-ttu-id="08a36-138">Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:</span><span class="sxs-lookup"><span data-stu-id="08a36-138">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="08a36-139">**Mevcut proje**</span><span class="sxs-lookup"><span data-stu-id="08a36-139">**Existing project**</span></span>

<span data-ttu-id="08a36-140">Güncelleştirme `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="08a36-140">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="08a36-141">Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="08a36-141">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="authentication-services-for-iis-or-httpsys"></a><span data-ttu-id="08a36-142">IIS veya HTTP.sys için kimlik doğrulama hizmetleri</span><span class="sxs-lookup"><span data-stu-id="08a36-142">Authentication services for IIS or HTTP.sys</span></span>

<span data-ttu-id="08a36-143">Barındırma senaryoya bağlı olarak sunulan yönergeleri **ya da** [IIS](#iis) bölümü **veya** [HTTP.sys](#httpsys) bölümü.</span><span class="sxs-lookup"><span data-stu-id="08a36-143">Depending on the hosting scenario, follow the guidance in **either** the [IIS](#iis) section **or** [HTTP.sys](#httpsys) section.</span></span>

### <a name="iis"></a><span data-ttu-id="08a36-144">IIS</span><span class="sxs-lookup"><span data-stu-id="08a36-144">IIS</span></span>

<span data-ttu-id="08a36-145">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="08a36-145">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="08a36-146">IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="08a36-146">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="08a36-147">Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="08a36-147">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="08a36-148">Aşağıdaki bölümlerde show nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="08a36-148">The following sections show how to:</span></span>

* <span data-ttu-id="08a36-149">Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="08a36-149">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="08a36-150">IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="08a36-150">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="08a36-151">Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="08a36-151">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="08a36-152">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="08a36-152">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="08a36-153">Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="08a36-153">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="08a36-154">Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="08a36-154">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="08a36-155">[IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) otomatik olarak isteklerinin kimliğini doğrulamak için varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="08a36-155">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="08a36-156">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="08a36-156">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="08a36-157">ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="08a36-157">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="08a36-158">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="08a36-158">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="08a36-159">Kullanım **ya da** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="08a36-159">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="08a36-160">**Projeyi dağıtma ve yayımlama önce** aşağıdaki *web.config* proje kök dosya:</span><span class="sxs-lookup"><span data-stu-id="08a36-160">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="08a36-161">Proje .NET Core SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="08a36-161">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="08a36-162">Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="08a36-162">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="08a36-163">**Sonra projeyi dağıtma ve yayımlama** sunucu tarafı yapılandırması IIS Yöneticisi ile gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="08a36-163">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="08a36-164">IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="08a36-164">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="08a36-165">Çift **kimlik doğrulaması** içinde **IIS** alan.</span><span class="sxs-lookup"><span data-stu-id="08a36-165">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="08a36-166">Seçin **anonim kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-166">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="08a36-167">Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="08a36-167">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="08a36-168">Seçin **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="08a36-168">Select **Windows Authentication**.</span></span> <span data-ttu-id="08a36-169">Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="08a36-169">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="08a36-170">Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="08a36-170">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="08a36-171">A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="08a36-171">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="08a36-172">`<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü.</span><span class="sxs-lookup"><span data-stu-id="08a36-172">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="08a36-173">Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama.</span><span class="sxs-lookup"><span data-stu-id="08a36-173">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="08a36-174">Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` .NET Core SDK'sını sağlanan bölüm.</span><span class="sxs-lookup"><span data-stu-id="08a36-174">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="08a36-175">IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="08a36-175">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="08a36-176">Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="08a36-176">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="08a36-177">Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="08a36-177">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="08a36-178">Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="08a36-178">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="08a36-179">Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="08a36-179">Add a *web.config file* to the app locally with the settings.</span></span>

### <a name="httpsys"></a><span data-ttu-id="08a36-180">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="08a36-180">HTTP.sys</span></span>

<span data-ttu-id="08a36-181">Ancak [Kestrel](xref:fundamentals/servers/kestrel) Windows kimlik doğrulamasını desteklemiyor kullanabileceğiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="08a36-181">Although [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span>

<span data-ttu-id="08a36-182">Kimlik doğrulama hizmetleri çağırarak ekleme <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="08a36-182">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="08a36-183">Uygulamanın web ana HTTP.sys Windows kimlik doğrulaması için yapılandırma (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="08a36-183">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="08a36-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> içinde <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="08a36-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="08a36-185">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="08a36-185">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="08a36-186">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="08a36-186">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="08a36-187">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="08a36-187">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="08a36-188">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="08a36-188">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="08a36-189">HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="08a36-189">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="08a36-190">Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="08a36-190">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="08a36-191">Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="08a36-191">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="08a36-192">Kullanıcıları yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="08a36-192">Authorize users</span></span>

<span data-ttu-id="08a36-193">Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="08a36-193">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="08a36-194">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="08a36-194">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="08a36-195">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="08a36-195">Disallow anonymous access</span></span>

<span data-ttu-id="08a36-196">Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="08a36-196">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="08a36-197">İstek, hiçbir zaman bir IIS sitesi anonim erişime izin vermeyecek şekilde yapılandırılmışsa, uygulama ulaşır.</span><span class="sxs-lookup"><span data-stu-id="08a36-197">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="08a36-198">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="08a36-198">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="08a36-199">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="08a36-199">Allow anonymous access</span></span>

<span data-ttu-id="08a36-200">Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="08a36-200">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="08a36-201">`[Authorize]` Özniteliği güvenli kimlik doğrulaması gerektiren uygulama uç olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="08a36-201">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="08a36-202">`[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamalarında öznitelik.</span><span class="sxs-lookup"><span data-stu-id="08a36-202">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="08a36-203">Öznitelik kullanım ayrıntıları için bkz. <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="08a36-203">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="08a36-204">Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="08a36-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="08a36-205">[StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="08a36-205">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="08a36-206">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="08a36-206">Impersonation</span></span>

<span data-ttu-id="08a36-207">ASP.NET Core, kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="08a36-207">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="08a36-208">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="08a36-208">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="08a36-209">Uygulamanın kullanıcı adına bir eylem gerçekleştirmeniz gereken kullanırsanız [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="08a36-209">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="08a36-210">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="08a36-210">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="08a36-211">`RunImpersonated` zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="08a36-211">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="08a36-212">Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="08a36-212">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="08a36-213">Talep dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="08a36-213">Claims transformations</span></span>

<span data-ttu-id="08a36-214">IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="08a36-214">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="08a36-215">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="08a36-215">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="08a36-216">Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="08a36-216">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08a36-217">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="08a36-217">Additional resources</span></span>

* [<span data-ttu-id="08a36-218">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="08a36-218">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
