---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754703"
---
<a name="adding-a-model"></a><span data-ttu-id="dcc5e-104">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="dcc5e-104">Adding a Model</span></span>
====================
<span data-ttu-id="dcc5e-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="dcc5e-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="dcc5e-106">Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="dcc5e-107">Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="dcc5e-108">Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="dcc5e-109">Bu sınıflar olacaktır &quot;modeli&quot; ASP.NET MVC uygulamasını bir parçası.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="dcc5e-110">Bir .NET Framework Veri erişim teknolojisi olarak bilinen kullanacağınız [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) tanımlayın ve bu model sınıfları ile çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="dcc5e-111">Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="dcc5e-112">Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="dcc5e-113">(Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnesi.&quot;) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="dcc5e-114">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="dcc5e-114">Adding Model Classes</span></span>

<span data-ttu-id="dcc5e-115">İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="dcc5e-116">Girin *sınıfı* adı &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="dcc5e-117">Aşağıdaki beş özelliği Ekle `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="dcc5e-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="dcc5e-118">Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="dcc5e-119">Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="dcc5e-120">Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="dcc5e-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="dcc5e-121">`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="dcc5e-122">`MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="dcc5e-123">Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="dcc5e-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="dcc5e-124">Tam *Movie.cs* dosya aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="dcc5e-125">(Çeşitli olmayan deyimleri kullanarak kaldırıldı.)</span><span class="sxs-lookup"><span data-stu-id="dcc5e-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="dcc5e-126">Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="dcc5e-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="dcc5e-127">`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="dcc5e-128">Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="dcc5e-129">Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="dcc5e-130">Uygulama kökü açın *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="dcc5e-131">(Değil *Web.config* dosyası *görünümleri* klasör.) Açık *Web.config* kırmızı renkle dosya.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="dcc5e-132">Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="dcc5e-133">Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="dcc5e-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="dcc5e-134">Bu küçük kod ve XML temsil eder ve film verileri bir veritabanında saklamak için yazmanız gereken her şeyi miktarıdır.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="dcc5e-135">Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dcc5e-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dcc5e-136">[Önceki](adding-a-view.md)
> [İleri](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="dcc5e-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
