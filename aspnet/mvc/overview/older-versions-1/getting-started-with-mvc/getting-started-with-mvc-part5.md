---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Bir denetleyiciden modelinizin verilerine erişme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752978"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="c1131-104">Bir denetleyiciden modelinizin verilerine erişme</span><span class="sxs-lookup"><span data-stu-id="c1131-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="c1131-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c1131-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c1131-106">ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="c1131-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c1131-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c1131-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c1131-108">Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.</span><span class="sxs-lookup"><span data-stu-id="c1131-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c1131-109">Bu bölümde yeni bir MoviesController sınıf oluşturun ve bizim film verileri alır ve bir görünüm şablonu kullanarak tarayıcıya görüntüleyen bazı kodlar yazma kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="c1131-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="c1131-110">Denetleyicileri klasörü sağ tıklatın ve yeni MoviesController olun.</span><span class="sxs-lookup"><span data-stu-id="c1131-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="c1131-111">[![Denetleyici ekleme](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c1131-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="c1131-112">Bu, bizim Projemizin \Controllers klasörün altında yeni bir "MoviesController.cs" dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1131-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="c1131-113">Sunduğumuz yeni doldurulmuş veritabanından filmler listesini almak için MovieController güncelleştirelim.</span><span class="sxs-lookup"><span data-stu-id="c1131-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="c1131-114">Biz bir LINQ Sorgu çalışıp çalışmadığını denetleyin ve böylece yalnızca 1984 ' Yaz sonra yayınlanan film alıyoruz.</span><span class="sxs-lookup"><span data-stu-id="c1131-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="c1131-115">Film geri bu listeyi işlemek, bu nedenle yönteminde sağ tıklatın ve görünüm oluşturmak için Ekle seçeneğini şablonu görüntüle gerekir.</span><span class="sxs-lookup"><span data-stu-id="c1131-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="c1131-116">Görünüm Ekle iletişim kutusunun içinden listesini geçiriyoruz biz belirtmek&lt;Movies.Models.Movie&gt; bizim görünüm şablonu için.</span><span class="sxs-lookup"><span data-stu-id="c1131-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="c1131-117">Görünüm Ekle iletişim kutusu kullanılan ve bir "Boş" şablonu oluşturmak seçtiğiniz önceki saatleri, biz belirtmek, bu kez otomatik olarak "bir şablonu görüntüleme bizim için bazı varsayılan içerik ile iskele oluşturmayı" Visual Studio istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="c1131-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="c1131-118">"Görünümü içerik açılan menüsü. içinde"Listesi"öğesini seçerek bunu</span><span class="sxs-lookup"><span data-stu-id="c1131-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="c1131-119">Unutmayın, oluşturulmuş bir olduğunda, Görünüm Ekle iletişim kutusunda görünmesini için uygulamanızı derlemek için ihtiyacınız olan yeni bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="c1131-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Görünüm Ekle](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="c1131-121">Ekle'ye ve sistem otomatik olarak kod için bir görünüm filmler listemizi görüntüleyen bizim için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1131-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="c1131-122">Bunu değiştirmek için iyi bir zamandır &lt;h2&gt; Hello World görünümüyle daha önce yaptığımız gibi "My film listesi" gibi bir şey başlığı.</span><span class="sxs-lookup"><span data-stu-id="c1131-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="c1131-123">[![Filmler - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c1131-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="c1131-124">Uygulamanızı çalıştırın ve adres çubuğuna /Movies ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="c1131-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="c1131-125">Şimdi biz denetleyici içinde temel bir sorgu kullanarak veritabanından veri alınır ve döndürülen veriler hakkında filmler bildiği bir görünüme.</span><span class="sxs-lookup"><span data-stu-id="c1131-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="c1131-126">Bu görünüm sonra filmler listesi üzerinden sanal makineleri çalıştırır ve bizim için verilerinizin bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1131-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="c1131-127">[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="c1131-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="c1131-128">İskele şablon bizim için oluşturulan varsayılan bağlantılara ihtiyaç duymayacağımız için Biz bu uygulamayla - düzenleme, ayrıntı ve silme işlevleri uygulama olmaz.</span><span class="sxs-lookup"><span data-stu-id="c1131-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="c1131-129">/Movies/Index.aspx dosyasını açın ve bunları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c1131-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="c1131-130">İşte Biz bu değişiklikleri yaptıktan sonra kaynak kodunu bizim güncelleştirilmiş görünüm şablonu aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="c1131-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="c1131-131">Bunları bu örneğin sileceğiz şekilde ihtiyacımız yoktur, bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1131-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="c1131-132">Sonraki şudur bizim oluşturma yeni bir bağlantı, saklayacağız!</span><span class="sxs-lookup"><span data-stu-id="c1131-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="c1131-133">Uygulamamızı kaldırılmış olan sütunu nasıl göründüğünü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c1131-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="c1131-134">[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c1131-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="c1131-135">Artık basit bir film verilerimizi listesi var.</span><span class="sxs-lookup"><span data-stu-id="c1131-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="c1131-136">Biz "Yeni Oluştur" bağlantısını tıklatın, bu bağlanmamıştır ancak biz hata yakalayacaksınız!</span><span class="sxs-lookup"><span data-stu-id="c1131-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="c1131-137">Şimdi bir oluşturma eylemi yöntemini uygulayın ve yeni filmler veritabanımızda yer girmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="c1131-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1131-138">[Önceki](getting-started-with-mvc-part4.md)
> [İleri](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="c1131-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
