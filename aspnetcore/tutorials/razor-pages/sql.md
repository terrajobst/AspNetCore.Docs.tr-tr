---
title: SQL Server yerel veritabanı ve ASP.NET Core ile çalışma
author: rick-anderson
description: SQL Server yerel veritabanı ve ASP.NET Core ile çalışmayı açıklar.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 92a5965e7a535ca729c0bec13911b6bf051a7b19
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582875"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="d4769-103">SQL Server yerel veritabanı ve ASP.NET Core ile çalışma</span><span class="sxs-lookup"><span data-stu-id="d4769-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="d4769-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d4769-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="d4769-105">`MovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="d4769-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d4769-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d4769-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="d4769-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="d4769-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d4769-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="d4769-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span></span>

<span data-ttu-id="d4769-109">Kullanılan yöntemleri hakkında daha fazla bilgi için `ConfigureServices`, bkz:</span><span class="sxs-lookup"><span data-stu-id="d4769-109">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="d4769-110">[ASP.NET Core AB genel veri koruma düzenleme (GDPR) desteği](xref:security/gdpr) için `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="d4769-110">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="d4769-111">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="d4769-111">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="d4769-112">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="d4769-112">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="d4769-113">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d4769-113">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="d4769-114">Veritabanı için ad değeri (`Database={Database name}`) oluşturulan kodunuz için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d4769-114">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="d4769-115">Ad değer isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="d4769-115">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="d4769-116">Bir test veya üretim sunucusuna uygulama dağıtırken, bir ortam değişkeni veya başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server bağlantı dizesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d4769-116">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="d4769-117">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d4769-117">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d4769-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d4769-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d4769-119">Yerel veritabanı, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için hafif bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="d4769-119">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="d4769-120">Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="d4769-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="d4769-121">Varsayılan olarak, yerel veritabanı bir veritabanı oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizini.</span><span class="sxs-lookup"><span data-stu-id="d4769-121">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="d4769-122">Gelen **Görünüm** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d4769-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="d4769-124">Sağ tıklayın `Movie` tablo ve seçin **Görünüm Tasarımcısı**:</span><span class="sxs-lookup"><span data-stu-id="d4769-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Bağlamsal menü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

<span data-ttu-id="d4769-127">Anahtar simgesine yanına Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="d4769-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="d4769-128">Varsayılan olarak, EF adlı bir özellik oluşturur `ID` birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="d4769-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="d4769-129">Sağ tıklayın `Movie` tablo ve seçin **görünüm verilerini**:</span><span class="sxs-lookup"><span data-stu-id="d4769-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verisi gösteren açık film tablosu](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="d4769-131">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="d4769-131">Seed the database</span></span>

<span data-ttu-id="d4769-132">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="d4769-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="d4769-133">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4769-133">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d4769-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="d4769-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d4769-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="d4769-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

<span data-ttu-id="d4769-136">Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür ve hiçbir filmler eklenir.</span><span class="sxs-lookup"><span data-stu-id="d4769-136">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="d4769-137">Çekirdek Başlatıcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="d4769-137">Add the seed initializer</span></span>

<span data-ttu-id="d4769-138">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="d4769-138">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="d4769-139">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcıdan alın.</span><span class="sxs-lookup"><span data-stu-id="d4769-139">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="d4769-140">İçin bağlamı geçirme çekirdek yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="d4769-140">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="d4769-141">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="d4769-141">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="d4769-142">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="d4769-142">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d4769-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4769-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d4769-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4769-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span></span>

::: moniker-end

<span data-ttu-id="d4769-145">Bir üretim uygulaması çağrılmayan `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="d4769-145">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="d4769-146">Şu özel durum önlemek için önce gelen kod eklenen zaman `Update-Database` değil çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4769-146">It's added to the preceeding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="d4769-147">SqlException: "RazorPagesMovieContext-21" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="d4769-147">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="d4769-148">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="d4769-148">The login failed.</span></span>
<span data-ttu-id="d4769-149">Oturum açma kullanıcısı 'user name' için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="d4769-149">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d4769-150">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d4769-150">Test the app</span></span>

* <span data-ttu-id="d4769-151">DB tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="d4769-151">Delete all the records in the DB.</span></span> <span data-ttu-id="d4769-152">Tarayıcıda veya gelen silme bağlantılarla bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="d4769-152">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="d4769-153">Uygulamayı başlatmak için zorlama (yöntemleri Çağır `Startup` sınıfı) seed yöntemi çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="d4769-153">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="d4769-154">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="d4769-154">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="d4769-155">Bu yaklaşımın şunlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d4769-155">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="d4769-156">IIS Express sistem tepsisi bildirim alanı simgesini sağ tıklatın ve dokunun **çıkış** veya **durdurmak Site**:</span><span class="sxs-lookup"><span data-stu-id="d4769-156">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlam menüsü](sql/_static/stopIIS.png)

    * <span data-ttu-id="d4769-159">VS olmayan hata ayıklama modunda çalışıyormuş hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d4769-159">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="d4769-160">Hata ayıklama modunda VS çalışıyormuş hata ayıklayıcıyı durdurduktan ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d4769-160">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="d4769-161">Uygulama hazırlığı yapmış veriler gösterir:</span><span class="sxs-lookup"><span data-stu-id="d4769-161">The app shows the seeded data:</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

<span data-ttu-id="d4769-163">Sonraki öğretici verilerin sunuyu temizler.</span><span class="sxs-lookup"><span data-stu-id="d4769-163">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d4769-164">[Önceki: İskele kurulmuş Razor sayfalarının](xref:tutorials/razor-pages/page)
> [sonraki: sayfalarını güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="d4769-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
