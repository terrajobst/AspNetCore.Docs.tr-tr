---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[Bunu nasıl yaparım:] Bir ASP.NET sayfasında HTML'yi değiştirilecek Reponse.Filter özelliğini kullanın. | Microsoft Docs"
author: rick-anderson
description: Bu video Chris piksel kesecek ve bir sayfaya gönderilmekte olan HTML alter Reponse.Filter özelliğini kullanmayı gösterir. İlk olarak, örnek bir sayfa w oluşturuldu...
ms.author: aspnetcontent
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: bf1936e5710b67185977f71bea03240e6c6abf41
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808109"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="42778-104">[Bunu nasıl yaparım:] Bir ASP.NET sayfasında HTML'yi değiştirilecek Reponse.Filter özelliğini kullanın</span><span class="sxs-lookup"><span data-stu-id="42778-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="42778-105">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="42778-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="42778-106">Bu video Chris piksel kesecek ve bir sayfaya gönderilmekte olan HTML alter Reponse.Filter özelliğini kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="42778-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="42778-107">İlk olarak, örnek bir sayfa bazı basit metinler ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="42778-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="42778-108">Ardından, kullanıcının tarayıcıya gönderilen geçerli akışı için değişiklik akışı olarak görev gören özel bir Stream sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="42778-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="42778-109">Bu özel akış sınıfta, değiştirilmiş ve ardından yanıt akışına yazılan akışından sayfasının içeriğini alınır.</span><span class="sxs-lookup"><span data-stu-id="42778-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="42778-110">Bu özel Stream sınıfta, böylece kullanıcının tarayıcıya gönderilen değiştirme HTML temel yanıt akışında değiştirmek için yazma yönteminin özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="42778-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="42778-111">Son olarak, Response.Filter özelliğinin sayfasında yeni akış sınıfı atandığı\_olay, böylece sayfa içeriğini değiştirme için bir mekanizma sunarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="42778-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="42778-112">&#9654;Videoyu (13 dakika)</span><span class="sxs-lookup"><span data-stu-id="42778-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
