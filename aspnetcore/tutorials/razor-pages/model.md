---
title: ASP.NET Core bir Razor Pages uygulamasına model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında film yönetmeye yönelik sınıfların nasıl ekleneceğini öğrenin.
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f2c9c2fc8112ef8a1a5afdbe448de6319c43521d
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761231"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="43472-103">ASP.NET Core bir Razor Pages uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="43472-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="43472-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="43472-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43472-105">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="43472-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="43472-106">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="43472-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="43472-107">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43472-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="43472-108">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="43472-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="43472-109">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="43472-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="43472-110">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="43472-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="43472-111">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="43472-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-113">**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="43472-114">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-114">Name the folder *Models*.</span></span>

<span data-ttu-id="43472-115">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-115">Right click the *Models* folder.</span></span> <span data-ttu-id="43472-116"> > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="43472-117">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="43472-119">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="43472-120">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43472-122">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="43472-123">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="43472-124">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="43472-125">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="43472-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="43472-126">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="43472-127">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="43472-128">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="43472-129">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="43472-130">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="43472-130">Scaffold the movie model</span></span>

<span data-ttu-id="43472-131">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="43472-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="43472-132">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="43472-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-134">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="43472-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="43472-135">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="43472-136">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="43472-136">Name the folder *Movies*</span></span>

<span data-ttu-id="43472-137">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="43472-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="43472-139">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="43472-141">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="43472-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="43472-142">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="43472-143">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="43472-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="43472-144">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="43472-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="43472-145">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43472-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="43472-146">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-146">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/3/arp.png)

<span data-ttu-id="43472-148">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="43472-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="43472-150">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="43472-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43472-151">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="43472-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43472-152">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="43472-153">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-154">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43472-155">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="43472-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43472-156">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="43472-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43472-157">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="43472-158">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="43472-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-160">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="43472-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="43472-161">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="43472-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="43472-162">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="43472-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="43472-163">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="43472-163">Updated</span></span>

* <span data-ttu-id="43472-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="43472-164">*Startup.cs*</span></span>

<span data-ttu-id="43472-165">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="43472-166">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="43472-167">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="43472-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="43472-168">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="43472-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="43472-169">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="43472-170">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="43472-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-172">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="43472-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="43472-173">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-173">Add an initial migration.</span></span>
* <span data-ttu-id="43472-174">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="43472-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="43472-175">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="43472-177">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="43472-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-179">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="43472-180">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="43472-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="43472-181">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="43472-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="43472-182">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="43472-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="43472-183">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="43472-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="43472-184">Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="43472-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="43472-185">Şema `DbContext` ' da belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="43472-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="43472-186">`InitialCreate` bağımsız değişkeni, geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43472-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="43472-187">Herhangi bir ad kullanılabilir, ancak geçiş işlemini açıklayan bir ad seçilir.</span><span class="sxs-lookup"><span data-stu-id="43472-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="43472-188">`update` komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="43472-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="43472-189">Bu durumda `update`, veritabanını oluşturan *geçişler/\<zaman damgası > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="43472-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="43472-191">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="43472-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="43472-192">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="43472-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43472-193">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="43472-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="43472-194">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="43472-195">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="43472-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="43472-196">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="43472-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="43472-197">`Startup.ConfigureServices` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="43472-198">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="43472-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="43472-199">`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder.</span><span class="sxs-lookup"><span data-stu-id="43472-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="43472-200">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="43472-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="43472-201">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="43472-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="43472-202">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43472-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="43472-203">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43472-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="43472-204">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43472-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="43472-205">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="43472-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="43472-206">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="43472-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43472-208">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-209">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="43472-210">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="43472-211">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="43472-211">Test the app</span></span>

* <span data-ttu-id="43472-212">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="43472-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="43472-213">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="43472-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="43472-214">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="43472-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="43472-215">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="43472-215">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="43472-217">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43472-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="43472-218">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="43472-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="43472-219">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="43472-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="43472-220">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="43472-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="43472-221">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43472-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="43472-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43472-223">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="43472-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43472-224">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="43472-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="43472-225">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="43472-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="43472-226">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43472-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="43472-227">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="43472-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="43472-228">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="43472-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="43472-229">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="43472-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="43472-230">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="43472-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-232">**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="43472-233">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-233">Name the folder *Models*.</span></span>

<span data-ttu-id="43472-234">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-234">Right click the *Models* folder.</span></span> <span data-ttu-id="43472-235"> > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="43472-236">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="43472-238">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="43472-239">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-240">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43472-241">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="43472-242">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="43472-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="43472-243">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="43472-244">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="43472-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="43472-245">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="43472-246">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="43472-247">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="43472-248">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="43472-249">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="43472-249">Scaffold the movie model</span></span>

<span data-ttu-id="43472-250">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="43472-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="43472-251">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="43472-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-253">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="43472-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="43472-254">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43472-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="43472-255">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="43472-255">Name the folder *Movies*</span></span>

<span data-ttu-id="43472-256">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="43472-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="43472-258">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="43472-260">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="43472-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="43472-261">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="43472-262">**Veri bağlamı sınıfı** satırında **+** (artı) Işaretini seçin ve oluşturulan **RazorPagesMovie. modeller. RazorPagesMovieContext**adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="43472-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="43472-263">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-263">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/arp.png)

<span data-ttu-id="43472-265">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="43472-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="43472-267">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="43472-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43472-268">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="43472-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43472-269">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="43472-270">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-271">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43472-272">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="43472-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43472-273">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="43472-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43472-274">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43472-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="43472-275">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="43472-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="43472-276">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="43472-276">Files created</span></span>

* <span data-ttu-id="43472-277">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="43472-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="43472-278">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="43472-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="43472-279">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="43472-279">File updated</span></span>

* <span data-ttu-id="43472-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="43472-280">*Startup.cs*</span></span>

<span data-ttu-id="43472-281">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="43472-282">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="43472-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43472-284">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="43472-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="43472-285">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-285">Add an initial migration.</span></span>
* <span data-ttu-id="43472-286">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="43472-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="43472-287">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="43472-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="43472-289">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="43472-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="43472-290">`Add-Migration` komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="43472-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="43472-291">Şema, `DbContext` ( *RazorPagesMovieContext.cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="43472-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="43472-292">`InitialCreate` bağımsız değişkeni, geçişi adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43472-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="43472-293">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş işlemini açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43472-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="43472-294">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="43472-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="43472-295">`Update-Database` komutu *geçişler/\<zaman damgası > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="43472-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="43472-296">`Up` yöntemi veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43472-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-298">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="43472-299">Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="43472-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43472-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="43472-301">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="43472-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="43472-302">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="43472-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43472-303">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="43472-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="43472-304">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="43472-305">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="43472-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="43472-306">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="43472-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="43472-307">`Startup.ConfigureServices` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="43472-308">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="43472-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="43472-309">`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder.</span><span class="sxs-lookup"><span data-stu-id="43472-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="43472-310">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="43472-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="43472-311">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="43472-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="43472-312">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43472-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="43472-313">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43472-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="43472-314">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43472-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="43472-315">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="43472-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="43472-316">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="43472-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43472-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43472-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43472-318">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43472-319">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43472-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="43472-320">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43472-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="43472-321">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="43472-321">Test the app</span></span>

* <span data-ttu-id="43472-322">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="43472-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="43472-323">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="43472-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="43472-324">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="43472-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="43472-325">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="43472-325">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="43472-327">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43472-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="43472-328">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="43472-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="43472-329">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="43472-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="43472-330">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="43472-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="43472-331">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43472-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43472-332">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="43472-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43472-333">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="43472-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
