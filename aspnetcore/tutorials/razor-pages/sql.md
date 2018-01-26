---
title: "SQL Server yerel veritabanı ve ASP.NET Core ile çalışma"
author: rick-anderson
description: "SQL Server yerel veritabanı ve ASP.NET Core ile çalışmayı açıklar."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 07f024e2e178828c4488adfd866fc6eec3b251dd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="53981-103">SQL Server yerel veritabanı ve ASP.NET Core ile çalışma</span><span class="sxs-lookup"><span data-stu-id="53981-103">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="53981-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="53981-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="53981-105">`MovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="53981-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="53981-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="53981-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="53981-107">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="53981-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="53981-108">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="53981-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="53981-109">Bir test veya üretim sunucusuna uygulama dağıtırken, bir ortam değişkeni veya başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server bağlantı dizesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="53981-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="53981-110">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="53981-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="53981-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="53981-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="53981-112">Yerel veritabanı, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="53981-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="53981-113">Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="53981-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="53981-114">Varsayılan olarak, yerel veritabanı bir veritabanı oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizini.</span><span class="sxs-lookup"><span data-stu-id="53981-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="53981-115">Gelen **Görünüm** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="53981-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="53981-117">Sağ tıklayın `Movie` tablo ve seçin **Görünüm Tasarımcısı**:</span><span class="sxs-lookup"><span data-stu-id="53981-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Bağlamsal menü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

<span data-ttu-id="53981-120">Anahtar simgesine yanına Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="53981-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="53981-121">Varsayılan olarak, EF adlı bir özellik oluşturur `ID` birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="53981-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="53981-122">Sağ tıklayın `Movie` tablo ve seçin **görünüm verilerini**:</span><span class="sxs-lookup"><span data-stu-id="53981-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verisi gösteren açık film tablosu](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="53981-124">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="53981-124">Seed the database</span></span>

<span data-ttu-id="53981-125">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="53981-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="53981-126">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="53981-126">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="53981-127">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür ve hiçbir filmler eklenir.</span><span class="sxs-lookup"><span data-stu-id="53981-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="53981-128">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="53981-128">Add the seed initializer</span></span>

<span data-ttu-id="53981-129">Çekirdek Başlatıcı sonuna ekleyin `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="53981-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="53981-130">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="53981-130">Test the app</span></span>

* <span data-ttu-id="53981-131">DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="53981-131">Delete all the records in the DB.</span></span> <span data-ttu-id="53981-132">Tarayıcıda veya gelen silme bağlantılarla bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="53981-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="53981-133">Uygulamayı başlatmak için zorlama (yöntemleri Çağır `Startup` sınıfı) seed yöntemi çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="53981-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="53981-134">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="53981-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="53981-135">Bu yaklaşımın şunlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="53981-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="53981-136">IIS Express sistem tepsisi bildirim alanı simgesini sağ tıklatın ve dokunun **çıkış** veya **durdurmak Site**:</span><span class="sxs-lookup"><span data-stu-id="53981-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlam menüsü](sql/_static/stopIIS.png)

   * <span data-ttu-id="53981-139">VS olmayan hata ayıklama modunda çalışıyormuş hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="53981-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="53981-140">Hata ayıklama modunda VS çalışıyormuş hata ayıklayıcıyı durdurduktan ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="53981-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="53981-141">Uygulama hazırlığı yapmış veriler gösterir:</span><span class="sxs-lookup"><span data-stu-id="53981-141">The app shows the seeded data:</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

<span data-ttu-id="53981-143">Sonraki öğretici verilerin sunuyu temizler.</span><span class="sxs-lookup"><span data-stu-id="53981-143">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="53981-144">[Önceki: İskele kurulmuş Razor sayfalarının](xref:tutorials/razor-pages/page)
[sonraki: sayfalarını güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="53981-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
