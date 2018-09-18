---
title: ASP.NET Core ve SQL Server LocalDB ile çalışma
author: rick-anderson
description: SQL Server LocalDB ve ASP.NET Core ile çalışmayı açıklar.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 20e2353eb2e453235c2fb04c696a7e3d27bed5bf
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011280"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="eb46b-103">ASP.NET Core ve SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="eb46b-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="eb46b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="eb46b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="eb46b-105">`MovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="eb46b-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="eb46b-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="eb46b-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="eb46b-107">Kullanılan yöntemler hakkında daha fazla bilgi için `ConfigureServices`, bkz:</span><span class="sxs-lookup"><span data-stu-id="eb46b-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="eb46b-108">[ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr) için `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="eb46b-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="eb46b-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="eb46b-109">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

::: moniker-end

<span data-ttu-id="eb46b-110">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="eb46b-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="eb46b-111">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="eb46b-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="eb46b-112">Veritabanı adı değeri (`Database={Database name}`) oluşturulan kodunuz için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="eb46b-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="eb46b-113">Ad değeri isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="eb46b-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="eb46b-114">Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım.</span><span class="sxs-lookup"><span data-stu-id="eb46b-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="eb46b-115">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="eb46b-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="eb46b-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="eb46b-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="eb46b-117">LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="eb46b-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="eb46b-118">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="eb46b-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="eb46b-119">Varsayılan olarak LocalDB veritabanına oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizin.</span><span class="sxs-lookup"><span data-stu-id="eb46b-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="eb46b-120">Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="eb46b-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](sql/_static/ssox.png)

* <span data-ttu-id="eb46b-122">Sağ tıklayın `Movie` tablosunu seçip **Görünüm Tasarımcısı**:</span><span class="sxs-lookup"><span data-stu-id="eb46b-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](sql/_static/dv.png)

<span data-ttu-id="eb46b-125">Anahtar simgesinin yanındaki Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="eb46b-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="eb46b-126">Varsayılan olarak EF adlı bir özellik oluşturur. `ID` birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="eb46b-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="eb46b-127">Sağ tıklayın `Movie` tablosunu seçip **görünüm verilerini**:</span><span class="sxs-lookup"><span data-stu-id="eb46b-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tablo verilerini gösteren açık film tablo](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="eb46b-129">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="eb46b-129">Seed the database</span></span>

<span data-ttu-id="eb46b-130">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="eb46b-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="eb46b-131">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="eb46b-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="eb46b-132">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb46b-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="eb46b-133">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="eb46b-133">Add the seed initializer</span></span>

<span data-ttu-id="eb46b-134">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="eb46b-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="eb46b-135">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="eb46b-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="eb46b-136">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="eb46b-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="eb46b-137">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="eb46b-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="eb46b-138">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="eb46b-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="eb46b-139">Bir üretim uygulaması değil çağırırsınız `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="eb46b-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="eb46b-140">Aşağıdaki özel durumu önlemek için önceki koda eklenir, `Update-Database` değil çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="eb46b-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="eb46b-141">Hata: "21 RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="eb46b-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="eb46b-142">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="eb46b-142">The login failed.</span></span>
<span data-ttu-id="eb46b-143">Oturum açma 'kullanıcı adı' kullanıcı için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="eb46b-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="eb46b-144">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="eb46b-144">Test the app</span></span>

* <span data-ttu-id="eb46b-145">Veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="eb46b-145">Delete all the records in the DB.</span></span> <span data-ttu-id="eb46b-146">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="eb46b-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="eb46b-147">Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="eb46b-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="eb46b-148">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="eb46b-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="eb46b-149">Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="eb46b-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="eb46b-150">IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **Durdur Site**:</span><span class="sxs-lookup"><span data-stu-id="eb46b-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express sistem tepsisi simgesi](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](sql/_static/stopIIS.png)

    * <span data-ttu-id="eb46b-153">VS hata ayıklama olmayan modunda çalışmakta olan hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="eb46b-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="eb46b-154">VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="eb46b-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="eb46b-155">Uygulama, çekirdeği oluşturulmuş veri gösterilir:</span><span class="sxs-lookup"><span data-stu-id="eb46b-155">The app shows the seeded data:</span></span>

![Film verileri gösteren Chrome'da açık film uygulaması](sql/_static/m55.png)

<span data-ttu-id="eb46b-157">Sonraki öğreticiye verilerin sunuyu temizler.</span><span class="sxs-lookup"><span data-stu-id="eb46b-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eb46b-158">[Önceki: İskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)
> [sonraki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="eb46b-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
