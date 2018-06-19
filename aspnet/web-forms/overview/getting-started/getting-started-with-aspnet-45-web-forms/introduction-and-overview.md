---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama | Microsoft Docs
author: Erikre
description: Bu adım adım öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: a3527b54d1936bc14e32a1828ac3a2be625107ba
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32078612"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="9c491-103">ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9c491-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="9c491-104">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="9c491-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="9c491-105">[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="9c491-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="9c491-106">Bu adım adım öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="9c491-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="9c491-107">Test ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="9c491-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="9c491-108">Bilginiz test edin ve ASP.NET Web Forms test gerçekleştirerek temel kavramları pekiştirmek.</span><span class="sxs-lookup"><span data-stu-id="9c491-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="9c491-109">Bu test, Bu öğretici serisinde bulunan içerikten özel olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9c491-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="9c491-110">Test her soruyu ek yönergelere bağlantılar ile birlikte bir açıklama sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c491-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="9c491-111">Giriş</span><span class="sxs-lookup"><span data-stu-id="9c491-111">Introduction</span></span>

<span data-ttu-id="9c491-112">Bu öğreticiler dizi Web ve ASP.NET 4.5 için Visual Studio Express 2013 kullanarak bir ASP.NET Web Forms uygulama oluşturmak için gereken adımlarda size kılavuzluk eder.</span><span class="sxs-lookup"><span data-stu-id="9c491-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="9c491-113">Oluşturacaksınız uygulama adlı **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="9c491-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="9c491-114">Çevrimiçi öğeleri sattığı deposu ön web sitesi basitleştirilmiş bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="9c491-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="9c491-115">Bu öğretici seri ASP.NET 4.5 içinde kullanılabilen yeni özellikleri vurgular.</span><span class="sxs-lookup"><span data-stu-id="9c491-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="9c491-116">Yorumlar Hoş Geldiniz ve, öneriler temelinde Bu öğretici seri güncelleştirmek için her türlü çabayı hale getireceğiz.</span><span class="sxs-lookup"><span data-stu-id="9c491-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="9c491-117">Yükleme Tamamlandı proje</span><span class="sxs-lookup"><span data-stu-id="9c491-117">Download completed project</span></span>

<span data-ttu-id="9c491-118">Tamamlanan öğretici içeren bir C# projesi indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="9c491-119">[ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys Başlarken](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="9c491-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="9c491-120">İlgili ASP.NET Web Forms test gerçekleştirerek içeriği gözden geçir</span><span class="sxs-lookup"><span data-stu-id="9c491-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="9c491-121">Bu öğreticiyi tamamladıktan sonra bilginizi sınayın ve temel kavramları gerçekleştirerek pekiştirmek [ASP.NET Web Forms test](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="9c491-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="9c491-122">Bu test, Bu öğretici serisinde bulunan içerikten özel olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9c491-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="9c491-123">Test her soruyu ek yönergelere bağlantılar ile birlikte bir açıklama sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c491-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="9c491-124">Test ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="9c491-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="9c491-125">Hedef Kitle</span><span class="sxs-lookup"><span data-stu-id="9c491-125">Audience</span></span>

<span data-ttu-id="9c491-126">Bu öğretici dizisinin hedef kitle ASP.NET Web formları için yeni olan deneyimli geliştiriciler ' dir.</span><span class="sxs-lookup"><span data-stu-id="9c491-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="9c491-127">Bu öğretici serisinde ilgilenen bir geliştirici, aşağıdaki yetenekleri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="9c491-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="9c491-128">Tanıdık bir nesne odaklı programlama (OOP) dil</span><span class="sxs-lookup"><span data-stu-id="9c491-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="9c491-129">Tanıdık Web geliştirme kavramları (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="9c491-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="9c491-130">İlişkisel veritabanı kavramları hakkında bilgi sahibi</span><span class="sxs-lookup"><span data-stu-id="9c491-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="9c491-131">N katmanlı mimari kavramlarını aşina</span><span class="sxs-lookup"><span data-stu-id="9c491-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="9c491-132">Yukarıda listelenen alanlarını gözden geçirme ilgileniyorsanız, aşağıdaki içeriği gözden geçirme göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="9c491-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="9c491-133">Visual C# kullanmaya Başlarken</span><span class="sxs-lookup"><span data-stu-id="9c491-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="9c491-134">[Web geliştirme](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="9c491-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="9c491-135">İlişkisel veritabanı</span><span class="sxs-lookup"><span data-stu-id="9c491-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="9c491-136">Çok katmanlı mimarisi</span><span class="sxs-lookup"><span data-stu-id="9c491-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="9c491-137">Uygulama özellikleri</span><span class="sxs-lookup"><span data-stu-id="9c491-137">Application Features</span></span>

<span data-ttu-id="9c491-138">Bu serideki sunulan ASP.NET Web formu özellikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="9c491-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="9c491-139">Web uygulaması projesi (Web sitesi projesini değil)</span><span class="sxs-lookup"><span data-stu-id="9c491-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="9c491-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="9c491-140">Web Forms</span></span>
- <span data-ttu-id="9c491-141">Ana sayfalar, yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9c491-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="9c491-142">önyükleme</span><span class="sxs-lookup"><span data-stu-id="9c491-142">Bootstrap</span></span>
- <span data-ttu-id="9c491-143">Entity Framework kod ilk olarak, yerel veritabanı</span><span class="sxs-lookup"><span data-stu-id="9c491-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="9c491-144">İstek doğrulama</span><span class="sxs-lookup"><span data-stu-id="9c491-144">Request Validation</span></span>
- <span data-ttu-id="9c491-145">Bağlama, veri ek açıklamaları Model ve sağlayıcıları değer kesin türü belirtilmiş veri denetimleri</span><span class="sxs-lookup"><span data-stu-id="9c491-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="9c491-146">SSL ve OAuth</span><span class="sxs-lookup"><span data-stu-id="9c491-146">SSL and OAuth</span></span>
- <span data-ttu-id="9c491-147">ASP.NET kimliği, yapılandırma ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="9c491-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="9c491-148">Örtük doğrulama</span><span class="sxs-lookup"><span data-stu-id="9c491-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="9c491-149">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="9c491-149">Routing</span></span>
- <span data-ttu-id="9c491-150">ASP.NET hata işleme</span><span class="sxs-lookup"><span data-stu-id="9c491-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="9c491-151">Uygulama senaryolar ve görevler</span><span class="sxs-lookup"><span data-stu-id="9c491-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="9c491-152">Bu serideki gösterilen görevler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="9c491-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="9c491-153">Oluşturma, gözden geçirme ve yeni projesi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9c491-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="9c491-154">Veritabanı yapısını oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c491-154">Creating the database structure</span></span>
- <span data-ttu-id="9c491-155">Başlatma ve veritabanı üretme</span><span class="sxs-lookup"><span data-stu-id="9c491-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="9c491-156">Stiller, grafik ve bir ana sayfa kullanarak kullanıcı Arabirimi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="9c491-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="9c491-157">Sayfalar ve gezinti ekleme</span><span class="sxs-lookup"><span data-stu-id="9c491-157">Adding pages and navigation</span></span>
- <span data-ttu-id="9c491-158">Menü ayrıntıları ve ürün verileri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="9c491-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="9c491-159">Alışveriş sepeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c491-159">Creating a shopping cart</span></span>
- <span data-ttu-id="9c491-160">Ekleme SSL ve OAuth desteği</span><span class="sxs-lookup"><span data-stu-id="9c491-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="9c491-161">Bir ödeme yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="9c491-161">Adding a payment method</span></span>
- <span data-ttu-id="9c491-162">Bir yönetici rolü ve bir kullanıcı uygulama da dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="9c491-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="9c491-163">Belirli sayfalara ve klasörüne erişimi kısıtlama</span><span class="sxs-lookup"><span data-stu-id="9c491-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="9c491-164">Web uygulaması için bir dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="9c491-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="9c491-165">Giriş Doğrulaması uygulama</span><span class="sxs-lookup"><span data-stu-id="9c491-165">Implementing input validation</span></span>
- <span data-ttu-id="9c491-166">Web uygulaması için yolları kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="9c491-166">Registering routes for the web application</span></span>
- <span data-ttu-id="9c491-167">Hata işleme ve hata günlüğünü uygulama</span><span class="sxs-lookup"><span data-stu-id="9c491-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="9c491-168">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9c491-168">Overview</span></span>

<span data-ttu-id="9c491-169">ASP.NET Web formları için yeniyseniz sağ öğretici sahip, ancak programlama kavramları ile benzerlik sahip.</span><span class="sxs-lookup"><span data-stu-id="9c491-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="9c491-170">ASP.NET Web Forms bilginiz varsa, ASP.NET 4.5 içinde kullanılabilen yeni özellikleri tarafından Bu öğretici serisinde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9c491-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="9c491-171">Programlama kavramları ve ASP.NET Web Forms bilginiz yoksa Web formları sağlanan ek öğreticilere bakın [Başlarken](../../../index.md) bölümü ASP.NET Web sitesinde.</span><span class="sxs-lookup"><span data-stu-id="9c491-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="9c491-172">Belirli **son** ASP.NET 4.5 özellikleri sağlanan bu Web Forms öğretici serisi şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="9c491-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="9c491-173">Bu teklif projeleri oluşturmak için basit bir kullanıcı Arabirimi [desteklemek için birden çok ASP.NET çerçeveyi](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC ve Web API).</span><span class="sxs-lookup"><span data-stu-id="9c491-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="9c491-174">[Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), esnek tasarım ve tema özellikleri sağlayan bir düzen ve tema framework.</span><span class="sxs-lookup"><span data-stu-id="9c491-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="9c491-175">[ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveler ve çalışan aynı web barındırma IIS dışında yazılım ile çalışan yeni bir ASP.NET üyelik sistemi.</span><span class="sxs-lookup"><span data-stu-id="9c491-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="9c491-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), bir güncelleştirme almak ve veri türü kesin olarak değiştirmek sağlayan Entity Framework nesneleri yazılan, erişim verilerini zaman uyumsuz olarak geçici bağlantı hataları işlemek ve oturum SQL deyimleri.</span><span class="sxs-lookup"><span data-stu-id="9c491-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="9c491-177">ASP.NET 4.5 özelliklerin tam listesi için bkz: [ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları için](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="9c491-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="9c491-178">Wingtip Toys örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="9c491-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="9c491-179">Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturacak ASP.NET Web forms uygulaması hızlı bir görünümünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c491-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="9c491-180">Uygulama için Visual Studio Express 2013 Web çalıştırdığınızda, aşağıdaki web giriş sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9c491-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - varsayılan sayfa](introduction-and-overview/_static/image1.png)

<span data-ttu-id="9c491-182">Yeni bir kullanıcı olarak Kaydol veya varolan bir kullanıcı oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9c491-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="9c491-183">Gezinti en üstünde mevcut ürünler veritabanından alarak her ürün kategorisi için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9c491-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="9c491-184">Ürünler bağlantısını seçerek, tüm kullanılabilir ürünlerin listesini görmeye olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9c491-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - ürünler](introduction-and-overview/_static/image2.png)

