---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Veritabanını oluşturmak için Code First geçişleri kullanın | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="660e5-102">Veritabanını oluşturmak için Code First geçişleri kullanın</span><span class="sxs-lookup"><span data-stu-id="660e5-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="660e5-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="660e5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="660e5-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="660e5-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="660e5-105">Bu bölümde, kullanacağınız [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) test verileri ile veritabanı oluşturmak için EF içinde.</span><span class="sxs-lookup"><span data-stu-id="660e5-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="660e5-106">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="660e5-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="660e5-107">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="660e5-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="660e5-108">Bu komut, projenize geçişler adlı bir klasör artı geçişler klasöründe Configuration.cs adlı bir kod dosyası ekler.</span><span class="sxs-lookup"><span data-stu-id="660e5-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="660e5-109">Configuration.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="660e5-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="660e5-110">Aşağıdakileri ekleyin **kullanarak** deyimi.</span><span class="sxs-lookup"><span data-stu-id="660e5-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="660e5-111">Ardından aşağıdaki kodu ekleyin **Configuration.Seed** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="660e5-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="660e5-112">Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın:</span><span class="sxs-lookup"><span data-stu-id="660e5-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="660e5-113">İlk komut veritabanını oluşturur kod oluşturur ve ikinci komut, kodu yürütür.</span><span class="sxs-lookup"><span data-stu-id="660e5-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="660e5-114">Veritabanı yerel olarak kullanılarak oluşturulan [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="660e5-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="660e5-115">(İsteğe bağlı) API'sini keşfedin</span><span class="sxs-lookup"><span data-stu-id="660e5-115">Explore the API (Optional)</span></span>

<span data-ttu-id="660e5-116">Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="660e5-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="660e5-117">Visual Studio IIS Express başlar ve web uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="660e5-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="660e5-118">Visual Studio bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar.</span><span class="sxs-lookup"><span data-stu-id="660e5-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="660e5-119">Visual Studio web projesini çalıştığında, bir bağlantı noktası numarası atar.</span><span class="sxs-lookup"><span data-stu-id="660e5-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="660e5-120">Aşağıdaki resimde 50524 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="660e5-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="660e5-121">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="660e5-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="660e5-122">Giriş sayfası, ASP.NET MVC kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="660e5-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="660e5-123">Sayfanın en üstünde "API" belirten bir bağlantı yoktur.</span><span class="sxs-lookup"><span data-stu-id="660e5-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="660e5-124">Bu bağlantıyı web API'si için otomatik olarak oluşturulan yardım sayfasına getirir.</span><span class="sxs-lookup"><span data-stu-id="660e5-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="660e5-125">(Bu Yardım sayfası nasıl oluşturulacağını ve kendi belgeleri sayfasına nasıl ekleyebileceğiniz öğrenmek için bkz: [ASP.NET Web API Yardım sayfaları oluşturma](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) İstek ve yanıt biçimi dahil olmak üzere API, ilgili ayrıntıları görmek için sayfa bağlantıları hakkında Yardım tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="660e5-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="660e5-126">API CRUD işlemleri veritabanında sağlar.</span><span class="sxs-lookup"><span data-stu-id="660e5-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="660e5-127">API özetler.</span><span class="sxs-lookup"><span data-stu-id="660e5-127">The following summarizes the API.</span></span>

| <span data-ttu-id="660e5-128">Yazarlar</span><span class="sxs-lookup"><span data-stu-id="660e5-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="660e5-129">API/yazarlar Al</span><span class="sxs-lookup"><span data-stu-id="660e5-129">GET api/authors</span></span> | <span data-ttu-id="660e5-130">Tüm yazarları alın.</span><span class="sxs-lookup"><span data-stu-id="660e5-130">Get all authors.</span></span> |
| <span data-ttu-id="660e5-131">GET API/yazarlar / {id}</span><span class="sxs-lookup"><span data-stu-id="660e5-131">GET api/authors/{id}</span></span> | <span data-ttu-id="660e5-132">Bir yazar tarafından kimliği alma</span><span class="sxs-lookup"><span data-stu-id="660e5-132">Get an author by ID.</span></span> |
| <span data-ttu-id="660e5-133">POST/api/yazarlar</span><span class="sxs-lookup"><span data-stu-id="660e5-133">POST /api/authors</span></span> | <span data-ttu-id="660e5-134">Yeni bir yazar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="660e5-134">Create a new author.</span></span> |
| <span data-ttu-id="660e5-135">PUT /api/yazarlar / {id}</span><span class="sxs-lookup"><span data-stu-id="660e5-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="660e5-136">Varolan bir yazar güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="660e5-136">Update an existing author.</span></span> |
| <span data-ttu-id="660e5-137">/Api/yazarlar / {id} Sil</span><span class="sxs-lookup"><span data-stu-id="660e5-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="660e5-138">Bir yazar silin.</span><span class="sxs-lookup"><span data-stu-id="660e5-138">Delete an author.</span></span> |

| <span data-ttu-id="660e5-139">Kitaplar</span><span class="sxs-lookup"><span data-stu-id="660e5-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="660e5-140">/Api/Books Al</span><span class="sxs-lookup"><span data-stu-id="660e5-140">GET /api/books</span></span> | <span data-ttu-id="660e5-141">Tüm books alın.</span><span class="sxs-lookup"><span data-stu-id="660e5-141">Get all books.</span></span> |
| <span data-ttu-id="660e5-142">/Api/books / {id} Al</span><span class="sxs-lookup"><span data-stu-id="660e5-142">GET /api/books/{id}</span></span> | <span data-ttu-id="660e5-143">Kimliğe göre defteri alma</span><span class="sxs-lookup"><span data-stu-id="660e5-143">Get a book by ID.</span></span> |
| <span data-ttu-id="660e5-144">/ Api/books sonrası</span><span class="sxs-lookup"><span data-stu-id="660e5-144">POST /api/books</span></span> | <span data-ttu-id="660e5-145">Yeni bir rehberi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="660e5-145">Create a new book.</span></span> |
| <span data-ttu-id="660e5-146">PUT /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="660e5-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="660e5-147">Varolan bir kitabı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="660e5-147">Update an existing book.</span></span> |
| <span data-ttu-id="660e5-148">/Api/books / {id} Sil</span><span class="sxs-lookup"><span data-stu-id="660e5-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="660e5-149">Kitap silin.</span><span class="sxs-lookup"><span data-stu-id="660e5-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="660e5-150">Görünüm veritabanı (isteğe bağlı)</span><span class="sxs-lookup"><span data-stu-id="660e5-150">View the Database (Optional)</span></span>

<span data-ttu-id="660e5-151">Update-Database komutunu çalıştırdığınızda EF veritabanı oluşturulur ve adlı `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="660e5-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="660e5-152">Uygulamayı yerel olarak çalıştırdığınızda EF kullanan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="660e5-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="660e5-153">Visual Studio'da veritabanı görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="660e5-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="660e5-154">Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="660e5-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="660e5-155">İçinde **sunucuya Bağlan** iletişim, **sunucu adı** düzenleme kutusu, "(localdb) \v11.0" yazın.</span><span class="sxs-lookup"><span data-stu-id="660e5-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="660e5-156">Bırakın **kimlik doğrulaması** seçeneği "Windows kimlik doğrulaması" olarak.</span><span class="sxs-lookup"><span data-stu-id="660e5-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="660e5-157">**Bağlan**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="660e5-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="660e5-158">Visual Studio yerel veritabanına bağlanır ve var olan veritabanlarını SQL Server Nesne Gezgini penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="660e5-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="660e5-159">EF oluşturulan tabloları görmek için düğümleri genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="660e5-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="660e5-160">Verileri görüntülemek için bir tabloyu sağ tıklatın ve seçin **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="660e5-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="660e5-161">Aşağıdaki ekran görüntüsünde Books tablosu için sonuçları gösterir.</span><span class="sxs-lookup"><span data-stu-id="660e5-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="660e5-162">EF veritabanı çekirdek verileri ile doldurulur ve yazarlar tablosuna yabancı anahtar tablosu içerir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="660e5-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="660e5-163">[Önceki](part-2.md)
[sonraki](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="660e5-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
