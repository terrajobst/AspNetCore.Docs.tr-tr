---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Bir yardımcı yükleme bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir yardımcı bir ASP.NET Web sayfaları (Razor) Web sitesi yüklemeyi açıklar. Bir yardımcı kod ve başına biçimlendirme içeren yeniden kullanılabilir bir bileşenidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896789"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c1af1-104">Bir ASP.NET Web sayfaları (Razor) sitesinde bir yardımcı yükleme</span><span class="sxs-lookup"><span data-stu-id="c1af1-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c1af1-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c1af1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c1af1-106">Bu makalede, bir yardımcı bir ASP.NET Web sayfaları (Razor) Web sitesi yüklemeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="c1af1-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="c1af1-107">A *yardımcı* kod ve sıkıcı veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="c1af1-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="c1af1-108">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="c1af1-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c1af1-109">Yardımcıyı WebMatrix 3 kullanılarak oluşturulan bir Web sitesinin yükleme.</span><span class="sxs-lookup"><span data-stu-id="c1af1-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c1af1-110">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="c1af1-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c1af1-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="c1af1-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="c1af1-112">Yardımcıları genel bakış</span><span class="sxs-lookup"><span data-stu-id="c1af1-112">Overview of Helpers</span></span>

<span data-ttu-id="c1af1-113">Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevleri çok fazla kod zorunlu veya ek bilgi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c1af1-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="c1af1-114">Veri için bir grafik görüntüleme örnekler; bir Twitter "İzle" düğmesine bir sayfada koyma; gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırma; PayPal siteniz için kullanma.</span><span class="sxs-lookup"><span data-stu-id="c1af1-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="c1af1-115">Bu tür bir şeyler yapın kolaylaştırmak için ASP.NET Web sayfaları kullanmanıza olanak sağlayan *Yardımcıları*.</span><span class="sxs-lookup"><span data-stu-id="c1af1-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="c1af1-116">Bileşenleri ve olanak sağlayan bir site için yüklediğiniz tipik yalnızca bir çizgi veya iki Razor kodunun kullanarak görevleri yardımcılardır.</span><span class="sxs-lookup"><span data-stu-id="c1af1-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="c1af1-117">ASP.NET Web sayfaları birkaç Yardımcıları yerleşik olarak sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c1af1-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="c1af1-118">Bununla birlikte, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketlerinde (eklentiler) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c1af1-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="c1af1-119">NuGet yüklemek için bir paket seçmenize olanak sağlar ve ardından bunu yükleme tüm ayrıntılarını mvc'deki.</span><span class="sxs-lookup"><span data-stu-id="c1af1-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="c1af1-120">WebMatrix 3 yardımcıyı yükleme</span><span class="sxs-lookup"><span data-stu-id="c1af1-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="c1af1-121">WebMatrix 3'te tıklatın **NuGet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c1af1-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![WebMatrix, NuGet Galerisi'nin iletişim kutusu](installing-helpers/_static/image1.png)
2. <span data-ttu-id="c1af1-123">Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c1af1-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="c1af1-124">Arama kutusuna, yüklemek istediğiniz Yardımcısı için bir anahtar sözcüğü girin.</span><span class="sxs-lookup"><span data-stu-id="c1af1-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![WebMatrix, NuGet Galerisi'nin iletişim kutusu](installing-helpers/_static/image2.png)
3. <span data-ttu-id="c1af1-126">Paketi seçin ve ardından **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="c1af1-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="c1af1-127">Tıklatın **Evet** paket yüklemek ve koşullarını kabul ettiğinizi belirtmek isteyip istemediğiniz sorulduğunda.</span><span class="sxs-lookup"><span data-stu-id="c1af1-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="c1af1-128">Yardımcıyı yüklediğiniz ilk kez kullanıyorsanız, NuGet klasörleri Web siteniz için Yardımcısı yapar kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1af1-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="c1af1-129">Yardımcıyı kaldırmak için tıklatın **galeri** düğmesini tıklatın, **yüklü** sekmesini tıklatın ve kaldırmak istediğiniz paketi seçin.</span><span class="sxs-lookup"><span data-stu-id="c1af1-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="c1af1-130">Twitter Yardımcısı yükleniyor</span><span class="sxs-lookup"><span data-stu-id="c1af1-130">Installing the Twitter helper</span></span>

<span data-ttu-id="c1af1-131">Twitter API'si en son sürümünü NuGet yükleme Twitter Yardımcısı ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="c1af1-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="c1af1-132">Bunun yerine bkz [Twitter Yardımcısı WebMatrix ile](twitter-helper.md) projenizdeki Twitter Yardımcısı ayarlama hakkında bilgi için konu.</span><span class="sxs-lookup"><span data-stu-id="c1af1-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c1af1-133">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c1af1-133">Additional Resources</span></span>


[<span data-ttu-id="c1af1-134">2 - Programlama temelleri ile tanışın ASP.NET Web sayfaları</span><span class="sxs-lookup"><span data-stu-id="c1af1-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="c1af1-135">WebMatrix ile twitter Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="c1af1-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
