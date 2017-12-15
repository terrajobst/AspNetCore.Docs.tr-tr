---
title: "Bir ASP.NET Core MVC uygulamasına bir modeli ekleme"
author: rick-anderson
description: "Bir model için basit bir ASP.NET Core uygulama ekleyin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 03c16e523fe2f91cae5c71357835684d813e3a1f
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="26c05-104">Not: ASP.NET Core 2.0 şablonlarını içeren *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="26c05-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="26c05-105">Sağ *modelleri* klasörü > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="26c05-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="26c05-106">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="26c05-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="26c05-107">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="26c05-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="26c05-108">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="26c05-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="26c05-109">Artık elinizde bir **M**odeldeki, **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="26c05-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="26c05-110">Bir denetleyici iskele kurma</span><span class="sxs-lookup"><span data-stu-id="26c05-110">Scaffolding a controller</span></span>

<span data-ttu-id="26c05-111">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="26c05-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

<span data-ttu-id="26c05-113">Varsa **MVC bağımlılıkları ekleyin** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="26c05-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="26c05-114">[Visual Studio en son sürüme güncelleştirin](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="26c05-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="26c05-115">Visual Studio sürümleri 15,5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="26c05-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="26c05-116">Update yapılamıyor, seçin **Ekle**ve ardından Ekle denetleyicisi adımları yeniden uygulayın.</span><span class="sxs-lookup"><span data-stu-id="26c05-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="26c05-117">İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="26c05-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim ekleyin](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="26c05-119">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="26c05-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="26c05-120">**Model sınıfı:** *film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="26c05-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="26c05-121">**Veri bağlamı sınıfı:** seçin  **+**  simgesini ve varsayılan ekleyin **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="26c05-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="26c05-123">**Görünümleri:** varsayılan her seçeneğinin işaretli koru</span><span class="sxs-lookup"><span data-stu-id="26c05-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="26c05-124">**Denetleyici adı:** varsayılan tutmak *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="26c05-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="26c05-125">Dokunun **Ekle**</span><span class="sxs-lookup"><span data-stu-id="26c05-125">Tap **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="26c05-127">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="26c05-127">Visual Studio creates:</span></span>

* <span data-ttu-id="26c05-128">Bir Entity Framework Çekirdek [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="26c05-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="26c05-129">Film denetleyicisi (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="26c05-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="26c05-130">Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="26c05-130">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="26c05-131">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*.</span><span class="sxs-lookup"><span data-stu-id="26c05-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="26c05-132">En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="26c05-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="26c05-133">Uygulamayı çalıştırın ve tıklayın, **Mvc film** bağlantı, aldığınız hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="26c05-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="26c05-134">Veritabanını oluşturmak gereken ve EF çekirdek kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="26c05-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="26c05-135">Geçişler veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şemasını güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="26c05-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="26c05-136">EF araç ekleyin ve başlangıç geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="26c05-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="26c05-137">Bu bölümde için Paket Yöneticisi Konsolu (PMC) kullanmanız:</span><span class="sxs-lookup"><span data-stu-id="26c05-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="26c05-138">Entity Framework Çekirdek araçları paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26c05-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="26c05-139">Bu paket, geçişler eklemek ve veritabanını güncelleştirmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="26c05-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="26c05-140">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26c05-140">Add an initial migration.</span></span>
* <span data-ttu-id="26c05-141">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="26c05-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="26c05-142">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="26c05-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="26c05-144">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="26c05-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="26c05-145">**Not:** ile bir hata alırsanız, `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="26c05-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="26c05-146">Bu, paket yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="26c05-146">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="26c05-147">Alternatif olarak, bkz: [CLI yaklaşım](#cli) PMC sorununuz varsa.</span><span class="sxs-lookup"><span data-stu-id="26c05-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="26c05-148">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="26c05-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="26c05-149">Belirtilen model şeması dayalı `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="26c05-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="26c05-150">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="26c05-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="26c05-151">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="26c05-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="26c05-152">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="26c05-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="26c05-153">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _Initial.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="26c05-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a><span data-ttu-id="26c05-154">Komut satırı arabirimi (CLI) kullanarak önceki adımları yerine PMC gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="26c05-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="26c05-155">Ekleme [EF çekirdek araç](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="26c05-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="26c05-156">(Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c05-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi bağlam menüsünde IntelliSense](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="26c05-158">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="26c05-158">Additional resources</span></span>

* [<span data-ttu-id="26c05-159">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="26c05-159">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="26c05-160">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="26c05-160">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="26c05-161">[Önceki bir görünümü ekleme](adding-view.md)
[sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="26c05-161">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
