---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[Nasıl stop yaparım] Bir ASP.NET sayfasının HTML değiştirmek için Reponse.Filter özelliğini kullanın | Microsoft Docs"
author: rick-anderson
description: "Bu video Chris Pels Reponse.Filter özelliğinin bir sayfaya gönderilen HTML kesecek şekilde nasıl kullanılacağını gösterir. İlk olarak, bir örnek sayfa w oluşturulur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="a4cca-104">[Nasıl stop yaparım] Bir ASP.NET sayfasının HTML değiştirmek için Reponse.Filter özelliğini kullanın</span><span class="sxs-lookup"><span data-stu-id="a4cca-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="a4cca-105">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a4cca-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a4cca-106">Bu video Chris Pels Reponse.Filter özelliğinin bir sayfaya gönderilen HTML kesecek şekilde nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4cca-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="a4cca-107">İlk olarak, bir örnek sayfa bazı basit metinler ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4cca-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="a4cca-108">Ardından, özel bir akış sınıf hangi kullanıcının tarayıcıya gönderilen geçerli akış için değiştirme akış görevi gören oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4cca-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="a4cca-109">Bu özel akış sınıfta sayfasının içeriği değiştirilmiş ve ardından yanıt akışına yazılan akışından alınır.</span><span class="sxs-lookup"><span data-stu-id="a4cca-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="a4cca-110">Bu özel akış sınıfında, böylece kullanıcının tarayıcıya gönderilen değiştirilmesine HTML temel yanıt akışında değiştirmek için yazma yönteminin özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4cca-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="a4cca-111">Son olarak, yeni stream sınıfı sayfadaki Response.Filter özelliğini atanmış\_olay, böylelikle, sayfa içeriği değiştirmeye yönelik bir mekanizma sağlamadan yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a4cca-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="a4cca-112">&#9654; (13 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="a4cca-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
