---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Nasıl stop yaparım]  Bir ASP.NET sayfası, HTTP üst bilgisinde dayalı önbellek | Microsoft Docs"
author: rick-anderson
description: "Bu video Chris Pels bilgi sayfanın HTTP üst bilgisinde temel ASP.NET çıktı önbelleğine bir sayfa tutmak gösterilmektedir. Birinci, olası HTTP üst..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="4ba68-104">[Nasıl stop yaparım]  Bir ASP.NET sayfasının HTTP üst bilgileri temel önbelleği</span><span class="sxs-lookup"><span data-stu-id="4ba68-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="4ba68-105">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="4ba68-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="4ba68-106">Bu video Chris Pels bilgi sayfanın HTTP üst bilgisinde temel ASP.NET çıktı önbelleğine bir sayfa tutmak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4ba68-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="4ba68-107">İlk olarak, olası HTTP üstbilgi değerleri incelenen.</span><span class="sxs-lookup"><span data-stu-id="4ba68-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="4ba68-108">Ardından, bir örnek sayfa oluşturulur ve OutputCache yönergesi "kabul-language", kullanıcının tarayıcısının dile bağlı tabanlı önbelleğe almayı denetlemek için bir HTTP üstbilgisi değeri içeren VaryByHeader özniteliğiyle sonra kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba68-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="4ba68-109">Örnek sayfasına İngilizce'ye ayarlanır IE ve ardından, Fransızca kullanmak üzere ayarlanan FireFox görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4ba68-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="4ba68-110">Son olarak, web.config dosyasında CacheProfile önbellek tanımı gitme seçeneği ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4ba68-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="4ba68-111">&#9654; (12 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="4ba68-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
