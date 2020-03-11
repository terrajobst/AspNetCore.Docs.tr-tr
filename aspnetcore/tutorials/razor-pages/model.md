---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658937"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="130da-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="130da-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="130da-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="130da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="130da-105">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="130da-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="130da-106">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="130da-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="130da-107">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="130da-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="130da-108">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="130da-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="130da-109">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="130da-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="130da-110">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="130da-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="130da-111">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="130da-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-113">**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="130da-114">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-114">Name the folder *Models*.</span></span>

<span data-ttu-id="130da-115">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-115">Right click the *Models* folder.</span></span> <span data-ttu-id="130da-116"> > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="130da-117">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="130da-119">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="130da-120">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="130da-122">Çözüm Bölmesi, **RazorPagesMovie** projesine sağ tıklayın ve ardından > **Yeni klasör ekle...** seçeneğini belirleyin. Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="130da-123">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya Ekle...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="130da-124">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="130da-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="130da-125">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="130da-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="130da-127">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="130da-128">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="130da-129">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="130da-129">Scaffold the movie model</span></span>

<span data-ttu-id="130da-130">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="130da-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="130da-131">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-133">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="130da-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="130da-134">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="130da-135">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="130da-135">Name the folder *Movies*</span></span>

<span data-ttu-id="130da-136">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="130da-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="130da-138">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="130da-140">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="130da-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="130da-141">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="130da-142">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="130da-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="130da-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="130da-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="130da-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="130da-145">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-145">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="130da-147">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="130da-149">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="130da-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="130da-150">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="130da-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="130da-151">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="130da-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="130da-152">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="130da-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="130da-154">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="130da-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="130da-155">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="130da-156">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="130da-156">Name the folder *Movies*</span></span>

<span data-ttu-id="130da-157">*Sayfalar/filmler* klasörüne sağ tıklayarak > **yeni yapı Iskelesi > ekleyin...** .</span><span class="sxs-lookup"><span data-stu-id="130da-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/scaMac.png)

<span data-ttu-id="130da-159">**Yeni yapı iskelesi** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **İleri**' yi kullanarak seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="130da-161">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="130da-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="130da-162">**Model sınıfı** açılan kutusunda, seçin veya yazın, **Film (RazorPagesMovie. modeller)** .</span><span class="sxs-lookup"><span data-stu-id="130da-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="130da-163">**Veri bağlamı sınıfı** satırına, RazorPagesMovie adlı yeni sınıfın adını yazın. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="130da-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="130da-164">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="130da-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="130da-165">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="130da-166">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-166">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arpMac.png)

<span data-ttu-id="130da-168">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="130da-169">EF araçları ekleme</span><span class="sxs-lookup"><span data-stu-id="130da-169">Add EF tools</span></span>

<span data-ttu-id="130da-170">Aşağıdaki .NET Core CLI komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="130da-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="130da-171">Yukarıdaki komut, .NET Core CLI için Entity Framework Core araçları ekler.</span><span class="sxs-lookup"><span data-stu-id="130da-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="130da-172">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="130da-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-174">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="130da-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="130da-175">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="130da-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="130da-176">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="130da-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="130da-177">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="130da-177">Updated</span></span>

* <span data-ttu-id="130da-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="130da-178">*Startup.cs*</span></span>

<span data-ttu-id="130da-179">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="130da-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-180">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="130da-181">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="130da-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="130da-182">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="130da-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="130da-183">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="130da-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="130da-184">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="130da-184">Updated</span></span>

* <span data-ttu-id="130da-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="130da-185">*Startup.cs*</span></span>

