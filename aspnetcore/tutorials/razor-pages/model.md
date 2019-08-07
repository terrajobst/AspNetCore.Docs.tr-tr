---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 6132f7b907014b4f57bb9ae0300e00b6ecb23f1a
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820073"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="63af5-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="63af5-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="63af5-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63af5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="63af5-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="63af5-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="63af5-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="63af5-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="63af5-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="63af5-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="63af5-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="63af5-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="63af5-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="63af5-110">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="63af5-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-112">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="63af5-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="63af5-113">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-113">Name the folder *Models*.</span></span>

<span data-ttu-id="63af5-114">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="63af5-114">Right click the *Models* folder.</span></span> <span data-ttu-id="63af5-115">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="63af5-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="63af5-116">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="63af5-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="63af5-118">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="63af5-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="63af5-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63af5-121">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="63af5-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="63af5-122">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="63af5-123">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="63af5-124">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="63af5-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="63af5-125">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="63af5-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="63af5-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="63af5-127">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="63af5-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="63af5-128">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="63af5-129">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="63af5-129">Scaffold the movie model</span></span>

<span data-ttu-id="63af5-130">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="63af5-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="63af5-131">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-133">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="63af5-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="63af5-134">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="63af5-135">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="63af5-135">Name the folder *Movies*</span></span>

<span data-ttu-id="63af5-136">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="63af5-138">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="63af5-140">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="63af5-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="63af5-141">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="63af5-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="63af5-142">**Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="63af5-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="63af5-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63af5-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="63af5-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="63af5-145">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-145">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="63af5-147">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="63af5-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="63af5-149">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="63af5-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="63af5-150">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="63af5-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="63af5-151">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="63af5-152">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63af5-154">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="63af5-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="63af5-155">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="63af5-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="63af5-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="63af5-157">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="63af5-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="63af5-158">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="63af5-158">Files created</span></span>

* <span data-ttu-id="63af5-159">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="63af5-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="63af5-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="63af5-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="63af5-161">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="63af5-161">File updated</span></span>

* <span data-ttu-id="63af5-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="63af5-162">*Startup.cs*</span></span>

<span data-ttu-id="63af5-163">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="63af5-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="63af5-164">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="63af5-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-166">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="63af5-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="63af5-167">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-167">Add an initial migration.</span></span>
* <span data-ttu-id="63af5-168">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="63af5-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="63af5-169">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="63af5-171">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="63af5-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-173">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="63af5-174">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="63af5-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="63af5-175">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="63af5-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="63af5-176">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="63af5-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="63af5-177">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="63af5-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="63af5-178">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="63af5-179">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="63af5-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="63af5-180">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="63af5-181">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="63af5-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="63af5-182">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="63af5-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="63af5-183">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="63af5-185">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="63af5-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="63af5-186">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="63af5-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="63af5-187">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63af5-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="63af5-188">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63af5-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="63af5-189">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="63af5-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="63af5-190">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="63af5-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="63af5-191">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="63af5-192">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="63af5-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="63af5-193">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="63af5-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="63af5-194">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="63af5-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="63af5-195">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="63af5-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="63af5-196">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="63af5-197">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="63af5-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="63af5-198">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="63af5-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="63af5-199">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="63af5-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="63af5-200">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="63af5-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="63af5-202">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-203">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="63af5-204">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="63af5-205">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="63af5-206">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="63af5-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="63af5-207">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="63af5-208">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="63af5-209">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="63af5-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="63af5-210">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="63af5-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="63af5-211">Test the app</span></span>

* <span data-ttu-id="63af5-212">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="63af5-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="63af5-213">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="63af5-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="63af5-214">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="63af5-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="63af5-215">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="63af5-215">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="63af5-217">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="63af5-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="63af5-218">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="63af5-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="63af5-219">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="63af5-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="63af5-220">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="63af5-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="63af5-221">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="63af5-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63af5-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63af5-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63af5-223">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="63af5-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="63af5-224">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="63af5-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="63af5-225">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="63af5-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="63af5-226">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="63af5-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="63af5-227">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="63af5-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="63af5-228">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="63af5-229">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="63af5-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-231">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="63af5-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="63af5-232">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-232">Name the folder *Models*.</span></span>

