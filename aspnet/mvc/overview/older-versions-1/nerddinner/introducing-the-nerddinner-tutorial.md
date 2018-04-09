---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner Öğreticisi giriş | Microsoft Docs
author: shanselman
description: Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır. Bu öğreticide ASP.NE kullanarak küçük, ancak tam, bir uygulama oluşturmak nasıl aracılığıyla açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="d085a-104">NerdDinner Öğreticisi Tanıtımı</span><span class="sxs-lookup"><span data-stu-id="d085a-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="d085a-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d085a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="d085a-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="d085a-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d085a-107">Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d085a-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="d085a-108">Bu öğretici, küçük bir yapı ancak tamamlanması, ASP.NET MVC 1'i kullanarak uygulama nasıl aracılığıyla anlatılmaktadır ve bazı arkasındaki temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="d085a-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="d085a-109">ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="d085a-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="d085a-110">NerdDinner Öğreticisi</span><span class="sxs-lookup"><span data-stu-id="d085a-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="d085a-111">Yeni bir çerçeve öğrenmenin en iyi yolu onunla bir şeyler oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d085a-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="d085a-112">Bu öğretici, küçük bir yapı ancak tamamlanması, ASP.NET MVC kullanarak uygulamayı nasıl aracılığıyla anlatılmaktadır ve bazı arkasındaki temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="d085a-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="d085a-113">Biz oluşturmak için uygulama "NerdDinner" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="d085a-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="d085a-114">NerdDinner bulmak ve çevrimiçi azalma düzenlemek kişiler için kolay bir yol sağlar:</span><span class="sxs-lookup"><span data-stu-id="d085a-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="d085a-115">NerdDinner kayıtlı kullanıcıların oluşturmak, düzenlemek ve azalma silmek olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d085a-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="d085a-116">Doğrulama ve iş kuralları tutarlı bir dizi uygulama zorunlu kılar:</span><span class="sxs-lookup"><span data-stu-id="d085a-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="d085a-117">Ziyaretçilerin bunları tutulan yaklaşan azalma aramak için bir AJAX tabanlı eşlemesi kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d085a-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="d085a-118">Bir Yemeği tıklayarak bunları ayrıntıları sayfasına nereden öğrenebilir hakkında daha fazla sürer:</span><span class="sxs-lookup"><span data-stu-id="d085a-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="d085a-119">Bunlar Yemeği katılan ilgileniyorsanız oturum açamaz veya sitesinde kaydedin:</span><span class="sxs-lookup"><span data-stu-id="d085a-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="d085a-120">Bunlar daha sonra olay katılmak için bir AJAX tabanlı RSVP bağlantıyı tıklatın:</span><span class="sxs-lookup"><span data-stu-id="d085a-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="d085a-121">Implementing NerdDinner</span><span class="sxs-lookup"><span data-stu-id="d085a-121">Implementing NerdDinner</span></span>

