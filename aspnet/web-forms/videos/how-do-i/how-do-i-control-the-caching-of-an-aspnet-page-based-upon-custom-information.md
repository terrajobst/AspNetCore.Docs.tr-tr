---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Nasıl stop yaparım] Bir ASP.NET sayfasının önbelleğe alma dayalı özel bilgilerine bağlı denetimi | Microsoft Docs'
author: rick-anderson
description: Bu video Chris Pels özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma ölçütlerini denetleme gösterir. Bir örnek sayfa oluşturulur ve ardından Hayır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572745"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="16eec-104">[Nasıl stop yaparım] Bir ASP.NET sayfasının önbelleğe alma özel bilgilere dayalı denetimi</span><span class="sxs-lookup"><span data-stu-id="16eec-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="16eec-105">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="16eec-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="16eec-106">Bu video Chris Pels özel bilgilere dayalı bir ASP.NET sayfasının önbelleğe alma ölçütlerini denetleme gösterir.</span><span class="sxs-lookup"><span data-stu-id="16eec-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="16eec-107">Bir örnek sayfa oluşturulur ve ardından OutputCache yönergesi özel bir değer içeren VaryByCustom özniteliği ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16eec-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="16eec-108">Ardından, GetVaryCustomByString() yöntemi özel öznitelik işlenmesini sağlayan global.asax modülünde geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="16eec-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="16eec-109">Bu yöntem, sayfanın önbelleğe alınmış sürümünü benzersiz olarak tanımlayan bir dize döndürülür.</span><span class="sxs-lookup"><span data-stu-id="16eec-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="16eec-110">Son olarak, nasıl özel bir değer kullanarak önbelleğe alma çeşitli yollarla bir web sitesi için kullanılabileceği hakkında tartışma yoktur.</span><span class="sxs-lookup"><span data-stu-id="16eec-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="16eec-111">&#9654; (12 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="16eec-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
