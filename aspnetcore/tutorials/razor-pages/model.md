---
title: ASP.NET Core bir Razor Pages uygulamasına model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında film yönetmeye yönelik sınıfların nasıl ekleneceğini öğrenin.
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 4f8b80cb51bd10eb3b136a780dc123c41d61c0a5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519082"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="bb1d3-103">ASP.NET Core bir Razor Pages uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="bb1d3-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="bb1d3-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="bb1d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb1d3-105">Bu bölümde, bir veritabanında film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="bb1d3-106">Bu sınıflar bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="bb1d3-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="bb1d3-108">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="bb1d3-109">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bb1d3-110">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="bb1d3-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-112">@No__t-2**Yeni klasör** **eklemek**> **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bb1d3-113">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-113">Name the folder *Models*.</span></span>

<span data-ttu-id="bb1d3-114">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-114">Right click the *Models* folder.</span></span> <span data-ttu-id="bb1d3-115">@No__t-1**sınıfı** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="bb1d3-116">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bb1d3-118">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="bb1d3-119">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bb1d3-121">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="bb1d3-122">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="bb1d3-123">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="bb1d3-124">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="bb1d3-125">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="bb1d3-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="bb1d3-127">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="bb1d3-128">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="bb1d3-129">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="bb1d3-129">Scaffold the movie model</span></span>

<span data-ttu-id="bb1d3-130">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="bb1d3-131">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-133">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="bb1d3-134">@No__t-2 **Yeni klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="bb1d3-135">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="bb1d3-135">Name the folder *Movies*</span></span>

<span data-ttu-id="bb1d3-136">*Sayfalar/filmler* klasörüne sağ tıklayarak > @no__t 2 **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="bb1d3-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="bb1d3-138">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="bb1d3-140">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="bb1d3-141">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="bb1d3-142">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="bb1d3-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="bb1d3-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="bb1d3-145">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-145">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/3/arp.png)

<span data-ttu-id="bb1d3-147">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="bb1d3-149">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bb1d3-150">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bb1d3-151">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="bb1d3-152">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bb1d3-154">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bb1d3-155">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bb1d3-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="bb1d3-157">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="bb1d3-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-159">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="bb1d3-160">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="bb1d3-161">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="bb1d3-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="bb1d3-162">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="bb1d3-162">Updated</span></span>

* <span data-ttu-id="bb1d3-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="bb1d3-163">*Startup.cs*</span></span>

<span data-ttu-id="bb1d3-164">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bb1d3-165">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="bb1d3-166">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="bb1d3-167">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="bb1d3-168">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="bb1d3-169">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="bb1d3-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-171">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="bb1d3-172">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-172">Add an initial migration.</span></span>
* <span data-ttu-id="bb1d3-173">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="bb1d3-174">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bb1d3-176">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-178">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="bb1d3-179">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="bb1d3-180">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="bb1d3-181">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="bb1d3-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="bb1d3-182">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="bb1d3-183">Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-183">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="bb1d3-184">Şema `DbContext` ' da belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-184">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="bb1d3-185">@No__t-0 bağımsız değişkeni geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="bb1d3-186">Herhangi bir ad kullanılabilir, ancak geçiş işlemini açıklayan bir ad seçilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="bb1d3-187">@No__t-0 komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-187">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="bb1d3-188">Bu durumda, `update`, veritabanını oluşturan *geçişler/\<time-damga > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-188">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="bb1d3-190">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="bb1d3-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="bb1d3-191">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bb1d3-192">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bb1d3-193">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bb1d3-194">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="bb1d3-195">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="bb1d3-196">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bb1d3-197">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="bb1d3-198">@No__t-0, `Movie` modeli için işlevselliği (oluşturma, okuma, güncelleştirme, silme, vb.) EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="bb1d3-199">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="bb1d3-200">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="bb1d3-201">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="bb1d3-202">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="bb1d3-203">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bb1d3-204">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="bb1d3-205">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bb1d3-207">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-208">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bb1d3-209">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-209">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="bb1d3-210">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="bb1d3-210">Test the app</span></span>

* <span data-ttu-id="bb1d3-211">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-211">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="bb1d3-212">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-212">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="bb1d3-213">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-213">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="bb1d3-214">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-214">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="bb1d3-216">@No__t-0 alanına ondalık virgüller giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-216">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="bb1d3-217">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-217">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="bb1d3-218">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-218">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="bb1d3-219">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-219">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="bb1d3-220">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-220">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb1d3-221">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bb1d3-221">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb1d3-222">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="bb1d3-222">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bb1d3-223">Bu bölümde, bir veritabanında film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-223">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="bb1d3-224">Bu sınıflar bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-224">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="bb1d3-225">EF Core, veri erişim kodunu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-225">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="bb1d3-226">Model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları ("düz eski CLR nesnelerinden") olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-226">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="bb1d3-227">Veritabanında depolanan verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-227">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bb1d3-228">Veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="bb1d3-228">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-230">@No__t-2**Yeni klasör** **eklemek**> **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-230">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bb1d3-231">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-231">Name the folder *Models*.</span></span>

