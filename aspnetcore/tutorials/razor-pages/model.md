---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: ab2bf372b305672a95d34879ad2c13a620d93c7a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159294"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="1049b-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="1049b-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="1049b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1049b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="1049b-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="1049b-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="1049b-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="1049b-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="1049b-107">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="1049b-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="1049b-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="1049b-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="1049b-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="1049b-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="1049b-110">[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) örnek.</span><span class="sxs-lookup"><span data-stu-id="1049b-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="1049b-111">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="1049b-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1049b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1049b-113">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1049b-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="1049b-114">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="1049b-114">Name the folder *Models*.</span></span>

<span data-ttu-id="1049b-115">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="1049b-115">Right click the *Models* folder.</span></span> <span data-ttu-id="1049b-116">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="1049b-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="1049b-117">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="1049b-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1049b-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1049b-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1049b-119">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="1049b-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="1049b-120">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="1049b-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1049b-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1049b-122">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1049b-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="1049b-123">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="1049b-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="1049b-124">Sağ *modelleri* klasöre tıklayın ve ardından **Ekle** > **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="1049b-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="1049b-125">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="1049b-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="1049b-126">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="1049b-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="1049b-127">Seçin **boş sınıf** Orta bölmedeki.</span><span class="sxs-lookup"><span data-stu-id="1049b-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="1049b-128">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="1049b-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="1049b-129">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="1049b-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="1049b-130">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="1049b-130">Scaffold the movie model</span></span>

<span data-ttu-id="1049b-131">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="1049b-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="1049b-132">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1049b-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1049b-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1049b-134">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="1049b-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="1049b-135">Sağ tıklayın *sayfaları* klasör > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1049b-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="1049b-136">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="1049b-136">Name the folder *Movies*</span></span>

<span data-ttu-id="1049b-137">Sağ tıklayın *sayfaları/filmler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="1049b-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="1049b-139">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="1049b-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="1049b-141">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="1049b-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="1049b-142">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="1049b-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="1049b-143">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="1049b-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="1049b-144">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="1049b-144">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="1049b-146">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1049b-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1049b-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1049b-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="1049b-148">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="1049b-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1049b-149">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="1049b-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="1049b-150">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1049b-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="1049b-151">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1049b-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1049b-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1049b-153">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="1049b-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1049b-154">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="1049b-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="1049b-155">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1049b-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="1049b-156">Yukarıdaki komutlarda aşağıdaki uyarı oluştur: "Hiçbir türü ondalık sütunu 'Fiyat' varlık türünün 'Film' için belirtildi.</span><span class="sxs-lookup"><span data-stu-id="1049b-156">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="1049b-157">Bu, bunlar varsayılan kesinlik ve ölçek uygun değilse sessizce kesilebilir değerleri neden olur.</span><span class="sxs-lookup"><span data-stu-id="1049b-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="1049b-158">"Açıkça 'HasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin."</span><span class="sxs-lookup"><span data-stu-id="1049b-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="1049b-159">Bu uyarıyı yoksayabilirsiniz, bir sonraki öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="1049b-159">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="1049b-160">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="1049b-160">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="1049b-161">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="1049b-161">Files created</span></span>

* <span data-ttu-id="1049b-162">*Sayfa/filmler*: Oluşturma, silme, Ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="1049b-162">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="1049b-163">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="1049b-163">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="1049b-164">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="1049b-164">File updated</span></span>

* <span data-ttu-id="1049b-165">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="1049b-165">*Startup.cs*</span></span>

<span data-ttu-id="1049b-166">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1049b-166">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="1049b-167">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="1049b-167">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1049b-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-168">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="1049b-169">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="1049b-169">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="1049b-170">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1049b-170">Add an initial migration.</span></span>
* <span data-ttu-id="1049b-171">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1049b-171">Update the database with the initial migration.</span></span>

<span data-ttu-id="1049b-172">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="1049b-172">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="1049b-174">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="1049b-174">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1049b-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1049b-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1049b-176">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="1049b-177">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1049b-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1049b-178">Belirtilen model şeması dayanır `DbContext` (içinde *RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="1049b-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="1049b-179">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1049b-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="1049b-180">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="1049b-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="1049b-181">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1049b-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1049b-182">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1049b-182">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1049b-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-183">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="1049b-184">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="1049b-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="1049b-185">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1049b-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1049b-186">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1049b-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="1049b-187">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1049b-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="1049b-188">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1049b-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="1049b-189">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="1049b-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="1049b-190">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1049b-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="1049b-191">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="1049b-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="1049b-192">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="1049b-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="1049b-193">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="1049b-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="1049b-194">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="1049b-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="1049b-195">Yukarıdaki kod oluşturur bir [ `DbSet<Movie>` ](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="1049b-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="1049b-196">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1049b-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="1049b-197">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1049b-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="1049b-198">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="1049b-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="1049b-199">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1049b-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1049b-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1049b-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1049b-201">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1049b-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="1049b-202">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1049b-202">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1049b-203">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="1049b-203">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="1049b-204">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1049b-204">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="1049b-205">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1049b-205">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="1049b-206">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="1049b-206">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="1049b-207">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1049b-207">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="1049b-208">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1049b-208">Test the app</span></span>

* <span data-ttu-id="1049b-209">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="1049b-209">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="1049b-210">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="1049b-210">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="1049b-211">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="1049b-211">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="1049b-212">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="1049b-212">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="1049b-214">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="1049b-214">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="1049b-215">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="1049b-215">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="1049b-216">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="1049b-216">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="1049b-217">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="1049b-217">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="1049b-218">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="1049b-218">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1049b-219">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: İskeleli Razor sayfaları](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="1049b-219">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
