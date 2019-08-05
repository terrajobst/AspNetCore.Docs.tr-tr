---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 1c944a10ced7219aa9b0635b822f27081c92995f
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776726"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="d63b7-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="d63b7-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="d63b7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d63b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d63b7-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="d63b7-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="d63b7-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="d63b7-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="d63b7-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="d63b7-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="d63b7-110">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="d63b7-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-112">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="d63b7-113">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-113">Name the folder *Models*.</span></span>

<span data-ttu-id="d63b7-114">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="d63b7-114">Right click the *Models* folder.</span></span> <span data-ttu-id="d63b7-115">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="d63b7-116">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d63b7-118">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="d63b7-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d63b7-121">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="d63b7-122">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="d63b7-123">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="d63b7-124">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d63b7-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="d63b7-125">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="d63b7-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="d63b7-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="d63b7-127">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="d63b7-128">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="d63b7-129">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="d63b7-129">Scaffold the movie model</span></span>

<span data-ttu-id="d63b7-130">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="d63b7-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="d63b7-131">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-133">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="d63b7-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="d63b7-134">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="d63b7-135">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="d63b7-135">Name the folder *Movies*</span></span>

<span data-ttu-id="d63b7-136">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="d63b7-138">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="d63b7-140">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d63b7-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="d63b7-141">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="d63b7-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="d63b7-142">**Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="d63b7-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="d63b7-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="d63b7-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="d63b7-145">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-145">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="d63b7-147">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="d63b7-149">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="d63b7-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d63b7-150">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="d63b7-151">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="d63b7-152">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d63b7-154">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="d63b7-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d63b7-155">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="d63b7-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="d63b7-157">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="d63b7-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="d63b7-158">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="d63b7-158">Files created</span></span>

* <span data-ttu-id="d63b7-159">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d63b7-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="d63b7-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="d63b7-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="d63b7-161">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="d63b7-161">File updated</span></span>

* <span data-ttu-id="d63b7-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="d63b7-162">*Startup.cs*</span></span>

<span data-ttu-id="d63b7-163">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="d63b7-164">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="d63b7-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-166">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d63b7-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="d63b7-167">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-167">Add an initial migration.</span></span>
* <span data-ttu-id="d63b7-168">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="d63b7-169">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="d63b7-171">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-173">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="d63b7-174">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="d63b7-175">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="d63b7-176">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="d63b7-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="d63b7-177">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="d63b7-178">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d63b7-179">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="d63b7-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d63b7-180">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="d63b7-181">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="d63b7-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="d63b7-182">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="d63b7-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="d63b7-183">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="d63b7-185">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="d63b7-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="d63b7-186">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d63b7-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d63b7-187">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d63b7-188">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d63b7-189">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d63b7-190">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="d63b7-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="d63b7-191">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d63b7-192">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="d63b7-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="d63b7-193">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="d63b7-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="d63b7-194">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="d63b7-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d63b7-195">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="d63b7-196">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="d63b7-197">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="d63b7-198">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d63b7-199">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="d63b7-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d63b7-200">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d63b7-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d63b7-202">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-203">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d63b7-204">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="d63b7-205">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d63b7-206">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="d63b7-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d63b7-207">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d63b7-208">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="d63b7-209">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="d63b7-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="d63b7-210">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="d63b7-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d63b7-211">Test the app</span></span>

* <span data-ttu-id="d63b7-212">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="d63b7-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="d63b7-213">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="d63b7-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="d63b7-214">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="d63b7-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="d63b7-215">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="d63b7-215">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="d63b7-217">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="d63b7-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="d63b7-218">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="d63b7-219">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="d63b7-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="d63b7-220">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="d63b7-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d63b7-221">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="d63b7-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d63b7-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d63b7-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d63b7-223">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="d63b7-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d63b7-224">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="d63b7-225">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="d63b7-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="d63b7-226">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="d63b7-227">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="d63b7-228">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="d63b7-229">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="d63b7-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-231">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="d63b7-232">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-232">Name the folder *Models*.</span></span>

<span data-ttu-id="d63b7-233">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="d63b7-233">Right click the *Models* folder.</span></span> <span data-ttu-id="d63b7-234">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="d63b7-235">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d63b7-237">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="d63b7-238">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-239">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d63b7-240">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="d63b7-241">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="d63b7-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="d63b7-242">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="d63b7-243">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d63b7-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="d63b7-244">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="d63b7-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="d63b7-245">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="d63b7-246">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="d63b7-247">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="d63b7-248">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="d63b7-248">Scaffold the movie model</span></span>

<span data-ttu-id="d63b7-249">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="d63b7-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="d63b7-250">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-252">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="d63b7-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="d63b7-253">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="d63b7-254">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="d63b7-254">Name the folder *Movies*</span></span>

<span data-ttu-id="d63b7-255">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d63b7-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="d63b7-257">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="d63b7-259">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d63b7-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="d63b7-260">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="d63b7-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="d63b7-261">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="d63b7-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="d63b7-262">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-262">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="d63b7-264">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="d63b7-266">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="d63b7-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d63b7-267">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="d63b7-268">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="d63b7-269">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-270">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d63b7-271">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="d63b7-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d63b7-272">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="d63b7-273">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d63b7-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="d63b7-274">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="d63b7-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="d63b7-275">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="d63b7-275">Files created</span></span>

* <span data-ttu-id="d63b7-276">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d63b7-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="d63b7-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="d63b7-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="d63b7-278">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="d63b7-278">File updated</span></span>

* <span data-ttu-id="d63b7-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="d63b7-279">*Startup.cs*</span></span>

<span data-ttu-id="d63b7-280">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="d63b7-281">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="d63b7-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d63b7-283">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d63b7-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="d63b7-284">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-284">Add an initial migration.</span></span>
* <span data-ttu-id="d63b7-285">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="d63b7-286">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="d63b7-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="d63b7-288">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="d63b7-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-290">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="d63b7-291">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="d63b7-292">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="d63b7-293">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="d63b7-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="d63b7-294">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="d63b7-295">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d63b7-296">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="d63b7-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d63b7-297">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="d63b7-298">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="d63b7-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="d63b7-299">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="d63b7-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="d63b7-300">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d63b7-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="d63b7-302">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="d63b7-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="d63b7-303">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d63b7-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d63b7-304">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d63b7-305">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d63b7-306">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d63b7-307">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="d63b7-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="d63b7-308">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d63b7-309">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="d63b7-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="d63b7-310">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="d63b7-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="d63b7-311">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="d63b7-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d63b7-312">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="d63b7-313">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="d63b7-314">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="d63b7-315">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d63b7-316">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="d63b7-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d63b7-317">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d63b7-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d63b7-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d63b7-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d63b7-319">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d63b7-320">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d63b7-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d63b7-321">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d63b7-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="d63b7-322">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d63b7-323">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="d63b7-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d63b7-324">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d63b7-325">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d63b7-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="d63b7-326">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="d63b7-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="d63b7-327">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d63b7-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="d63b7-328">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d63b7-328">Test the app</span></span>

* <span data-ttu-id="d63b7-329">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="d63b7-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="d63b7-330">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="d63b7-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="d63b7-331">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="d63b7-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="d63b7-332">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="d63b7-332">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="d63b7-334">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="d63b7-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="d63b7-335">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="d63b7-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="d63b7-336">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="d63b7-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="d63b7-337">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="d63b7-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d63b7-338">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="d63b7-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d63b7-339">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d63b7-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d63b7-340">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="d63b7-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
