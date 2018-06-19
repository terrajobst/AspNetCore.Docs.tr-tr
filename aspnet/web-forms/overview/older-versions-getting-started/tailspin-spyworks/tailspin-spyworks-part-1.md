---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Bölüm 1: Dosya -> Yeni Proje | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 genel bakış ve dosya/yeni proje kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892471"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="d0f47-104">Bölüm 1: Dosya -> Yeni Proje</span><span class="sxs-lookup"><span data-stu-id="d0f47-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="d0f47-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d0f47-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d0f47-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d0f47-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d0f47-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d0f47-109">Bölüm 1 genel bakış ve dosya/yeni proje kapsar.</span><span class="sxs-lookup"><span data-stu-id="d0f47-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="d0f47-110">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="d0f47-110">Overview</span></span>

<span data-ttu-id="d0f47-111">Bu öğretici ASP.NET WebForms giriş ' dir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="d0f47-112">Biz yavaş başlatma, başlangıç düzeyi web geliştirme deneyimi için uygundur.</span><span class="sxs-lookup"><span data-stu-id="d0f47-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="d0f47-113">Biz oluşturma basit bir çevrimiçi mağaza uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="d0f47-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="d0f47-114">Ziyaretçilerin ürünleri kategoriye göre göz atabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d0f47-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="d0f47-115">Tek bir ürün görüntüleyebilir ve bunların Sepete Ekle:</span><span class="sxs-lookup"><span data-stu-id="d0f47-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="d0f47-116">Bunlar artık istedikleri öğeleri kaldırma kendi Sepeti gözden geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d0f47-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="d0f47-117">Checkout izlemeye devam etmeden bunları ister</span><span class="sxs-lookup"><span data-stu-id="d0f47-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="d0f47-118">Sıralandıktan sonra bunlar bir basit onay ekranı bakın:</span><span class="sxs-lookup"><span data-stu-id="d0f47-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="d0f47-119">Visual Studio 2010'da yeni bir ASP.NET WebForms projesi oluşturarak başlamadan ve tam çalışan bir uygulamayı oluşturmak için özellikleri artımlı olarak ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="d0f47-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="d0f47-120">Yol boyunca biz veritabanı erişimi, liste ve kılavuz görünümleri, veri güncelleştirme sayfaları, veri doğrulama tutarlı sayfa düzeni, AJAX, doğrulama, kullanıcı üyeliği ve daha fazla bilgi için ana sayfalar kullanarak ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="d0f47-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="d0f47-121">Adım adım izleyebilirsiniz veya tamamlanmış uygulamadan indirebilirsiniz [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="d0f47-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="d0f47-122">Visual Studio 2010 veya ücretsiz Visual Web Developer 2010'dan kullanabileceğiniz [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="d0f47-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="d0f47-123">Uygulama oluşturmak için SQL Server veya ücretsiz SQL Server Express ana veritabanı için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0f47-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="d0f47-124">Dosya / yeni proje</span><span class="sxs-lookup"><span data-stu-id="d0f47-124">File / New Project</span></span>

<span data-ttu-id="d0f47-125">Dosya menüsünde Visual Studio'da yeni proje seçerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="d0f47-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="d0f47-126">Yeni Proje iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="d0f47-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="d0f47-127">Biz Visual C# seçersiniz / Web şablonları grup sol tarafta ve ardından merkezi sütununda "ASP.NET Web uygulaması" şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d0f47-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="d0f47-128">TailspinSpyworks projenizi adlandırın ve Tamam düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="d0f47-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="d0f47-129">Bu bizim projesi oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d0f47-129">This will create our project.</span></span> <span data-ttu-id="d0f47-130">Sağ taraftaki Çözüm Gezgini'nde uygulamamız içinde yer alan klasörleri bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="d0f47-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="d0f47-131">Boş çözüm tamamen boş olmayan – temel klasör yapısı ekler:</span><span class="sxs-lookup"><span data-stu-id="d0f47-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="d0f47-132">ASP.NET 4 varsayılan proje şablonu tarafından uygulanan kuralları unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d0f47-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="d0f47-133">"Hesap" klasörü ASP için bir temel kullanıcı arabirimini uygular. NET'in üyelik alt sistemi.</span><span class="sxs-lookup"><span data-stu-id="d0f47-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="d0f47-134">İstemci tarafı JavaScript dosyaları için depo "Scripts" klasörü görür ve çekirdek jQuery .js dosyaları varsayılan olarak kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="d0f47-135">"Stilleri" klasörü bizim web sitesi görselleri (CSS stil sayfaları) düzenlemek için kullanılır</span><span class="sxs-lookup"><span data-stu-id="d0f47-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="d0f47-136">Biz, biz uygulamamızı çalıştırmak ve default.aspx sayfasında işlemek için F5 tuşuna bastığınızda aşağıdakilere bakın.</span><span class="sxs-lookup"><span data-stu-id="d0f47-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="d0f47-137">Bizim ilk uygulama geliştirme, CSS sınıfları ve Tailspin Spyworks uygulamamız için istiyoruz visual asthetics kılacak ilişkili görüntü dosyaları varsayılan WebForms şablondan Style.css dosyasını değiştirmek için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d0f47-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="d0f47-138">Bunu yaptıktan sonra default.aspx sayfamızı şu şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="d0f47-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="d0f47-139">Üst görüntü bağlantılar fark sağında sayfa ve ana sayfaya eklenen menü öğeleri.</span><span class="sxs-lookup"><span data-stu-id="d0f47-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="d0f47-140">(Varsayılan şablon tarafından oluşturulan) mevcut sayfaları ve biz uygulamamız yapı olarak biz gerçekleştireceksiniz sayfaları geri kalanı için yalnızca "Oturum Aç" ve "Hesap" bağlantıları gelin.</span><span class="sxs-lookup"><span data-stu-id="d0f47-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="d0f47-141">Ayrıca ana sayfa stilleri dizinine taşıyabilir yapacağız.</span><span class="sxs-lookup"><span data-stu-id="d0f47-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="d0f47-142">Bu yalnızca bir tercih olsa uygulamamız gelecekte "skinable" olun karar verirseniz şeyler biraz kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="d0f47-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="d0f47-143">Ana sayfa değiştirmek için gerekir yaptıktan sonra tüm .aspx dosyalarını başvurularında ASP.NET WebForms sayfaları varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d0f47-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d0f47-144">Next</span><span class="sxs-lookup"><span data-stu-id="d0f47-144">Next</span></span>](tailspin-spyworks-part-2.md)
