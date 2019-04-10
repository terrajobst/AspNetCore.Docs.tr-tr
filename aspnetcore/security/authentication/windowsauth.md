---
title: ASP.NET Core Windows kimlik doğrulamasını yapılandırma
author: scottaddie
description: IIS Express, IIS ve HTTP.sys kullanarak ASP.NET Core Windows kimlik doğrulaması yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468659"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="38b38-103">ASP.NET Core Windows kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="38b38-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="38b38-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="38b38-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="38b38-105">[Windows kimlik doğrulaması](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ile barındırılan ASP.NET Core uygulamaları için yapılandırılabilir [IIS](xref:host-and-deploy/iis/index) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="38b38-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="38b38-106">Windows kimlik doğrulaması, ASP.NET Core uygulamaları, kullanıcıların kimliklerini doğrulamak için işletim sistemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="38b38-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="38b38-107">Sunucunuz, kullanıcıları tanımlamak için Active Directory etki alanı kimlikleri veya Windows hesaplarını kullanarak bir şirket ağında çalıştığında, Windows kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38b38-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="38b38-108">Windows kimlik doğrulaması nerede kullanıcılar, istemci uygulamaları ve web sunucuları aynı Windows etki alanına ait intranet ortamları için idealdir.</span><span class="sxs-lookup"><span data-stu-id="38b38-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="38b38-109">ASP.NET Core uygulaması Windows kimlik doğrulamasını etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="38b38-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="38b38-110">**Web uygulaması** şablonu Visual Studio veya .NET Core CLI aracılığıyla kullanılabilir, Windows kimlik doğrulamayı destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="38b38-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="38b38-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="38b38-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="38b38-112">Yeni bir proje için Windows kimlik doğrulama uygulaması şablonunu kullanma</span><span class="sxs-lookup"><span data-stu-id="38b38-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="38b38-113">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="38b38-113">In Visual Studio:</span></span>

1. <span data-ttu-id="38b38-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="38b38-114">Create a new project.</span></span>
1. <span data-ttu-id="38b38-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="38b38-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="38b38-116">Select **Next**.</span></span>
1. <span data-ttu-id="38b38-117">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="38b38-117">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="38b38-118">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="38b38-118">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="38b38-119">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="38b38-119">Select **Create**.</span></span>
1. <span data-ttu-id="38b38-120">Seçin **değişiklik** altında **kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-120">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="38b38-121">İçinde **kimlik doğrulamayı Değiştir** penceresinde **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-121">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="38b38-122">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="38b38-122">Select **OK**.</span></span>
1. <span data-ttu-id="38b38-123">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-123">Select **Web Application**.</span></span>
1. <span data-ttu-id="38b38-124">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="38b38-124">Select **Create**.</span></span>

<span data-ttu-id="38b38-125">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="38b38-125">Run the app.</span></span> <span data-ttu-id="38b38-126">Kullanıcı işlenen uygulamanın kullanıcı arabiriminde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="38b38-126">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="38b38-127">Var olan bir proje için el ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="38b38-127">Manual configuration for an existing project</span></span>

<span data-ttu-id="38b38-128">Projenin özelliklerini Windows kimlik doğrulamasını etkinleştirmenizi ve anonim kimlik doğrulamasını devre dışı izin ver:</span><span class="sxs-lookup"><span data-stu-id="38b38-128">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="38b38-129">Visual Studio'nun'nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="38b38-129">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="38b38-130">Seçin **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="38b38-130">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="38b38-131">Onay kutusunu temizleyin **anonim kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="38b38-131">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="38b38-132">Onay kutusunu seçin **Windows kimlik doğrulamasını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="38b38-132">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="38b38-133">Alternatif olarak, özellikler, yapılandırılabilir `iisSettings` düğümünün *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="38b38-133">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# [<a name="net-core-cli"></a><span data-ttu-id="38b38-134">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="38b38-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="38b38-135">Kullanım **Windows kimlik doğrulaması** uygulaması şablonu.</span><span class="sxs-lookup"><span data-stu-id="38b38-135">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="38b38-136">Yürütme [yeni dotnet](/dotnet/core/tools/dotnet-new) komutunu `webapp` bağımsız değişken (ASP.NET Core Web uygulaması) ve `--auth Windows` geçin:</span><span class="sxs-lookup"><span data-stu-id="38b38-136">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="38b38-137">Mevcut bir projeyi değiştirirken, proje dosyası için bir paket başvurusu içerdiğini onaylamak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **veya** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="38b38-137">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="38b38-138">IIS ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="38b38-138">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="38b38-139">IIS kullanan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konak ASP.NET Core uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="38b38-139">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="38b38-140">Windows kimlik doğrulaması için IIS yapılandırılır *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="38b38-140">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="38b38-141">Aşağıdaki bölümlerde show nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="38b38-141">The following sections show how to:</span></span>

* <span data-ttu-id="38b38-142">Yerel bir sağlamak *web.config* dosya uygulama dağıtıldığında, sunucu üzerinde Windows kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="38b38-142">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="38b38-143">IIS Yöneticisi'ni yapılandırmak için kullanın *web.config* dosyası sunucuda zaten dağıtılmış bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="38b38-143">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="38b38-144">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="38b38-144">IIS configuration</span></span>

<span data-ttu-id="38b38-145">Zaten yapmadıysanız, IIS için ASP.NET Core uygulamaları barındırın etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="38b38-145">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="38b38-146">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="38b38-146">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="38b38-147">Windows kimlik doğrulaması için IIS rolü hizmetini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="38b38-147">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="38b38-148">Daha fazla bilgi için [IIS rol hizmetlerini (bkz. 2. adım), Windows kimlik doğrulamasını etkinleştir](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="38b38-148">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="38b38-149">IIS tümleştirme ara yazılımı varsayılan olarak, varsayılan olarak otomatik olarak isteklerinde kimlik doğrulamak üzere yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="38b38-149">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="38b38-150">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core: IIS seçeneklerini (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="38b38-150">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="38b38-151">ASP.NET Core modülü varsayılan olarak, varsayılan olarak Windows kimlik doğrulaması belirteci uygulamaya iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="38b38-151">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="38b38-152">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu: AspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="38b38-152">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="38b38-153">Yeni bir IIS sitesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="38b38-153">Create a new IIS site</span></span>

<span data-ttu-id="38b38-154">Bir ad ve klasör belirtin ve yeni bir uygulama havuzu oluşturmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="38b38-154">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="38b38-155">IIS uygulama için Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="38b38-155">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="38b38-156">Kullanım **ya da** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="38b38-156">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="38b38-157">[Uygulamayı yayımlamadan önce geliştirme tarafı Yapılandırması](#development-side-configuration-with-a-local-webconfig-file) (*önerilen*)</span><span class="sxs-lookup"><span data-stu-id="38b38-157">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="38b38-158">Uygulamayı yayımladıktan sonra sunucu tarafı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="38b38-158">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="38b38-159">Yerel web.config dosyasıyla geliştirme tarafında yapılandırma</span><span class="sxs-lookup"><span data-stu-id="38b38-159">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="38b38-160">Aşağıdaki adımları gerçekleştirin **önce** , [projenizi dağıtma ve yayımlama](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="38b38-160">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="38b38-161">Aşağıdaki *web.config* proje kök dosya:</span><span class="sxs-lookup"><span data-stu-id="38b38-161">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="38b38-162">Proje SDK'sı tarafından yayımlanmıştır ne zaman (olmadan `<IsTransformWebConfigDisabled>` özelliğini `true` proje dosyasında), yayımlanan *web.config* dosyasını içeren `<location><system.webServer><security><authentication>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="38b38-162">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="38b38-163">Daha fazla bilgi için `<IsTransformWebConfigDisabled>` özelliği bkz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="38b38-163">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="38b38-164">IIS Yöneticisi ile sunucu tarafı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="38b38-164">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="38b38-165">Aşağıdaki adımları gerçekleştirin **sonra** , [projenizi dağıtma ve yayımlama](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="38b38-165">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="38b38-166">IIS Yöneticisi'nde IIS sitesi altında seçin **siteleri** düğümünün **bağlantıları** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="38b38-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="38b38-167">Çift **kimlik doğrulaması** içinde **IIS** alan.</span><span class="sxs-lookup"><span data-stu-id="38b38-167">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="38b38-168">Seçin **anonim kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="38b38-169">Seçin **devre dışı** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="38b38-169">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="38b38-170">Seçin **Windows kimlik doğrulaması**.</span><span class="sxs-lookup"><span data-stu-id="38b38-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="38b38-171">Seçin **etkinleştirme** içinde **eylemleri** kenar çubuğu.</span><span class="sxs-lookup"><span data-stu-id="38b38-171">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="38b38-172">Bu eylemler gerçekleştirildikçe, IIS Yöneticisi'ni uygulamanın değiştirir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="38b38-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="38b38-173">A `<system.webServer><security><authentication>` düğümü için güncelleştirilmiş ayarlarla eklenir `anonymousAuthentication` ve `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="38b38-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="38b38-174">`<system.webServer>` Bölümüne eklenen *web.config* dosyasıdır IIS Yöneticisi tarafından uygulamanın dışında `<location>` uygulama yayımlandığında, .NET Core SDK'sı tarafından eklenen bölümü.</span><span class="sxs-lookup"><span data-stu-id="38b38-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="38b38-175">Bölümün dışında eklendiğinden `<location>` düğümünü ayarları tarafından devralınır [alt uygulamaları](xref:host-and-deploy/iis/index#sub-applications) geçerli uygulama.</span><span class="sxs-lookup"><span data-stu-id="38b38-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="38b38-176">Devralma önlemek için ek taşıma `<security>` içine bölümünde `<location><system.webServer>` SDK'da bölümü.</span><span class="sxs-lookup"><span data-stu-id="38b38-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="38b38-177">IIS Yöneticisi, IIS yapılandırması eklemek için kullanıldığında, uygulamanın yalnızca etkiler *web.config* dosya sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="38b38-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="38b38-178">Uygulamanın bir sonraki dağıtım sunucudaki ayarları varsa üzerine yazabilir sunucunun kopyasını *web.config* projenin tarafından değiştirilir *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="38b38-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="38b38-179">Kullanım **ya da** ayarlarını yönetmek için aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="38b38-179">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="38b38-180">Ayarlar sıfırlamak için IIS Yöneticisi'ni kullanın *web.config* dağıtımı dosyanın üzerine yazılır sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="38b38-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="38b38-181">Ekleme bir *web.config dosyasını* uygulamada yerel olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="38b38-181">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="38b38-182">Daha fazla bilgi için [geliştirme tarafı Yapılandırması](#development-side-configuration-with-a-local-webconfig-file) bölümü.</span><span class="sxs-lookup"><span data-stu-id="38b38-182">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="38b38-183">Yayımlama ve IIS site klasöre projenizi dağıtın</span><span class="sxs-lookup"><span data-stu-id="38b38-183">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="38b38-184">Visual Studio veya .NET Core CLI'yı kullanarak, yayımlayın ve hedef klasöre uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="38b38-184">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="38b38-185">IIS ile barındırma ile ilgili daha fazla bilgi için yayımlama ve dağıtım, aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="38b38-185">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="38b38-186">DotNet yayımlama</span><span class="sxs-lookup"><span data-stu-id="38b38-186">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="38b38-187">Windows kimlik doğrulaması çalıştığını doğrulamak için uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="38b38-187">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="38b38-188">HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="38b38-188">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="38b38-189">Windows kimlik doğrulaması Kestrel desteklemiyor olsa da, kullanabileceğiniz [HTTP.sys](xref:fundamentals/servers/httpsys) Windows üzerinde şirket içinde barındırılan senaryoları desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="38b38-189">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="38b38-190">Aşağıdaki örnek, HTTP.sys ile Windows kimlik doğrulaması kullanmak için uygulamanın web ana bilgisayarı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="38b38-190">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="38b38-191">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="38b38-191">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="38b38-192">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="38b38-192">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="38b38-193">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="38b38-193">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="38b38-194">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="38b38-194">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="38b38-195">HTTP.sys sürüm 1709 veya üzeri Nano Sunucu'da desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="38b38-195">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="38b38-196">Windows kimlik doğrulaması ve HTTP.sys Nano Server ile kullanmak için bir [(microsoft/windowsservercore) Server Core kapsayıcı](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="38b38-196">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="38b38-197">Sunucu Çekirdeği hakkında daha fazla bilgi için bkz. [Windows Server'da Sunucu Çekirdeği yükleme seçeneği nedir?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="38b38-197">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="38b38-198">Windows kimlik doğrulaması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="38b38-198">Work with Windows Authentication</span></span>

<span data-ttu-id="38b38-199">Anonim erişim yapılandırma durumunu yolla belirler `[Authorize]` ve `[AllowAnonymous]` öznitelikleri, uygulamada kullanılır.</span><span class="sxs-lookup"><span data-stu-id="38b38-199">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="38b38-200">Aşağıdaki iki bölümü anonim erişime izin verilmeyen ve izin verilen yapılandırma durumunu nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="38b38-200">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="38b38-201">Anonim erişime izin verme</span><span class="sxs-lookup"><span data-stu-id="38b38-201">Disallow anonymous access</span></span>

<span data-ttu-id="38b38-202">Windows kimlik doğrulaması etkinleştirildiğinde ve adsız erişim devre dışıysa, `[Authorize]` ve `[AllowAnonymous]` özniteliklerinin etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="38b38-202">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="38b38-203">IIS sitesi (veya HTTP.sys), anonim erişime izin vermeyecek şekilde yapılandırıldığından, istek, uygulamanızı hiçbir zaman ulaşır.</span><span class="sxs-lookup"><span data-stu-id="38b38-203">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="38b38-204">Bu nedenle, `[AllowAnonymous]` özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="38b38-204">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="38b38-205">Anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="38b38-205">Allow anonymous access</span></span>

<span data-ttu-id="38b38-206">Windows kimlik doğrulaması hem anonim erişim etkin olduğunda, kullanmak `[Authorize]` ve `[AllowAnonymous]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="38b38-206">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="38b38-207">`[Authorize]` Özniteliği gerçekten Windows kimlik doğrulaması gerektiren uygulama parçalarını güvenli olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="38b38-207">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="38b38-208">`[AllowAnonymous]` Öznitelik geçersiz kılmalarını `[Authorize]` anonim erişime izin veren uygulamaların içindeki kullanım özniteliği.</span><span class="sxs-lookup"><span data-stu-id="38b38-208">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="38b38-209">Bkz: [basit yetkilendirme](xref:security/authorization/simple) özniteliği kullanım ayrıntıları için.</span><span class="sxs-lookup"><span data-stu-id="38b38-209">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="38b38-210">ASP.NET core'da 2.x `[Authorize]` öznitelik ek yapılandırma gerektirir *Startup.cs* anonim istekler için Windows kimlik doğrulaması meydan okuyun.</span><span class="sxs-lookup"><span data-stu-id="38b38-210">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="38b38-211">Önerilen yapılandırma kullanılan web sunucusu göre biraz farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="38b38-211">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="38b38-212">Varsayılan olarak, yetersiz bir sayfaya erişmek için yetkilendirme kullanıcılar ile boş bir HTTP 403 yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="38b38-212">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="38b38-213">[StatusCodePages ara yazılım](xref:fundamentals/error-handling#usestatuscodepages) kullanıcılar daha iyi bir "Erişim engellendi" deneyimi sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="38b38-213">The [StatusCodePages middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="38b38-214">IIS</span><span class="sxs-lookup"><span data-stu-id="38b38-214">IIS</span></span>

<span data-ttu-id="38b38-215">IIS kullanarak aşağıdakileri ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="38b38-215">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="38b38-216">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="38b38-216">HTTP.sys</span></span>

<span data-ttu-id="38b38-217">HTTP.sys kullanıyorsanız, ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="38b38-217">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="38b38-218">Kimliğe bürünme</span><span class="sxs-lookup"><span data-stu-id="38b38-218">Impersonation</span></span>

<span data-ttu-id="38b38-219">ASP.NET Core, kimliğe bürünme uygulamaz.</span><span class="sxs-lookup"><span data-stu-id="38b38-219">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="38b38-220">Uygulamalar, uygulama havuzu veya işlem kimliğini kullanarak, tüm istekler için uygulamanın kimlik ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="38b38-220">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="38b38-221">Açıkça bir kullanıcı adına eylem gerçekleştirmek ihtiyacınız varsa [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) içinde bir [terminal satır içi ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="38b38-221">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="38b38-222">Bu bağlamda tek bir eylem çalıştırın ve ardından bağlam kapatın.</span><span class="sxs-lookup"><span data-stu-id="38b38-222">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` <span data-ttu-id="38b38-223">zaman uyumsuz işlemleri desteklemeyen ve karmaşık senaryolar için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="38b38-223">doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="38b38-224">Örneğin, tüm istekleri veya bir ara yazılım zincirleri sarmalama desteklenen önerilen veya değil.</span><span class="sxs-lookup"><span data-stu-id="38b38-224">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="38b38-225">Talep dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="38b38-225">Claims transformations</span></span>

<span data-ttu-id="38b38-226">IIS işlem içi moduyla barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="38b38-226">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="38b38-227">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="38b38-227">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="38b38-228">Daha fazla bilgi ve barındırma işlemi içinde talep dönüştürmeleri etkinleştiren bir kod örneği için bkz: <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="38b38-228">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
