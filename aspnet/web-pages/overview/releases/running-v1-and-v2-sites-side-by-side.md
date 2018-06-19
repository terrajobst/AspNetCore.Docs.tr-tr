---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: ASP.NET Web sayfaları (Razor) yan yana farklı sürümlerini çalıştıran | Microsoft Docs
author: tfitzmac
description: Bu makalede, Web siteleri farklı sürümlerini kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web sitelerine aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898412"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="25bbc-103">ASP.NET Web sayfaları (Razor) farklı sürümlerini yan yana çalıştırma</span><span class="sxs-lookup"><span data-stu-id="25bbc-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="25bbc-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="25bbc-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="25bbc-105">Bu makalede, Web siteleri farklı sürümler, ASP.NET Web sayfaları kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web sitelerine aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="25bbc-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="25bbc-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="25bbc-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="25bbc-107">ASP.NET Web Pages ile oluşturulmuş sitelerin olduğunda ne varsayılan ASP.NET davranıştır.</span><span class="sxs-lookup"><span data-stu-id="25bbc-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="25bbc-108">Yeni bir site, ASP.NET Web sayfaları daha eski bir sürümü ile çalışacak şekilde yapılandırmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="25bbc-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="25bbc-109">Bu makalede sunulan ASP.NET özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="25bbc-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="25bbc-110">`webPages:Version` Yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="25bbc-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="25bbc-111">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="25bbc-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="25bbc-112">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="25bbc-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="25bbc-113">Bu öğreticide, ASP.NET Web Pages 2 ve ASP.NET Web sayfaları 1.0 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="25bbc-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="25bbc-114">ASP.NET Web sayfaları Web siteleri yan yana çalıştırma yeteneğini destekler.</span><span class="sxs-lookup"><span data-stu-id="25bbc-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="25bbc-115">Bu, eski ASP.NET Web sayfaları uygulamalarınızı çalıştırmak, yeni ASP.NET Web Pages uygulamaları geliştirmek ve bunların tümünün aynı bilgisayarda çalışan devam sağlar.</span><span class="sxs-lookup"><span data-stu-id="25bbc-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="25bbc-116">WebMatrix ile Web sayfaları yüklediğinizde unutmamanız gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="25bbc-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="25bbc-117">Varsayılan olarak, bilgisayarınızda en son sürümü olarak mevcut Web Pages uygulamaları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="25bbc-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="25bbc-118">(Derlemeleri genel derleme önbelleğinde (GAC) yüklü olduğundan ve otomatik olarak kullanılır.)</span><span class="sxs-lookup"><span data-stu-id="25bbc-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="25bbc-119">ASP.NET Web sayfaları, farklı bir sürümünü kullanarak bir site çalıştırmak istiyorsanız, bunu yapmak için site yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25bbc-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="25bbc-120">Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML dosyasını buraya var olan içeriğin üzerine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="25bbc-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="25bbc-121">Site zaten varsa bir *web.config* dosya, ekleme bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="25bbc-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="25bbc-122">'-'Sürümünde belirtmezseniz *web.config* dosya, bir site en son sürümü olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="25bbc-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="25bbc-123">(Derlemeler kopyalanır *bin* sitesi dağıtıldı klasöründe.)</span><span class="sxs-lookup"><span data-stu-id="25bbc-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="25bbc-124">Web Matrix site şablonları kullanarak oluşturduğunuz yeni uygulamalar dahil Web sayfaları sürüm derlemeleri sitenin içinde *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="25bbc-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="25bbc-125">Genel olarak, her zaman uygun derlemelerini sitenin yüklemek için NuGet kullanarak sitenizle kullanmak için Web sayfalarının hangi sürümü kontrol edebilirsiniz *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="25bbc-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="25bbc-126">Paketler için ziyaret edin [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="25bbc-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25bbc-127">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="25bbc-127">Additional Resources</span></span>

[<span data-ttu-id="25bbc-128">ASP.NET Web Pages 2 üst özellikleri</span><span class="sxs-lookup"><span data-stu-id="25bbc-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
