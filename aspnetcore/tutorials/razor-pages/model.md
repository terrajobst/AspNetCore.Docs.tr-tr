---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 95b6d3e016edcd2e13207c8e658cf0d2fb21f945
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959092"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="b2f2a-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="b2f2a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2f2a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b2f2a-105">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="b2f2a-106">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="b2f2a-107">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="b2f2a-108">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="b2f2a-109">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="b2f2a-110">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b2f2a-111">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-113">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b2f2a-114">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-114">Name the folder *Models*.</span></span>

<span data-ttu-id="b2f2a-115">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-115">Right click the *Models* folder.</span></span> <span data-ttu-id="b2f2a-116">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="b2f2a-117">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b2f2a-119">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="b2f2a-120">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b2f2a-122">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="b2f2a-123">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="b2f2a-124">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="b2f2a-125">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="b2f2a-126">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="b2f2a-127">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="b2f2a-128">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="b2f2a-129">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="b2f2a-130">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="b2f2a-130">Scaffold the movie model</span></span>

<span data-ttu-id="b2f2a-131">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="b2f2a-132">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-134">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="b2f2a-135">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="b2f2a-136">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-136">Name the folder *Movies*</span></span>

<span data-ttu-id="b2f2a-137">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="b2f2a-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="b2f2a-139">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="b2f2a-141">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="b2f2a-142">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="b2f2a-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="b2f2a-143">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="b2f2a-144">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="b2f2a-145">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="b2f2a-146">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-146">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="b2f2a-148">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="b2f2a-150">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2f2a-151">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="b2f2a-152">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="b2f2a-153">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-154">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b2f2a-155">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2f2a-156">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="b2f2a-157">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="b2f2a-158">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="b2f2a-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-160">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="b2f2a-161">*Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="b2f2a-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="b2f2a-163">Güncelleştirme tarihi</span><span class="sxs-lookup"><span data-stu-id="b2f2a-163">Updated</span></span>

* <span data-ttu-id="b2f2a-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-164">*Startup.cs*</span></span>

<span data-ttu-id="b2f2a-165">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b2f2a-166">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b2f2a-167">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="b2f2a-168">*Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="b2f2a-169">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="b2f2a-170">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="b2f2a-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-172">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="b2f2a-173">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-173">Add an initial migration.</span></span>
* <span data-ttu-id="b2f2a-174">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="b2f2a-175">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b2f2a-177">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-179">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="b2f2a-180">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="b2f2a-181">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="b2f2a-182">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="b2f2a-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="b2f2a-183">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="b2f2a-184">Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="b2f2a-185">Şema, `DbContext`belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="b2f2a-186">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="b2f2a-187">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="b2f2a-188">`update` komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="b2f2a-189">Bu durumda `update`, veritabanını oluşturan *geçiş/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="b2f2a-191">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="b2f2a-192">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b2f2a-193">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="b2f2a-194">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="b2f2a-195">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="b2f2a-196">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="b2f2a-197">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b2f2a-198">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="b2f2a-199">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="b2f2a-200">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="b2f2a-201">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="b2f2a-202">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-202">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="b2f2a-203">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="b2f2a-204">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="b2f2a-205">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="b2f2a-206">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b2f2a-208">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-209">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b2f2a-210">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="b2f2a-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-211">Test the app</span></span>

* <span data-ttu-id="b2f2a-212">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="b2f2a-213">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="b2f2a-214">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="b2f2a-215">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-215">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="b2f2a-217">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="b2f2a-218">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="b2f2a-219">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="b2f2a-220">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b2f2a-221">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2f2a-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b2f2a-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2f2a-223">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b2f2a-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b2f2a-224">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="b2f2a-225">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="b2f2a-226">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="b2f2a-227">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="b2f2a-228">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="b2f2a-229">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b2f2a-230">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-232">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b2f2a-233">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-233">Name the folder *Models*.</span></span>

<span data-ttu-id="b2f2a-234">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-234">Right click the *Models* folder.</span></span> <span data-ttu-id="b2f2a-235">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="b2f2a-236">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b2f2a-238">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="b2f2a-239">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-240">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b2f2a-241">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="b2f2a-242">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="b2f2a-243">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="b2f2a-244">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="b2f2a-245">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="b2f2a-246">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="b2f2a-247">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="b2f2a-248">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="b2f2a-249">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="b2f2a-249">Scaffold the movie model</span></span>

<span data-ttu-id="b2f2a-250">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="b2f2a-251">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-253">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="b2f2a-254">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="b2f2a-255">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-255">Name the folder *Movies*</span></span>

<span data-ttu-id="b2f2a-256">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="b2f2a-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="b2f2a-258">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="b2f2a-260">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="b2f2a-261">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="b2f2a-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="b2f2a-262">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="b2f2a-263">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-263">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="b2f2a-265">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="b2f2a-267">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="b2f2a-268">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-268">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="b2f2a-269">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-269">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-270">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b2f2a-271">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2f2a-272">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="b2f2a-273">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="b2f2a-274">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="b2f2a-274">Files created</span></span>

* <span data-ttu-id="b2f2a-275">*Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="b2f2a-276">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="b2f2a-277">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="b2f2a-277">File updated</span></span>

* <span data-ttu-id="b2f2a-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="b2f2a-278">*Startup.cs*</span></span>

<span data-ttu-id="b2f2a-279">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="b2f2a-280">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="b2f2a-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2f2a-282">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="b2f2a-283">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-283">Add an initial migration.</span></span>
* <span data-ttu-id="b2f2a-284">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="b2f2a-285">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b2f2a-287">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b2f2a-288">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b2f2a-289">Şema, `DbContext` belirtilen modele dayalıdır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="b2f2a-290">`InitialCreate` bağımsız değişkeni, geçişi adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="b2f2a-291">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="b2f2a-292">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="b2f2a-293">`Update-Database` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="b2f2a-294">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-296">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="b2f2a-297">Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2f2a-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="b2f2a-299">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="b2f2a-300">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b2f2a-301">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="b2f2a-302">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="b2f2a-303">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="b2f2a-304">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="b2f2a-305">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b2f2a-306">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="b2f2a-307">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="b2f2a-308">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="b2f2a-309">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="b2f2a-310">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-310">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="b2f2a-311">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="b2f2a-312">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="b2f2a-313">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="b2f2a-314">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2f2a-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2f2a-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b2f2a-316">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2f2a-317">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f2a-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b2f2a-318">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="b2f2a-319">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b2f2a-319">Test the app</span></span>

* <span data-ttu-id="b2f2a-320">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="b2f2a-321">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="b2f2a-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="b2f2a-322">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="b2f2a-323">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-323">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="b2f2a-325">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="b2f2a-326">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="b2f2a-327">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="b2f2a-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="b2f2a-328">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b2f2a-329">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="b2f2a-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2f2a-330">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b2f2a-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2f2a-331">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b2f2a-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
