---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: '1. Kısım: Genel bakış ve Yeni Proje -> Dosya | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 kapak genel bakış ve Dosya -> Yeni proje.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="78be0-104">1. Kısım: Genel bakış ve Yeni Proje -> Dosya</span><span class="sxs-lookup"><span data-stu-id="78be0-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="78be0-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="78be0-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="78be0-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="78be0-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="78be0-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="78be0-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="78be0-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="78be0-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="78be0-109">Bölüm 1 kapsayan genel bakış ve dosya -&gt;yeni proje.</span><span class="sxs-lookup"><span data-stu-id="78be0-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="78be0-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="78be0-110">Overview</span></span>

<span data-ttu-id="78be0-111">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Web Developer web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="78be0-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="78be0-112">Biz yavaş başlatma, başlangıç düzeyi web geliştirme deneyimi için uygundur.</span><span class="sxs-lookup"><span data-stu-id="78be0-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="78be0-113">Biz oluşturmakta uygulama basit müzik deposudur.</span><span class="sxs-lookup"><span data-stu-id="78be0-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="78be0-114">Uygulamayı üç ana bölümü vardır: alışveriş, kullanıma alma ve yönetim.</span><span class="sxs-lookup"><span data-stu-id="78be0-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="78be0-115">Ziyaretçilerin Tarz albümlerini göz atabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="78be0-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="78be0-116">Tek bir albümü görüntüleyebilir ve bunların Sepete Ekle:</span><span class="sxs-lookup"><span data-stu-id="78be0-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="78be0-117">Bunlar artık istedikleri öğeleri kaldırma kendi Sepeti gözden geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="78be0-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="78be0-118">Checkout geçmeden oturum açmak veya kaydetmek için bir kullanıcı hesabı için bunları ister.</span><span class="sxs-lookup"><span data-stu-id="78be0-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="78be0-119">Bir hesap oluşturduktan sonra bunların sırası aktarma ve ödeme bilgilerini doldurarak tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78be0-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="78be0-120">Örneği basit tutmak için size harika bir yükseltme çalıştırıyorsanız: her şeyi promosyon kodu "Serbest" girerseniz ücretsizdir!</span><span class="sxs-lookup"><span data-stu-id="78be0-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="78be0-121">Sıralandıktan sonra bunlar bir basit onay ekranı bakın:</span><span class="sxs-lookup"><span data-stu-id="78be0-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="78be0-122">Müşteri faceing sayfalarına ek olarak Biz ayrıca, yöneticiler oluşturabilir, albümleri düzenleme, bir listesini gösterir bir yönetici bölümü oluşturmak ve albümleri silin:</span><span class="sxs-lookup"><span data-stu-id="78be0-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="78be0-123">1. Dosya -&gt; yeni proje</span><span class="sxs-lookup"><span data-stu-id="78be0-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="78be0-124">Yazılım yükleme</span><span class="sxs-lookup"><span data-stu-id="78be0-124">Installing the software</span></span>

