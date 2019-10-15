---
title: ASP.NET Core bir Razor Pages uygulamasına model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında film yönetmeye yönelik sınıfların nasıl ekleneceğini öğrenin.
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: c101fe4aee9a1fbf28d66a8527e3c199194d73fe
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334172"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="352ea-103">ASP.NET Core bir Razor Pages uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="352ea-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="352ea-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="352ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="352ea-105">Bu bölümde, bir veritabanında film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="352ea-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="352ea-106">Bu sınıflar bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="352ea-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="352ea-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="352ea-108">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="352ea-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="352ea-109">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="352ea-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="352ea-110">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="352ea-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-112">@No__t-2**Yeni klasör** **eklemek**> **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="352ea-113">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-113">Name the folder *Models*.</span></span>

<span data-ttu-id="352ea-114">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-114">Right click the *Models* folder.</span></span> <span data-ttu-id="352ea-115">@No__t-1**sınıfı** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="352ea-116">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="352ea-118">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="352ea-119">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="352ea-121">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="352ea-122">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="352ea-123">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="352ea-124">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="352ea-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="352ea-125">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="352ea-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="352ea-127">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="352ea-128">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="352ea-129">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="352ea-129">Scaffold the movie model</span></span>

<span data-ttu-id="352ea-130">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="352ea-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="352ea-131">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-133">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="352ea-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="352ea-134">@No__t-2 **Yeni klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="352ea-135">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="352ea-135">Name the folder *Movies*</span></span>

<span data-ttu-id="352ea-136">*Sayfalar/filmler* klasörüne sağ tıklayarak > @no__t 2 **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="352ea-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="352ea-138">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="352ea-140">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="352ea-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="352ea-141">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="352ea-142">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="352ea-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="352ea-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="352ea-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="352ea-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="352ea-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="352ea-145">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-145">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/3/arp.png)

<span data-ttu-id="352ea-147">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="352ea-149">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="352ea-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="352ea-150">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="352ea-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="352ea-151">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="352ea-152">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="352ea-154">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="352ea-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="352ea-155">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="352ea-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="352ea-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="352ea-157">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="352ea-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-159">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="352ea-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="352ea-160">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="352ea-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="352ea-161">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="352ea-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="352ea-162">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="352ea-162">Updated</span></span>

* <span data-ttu-id="352ea-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="352ea-163">*Startup.cs*</span></span>

<span data-ttu-id="352ea-164">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="352ea-165">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="352ea-166">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="352ea-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="352ea-167">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="352ea-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="352ea-168">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="352ea-169">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="352ea-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-171">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="352ea-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="352ea-172">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-172">Add an initial migration.</span></span>
* <span data-ttu-id="352ea-173">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="352ea-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="352ea-174">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="352ea-176">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="352ea-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-178">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="352ea-179">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="352ea-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="352ea-180">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="352ea-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="352ea-181">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="352ea-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="352ea-182">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="352ea-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="352ea-183">@No__t-0 komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="352ea-184">Şema, `DbContext` ( *RazorPagesMovieContext.cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="352ea-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="352ea-185">@No__t-0 bağımsız değişkeni geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="352ea-186">Herhangi bir ad kullanılabilir, ancak geçiş işlemini açıklayan bir ad seçilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="352ea-187">@No__t-0 komutu, *geçişleri/\<Zaman damgası > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="352ea-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="352ea-188">@No__t-0 yöntemi veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="352ea-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="352ea-190">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="352ea-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="352ea-191">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="352ea-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="352ea-192">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="352ea-193">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="352ea-194">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="352ea-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="352ea-195">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="352ea-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="352ea-196">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="352ea-197">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="352ea-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="352ea-198">@No__t-0, `Movie` modeli için işlevselliği (oluşturma, okuma, güncelleştirme, silme, vb.) EF Core.</span><span class="sxs-lookup"><span data-stu-id="352ea-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="352ea-199">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="352ea-200">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="352ea-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="352ea-201">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="352ea-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="352ea-202">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="352ea-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="352ea-203">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="352ea-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="352ea-204">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="352ea-205">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="352ea-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="352ea-207">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-208">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="352ea-209">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="352ea-210">@No__t-0 komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="352ea-211">Şema, `RazorPagesMovieContext` ( *Data/RazorPagesMovieContext. cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="352ea-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="352ea-212">@No__t-0 bağımsız değişkeni geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="352ea-213">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş işlemini açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="352ea-214">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="352ea-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="352ea-215">@No__t-0 komutu, veritabanını oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="352ea-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="352ea-216">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="352ea-216">Test the app</span></span>

* <span data-ttu-id="352ea-217">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="352ea-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="352ea-218">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="352ea-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="352ea-219">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="352ea-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="352ea-220">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="352ea-220">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="352ea-222">@No__t-0 alanına ondalık virgüller giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="352ea-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="352ea-223">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="352ea-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="352ea-224">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="352ea-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="352ea-225">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="352ea-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="352ea-226">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="352ea-227">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="352ea-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="352ea-228">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="352ea-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="352ea-229">Bu bölümde, bir veritabanında film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="352ea-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="352ea-230">Bu sınıflar bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="352ea-231">EF Core, veri erişim kodunu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="352ea-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="352ea-232">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="352ea-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="352ea-233">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="352ea-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="352ea-234">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="352ea-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-236">@No__t-2**Yeni klasör** **eklemek**> **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="352ea-237">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-237">Name the folder *Models*.</span></span>

<span data-ttu-id="352ea-238">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-238">Right click the *Models* folder.</span></span> <span data-ttu-id="352ea-239">@No__t-1**sınıfı** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="352ea-240">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="352ea-242">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="352ea-243">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-244">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="352ea-245">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="352ea-246">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="352ea-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="352ea-247">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="352ea-248">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="352ea-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="352ea-249">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="352ea-250">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="352ea-251">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="352ea-252">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="352ea-253">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="352ea-253">Scaffold the movie model</span></span>

<span data-ttu-id="352ea-254">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="352ea-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="352ea-255">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-257">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="352ea-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="352ea-258">@No__t-2 **Yeni klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="352ea-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="352ea-259">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="352ea-259">Name the folder *Movies*</span></span>

<span data-ttu-id="352ea-260">*Sayfalar/filmler* klasörüne sağ tıklayarak > @no__t 2 **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="352ea-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="352ea-262">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="352ea-264">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="352ea-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="352ea-265">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="352ea-266">**Veri bağlamı sınıfı** satırında **+** (artı) Işaretini seçin ve oluşturulan **RazorPagesMovie. modeller. RazorPagesMovieContext**adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="352ea-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="352ea-267">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-267">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/arp.png)

