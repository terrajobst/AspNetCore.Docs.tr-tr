---
title: ASP.NET çekirdek için sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve hataları ASP.NET Core projelerle sorun giderme.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 3bba085c69ee96b5725331b14dcf15350d66e4a4
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="11e2d-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="11e2d-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="11e2d-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="11e2d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="11e2d-105">Aşağıdaki bağlantılar sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="11e2d-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="11e2d-106">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="11e2d-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="11e2d-107">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="11e2d-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="11e2d-108">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="11e2d-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="11e2d-109">YouTube: ASP.NET Core uygulamaları sorunları tanılama</span><span class="sxs-lookup"><span data-stu-id="11e2d-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="11e2d-110">.NET core SDK uyarıları</span><span class="sxs-lookup"><span data-stu-id="11e2d-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="11e2d-111">32 bit ve 64 bit sürümlerini .NET Core SDK yüklü</span><span class="sxs-lookup"><span data-stu-id="11e2d-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="11e2d-112">İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="11e2d-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="11e2d-114">Bu uyarı ne zaman görünür (x86) 32-bit ve 64-bit (x 64) sürümleri [.NET Core SDK](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="11e2d-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="11e2d-115">Her iki sürümünü de yüklenebilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="11e2d-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="11e2d-116">İlk olarak bir 32-bit makine kullanarak .NET Core SDK'sı yükleyicisi indirilen ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="11e2d-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="11e2d-117">32-bit .NET Core SDK başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="11e2d-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="11e2d-118">Sürümü yanlış indirilir ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="11e2d-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="11e2d-119">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="11e2d-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="11e2d-120">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="11e2d-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="11e2d-121">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11e2d-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="11e2d-122">.NET Core SDK birden fazla konumda yüklü</span><span class="sxs-lookup"><span data-stu-id="11e2d-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="11e2d-123">İçinde **yeni proje** iletişim için ASP.NET Core aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="11e2d-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="11e2d-124">.NET Core SDK birden fazla konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="11e2d-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="11e2d-125">Yüklü SDK(s) sadece şablonlardan ' C:\Program Files\dotnet\sdk\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="11e2d-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="11e2d-127">.NET Core SDK'sının en az bir yüklemesi dışında bir dizinde sahip olduğunda bu iletiyi görmesini * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="11e2d-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="11e2d-128">.NET Core SDK yerine MSI yükleyicisi kopyala/yapıştır kullanarak bir makinede dağıtılan genellikle, olur.</span><span class="sxs-lookup"><span data-stu-id="11e2d-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="11e2d-129">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="11e2d-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="11e2d-130">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="11e2d-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="11e2d-131">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11e2d-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="11e2d-132">Hiçbir .NET Core SDK'ları algılandı</span><span class="sxs-lookup"><span data-stu-id="11e2d-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="11e2d-133">İçinde **yeni proje** iletişim için ASP.NET Core aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="11e2d-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="11e2d-134">**Hiçbir .NET Core SDK'ları algılandı, ortam değişkeni 'PATH' içerdiği emin olun**</span><span class="sxs-lookup"><span data-stu-id="11e2d-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="11e2d-136">Bu uyarı ne zaman görünür ortam değişkeni `PATH` tüm .NET Core SDK makinedeki gösteremiyor.</span><span class="sxs-lookup"><span data-stu-id="11e2d-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="11e2d-137">Bu sorunu gidermek için:</span><span class="sxs-lookup"><span data-stu-id="11e2d-137">To resolve this problem:</span></span>

* <span data-ttu-id="11e2d-138">Yükleme veya .NET Core SDK yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="11e2d-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="11e2d-139">Doğrulama `PATH` ortam değişkeni SDK yüklü konuma işaret eder.</span><span class="sxs-lookup"><span data-stu-id="11e2d-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="11e2d-140">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="11e2d-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="11e2d-141">Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir</span><span class="sxs-lookup"><span data-stu-id="11e2d-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="11e2d-142">ASP.NET Core 2.1 ve daha sonra çağırma `Html.Partial` Çözümleyicisi uyarı olası kilitlenme nedeniyle sonuçlanıyor.</span><span class="sxs-lookup"><span data-stu-id="11e2d-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="11e2d-143">Uyarı iletisi şudur:</span><span class="sxs-lookup"><span data-stu-id="11e2d-143">The warning message is:</span></span>

<span data-ttu-id="11e2d-144">*IHtmlHelper.Partial kullanımını uygulama kilitlenmeleri neden olabilir. Kullanmayı `<partial>` etiket Yardımcısı veya `IHtmlHelper.PartialAsync`.*</span><span class="sxs-lookup"><span data-stu-id="11e2d-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="11e2d-145">Çağrılar `@Html.Partial` tarafından değiştirilmelidir `@await Html.PartialAsync` veya kısmi etiket Yardımcısı `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="11e2d-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
