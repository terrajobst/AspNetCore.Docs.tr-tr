---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Nasıl yapılır: bir MVC uygulaması için özel HTML Yardımcısı oluşturma? | Microsoft Docs'
author: rick-anderson
description: Bu videoda, Chris piksel MVC uygulamasındaki standart kümesinde kullanılabilir değil özel bir HtmlHelper oluşturma işlemi gösterilmektedir. İlk olarak, bir örnek MVC uygulana...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 4061c06cfeab2278e5732295b034f81f7995c2a4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752950"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="a612a-105">Nasıl yapılır: bir MVC uygulaması için özel HTML Yardımcısı oluşturma?</span><span class="sxs-lookup"><span data-stu-id="a612a-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="a612a-106">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a612a-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a612a-107">Bu videoda, Chris piksel MVC uygulamasındaki standart kümesinde kullanılabilir değil özel bir HtmlHelper oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a612a-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="a612a-108">İlk olarak, örnek bir MVC uygulaması bir tanıtım denetleyici ve görünüm özel HtmlHelper test etmek için ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a612a-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="a612a-109">Ardından, bir modül olan özel HtmlHelper uygulamasını temsil eden bir genişletme yöntemi genel bir işlev oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a612a-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="a612a-110">Oluşturmak için özel yardımcı olan `<img>` etiketleri bir sayfasında ve kimliği, url ve resim etiketi için alternatif metin dahil olmak üzere birkaç gelen parametre alır.</span><span class="sxs-lookup"><span data-stu-id="a612a-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="a612a-111">Mantıksal tamamlanmış döndürmek için işleve eklenir `<img>` belirtilen bilgileri içeren bir etiketi.</span><span class="sxs-lookup"><span data-stu-id="a612a-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="a612a-112">Ardından özel HtmlHelper tanıtım sayfasında resim görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a612a-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="a612a-113">Son olarak, özel HtmlHelper daha kolayca farklı oluşturmak için esneklik sağlayan birden çok Oluşturucu geçersiz kılmaları içerecek şekilde Genişletilmiş `<img>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="a612a-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="a612a-114">&#9654;Videoyu (18 dakika)</span><span class="sxs-lookup"><span data-stu-id="a612a-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="a612a-115">[Önceki](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [İleri](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="a612a-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
