---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: AJAX Denetim Araç Seti ile (VB) kullanmaya başlayın | Microsoft Docs
author: microsoft
description: AJAX Denetim Araç Seti ile çalışmaya başlamak için bilmeniz gereken her şeyi öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 6041087df3a15ef42d2364881f08a991f4eeb4fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367780"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="f866d-103">AJAX Denetim Araç Seti ile (VB) kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="f866d-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="f866d-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f866d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f866d-105">AJAX Denetim Araç Seti ile çalışmaya başlamak için bilmeniz gereken her şeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="f866d-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="f866d-106">AJAX Denetim Araç Seti, ASP.NET uygulamalarınızda kullanabileceğiniz 30'dan fazla ücretsiz denetimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f866d-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="f866d-107">Bu öğreticide, AJAX Denetim Araç Seti indirme ve araç seti denetimlerini, Visual Studio/Visual Web Developer Express araç kutusuna ekleme hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="f866d-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="f866d-108">AJAX Denetim Araç Seti yükleniyor</span><span class="sxs-lookup"><span data-stu-id="f866d-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="f866d-109">[AJAX Denetim Araç Seti](http://devexpress.com/act) ASP.NET topluluğu ve ASP.NET takım üyeleri tarafından geliştirilen bir açık kaynak projesi.</span><span class="sxs-lookup"><span data-stu-id="f866d-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="f866d-110">[![AJAX Denetim Araç Seti yükleniyor](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f866d-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="f866d-111">**Şekil 01**: AJAX Denetim Araç Seti indiriliyor ([tam boyutlu görüntüyü görmek için tıklatın](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f866d-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="f866d-112">Dosyayı indirdikten sonra dosyayı engelini kaldırmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="f866d-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="f866d-113">Dosyaya sağ tıklayın, Özellikler'i seçin ve tıklayın **Engellemeyi Kaldır** (bkz: Şekil 2) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f866d-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="f866d-114">[![AJAX Denetim Araç Seti ZIP dosyası kaldırma](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f866d-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="f866d-115">**Şekil 02**: AJAX Denetim Araç Seti ZIP dosyası kaldırma ([tam boyutlu görüntüyü görmek için tıklatın](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f866d-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="f866d-116">Dosya engelini kaldırdıktan sonra dosyayı sıkıştırılmış dosyayı açabilirsiniz: dosyaya sağ tıklayıp **tümünü Ayıkla** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="f866d-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="f866d-117">Şimdi, araç seti Visual Studio/Visual Web Developer araç kutusuna ekleme hazırız.</span><span class="sxs-lookup"><span data-stu-id="f866d-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="f866d-118">AJAX Denetim Araç Seti araç kutusuna ekleme</span><span class="sxs-lookup"><span data-stu-id="f866d-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="f866d-119">AJAX Denetim Araç Seti kullanmanın en kolay yolu, Visual Studio/Visual Web Developer araç için araç seti eklemektir (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="f866d-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="f866d-120">Bunu kullanmak istediğinizde bu şekilde, yalnızca bir araç seti denetim bir sayfaya sürükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f866d-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="f866d-121">[![AJAX Denetim Araç Seti araç kutusunda görünür.](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f866d-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="f866d-122">**Şekil 03**: AJAX Denetim Araç Seti araç kutusunda görünür ([tam boyutlu görüntüyü görmek için tıklatın](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f866d-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="f866d-123">İlk olarak, bir AJAX Denetim Araç Seti sekmesi, araç kutusuna ekleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="f866d-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="f866d-124">Bu adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="f866d-124">Follow these steps.</span></span>

1. <span data-ttu-id="f866d-125">Dosya, yeni Web sitesi menü seçeneğini seçerek yeni bir ASP.NET Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f866d-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="f866d-126">Çözüm Gezgini penceresinde Default.aspx Düzenleyicisi'nde dosyayı açmak için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f866d-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="f866d-127">Genel sekmesi altında araç kutusunu sağ tıklatın ve menü seçeneğini **Sekme Ekle** (bkz: Şekil 4).</span><span class="sxs-lookup"><span data-stu-id="f866d-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="f866d-128">AJAX Denetim Araç Seti adlı yeni bir sekme girin.</span><span class="sxs-lookup"><span data-stu-id="f866d-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="f866d-129">[![Yeni bir sekme ekleme](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f866d-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="f866d-130">**Şekil 04**: yeni bir sekme ekleme ([tam boyutlu görüntüyü görmek için tıklatın](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f866d-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="f866d-131">Ardından, yeni bir sekmeye AJAX Denetim Araç Seti denetim eklemek gerekir. Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f866d-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="f866d-132">AJAX Denetim Araç Seti sekmesi sağ tıklayın ve menü seçeneğini **seçin (bkz: Şekil 5) öğeleri**.</span><span class="sxs-lookup"><span data-stu-id="f866d-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="f866d-133">Burada AJAX Denetim Araç Seti sıkıştırması açılan ve AjaxControlToolkit.dll derlemeyi seçin konumuna göz atın.</span><span class="sxs-lookup"><span data-stu-id="f866d-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="f866d-134">[![Araç kutusuna eklenecek öğeleri seçin](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f866d-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="f866d-135">**Şekil 05**: araç kutusuna eklenecek öğeleri seçin ([tam boyutlu görüntüyü görmek için tıklatın](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f866d-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="f866d-136">Bu adımları tamamladıktan sonra tüm araç seti denetimlerini, araç kutusunda görünür.</span><span class="sxs-lookup"><span data-stu-id="f866d-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="f866d-137">Araç Seti yeni bir sürüme yükseltme</span><span class="sxs-lookup"><span data-stu-id="f866d-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="f866d-138">Araç Seti eski bir sürümünü kullanıyor ve taşımak artık gereksinim burada sonraki bir sürümü önerilen adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f866d-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="f866d-139">İkili - AjaxControlToolkit.dll derlemenin eski sürümü, Web sitesi Bin klasöründen silin.</span><span class="sxs-lookup"><span data-stu-id="f866d-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="f866d-140">Araç kutusu öğeleri - AJAX Denetim Araç Seti sekmesini silme ve AjaxControlToolkit.dll derlemenin yeni sürümünü içeren sekmeye yeniden oluşturmak için yukarıdaki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="f866d-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f866d-141">[Önceki](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [İleri](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f866d-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
