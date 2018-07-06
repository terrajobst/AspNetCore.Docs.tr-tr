---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Model (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827812"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="447ea-103">Model (VB) ekleme</span><span class="sxs-lookup"><span data-stu-id="447ea-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="447ea-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="447ea-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="447ea-105">Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır.</span><span class="sxs-lookup"><span data-stu-id="447ea-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="447ea-106">Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="447ea-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="447ea-107">Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="447ea-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="447ea-108">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="447ea-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="447ea-109">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="447ea-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="447ea-110">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="447ea-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="447ea-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)</span><span class="sxs-lookup"><span data-stu-id="447ea-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="447ea-112">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="447ea-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="447ea-113">Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="447ea-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="447ea-114">[VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="447ea-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="447ea-115">C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-model.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="447ea-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="447ea-116">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="447ea-116">Adding a Model</span></span>

<span data-ttu-id="447ea-117">Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="447ea-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="447ea-118">Bu sınıf, ASP.NET MVC uygulamasını "modeli" parçası olacak.</span><span class="sxs-lookup"><span data-stu-id="447ea-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="447ea-119">Entity Framework bilinen bir .NET Framework Veri erişim teknolojisi tanımlayın ve bu model sınıfları ile çalışmak için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="447ea-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="447ea-120">Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="447ea-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="447ea-121">Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="447ea-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="447ea-122">(Bunlar da POCO sınıflardan "düz eski CLR nesnesi." verilir) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="447ea-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="447ea-123">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="447ea-123">Adding Model Classes</span></span>

<span data-ttu-id="447ea-124">İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="447ea-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="447ea-125">"Film" sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="447ea-125">Name the class "Movie".</span></span>

<span data-ttu-id="447ea-126">Aşağıdaki beş özelliği Ekle `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="447ea-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="447ea-127">Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="447ea-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="447ea-128">Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="447ea-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="447ea-129">Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="447ea-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="447ea-130">`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri.</span><span class="sxs-lookup"><span data-stu-id="447ea-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="447ea-131">`MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="447ea-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="447ea-132">Hakkında daha fazla bilgi için `DbContext` ve `DbSet`, bkz: [Entity Framework için üretkenlik geliştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="447ea-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="447ea-133">Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `imports` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="447ea-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="447ea-134">Tam *Movie.vb* dosya aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="447ea-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="447ea-135">Bağlantı dizesi oluşturma ve SQL Server Compact ile çalışma</span><span class="sxs-lookup"><span data-stu-id="447ea-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="447ea-136">`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="447ea-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="447ea-137">Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="447ea-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="447ea-138">Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="447ea-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="447ea-139">Uygulama kökü açın *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="447ea-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="447ea-140">(Değil *Web.config* dosyası *görünümleri* klasör.) Aşağıdaki görüntüde göstermek hem de *Web.config* dosyaları; açık *Web.config* dosya kırmızı daire içinde.</span><span class="sxs-lookup"><span data-stu-id="447ea-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="447ea-141">Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="447ea-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="447ea-142">Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="447ea-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="447ea-143">Bu küçük kod ve XML temsil eder ve film verileri bir veritabanında saklamak için yazmanız gereken her şeyi miktarıdır.</span><span class="sxs-lookup"><span data-stu-id="447ea-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="447ea-144">Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="447ea-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="447ea-145">[Önceki](adding-a-view.md)
> [İleri](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="447ea-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
