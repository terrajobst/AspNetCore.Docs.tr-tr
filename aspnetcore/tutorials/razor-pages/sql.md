---
title: Veritabanı ve ASP.NET Core çalışma
author: rick-anderson
description: Bir veritabanı ve ASP.NET Core çalışmayı açıklar.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440384"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="b60d3-103">Veritabanı ve ASP.NET Core çalışma</span><span class="sxs-lookup"><span data-stu-id="b60d3-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="b60d3-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b60d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="b60d3-105">Nesnesi veritabanına bağlanma ve nesneleri veritabanı kayıtlarına eşleme `Movie` görevini işler. `RazorPagesMovieContext`</span><span class="sxs-lookup"><span data-stu-id="b60d3-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b60d3-106">Veritabanı bağlamı, `ConfigureServices` *Startup.cs*içindeki yöntemde [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="b60d3-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b60d3-108">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="b60d3-109">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi okur `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b60d3-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b60d3-110">Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b60d3-112">Veritabanı (`Database={Database name}`) için ad değeri, üretilen kodunuz için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="b60d3-113">Ad değeri rastgele.</span><span class="sxs-lookup"><span data-stu-id="b60d3-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b60d3-114">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="b60d3-115">Uygulama bir test veya üretim sunucusuna dağıtıldığında, bağlantı dizesini gerçek bir veritabanı sunucusuna ayarlamak için bir ortam değişkeni kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="b60d3-116">Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="b60d3-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b60d3-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b60d3-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b60d3-119">LocalDB, program geliştirmeye yönelik SQL Server Express veritabanı altyapısının hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="b60d3-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="b60d3-120">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b60d3-121">Varsayılan olarak, LocalDB veritabanı `*.mdf` `C:/Users/<user/>` dizinde dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b60d3-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="b60d3-122">**Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="b60d3-124">`Movie` Tabloya sağ tıklayıp **Görünüm Tasarımcısı**' nı seçin:</span><span class="sxs-lookup"><span data-stu-id="b60d3-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Film tablosunda açık bağlamsal menüler](sql/_static/design.png)

  ![Tasarımcı 'da açık film tabloları](sql/_static/dv.png)

<span data-ttu-id="b60d3-127">Seçeneğinin yanında `ID`bulunan anahtar simgesine göz önünde edin.</span><span class="sxs-lookup"><span data-stu-id="b60d3-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b60d3-128">Varsayılan olarak, EF birincil anahtar için adlı `ID` bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b60d3-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="b60d3-129">`Movie` Tabloya sağ tıklayın ve **verileri görüntüle**' yi seçin:</span><span class="sxs-lookup"><span data-stu-id="b60d3-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verilerini gösteren film tablosu açma](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b60d3-131">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="b60d3-132">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="b60d3-132">Seed the database</span></span>

<span data-ttu-id="b60d3-133">Modeller klasöründe aşağıdaki kodla adlı `SeedData` yeni bir  sınıf oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b60d3-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b60d3-134">VERITABANıNDA herhangi bir film varsa, tohum başlatıcısı döner ve hiçbir film eklenmez.</span><span class="sxs-lookup"><span data-stu-id="b60d3-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="b60d3-135">Tohum başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="b60d3-135">Add the seed initializer</span></span>

<span data-ttu-id="b60d3-136">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="b60d3-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="b60d3-137">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="b60d3-138">Temel yöntemi çağırın ve bu yönteme geçerek bağlamı geçer.</span><span class="sxs-lookup"><span data-stu-id="b60d3-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="b60d3-139">Çekirdek yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="b60d3-140">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="b60d3-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="b60d3-141">Bir üretim uygulaması çağırmaz `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="b60d3-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="b60d3-142">Çalıştırılmayan aşağıdaki özel durumu `Update-Database` engellemek için önceki koda eklenir:</span><span class="sxs-lookup"><span data-stu-id="b60d3-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="b60d3-143">SqlException Oturum açma tarafından istenen "RazorPagesMovieContext-21" veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="b60d3-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="b60d3-144">Oturum açılamadı.</span><span class="sxs-lookup"><span data-stu-id="b60d3-144">The login failed.</span></span>
<span data-ttu-id="b60d3-145">' Kullanıcı adı ' kullanıcısı için oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="b60d3-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="b60d3-146">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b60d3-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b60d3-148">VERITABANıNDAKI tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="b60d3-148">Delete all the records in the DB.</span></span> <span data-ttu-id="b60d3-149">Bunu, tarayıcıda veya [Ssox](xref:tutorials/razor-pages/new-field#ssox) 'ten silme bağlantılarıyla yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="b60d3-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="b60d3-150">Çekirdek yöntemin çalışması için uygulamayı başlamaya zorlayın ( `Startup` sınıftaki yöntemleri çağırın).</span><span class="sxs-lookup"><span data-stu-id="b60d3-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b60d3-151">Başlatmayı zorlamak için IIS Express durdurulup yeniden başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b60d3-152">Bunu aşağıdaki yaklaşımlardan biriyle yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b60d3-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b60d3-153">Bildirim alanında IIS Express sistem tepsisi simgesine sağ tıklayın ve **Çıkış** veya **siteyi durdur**' a dokunun:</span><span class="sxs-lookup"><span data-stu-id="b60d3-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * <span data-ttu-id="b60d3-156">VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="b60d3-157">Ile hata ayıklama modunda çalıştırıyorsanız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b60d3-158">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b60d3-159">VERITABANıNDAKI tüm kayıtları silin (Bu nedenle çekirdek yöntemi çalışacaktır).</span><span class="sxs-lookup"><span data-stu-id="b60d3-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b60d3-160">Veritabanını temel alarak uygulamayı durdurup başlatın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b60d3-161">Uygulama, sağlanan verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="b60d3-162">Sonraki öğreticide, verilerin sunumu gelişmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b60d3-163">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b60d3-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b60d3-164">[Öncekini Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)
> [ileri: Sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="b60d3-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="b60d3-165">Nesnesi veritabanına bağlanma ve nesneleri veritabanı kayıtlarına eşleme `Movie` görevini işler. `RazorPagesMovieContext`</span><span class="sxs-lookup"><span data-stu-id="b60d3-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b60d3-166">Veritabanı bağlamı, `ConfigureServices` *Startup.cs*içindeki yöntemde [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="b60d3-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b60d3-168">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="b60d3-169">İçinde `ConfigureServices`kullanılan yöntemler hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="b60d3-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="b60d3-170">`CookiePolicyOptions` [ASP.NET Core IÇIN AB genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr) .</span><span class="sxs-lookup"><span data-stu-id="b60d3-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="b60d3-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="b60d3-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="b60d3-172">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistemi okur `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b60d3-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b60d3-173">Yerel geliştirme için, *appSettings. JSON* dosyasından bağlantı dizesini alır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b60d3-175">Veritabanı (`Database={Database name}`) için ad değeri, üretilen kodunuz için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="b60d3-176">Ad değeri rastgele.</span><span class="sxs-lookup"><span data-stu-id="b60d3-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b60d3-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b60d3-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b60d3-178">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="b60d3-179">Uygulama bir test veya üretim sunucusuna dağıtıldığında, bağlantı dizesini gerçek bir veritabanı sunucusuna ayarlamak için bir ortam değişkeni kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="b60d3-180">Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="b60d3-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b60d3-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b60d3-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b60d3-183">LocalDB, program geliştirmeye yönelik SQL Server Express veritabanı altyapısının hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="b60d3-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="b60d3-184">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="b60d3-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b60d3-185">Varsayılan olarak, LocalDB veritabanı `*.mdf` `C:/Users/<user/>` dizinde dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b60d3-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="b60d3-186">**Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="b60d3-188">`Movie` Tabloya sağ tıklayıp **Görünüm Tasarımcısı**' nı seçin:</span><span class="sxs-lookup"><span data-stu-id="b60d3-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Film tablosunda bağlam menüsü açık](sql/_static/design.png)

  ![Tasarımcıda film tablosu aç](sql/_static/dv.png)

<span data-ttu-id="b60d3-191">Seçeneğinin yanında `ID`bulunan anahtar simgesine göz önünde edin.</span><span class="sxs-lookup"><span data-stu-id="b60d3-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b60d3-192">Varsayılan olarak, EF birincil anahtar için adlı `ID` bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b60d3-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="b60d3-193">`Movie` Tabloya sağ tıklayın ve **verileri görüntüle**' yi seçin:</span><span class="sxs-lookup"><span data-stu-id="b60d3-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verilerini gösteren film tablosu açma](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b60d3-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b60d3-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b60d3-196">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="b60d3-197">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="b60d3-197">Seed the database</span></span>

<span data-ttu-id="b60d3-198">Modeller klasöründe aşağıdaki kodla adlı `SeedData` yeni bir  sınıf oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b60d3-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b60d3-199">VERITABANıNDA herhangi bir film varsa, tohum başlatıcısı döner ve hiçbir film eklenmez.</span><span class="sxs-lookup"><span data-stu-id="b60d3-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="b60d3-200">Tohum başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="b60d3-200">Add the seed initializer</span></span>

<span data-ttu-id="b60d3-201">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="b60d3-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="b60d3-202">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="b60d3-203">Temel yöntemi çağırın ve bu yönteme geçerek bağlamı geçer.</span><span class="sxs-lookup"><span data-stu-id="b60d3-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="b60d3-204">Çekirdek yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="b60d3-205">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="b60d3-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="b60d3-206">Bir üretim uygulaması çağırmaz `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="b60d3-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="b60d3-207">Çalıştırılmayan aşağıdaki özel durumu `Update-Database` engellemek için önceki koda eklenir:</span><span class="sxs-lookup"><span data-stu-id="b60d3-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="b60d3-208">SqlException Oturum açma tarafından istenen "RazorPagesMovieContext-21" veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="b60d3-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="b60d3-209">Oturum açılamadı.</span><span class="sxs-lookup"><span data-stu-id="b60d3-209">The login failed.</span></span>
<span data-ttu-id="b60d3-210">' Kullanıcı adı ' kullanıcısı için oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="b60d3-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="b60d3-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b60d3-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b60d3-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b60d3-213">VERITABANıNDAKI tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="b60d3-213">Delete all the records in the DB.</span></span> <span data-ttu-id="b60d3-214">Bunu, tarayıcıda veya [Ssox](xref:tutorials/razor-pages/new-field#ssox) 'ten silme bağlantılarıyla yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="b60d3-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="b60d3-215">Çekirdek yöntemin çalışması için uygulamayı başlamaya zorlayın ( `Startup` sınıftaki yöntemleri çağırın).</span><span class="sxs-lookup"><span data-stu-id="b60d3-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b60d3-216">Başlatmayı zorlamak için IIS Express durdurulup yeniden başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b60d3-217">Bunu aşağıdaki yaklaşımlardan biriyle yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b60d3-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b60d3-218">Bildirim alanında IIS Express sistem tepsisi simgesine sağ tıklayın ve **Çıkış** veya **siteyi durdur**' a dokunun:</span><span class="sxs-lookup"><span data-stu-id="b60d3-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * <span data-ttu-id="b60d3-221">VS hata ayıklama modunda çalıştırıyorsanız, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="b60d3-222">Ile hata ayıklama modunda çalıştırıyorsanız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b60d3-223">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b60d3-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b60d3-224">VERITABANıNDAKI tüm kayıtları silin (Bu nedenle çekirdek yöntemi çalışacaktır).</span><span class="sxs-lookup"><span data-stu-id="b60d3-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b60d3-225">Veritabanını temel alarak uygulamayı durdurup başlatın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b60d3-226">Uygulama, sağlanan verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b60d3-227">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b60d3-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b60d3-228">VERITABANıNDAKI tüm kayıtları silin (Bu nedenle çekirdek yöntemi çalışacaktır).</span><span class="sxs-lookup"><span data-stu-id="b60d3-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b60d3-229">Veritabanını temel alarak uygulamayı durdurup başlatın.</span><span class="sxs-lookup"><span data-stu-id="b60d3-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b60d3-230">Uygulama, sağlanan verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="b60d3-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="b60d3-231">Uygulama, sağlanan verileri gösterir:</span><span class="sxs-lookup"><span data-stu-id="b60d3-231">The app shows the seeded data:</span></span>

![Film verilerini gösteren Chrome 'da film uygulaması açık](sql/_static/m55.png)

<span data-ttu-id="b60d3-233">Sonraki öğretici, verilerin sunumunu temizler.</span><span class="sxs-lookup"><span data-stu-id="b60d3-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b60d3-234">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b60d3-234">Additional resources</span></span>

* [<span data-ttu-id="b60d3-235">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="b60d3-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="b60d3-236">[Öncekini Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)
> [ileri: Sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="b60d3-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end