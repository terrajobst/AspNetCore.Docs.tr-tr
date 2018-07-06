---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı denetimi | Microsoft Docs'
author: rick-anderson
description: Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir. Örnek bir sayfa oluşturulur ve ardından O...
ms.author: aspnetcontent
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d069b7798d3659e9f6786fb8d63862817fbdd68b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808700"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="d8c32-104">[Bunu nasıl yaparım:] Bir ASP.NET sayfasının önbelleğe alınmasını özel bilgilere dayalı olarak denetleme</span><span class="sxs-lookup"><span data-stu-id="d8c32-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="d8c32-105">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d8c32-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d8c32-106">Bu video Chris piksel, özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma için ölçütleri denetlemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="d8c32-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="d8c32-107">Örnek bir sayfa oluşturulur ve ardından OutputCache yönergesi ile özel bir değer içeren VaryByCustom özniteliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d8c32-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="d8c32-108">Ardından, GetVaryCustomByString() yöntemi özel özniteliği işlenmesini sağlayan global.asax modülünde geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="d8c32-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="d8c32-109">Bu yöntemde, sayfanın önbelleğe alınmış sürümünü benzersiz olarak tanımlayan bir dize döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8c32-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="d8c32-110">Son olarak, nasıl özel bir değer kullanarak önbelleğe alma çeşitli yollarla bir web sitesi için kullanılabileceğini hakkında bir tartışma yoktur.</span><span class="sxs-lookup"><span data-stu-id="d8c32-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="d8c32-111">&#9654;(12 dakika) videosunu izleyin</span><span class="sxs-lookup"><span data-stu-id="d8c32-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
