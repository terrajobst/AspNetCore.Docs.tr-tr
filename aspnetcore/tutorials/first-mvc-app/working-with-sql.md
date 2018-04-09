---
title: ASP.NET Core içinde SQL Server yerel veritabanı ile çalışma
author: rick-anderson
description: SQL Server yerel veritabanı basit bir ASP.NET Core MVC uygulamasında kullanma hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 3f69657cb21e163bdf00fb1faea98889046e9b45
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="81dfb-103">ASP.NET Core içinde SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="81dfb-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="81dfb-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="81dfb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="81dfb-105">`MvcMovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="81dfb-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="81dfb-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="81dfb-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="81dfb-107">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="81dfb-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="81dfb-108">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="81dfb-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="81dfb-109">Bir test veya üretim sunucusuna uygulama dağıtırken, bir ortam değişkeni veya başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server bağlantı dizesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="81dfb-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="81dfb-110">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="81dfb-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="81dfb-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="81dfb-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="81dfb-112">Yerel veritabanı, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="81dfb-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="81dfb-113">Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="81dfb-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="81dfb-114">Varsayılan olarak, yerel veritabanı bir veritabanı oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizini.</span><span class="sxs-lookup"><span data-stu-id="81dfb-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="81dfb-115">Gelen **Görünüm** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="81dfb-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* <span data-ttu-id="81dfb-117">Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**</span><span class="sxs-lookup"><span data-stu-id="81dfb-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

<span data-ttu-id="81dfb-120">Anahtar simgesine yanına Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="81dfb-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="81dfb-121">Varsayılan olarak, EF adlı bir özellik yapacak `ID` birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="81dfb-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="81dfb-122">Sağ tıklayın `Movie` tablo **> verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="81dfb-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verisi gösteren açık film tablosu](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="81dfb-125">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="81dfb-125">Seed the database</span></span>

<span data-ttu-id="81dfb-126">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="81dfb-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="81dfb-127">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="81dfb-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="81dfb-128">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür ve hiçbir filmler eklenir.</span><span class="sxs-lookup"><span data-stu-id="81dfb-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="81dfb-129">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="81dfb-129">Add the seed initializer</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81dfb-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81dfb-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="81dfb-131">Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="81dfb-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81dfb-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81dfb-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="81dfb-133">Çekirdek Başlatıcı sonuna ekleyin `Configure` yönteminde *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="81dfb-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

* * *
<span data-ttu-id="81dfb-134">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="81dfb-134">Test the app</span></span>

* <span data-ttu-id="81dfb-135">DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="81dfb-135">Delete all the records in the DB.</span></span> <span data-ttu-id="81dfb-136">Tarayıcıda veya SSOX delete bağlantılar yapın.</span><span class="sxs-lookup"><span data-stu-id="81dfb-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="81dfb-137">Uygulamayı başlatmak için zorlama (yöntemleri Çağır `Startup` sınıfı) seed yöntemi çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="81dfb-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="81dfb-138">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="81dfb-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="81dfb-139">Bu yaklaşımın şunlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="81dfb-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="81dfb-140">IIS Express sistem tepsisi bildirim alanı simgesini sağ tıklatın ve dokunun **çıkış** veya **sitesini Durdur**</span><span class="sxs-lookup"><span data-stu-id="81dfb-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlam menüsü](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="81dfb-143">VS olmayan hata ayıklama modunda çalışıyormuş hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="81dfb-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="81dfb-144">Hata ayıklama modunda VS çalışıyormuş hata ayıklayıcıyı durdurduktan ve F5 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="81dfb-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="81dfb-145">Uygulama hazırlığı yapmış veriler gösterir.</span><span class="sxs-lookup"><span data-stu-id="81dfb-145">The app shows the seeded data.</span></span>

![MVC film uygulaması Microsoft Edge'de film verileri gösteren açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="81dfb-147">[Önceki](adding-model.md)
> [sonraki](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="81dfb-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
