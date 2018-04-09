---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Bir Model (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-vb"></a><span data-ttu-id="62b8a-103">Bir Model (VB) ekleme</span><span class="sxs-lookup"><span data-stu-id="62b8a-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="62b8a-104">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="62b8a-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="62b8a-105">Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="62b8a-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="62b8a-106">Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="62b8a-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="62b8a-107">Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="62b8a-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="62b8a-108">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="62b8a-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="62b8a-109">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="62b8a-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="62b8a-110">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="62b8a-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="62b8a-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)</span><span class="sxs-lookup"><span data-stu-id="62b8a-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="62b8a-112">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="62b8a-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="62b8a-113">VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="62b8a-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="62b8a-114">[VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="62b8a-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="62b8a-115">C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-model.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="62b8a-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="62b8a-116">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="62b8a-116">Adding a Model</span></span>

<span data-ttu-id="62b8a-117">Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="62b8a-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="62b8a-118">Bu sınıfların "modeli" bölümü ASP.NET MVC uygulamasının olacaktır.</span><span class="sxs-lookup"><span data-stu-id="62b8a-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="62b8a-119">Entity Framework bilinen bir .NET Framework veri erişimi teknoloji tanımlayabilir ve bu modeli sınıfları ile çalışmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="62b8a-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="62b8a-120">Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="62b8a-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="62b8a-121">Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="62b8a-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="62b8a-122">(Bu da POCO sınıflardan "düz eski CLR nesneler." verilir) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.</span><span class="sxs-lookup"><span data-stu-id="62b8a-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="62b8a-123">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="62b8a-123">Adding Model Classes</span></span>

<span data-ttu-id="62b8a-124">İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="62b8a-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="62b8a-125">"Film" sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="62b8a-125">Name the class "Movie".</span></span>

<span data-ttu-id="62b8a-126">Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="62b8a-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="62b8a-127">Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="62b8a-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="62b8a-128">Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="62b8a-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="62b8a-129">Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="62b8a-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="62b8a-130">`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği.</span><span class="sxs-lookup"><span data-stu-id="62b8a-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="62b8a-131">`MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="62b8a-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="62b8a-132">Hakkında daha fazla bilgi için `DbContext` ve `DbSet`, bkz: [Entity Framework için üretkenlik iyileştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="62b8a-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="62b8a-133">Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `imports` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="62b8a-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="62b8a-134">Tam *Movie.vb* dosya aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="62b8a-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="62b8a-135">Bağlantı dizesi oluşturma ve SQL Server Compact ile çalışma</span><span class="sxs-lookup"><span data-stu-id="62b8a-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="62b8a-136">`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="62b8a-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="62b8a-137">Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="62b8a-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="62b8a-138">Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="62b8a-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="62b8a-139">Uygulama kök açmak *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="62b8a-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="62b8a-140">(Değil *Web.config* dosyasını *görünümleri* klasörü.) Aşağıdaki görüntü Göster her ikisi de *Web.config* ; açık dosyalar *Web.config* dosya kırmızıyla yuvarlak içine alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="62b8a-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="62b8a-141">Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="62b8a-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="62b8a-142">Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="62b8a-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="62b8a-143">Bu küçük kod ve XML temsil eder ve bir veritabanında film verileri depolamak için yazma için gereken her şeyi miktarıdır.</span><span class="sxs-lookup"><span data-stu-id="62b8a-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="62b8a-144">Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="62b8a-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62b8a-145">[Önceki](adding-a-view.md)
> [sonraki](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="62b8a-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
