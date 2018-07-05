---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: '1. Bölüm: Genel bakış ve Dosya -> Yeni Proje | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 1 kapak genel bakış ve Dosya -> Yeni proje.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377895"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="c6252-104">1. Bölüm: Genel bakış ve Dosya -> Yeni Proje</span><span class="sxs-lookup"><span data-stu-id="c6252-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="c6252-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c6252-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c6252-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c6252-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c6252-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c6252-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c6252-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c6252-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c6252-109">1. Bölüm kapsayan genel bakış ve dosya -&gt;yeni bir proje.</span><span class="sxs-lookup"><span data-stu-id="c6252-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="c6252-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="c6252-110">Overview</span></span>

<span data-ttu-id="c6252-111">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Web Developer web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c6252-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="c6252-112">Biz, yavaş başlatma, başlangıç düzeyi bir web geliştirme deneyimi için uygundur.</span><span class="sxs-lookup"><span data-stu-id="c6252-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="c6252-113">Biz oluşturuyor olacaksınız basit müzik deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c6252-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="c6252-114">Uygulamayı üç ana bölümü vardır: alışveriş, kullanıma alma ve yönetim.</span><span class="sxs-lookup"><span data-stu-id="c6252-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="c6252-115">Ziyaretçi Tarz albümlerini göz atabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c6252-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="c6252-116">Tek bir albümü görüntülemek ve bunların sepetinize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c6252-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="c6252-117">Bunlar, artık istedikleri tüm öğeleri kaldırma, kendi Sepeti gözden geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c6252-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="c6252-118">Kullanıma alma için devam etmeden bunları bir kullanıcı hesabına kaydolun veya oturum açma için ister.</span><span class="sxs-lookup"><span data-stu-id="c6252-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="c6252-119">Bir hesap oluşturduktan sonra bunların sırası sevkıyat ve ödeme bilgilerini doldurarak tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6252-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="c6252-120">Örneği basit tutmak için biz harika bir yükseltme çalıştırıyorsanız: her şeyi promosyon kodunu "Ücretsiz" olacaklardır ücretsizdir!</span><span class="sxs-lookup"><span data-stu-id="c6252-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="c6252-121">Sıralandıktan sonra bunlar basit bir onay ekranı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="c6252-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="c6252-122">Ayrıca müşteri faceing sayfalarına ek olarak albümleri, yöneticiler oluşturabilir, düzenleme, listesini gösteren bir yönetici bölümü oluşturun ve albümleri Sil:</span><span class="sxs-lookup"><span data-stu-id="c6252-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="c6252-123">1. Dosya -&gt; yeni proje</span><span class="sxs-lookup"><span data-stu-id="c6252-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="c6252-124">Yazılım yükleme</span><span class="sxs-lookup"><span data-stu-id="c6252-124">Installing the software</span></span>

<span data-ttu-id="c6252-125">Bu öğreticide, ücretsiz Visual Web Developer 2010 (ücretsiz olan) Express kullanarak yeni bir ASP.NET MVC 3 projesi oluşturarak başlar ve ardından eksiksiz, çalışan bir uygulamayı oluşturmak için özellikler artımlı olarak ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c6252-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="c6252-126">Ana sayfalar tutarlı sayfa düzeni için AJAX Sayfa güncelleştirmelerini ve doğrulama, kullanıcı oturum açma ve daha fazlasını kullanarak, süreç boyunca size veritabanı erişimi, formu posta senaryolar ve veri doğrulama değineceğiz.</span><span class="sxs-lookup"><span data-stu-id="c6252-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="c6252-127">Adım adım takip edebilirsiniz veya tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="c6252-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="c6252-128">Uygulamayı oluşturmak için Visual Studio 2010 SP1 veya Visual Web Developer 2010 Express SP1 (Visual Studio 2010 ücretsiz bir sürümü) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6252-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="c6252-129">SQL Server Compact (da ücretsiz) veritabanını barındırmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="c6252-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="c6252-130">Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="c6252-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="c6252-131">[Visual Studio Web Developer Express SP1 Önkoşullar]</span><span class="sxs-lookup"><span data-stu-id="c6252-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="c6252-132">[ASP.NET MVC 3 araçları güncelleştirme]</span><span class="sxs-lookup"><span data-stu-id="c6252-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="c6252-133">[SQL Server Compact 4.0] - hem çalışma zamanı ve araçları desteği dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="c6252-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="c6252-134">Yeni bir ASP.NET MVC 3 projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c6252-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="c6252-135">Visual Web Developer Dosya menüsünden "Yeni Proje" seçerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="c6252-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="c6252-136">Bu, yeni proje iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="c6252-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="c6252-137">Visual C# - seçeneğini belirleyeceğiz&gt; Web şablonları grup sol tarafta, ardından Orta sütunda "ASP.NET MVC 3 Web uygulaması" şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="c6252-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="c6252-138">MvcMusicStore projenizi adlandırın ve Tamam düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="c6252-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="c6252-139">Bu MVC belirli ayarlara Projemizin olmak bize izin veren bir ikincil bir iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c6252-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="c6252-140">Aşağıdakileri seçin:</span><span class="sxs-lookup"><span data-stu-id="c6252-140">Select the following:</span></span>