<span data-ttu-id="d085a-122">Dosya - kullanarak NerdDinner uygulamamız başlamaya kalacaklarını&gt;yepyeni bir ASP.NET MVC projesini oluşturmak için Visual Studio içindeki yeni proje komutu.</span><span class="sxs-lookup"><span data-stu-id="d085a-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="d085a-123">Ardından artımlı olarak işlev ve özellikleri ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="d085a-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="d085a-124">Yol boyunca şu konulara değineceğiz:</span><span class="sxs-lookup"><span data-stu-id="d085a-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="d085a-125">Yeni bir ASP.NET MVC projesi oluşturmak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-125">How to create a new ASP.NET MVC Project</span></span>](# "yeni bir ASP.NET MVC projesi oluşturma")
2. [<span data-ttu-id="d085a-126">Bir veritabanı oluşturmak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-126">How to create a database</span></span>](# "bir veritabanı oluşturun")
3. [<span data-ttu-id="d085a-127">İş kuralı doğrulamaları ile bir model oluşturmak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-127">How to build a model with business rule validations</span></span>](# "iş kuralı doğrulamaları ile bir Model oluşturma")
4. [<span data-ttu-id="d085a-128">Denetleyicileri ve görünümleri listeleme/Ayrıntılar UI uygulamak için nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="d085a-128">How to use controllers and views to implement a listing/details UI</span></span>](# "denetleyicileri kullanın ve görünümleri listeleme/Ayrıntılar UI uygulamak için")
5. <span data-ttu-id="d085a-129">[CRUD sağlamak nasıl (oluşturma, okuma, güncelleştirme, silme) veri form girişi Destek](# "sağlamak CRUD (oluşturma, okuma, güncelleştirme, silme) veri Form girişini destekler")</span><span class="sxs-lookup"><span data-stu-id="d085a-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="d085a-130">ViewData kullanın ve ViewModel sınıfları uygulamak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData kullanın ve uygulama ViewModel sınıfları")
7. [<span data-ttu-id="d085a-131">Ana sayfalar ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanmak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-131">How to re-use UI using master pages and partials</span></span>](# "ana sayfa kullanarak kullanıcı Arabirimi yeniden kullanma ve kısmi")
8. [<span data-ttu-id="d085a-132">Verimli veri Sayfalaması uygulamak üzere nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-132">How to implement efficient data paging</span></span>](# "uygulamak verimli veri disk belleği")
9. [<span data-ttu-id="d085a-133">Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini sağlamak nasıl</span><span class="sxs-lookup"><span data-stu-id="d085a-133">How to secure applications using authentication and authorization</span></span>](# "güvenli uygulamaları kullanarak kimlik doğrulaması ve yetkilendirme")
10. [<span data-ttu-id="d085a-134">AJAX dinamik güncelleştirmelerini göndermek için nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="d085a-134">How to use AJAX to deliver dynamic updates</span></span>](# "dinamik güncelleştirmeleri sunmak için AJAX'ı kullanın")
11. [<span data-ttu-id="d085a-135">AJAX eşleme senaryolar uygulamak için nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="d085a-135">How to use AJAX to implement mapping scenarios</span></span>](# "eşleme senaryolar uygulamak için AJAX'ı kullanın")
12. [<span data-ttu-id="d085a-136">Otomatik birim testi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="d085a-136">How to enable automated unit testing</span></span>](# "otomatik birim testi etkinleştir")

<span data-ttu-id="d085a-137">Kendi NerdDinner kopyasını oluşturabilirsiniz her tamamlayarak sıfırdan adım Biz bu bölümdeki gözden geçirme.</span><span class="sxs-lookup"><span data-stu-id="d085a-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="d085a-138">Alternatif olarak, kaynak kodu buraya tamamlanmış bir sürümünü yükleyebilirsiniz: [github'da NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="d085a-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="d085a-139">Ayrıca isteğe bağlı olarak kullanabileceğiniz [Bu öğretici ücretsiz bir PDF sürümünü yükleyin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) öğretici çevrimdışı okumak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="d085a-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="d085a-140">Uygulamanızı oluşturmak için Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d085a-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="d085a-141">Veritabanı için SQL Server veya ücretsiz SQL Server Express kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d085a-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="d085a-142">ASP.NET MVC, Visual Web Developer 2008 Express ve SQL Server, V2 kullanarak Express (tüm ücretsiz) yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="d085a-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="d085a-143">Şimdi başlayalım...</span><span class="sxs-lookup"><span data-stu-id="d085a-143">Now let's get started....</span></span>

<span data-ttu-id="d085a-144">Biz NerdDinner nedir kapsamına, şimdi bizim kollarýný attıysanız ve biraz kod yazalım.</span><span class="sxs-lookup"><span data-stu-id="d085a-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="d085a-145">Biz dosya - kullanarak başlarsınız&gt;NerdDinner uygulaması oluşturmak için Visual Studio'da yeni proje.</span><span class="sxs-lookup"><span data-stu-id="d085a-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d085a-146">Next</span><span class="sxs-lookup"><span data-stu-id="d085a-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
