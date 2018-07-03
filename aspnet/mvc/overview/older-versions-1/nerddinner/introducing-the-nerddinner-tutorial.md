---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner Öğreticisine giriş | Microsoft Docs
author: shanselman
description: Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır. Bu öğreticide ASP.NE kullanarak küçük ancak tam bir uygulama oluşturma hakkında bilgi vermektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 735a775752beda3fc24852422b1ff9d60a7c46b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371634"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="113c2-104">NerdDinner Öğreticisine giriş</span><span class="sxs-lookup"><span data-stu-id="113c2-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="113c2-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="113c2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="113c2-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="113c2-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="113c2-107">Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="113c2-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="113c2-108">Bu öğretici, küçük bir derleme, ancak tamamlamak, ASP.NET MVC 1'i kullanarak uygulama hakkında bilgi vermektedir ve bazı ardındaki temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="113c2-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="113c2-109">ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.</span><span class="sxs-lookup"><span data-stu-id="113c2-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="113c2-110">NerdDinner Öğreticisine</span><span class="sxs-lookup"><span data-stu-id="113c2-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="113c2-111">Yeni bir çerçeve öğrenmenin en iyi yolu olan bir şey oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="113c2-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="113c2-112">Bu öğretici, küçük bir derleme, ancak tamamlamak, ASP.NET MVC kullanarak uygulama hakkında bilgi vermektedir ve bazı ardındaki temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="113c2-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="113c2-113">Uygulama oluşturmak için kullanacağız "NerdDinner" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="113c2-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="113c2-114">NerdDinner çevrimiçi azalma bulun ve bu kişiler için kolay bir yol sağlar:</span><span class="sxs-lookup"><span data-stu-id="113c2-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="113c2-115">NerdDinner kayıtlı kullanıcıların oluşturma, düzenleme ve silme azalma sağlar.</span><span class="sxs-lookup"><span data-stu-id="113c2-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="113c2-116">Uygulamanın tamamında tutarlı özellik kümesi doğrulama ve iş kuralları zorunlu kılar:</span><span class="sxs-lookup"><span data-stu-id="113c2-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="113c2-117">Ziyaretçiler, bunları tutulmakta yaklaşan azalma aramak için bir AJAX tabanlı eşleme kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="113c2-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="113c2-118">Bir Akşam Yemeği tıklayarak bunları ayrıntıları sayfasına nereden öğrenebilir hakkında daha fazla sürer:</span><span class="sxs-lookup"><span data-stu-id="113c2-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="113c2-119">Bunlar dinner katılan içinde ilgileniyorsanız oturum açma veya sitesine kaydolun:</span><span class="sxs-lookup"><span data-stu-id="113c2-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="113c2-120">Bunlar, ardından etkinliğine katılın için bir AJAX tabanlı RSVP bağlantısına tıklayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="113c2-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="113c2-121">NerdDinner uygulama</span><span class="sxs-lookup"><span data-stu-id="113c2-121">Implementing NerdDinner</span></span>

