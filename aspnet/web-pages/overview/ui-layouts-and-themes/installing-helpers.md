---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Bir yardımcıyı yükleme bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: Rick-Anderson
description: Bu makalede, nasıl bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı yükleneceği açıklanır. Bir yardımcı kod ve işaretlemede başına içeren yeniden kullanılabilir bir bileşen olan...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 5ad717cd7c64e830ce66d5e1361d0eb6ef3cbbec
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021371"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="91e30-104">Bir ASP.NET Web sayfaları (Razor) sitesinde bir yardımcıyı yükleme</span><span class="sxs-lookup"><span data-stu-id="91e30-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="91e30-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="91e30-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="91e30-106">Bu makalede, nasıl bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı yükleneceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="91e30-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="91e30-107">A *Yardımcısı* kod ve yorucu bir süreç veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="91e30-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="91e30-108">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="91e30-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="91e30-109">Nasıl yardımcı WebMatrix 3'ü kullanarak oluşturulan bir Web sitesine yüklenir.</span><span class="sxs-lookup"><span data-stu-id="91e30-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="91e30-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="91e30-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="91e30-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="91e30-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="91e30-112">Yardımcıları genel bakış</span><span class="sxs-lookup"><span data-stu-id="91e30-112">Overview of Helpers</span></span>

<span data-ttu-id="91e30-113">Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevler bir sürü kod gerektiren veya ek bilgi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="91e30-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="91e30-114">Veriler için bir grafik görüntüleme verilebilir; bir Twitter "İzle" düğmesine bir sayfa koyarak; gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırmayı; PayPal, siteniz için kullanma.</span><span class="sxs-lookup"><span data-stu-id="91e30-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="91e30-115">Bu tür bir şeyler yapmasını kolaylaştırır, ASP.NET Web Pages kullanmanıza olanak sağlar. *Yardımcıları*.</span><span class="sxs-lookup"><span data-stu-id="91e30-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="91e30-116">Bileşenleri ve olanak sağlayan bir site için yüklediğiniz tipik bir çizgi veya iki Razor kodunun kullanarak görevleri yardımcılardır.</span><span class="sxs-lookup"><span data-stu-id="91e30-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="91e30-117">ASP.NET Web Pages birkaç Yardımcıları yerleşik olarak sahiptir.</span><span class="sxs-lookup"><span data-stu-id="91e30-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="91e30-118">Ancak, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketleri (add-INS) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91e30-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="91e30-119">NuGet paketini yüklemek için seçmenize olanak sağlar ve ardından bu yükleme tüm ayrıntılarını üstlenir.</span><span class="sxs-lookup"><span data-stu-id="91e30-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="91e30-120">WebMatrix 3 ' bir yardımcıyı yükleme</span><span class="sxs-lookup"><span data-stu-id="91e30-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="91e30-121">WebMatrix 3'te tıklatın **NuGet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="91e30-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![WebMatrix, NuGet galerisindeki iletişim kutusu](installing-helpers/_static/image1.png)
2. <span data-ttu-id="91e30-123">Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler.</span><span class="sxs-lookup"><span data-stu-id="91e30-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="91e30-124">Arama kutusuna Yardımcısını yüklemek istediğiniz bir anahtar sözcüğü girin.</span><span class="sxs-lookup"><span data-stu-id="91e30-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![WebMatrix, NuGet galerisindeki iletişim kutusu](installing-helpers/_static/image2.png)
3. <span data-ttu-id="91e30-126">Paketi seçin ve ardından **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="91e30-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="91e30-127">Tıklayın **Evet** paketi yükleyin ve koşullarını kabul ettiğinizi belirtmek isteyip istemediğiniz sorulduğunda.</span><span class="sxs-lookup"><span data-stu-id="91e30-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="91e30-128">Bu yardımcı yüklediğiniz ilk kez kullanıyorsanız, NuGet klasörleri Web siteniz için yardımcı yapan kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="91e30-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="91e30-129">Bir yardımcı kaldırmak için tıklayın **galeri** düğmesini tıklatın, **yüklü** sekmesini ve kaldırmak istediğiniz paketi seçin.</span><span class="sxs-lookup"><span data-stu-id="91e30-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="91e30-130">Twitter Yardımcısı'nı yükleme</span><span class="sxs-lookup"><span data-stu-id="91e30-130">Installing the Twitter helper</span></span>

<span data-ttu-id="91e30-131">En son sürümünü Twitter API'si NuGet yükleme için Twitter Yardımcısı ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="91e30-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="91e30-132">Bunun yerine bkz [WebMatrix ile Twitter Yardımcısı](twitter-helper.md) için Twitter Yardımcısı, projenizdeki ayarlama hakkında bilgi için konu.</span><span class="sxs-lookup"><span data-stu-id="91e30-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="91e30-133">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="91e30-133">Additional Resources</span></span>


[<span data-ttu-id="91e30-134">Karşınızda ASP.NET Web sayfaları 2 - Programlama temelleri</span><span class="sxs-lookup"><span data-stu-id="91e30-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="91e30-135">WebMatrix ile twitter Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="91e30-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
