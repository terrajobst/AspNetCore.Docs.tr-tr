---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Mobil cihazlar için sayfaları (Razor) siteleri ASP.NET Web işleme | Microsoft Docs
author: tfitzmac
description: 'Bu makale, mobil cihazlarda uygun şekilde işlenir bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar. Öğrenecekleriniz: sizinle nasıl...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: d5c94af644c0dbc918544fe5112545c348136587
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369424"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="20d70-104">Mobil cihazlar için ASP.NET Web sayfaları (Razor) siteleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="20d70-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="20d70-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="20d70-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="20d70-106">Bu makale, mobil cihazlarda uygun şekilde işlenir bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="20d70-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="20d70-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="20d70-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="20d70-108">Bir sayfa belirtmek için bir adlandırma kuralı kullanmak nasıl mobil cihazlar için özel olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="20d70-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="20d70-109">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="20d70-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="20d70-110">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="20d70-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="20d70-111">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="20d70-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="20d70-112">ASP.NET Web sayfaları, mobil veya diğer cihazlar işleme içerik için özel görüntüler oluşturma olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="20d70-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="20d70-113">Böyle bir dosya adlandırma deseni kullanarak bir ASP.NET Web sayfaları sitesinde cihaza özgü sayfa oluşturmanın en kolay yolu olan: <em>FileName.</em> <em>Mobil</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="20d70-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="20d70-114">Sayfanın iki farklı sürümlerini oluşturabilirsiniz (örneğin, bir adlı <em>MyFile.cshtml</em> adlı bir <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="20d70-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="20d70-115">Çalışma zamanında, bir mobil cihaz istediğinde <em>MyFile.cshtml</em>, ASP.NET içeriği işler <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="20d70-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="20d70-116">Aksi takdirde, <em>MyFile.cshtml</em> işlenir.</span><span class="sxs-lookup"><span data-stu-id="20d70-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="20d70-117">Aşağıdaki örnek, mobil cihazlar için içerik sayfası ekleyerek mobil işleme olanağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="20d70-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="20d70-118">*Page1.cshtml* içerik gezinti kenar çubuğu içerir.</span><span class="sxs-lookup"><span data-stu-id="20d70-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="20d70-119">*Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.</span><span class="sxs-lookup"><span data-stu-id="20d70-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="20d70-120">Bir ASP.NET Web sayfaları sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve geçerli içerik aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="20d70-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="20d70-121">Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve varolan içeriği aşağıdaki biçimlendirme ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="20d70-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="20d70-122">Sayfayı mobil sürümü daha küçük bir ekranda daha iyi işleme için Gezinti bölümde atlar dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="20d70-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="20d70-123">Bir masaüstü tarayıcısı çalıştırın ve göz atın *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20d70-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="20d70-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20d70-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="20d70-125">Bir mobil tarayıcı (veya bir mobil cihaz öykünücüsünü) çalıştırın ve göz atın *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20d70-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="20d70-126">(Dahil etmezseniz bildirimi *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="20d70-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="20d70-127">URL'nin bir parçası.) İstek için olsa da *Page1.cshtml*, ASP.NET işler *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20d70-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="20d70-129">Mobil sayfalar test etmek için bir masaüstü bilgisayarda çalışan bir mobil cihaz simülatörünü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20d70-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="20d70-130">Bu aracı, mobil cihazlarda görüntülendiği şekilde, web sayfaları test sağlar (diğer bir deyişle, genellikle bir daha küçük alan görüntüler).</span><span class="sxs-lookup"><span data-stu-id="20d70-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="20d70-131">Simülatör biri [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan çeşitli mobil tarayıcılar Firefox Masaüstü sürümünden öykünmek.</span><span class="sxs-lookup"><span data-stu-id="20d70-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="20d70-132">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="20d70-132">Additional Resources</span></span>


<span data-ttu-id="20d70-133">[Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="20d70-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
