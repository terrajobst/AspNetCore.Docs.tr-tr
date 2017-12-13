---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a><span data-ttu-id="9b4fb-104">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="9b4fb-104">Adding a Model</span></span>
====================
<span data-ttu-id="9b4fb-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9b4fb-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="9b4fb-106">Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="9b4fb-107">Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="9b4fb-108">Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="9b4fb-109">Bu sınıfların olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması parçası.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="9b4fb-110">Olarak bilinen bir .NET Framework veri erişimi teknoloji kullanacağınız [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) tanımlamak ve bu modeli sınıfları ile çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="9b4fb-111">Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="9b4fb-112">Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="9b4fb-113">(Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnelerini.&quot;) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="9b4fb-114">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="9b4fb-114">Adding Model Classes</span></span>

<span data-ttu-id="9b4fb-115">İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="9b4fb-116">Girin *sınıfı* adı &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="9b4fb-117">Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b4fb-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="9b4fb-118">Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="9b4fb-119">Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="9b4fb-120">Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b4fb-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="9b4fb-121">`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="9b4fb-122">`MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="9b4fb-123">Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="9b4fb-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="9b4fb-124">Tam *Movie.cs* dosya aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="9b4fb-125">(Birkaç olmayan deyimleri kullanarak kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="9b4fb-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="9b4fb-126">Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="9b4fb-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="9b4fb-127">`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9b4fb-128">Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="9b4fb-129">Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="9b4fb-130">Uygulama kök açmak *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="9b4fb-131">(Değil *Web.config* dosyasını *görünümleri* klasörü.) Açık *Web.config* kırmızı renkle dosya.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="9b4fb-132">Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="9b4fb-133">Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="9b4fb-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="9b4fb-134">Bu küçük kod ve XML temsil eder ve bir veritabanında film verileri depolamak için yazma için gereken her şeyi miktarıdır.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="9b4fb-135">Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9b4fb-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9b4fb-136">[Önceki](adding-a-view.md)
[sonraki](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="9b4fb-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
