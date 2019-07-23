---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: b7f77cfa51f8d86504939e31eade0dfda8a6b1c9
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371930"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="0a2f3-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="0a2f3-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a2f3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0a2f3-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="0a2f3-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="0a2f3-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="0a2f3-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="0a2f3-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0a2f3-110">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-112">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="0a2f3-113">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-113">Name the folder *Models*.</span></span>

<span data-ttu-id="0a2f3-114">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-114">Right click the *Models* folder.</span></span> <span data-ttu-id="0a2f3-115">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="0a2f3-116">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a2f3-118">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0a2f3-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a2f3-121">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="0a2f3-122">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="0a2f3-123">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="0a2f3-124">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="0a2f3-125">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="0a2f3-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="0a2f3-127">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="0a2f3-128">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="0a2f3-129">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="0a2f3-129">Scaffold the movie model</span></span>

<span data-ttu-id="0a2f3-130">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="0a2f3-131">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-133">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="0a2f3-134">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="0a2f3-135">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-135">Name the folder *Movies*</span></span>

<span data-ttu-id="0a2f3-136">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="0a2f3-138">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="0a2f3-140">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="0a2f3-141">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="0a2f3-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="0a2f3-142">**Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="0a2f3-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="0a2f3-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="0a2f3-145">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-145">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="0a2f3-147">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="0a2f3-149">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0a2f3-150">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0a2f3-151">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0a2f3-152">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a2f3-154">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0a2f3-155">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0a2f3-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="0a2f3-157">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="0a2f3-158">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="0a2f3-158">Files created</span></span>

* <span data-ttu-id="0a2f3-159">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="0a2f3-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="0a2f3-161">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="0a2f3-161">File updated</span></span>

* <span data-ttu-id="0a2f3-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-162">*Startup.cs*</span></span>

<span data-ttu-id="0a2f3-163">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="0a2f3-164">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="0a2f3-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-166">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="0a2f3-167">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-167">Add an initial migration.</span></span>
* <span data-ttu-id="0a2f3-168">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="0a2f3-169">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0a2f3-171">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-173">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="0a2f3-174">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="0a2f3-175">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="0a2f3-176">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="0a2f3-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="0a2f3-177">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="0a2f3-178">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="0a2f3-179">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="0a2f3-180">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="0a2f3-181">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="0a2f3-182">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="0a2f3-183">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="0a2f3-185">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="0a2f3-186">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0a2f3-187">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="0a2f3-188">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="0a2f3-189">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="0a2f3-190">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="0a2f3-191">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0a2f3-192">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="0a2f3-193">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="0a2f3-194">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="0a2f3-195">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="0a2f3-196">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="0a2f3-197">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="0a2f3-198">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="0a2f3-199">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="0a2f3-200">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0a2f3-202">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-203">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0a2f3-204">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="0a2f3-205">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="0a2f3-206">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="0a2f3-207">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="0a2f3-208">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="0a2f3-209">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="0a2f3-210">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="0a2f3-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-211">Test the app</span></span>

* <span data-ttu-id="0a2f3-212">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="0a2f3-213">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="0a2f3-214">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="0a2f3-215">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-215">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="0a2f3-217">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="0a2f3-218">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="0a2f3-219">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="0a2f3-220">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="0a2f3-221">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a2f3-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0a2f3-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a2f3-223">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="0a2f3-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0a2f3-224">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="0a2f3-225">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="0a2f3-226">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="0a2f3-227">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="0a2f3-228">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0a2f3-229">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-231">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="0a2f3-232">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-232">Name the folder *Models*.</span></span>

<span data-ttu-id="0a2f3-233">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-233">Right click the *Models* folder.</span></span> <span data-ttu-id="0a2f3-234">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="0a2f3-235">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a2f3-237">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0a2f3-238">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-239">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a2f3-240">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="0a2f3-241">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="0a2f3-242">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="0a2f3-243">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="0a2f3-244">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="0a2f3-245">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="0a2f3-246">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="0a2f3-247">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="0a2f3-248">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="0a2f3-248">Scaffold the movie model</span></span>

<span data-ttu-id="0a2f3-249">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="0a2f3-250">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-252">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="0a2f3-253">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="0a2f3-254">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-254">Name the folder *Movies*</span></span>

<span data-ttu-id="0a2f3-255">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="0a2f3-257">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="0a2f3-259">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="0a2f3-260">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="0a2f3-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="0a2f3-261">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="0a2f3-262">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-262">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="0a2f3-264">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="0a2f3-266">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0a2f3-267">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0a2f3-268">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0a2f3-269">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-270">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a2f3-271">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0a2f3-272">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0a2f3-273">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="0a2f3-274">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="0a2f3-275">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="0a2f3-275">Files created</span></span>

* <span data-ttu-id="0a2f3-276">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="0a2f3-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="0a2f3-278">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="0a2f3-278">File updated</span></span>

* <span data-ttu-id="0a2f3-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="0a2f3-279">*Startup.cs*</span></span>

<span data-ttu-id="0a2f3-280">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="0a2f3-281">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="0a2f3-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a2f3-283">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="0a2f3-284">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-284">Add an initial migration.</span></span>
* <span data-ttu-id="0a2f3-285">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="0a2f3-286">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0a2f3-288">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-290">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="0a2f3-291">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="0a2f3-292">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="0a2f3-293">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="0a2f3-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="0a2f3-294">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="0a2f3-295">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="0a2f3-296">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="0a2f3-297">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="0a2f3-298">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="0a2f3-299">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="0a2f3-300">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a2f3-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="0a2f3-302">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="0a2f3-303">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0a2f3-304">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="0a2f3-305">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="0a2f3-306">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="0a2f3-307">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="0a2f3-308">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0a2f3-309">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="0a2f3-310">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="0a2f3-311">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="0a2f3-312">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="0a2f3-313">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="0a2f3-314">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="0a2f3-315">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="0a2f3-316">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="0a2f3-317">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a2f3-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a2f3-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0a2f3-319">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a2f3-320">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a2f3-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0a2f3-321">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="0a2f3-322">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="0a2f3-323">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="0a2f3-324">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="0a2f3-325">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="0a2f3-326">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="0a2f3-327">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="0a2f3-328">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0a2f3-328">Test the app</span></span>

* <span data-ttu-id="0a2f3-329">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="0a2f3-330">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="0a2f3-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="0a2f3-331">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="0a2f3-332">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-332">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="0a2f3-334">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="0a2f3-335">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="0a2f3-336">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="0a2f3-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="0a2f3-337">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="0a2f3-338">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="0a2f3-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a2f3-339">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0a2f3-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a2f3-340">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="0a2f3-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end