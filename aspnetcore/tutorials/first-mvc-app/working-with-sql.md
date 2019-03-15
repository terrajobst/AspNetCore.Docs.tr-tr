---
title: Bir ASP.NET Core MVC uygulaması SQL ile çalışma
author: rick-anderson
description: Bir ASP.NET Core MVC uygulamasında SQL Server LocalDB veya SQLite kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a6b2026a43bda94a84a05749e9ea395308dec231
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841449"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="e49cd-103">ASP.NET core'da SQL ile çalışma</span><span class="sxs-lookup"><span data-stu-id="e49cd-103">Work with SQL in ASP.NET Core</span></span>

<span data-ttu-id="e49cd-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e49cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e49cd-105">`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="e49cd-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e49cd-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e49cd-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e49cd-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e49cd-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="e49cd-108">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e49cd-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e49cd-109">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e49cd-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e49cd-110">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="e49cd-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="e49cd-111">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e49cd-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e49cd-112">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e49cd-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="e49cd-113">Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım.</span><span class="sxs-lookup"><span data-stu-id="e49cd-113">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e49cd-114">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="e49cd-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e49cd-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e49cd-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e49cd-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e49cd-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e49cd-117">LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="e49cd-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="e49cd-118">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="e49cd-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e49cd-119">Varsayılan olarak LocalDB veritabanına oluşturur *.mdf* dosyalar *C:/Users / {user}* dizin.</span><span class="sxs-lookup"><span data-stu-id="e49cd-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="e49cd-120">Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e49cd-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* <span data-ttu-id="e49cd-122">Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**</span><span class="sxs-lookup"><span data-stu-id="e49cd-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

<span data-ttu-id="e49cd-125">Anahtar simgesinin yanındaki Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="e49cd-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e49cd-126">Varsayılan olarak EF adlı bir özellik hale getirecek `ID` birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="e49cd-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="e49cd-127">Sağ tıklayın `Movie` tablo **> verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="e49cd-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren açık film tablo](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e49cd-130">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="e49cd-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="e49cd-131">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="e49cd-131">Seed the database</span></span>

<span data-ttu-id="e49cd-132">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e49cd-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e49cd-133">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e49cd-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e49cd-134">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.</span><span class="sxs-lookup"><span data-stu-id="e49cd-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e49cd-135">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="e49cd-135">Add the seed initializer</span></span>

<span data-ttu-id="e49cd-136">Öğesinin içeriğini değiştirin *Program.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e49cd-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="e49cd-137">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="e49cd-137">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e49cd-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e49cd-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e49cd-139">Veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="e49cd-139">Delete all the records in the DB.</span></span> <span data-ttu-id="e49cd-140">Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e49cd-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="e49cd-141">Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="e49cd-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e49cd-142">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="e49cd-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e49cd-143">Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e49cd-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e49cd-144">IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **sitesini Durdur**</span><span class="sxs-lookup"><span data-stu-id="e49cd-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="e49cd-147">VS hata ayıklama olmayan modda çalışıyormuş, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="e49cd-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="e49cd-148">VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="e49cd-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e49cd-149">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="e49cd-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e49cd-150">Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="e49cd-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e49cd-151">Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="e49cd-151">Stop and start the app to seed the database.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="e49cd-152">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="e49cd-152">The app shows the seeded data.</span></span>

![MVC film uygulaması film verileri gösteren Microsoft Edge'de açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="e49cd-154">[Önceki](adding-model.md)
> [İleri](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="e49cd-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
