---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: '1. Bölüm: Dosya -> Yeni Proje | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 1. Bölüm genel bakış ve dosya/yeni proje kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 8c346e88277b6cf43676f7c6b58f9679a591d634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388217"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="c9a92-104">1. Bölüm: Dosya -> Yeni Proje</span><span class="sxs-lookup"><span data-stu-id="c9a92-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="c9a92-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c9a92-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="c9a92-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9a92-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="c9a92-107">Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9a92-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="c9a92-108">Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c9a92-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="c9a92-109">1. Bölüm genel bakış ve dosya/yeni proje kapsar.</span><span class="sxs-lookup"><span data-stu-id="c9a92-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="c9a92-110">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="c9a92-110">Overview</span></span>

<span data-ttu-id="c9a92-111">Bu öğreticide, ASP.NET WebForms giriş niteliğindedir.</span><span class="sxs-lookup"><span data-stu-id="c9a92-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="c9a92-112">Biz, yavaş başlatma, başlangıç düzeyi bir web geliştirme deneyimi için uygundur.</span><span class="sxs-lookup"><span data-stu-id="c9a92-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="c9a92-113">Biz oluşturuyor olacaksınız basit bir çevrimiçi mağaza uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c9a92-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="c9a92-114">Ziyaretçi ürünleri kategoriye göre göz atabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c9a92-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="c9a92-115">Tek bir ürün görüntülemek ve bunların sepetinize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c9a92-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="c9a92-116">Bunlar, artık istedikleri tüm öğeleri kaldırma, kendi Sepeti gözden geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c9a92-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="c9a92-117">Kullanıma alma için devam etmeden bunları ister.</span><span class="sxs-lookup"><span data-stu-id="c9a92-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="c9a92-118">Sıralandıktan sonra bunlar basit bir onay ekranı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="c9a92-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="c9a92-119">Biz Visual Studio 2010'da yeni bir ASP.NET WebForms projesi oluşturarak başlarsınız ve eksiksiz, çalışan bir uygulamayı oluşturmak için özellikler artımlı olarak ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c9a92-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="c9a92-120">Bu doğrultuda, biz veritabanı erişimi, liste ve kılavuz görünümleri, veri güncelleştirme sayfaları, veri doğrulama tutarlı sayfa düzeni, AJAX, doğrulama, kullanıcının üyelik ve daha fazla bilgi için ana sayfaları kullanarak ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="c9a92-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="c9a92-121">Adım adım takip edebilirsiniz veya tamamlanmış uygulamayı karşıdan yükleme [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="c9a92-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="c9a92-122">Visual Studio 2010 veya ücretsiz Visual Web Developer 2010'dan kullanabileceğiniz [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="c9a92-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="c9a92-123">Uygulamayı oluşturmak için SQL Server veya ücretsiz SQL Server Express veritabanı barındırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9a92-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="c9a92-124">Dosya / yeni proje</span><span class="sxs-lookup"><span data-stu-id="c9a92-124">File / New Project</span></span>

<span data-ttu-id="c9a92-125">Yeni Proje Visual Studio'da Dosya menüsü seçerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="c9a92-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="c9a92-126">Bu, yeni proje iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="c9a92-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="c9a92-127">Visual C# seçeneğini belirleyeceğiz / Web şablonları Grup solda ve ardından Orta sütundaki "ASP.NET Web uygulaması" şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="c9a92-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="c9a92-128">TailspinSpyworks projenizi adlandırın ve Tamam düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="c9a92-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="c9a92-129">Bu, bizim projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c9a92-129">This will create our project.</span></span> <span data-ttu-id="c9a92-130">Uygulamamızı Çözüm Gezgini'nde sağ tarafta bulunan klasörleri bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="c9a92-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="c9a92-131">Boş çözüm tamamen boş değildir – temel klasör yapısı ekler:</span><span class="sxs-lookup"><span data-stu-id="c9a92-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="c9a92-132">ASP.NET 4 varsayılan proje şablonu tarafından uygulanan kuralları unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c9a92-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="c9a92-133">"Hesap" klasörü, ASP temel kullanıcı arabirimini uygular. NET üyeliği alt sistemi.</span><span class="sxs-lookup"><span data-stu-id="c9a92-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="c9a92-134">İstemci tarafı JavaScript dosyaları için depo "Betikler" klasörüne görür ve çekirdek jQuery .js dosyaları varsayılan olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="c9a92-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="c9a92-135">"Stilleri" klasörü, web sitesi görsellerimize (CSS stil sayfaları) düzenlemek için kullanılır</span><span class="sxs-lookup"><span data-stu-id="c9a92-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="c9a92-136">Biz default.aspx sayfayı oluşturmak ve uygulamamızı çalıştırmak için F5 tuşuna bastığınızda seçtiğimizde aşağıdaki seçenekleri görüyoruz.</span><span class="sxs-lookup"><span data-stu-id="c9a92-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="c9a92-137">Tailspin Spyworks uygulamamız için istediğimiz visual asthetics işlenir ilişkili görüntü dosyaları ve CSS sınıfları varsayılan WebForms şablondan Style.css dosyasını değiştirmek için ilk uygulama geliştirme olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c9a92-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="c9a92-138">Bunu yaptıktan sonra default.aspx sayfamızı şu şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="c9a92-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="c9a92-139">Üst görüntü bağlantılarının fark sağında sayfası ve ana sayfaya eklenen menü öğeleri.</span><span class="sxs-lookup"><span data-stu-id="c9a92-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="c9a92-140">Yalnızca "Oturum açma" ve "Hesap" bağlantıları (varsayılan şablon tarafından oluşturulan) mevcut sayfalar ve kalan uygulamamız ekleriz gibi biz uygulayacak sayfalarında işaret.</span><span class="sxs-lookup"><span data-stu-id="c9a92-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="c9a92-141">Bunu ana sayfa stilleri dizine dışında yeniden konumlandırmakta dağıtacağız.</span><span class="sxs-lookup"><span data-stu-id="c9a92-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="c9a92-142">Yalnızca bir tercih budur ancak biz uygulamamız "skinable" gelecekte yapmaya karar verirseniz şeyler biraz kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="c9a92-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="c9a92-143">Biz ana sayfaya değiştirmeniz gerekir Bunu yaptıktan sonra tüm .aspx dosyalarını başvuruları ASP.NET WebForms sayfaları varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9a92-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c9a92-144">Next</span><span class="sxs-lookup"><span data-stu-id="c9a92-144">Next</span></span>](tailspin-spyworks-part-2.md)
