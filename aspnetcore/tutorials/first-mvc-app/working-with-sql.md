---
title: Bir ASP.NET Core MVC uygulama içinde SQL Server yerel veritabanı ile çalışma
author: rick-anderson
description: SQL Server yerel veritabanı basit bir ASP.NET Core MVC uygulamasında kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 05bd76038e5856d2a3e392e3b00e589dbc26fcc6
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961064"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="54ae5-103">ASP.NET Core içinde SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="54ae5-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="54ae5-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54ae5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="54ae5-105">`MvcMovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="54ae5-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="54ae5-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="54ae5-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

<span data-ttu-id="54ae5-107">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="54ae5-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="54ae5-108">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="54ae5-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="54ae5-109">Bir test veya üretim sunucusuna uygulama dağıtırken, bir ortam değişkeni veya başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server bağlantı dizesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="54ae5-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="54ae5-110">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="54ae5-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="54ae5-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="54ae5-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="54ae5-112">Yerel veritabanı, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="54ae5-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="54ae5-113">Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="54ae5-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="54ae5-114">Varsayılan olarak, yerel veritabanı bir veritabanı oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizini.</span><span class="sxs-lookup"><span data-stu-id="54ae5-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="54ae5-115">Gelen **Görünüm** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="54ae5-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* <span data-ttu-id="54ae5-117">Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**</span><span class="sxs-lookup"><span data-stu-id="54ae5-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

<span data-ttu-id="54ae5-120">Anahtar simgesine yanına Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="54ae5-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="54ae5-121">Varsayılan olarak, EF adlı bir özellik yapacak `ID` birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="54ae5-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="54ae5-122">Sağ tıklayın `Movie` tablo **> verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="54ae5-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Bağlamsal menü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verisi gösteren açık film tablosu](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="54ae5-125">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="54ae5-125">Seed the database</span></span>

<span data-ttu-id="54ae5-126">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="54ae5-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="54ae5-127">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="54ae5-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="54ae5-128">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür ve hiçbir filmler eklenir.</span><span class="sxs-lookup"><span data-stu-id="54ae5-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="54ae5-129">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="54ae5-129">Add the seed initializer</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ae5-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ae5-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="54ae5-131">Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="54ae5-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ae5-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ae5-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="54ae5-133">Çekirdek Başlatıcı sonuna ekleyin `Configure` yönteminde *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="54ae5-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---
::: moniker-end

<span data-ttu-id="54ae5-134">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="54ae5-134">Test the app</span></span>

* <span data-ttu-id="54ae5-135">DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="54ae5-135">Delete all the records in the DB.</span></span> <span data-ttu-id="54ae5-136">Tarayıcıda veya SSOX delete bağlantılar yapın.</span><span class="sxs-lookup"><span data-stu-id="54ae5-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="54ae5-137">Uygulamayı başlatmak için zorlama (yöntemleri Çağır `Startup` sınıfı) seed yöntemi çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="54ae5-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="54ae5-138">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="54ae5-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="54ae5-139">Bu yaklaşımın şunlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="54ae5-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="54ae5-140">IIS Express sistem tepsisi bildirim alanı simgesini sağ tıklatın ve dokunun **çıkış** veya **sitesini Durdur**</span><span class="sxs-lookup"><span data-stu-id="54ae5-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlam menüsü](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="54ae5-143">VS olmayan hata ayıklama modunda çalışıyormuş hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="54ae5-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="54ae5-144">Hata ayıklama modunda VS çalışıyormuş hata ayıklayıcıyı durdurduktan ve F5 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="54ae5-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="54ae5-145">Uygulama hazırlığı yapmış veriler gösterir.</span><span class="sxs-lookup"><span data-stu-id="54ae5-145">The app shows the seeded data.</span></span>

![MVC film uygulaması Microsoft Edge'de film verileri gösteren açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="54ae5-147">[Önceki](adding-model.md)
> [sonraki](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="54ae5-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
