---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 91fee1db820493be671fecaee3cfb4c1b7df8bd3
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121369"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="6a648-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="6a648-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="6a648-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a648-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="6a648-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="6a648-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="6a648-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="6a648-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="6a648-107">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6a648-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="6a648-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="6a648-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6a648-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="6a648-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="6a648-110">[Görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) örnek.</span><span class="sxs-lookup"><span data-stu-id="6a648-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="6a648-111">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="6a648-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a648-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a648-113">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="6a648-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6a648-114">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="6a648-114">Name the folder *Models*.</span></span>

<span data-ttu-id="6a648-115">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6a648-115">Right click the *Models* folder.</span></span> <span data-ttu-id="6a648-116">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6a648-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="6a648-117">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="6a648-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a648-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a648-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a648-119">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="6a648-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6a648-120">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a648-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a648-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a648-122">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="6a648-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6a648-123">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="6a648-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="6a648-124">Sağ *modelleri* klasöre tıklayın ve ardından **Ekle** > **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="6a648-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6a648-125">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="6a648-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6a648-126">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="6a648-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6a648-127">Seçin **boş sınıf** içinde merkezi kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="6a648-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="6a648-128">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="6a648-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="6a648-129">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="6a648-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6a648-130">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="6a648-130">Scaffold the movie model</span></span>

<span data-ttu-id="6a648-131">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="6a648-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6a648-132">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a648-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a648-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a648-134">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="6a648-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6a648-135">Sağ tıklayın *sayfaları* klasör > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="6a648-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6a648-136">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="6a648-136">Name the folder *Movies*</span></span>

<span data-ttu-id="6a648-137">Sağ tıklayın *sayfaları/filmler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6a648-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="6a648-139">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6a648-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="6a648-141">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="6a648-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="6a648-142">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="6a648-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6a648-143">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="6a648-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="6a648-144">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="6a648-144">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="6a648-146">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6a648-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a648-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a648-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6a648-148">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6a648-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6a648-149">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6a648-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6a648-150">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6a648-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6a648-151">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6a648-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a648-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a648-153">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6a648-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6a648-154">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6a648-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="6a648-155">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="6a648-155">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="6a648-156">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="6a648-156">Files created</span></span>

* <span data-ttu-id="6a648-157">*Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="6a648-157">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="6a648-158">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="6a648-158">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="6a648-159">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="6a648-159">File updated</span></span>

* <span data-ttu-id="6a648-160">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6a648-160">*Startup.cs*</span></span>

<span data-ttu-id="6a648-161">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6a648-161">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6a648-162">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="6a648-162">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a648-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-163">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="6a648-164">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6a648-164">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6a648-165">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6a648-165">Add an initial migration.</span></span>
* <span data-ttu-id="6a648-166">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6a648-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="6a648-167">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="6a648-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6a648-169">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6a648-169">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a648-170">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a648-170">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a648-171">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="6a648-172">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a648-172">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6a648-173">Belirtilen model şeması dayanır `DbContext` (içinde *Models/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="6a648-173">The schema is based on the model specified in the `DbContext` (In the *Models/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6a648-174">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6a648-174">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="6a648-175">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="6a648-175">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="6a648-176">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="6a648-176">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="6a648-177">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a648-177">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a648-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-178">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6a648-179">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="6a648-179">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6a648-180">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6a648-180">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6a648-181">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6a648-181">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6a648-182">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6a648-182">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6a648-183">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6a648-183">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6a648-184">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="6a648-184">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6a648-185">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6a648-185">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6a648-186">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="6a648-186">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="6a648-187">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="6a648-187">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6a648-188">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6a648-188">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6a648-189">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="6a648-189">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6a648-190">Yukarıdaki kod oluşturur bir [olan DB /\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="6a648-190">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6a648-191">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6a648-191">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6a648-192">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6a648-192">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6a648-193">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="6a648-193">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6a648-194">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6a648-194">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a648-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a648-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a648-196">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a648-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="6a648-197">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a648-197">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6a648-198">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="6a648-198">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6a648-199">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6a648-199">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6a648-200">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6a648-200">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="6a648-201">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6a648-201">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6a648-202">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a648-202">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6a648-203">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6a648-203">Test the app</span></span>

* <span data-ttu-id="6a648-204">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6a648-204">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6a648-205">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="6a648-205">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6a648-206">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6a648-206">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6a648-207">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6a648-207">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="6a648-209">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="6a648-209">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6a648-210">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="6a648-210">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6a648-211">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6a648-211">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6a648-212">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="6a648-212">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6a648-213">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="6a648-213">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6a648-214">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6a648-214">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
