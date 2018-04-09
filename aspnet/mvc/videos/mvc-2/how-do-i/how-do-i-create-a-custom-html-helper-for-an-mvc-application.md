---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'I: bir MVC uygulaması için özel HTML Yardımcısı nasıl oluşturulur? | Microsoft Docs'
author: rick-anderson
description: Bu videoda Chris Pels MVC uygulamasındaki standart kümesindeki kullanılamıyor özel bir HtmlHelper oluşturulacağını gösterir. İlk olarak, bir örnek MVC applica...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="67587-105">I: bir MVC uygulaması için özel HTML Yardımcısı nasıl oluşturulur?</span><span class="sxs-lookup"><span data-stu-id="67587-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="67587-106">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="67587-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="67587-107">Bu videoda Chris Pels MVC uygulamasındaki standart kümesindeki kullanılamıyor özel bir HtmlHelper oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="67587-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="67587-108">İlk olarak, bir örnek MVC uygulaması bir tanıtım denetleyici ve görünüm özel HtmlHelper test etmek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="67587-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="67587-109">Ardından, bir modül özel HtmlHelper uyarlamasını temsil eden bir genişletme yöntemi olduğu için ortak bir işlev oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="67587-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="67587-110">Oluşturmak için özel yardımcı olan `<img>` etiketleri içindeki bir sayfayı ve kimliği, url ve resim etiketi için alternatif metin dahil olmak üzere birkaç gelen parametreleri alır.</span><span class="sxs-lookup"><span data-stu-id="67587-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="67587-111">Mantığı sonra tamamlanmış döndürmek için işlevi eklenen `<img>` belirtilen bilgileri içeren etiketi.</span><span class="sxs-lookup"><span data-stu-id="67587-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="67587-112">Ardından özel HtmlHelper demo sayfasında görüntüyü görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="67587-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="67587-113">Daha fazla bilgi için kolayca farklı oluşturma esneklik sağlayan birden çok Oluşturucusu geçersiz kılmaları kapsayacak şekilde özel HtmlHelper son olarak, Genişletilmiş `<img>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="67587-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="67587-114">&#9654;(18 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="67587-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="67587-115">[Önceki](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [sonraki](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="67587-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
