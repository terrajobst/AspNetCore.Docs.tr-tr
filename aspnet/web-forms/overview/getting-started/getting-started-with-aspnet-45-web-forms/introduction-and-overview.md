---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.7 Web Forms ve Visual Studio 2017 ile çalışmaya başlama | Microsoft Docs
author: Erikre
description: Bu adım adım öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri sağlanır.
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341559"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="5d76b-103">ASP.NET 4.5 Web Forms ve Visual Studio 2017 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5d76b-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="5d76b-104">[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="5d76b-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="5d76b-105">Bu öğretici serisinde, ASP.NET 4.5 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturma işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="5d76b-106">Giriş</span><span class="sxs-lookup"><span data-stu-id="5d76b-106">Introduction</span></span>

<span data-ttu-id="5d76b-107">Bu öğretici serisinde, Visual Studio 2017 ve ASP.NET 4.5 kullanarak bir ASP.NET Web Forms uygulaması oluşturma işleminde size yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="5d76b-108">Adlı bir uygulama oluşturacaksınız **Wingtip Toys** - çevrimiçi öğe satış basitleştirilmiş bir mağaza web sitesi.</span><span class="sxs-lookup"><span data-stu-id="5d76b-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="5d76b-109">Serinin sırasında yeni ASP.NET 4.5 özellikleri vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="5d76b-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="5d76b-110">Hedef kitle</span><span class="sxs-lookup"><span data-stu-id="5d76b-110">Target audience</span></span>

