---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı denetimi | Microsoft Docs'
author: rick-anderson
description: Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir. Örnek bir sayfa oluşturulur ve ardından O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376026"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="3cacb-104">[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı olarak denetleme</span><span class="sxs-lookup"><span data-stu-id="3cacb-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="3cacb-105">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3cacb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3cacb-106">Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="3cacb-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="3cacb-107">Örnek bir sayfa oluşturulur ve ardından OutputCache yönergesi ile özel bir değer içeren VaryByCustom özniteliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3cacb-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="3cacb-108">Ardından, GetVaryCustomByString() yöntemi özel özniteliği işlenmesini sağlayan global.asax modülünde geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="3cacb-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="3cacb-109">Bu yöntemde, sayfanın önbelleğe alınmış sürümünü benzersiz olarak tanımlayan bir dize döndürülür.</span><span class="sxs-lookup"><span data-stu-id="3cacb-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="3cacb-110">Son olarak, nasıl özel bir değer kullanarak önbelleğe alma çeşitli yollarla bir web sitesi için kullanılabileceğini hakkında bir tartışma yoktur.</span><span class="sxs-lookup"><span data-stu-id="3cacb-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="3cacb-111">&#9654;(12 dakika) videosunu izleyin</span><span class="sxs-lookup"><span data-stu-id="3cacb-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