<span data-ttu-id="78be0-125">Bu öğretici ücretsiz Visual Web Developer 2010 (olan ücretsiz) Express kullanarak yeni bir ASP.NET MVC 3 projesi oluşturarak başlar ve ardından tam çalışan bir uygulamayı oluşturmak için özellikleri artımlı olarak ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="78be0-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="78be0-126">Yol boyunca biz veritabanı erişimi, form nakil senaryoları, veri doğrulama, ana sayfalar için tutarlı sayfa düzeni, AJAX Sayfa güncelleştirmelerini ve doğrulama, kullanıcı oturum açma ve daha fazla bilgi için kullanarak ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="78be0-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="78be0-127">Adım adım izleyebilirsiniz veya tamamlanmış uygulamadan indirebilirsiniz [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="78be0-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="78be0-128">Uygulamanızı oluşturmak için Visual Studio 2010 SP1 veya Visual Web Developer 2010 Express SP1 (Visual Studio 2010 'un boş bir sürüm) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78be0-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="78be0-129">Biz SQL Server Compact (de serbest) veritabanını barındırmak için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="78be0-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="78be0-130">Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="78be0-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="78be0-131">[Visual Studio Web Developer Express SP1 Önkoşullar]</span><span class="sxs-lookup"><span data-stu-id="78be0-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="78be0-132">[ASP.NET MVC 3 araçları güncelleştirme]</span><span class="sxs-lookup"><span data-stu-id="78be0-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="78be0-133">[SQL Server Compact 4.0] - çalışma zamanı ve Araçlar desteği dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="78be0-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="78be0-134">Yeni bir ASP.NET MVC 3 projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="78be0-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="78be0-135">Visual Web Developer Dosya menüsünden "Yeni Proje" seçerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="78be0-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="78be0-136">Yeni Proje iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="78be0-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="78be0-137">Biz Visual C# - seçersiniz&gt; Web şablonları grup sol tarafta, sonra merkezi sütununda "ASP.NET MVC 3 Web uygulaması" şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="78be0-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="78be0-138">MvcMusicStore projenizi adlandırın ve Tamam düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="78be0-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="78be0-139">Bu bizim proje için bazı MVC belirli ayarları hale getirmemize sağlayan ikincil bir iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="78be0-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="78be0-140">Aşağıdakileri seçin:</span><span class="sxs-lookup"><span data-stu-id="78be0-140">Select the following:</span></span>

<span data-ttu-id="78be0-141">Şablonu proje - boş seçin</span><span class="sxs-lookup"><span data-stu-id="78be0-141">Project Template - select Empty</span></span>

<span data-ttu-id="78be0-142">Altyapısı görüntüleme - Razor seçin</span><span class="sxs-lookup"><span data-stu-id="78be0-142">View Engine - select Razor</span></span>

<span data-ttu-id="78be0-143">HTML5 anlamsal biçimlendirme - işaretli kullanın</span><span class="sxs-lookup"><span data-stu-id="78be0-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="78be0-144">Ayarlarınızı aşağıda gösterildiği gibi ardından Tamam düğmesine basın doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="78be0-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="78be0-145">Bu bizim projesi oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="78be0-145">This will create our project.</span></span> <span data-ttu-id="78be0-146">Uygulamamız sağ tarafında Çözüm Gezgini'nde eklenmiş klasörleri bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="78be0-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="78be0-147">Boş MVC 3 şablon tamamen boş değil – temel klasör yapısı ekler:</span><span class="sxs-lookup"><span data-stu-id="78be0-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="78be0-148">ASP.NET MVC klasör adları için bazı temel adlandırma kuralları kullanır:</span><span class="sxs-lookup"><span data-stu-id="78be0-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="78be0-149">**Klasör**</span><span class="sxs-lookup"><span data-stu-id="78be0-149">**Folder**</span></span> | <span data-ttu-id="78be0-150">**Amaç**</span><span class="sxs-lookup"><span data-stu-id="78be0-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="78be0-151">**/ Denetleyicileri**</span><span class="sxs-lookup"><span data-stu-id="78be0-151">**/Controllers**</span></span> | <span data-ttu-id="78be0-152">Denetleyicileri yanıt tarayıcıdan giriş, kendisiyle yapın ve yanıtın kullanıcıya dönmek karar vermek için.</span><span class="sxs-lookup"><span data-stu-id="78be0-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="78be0-153">**/ Görünümleri**</span><span class="sxs-lookup"><span data-stu-id="78be0-153">**/Views**</span></span> | <span data-ttu-id="78be0-154">UI şablonlarımız görünümleri tutun</span><span class="sxs-lookup"><span data-stu-id="78be0-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="78be0-155">**/ Modelleri**</span><span class="sxs-lookup"><span data-stu-id="78be0-155">**/Models**</span></span> | <span data-ttu-id="78be0-156">Modelleri basılı tutun ve verileri işlemek</span><span class="sxs-lookup"><span data-stu-id="78be0-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="78be0-157">**/ İçeriği**</span><span class="sxs-lookup"><span data-stu-id="78be0-157">**/Content**</span></span> | <span data-ttu-id="78be0-158">Bu klasör bizim görüntüleri, CSS ve diğer statik içeriği tutar</span><span class="sxs-lookup"><span data-stu-id="78be0-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="78be0-159">**/ Komut dosyaları**</span><span class="sxs-lookup"><span data-stu-id="78be0-159">**/Scripts**</span></span> | <span data-ttu-id="78be0-160">Bu klasör bizim JavaScript dosyaları tutar</span><span class="sxs-lookup"><span data-stu-id="78be0-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="78be0-161">ASP.NET MVC çerçevesi varsayılan olarak "kuralı yapılandırması üzerinden" bir yaklaşım kullanır ve klasör adlandırma kurallarına göre bazı varsayılan varsayımlar yapar çünkü bu klasörleri bile bir boş ASP.NET MVC uygulamasındaki dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="78be0-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="78be0-162">Örneğin, denetleyicileri görünümler klasöründe görünümlerinde varsayılan olarak bu kodunuzda açıkça belirtmek zorunda kalmadan arayın.</span><span class="sxs-lookup"><span data-stu-id="78be0-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="78be0-163">Varsayılan kuralları ile kalmanız yazmak için gereken kod miktarını azaltır ve aynı zamanda, projenizin anlamak diğer geliştiriciler için kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="78be0-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="78be0-164">Bu kuralları biz uygulamamız yapı gibi daha fazla açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="78be0-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="78be0-165">Next</span><span class="sxs-lookup"><span data-stu-id="78be0-165">Next</span></span>](mvc-music-store-part-2.md)
