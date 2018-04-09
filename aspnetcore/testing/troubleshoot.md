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
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="d8d42-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="d8d42-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="d8d42-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8d42-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8d42-105">Aşağıdaki bağlantılar sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="d8d42-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="d8d42-106">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="d8d42-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="d8d42-107">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="d8d42-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="d8d42-108">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="d8d42-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="d8d42-109">.NET core SDK uyarıları</span><span class="sxs-lookup"><span data-stu-id="d8d42-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="d8d42-110">32 ve 64 bit sürümleri .NET Core SDK'ın yüklü</span><span class="sxs-lookup"><span data-stu-id="d8d42-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="d8d42-111">İçinde **yeni proje** iletişim ASP.NET Core için üstünde görünür aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d8d42-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="d8d42-113">Bu uyarı ne zaman görünür (x86) 32-bit ve 64-bit (x 64) sürümleri [.NET Core SDK](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d8d42-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="d8d42-114">Her iki sürümünü de yüklenebilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d8d42-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="d8d42-115">İlk olarak bir 32-bit makine kullanarak .NET Core SDK'sı yükleyicisi indirilen ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="d8d42-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="d8d42-116">32-bit .NET Core SDK başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d8d42-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="d8d42-117">Sürümü yanlış indirilir ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d8d42-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="d8d42-118">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d8d42-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="d8d42-119">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="d8d42-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d8d42-120">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8d42-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="d8d42-121">.NET Core SDK birden fazla konumda yüklü</span><span class="sxs-lookup"><span data-stu-id="d8d42-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="d8d42-122">İçinde **yeni proje** iletişim kutusu için ASP.NET Core üstünde görünür aşağıdaki uyarıyı karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d8d42-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="d8d42-123">.NET Core SDK birden fazla konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d8d42-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="d8d42-124">Yüklü SDK(s) sadece şablonlardan ' C:\Program Files\dotnet\sdk\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d8d42-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="d8d42-126">Dışında bir dizinde en az bir yükleme .NET Core SDK'ın yüklü olduğundan bu iletiyi görüyorsunuz * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="d8d42-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="d8d42-127">.NET Core SDK yerine MSI yükleyicisi kopyala/yapıştır kullanarak bir makinede dağıtılan genellikle, olur.</span><span class="sxs-lookup"><span data-stu-id="d8d42-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="d8d42-128">Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d8d42-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="d8d42-129">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="d8d42-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d8d42-130">Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8d42-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
