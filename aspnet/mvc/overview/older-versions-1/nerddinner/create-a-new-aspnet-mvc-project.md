---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "Yeni bir ASP.NET MVC projesi oluşturun | Microsoft Docs"
author: microsoft
description: "1. adım temel NerdDinner uygulama yapısı yerleştirdiniz gösterilmiştir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="59fc5-103">Yeni bir ASP.NET MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="59fc5-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="59fc5-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59fc5-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="59fc5-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="59fc5-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="59fc5-106">Adım 1 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="59fc5-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="59fc5-107">1. adım temel NerdDinner uygulama yapısı yerleştirdiniz gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="59fc5-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="59fc5-108">ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="59fc5-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="59fc5-109">NerdDinner adım 1: Dosya -&gt;yeni proje</span><span class="sxs-lookup"><span data-stu-id="59fc5-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="59fc5-110">Biz seçerek NerdDinner uygulamamız başlarsınız **dosya -&gt;yeni proje** menü öğesi içinde Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="59fc5-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="59fc5-111">Bu, "Yeni Proje" iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="59fc5-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="59fc5-112">Yeni bir ASP.NET MVC uygulaması oluşturmak için iletişim kutusunun sol taraftaki "Web" düğümünü seçin ve ardından sağdaki "ASP.NET MVC Web uygulaması" proje şablonu seçin:</span><span class="sxs-lookup"><span data-stu-id="59fc5-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="59fc5-113">*Önemli: indirilir ve ASP.NET yeni proje iletişim kutusunda görüntülenmeyecektir MVC - Aksi takdirde yüklü olduğundan emin olun. V2 olarak kullanabileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) , onu henüz yüklemediyseniz (ASP.NET MVC içinde kullanılabilir "Web Platformu -&gt;çerçeveler ve çalışma zamanları" bölümü).*</span><span class="sxs-lookup"><span data-stu-id="59fc5-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="59fc5-114">Biz, biz "NerdDinner" oluşturabilir ve oluşturmak için "Tamam" düğmesini tıklatın olacak yeni proje adı.</span><span class="sxs-lookup"><span data-stu-id="59fc5-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="59fc5-115">Biz "Tamam" düğmesini tıklatın, Visual Studio ister bize isteğe bağlı olarak da yeni uygulama için birim testi projesi oluşturmak için ek bir iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="59fc5-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="59fc5-116">Bu birim testi projesi bizi uygulamamız davranışını ve işlevlerinin doğrulayın otomatikleştirilmiş test oluşturmak sağlar (bir şey kapak nasıl daha sonra Bu öğreticide Yapılacaklar).</span><span class="sxs-lookup"><span data-stu-id="59fc5-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="59fc5-117">Yukarıdaki iletişim kutusunda "Test framework" açılır makinede yüklü tüm kullanılabilir ASP.NET MVC birim testi projesi şablonları ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="59fc5-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="59fc5-118">Sürümleri NUnit, MBUnit ve XUnit indirilebilir.</span><span class="sxs-lookup"><span data-stu-id="59fc5-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="59fc5-119">Yerleşik Visual Studio birim testi çerçevesi da desteklenir.</span><span class="sxs-lookup"><span data-stu-id="59fc5-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="59fc5-120">*Not: Visual Studio Birim Test Çerçevesi yalnızca Visual Studio 2008 Professional ve daha sonraki sürümler ile kullanılabilir. VS 2008 Standard Edition veya Visual Web Developer 2008 Express kullanıyorsanız yükleyip NUnit, MBUnit veya XUnit uzantıları için ASP.NET MVC gösterilmesi için bu iletişim kutusu için sırayla gerekecektir. Yüklü herhangi bir test çerçeve yoksa iletişim kutusunda görüntülenmez.*</span><span class="sxs-lookup"><span data-stu-id="59fc5-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="59fc5-121">Biz oluşturuyoruz test projesi için varsayılan "NerdDinner.Tests" adı kullanın ve "Visual Studio Birim Test" framework seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="59fc5-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="59fc5-122">Biz tıklattığınızda "Tamam" düğmesine Visual Studio çözüm bize bu - web uygulamamız için diğeri bizim birim testleri için iki proje ile oluşturacak:</span><span class="sxs-lookup"><span data-stu-id="59fc5-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="59fc5-123">NerdDinner dizin yapısını inceleniyor</span><span class="sxs-lookup"><span data-stu-id="59fc5-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="59fc5-124">Visual Studio ile yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, projeye otomatik olarak bir dizi dosyaları ve dizinleri ekler:</span><span class="sxs-lookup"><span data-stu-id="59fc5-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="59fc5-125">Varsayılan olarak ASP.NET MVC projeleri altı en üst düzey dizinleri sahiptir:</span><span class="sxs-lookup"><span data-stu-id="59fc5-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="59fc5-126">**Dizin**</span><span class="sxs-lookup"><span data-stu-id="59fc5-126">**Directory**</span></span> | <span data-ttu-id="59fc5-127">**Amaç**</span><span class="sxs-lookup"><span data-stu-id="59fc5-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="59fc5-128">**/ Denetleyicileri**</span><span class="sxs-lookup"><span data-stu-id="59fc5-128">**/Controllers**</span></span> | <span data-ttu-id="59fc5-129">Burada URL isteklerini işleyen denetleyici sınıfları yerleştirme</span><span class="sxs-lookup"><span data-stu-id="59fc5-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="59fc5-130">**/ Modelleri**</span><span class="sxs-lookup"><span data-stu-id="59fc5-130">**/Models**</span></span> | <span data-ttu-id="59fc5-131">Burada temsil eder ve verileri işlemek sınıfları yerleştirme</span><span class="sxs-lookup"><span data-stu-id="59fc5-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="59fc5-132">**/ Görünümleri**</span><span class="sxs-lookup"><span data-stu-id="59fc5-132">**/Views**</span></span> | <span data-ttu-id="59fc5-133">Burada işleme çıktısı için sorumlu kullanıcı Arabirimi şablon dosyalarını yerleştirme</span><span class="sxs-lookup"><span data-stu-id="59fc5-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="59fc5-134">**/ Komut dosyaları**</span><span class="sxs-lookup"><span data-stu-id="59fc5-134">**/Scripts**</span></span> | <span data-ttu-id="59fc5-135">Burada JavaScript kitaplığı dosyaları ve komut dosyaları (.js) yerleştirin</span><span class="sxs-lookup"><span data-stu-id="59fc5-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="59fc5-136">**/ İçeriği**</span><span class="sxs-lookup"><span data-stu-id="59fc5-136">**/Content**</span></span> | <span data-ttu-id="59fc5-137">Burada, CSS ve görüntü dosyaları ve diğer olmayan-dinamik/olmayan-JavaScript içeriği yerleştirme</span><span class="sxs-lookup"><span data-stu-id="59fc5-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="59fc5-138">**/ Uygulama\_veri**</span><span class="sxs-lookup"><span data-stu-id="59fc5-138">**/App\_Data**</span></span> | <span data-ttu-id="59fc5-139">Veri dosyalarını depolamak burada okuma/yazma istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="59fc5-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="59fc5-140">ASP.NET MVC bu yapı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="59fc5-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="59fc5-141">Aslında, büyük uygulamalar üzerinde çalışan geliştiriciler genellikle uygulama yukarı daha kolay yönetilebilmesi için birden çok projede bölüm (örneğin: veri modeli sınıflarını genellikle web uygulamasından ayrı sınıf kitaplığında Git).</span><span class="sxs-lookup"><span data-stu-id="59fc5-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="59fc5-142">Varsayılan proje yapısı, ancak bizim uygulama sorunları temiz tutma kullanırız iyi varsayılan dizin kuralını sağlar.</span><span class="sxs-lookup"><span data-stu-id="59fc5-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="59fc5-143">Biz /Controllers dizin genişlettiğinizde Biz Visual Studio varsayılan olarak iki denetleyicisi sınıf – HomeController ve AccountController – projeye eklenen bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59fc5-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="59fc5-144">Biz /Views dizin genişlettiğinizde, biz üç alt dizinleri – /Home, ApplicationTier/account ve /Shared – yanı sıra birkaç şablonu içindeki dosyalara Ayrıca varsayılan projeye eklenen bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59fc5-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="59fc5-145">Biz/scripts dizinler ve/Content genişlettiğinizde, biz ASP.NET AJAX ve jQuery etkinleştirebilirsiniz JavaScript kitaplıklarını yanı sıra tüm HTML sitesinde stilini belirlemek için kullanılan bir Site.css dosya uygulama içerisinden destek bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59fc5-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="59fc5-146">Biz NerdDinner.Tests proje genişlettiğinizde biz bizim denetleyicisi sınıfları için birim testleri içeren iki sınıf bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59fc5-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="59fc5-147">Visual Studio tarafından eklenen bu varsayılan dosyalar, bize çalışan bir uygulama - sayfasında, hesap oturum açma/oturum kapatma/kayıt sayfaları ve bir işlenmemiş bir hata sayfası (tüm kablolu yukarı ve kutunun dışında çalışma) hakkında giriş sayfası için basit bir yapı ile sağlayın.</span><span class="sxs-lookup"><span data-stu-id="59fc5-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="59fc5-148">NerdDinner uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="59fc5-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="59fc5-149">Biz ya da seçerek proje çalıştırabilirsiniz **hata ayıklama -&gt;hata ayıklamayı Başlat** veya **hata ayıklama -&gt;hata ayıklama olmadan Başlat** menü öğeleri:</span><span class="sxs-lookup"><span data-stu-id="59fc5-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="59fc5-150">Bu yerleşik ASP.NET Web-Visual Studio ile birlikte gelen sunucusunu başlatmaya ve uygulamamızı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59fc5-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="59fc5-151">Yeni Projemizin giriş sayfasını aşağıdadır (URL: "/") çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="59fc5-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="59fc5-152">"Hakkında" sekmesini tıklatarak görüntüleyen bir sayfa hakkında (URL: "/ ev/hakkında"):</span><span class="sxs-lookup"><span data-stu-id="59fc5-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="59fc5-153">Sağ üst tarafında "Oturum Aç" bağlantısını tıklatarak alır bize bir oturum açma sayfasına (URL: "/ Account/oturum açma")</span><span class="sxs-lookup"><span data-stu-id="59fc5-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="59fc5-154">Biz kayıt bağlantıyı tıklatabilir bir oturum açma hesabı yoksa (URL: "/ Account/Register") oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="59fc5-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="59fc5-155">Oturum kapatma ve hakkında yukarıdaki Giriş uygulamak için kodu Register işlevselliği, biz bizim yeni bir proje oluşturduğunuzda varsayılan olarak eklendi.</span><span class="sxs-lookup"><span data-stu-id="59fc5-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="59fc5-156">Uygulama başlangıç noktası olarak bunu kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="59fc5-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="59fc5-157">NerdDinner uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="59fc5-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="59fc5-158">Biz Professional Edition veya Visual Studio 2008'in daha yüksek sürümünü kullanıyorsanız, biz IDE desteği Visual Studio'dan Test yerleşik birimi projeyi test etmek için kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59fc5-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="59fc5-159">Yukarıdaki seçeneklerden birini seçerek IDE içinde "Test sonuçlarını" bölmesini açın ve bize yerleşik işlevselliği kapak 27 birim testlerini bizim yeni projeye dahil geçişi/başarısız durumuna sağlar:</span><span class="sxs-lookup"><span data-stu-id="59fc5-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="59fc5-160">Daha sonra Bu öğreticide biz otomatikleştirilmiş testleri hakkında daha fazla konuşun ve biz uygulamak uygulama işlevsellikler içeren ek birim testleri ekleme.</span><span class="sxs-lookup"><span data-stu-id="59fc5-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="59fc5-161">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="59fc5-161">Next Step</span></span>

<span data-ttu-id="59fc5-162">Temel uygulama yapısı artık yerinde açıyor.</span><span class="sxs-lookup"><span data-stu-id="59fc5-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="59fc5-163">Şimdi, şimdi [bizim uygulama verilerini depolamak için bir veritabanı oluşturun](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="59fc5-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="59fc5-164">[Önceki](introducing-the-nerddinner-tutorial.md)
[sonraki](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="59fc5-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