<span data-ttu-id="113c2-122">Dosya - kullanarak NerdDinner uygulamamız başlamak kullanacağız&gt;yepyeni bir ASP.NET MVC projesi oluşturmak için Visual Studio'da yeni proje komutu.</span><span class="sxs-lookup"><span data-stu-id="113c2-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="113c2-123">Ardından artımlı olarak işlevleri ve özellikleri ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="113c2-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="113c2-124">Süreç boyunca şu konulara değineceğiz:</span><span class="sxs-lookup"><span data-stu-id="113c2-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="113c2-125">Yeni bir ASP.NET MVC projesi oluşturma işlemini</span><span class="sxs-lookup"><span data-stu-id="113c2-125">How to create a new ASP.NET MVC Project</span></span>](# "yeni bir ASP.NET MVC projesi oluşturma")
2. [<span data-ttu-id="113c2-126">Bir veritabanı oluşturmak nasıl</span><span class="sxs-lookup"><span data-stu-id="113c2-126">How to create a database</span></span>](# "veritabanı oluşturma")
3. [<span data-ttu-id="113c2-127">İş kuralı doğrulamaları ile model oluşturma konusunda</span><span class="sxs-lookup"><span data-stu-id="113c2-127">How to build a model with business rule validations</span></span>](# "iş kuralı doğrulamaları ile Model oluşturma")
4. [<span data-ttu-id="113c2-128">Denetleyicileri ve görünümleri kullanarak listeleme/Ayrıntılar kullanıcı Arabirimi gerçekleştirmeyi nasıl</span><span class="sxs-lookup"><span data-stu-id="113c2-128">How to use controllers and views to implement a listing/details UI</span></span>](# "kullanın denetleyicileri ve görünümleri listeleme/Ayrıntılar kullanıcı Arabirimi uygulama")
5. <span data-ttu-id="113c2-129">[Nasıl CRUD (oluşturma, okuma, güncelleştirme ve silme) veri formu giriş desteği](# "sağlamak CRUD (oluşturma, okuma, güncelleştirme, silme) veri formu giriş desteği")</span><span class="sxs-lookup"><span data-stu-id="113c2-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="113c2-130">ViewData kullanma ve ViewModel sınıfları uygulama</span><span class="sxs-lookup"><span data-stu-id="113c2-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData kullanma ve ViewModel sınıfları uygulama")
7. [<span data-ttu-id="113c2-131">Ana sayfaları ve kısmi bölümleri kullanarak kullanıcı arabirimini yeniden kullanma</span><span class="sxs-lookup"><span data-stu-id="113c2-131">How to re-use UI using master pages and partials</span></span>](# "ana sayfaları kullanarak kullanıcı arabirimini yeniden kullanma ve kısmi bölümleri")
8. [<span data-ttu-id="113c2-132">Verimli veri sayfalama uygulama nasıl</span><span class="sxs-lookup"><span data-stu-id="113c2-132">How to implement efficient data paging</span></span>](# "uygulamak verimli veri sayfalama")
9. [<span data-ttu-id="113c2-133">Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini nasıl</span><span class="sxs-lookup"><span data-stu-id="113c2-133">How to secure applications using authentication and authorization</span></span>](# "güvenli uygulamaları kullanarak kimlik doğrulaması ve yetkilendirme")
10. [<span data-ttu-id="113c2-134">AJAX dinamik güncelleştirmeler sunma konusunda</span><span class="sxs-lookup"><span data-stu-id="113c2-134">How to use AJAX to deliver dynamic updates</span></span>](# "dinamik güncelleştirmelerini iletmek için AJAX kullanın")
11. [<span data-ttu-id="113c2-135">AJAX kullanarak eşleme senaryoları gerçekleştirmeyi nasıl</span><span class="sxs-lookup"><span data-stu-id="113c2-135">How to use AJAX to implement mapping scenarios</span></span>](# "kullanan AJAX eşleme senaryoları uygulama")
12. [<span data-ttu-id="113c2-136">Otomatik birim testi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="113c2-136">How to enable automated unit testing</span></span>](# "otomatik birim testi etkinleştir")

<span data-ttu-id="113c2-137">NerdDinner kopyasına oluşturabileceğinizi her tamamlayarak sıfırdan adım Biz bu bölümdeki gözden geçirme.</span><span class="sxs-lookup"><span data-stu-id="113c2-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="113c2-138">Alternatif olarak, kaynak kodu buraya tamamlanmış bir sürümünü indirebilirsiniz: [github'da NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="113c2-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="113c2-139">Ayrıca isteğe bağlı olarak ayrıca [Bu öğretici ücretsiz bir PDF sürümünü indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) öğretici çevrimdışı okumak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="113c2-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="113c2-140">Uygulamayı oluşturmak için Visual Studio 2008 ya da ücretsiz Visual Web Developer 2008 Express kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="113c2-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="113c2-141">Veritabanı için SQL Server veya ücretsiz SQL Server Express kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="113c2-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="113c2-142">ASP.NET MVC, Visual Web Developer 2008 Express ve SQL Server'ın V2 kullanarak Express (tüm ücretsiz) yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="113c2-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="113c2-143">Şimdi başlayalım...</span><span class="sxs-lookup"><span data-stu-id="113c2-143">Now let's get started....</span></span>

<span data-ttu-id="113c2-144">NerdDinner nedir ele aldığımız, şimdi bizim kollarınızı sıvayın ve biraz kod yazalım.</span><span class="sxs-lookup"><span data-stu-id="113c2-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="113c2-145">Şu dosya - kullanarak başlarsınız&gt;NerdDinner uygulama oluşturmak için Visual Studio'da yeni proje.</span><span class="sxs-lookup"><span data-stu-id="113c2-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="113c2-146">Next</span><span class="sxs-lookup"><span data-stu-id="113c2-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
