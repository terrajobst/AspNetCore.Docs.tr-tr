---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Bir denetleyicisinden modelinizin verilere | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="26bb7-104">Bir denetleyicisinden modelinizin verilerine erişme</span><span class="sxs-lookup"><span data-stu-id="26bb7-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="26bb7-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="26bb7-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="26bb7-106">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="26bb7-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="26bb7-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="26bb7-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="26bb7-108">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="26bb7-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="26bb7-109">Bu bölümde size yeni bir MoviesController sınıf oluşturun ve bizim film verileri alır ve bir görünüm şablonu kullanarak tarayıcıya görüntüleyen bazı kodlar yazmak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="26bb7-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="26bb7-110">Denetleyicileri klasörü sağ tıklatın ve yeni MoviesController olun.</span><span class="sxs-lookup"><span data-stu-id="26bb7-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="26bb7-111">[![Denetleyici ekleme](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="26bb7-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="26bb7-112">Bu bizim \Controllers klasör Projemizin içinde altında yeni bir "MoviesController.cs" dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="26bb7-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="26bb7-113">Şimdi yeni doldurulan bizim veritabanından filmler listesini almak için MovieController güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="26bb7-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="26bb7-114">Yalnızca 1984 Yaz sonra yayımlanan filmler alıyoruz böylece biz LINQ sorgusu yapıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="26bb7-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="26bb7-115">Biz geri filmler listesini oluşturmak, böylece yöntemi içinde sağ tıklatın ve görünümü oluşturmak için Ekle seçmek için bir görünüm şablonu gerekir.</span><span class="sxs-lookup"><span data-stu-id="26bb7-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="26bb7-116">Görünüm Ekle iletişim kutusu içinde listesini geçiriyoruz biz belirtmek&lt;Movies.Models.Movie&gt; bizim şablonu görüntüleme için.</span><span class="sxs-lookup"><span data-stu-id="26bb7-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="26bb7-117">Görünüm Ekle iletişim kutusu kullanılan ve bir "Boş" şablonu oluşturmak seçtiğiniz önceki zamanları, biz belirtmek bu kez otomatik olarak "şablonu görüntüleme bize için bazı varsayılan içerikle iskele için" Visual Studio istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="26bb7-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="26bb7-118">Biz "görünümü içerik açılır menüsünde. içinde"List"öğesini seçerek gerçekleştirirsiniz</span><span class="sxs-lookup"><span data-stu-id="26bb7-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="26bb7-119">Unutmayın, oluşturulmuş bir olduğunda, Görünüm Ekle iletişim kutusunda göstermeyi uygulamanızın derleme gerekir yeni bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="26bb7-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Görünüm Ekle](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="26bb7-121">Ekle'yi tıklatın ve sistem otomatik olarak kod için bir görünüm filmler listemiz görüntüleyen bize oluşturur.</span><span class="sxs-lookup"><span data-stu-id="26bb7-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="26bb7-122">Bu değiştirmek için iyi bir zamandır &lt;h2&gt; "My film listesi" gibi bir Hello World görünümü ile daha önce yaptığımız gibi başlığı.</span><span class="sxs-lookup"><span data-stu-id="26bb7-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="26bb7-123">[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="26bb7-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="26bb7-124">Uygulamanızı çalıştırın ve adres çubuğuna /Movies ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="26bb7-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="26bb7-125">Şimdi artık denetleyicisi içinde temel bir sorgu kullanarak veritabanı veri alınır ve filmler hakkında bildiği bir görünüme veri döndürdü.</span><span class="sxs-lookup"><span data-stu-id="26bb7-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="26bb7-126">Bu görünüm sonra filmler listesi boyunca döner ve veri tablosu bize oluşturur.</span><span class="sxs-lookup"><span data-stu-id="26bb7-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="26bb7-127">[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="26bb7-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="26bb7-128">İskele şablon bize için oluşturulan varsayılan bağlantıları gerekmez böylece Biz bu uygulamayla - düzenleme, Ayrıntılar ve silme işlevleri uygulama olmaz.</span><span class="sxs-lookup"><span data-stu-id="26bb7-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="26bb7-129">/Movies/Index.aspx dosyasını açın ve bunları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="26bb7-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="26bb7-130">Biz bu değişiklikleri yaptıktan sonra güncelleştirilmiş şablonu görüntüleme aşağıdaki gibi görünmelidir için kaynak kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="26bb7-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="26bb7-131">Biz bu örnek için silersiniz böylece biz gerekmez, bağlantılar oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="26bb7-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="26bb7-132">Sonraki olduğu gibi biz yine de bizim Yeni Oluştur bağlantı devam edilecek!</span><span class="sxs-lookup"><span data-stu-id="26bb7-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="26bb7-133">İşte uygulamamıza nasıl kaldırılır bu sütunla göründüğünü.</span><span class="sxs-lookup"><span data-stu-id="26bb7-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="26bb7-134">[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="26bb7-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="26bb7-135">Şimdi sahip olduğumuz film verilerimizi, basit bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="26bb7-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="26bb7-136">Biz "Yeni Oluştur" bağlantısını tıklatın, bu sayfaya değil olarak ancak biz bir hata iletisi alırsınız!</span><span class="sxs-lookup"><span data-stu-id="26bb7-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="26bb7-137">Şimdi oluşturmak eylem yöntemi uygulayabilirsiniz ve yeni filmler bizim veritabanında girmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="26bb7-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26bb7-138">[Önceki](getting-started-with-mvc-part4.md)
> [sonraki](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="26bb7-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