<span data-ttu-id="9c491-186">Bireysel Ürün Ayrıntıları listelenen ürünlerden herhangi birini seçerek de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

<span data-ttu-id="9c491-188">Bir kullanıcı olarak kaydedebilir ve Web Forms şablonu varsayılan işlevselliğini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9c491-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="9c491-189">Bu öğretici aynı zamanda var olan bir Gmail hesabını kullanarak oturum açma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9c491-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="9c491-190">Ayrıca, eklemek ve ürünleri veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

<span data-ttu-id="9c491-192">Bir kullanıcı olarak oturum açtıktan sonra alışveriş sepeti ve checkout PayPal ile ürünleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="9c491-193">Bu örnek uygulama PayPal'ın Geliştirici sandbox ile çalışacak biçimde tasarlanmıştır unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9c491-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="9c491-194">Hiç gerçek para işlem gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="9c491-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

<span data-ttu-id="9c491-196">PayPal hesabı, sipariş ve ödeme bilgileri onaylar.</span><span class="sxs-lookup"><span data-stu-id="9c491-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="9c491-198">PayPal döndüğünüzde gözden geçirin ve siparişinizin tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="9c491-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - sipariş gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="9c491-200">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9c491-200">Prerequisites</span></span>

<span data-ttu-id="9c491-201">Başlamadan önce aşağıdaki yazılım bilgisayarınızda yüklü olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="9c491-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="9c491-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="9c491-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="9c491-203">.NET Framework otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="9c491-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="9c491-204">Bu öğretici seri Web için Microsoft Visual Studio Express 2013 kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c491-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="9c491-205">Bu öğretici seri tamamlamak için Web için Visual Studio Express 2013 Microsoft veya Microsoft Visual Studio 2013'nı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9c491-206">Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici seri anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="9c491-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="9c491-207">Yüklü Visual Studio sürümü zaten varsa, yükleme işlemi yanındaki mevcut sürümünü Visual Studio 2013 veya Web için Visual Studio Express 2013 Microsoft yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9c491-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="9c491-208">Önceki sürümlerde oluşturan siteleri Visual Studio 2013'te açılabilir ve önceki sürümlerde açmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="9c491-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9c491-209">Bu kılavuz, seçtiğiniz varsayar *Web geliştirme* ayarlar koleksiyonu, Visual Studio ilk başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="9c491-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="9c491-210">Daha fazla bilgi için bkz: [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c491-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="9c491-211">Örnek uygulamayı indirin</span><span class="sxs-lookup"><span data-stu-id="9c491-211">Download the Sample Application</span></span>

