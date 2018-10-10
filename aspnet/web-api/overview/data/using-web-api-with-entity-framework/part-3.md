---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Veritabanının çekirdeğini oluşturma için Code First geçişleri kullanın | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910505"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="8b5ff-102">Veritabanının çekirdeğini oluşturma için Code First Migrations'ı kullanın</span><span class="sxs-lookup"><span data-stu-id="8b5ff-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="8b5ff-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b5ff-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8b5ff-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="8b5ff-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8b5ff-105">Bu bölümde, kullanacağınız [Code First Migrations](https://msdn.microsoft.com/data/jj591621) test verileri ile veritabanının çekirdeğini oluşturma için EF içinde.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="8b5ff-106">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8b5ff-107">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="8b5ff-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="8b5ff-108">Bu komut, projenize geçişleri adlı bir klasör yanı sıra, geçişler klasöründe Configuration.cs adlı bir kod dosyası ekler.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="8b5ff-109">Configuration.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="8b5ff-110">Aşağıdaki **kullanarak** deyimi.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="8b5ff-111">Ardından aşağıdaki kodu ekleyin **Configuration.Seed** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8b5ff-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="8b5ff-112">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları yazın:</span><span class="sxs-lookup"><span data-stu-id="8b5ff-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="8b5ff-113">İlk komut veritabanı oluşturan kodu oluşturur ve ikinci komut, kodu yürütür.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="8b5ff-114">Veritabanı kullanılarak yerel olarak oluşturulan [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b5ff-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="8b5ff-115">(İsteğe bağlı) API'sini keşfedin</span><span class="sxs-lookup"><span data-stu-id="8b5ff-115">Explore the API (Optional)</span></span>

<span data-ttu-id="8b5ff-116">Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="8b5ff-117">Visual Studio, IIS Express başlar ve web uygulamanızı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="8b5ff-118">Visual Studio bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="8b5ff-119">Visual Studio web projesini çalıştığında, bir bağlantı noktası numarası atar.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="8b5ff-120">Aşağıdaki görüntüde, 50524 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="8b5ff-121">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="8b5ff-122">Giriş sayfası, ASP.NET MVC kullanarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="8b5ff-123">Sayfanın üst kısmında "API" ifadesini içeren bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="8b5ff-124">Bu bağlantıyı web API'si için bir otomatik olarak oluşturulmuş yardım sayfasına getirir.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="8b5ff-125">(Bu Yardım sayfasını nasıl oluşturulacağını ve kendi belge sayfasına nasıl ekleyebileceğinizi öğrenmek için bkz: [ASP.NET Web API Yardım sayfaları oluşturma](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) İstek ve yanıt biçimi de dahil olmak üzere bu API hakkında daha fazla ayrıntı görmek için sayfayı bağlantıları hakkında Yardım tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="8b5ff-126">API veritabanında CRUD işlemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="8b5ff-127">API özetler.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-127">The following summarizes the API.</span></span>

| <span data-ttu-id="8b5ff-128">Yazarlar</span><span class="sxs-lookup"><span data-stu-id="8b5ff-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="8b5ff-129">API/yazarlar Al</span><span class="sxs-lookup"><span data-stu-id="8b5ff-129">GET api/authors</span></span> | <span data-ttu-id="8b5ff-130">Tüm yazarları alın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-130">Get all authors.</span></span> |
| <span data-ttu-id="8b5ff-131">GET API/yazarlar / {id}</span><span class="sxs-lookup"><span data-stu-id="8b5ff-131">GET api/authors/{id}</span></span> | <span data-ttu-id="8b5ff-132">Bir yazar kimliğe göre Al</span><span class="sxs-lookup"><span data-stu-id="8b5ff-132">Get an author by ID.</span></span> |
| <span data-ttu-id="8b5ff-133">POST/api/yazarları</span><span class="sxs-lookup"><span data-stu-id="8b5ff-133">POST /api/authors</span></span> | <span data-ttu-id="8b5ff-134">Yeni bir yazar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-134">Create a new author.</span></span> |
| <span data-ttu-id="8b5ff-135">PUT/API'si/yazarlar / {id}</span><span class="sxs-lookup"><span data-stu-id="8b5ff-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="8b5ff-136">Mevcut bir yazar güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-136">Update an existing author.</span></span> |
| <span data-ttu-id="8b5ff-137">/ API'si/yazarlar / {id} Sil</span><span class="sxs-lookup"><span data-stu-id="8b5ff-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="8b5ff-138">Bir yazar silin.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-138">Delete an author.</span></span> |

| <span data-ttu-id="8b5ff-139">Kitaplar</span><span class="sxs-lookup"><span data-stu-id="8b5ff-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="8b5ff-140">/Api/Books Al</span><span class="sxs-lookup"><span data-stu-id="8b5ff-140">GET /api/books</span></span> | <span data-ttu-id="8b5ff-141">Tüm Kitaplar alın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-141">Get all books.</span></span> |
| <span data-ttu-id="8b5ff-142">Alma/API'si/books / {id}</span><span class="sxs-lookup"><span data-stu-id="8b5ff-142">GET /api/books/{id}</span></span> | <span data-ttu-id="8b5ff-143">Bir kitap kimliğe göre Al</span><span class="sxs-lookup"><span data-stu-id="8b5ff-143">Get a book by ID.</span></span> |
| <span data-ttu-id="8b5ff-144">/ Api/books gönderin</span><span class="sxs-lookup"><span data-stu-id="8b5ff-144">POST /api/books</span></span> | <span data-ttu-id="8b5ff-145">Yeni bir kitap oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-145">Create a new book.</span></span> |
| <span data-ttu-id="8b5ff-146">PUT/API'si/books / {id}</span><span class="sxs-lookup"><span data-stu-id="8b5ff-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="8b5ff-147">Mevcut bir kitap güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-147">Update an existing book.</span></span> |
| <span data-ttu-id="8b5ff-148">/ API'si/books / {id} Sil</span><span class="sxs-lookup"><span data-stu-id="8b5ff-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="8b5ff-149">Bir kitap silin.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="8b5ff-150">(İsteğe bağlı) veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="8b5ff-150">View the Database (Optional)</span></span>

<span data-ttu-id="8b5ff-151">EF veritabanını Güncelleştir komutu çalıştırdığınızda, veritabanı oluşturulur ve adlı `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="8b5ff-152">Uygulamayı yerel olarak çalıştırdığınızda EF kullanan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b5ff-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="8b5ff-153">Visual Studio'da veritabanı görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="8b5ff-154">Gelen **görünümü** menüsünde **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="8b5ff-155">İçinde **sunucuya Bağlan** iletişim, **sunucu adı** düzenleme kutusu, "(localdb) \v11.0" yazın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="8b5ff-156">Bırakın **kimlik doğrulaması** seçeneği olarak "Windows kimlik doğrulaması".</span><span class="sxs-lookup"><span data-stu-id="8b5ff-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="8b5ff-157">**Bağlan**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="8b5ff-158">Visual Studio yerel veritabanına bağlanır ve mevcut veritabanlarınızı SQL Server Nesne Gezgini penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="8b5ff-159">EF oluşturduğunuz tabloları görmek için düğümü genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="8b5ff-160">Verileri görüntülemek için bir tablo sağ tıklayıp **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="8b5ff-161">Aşağıdaki ekran görüntüsünde Books tablosu için sonuçları gösterir.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="8b5ff-162">EF veritabanı çekirdek verilerle doldurulmuş ve yazarlar tabloya yabancı anahtar tablosu içeren olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8b5ff-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="8b5ff-163">[Önceki](part-2.md)
> [İleri](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8b5ff-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