<span data-ttu-id="bb1d3-232">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-232">Right click the *Models* folder.</span></span> <span data-ttu-id="bb1d3-233">@No__t-1**sınıfı** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-233">Select **Add** > **Class**.</span></span> <span data-ttu-id="bb1d3-234">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-234">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bb1d3-236">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-236">Add a folder named *Models*.</span></span>
* <span data-ttu-id="bb1d3-237">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-237">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-238">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-238">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bb1d3-239">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından  > **Yeni klasör** **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-239">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="bb1d3-240">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-240">Name the folder *Models*.</span></span>
* <span data-ttu-id="bb1d3-241">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-241">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="bb1d3-242">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-242">In the **New File** dialog:</span></span>

  * <span data-ttu-id="bb1d3-243">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-243">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="bb1d3-244">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-244">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="bb1d3-245">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-245">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="bb1d3-246">Derleme hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-246">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="bb1d3-247">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="bb1d3-247">Scaffold the movie model</span></span>

<span data-ttu-id="bb1d3-248">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-248">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="bb1d3-249">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-249">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-251">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-251">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="bb1d3-252">@No__t-2 **Yeni klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-252">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="bb1d3-253">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="bb1d3-253">Name the folder *Movies*</span></span>

<span data-ttu-id="bb1d3-254">*Sayfalar/filmler* klasörüne sağ tıklayarak > @no__t 2 **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="bb1d3-254">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/sca.png)

<span data-ttu-id="bb1d3-256">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-256">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/add_scaffold.png)

<span data-ttu-id="bb1d3-258">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-258">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="bb1d3-259">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-259">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="bb1d3-260">**Veri bağlamı sınıfı** satırında **+** (artı) Işaretini seçin ve oluşturulan **RazorPagesMovie. modeller. RazorPagesMovieContext**adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-260">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="bb1d3-261">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-261">Select **Add**.</span></span>

![Önceki yönergelerden görüntü.](model/_static/arp.png)

<span data-ttu-id="bb1d3-263">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-263">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-264">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-264">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="bb1d3-265">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-265">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bb1d3-266">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-266">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bb1d3-267">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-267">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="bb1d3-268">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-268">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-269">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-269">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bb1d3-270">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bb1d3-271">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-271">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bb1d3-272">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="bb1d3-273">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur ve güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="bb1d3-274">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="bb1d3-274">Files created</span></span>

* <span data-ttu-id="bb1d3-275">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="bb1d3-276">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="bb1d3-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="bb1d3-277">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="bb1d3-277">File updated</span></span>

* <span data-ttu-id="bb1d3-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="bb1d3-278">*Startup.cs*</span></span>

<span data-ttu-id="bb1d3-279">Oluşturulan ve güncelleştirilmiş dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="bb1d3-280">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="bb1d3-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb1d3-282">Bu bölümde, Paket Yöneticisi Konsolu (PMC) şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="bb1d3-283">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-283">Add an initial migration.</span></span>
* <span data-ttu-id="bb1d3-284">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="bb1d3-285">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bb1d3-287">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="bb1d3-288">@No__t-0 komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="bb1d3-289">Şema, `DbContext` ( *RazorPagesMovieContext.cs* dosyasında) içinde belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="bb1d3-290">@No__t-0 bağımsız değişkeni, geçişi adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="bb1d3-291">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş işlemini açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="bb1d3-292">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="bb1d3-293">@No__t-0 komutu, *geçişleri/\<Zaman damgası > _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="bb1d3-294">@No__t-0 yöntemi veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-296">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="bb1d3-297">Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb1d3-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="bb1d3-299">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="bb1d3-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="bb1d3-300">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bb1d3-301">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bb1d3-302">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bb1d3-303">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="bb1d3-304">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="bb1d3-305">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bb1d3-306">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="bb1d3-307">@No__t-0, `Movie` modeli için işlevselliği (oluşturma, okuma, güncelleştirme, silme, vb.) EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="bb1d3-308">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="bb1d3-309">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="bb1d3-310">Önceki kod, varlık kümesi için bir [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-310">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="bb1d3-311">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="bb1d3-312">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bb1d3-313">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="bb1d3-314">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb1d3-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb1d3-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bb1d3-316">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb1d3-317">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb1d3-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bb1d3-318">@No__t-0 yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="bb1d3-319">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="bb1d3-319">Test the app</span></span>

* <span data-ttu-id="bb1d3-320">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="bb1d3-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="bb1d3-321">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="bb1d3-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="bb1d3-322">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="bb1d3-323">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-323">Test the **Create** link.</span></span>

  ![Sayfa oluştur](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="bb1d3-325">@No__t-0 alanına ondalık virgüller giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="bb1d3-326">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="bb1d3-327">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="bb1d3-328">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="bb1d3-329">Sonraki öğreticide, yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb1d3-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb1d3-330">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bb1d3-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb1d3-331">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Sonraki: Scafkatal Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="bb1d3-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
