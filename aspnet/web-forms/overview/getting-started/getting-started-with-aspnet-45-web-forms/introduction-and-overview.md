---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile çalışmaya başlama | Microsoft Docs
author: Erikre
description: Bu adım adım öğretici serisinin ASP.NET 4.5 ve Microsoft Visual Studio ifade etme kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450690"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="a41d5-103">ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile Çalışmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="a41d5-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="a41d5-104">tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a41d5-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a41d5-105">[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a41d5-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="a41d5-106">Bu adım adım öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> 

## <a name="introduction"></a><span data-ttu-id="a41d5-107">Giriş</span><span class="sxs-lookup"><span data-stu-id="a41d5-107">Introduction</span></span>

<span data-ttu-id="a41d5-108">Bu öğretici serisinde, Web uygulamaları ve ASP.NET 4.5 için Visual Studio Express 2013 kullanarak bir ASP.NET Web Forms uygulaması oluşturmak için gereken adımlarda size kılavuzluk eder.</span><span class="sxs-lookup"><span data-stu-id="a41d5-108">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="a41d5-109">Oluşturduğunuz uygulama adlı **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="a41d5-109">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="a41d5-110">Çevrimiçi öğe satan bir ön mağazası web sitesine basitleştirilmiş bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="a41d5-110">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="a41d5-111">Bu öğretici serisinde, ASP.NET 4.5 içinde kullanılabilen yeni özellikleri vurgular.</span><span class="sxs-lookup"><span data-stu-id="a41d5-111">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="a41d5-112">Açıklamalar davetlidir ve Bu öğretici serisinde, öneriler temelinde güncelleştirmek için her türlü çabayı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="a41d5-112">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="a41d5-113">Proje indirme tamamlandı</span><span class="sxs-lookup"><span data-stu-id="a41d5-113">Download completed project</span></span>

<span data-ttu-id="a41d5-114">Tamamlanan öğretici içeren bir C# projesi indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-114">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="a41d5-115">[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="a41d5-115">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="a41d5-116">İlgili ASP.NET Web Forms test gerçekleştirerek içeriği gözden geçir</span><span class="sxs-lookup"><span data-stu-id="a41d5-116">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="a41d5-117">Bu öğreticiyi tamamladıktan sonra bilginizi test ve yararlanarak önemli kavramları güçlendiren [ASP.NET Web Forms test](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="a41d5-117">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="a41d5-118">Bu test, Bu öğretici serisinde bulunan içeriğinden özel olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-118">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="a41d5-119">Testteki her soru olduğuyla ilgili ek yönergeler için bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="a41d5-119">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="a41d5-120">Test ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="a41d5-120">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="a41d5-121">Hedef Kitle</span><span class="sxs-lookup"><span data-stu-id="a41d5-121">Audience</span></span>

