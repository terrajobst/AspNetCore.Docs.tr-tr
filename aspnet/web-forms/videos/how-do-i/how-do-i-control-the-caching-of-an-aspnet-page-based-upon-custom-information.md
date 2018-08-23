---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı denetimi | Microsoft Docs'
author: rick-anderson
description: Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir. Örnek bir sayfa oluşturulur ve ardından O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756773"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="450ed-104">[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı olarak denetleme</span><span class="sxs-lookup"><span data-stu-id="450ed-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="450ed-105">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="450ed-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="450ed-106">Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="450ed-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="450ed-107">Örnek bir sayfa oluşturulur ve ardından OutputCache yönergesi ile özel bir değer içeren VaryByCustom özniteliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="450ed-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="450ed-108">Ardından, GetVaryCustomByString() yöntemi özel özniteliği işlenmesini sağlayan global.asax modülünde geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="450ed-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="450ed-109">Bu yöntemde, sayfanın önbelleğe alınmış sürümünü benzersiz olarak tanımlayan bir dize döndürülür.</span><span class="sxs-lookup"><span data-stu-id="450ed-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="450ed-110">Son olarak, nasıl özel bir değer kullanarak önbelleğe alma çeşitli yollarla bir web sitesi için kullanılabileceğini hakkında bir tartışma yoktur.</span><span class="sxs-lookup"><span data-stu-id="450ed-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="450ed-111">&#9654;(12 dakika) videosunu izleyin</span><span class="sxs-lookup"><span data-stu-id="450ed-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
