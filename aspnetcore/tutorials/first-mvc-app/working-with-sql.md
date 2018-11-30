---
title: Bir ASP.NET Core MVC uygulaması, SQL Server LocalDB ile çalışma
author: rick-anderson
description: SQL Server LocalDB basit bir ASP.NET Core MVC uygulamasında kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 49615c25d51cfa671157c2e56b8e0753719c678a
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710107"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="0614c-103">ASP.NET Core, SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="0614c-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="0614c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0614c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0614c-105">`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="0614c-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="0614c-106">Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0614c-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

<span data-ttu-id="0614c-107">ASP.NET Core [yapılandırma](xref:fundamentals/configuration/index) sistem okuma `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="0614c-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="0614c-108">İsteğe bağlı olarak yerel geliştirme için bağlantı dizesinden alır *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0614c-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="0614c-109">Bir test veya üretim sunucusuna uygulamasını dağıttığınızda, bir ortam değişkenine ya da başka bir kullanabilirsiniz yaklaşım gerçek bir SQL Server'a bağlantı dizesini ayarlayalım.</span><span class="sxs-lookup"><span data-stu-id="0614c-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="0614c-110">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0614c-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="0614c-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="0614c-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="0614c-112">LocalDB, SQL Server Express Veritabanı Altyapısı'nın hedeflenen program geliştirme için basit bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="0614c-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="0614c-113">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="0614c-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="0614c-114">Varsayılan olarak LocalDB veritabanına oluşturur "\*.mdf" dosyalar *C:/Users/\<kullanıcı\>*  dizin.</span><span class="sxs-lookup"><span data-stu-id="0614c-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="0614c-115">Gelen **görünümü** menüsünde, açık **SQL Server Nesne Gezgini** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="0614c-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Görünüm menüsü](working-with-sql/_static/ssox.png)

* <span data-ttu-id="0614c-117">Sağ tıklayın `Movie` tablo **> Görünüm Tasarımcısı**</span><span class="sxs-lookup"><span data-stu-id="0614c-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/design.png)

  ![Film Tablo Tasarımcısı'nda Aç](working-with-sql/_static/dv.png)

<span data-ttu-id="0614c-120">Anahtar simgesinin yanındaki Not `ID`.</span><span class="sxs-lookup"><span data-stu-id="0614c-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="0614c-121">Varsayılan olarak EF adlı bir özellik hale getirecek `ID` birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="0614c-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="0614c-122">Sağ tıklayın `Movie` tablo **> verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="0614c-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Bağlamsal menüyü film tablosunda Aç](working-with-sql/_static/ssox2.png)

  ![Tablo verilerini gösteren açık film tablo](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="0614c-125">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="0614c-125">Seed the database</span></span>

<span data-ttu-id="0614c-126">Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0614c-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="0614c-127">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0614c-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="0614c-128">Varsa tüm film DB'de, çekirdek Başlatıcı döndürür ve film eklenir.</span><span class="sxs-lookup"><span data-stu-id="0614c-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="0614c-129">Çekirdek Başlatıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="0614c-129">Add the seed initializer</span></span>

<span data-ttu-id="0614c-130">Öğesinin içeriğini değiştirin *Program.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="0614c-130">Replace the contents of *Program.cs* with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0614c-131">İçin çekirdek Başlatıcı Ekle `Main` yönteminde *Program.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0614c-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

::: moniker-end

<span data-ttu-id="0614c-132">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0614c-132">Test the app</span></span>

* <span data-ttu-id="0614c-133">Veritabanındaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="0614c-133">Delete all the records in the DB.</span></span> <span data-ttu-id="0614c-134">Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0614c-134">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="0614c-135">Başlatmaya zorlamak (yöntemleri çağırmak `Startup` sınıfı) bu nedenle seed yöntemi çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="0614c-135">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="0614c-136">Başlatma zorlamak için IIS Express durdurulup yeniden gerekir.</span><span class="sxs-lookup"><span data-stu-id="0614c-136">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="0614c-137">Bunu aşağıdaki yaklaşımlardan birini yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0614c-137">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="0614c-138">IIS Express sistem tepsisi simgesi bildirim alanında sağ tıklayın ve dokunun **çıkış** veya **sitesini Durdur**</span><span class="sxs-lookup"><span data-stu-id="0614c-138">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express sistem tepsisi simgesi](working-with-sql/_static/iisExIcon.png)

    ![Bağlamsal menü](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="0614c-141">VS hata ayıklama olmayan modda çalışıyormuş, hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="0614c-141">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="0614c-142">VS hata ayıklama modunda çalıştırdığınız, hata ayıklayıcıyı durdurun ve F5 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="0614c-142">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="0614c-143">Uygulama, çekirdeği oluşturulmuş veri gösterir.</span><span class="sxs-lookup"><span data-stu-id="0614c-143">The app shows the seeded data.</span></span>

![MVC film uygulaması film verileri gösteren Microsoft Edge'de açın](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="0614c-145">[Önceki](adding-model.md)
> [İleri](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="0614c-145">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