<span data-ttu-id="a41d5-122">Bu öğretici serisinin hedef kitlesi, ASP.NET Web formları için yeni deneyimli geliştiriciler olur.</span><span class="sxs-lookup"><span data-stu-id="a41d5-122">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="a41d5-123">Bu öğretici serisinde isteyen bir geliştirici, aşağıdaki yetenekleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="a41d5-123">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="a41d5-124">Tanıdık bir nesne yönelimli programlama (OOP) dil</span><span class="sxs-lookup"><span data-stu-id="a41d5-124">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="a41d5-125">Tanıdık, Web geliştirme kavramları (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="a41d5-125">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="a41d5-126">İlişkisel Veritabanı kavramlarına aşina</span><span class="sxs-lookup"><span data-stu-id="a41d5-126">Familiar with relational database concepts</span></span>
- <span data-ttu-id="a41d5-127">N katmanlı mimari kavramları hakkında bilgi sahibi</span><span class="sxs-lookup"><span data-stu-id="a41d5-127">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="a41d5-128">Yukarıda listelenen alanları gözden geçirme içinde ilgileniyorsanız aşağıdaki içeriği gözden geçirme göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="a41d5-128">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="a41d5-129">Visual C# kullanmaya Başlarken</span><span class="sxs-lookup"><span data-stu-id="a41d5-129">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="a41d5-130">[Web geliştirme](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="a41d5-130">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="a41d5-131">İlişkisel veritabanı</span><span class="sxs-lookup"><span data-stu-id="a41d5-131">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="a41d5-132">Çok katmanlı mimari</span><span class="sxs-lookup"><span data-stu-id="a41d5-132">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="a41d5-133">Uygulama özellikleri</span><span class="sxs-lookup"><span data-stu-id="a41d5-133">Application Features</span></span>

<span data-ttu-id="a41d5-134">Bu seride sunulan ASP.NET Web formu özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a41d5-134">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="a41d5-135">Web uygulama projesi (Web sitesi projesi değil)</span><span class="sxs-lookup"><span data-stu-id="a41d5-135">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="a41d5-136">Web Forms</span><span class="sxs-lookup"><span data-stu-id="a41d5-136">Web Forms</span></span>
- <span data-ttu-id="a41d5-137">Ana sayfalar, yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a41d5-137">Master Pages, Configuration</span></span>
- <span data-ttu-id="a41d5-138">Önyükleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-138">Bootstrap</span></span>
- <span data-ttu-id="a41d5-139">Entity Framework Code ilk olarak LocalDB</span><span class="sxs-lookup"><span data-stu-id="a41d5-139">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="a41d5-140">İsteği doğrulama</span><span class="sxs-lookup"><span data-stu-id="a41d5-140">Request Validation</span></span>
- <span data-ttu-id="a41d5-141">Kesin türü belirtilmiş veri denetimleri, Model, veri ek açıklamaları, bağlama ve değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a41d5-141">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="a41d5-142">SSL ve OAuth</span><span class="sxs-lookup"><span data-stu-id="a41d5-142">SSL and OAuth</span></span>
- <span data-ttu-id="a41d5-143">ASP.NET Identity, yapılandırma ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="a41d5-143">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="a41d5-144">Örtük doğrulama</span><span class="sxs-lookup"><span data-stu-id="a41d5-144">Unobtrusive Validation</span></span>
- <span data-ttu-id="a41d5-145">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a41d5-145">Routing</span></span>
- <span data-ttu-id="a41d5-146">ASP.NET Hata İşleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-146">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="a41d5-147">Uygulama senaryolar ve görevler</span><span class="sxs-lookup"><span data-stu-id="a41d5-147">Application Scenarios and Tasks</span></span>

<span data-ttu-id="a41d5-148">Bu seride gösterilen görevler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="a41d5-148">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="a41d5-149">Oluşturma, gözden geçirme ve yeni proje çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a41d5-149">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="a41d5-150">Veritabanı yapısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a41d5-150">Creating the database structure</span></span>
- <span data-ttu-id="a41d5-151">Başlatma ve veritabanında dengeli dağıtım</span><span class="sxs-lookup"><span data-stu-id="a41d5-151">Initializing and seeding the database</span></span>
- <span data-ttu-id="a41d5-152">Stilleri, grafik ve bir ana sayfa kullanarak kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="a41d5-152">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="a41d5-153">Sayfalar ve gezinti ekleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-153">Adding pages and navigation</span></span>
- <span data-ttu-id="a41d5-154">Menü ayrıntılarını ve ürün verileri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-154">Displaying menu details and product data</span></span>
- <span data-ttu-id="a41d5-155">Bir alışveriş sepeti oluşturmak</span><span class="sxs-lookup"><span data-stu-id="a41d5-155">Creating a shopping cart</span></span>
- <span data-ttu-id="a41d5-156">Ekleme SSL ve OAuth desteği</span><span class="sxs-lookup"><span data-stu-id="a41d5-156">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="a41d5-157">Bir ödeme yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-157">Adding a payment method</span></span>
- <span data-ttu-id="a41d5-158">Bir yönetici rolü ve bir kullanıcı uygulamaya dahil</span><span class="sxs-lookup"><span data-stu-id="a41d5-158">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="a41d5-159">Belirli bir sayfa ve klasörüne erişimi kısıtlama</span><span class="sxs-lookup"><span data-stu-id="a41d5-159">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="a41d5-160">Web uygulaması için bir dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="a41d5-160">Uploading a file to the web application</span></span>
- <span data-ttu-id="a41d5-161">Giriş Doğrulaması uygulama</span><span class="sxs-lookup"><span data-stu-id="a41d5-161">Implementing input validation</span></span>
- <span data-ttu-id="a41d5-162">Web uygulama için rota kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="a41d5-162">Registering routes for the web application</span></span>
- <span data-ttu-id="a41d5-163">Uygulama hata işleme ve hata günlüğü</span><span class="sxs-lookup"><span data-stu-id="a41d5-163">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="a41d5-164">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a41d5-164">Overview</span></span>

<span data-ttu-id="a41d5-165">ASP.NET Web formları için yeni başladıysanız ancak programlama kavramları ile aşinalık yoksa, doğru öğretici sahip.</span><span class="sxs-lookup"><span data-stu-id="a41d5-165">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="a41d5-166">ASP.NET Web Forms ile bilginiz varsa, Bu öğretici serisine ASP.NET 4.5 içinde kullanılabilen yeni özellikler yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a41d5-166">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="a41d5-167">Web formları içinde sağlanan ek öğreticiler programlama kavramları ve ASP.NET Web Forms ile bilginiz yoksa bkz [Başlarken](../../../index.md) ASP.NET Web sitesinde bölümü.</span><span class="sxs-lookup"><span data-stu-id="a41d5-167">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="a41d5-168">Belirli **son** ASP.NET 4.5 özellikleri koşuluyla bu Web formlarında öğretici serisinin şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a41d5-168">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="a41d5-169">Bu teklifi oluşturmak için basit bir kullanıcı Arabirimi projeleri [desteklemek için birden çok ASP.NET çerçeve](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web formları, MVC ve Web API'si).</span><span class="sxs-lookup"><span data-stu-id="a41d5-169">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="a41d5-170">[Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), duyarlı tasarım ve Tema oluşturma özellikleri sağlayan bir düzen ve Tema oluşturma çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="a41d5-170">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="a41d5-171">[ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveleri ve çalışan aynı web barındırma yazılımı dışında IIS ile çalışan yeni bir ASP.NET üyelik sistemi.</span><span class="sxs-lookup"><span data-stu-id="a41d5-171">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="a41d5-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), nesneleri almak ve veri türü kesin olarak işlemek sağlayan Entity Framework için bir güncelleştirme yazılan, verilere zaman uyumsuz olarak geçici bağlantı hataları işlemek ve oturum SQL deyimleri.</span><span class="sxs-lookup"><span data-stu-id="a41d5-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="a41d5-173">ASP.NET 4.5 özelliklerin tam bir listesi için bkz. [için ASP.NET and Web Tools Visual Studio 2013 sürüm notları](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="a41d5-173">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="a41d5-174">Wingtip Toys örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="a41d5-174">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="a41d5-175">Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturacağınız ASP.NET Web forms uygulaması hızlı bir görünümünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="a41d5-175">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="a41d5-176">Visual Studio Express 2013 Web için gelen uygulamayı çalıştırdığınızda, aşağıdaki web giriş sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-176">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - varsayılan sayfası](introduction-and-overview/_static/image1.png)

<span data-ttu-id="a41d5-178">Yeni bir kullanıcı olarak Kaydol veya var olan bir kullanıcı olarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-178">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="a41d5-179">Gezinti üst kısmında, veritabanından çalıştırarak kullanılabilir ürünlere alarak her ürün kategorisi için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-179">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="a41d5-180">Ürünleri bağlantıyı seçerek kullanılabilir tüm ürünlerin listesini görmek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-180">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - ürünleri](introduction-and-overview/_static/image2.png)

<span data-ttu-id="a41d5-182">Ayrıca, listelenen ürünlerden birini seçerek tek tek ürün ayrıntıları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-182">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

<span data-ttu-id="a41d5-184">Bir kullanıcı olarak kaydedin ve Web Forms şablonu varsayılan işlevlerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-184">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="a41d5-185">Bu öğreticide, ayrıca var olan bir Gmail hesabını kullanarak oturum nasıl açıklar.</span><span class="sxs-lookup"><span data-stu-id="a41d5-185">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="a41d5-186">Ayrıca, eklemek ve ürün veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-186">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

<span data-ttu-id="a41d5-188">Bir kullanıcı olarak oturum açtıktan sonra alışveriş sepeti ve PayPal ile kasa işlemleri ürünleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-188">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="a41d5-189">Bu örnek uygulama PayPal'ın Geliştirici sanal işlev görecek şekilde tasarlanmış olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-189">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="a41d5-190">Hiçbir gerçek parayı işlem gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="a41d5-190">No actual money transaction will take place.</span></span>

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

<span data-ttu-id="a41d5-192">PayPal, hesap, siparişi ve Ödeme bilgilerinizi onaylar.</span><span class="sxs-lookup"><span data-stu-id="a41d5-192">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="a41d5-194">PayPal ' iade edildikten sonra gözden geçirin ve siparişinizi tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-194">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - siparişi gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="a41d5-196">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a41d5-196">Prerequisites</span></span>

<span data-ttu-id="a41d5-197">Başlamadan önce aşağıdaki yazılımlar bilgisayarınızda yüklü olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="a41d5-197">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="a41d5-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="a41d5-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="a41d5-199">.NET Framework otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a41d5-199">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="a41d5-200">Bu öğretici serisinde, Web için Microsoft Visual Studio Express 2013 kullanır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-200">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a41d5-201">Bu öğretici serisinin tamamlanmasını Web için Visual Studio Express 2013 Microsoft veya Microsoft Visual Studio 2013'ü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-201">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a41d5-202">Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici serisinin denir.</span><span class="sxs-lookup"><span data-stu-id="a41d5-202">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="a41d5-203">Bir Visual Studio sürümü zaten varsa, yükleme işlemi yanındaki mevcut sürümü Visual Studio 2013 veya Web için Visual Studio Express 2013 Microsoft yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a41d5-203">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="a41d5-204">Önceki sürümlerinde oluşturulan siteleri, Visual Studio 2013'te açılabilir ve önceki sürümlerde açmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="a41d5-204">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a41d5-205">Bu kılavuzda, seçtiğiniz varsayılır *Web geliştirme* ayarlar koleksiyonu, Visual Studio'yu ilk başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="a41d5-205">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="a41d5-206">Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="a41d5-206">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="a41d5-207">Örnek uygulamayı indirin</span><span class="sxs-lookup"><span data-stu-id="a41d5-207">Download the Sample Application</span></span>

<span data-ttu-id="a41d5-208">Önkoşullar yüklendikten sonra Bu öğretici serisinde sunulan yeni Web projesi oluşturmaya başlamak hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-208">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="a41d5-209">İsteyip istemediğini **isteğe bağlı olarak** oluşturan Bu öğretici serisinin örnek uygulamayı çalıştırın, MSDN Örnekler sitesinden indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-209">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="a41d5-210">Bu indirme, aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="a41d5-210">This download contains the following:</span></span>

- <span data-ttu-id="a41d5-211">Örnek uygulamada *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="a41d5-211">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="a41d5-212">Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="a41d5-212">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="a41d5-213">MSDN Örnekler siteden dosya indirme:</span><span class="sxs-lookup"><span data-stu-id="a41d5-213">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="a41d5-214">[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="a41d5-214">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="a41d5-215">İndirme bir <em>.zip</em> dosya.</span><span class="sxs-lookup"><span data-stu-id="a41d5-215">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="a41d5-216">Bu öğretici serisinin oluşturan projeyi bulun ve seçin görmek için <em>C#</em>klasöründe <em>.zip</em> dosya.</span><span class="sxs-lookup"><span data-stu-id="a41d5-216">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="a41d5-217">Kaydet <em>C#</em> Folder Visual Studio 2013 projeleriyle çalışmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="a41d5-217">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="a41d5-218">Varsayılan olarak, Visual Studio 2013 projeler klasöründe aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a41d5-218">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="a41d5-219"><strong>C:\Users&#92;</strong><strong><em>&lt;kullanıcıadı&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="a41d5-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="a41d5-220">Yeniden adlandırma ***C#*** klasörüne ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="a41d5-220">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="a41d5-221">Adlı bir klasör zaten varsa *WingtipToys* projeler klasörünüze geçici olarak var olan bir klasörü yeniden adlandırmadan önce Yeniden Adlandır *C#* klasörüne *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="a41d5-221">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="a41d5-222">Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="a41d5-222">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="a41d5-223">Visual Studio 2013 proje açılır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-223">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="a41d5-224">Ardından, sağ *Default.aspx* dosya Çözüm Gezgini penceresinde ve sağ tıklama menüsünden tarayıcıda görüntüle'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-224">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="a41d5-225">Öğretici desteği ve açıklamalar</span><span class="sxs-lookup"><span data-stu-id="a41d5-225">Tutorial Support and Comments</span></span>

<span data-ttu-id="a41d5-226">Bulunan soru cevap bölümünde kullanın [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) soru ya da Yorumlarınız için örneği (C#).</span><span class="sxs-lookup"><span data-stu-id="a41d5-226">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="a41d5-227">Bu öğretici serisinin üzerine yorum davetlidir ve hesap düzeltmeleri veya öğretici yorumlar bölümünde sağlanan geliştirmeleri için öneriler almak için Bu öğretici serisinin güncelleştirildiğinde her türlü çabayı yapılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a41d5-227">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="a41d5-228">Geliştirme sırasında bir hata meydana geldiğinde veya Web sitesi düzgün çalışmıyorsa, hata iletileri karmaşık nedene sorun kaynağına verebilir veya bu sorunun nasıl açıklayabilir değil.</span><span class="sxs-lookup"><span data-stu-id="a41d5-228">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="a41d5-229">Bazı yaygın sorun senaryoları ile yardımcı olmak için de kullanabilirsiniz [ASP.NET forumları](https://forums.asp.net/) veya bulunan soru cevap bölümünde [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) örnek.</span><span class="sxs-lookup"><span data-stu-id="a41d5-229">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="a41d5-230">Şu öğreticileri gibi bir sorun oluşması veya bir hata iletisi alırsanız, yukarıdaki konumları kontrol etmeyi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a41d5-230">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a41d5-231">Next</span><span class="sxs-lookup"><span data-stu-id="a41d5-231">Next</span></span>](create-the-project.md)