<span data-ttu-id="63af5-233">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="63af5-233">Right click the *Models* folder.</span></span> <span data-ttu-id="63af5-234">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="63af5-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="63af5-235">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="63af5-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="63af5-237">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="63af5-238">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="63af5-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-239">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63af5-240">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="63af5-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="63af5-241">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="63af5-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="63af5-242">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="63af5-243">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="63af5-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="63af5-244">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="63af5-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="63af5-245">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="63af5-246">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="63af5-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="63af5-247">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="63af5-248">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="63af5-248">Scaffold the movie model</span></span>

<span data-ttu-id="63af5-249">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="63af5-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="63af5-250">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-252">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="63af5-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="63af5-253">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="63af5-254">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="63af5-254">Name the folder *Movies*</span></span>

<span data-ttu-id="63af5-255">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63af5-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="63af5-257">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="63af5-259">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="63af5-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="63af5-260">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="63af5-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="63af5-261">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="63af5-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="63af5-262">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-262">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="63af5-264">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="63af5-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="63af5-266">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="63af5-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="63af5-267">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="63af5-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="63af5-268">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="63af5-269">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-270">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63af5-271">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="63af5-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="63af5-272">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="63af5-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="63af5-273">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="63af5-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="63af5-274">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="63af5-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="63af5-275">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="63af5-275">Files created</span></span>

* <span data-ttu-id="63af5-276">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="63af5-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="63af5-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="63af5-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="63af5-278">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="63af5-278">File updated</span></span>

* <span data-ttu-id="63af5-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="63af5-279">*Startup.cs*</span></span>

<span data-ttu-id="63af5-280">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="63af5-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="63af5-281">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="63af5-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63af5-283">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="63af5-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="63af5-284">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63af5-284">Add an initial migration.</span></span>
* <span data-ttu-id="63af5-285">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="63af5-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="63af5-286">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="63af5-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="63af5-288">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="63af5-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-290">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="63af5-291">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="63af5-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="63af5-292">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="63af5-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="63af5-293">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="63af5-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="63af5-294">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="63af5-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="63af5-295">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="63af5-296">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="63af5-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="63af5-297">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="63af5-298">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="63af5-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="63af5-299">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="63af5-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="63af5-300">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63af5-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="63af5-302">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="63af5-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="63af5-303">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="63af5-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="63af5-304">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63af5-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="63af5-305">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63af5-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="63af5-306">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="63af5-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="63af5-307">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="63af5-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="63af5-308">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="63af5-309">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="63af5-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="63af5-310">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="63af5-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="63af5-311">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="63af5-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="63af5-312">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="63af5-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="63af5-313">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="63af5-314">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="63af5-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="63af5-315">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="63af5-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="63af5-316">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="63af5-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="63af5-317">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="63af5-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63af5-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63af5-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="63af5-319">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63af5-320">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63af5-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="63af5-321">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63af5-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="63af5-322">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="63af5-323">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="63af5-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="63af5-324">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="63af5-325">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63af5-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="63af5-326">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="63af5-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="63af5-327">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63af5-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="63af5-328">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="63af5-328">Test the app</span></span>

* <span data-ttu-id="63af5-329">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="63af5-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="63af5-330">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="63af5-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="63af5-331">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="63af5-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="63af5-332">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="63af5-332">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="63af5-334">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="63af5-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="63af5-335">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="63af5-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="63af5-336">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="63af5-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="63af5-337">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="63af5-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="63af5-338">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="63af5-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63af5-339">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63af5-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63af5-340">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="63af5-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
