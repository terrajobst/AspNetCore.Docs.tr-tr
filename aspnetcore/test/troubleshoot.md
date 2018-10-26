---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090117"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="fc23d-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="fc23d-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="fc23d-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc23d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fc23d-105">Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc23d-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="fc23d-106">NDC konferansı (Londra, 2018): ASP.NET Core uygulamaları tanılama sorunları</span><span class="sxs-lookup"><span data-stu-id="fc23d-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="fc23d-107">ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="fc23d-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="fc23d-108">.NET core SDK'sı uyarıları</span><span class="sxs-lookup"><span data-stu-id="fc23d-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="fc23d-109">Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir</span><span class="sxs-lookup"><span data-stu-id="fc23d-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="fc23d-110">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fc23d-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="fc23d-111">.NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="fc23d-112">Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="fc23d-114">Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="fc23d-115">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fc23d-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="fc23d-116">İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="fc23d-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="fc23d-117">32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="fc23d-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="fc23d-118">Yanlış sürümü indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="fc23d-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="fc23d-119">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="fc23d-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="fc23d-120">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="fc23d-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="fc23d-121">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc23d-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="fc23d-122">.NET Core SDK'sını birden çok konumda yüklenir</span><span class="sxs-lookup"><span data-stu-id="fc23d-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="fc23d-123">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fc23d-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="fc23d-124">.NET Core SDK'sı, birden çok konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="fc23d-125">Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="fc23d-127">.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="fc23d-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="fc23d-128">Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="fc23d-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="fc23d-129">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="fc23d-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="fc23d-130">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="fc23d-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="fc23d-131">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc23d-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="fc23d-132">.NET çekirdek SDK algılanmadı</span><span class="sxs-lookup"><span data-stu-id="fc23d-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="fc23d-133">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fc23d-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="fc23d-134">.NET çekirdek SDK algılanmadı, 'PATH' ortam değişkenine dahil olduklarından emin olun.</span><span class="sxs-lookup"><span data-stu-id="fc23d-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="fc23d-136">Bu uyarı görüntülenir ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor.</span><span class="sxs-lookup"><span data-stu-id="fc23d-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="fc23d-137">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="fc23d-137">To resolve this problem:</span></span>

* <span data-ttu-id="fc23d-138">Yükleme veya .NET Core SDK'sı yüklü olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc23d-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="fc23d-139">Doğrulayın `PATH` ortam değişkeni, SDK'ın yüklendiği konumun işaret eder.</span><span class="sxs-lookup"><span data-stu-id="fc23d-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="fc23d-140">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="fc23d-140">The installer normally sets the `PATH`.</span></span>