<span data-ttu-id="9c491-212">Önkoşulları yüklendikten sonra Bu öğretici serisinde sunulan yeni Web projesi oluşturmaya başlamak hazırsınız demektir.</span><span class="sxs-lookup"><span data-stu-id="9c491-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="9c491-213">Başlamayı tercih ederseniz **isteğe bağlı olarak** Bu öğretici seri oluşturur örnek uygulamayı çalıştırın, MSDN örnekleri sitesinden yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9c491-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="9c491-214">Bu yükleme aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="9c491-214">This download contains the following:</span></span>

- <span data-ttu-id="9c491-215">Örnek uygulama *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c491-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="9c491-216">Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="9c491-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="9c491-217">Dosya MSDN örnekleri sitesinden yükleyin:</span><span class="sxs-lookup"><span data-stu-id="9c491-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="9c491-218">[ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys Başlarken](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="9c491-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="9c491-219">Karşıdan yüklemenin bir <em>.zip</em> dosyası.</span><span class="sxs-lookup"><span data-stu-id="9c491-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="9c491-220">Bu öğretici seri oluşturur projeyi bulun ve seçin, görmek için <em>C#</em>klasöründe <em>.zip</em> dosyası.</span><span class="sxs-lookup"><span data-stu-id="9c491-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="9c491-221">Kaydet <em>C#</em> Folder Visual Studio 2013 projelerle çalışmak için kullandığınız klasör.</span><span class="sxs-lookup"><span data-stu-id="9c491-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="9c491-222">Varsayılan olarak, Visual Studio 2013 projeleri klasör aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9c491-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="9c491-223"><strong>C:\Users&#92;</strong><strong><em>&lt;kullanıcıadı&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="9c491-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="9c491-224">Yeniden Adlandır ***C#*** klasörüne ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="9c491-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="9c491-225">Adlı bir klasör zaten varsa *WingtipToys* projeleri klasörünüzde geçici olarak bu varolan bir klasörü yeniden adlandırmadan önce yeniden adlandırın *C#* klasörüne *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="9c491-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="9c491-226">Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="9c491-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="9c491-227">Visual Studio 2013 proje açılır.</span><span class="sxs-lookup"><span data-stu-id="9c491-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="9c491-228">Ardından, sağ *Default.aspx* dosya Çözüm Gezgini penceresinde ve sağ tıklatma menüsünden tarayıcıda görüntüle'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9c491-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="9c491-229">Eğitmen desteği ve açıklamaları</span><span class="sxs-lookup"><span data-stu-id="9c491-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="9c491-230">İle birlikte gelen Q ve bir bölümün [ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) herhangi bir sorunuz veya açıklamalar için örnek (C#).</span><span class="sxs-lookup"><span data-stu-id="9c491-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="9c491-231">Bu öğretici serilerinde açıklamaları Hoş Geldiniz ve hesap düzeltmeleri veya öğretici açıklamalarda sağlanan geliştirmeleri önerileri almak için Bu öğretici seri güncelleştirildiğinde her türlü çabayı yapılır.</span><span class="sxs-lookup"><span data-stu-id="9c491-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="9c491-232">Geliştirme sırasında bir hata olduğunda ya da Web sitesinin düzgün çalışmıyorsa, hata iletileri karmaşık ipuçları sorun kaynağına verebilir veya bu sorunun nasıl açıklayabilir değil.</span><span class="sxs-lookup"><span data-stu-id="9c491-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="9c491-233">Bazı yaygın sorun senaryolar ile yardımcı olmak için de kullanabilirsiniz [ASP.NET forumları](https://forums.asp.net/) veya birlikte Q ve bir bölüm [ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) örnek.</span><span class="sxs-lookup"><span data-stu-id="9c491-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="9c491-234">Bir hata iletisi alırsınız veya öğreticileri devam ederken bir sorun oluşması, yukarıdaki konumları denetlediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="9c491-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9c491-235">Next</span><span class="sxs-lookup"><span data-stu-id="9c491-235">Next</span></span>](create-the-project.md)
