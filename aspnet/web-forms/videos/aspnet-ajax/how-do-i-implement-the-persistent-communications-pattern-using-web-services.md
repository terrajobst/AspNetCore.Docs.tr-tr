---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Bunu nasıl yaparım:] Web hizmetlerini kullanarak kalıcı iletişim desenini uygulansın mı? | Microsoft Docs'
author: JoeStagner
description: Geleneksel bir Web sitesinde tarayıcı ve sunucu bir devam eden iletişimleri korumak değil, ancak yalnızca bir eylemi gerçekleştiren kullanıcı yanıtta iletişim...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/22/2007
ms.topic: article
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: 8d7aac37b3b5b47f0533454f2d1d6f1f8677af99
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367480"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="6693e-104">[Bunu nasıl yaparım:] Web hizmetlerini kullanarak kalıcı iletişim desenini uygulansın mı?</span><span class="sxs-lookup"><span data-stu-id="6693e-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="6693e-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6693e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="6693e-106">Geleneksel bir Web sitesinde tarayıcı ve sunucu bir devam eden iletişimleri korumak değil, ancak yalnızca bir eylem gerçekleştiren kullanıcı yanıtta iletişim.</span><span class="sxs-lookup"><span data-stu-id="6693e-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="6693e-107">Modern Web burada da bir uygulama kapsayıcısı sayfa olur sitesinde, tarayıcı ve Sayfa güncelleştirmelerini eylemi gerçekleştiren kullanıcı gerçekleştirilmesi bir devam eden iletişimleri korumak üzere sunucusu için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6693e-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="6693e-108">Bu AJAX için kalıcı iletişim desenini olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="6693e-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="6693e-109">ASP.NET AJAX Web geliştiricilerin kalıcı iletişim desenini uygulama başlıca iki yolu sağlar.</span><span class="sxs-lookup"><span data-stu-id="6693e-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="6693e-110">Önceki bir videoda ASP.NET AJAX UpdatePanel uygulama temeli olarak kullanmak nasıl gördük.</span><span class="sxs-lookup"><span data-stu-id="6693e-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="6693e-111">Bu videoda ASP.NET AJAX UpdatePanel gereksinimini ortadan kaldırır bir Web hizmeti JavaScrpt çağrısı kullanarak aynı düzeni nasıl uygulayacağınıza karar biz öğrenin.</span><span class="sxs-lookup"><span data-stu-id="6693e-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="6693e-112">&#9654;Videoyu (16 dakika)</span><span class="sxs-lookup"><span data-stu-id="6693e-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="6693e-113">[Önceki](how-do-i-localize-an-aspnet-ajax-application.md)
> [İleri](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="6693e-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
