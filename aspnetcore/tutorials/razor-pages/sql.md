---
title: Bir veritabanı ve ASP.NET Core ile çalışma
author: rick-anderson
description: Veritabanı ile ASP.NET Core ile çalışmayı açıklar.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 6cef55382d8c77e95280ea4eea2dbc2af1c81987
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265559"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="1bd39-103">Bir veritabanı ve ASP.NET Core ile çalışma</span><span class="sxs-lookup"><span data-stu-id="1bd39-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="1bd39-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="1bd39-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="1bd39-105">`RazorPagesMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="1bd39-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1bd39-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1bd39-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1bd39-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1bd39-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1bd39-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1bd39-109">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="1bd39-110">Kullanılan yöntemler hakkında daha fazla bilgi için `ConfigureServices`, bkz:</span><span class="sxs-lookup"><span data-stu-id="1bd39-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="1bd39-111">[ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr) için `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="1bd39-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="1bd39-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="1bd39-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="1bd39-113">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="1bd39-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="1bd39-114">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1bd39-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1bd39-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1bd39-116">Veritabanı adı değeri (`Database={Database name}`) oluşturulan kodunuz için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1bd39-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="1bd39-117">Ad değeri isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1bd39-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1bd39-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1bd39-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1bd39-119">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="1bd39-120">Bir test veya üretim sunucusuna uygulama dağıtıldığında, bir ortam değişkeni gerçek veritabanı sunucusuna bağlantı dizesini ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bd39-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="1bd39-121">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1bd39-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1bd39-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="1bd39-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="1bd39-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="1bd39-124">LocalDB, hedeflenen SQL Server Express Veritabanı Altyapısı'nın program geliştirme için basit bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="1bd39-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="1bd39-125">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="1bd39-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="1bd39-126">Varsayılan olarak LocalDB veritabanına oluşturur `*.mdf` dosyalar `C:/Users/<user/>` dizin.</span><span class="sxs-lookup"><span data-stu-id="1bd39-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="1bd39-127">Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="1bd39-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="1bd39-129">Sağ tıklayın `Movie` tablosunu seçip **Görünüm Tasarımcısı**:</span><span class="sxs-lookup"><span data-stu-id="1bd39-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

<span data-ttu-id="1bd39-132">Anahtar simgesinin yanındaki Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="1bd39-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="1bd39-133">Varsayılan olarak EF adlı bir özellik oluşturur. `ID` birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="1bd39-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="1bd39-134">Sağ tıklayın `Movie` tablosunu seçip **görünüm verilerini**:</span><span class="sxs-lookup"><span data-stu-id="1bd39-134">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verilerini gösteren açık film tablo](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1bd39-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1bd39-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1bd39-137">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="1bd39-138">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="1bd39-138">Seed the database</span></span>

<span data-ttu-id="1bd39-139">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="1bd39-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="1bd39-140">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.</span><span class="sxs-lookup"><span data-stu-id="1bd39-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="1bd39-141">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="1bd39-141">Add the seed initializer</span></span>

<span data-ttu-id="1bd39-142">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="1bd39-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="1bd39-143">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="1bd39-144">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="1bd39-145">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="1bd39-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="1bd39-146">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1bd39-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="1bd39-147">Bir üretim uygulaması değil çağırırsınız `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="1bd39-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="1bd39-148">Aşağıdaki özel durumu önlemek için önceki koda eklenir, `Update-Database` değil çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1bd39-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="1bd39-149">SqlException: "21 RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1bd39-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="1bd39-150">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="1bd39-150">The login failed.</span></span>
<span data-ttu-id="1bd39-151">Oturum açma 'kullanıcı adı' kullanıcı için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="1bd39-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="1bd39-152">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1bd39-152">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1bd39-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1bd39-154">Veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="1bd39-154">Delete all the records in the DB.</span></span> <span data-ttu-id="1bd39-155">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="1bd39-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="1bd39-156">Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="1bd39-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="1bd39-157">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="1bd39-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="1bd39-158">Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1bd39-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="1bd39-159">IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **Durdur Site**:</span><span class="sxs-lookup"><span data-stu-id="1bd39-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * <span data-ttu-id="1bd39-162">VS hata ayıklama olmayan modunda çalışmakta olan hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="1bd39-163">VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1bd39-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1bd39-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1bd39-165">Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="1bd39-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="1bd39-166">Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="1bd39-167">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="1bd39-167">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1bd39-168">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bd39-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1bd39-169">Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="1bd39-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="1bd39-170">Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="1bd39-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="1bd39-171">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="1bd39-171">The app shows the seeded data.</span></span>

---

<span data-ttu-id="1bd39-172">Uygulama, çekirdeği oluşturulmuş veri gösterilir:</span><span class="sxs-lookup"><span data-stu-id="1bd39-172">The app shows the seeded data:</span></span>

![Film verileri gösteren Chrome'da açık film uygulaması](sql/_static/m55.png)

<span data-ttu-id="1bd39-174">Sonraki öğreticiye verilerin sunuyu temizler.</span><span class="sxs-lookup"><span data-stu-id="1bd39-174">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bd39-175">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1bd39-175">Additional resources</span></span>

* [<span data-ttu-id="1bd39-176">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="1bd39-176">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="1bd39-177">[Önceki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)
> [sonraki: Sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="1bd39-177">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