<span data-ttu-id="5d76b-111">ASP.NET Web formları için yeni geliştiricilerin, Bu öğretici serisinin hedef kitlesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="5d76b-112">Aşağıdaki alanlarda bazı bilgisine sahip olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="5d76b-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="5d76b-113">Nesne odaklı programlama (OOP) ve dilleri</span><span class="sxs-lookup"><span data-stu-id="5d76b-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="5d76b-114">Web geliştirme (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="5d76b-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="5d76b-115">İlişkisel veritabanları</span><span class="sxs-lookup"><span data-stu-id="5d76b-115">Relational databases</span></span>
- <span data-ttu-id="5d76b-116">N katmanlı mimari</span><span class="sxs-lookup"><span data-stu-id="5d76b-116">N-tier architecture</span></span>

<span data-ttu-id="5d76b-117">Bu alanlar gözden geçirmek için aşağıdaki içeriği Çincesi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5d76b-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="5d76b-118">Visual C# kullanmaya Başlarken</span><span class="sxs-lookup"><span data-stu-id="5d76b-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="5d76b-119">[Web geliştirme](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="5d76b-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="5d76b-120">İlişkisel veritabanı</span><span class="sxs-lookup"><span data-stu-id="5d76b-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="5d76b-121">Çok katmanlı mimari</span><span class="sxs-lookup"><span data-stu-id="5d76b-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="5d76b-122">Uygulama özellikleri</span><span class="sxs-lookup"><span data-stu-id="5d76b-122">Application features</span></span>

<span data-ttu-id="5d76b-123">Bu seride sunulan ASP.NET Web formu özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5d76b-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="5d76b-124">Web uygulama projesi (Web sitesi projesi değil)</span><span class="sxs-lookup"><span data-stu-id="5d76b-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="5d76b-125">Web Forms</span><span class="sxs-lookup"><span data-stu-id="5d76b-125">Web Forms</span></span>
- <span data-ttu-id="5d76b-126">Ana sayfalar, yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5d76b-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="5d76b-127">Önyükleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-127">Bootstrap</span></span>
- <span data-ttu-id="5d76b-128">Entity Framework Code ilk olarak LocalDB</span><span class="sxs-lookup"><span data-stu-id="5d76b-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="5d76b-129">İsteği doğrulama</span><span class="sxs-lookup"><span data-stu-id="5d76b-129">Request Validation</span></span>
- <span data-ttu-id="5d76b-130">Kesin türü belirtilmiş veri denetimleri</span><span class="sxs-lookup"><span data-stu-id="5d76b-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="5d76b-131">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="5d76b-131">Model Binding</span></span>
- <span data-ttu-id="5d76b-132">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5d76b-132">Data Annotations</span></span>
- <span data-ttu-id="5d76b-133">Değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="5d76b-133">Value Providers</span></span>
- <span data-ttu-id="5d76b-134">SSL ve OAuth</span><span class="sxs-lookup"><span data-stu-id="5d76b-134">SSL and OAuth</span></span>
- <span data-ttu-id="5d76b-135">ASP.NET Identity, yapılandırma ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5d76b-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="5d76b-136">Örtük doğrulama</span><span class="sxs-lookup"><span data-stu-id="5d76b-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="5d76b-137">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="5d76b-137">Routing</span></span>
- <span data-ttu-id="5d76b-138">ASP.NET Hata İşleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="5d76b-139">Uygulama senaryolar ve görevler</span><span class="sxs-lookup"><span data-stu-id="5d76b-139">Application scenarios and tasks</span></span>

<span data-ttu-id="5d76b-140">Öğretici serisinin görevler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="5d76b-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="5d76b-141">Çalıştıran yeni bir proje oluşturma ve gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="5d76b-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="5d76b-142">Veritabanı yapısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="5d76b-142">Creating a database structure</span></span>
- <span data-ttu-id="5d76b-143">Başlatma ve bir veritabanı dengeli dağıtım</span><span class="sxs-lookup"><span data-stu-id="5d76b-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="5d76b-144">Stilleri, grafik ve bir ana sayfa ile kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5d76b-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="5d76b-145">Sayfalar ve gezinti ekleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-145">Adding pages and navigation</span></span>
- <span data-ttu-id="5d76b-146">Menü ayrıntılarını ve ürün verileri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="5d76b-147">Bir alışveriş sepeti oluşturmak</span><span class="sxs-lookup"><span data-stu-id="5d76b-147">Creating a shopping cart</span></span>
- <span data-ttu-id="5d76b-148">Ekleme SSL ve OAuth desteği</span><span class="sxs-lookup"><span data-stu-id="5d76b-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="5d76b-149">Bir ödeme yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-149">Adding a payment method</span></span>
- <span data-ttu-id="5d76b-150">Bir yönetici rolü ve bir kullanıcı uygulamaya dahil</span><span class="sxs-lookup"><span data-stu-id="5d76b-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="5d76b-151">Belirli bir sayfa ve klasörüne erişimi kısıtlama</span><span class="sxs-lookup"><span data-stu-id="5d76b-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="5d76b-152">Web uygulaması için bir dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="5d76b-153">Giriş Doğrulaması uygulama</span><span class="sxs-lookup"><span data-stu-id="5d76b-153">Implementing input validation</span></span>
- <span data-ttu-id="5d76b-154">Web uygulama için rota kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="5d76b-154">Registering routes for the web application</span></span>
- <span data-ttu-id="5d76b-155">Uygulama hata işleme ve hata günlüğü</span><span class="sxs-lookup"><span data-stu-id="5d76b-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="5d76b-156">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5d76b-156">Overview</span></span>

<span data-ttu-id="5d76b-157">Bu öğretici serisinin programlama kavramları bildiğiniz birisi yöneliktir, ancak yeni ASP.NET Web formları için değildir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="5d76b-158">Zaten ASP.NET Web Forms ile biliyorsanız, bu seri hala yeni ASP.NET 4.5 özellikleri hakkında bilgi edinmenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="5d76b-159">Sağlanan ek Web Forms öğreticiler için bkz. okuyucular programlama kavramları ve ASP.NET Web Forms ile bilinmeyen [Başlarken](../../../index.md) ASP.NET Web sitesinde bölümü.</span><span class="sxs-lookup"><span data-stu-id="5d76b-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="5d76b-160">Bu öğretici serisinde sağlanan ASP.NET 4.5 aşağıdaki özellikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5d76b-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="5d76b-161">Sunan projeleri oluşturmak için basit bir kullanıcı Arabirimiyle [birçok ASP.NET çerçeveler için destek](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web formları, MVC ve Web API'si).</span><span class="sxs-lookup"><span data-stu-id="5d76b-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="5d76b-162">[Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), düzen, tema oluşturma ve duyarlı tasarım framework.</span><span class="sxs-lookup"><span data-stu-id="5d76b-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="5d76b-163">[ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveleri ve çalışan aynı web barındırma yazılımı dışında IIS ile çalışan yeni bir ASP.NET üyelik sistemi.</span><span class="sxs-lookup"><span data-stu-id="5d76b-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="5d76b-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5d76b-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="5d76b-165">Bir güncelleştirme sağlayarak Entity Framework için:</span><span class="sxs-lookup"><span data-stu-id="5d76b-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="5d76b-166">Alma ve kesin tür belirtilmiş nesneler olarak verilerini işleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="5d76b-167">Zaman uyumsuz olarak veri erişimi</span><span class="sxs-lookup"><span data-stu-id="5d76b-167">Access data asynchronously</span></span>
  - <span data-ttu-id="5d76b-168">Geçici bağlantı hataları işleme</span><span class="sxs-lookup"><span data-stu-id="5d76b-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="5d76b-169">Günlük SQL deyimleri</span><span class="sxs-lookup"><span data-stu-id="5d76b-169">Log SQL statements</span></span>

<span data-ttu-id="5d76b-170">Tam ASP.NET 4.5 özellik listesi için bkz: [için ASP.NET and Web Tools Visual Studio 2013 sürüm notları](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="5d76b-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="5d76b-171">Wingtip Toys örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="5d76b-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="5d76b-172">Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturduğunuz ASP.NET Web Forms uygulaması arasındadır.</span><span class="sxs-lookup"><span data-stu-id="5d76b-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="5d76b-173">Visual Studio'da uygulamayı çalıştırdığınızda, aşağıdaki web giriş sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys - varsayılan sayfası](introduction-and-overview/_static/image1.png)

<span data-ttu-id="5d76b-175">Yeni bir kullanıcı olarak Kaydol veya var olan bir kullanıcı olarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="5d76b-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="5d76b-176">Üst gezinti, ürün kategorilerini ve bunların ürünlerini veritabanından bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="5d76b-177">Seçerseniz **ürünleri**, kullanılabilir tüm ürünleri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys - ürünleri](introduction-and-overview/_static/image2.png)

<span data-ttu-id="5d76b-179">Belirli bir ürün seçerseniz, ürün ayrıntıları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

<span data-ttu-id="5d76b-181">Bir kullanıcı olarak kaydedin ve Web Forms şablonu varsayılan işlevsellik bilgilerinizle oturum açın.</span><span class="sxs-lookup"><span data-stu-id="5d76b-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="5d76b-182">Bu öğreticide, ayrıca var olan bir Gmail hesabını kullanarak oturum açmanız nasıl açıklar.</span><span class="sxs-lookup"><span data-stu-id="5d76b-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="5d76b-183">Ayrıca, eklemek ve ürün veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5d76b-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

<span data-ttu-id="5d76b-185">Bir kullanıcı olarak açtıktan sonra alışveriş sepeti ve PayPal ile kasa işlemleri ürünleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5d76b-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="5d76b-186">Örnek uygulama, PayPal'ın Geliştirici korumalı alanında çalışmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5d76b-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="5d76b-187">Hiçbir gerçek parayı işlem gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-187">No actual money transaction takes place.</span></span>

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

<span data-ttu-id="5d76b-189">PayPal, hesap, siparişi ve Ödeme bilgilerinizi onaylar.</span><span class="sxs-lookup"><span data-stu-id="5d76b-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="5d76b-191">PayPal ' iade edildikten sonra gözden geçirin ve siparişinizi tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="5d76b-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - siparişi gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="5d76b-193">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5d76b-193">Prerequisites</span></span>

<span data-ttu-id="5d76b-194">Başlamadan önce aşağıdaki yazılımlar bilgisayarınızda yüklü olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="5d76b-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="5d76b-195">[Microsoft Visual Studio 2017 veya Microsoft Visual Studio Community 2017'yi](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5d76b-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="5d76b-196">.NET Framework otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="5d76b-197">Bu öğretici serisinde, Microsoft Visual Studio Community 2017 kullanır.</span><span class="sxs-lookup"><span data-stu-id="5d76b-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="5d76b-198">Ya da bu hesabı kullanabilirsiniz veya Microsoft Visual Studio 2017 Bu öğretici serisinin tamamlanmasını.</span><span class="sxs-lookup"><span data-stu-id="5d76b-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="5d76b-199">Visual Studio hakkında aşağıdakileri unutmayın:</span><span class="sxs-lookup"><span data-stu-id="5d76b-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="5d76b-200">Microsoft Visual Studio 2017 ve Microsoft Visual Studio Community 2017 denir *Visual Studio* Bu öğretici serisinin boyunca.</span><span class="sxs-lookup"><span data-stu-id="5d76b-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="5d76b-201">Visual Studio 2017, herhangi bir eski sürümü zaten yüklü yanındaki yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="5d76b-202">Önceki sürümlerinde oluşturulan siteleri, Visual Studio 2017'de açılabilir ve önceki sürümlerde açmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="5d76b-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="5d76b-203">İlk kez Visual Studio'yu başlattığınız, seçtiğiniz varsayılır *Web geliştirme* ayarları.</span><span class="sxs-lookup"><span data-stu-id="5d76b-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="5d76b-204">Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortam ayarlarını Seç](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d76b-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="5d76b-205">Önkoşullar yüklendikten sonra Bu öğretici serisinde sunulan Web projesi oluşturmaya başlamak hazırsınız demektir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="5d76b-206">Örnek uygulamayı indirin</span><span class="sxs-lookup"><span data-stu-id="5d76b-206">Download the sample application</span></span>

 <span data-ttu-id="5d76b-207">Tamamlanan örnek applicatiion adresindeki MSDN Örnekler sitesinden dilediğiniz zaman indirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5d76b-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="5d76b-208">[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5d76b-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="5d76b-209">Bu indirme, aşağıdaki öğeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5d76b-209">This download has the following items:</span></span>

- <span data-ttu-id="5d76b-210">Örnek uygulamada *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="5d76b-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="5d76b-211">Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.</span><span class="sxs-lookup"><span data-stu-id="5d76b-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="5d76b-212">İndirme bir *.zip* dosya.</span><span class="sxs-lookup"><span data-stu-id="5d76b-212">The download is a *.zip* file.</span></span> <span data-ttu-id="5d76b-213">Bu öğretici serisinin oluşturan projeyi bulun ve seçin görmek için *C#* klasöründeki .zip dosyası.</span><span class="sxs-lookup"><span data-stu-id="5d76b-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="5d76b-214">Kaydet C# Visual Studio projeleriyle çalışmak için klasörü klasörü.</span><span class="sxs-lookup"><span data-stu-id="5d76b-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="5d76b-215">Varsayılan olarak, Visual Studio 2017 projeleri klasördür:</span><span class="sxs-lookup"><span data-stu-id="5d76b-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="5d76b-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="5d76b-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="5d76b-217">Yeniden adlandırma ***C#*** klasörüne ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="5d76b-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="5d76b-218">Adlı bir klasör zaten varsa *WingtipToys* projeler klasörünüze geçici olarak var olan bir klasörü yeniden adlandırmadan önce Yeniden Adlandır *C#* klasörüne *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="5d76b-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="5d76b-219">Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="5d76b-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="5d76b-220">Visual Studio 2017, projeyi açar.</span><span class="sxs-lookup"><span data-stu-id="5d76b-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="5d76b-221">Ardından, sağ *Default.aspx* dosyası **Çözüm Gezgini** seçip **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="5d76b-222">Bir ASP.NET Web Forms sınava içeriği gözden geçirmek için</span><span class="sxs-lookup"><span data-stu-id="5d76b-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="5d76b-223">Öğretici serisinin tamamladıktan sonra bilginizi test ve önemli kavramları güçlendiren bir testi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5d76b-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="5d76b-224">Bütün soruların yanıtını bir açıklama ve ek yönergeler için bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="5d76b-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="5d76b-225">Test ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="5d76b-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="5d76b-226">Öğretici desteği ve açıklamalar</span><span class="sxs-lookup"><span data-stu-id="5d76b-226">Tutorial support and comments</span></span>

<span data-ttu-id="5d76b-227">Sorularınız ve Yorumlarınız için dahil soru cevap bölümünde kullanın [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) örnek sayfası.</span><span class="sxs-lookup"><span data-stu-id="5d76b-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="5d76b-228">Bu öğretici serisinin üzerine yorum davetlidir.</span><span class="sxs-lookup"><span data-stu-id="5d76b-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="5d76b-229">Bu öğretici serisinin güncelleştirildiğinde, her türlü çabayı düzeltmeleri veya geliştirme önerileri göz önünde bulundurun yapılır.</span><span class="sxs-lookup"><span data-stu-id="5d76b-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="5d76b-230">Bir hata, ilgili hata iletilerini karıştırıcı olabilir oluyorsa sorunu gidermek nasıl iyi bir açıklama ile.</span><span class="sxs-lookup"><span data-stu-id="5d76b-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="5d76b-231">Yardım için kontrol edebilirsiniz [ASP.NET forumları](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="5d76b-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="5d76b-232">Başka bir iyi soru cevap bölümünde kaynağıdır [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) örnek sayfası.</span><span class="sxs-lookup"><span data-stu-id="5d76b-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="5d76b-233">Next</span><span class="sxs-lookup"><span data-stu-id="5d76b-233">Next</span></span>](create-the-project.md)