<span data-ttu-id="352ea-269">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="352ea-271">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="352ea-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="352ea-272">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="352ea-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="352ea-273">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="352ea-274">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-275">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="352ea-276">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="352ea-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="352ea-277">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="352ea-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="352ea-278">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="352ea-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="352ea-279">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="352ea-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="352ea-280">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="352ea-280">Files created</span></span>

* <span data-ttu-id="352ea-281">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="352ea-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="352ea-282">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="352ea-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="352ea-283">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="352ea-283">File updated</span></span>

* <span data-ttu-id="352ea-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="352ea-284">*Startup.cs*</span></span>

<span data-ttu-id="352ea-285">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="352ea-286">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="352ea-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="352ea-288">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="352ea-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="352ea-289">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-289">Add an initial migration.</span></span>
* <span data-ttu-id="352ea-290">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="352ea-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="352ea-291">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="352ea-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="352ea-293">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="352ea-293">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-295">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="352ea-296">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="352ea-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="352ea-297">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="352ea-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="352ea-298">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="352ea-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="352ea-299">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="352ea-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="352ea-300">@No__t-0 komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="352ea-301">Şema, `DbContext` ( *RazorPagesMovieContext.cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="352ea-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="352ea-302">@No__t-0 bağımsız değişkeni geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="352ea-303">Herhangi bir ad kullanılabilir, ancak geçiş işlemini açıklayan bir ad seçilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="352ea-304">@No__t-0 komutu, *geçişleri/\<Zaman damgası > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="352ea-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="352ea-305">@No__t-0 yöntemi veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="352ea-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="352ea-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="352ea-307">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="352ea-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="352ea-308">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="352ea-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="352ea-309">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="352ea-310">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="352ea-311">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="352ea-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="352ea-312">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="352ea-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="352ea-313">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="352ea-314">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="352ea-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="352ea-315">@No__t-0, `Movie` modeli için işlevselliği (oluşturma, okuma, güncelleştirme, silme, vb.) EF Core.</span><span class="sxs-lookup"><span data-stu-id="352ea-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="352ea-316">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="352ea-317">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="352ea-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="352ea-318">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="352ea-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="352ea-319">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="352ea-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="352ea-320">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="352ea-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="352ea-321">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="352ea-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="352ea-322">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="352ea-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="352ea-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="352ea-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="352ea-324">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="352ea-325">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="352ea-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="352ea-326">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="352ea-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="352ea-327">@No__t-0 komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="352ea-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="352ea-328">Şema, `RazorPagesMovieContext` ( *Data/RazorPagesMovieContext. cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="352ea-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="352ea-329">@No__t-0 bağımsız değişkeni geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="352ea-330">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş işlemini açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="352ea-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="352ea-331">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="352ea-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="352ea-332">@No__t-0 komutu, veritabanını oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="352ea-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="352ea-333">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="352ea-333">Test the app</span></span>

* <span data-ttu-id="352ea-334">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="352ea-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="352ea-335">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="352ea-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="352ea-336">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="352ea-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="352ea-337">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="352ea-337">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="352ea-339">@No__t-0 alanına ondalık virgüller giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="352ea-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="352ea-340">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="352ea-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="352ea-341">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="352ea-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="352ea-342">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="352ea-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="352ea-343">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="352ea-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="352ea-344">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="352ea-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="352ea-345">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="352ea-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
