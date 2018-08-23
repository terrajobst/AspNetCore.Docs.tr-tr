---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: ASP.NET Web sayfaları (Razor) yan yana farklı sürümlerini çalıştıran | Microsoft Docs
author: tfitzmac
description: Bu makalede, Web siteleri farklı sürümler kullanmak için yapılandırıldığında, ASP.NET Web sayfaları (Razor) Web siteleri aynı bilgisayara veya sunucuya çalıştırılması açıklanmaktadır...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752737"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="2e6d3-103">ASP.NET Web sayfaları (Razor) farklı sürümlerini yan yana çalıştırma</span><span class="sxs-lookup"><span data-stu-id="2e6d3-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="2e6d3-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2e6d3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2e6d3-105">Bu makalede, Web siteleri, ASP.NET Web Pages farklı sürümlerini kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web siteleri aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="2e6d3-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="2e6d3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2e6d3-107">ASP.NET Web Pages ile oluşturulmuş sitelerin olduğunda varsayılan davranışı ASP.NET'te nedir?</span><span class="sxs-lookup"><span data-stu-id="2e6d3-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="2e6d3-108">Yeni bir site, ASP.NET Web sayfaları'nın eski bir sürümü ile çalışacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2e6d3-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="2e6d3-109">Bu makalede sunulan ASP.NET özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="2e6d3-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="2e6d3-110">`webPages:Version` Yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="2e6d3-111">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2e6d3-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="2e6d3-112">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2e6d3-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2e6d3-113">Bu öğreticide, ASP.NET Web sayfaları 1.0 ve ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="2e6d3-114">ASP.NET Web Pages Web siteleri yan yana çalıştırılması yeteneğini destekler.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="2e6d3-115">Bu, eski ASP.NET Web Pages uygulamalarınızı çalıştırmak için yeni ASP.NET Web Pages uygulamaları oluşturup bunların tümünün aynı bilgisayarda çalıştırma devam sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="2e6d3-116">WebMatrix ile Web sayfalarını yüklemek unutmayın gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2e6d3-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="2e6d3-117">Varsayılan olarak, bilgisayarınızda en son sürümü olarak mevcut Web Pages uygulamaları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="2e6d3-118">(Derlemeler genel derleme önbelleğinde (GAC) yüklü ve otomatik olarak kullanılır.)</span><span class="sxs-lookup"><span data-stu-id="2e6d3-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="2e6d3-119">Farklı bir sürümü, ASP.NET Web Pages kullanılarak bir site çalıştırmak istiyorsanız, bunu yapmak için siteyi yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="2e6d3-120">Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML'i dosyayı, var olan içeriğin üzerine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="2e6d3-121">Site zaten varsa bir *web.config* ekleyin bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="2e6d3-122">'-'Sürümünde belirtmezseniz *web.config* dosyası, bir siteyi en son sürümü olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="2e6d3-123">(Derlemeler kopyalanır *bin* klasöründe sitesi dağıtıldı.)</span><span class="sxs-lookup"><span data-stu-id="2e6d3-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="2e6d3-124">Web Matrix site şablonları kullanarak oluşturduğunuz yeni uygulamalar dahil Web sayfaları sürüm derlemeleri sitenin içinde *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="2e6d3-125">Genel olarak, her zaman uygun derlemelere sitenin yüklemek için NuGet kullanarak siteniz ile kullanmak için Web sayfaları hangi sürümünü denetleyebilir *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="2e6d3-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="2e6d3-126">Paketler için ziyaret edin [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="2e6d3-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e6d3-127">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2e6d3-127">Additional Resources</span></span>

[<span data-ttu-id="2e6d3-128">ASP.NET Web sayfaları 2'de en iyi Özellikler</span><span class="sxs-lookup"><span data-stu-id="2e6d3-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