<span data-ttu-id="130da-186">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="130da-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="130da-188">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="130da-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="130da-189">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="130da-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="130da-190">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="130da-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="130da-191">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="130da-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-193">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="130da-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="130da-194">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-194">Add an initial migration.</span></span>
* <span data-ttu-id="130da-195">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="130da-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="130da-196">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="130da-198">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="130da-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-200">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="130da-201">Yukarıdaki komutlar şu uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="130da-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="130da-202">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="130da-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="130da-203">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="130da-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="130da-204">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="130da-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="130da-205">Geçişler komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="130da-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="130da-206">Şema, `DbContext`belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="130da-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="130da-207">`InitialCreate` bağımsız değişkeni, geçişleri adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="130da-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="130da-208">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="130da-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="130da-209">`update` komutu uygulanmamış geçişlerde `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="130da-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="130da-210">Bu durumda `update`, veritabanını oluşturan *geçiş/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="130da-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="130da-212">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="130da-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="130da-213">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="130da-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="130da-214">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="130da-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="130da-215">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="130da-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="130da-216">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="130da-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="130da-217">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="130da-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="130da-218">`Startup.ConfigureServices` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="130da-219">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="130da-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="130da-220">`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder.</span><span class="sxs-lookup"><span data-stu-id="130da-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="130da-221">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="130da-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="130da-222">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="130da-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="130da-223">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="130da-224">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="130da-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="130da-225">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="130da-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="130da-226">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="130da-227">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="130da-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="130da-229">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-230">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="130da-231">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="130da-232">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="130da-232">Test the app</span></span>

* <span data-ttu-id="130da-233">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="130da-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="130da-234">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="130da-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="130da-235">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="130da-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="130da-236">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="130da-236">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="130da-238">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="130da-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="130da-239">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="130da-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="130da-240">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="130da-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="130da-241">**Düzenle**, **Ayrıntılar** ve **Sil** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="130da-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="130da-242">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="130da-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="130da-243">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="130da-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="130da-244">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Ileri: scafkatRazor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="130da-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="130da-245">Bu bölümde, platformlar arası bir [SQLite veritabanında](https://www.sqlite.org/index.html)film yönetimi için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="130da-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="130da-246">Bir ASP.NET Core şablondan oluşturulan uygulamalar bir SQLite veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="130da-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="130da-247">Uygulamanın model sınıfları, veritabanıyla çalışmak için [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core veritabanı sağlayıcısı](/ef/core/providers/sqlite)) ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="130da-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="130da-248">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="130da-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="130da-249">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="130da-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="130da-250">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="130da-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="130da-251">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="130da-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-253">**Yeni > klasör** **eklemek** > **RazorPagesMovie** projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="130da-254">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-254">Name the folder *Models*.</span></span>

<span data-ttu-id="130da-255">*Modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-255">Right click the *Models* folder.</span></span> <span data-ttu-id="130da-256"> > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="130da-257">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="130da-259">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="130da-260">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-261">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="130da-262">Çözüm Gezgini, **RazorPagesMovie** projesine sağ tıklayın ve ardından > **Yeni klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="130da-263">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="130da-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="130da-264">*Modeller* klasörüne sağ tıklayın ve ardından > **yeni dosya** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="130da-265">**Yeni dosya** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="130da-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="130da-266">Sol bölmedeki **genel** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="130da-267">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="130da-268">Sınıfı **filmi** adlandırın ve **Yeni**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="130da-269">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="130da-270">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="130da-270">Scaffold the movie model</span></span>

<span data-ttu-id="130da-271">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="130da-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="130da-272">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-274">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="130da-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="130da-275">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="130da-276">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="130da-276">Name the folder *Movies*</span></span>

<span data-ttu-id="130da-277">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="130da-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="130da-279">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="130da-281">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="130da-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="130da-282">**Model sınıfı** açılan kutusunda **Film (RazorPagesMovie. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="130da-283">**Veri bağlamı sınıfı** satırında **+** (artı) Işaretini seçin ve oluşturulan **RazorPagesMovie. modeller. RazorPagesMovieContext**adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="130da-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="130da-284">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-284">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="130da-286">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="130da-288">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="130da-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="130da-289">**Windows için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="130da-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="130da-290">**MacOS ve Linux için**: aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="130da-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-291">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="130da-292">*Sayfalar/filmler* klasörü oluştur:</span><span class="sxs-lookup"><span data-stu-id="130da-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="130da-293">**Yeni > klasör** **eklemek** > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="130da-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="130da-294">Klasör *filmlerini* adlandırın</span><span class="sxs-lookup"><span data-stu-id="130da-294">Name the folder *Movies*</span></span>

<span data-ttu-id="130da-295">*Sayfalar/filmler* klasörüne sağ tıklayın > > **yeni yapı Iskelesi öğesi** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="130da-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/scaMac.png)

<span data-ttu-id="130da-297">**Yeni yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="130da-299">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="130da-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="130da-300">**Model sınıfı** açılan kutusunda **film**' ı seçin veya yazın.</span><span class="sxs-lookup"><span data-stu-id="130da-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="130da-301">**Veri bağlamı sınıfı** satırına **RazorPagesMovieContext** öğesini seçin. Bu, doğru ad alanı ile yeni bir DB bağlam sınıfı oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="130da-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="130da-302">Bu durumda, **RazorPagesMovie. modeller. RazorPagesMovieContext**olacaktır.</span><span class="sxs-lookup"><span data-stu-id="130da-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="130da-303">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-303">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arpMac.png)

<span data-ttu-id="130da-305">*AppSettings. JSON* dosyası, yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesiyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="130da-306">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="130da-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="130da-307">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="130da-307">Files created</span></span>

* <span data-ttu-id="130da-308">*Sayfalar/filmler*: oluşturma, silme, ayrıntılar, düzenleme ve dizin.</span><span class="sxs-lookup"><span data-stu-id="130da-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="130da-309">*Data/RazorPagesMovieContext. cs*</span><span class="sxs-lookup"><span data-stu-id="130da-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="130da-310">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="130da-310">File updated</span></span>

* <span data-ttu-id="130da-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="130da-311">*Startup.cs*</span></span>

<span data-ttu-id="130da-312">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="130da-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="130da-313">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="130da-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="130da-315">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="130da-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="130da-316">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-316">Add an initial migration.</span></span>
* <span data-ttu-id="130da-317">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="130da-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="130da-318">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="130da-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="130da-320">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="130da-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="130da-321">`Add-Migration` komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="130da-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="130da-322">Şema, `DbContext` belirtilen modele dayalıdır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="130da-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="130da-323">`InitialCreate` bağımsız değişkeni, geçişi adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="130da-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="130da-324">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="130da-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="130da-325">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="130da-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="130da-326">`Update-Database` komutu *geçişler/\<zaman damgası > _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="130da-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="130da-327">`Up` yöntemi veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-329">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="130da-330">Yukarıdaki komutlar şu uyarıyı oluşturur: " *' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi. Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur. ' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin.* " Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="130da-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="130da-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="130da-332">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="130da-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="130da-333">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="130da-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="130da-334">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="130da-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="130da-335">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="130da-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="130da-336">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="130da-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="130da-337">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="130da-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="130da-338">`Startup.ConfigureServices` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="130da-339">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="130da-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="130da-340">`RazorPagesMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder.</span><span class="sxs-lookup"><span data-stu-id="130da-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="130da-341">Veri bağlamı (`RazorPagesMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="130da-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="130da-342">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="130da-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="130da-343">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="130da-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="130da-344">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="130da-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="130da-345">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="130da-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="130da-346">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="130da-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="130da-347">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="130da-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="130da-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="130da-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="130da-349">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="130da-350">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="130da-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="130da-351">`Up` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="130da-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="130da-352">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="130da-352">Test the app</span></span>

* <span data-ttu-id="130da-353">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="130da-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="130da-354">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="130da-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="130da-355">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="130da-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="130da-356">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="130da-356">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="130da-358">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="130da-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="130da-359">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="130da-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="130da-360">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="130da-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="130da-361">**Düzenle**, **Ayrıntılar** ve **Sil** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="130da-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="130da-362">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="130da-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="130da-363">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="130da-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="130da-364">[Önceki: başlangıç](xref:tutorials/razor-pages/razor-pages-start)
> [Ileri: scafkatRazor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="130da-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
