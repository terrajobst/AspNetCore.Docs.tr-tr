---
title: ASP.NET Core bir Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: Entity Framework Çekirdek (EF çekirdek) kullanarak bir veritabanındaki filmler yönetmek için sınıfları ekleme bulur.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: edc8ceb3de7e2fe0afc106278307d3961e9d8845
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277364"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="28e48-103">ASP.NET Core bir Razor sayfalarının uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="28e48-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="28e48-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="28e48-104">Add a data model</span></span>

<span data-ttu-id="28e48-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="28e48-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="28e48-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="28e48-106">Name the folder *Models*.</span></span>

<span data-ttu-id="28e48-107">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="28e48-107">Right click the *Models* folder.</span></span> <span data-ttu-id="28e48-108">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="28e48-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="28e48-109">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="28e48-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="28e48-110">Değiştir `Movie` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="28e48-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="28e48-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="28e48-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="28e48-112">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="28e48-112">Scaffold the movie model</span></span>

<span data-ttu-id="28e48-113">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="28e48-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="28e48-114">Diğer bir deyişle, yapı iskelesi aracı film modeli için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleri için sayfaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e48-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="28e48-115">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="28e48-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="28e48-116">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasörü > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="28e48-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="28e48-117">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="28e48-117">Name the folder *Movies*</span></span>

<span data-ttu-id="28e48-118">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasörü > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="28e48-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="28e48-120">İçinde **İskele Ekle** iletişim kutusunda **Razor Entity Framework (CRUD) kullanarak sayfaları** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="28e48-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="28e48-122">Tamamlamak **Razor Entity Framework (CRUD) kullanarak Sayfa Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="28e48-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="28e48-123">İçinde **Model sınıfı** seçin, açılan **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="28e48-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="28e48-124">İçinde **veri bağlamı sınıfı** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="28e48-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="28e48-125">İçinde **veri bağlamı sınıfı** seçin, açılan **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="28e48-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="28e48-126">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="28e48-126">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="28e48-128">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="28e48-128">Perform initial migration</span></span>

<span data-ttu-id="28e48-129">Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="28e48-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="28e48-130">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e48-130">Add an initial migration.</span></span>
* <span data-ttu-id="28e48-131">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="28e48-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="28e48-132">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="28e48-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="28e48-134">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="28e48-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="28e48-135">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="28e48-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="28e48-136">Aşağıdaki uyarı iletisini yoksaymak, sonraki öğreticide düzeltin:</span><span class="sxs-lookup"><span data-stu-id="28e48-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="28e48-137">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e48-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="28e48-138">Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="28e48-138">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="28e48-139">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="28e48-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="28e48-140">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="28e48-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="28e48-141">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="28e48-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="28e48-142">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e48-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="28e48-143">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="28e48-143">If you get the error:</span></span>

<span data-ttu-id="28e48-144">SqlException: "GUID RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="28e48-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="28e48-145">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="28e48-145">The login failed.</span></span>
<span data-ttu-id="28e48-146">Oturum açma kullanıcısı 'User name' için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="28e48-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="28e48-147">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="28e48-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="28e48-148">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="28e48-148">Add a data model</span></span>

<span data-ttu-id="28e48-149">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="28e48-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="28e48-150">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="28e48-150">Name the folder *Models*.</span></span>

<span data-ttu-id="28e48-151">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="28e48-151">Right click the *Models* folder.</span></span> <span data-ttu-id="28e48-152">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="28e48-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="28e48-153">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="28e48-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="28e48-154">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="28e48-154">Add a database connection string</span></span>

<span data-ttu-id="28e48-155">Bir bağlantı dizesi eklemek *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="28e48-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="28e48-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="28e48-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="28e48-157">Veritabanı bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="28e48-157">Register the database context</span></span>

<span data-ttu-id="28e48-158">Veritabanı bağlamı ile kayıt [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının ConfigureServices yöntemi](xref:fundamentals/startup#the-startup-class) (*haline*):</span><span class="sxs-lookup"><span data-stu-id="28e48-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="28e48-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="28e48-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="28e48-160">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="28e48-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="28e48-161">İskele araç ekleyin ve başlangıç geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="28e48-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="28e48-162">Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="28e48-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="28e48-163">Visual Studio web kod oluşturma paketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e48-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="28e48-164">Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="28e48-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="28e48-165">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e48-165">Add an initial migration.</span></span>
* <span data-ttu-id="28e48-166">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="28e48-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="28e48-167">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="28e48-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="28e48-169">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="28e48-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="28e48-170">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="28e48-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="28e48-171">Aşağıdaki iletiyi göz ardı edin:</span><span class="sxs-lookup"><span data-stu-id="28e48-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="28e48-172">Sonraki öğreticide düzeltin.</span><span class="sxs-lookup"><span data-stu-id="28e48-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="28e48-173">`Install-Package` Komutu yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.</span><span class="sxs-lookup"><span data-stu-id="28e48-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="28e48-174">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e48-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="28e48-175">Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="28e48-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="28e48-176">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="28e48-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="28e48-177">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="28e48-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="28e48-178">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="28e48-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="28e48-179">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e48-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="28e48-180">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="28e48-180">Test the app</span></span>

* <span data-ttu-id="28e48-181">Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="28e48-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="28e48-182">Test **oluşturma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="28e48-182">Test the **Create** link.</span></span>

  ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="28e48-184">Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="28e48-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="28e48-185">Bir SQL özel durumu alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="28e48-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="28e48-186">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="28e48-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28e48-187">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="28e48-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
