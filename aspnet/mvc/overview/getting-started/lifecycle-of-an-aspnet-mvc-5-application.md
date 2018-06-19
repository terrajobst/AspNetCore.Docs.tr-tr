---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Bir ASP.NET MVC 5 uygulama yaşam döngüsü | Microsoft Docs
author: cephalin
description: Bir ASP.NET MVC 5 uygulama yaşam döngüsü grafikleri bir PDF belgesini indirin. Bu yaşam döngüsü belge MVC yaşam döngüsü üst düzey bir görünümünü sunan bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036498"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="78fb0-104">Bir ASP.NET MVC 5 uygulama yaşam döngüsü</span><span class="sxs-lookup"><span data-stu-id="78fb0-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="78fb0-105">tarafından [Cephas Bağla](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="78fb0-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="78fb0-106">PDF belgesini indirin</span><span class="sxs-lookup"><span data-stu-id="78fb0-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="78fb0-107">Burada HTTP almasını her ASP.NET MVC 5 uygulama yaşam döngüsü İstek HTTP yanıtı göndermeyi grafikleri istemciye bir PDF belgesini indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78fb0-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="78fb0-108">ASP.NET MVC için yeni olanlar için eğitim bir aracı hem de ayrıca olarak bir başvuru uygulama belirli yönlerini incelemek için ihtiyacınız olanlar için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="78fb0-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="78fb0-109">PDF belgesini aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="78fb0-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="78fb0-110">İlgili [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) MVC tümleştirir burada anlamanıza yardımcı olması için aşamaları [ASP.NET uygulama yaşam döngüsü](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="78fb0-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="78fb0-111">İstek işleme ardışık düzeninde her MVC uygulaması geçtiği önemli aşamalar burada anlayabileceği MVC uygulama yaşam döngüsü, üst düzey görünümü.</span><span class="sxs-lookup"><span data-stu-id="78fb0-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="78fb0-112">İstek işleme ardışık düzenine ayrıntılarını aşağı ayrıntısına gösterir ayrıntı görünümü.</span><span class="sxs-lookup"><span data-stu-id="78fb0-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="78fb0-113">Üst düzey görünümü ve yaşam döngüleri ayrıntıları çeşitli aşamaları nasıl toplanır görmek için ayrıntı görünümü karşılaştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78fb0-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="78fb0-114">[PDF'yi indirmek](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) daha büyük bir görmek için.</span><span class="sxs-lookup"><span data-stu-id="78fb0-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="78fb0-115">Yerleştirme ve tüm geçersiz kılınabilir yöntemleri amacı [denetleyicisi](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) istek işleme ardışık düzeninde nesnesi.</span><span class="sxs-lookup"><span data-stu-id="78fb0-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="78fb0-116">Herhangi bir yöntemini geçersiz kılın gerek olmayabilir veya olabilir, ancak düşündüğünüz etkisi için uygun yaşam döngüsü aşamada kod yazabilirsiniz uygulama yaşam döngüsü içindeki rollerine anlaşılması önemlidir.</span><span class="sxs-lookup"><span data-stu-id="78fb0-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="78fb0-117">Her filtre türü (kimlik doğrulama, yetkilendirme, eylem ve sonuç) nasıl çağrıldığını gösteren attı yukarı diyagramları.</span><span class="sxs-lookup"><span data-stu-id="78fb0-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="78fb0-118">Ayrıntılar görünümünde ilgilendiğiniz her noktasından bir yararlı makale veya blog bağlayın.</span><span class="sxs-lookup"><span data-stu-id="78fb0-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="78fb0-119">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="78fb0-119">Next Steps</span></span>

<span data-ttu-id="78fb0-120">Bu belge, gereksinimini karşılıyor mu?</span><span class="sxs-lookup"><span data-stu-id="78fb0-120">Does this document meet your need?</span></span> <span data-ttu-id="78fb0-121">Görüşleriniz bizim için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="78fb0-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="78fb0-122">Uygulamanızda, ASP.NET MVC yaşam döngüsü herhangi bir sorunuz varsa [Stackoverflow](http://stackoverflow.com/help) ve [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) sormak için harika yerlerdir.</span><span class="sxs-lookup"><span data-stu-id="78fb0-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="78fb0-123">İzleyin [bana](https://twitter.com/Cephas_MSFT) my son öğreticileri güncelleştirmeleri alabilmek için Twitter'da.</span><span class="sxs-lookup"><span data-stu-id="78fb0-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