<span data-ttu-id="c6252-141">Şablon proje - boş seçin</span><span class="sxs-lookup"><span data-stu-id="c6252-141">Project Template - select Empty</span></span>

<span data-ttu-id="c6252-142">Altyapısı görüntüleme - Razor seçin</span><span class="sxs-lookup"><span data-stu-id="c6252-142">View Engine - select Razor</span></span>

<span data-ttu-id="c6252-143">Kontrol anlam biçimlendirme HTML5 - kullanın</span><span class="sxs-lookup"><span data-stu-id="c6252-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="c6252-144">Ayarlarınızı aşağıda gösterildiği gibi ardından Tamam düğmesine basın doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c6252-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="c6252-145">Bu, bizim projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6252-145">This will create our project.</span></span> <span data-ttu-id="c6252-146">Çözüm Gezgini'nde sağ tarafındaki uygulamamız eklenmiş klasörleri bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="c6252-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="c6252-147">Boş MVC 3 şablonu tamamen boş değildir – temel klasör yapısı ekler:</span><span class="sxs-lookup"><span data-stu-id="c6252-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="c6252-148">ASP.NET MVC klasör adları için bazı temel adlandırma kurallarını kullanır:</span><span class="sxs-lookup"><span data-stu-id="c6252-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="c6252-149">**Klasör**</span><span class="sxs-lookup"><span data-stu-id="c6252-149">**Folder**</span></span> | <span data-ttu-id="c6252-150">**Amaç**</span><span class="sxs-lookup"><span data-stu-id="c6252-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="c6252-151">**/ Denetleyicileri**</span><span class="sxs-lookup"><span data-stu-id="c6252-151">**/Controllers**</span></span> | <span data-ttu-id="c6252-152">Denetleyicileri yanıt tarayıcıdan giriş, ne ile bunu ve kullanıcıya bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="c6252-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="c6252-153">**/ Görünümler**</span><span class="sxs-lookup"><span data-stu-id="c6252-153">**/Views**</span></span> | <span data-ttu-id="c6252-154">UI şablonlarımızı görünümleri tutun</span><span class="sxs-lookup"><span data-stu-id="c6252-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="c6252-155">**/ Modelleri**</span><span class="sxs-lookup"><span data-stu-id="c6252-155">**/Models**</span></span> | <span data-ttu-id="c6252-156">Modelleri basılı tutun ve verilerini işleme</span><span class="sxs-lookup"><span data-stu-id="c6252-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="c6252-157">**/ İçerik**</span><span class="sxs-lookup"><span data-stu-id="c6252-157">**/Content**</span></span> | <span data-ttu-id="c6252-158">Bu klasör görüntülerimizi, CSS ve diğer statik içeriklerdeki tutar.</span><span class="sxs-lookup"><span data-stu-id="c6252-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="c6252-159">**/ Komut dosyaları**</span><span class="sxs-lookup"><span data-stu-id="c6252-159">**/Scripts**</span></span> | <span data-ttu-id="c6252-160">Bu klasör tutan sunduğumuz JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="c6252-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="c6252-161">ASP.NET MVC çerçevesi varsayılan olarak "kuralı yapılandırmanız üzerinde" bir yaklaşım kullanır ve klasör adlandırma kurallarına dayalı bazı varsayılan varsayımlarda bulunur çünkü bu klasörleri bile bir boş ASP.NET MVC uygulamasındaki dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="c6252-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="c6252-162">Örneği için denetleyicileri görünümleri görünümleri klasöründe varsayılan olarak bu kodunuzda açıkça belirtmeniz gerek kalmadan arayın.</span><span class="sxs-lookup"><span data-stu-id="c6252-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="c6252-163">Varsayılan kuralları ile kalmanız yazmak için gereken kod miktarını azaltır ve ayrıca, projenizi anlamak diğer geliştiriciler için kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="c6252-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="c6252-164">Bu kuralları uygulamamız ekleriz gibi daha fazla açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="c6252-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c6252-165">Next</span><span class="sxs-lookup"><span data-stu-id="c6252-165">Next</span></span>](mvc-music-store-part-2.md)
