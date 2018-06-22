---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve hataları ASP.NET Core projelerle sorun giderme.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274599"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="c0f64-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="c0f64-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="c0f64-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0f64-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0f64-105">Aşağıdaki bağlantılar sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="c0f64-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="c0f64-106">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="c0f64-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="c0f64-107">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="c0f64-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="c0f64-108">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="c0f64-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="c0f64-109">NDC konferans (Londra, 2018): ASP.NET Core uygulamaları sorunları tanılama</span><span class="sxs-lookup"><span data-stu-id="c0f64-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="c0f64-110">ASP.NET Blog: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="c0f64-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="c0f64-111">.NET core SDK uyarıları</span><span class="sxs-lookup"><span data-stu-id="c0f64-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="c0f64-112">32 bit ve 64 bit sürümlerini .NET Core SDK yüklü</span><span class="sxs-lookup"><span data-stu-id="c0f64-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="c0f64-113">İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c0f64-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="c0f64-114">.NET Core SDK 32 ve 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="c0f64-115">Yüklü 64 bit sürümleri yalnızca bir şablondan ' C:\\Program Files\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="c0f64-117">Bu uyarı ne zaman görünür (x86) 32-bit ve 64-bit (x 64) sürümleri [.NET Core SDK](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="c0f64-118">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c0f64-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="c0f64-119">İlk olarak bir 32-bit makine kullanarak .NET Core SDK'sı yükleyicisi indirilen ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="c0f64-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="c0f64-120">32-bit .NET Core SDK başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="c0f64-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="c0f64-121">Sürümü yanlış indirilir ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="c0f64-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="c0f64-122">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c0f64-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="c0f64-123">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="c0f64-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="c0f64-124">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0f64-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="c0f64-125">.NET Core SDK birden fazla konumda yüklü</span><span class="sxs-lookup"><span data-stu-id="c0f64-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="c0f64-126">İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c0f64-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="c0f64-127">.NET Core SDK birden fazla konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="c0f64-128">Yalnızca yüklü SDK(s) şablonlardan ' C:\\Program Files\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="c0f64-130">.NET Core SDK'sının en az bir yüklemesi dışında bir dizinde sahip olduğunda bu iletiyi görmesini *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="c0f64-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="c0f64-131">Bu genellikle .NET Core SDK yerine MSI yükleyicisi kopyala/yapıştır kullanarak bir makinede dağıtılan ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="c0f64-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="c0f64-132">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c0f64-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="c0f64-133">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="c0f64-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="c0f64-134">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0f64-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="c0f64-135">Hiçbir .NET Core SDK'ları algılandı</span><span class="sxs-lookup"><span data-stu-id="c0f64-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="c0f64-136">İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c0f64-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="c0f64-137">Hiçbir .NET Core SDK'ları algılandı, ortam değişkeni 'PATH' içerdiği emin olun.</span><span class="sxs-lookup"><span data-stu-id="c0f64-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="c0f64-139">Bu uyarı ne zaman görünür ortam değişkeni `PATH` tüm .NET Core SDK makinedeki gösteremiyor.</span><span class="sxs-lookup"><span data-stu-id="c0f64-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="c0f64-140">Bu sorunu gidermek için:</span><span class="sxs-lookup"><span data-stu-id="c0f64-140">To resolve this problem:</span></span>

* <span data-ttu-id="c0f64-141">Yükleme veya .NET Core SDK yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c0f64-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="c0f64-142">Doğrulama `PATH` ortam değişkeni SDK yüklü konuma işaret eder.</span><span class="sxs-lookup"><span data-stu-id="c0f64-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="c0f64-143">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="c0f64-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="c0f64-144">Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir</span><span class="sxs-lookup"><span data-stu-id="c0f64-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="c0f64-145">ASP.NET Core 2.1 ve daha sonra çağırma `Html.Partial` Çözümleyicisi uyarı olası kilitlenme nedeniyle sonuçlanıyor.</span><span class="sxs-lookup"><span data-stu-id="c0f64-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="c0f64-146">Uyarı iletisi şudur:</span><span class="sxs-lookup"><span data-stu-id="c0f64-146">The warning message is:</span></span>

> <span data-ttu-id="c0f64-147">IHtmlHelper.Partial kullanımını uygulama kilitlenmeleri neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c0f64-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="c0f64-148">Kullanmayı `<partial>` etiket Yardımcısı veya `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="c0f64-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="c0f64-149">Çağrılar `@Html.Partial` tarafından değiştirilmelidir `@await Html.PartialAsync` veya kısmi etiket Yardımcısı `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="c0f64-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
