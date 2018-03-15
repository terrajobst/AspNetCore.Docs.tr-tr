---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Mobil cihazlar için sayfaları (Razor) siteleri ASP.NET Web işleme | Microsoft Docs"
author: tfitzmac
description: "Bu makalede, mobil cihazlarda uygun şekilde kılacak bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar. Öğrenecekleriniz: size nasıl..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="b22a7-104">Mobil cihazlar için ASP.NET Web sayfaları (Razor) siteleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="b22a7-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="b22a7-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b22a7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b22a7-106">Bu makalede, mobil cihazlarda uygun şekilde kılacak bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="b22a7-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="b22a7-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="b22a7-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b22a7-108">Bir adlandırma kuralı belirten bir sayfa için nasıl kullanılacağını mobil cihazlar için özel olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b22a7-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b22a7-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="b22a7-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b22a7-110">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b22a7-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b22a7-111">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="b22a7-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="b22a7-112">ASP.NET Web sayfaları, cep telefonu numarası veya diğer cihazları işleme içerik için özel görüntüler oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b22a7-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="b22a7-113">Böyle bir dosya adlandırma deseni kullanarak bir ASP.NET Web Pages sitesinde aygıta özgü sayfası oluşturmak için en basit yolu olan: *FileName. **Mobil**.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b22a7-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="b22a7-114">Sayfanın iki farklı sürümlerini oluşturabilirsiniz (örneğin, bir adlı *MyFile.cshtml* ve adlı bir *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b22a7-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="b22a7-115">Çalışma zamanında, bir mobil cihaz isteğinde bulunduğunda *MyFile.cshtml*, ASP.NET işler içerikten *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b22a7-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="b22a7-116">Aksi takdirde, *MyFile.cshtml* işlenir.</span><span class="sxs-lookup"><span data-stu-id="b22a7-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="b22a7-117">Aşağıdaki örnek, mobil cihazlar için bir içerik sayfasını ekleyerek mobil işleme etkinleştirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b22a7-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="b22a7-118">*Page1.cshtml* içerik ek olarak bir gezinti Kenar Çubuğu'nu içerir.</span><span class="sxs-lookup"><span data-stu-id="b22a7-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="b22a7-119">*Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.</span><span class="sxs-lookup"><span data-stu-id="b22a7-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="b22a7-120">Bir ASP.NET Web Pages sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve geçerli içerik aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b22a7-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="b22a7-121">Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve varolan içeriği aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b22a7-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="b22a7-122">Sayfanın mobil sürümünü daha küçük bir ekran üzerinde daha iyi işleme için gezinti bölümüne atlar dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b22a7-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="b22a7-123">Bir masaüstü tarayıcısı çalıştırın ve Gözat *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b22a7-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="b22a7-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b22a7-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="b22a7-125">Bir mobil tarayıcı (veya bir mobil aygıt benzeticisi) çalıştıran ve Gözat *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b22a7-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="b22a7-126">(İçermiyor bildirimi *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="b22a7-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="b22a7-127">URL'SİNİN bir parçası.) İstek için olsa da *Page1.cshtml*, ASP.NET işler *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b22a7-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="b22a7-129">Mobil sayfalar sınamak için bir masaüstü bilgisayar üzerinde çalışan bir mobil aygıt benzeticisi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b22a7-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="b22a7-130">Bu araç, mobil cihazlarda görüneceği şekilde web sayfalarını test sağlar (diğer bir deyişle, genellikle bir daha küçük alan gösterir).</span><span class="sxs-lookup"><span data-stu-id="b22a7-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="b22a7-131">Bir simulator bir örnektir [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan, Firefox Masaüstü sürümünden çeşitli mobil tarayıcılar öykünmek.</span><span class="sxs-lookup"><span data-stu-id="b22a7-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b22a7-132">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b22a7-132">Additional Resources</span></span>


<span data-ttu-id="b22a7-133">[Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="b22a7-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
