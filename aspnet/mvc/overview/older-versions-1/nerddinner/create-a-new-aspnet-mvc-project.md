---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Yeni bir ASP.NET MVC projesi oluşturun | Microsoft Docs
author: microsoft
description: 1. adım yerleştirdiniz temel NerdDinner uygulama yapısı gösterilmektedir.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 3f34f17aa35dbfed2d52daf615c8dc81be6e7847
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754226"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="ef2cb-103">Yeni bir ASP.NET MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ef2cb-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="ef2cb-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ef2cb-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ef2cb-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="ef2cb-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="ef2cb-106">Adım 1 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="ef2cb-107">1. adım yerleştirdiniz temel NerdDinner uygulama yapısı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="ef2cb-108">ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="ef2cb-109">NerdDinner adım 1: Dosya -&gt;yeni proje</span><span class="sxs-lookup"><span data-stu-id="ef2cb-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="ef2cb-110">Biz seçerek NerdDinner uygulamamız başlarsınız **File -&gt;yeni proje** Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="ef2cb-111">Bu, "Yeni Proje" iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="ef2cb-112">Yeni bir ASP.NET MVC uygulaması oluşturmak için biz iletişim kutusunun sol taraftaki "Web" düğümü seçin ve ardından sağdaki "ASP.NET MVC Web uygulaması" proje şablonu seçin:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="ef2cb-113">*Önemli: indirilen ve ASP.NET yeni proje iletişim kutusunda görünmez MVC - aksi yüklü olduğundan emin olun. V2, kullanabileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) henüz yüklemediyseniz, (ASP.NET MVC, içinde kullanılabilir "Web Platformu -&gt;çerçeveleri ve çalışma zamanları" bölümü).*</span><span class="sxs-lookup"><span data-stu-id="ef2cb-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="ef2cb-114">Biz "NerdDinner" oluşturup oluşturmak için "Tamam" düğmesini tıklatıp kullanacağız yeni proje adı.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="ef2cb-115">Biz "Tamam" düğmesine tıklayın, Visual Studio ister bize isteğe bağlı olarak birim testi projesi için de yeni uygulama oluşturmak için ek bir iletişim kutusu çıkarır.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="ef2cb-116">Bu birim testi projesi uygulamamız davranışını ve işlevsellik doğrulamak otomatik testler oluşturma sağlıyor (bir şey ele nasıl Bu öğreticinin sonraki adımlarında Yapılacaklar).</span><span class="sxs-lookup"><span data-stu-id="ef2cb-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="ef2cb-117">"Test Çerçevesi" açılan yukarıdaki iletişim kutusunda, makinede yüklü tüm kullanılabilir ASP.NET MVC birim test projesi şablonları ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="ef2cb-118">NUnit, MBUnit ve XUnit sürümleri indirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="ef2cb-119">Yerleşik Visual Studio birim testi çerçevesi de desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="ef2cb-120">*Not: Visual Studio birim testi çerçevesi yalnızca Visual Studio 2008 Professional ve üzeri sürümlerde kullanılabilir. VS 2008 Standard Edition veya Visual Web Developer 2008 Express kullanıyorsanız indirip sırada gösterilecek bu iletişim için ASP.NET MVC NUnit, MBUnit veya XUnit uzantıları yüklemeniz gerekir. Tüm test çerçeveleri yüklü değilse, iletişim kutusu görüntülenmez.*</span><span class="sxs-lookup"><span data-stu-id="ef2cb-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="ef2cb-121">Biz oluştururuz test projesi için varsayılan "NerdDinner.Tests" adını kullanın ve "Visual Studio birim testi" framework seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="ef2cb-122">Biz tıkladığınızda Visual Studio "Tamam" düğmesine bir çözüm bizim için - bir web uygulamamız için ve bir birim testlerimiz için iki proje oluşturur:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="ef2cb-123">NerdDinner dizin yapısını İnceleme</span><span class="sxs-lookup"><span data-stu-id="ef2cb-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="ef2cb-124">Visual Studio ile yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, projeye otomatik olarak bir dizi dosyaları ve dizinleri ekler:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="ef2cb-125">ASP.NET MVC projeleri varsayılan olarak altı en üst düzey dizinleriniz bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="ef2cb-126">**Dizin**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-126">**Directory**</span></span> | <span data-ttu-id="ef2cb-127">**Amaç**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="ef2cb-128">**/ Denetleyicileri**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-128">**/Controllers**</span></span> | <span data-ttu-id="ef2cb-129">URL isteklerini işleyen denetleyici sınıflarına yerleştirdiğiniz yere</span><span class="sxs-lookup"><span data-stu-id="ef2cb-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="ef2cb-130">**/ Modelleri**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-130">**/Models**</span></span> | <span data-ttu-id="ef2cb-131">Verileri işlemek ve temsil eden sınıflar yerleştirdiğiniz yere</span><span class="sxs-lookup"><span data-stu-id="ef2cb-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="ef2cb-132">**/ Görünümler**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-132">**/Views**</span></span> | <span data-ttu-id="ef2cb-133">Oluşturma çıkışı için sorumlu kullanıcı Arabirimi şablon dosyaları yerleştirdiğiniz burada</span><span class="sxs-lookup"><span data-stu-id="ef2cb-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="ef2cb-134">**/ Komut dosyaları**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-134">**/Scripts**</span></span> | <span data-ttu-id="ef2cb-135">JavaScript kitaplığı dosyaları ve komut dosyaları (.js) yerleştirdiğiniz yere</span><span class="sxs-lookup"><span data-stu-id="ef2cb-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="ef2cb-136">**/ İçerik**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-136">**/Content**</span></span> | <span data-ttu-id="ef2cb-137">Burada, CSS ve resim dosyaları ve diğer olmayan-dinamik/olmayan-JavaScript içeriği yerleştirin</span><span class="sxs-lookup"><span data-stu-id="ef2cb-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="ef2cb-138">**/ Uygulama\_veri**</span><span class="sxs-lookup"><span data-stu-id="ef2cb-138">**/App\_Data**</span></span> | <span data-ttu-id="ef2cb-139">Burada veri dosyalarını depolamak okuma/yazma istersiniz.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="ef2cb-140">ASP.NET MVC, bu yapı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="ef2cb-141">Aslında, büyük uygulamalar üzerinde çalışan geliştiriciler genellikle uygulamayı oluşturan daha kolay yönetilebilir hale getirmek için birden çok projede bölüm (örneğin: veri modeli sınıfları genellikle ayrı bir sınıf kitaplığı projesinde bir web uygulamasından gidin).</span><span class="sxs-lookup"><span data-stu-id="ef2cb-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="ef2cb-142">Varsayılan proje yapısı, ancak uygulama önde gelen kaygılarımızdan düzenli tutmak için kullanabileceğiniz bir iyi varsayılan dizin kuralı sunar.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="ef2cb-143">Biz /Controllers dizini genişlettiğinizde Biz Visual Studio varsayılan olarak iki denetleyici sınıflarına – HomeController ve AccountController – projeye eklenen bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="ef2cb-144">Biz /Views dizini genişlettiğinizde, biz üç alt dizinleri – /Home/Account ve /Shared – yanı, varsayılan olarak içindeki dosyalara da projeye eklenen birkaç şablon bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="ef2cb-145">Biz/scripts dizinleri ve/Content genişlettiğinizde, biz olanak veren ASP.NET AJAX ve jQuery JavaScript kitaplıklarını yanı sıra tüm HTML sitesinde biçimlendirmek için kullanılan bir Site.css dosyası içinde uygulama destek bulacaksınız:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="ef2cb-146">Biz NerdDinner.Tests proje genişlettiğinizde biz bizim denetleyici sınıflarına için birim testleri içeren iki sınıfınız bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="ef2cb-147">Visual Studio tarafından eklenen bu varsayılan dosyalar, bize için çalışan bir uygulama - ile giriş sayfası, sayfayı, hesap oturum açma/kapatma/kayıt sayfaları ve bir işlenmeyen bir hata sayfası (tüm kablolu yukarı ve yepyeni çalışma) hakkında temel bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="ef2cb-148">NerdDinner uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ef2cb-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="ef2cb-149">Biz ya da seçerek proje çalıştırabilirsiniz **hata ayıklama -&gt;hata ayıklamayı Başlat** veya **hata ayıklama -&gt;hata ayıklama olmadan Başlat** menü öğeleri:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="ef2cb-150">Bu yerleşik ASP.NET Web-Visual Studio ile birlikte gelen sunucusunu başlatmak ve uygulamamızı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="ef2cb-151">Giriş sayfasına yeni Projemizin aşağıda verilmiştir (URL: "/") çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="ef2cb-152">"Hakkında" sekmesini tıklatarak görüntüleyen bir sayfa hakkında (URL: "/ Home/About"):</span><span class="sxs-lookup"><span data-stu-id="ef2cb-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="ef2cb-153">Sağ üst kısmında "Oturum Aç" bağlantısına tıklayarak alan bize bir oturum açma sayfasına (URL: "/ hesabı/oturum açma")</span><span class="sxs-lookup"><span data-stu-id="ef2cb-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="ef2cb-154">Biz Kaydet bağlantısına tıklayabilirsiniz bir oturum açma hesabı yoksa (URL: "/ hesabı/Register") oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="ef2cb-155">Oturum kapatma ve, yukarıdaki Giriş uygulamak için kod / register işlevleri size sunduğumuz yeni proje oluşturulduğunda varsayılan olarak eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="ef2cb-156">Uygulamamızı başlangıç noktası olarak kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="ef2cb-157">NerdDinner uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="ef2cb-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="ef2cb-158">Professional sürümü veya üzeri bir sürüm Visual Studio 2008'in kullanıyoruz, yerleşik birim testi Visual Studio içinde IDE desteği projeyi test etmek için kullanabiliriz:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="ef2cb-159">Yukarıdaki seçeneklerden birini seçerek IDE içinde "Test sonuçlarını" bölmesini açın ve yerleşik işlevi kapsayacak yeni Projemizin dahil 27 birim testleri geçer/başarısız durumuna sahip sağlayın:</span><span class="sxs-lookup"><span data-stu-id="ef2cb-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="ef2cb-160">Bu öğreticinin ilerleyen bölümlerinde biz otomatik testi hakkında daha fazla konuşacak ve biz uygulamak uygulama işlevsellikler ek bir birim testleri ekleme.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="ef2cb-161">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="ef2cb-161">Next Step</span></span>

<span data-ttu-id="ef2cb-162">Temel uygulama yapısı artık yerinde açıyor.</span><span class="sxs-lookup"><span data-stu-id="ef2cb-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="ef2cb-163">Haydi şimdi [bizim uygulama verilerini depolamak için bir veritabanı oluşturma](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="ef2cb-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef2cb-164">[Önceki](introducing-the-nerddinner-tutorial.md)
> [İleri](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="ef2cb-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
