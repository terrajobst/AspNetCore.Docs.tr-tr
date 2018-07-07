---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889018"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="bf269-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bf269-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="bf269-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf269-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bf269-105">Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="bf269-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="bf269-106">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="bf269-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="bf269-107">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="bf269-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="bf269-108">Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="bf269-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="bf269-109">NDC konferansı (Londra, 2018): ASP.NET Core uygulamaları tanılama sorunları</span><span class="sxs-lookup"><span data-stu-id="bf269-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="bf269-110">ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="bf269-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="bf269-111">.NET core SDK'sı uyarıları</span><span class="sxs-lookup"><span data-stu-id="bf269-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="bf269-112">Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir</span><span class="sxs-lookup"><span data-stu-id="bf269-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="bf269-113">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf269-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="bf269-114">.NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="bf269-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="bf269-115">Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bf269-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="bf269-117">Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="bf269-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="bf269-118">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="bf269-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="bf269-119">İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="bf269-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="bf269-120">32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bf269-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="bf269-121">Yanlış sürümü indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bf269-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="bf269-122">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bf269-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="bf269-123">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="bf269-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="bf269-124">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf269-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="bf269-125">.NET Core SDK'sını birden çok konumda yüklenir</span><span class="sxs-lookup"><span data-stu-id="bf269-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="bf269-126">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf269-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="bf269-127">.NET Core SDK'sı, birden çok konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="bf269-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="bf269-128">Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bf269-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="bf269-130">.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="bf269-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="bf269-131">Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bf269-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="bf269-132">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bf269-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="bf269-133">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="bf269-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="bf269-134">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf269-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="bf269-135">.NET çekirdek SDK algılanmadı</span><span class="sxs-lookup"><span data-stu-id="bf269-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="bf269-136">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf269-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="bf269-137">.NET çekirdek SDK algılanmadı, 'PATH' ortam değişkenine dahil olduklarından emin olun.</span><span class="sxs-lookup"><span data-stu-id="bf269-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="bf269-139">Bu uyarı görüntülenir ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor.</span><span class="sxs-lookup"><span data-stu-id="bf269-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="bf269-140">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="bf269-140">To resolve this problem:</span></span>

* <span data-ttu-id="bf269-141">Yükleme veya .NET Core SDK'sı yüklü olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bf269-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="bf269-142">Doğrulayın `PATH` ortam değişkeni, SDK'ın yüklendiği konumun işaret eder.</span><span class="sxs-lookup"><span data-stu-id="bf269-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="bf269-143">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="bf269-143">The installer normally sets the `PATH`.</span></span>