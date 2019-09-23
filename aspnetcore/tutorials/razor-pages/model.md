---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: dcbcf37dfd95d784ebe249ec6e9e4184a8853d3d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187191"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="aef9c-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="aef9c-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="aef9c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aef9c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="aef9c-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="aef9c-106">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="aef9c-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="aef9c-107">EF Core, veri erişimini kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="aef9c-108">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="aef9c-109">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="aef9c-110">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="aef9c-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-112">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="aef9c-113">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-113">Name the folder *Models*.</span></span>

<span data-ttu-id="aef9c-114">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="aef9c-114">Right click the *Models* folder.</span></span> <span data-ttu-id="aef9c-115">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="aef9c-116">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="aef9c-118">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="aef9c-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="aef9c-121">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="aef9c-122">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="aef9c-123">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="aef9c-124">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="aef9c-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="aef9c-125">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="aef9c-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="aef9c-126">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="aef9c-127">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="aef9c-128">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="aef9c-129">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="aef9c-129">Scaffold the movie model</span></span>

<span data-ttu-id="aef9c-130">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="aef9c-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="aef9c-131">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-133">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="aef9c-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="aef9c-134">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="aef9c-135">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="aef9c-135">Name the folder *Movies*</span></span>

<span data-ttu-id="aef9c-136">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="aef9c-138">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="aef9c-140">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="aef9c-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="aef9c-141">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="aef9c-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="aef9c-142">**Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin ve oluşturulan adı RazorPagesMovie ' dan değiştirin. **Modeller**. RazorPagesMovieContext to RazorPagesMovie. **Veri**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="aef9c-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="aef9c-143">[Bu değişiklik](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="aef9c-144">Doğru ad alanıyla veritabanı bağlamı sınıfını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="aef9c-145">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-145">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/3/arp.png)

<span data-ttu-id="aef9c-147">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="aef9c-149">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="aef9c-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="aef9c-150">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="aef9c-151">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="aef9c-152">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="aef9c-154">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="aef9c-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="aef9c-155">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="aef9c-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="aef9c-157">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="aef9c-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-159">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="aef9c-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="aef9c-160">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="aef9c-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="aef9c-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="aef9c-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="aef9c-162">Güncellendi</span><span class="sxs-lookup"><span data-stu-id="aef9c-162">Updated</span></span>

* <span data-ttu-id="aef9c-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="aef9c-163">*Startup.cs*</span></span>

<span data-ttu-id="aef9c-164">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="aef9c-165">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="aef9c-166">Yapı iskelesi işlemi aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="aef9c-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="aef9c-167">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="aef9c-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="aef9c-168">Oluşturulan dosyalar sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="aef9c-169">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="aef9c-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-171">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="aef9c-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="aef9c-172">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-172">Add an initial migration.</span></span>
* <span data-ttu-id="aef9c-173">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="aef9c-174">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="aef9c-176">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-178">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="aef9c-179">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="aef9c-180">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="aef9c-181">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="aef9c-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="aef9c-182">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="aef9c-183">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aef9c-184">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="aef9c-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="aef9c-185">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="aef9c-186">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="aef9c-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="aef9c-187">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="aef9c-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="aef9c-188">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="aef9c-190">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="aef9c-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="aef9c-191">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="aef9c-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="aef9c-192">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="aef9c-193">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="aef9c-194">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="aef9c-195">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="aef9c-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="aef9c-196">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="aef9c-197">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="aef9c-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="aef9c-198">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="aef9c-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="aef9c-199">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="aef9c-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="aef9c-200">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="aef9c-201">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="aef9c-202">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="aef9c-203">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="aef9c-204">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="aef9c-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="aef9c-205">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="aef9c-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="aef9c-207">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-208">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="aef9c-209">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="aef9c-210">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aef9c-211">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="aef9c-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="aef9c-212">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="aef9c-213">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="aef9c-214">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="aef9c-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="aef9c-215">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="aef9c-216">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="aef9c-216">Test the app</span></span>

* <span data-ttu-id="aef9c-217">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="aef9c-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="aef9c-218">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="aef9c-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="aef9c-219">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="aef9c-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="aef9c-220">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="aef9c-220">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="aef9c-222">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="aef9c-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="aef9c-223">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="aef9c-224">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="aef9c-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="aef9c-225">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="aef9c-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="aef9c-226">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="aef9c-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aef9c-227">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="aef9c-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aef9c-228">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="aef9c-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="aef9c-229">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar eklenir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="aef9c-230">İle kullanılan bu sınıflar [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="aef9c-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="aef9c-231">EF Core, veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="aef9c-232">EF Core üzerinde herhangi bir bağımlılığı olmadığından model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="aef9c-233">Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="aef9c-234">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="aef9c-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-236">Sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="aef9c-237">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-237">Name the folder *Models*.</span></span>

<span data-ttu-id="aef9c-238">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="aef9c-238">Right click the *Models* folder.</span></span> <span data-ttu-id="aef9c-239">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="aef9c-240">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="aef9c-242">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="aef9c-243">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-244">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="aef9c-245">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="aef9c-246">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="aef9c-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="aef9c-247">*Modeller* klasörüne sağ tıklayın ve ardından **yeni dosya** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="aef9c-248">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="aef9c-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="aef9c-249">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="aef9c-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="aef9c-250">Orta bölmede **boş sınıf** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="aef9c-251">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="aef9c-252">Derleme hata doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="aef9c-253">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="aef9c-253">Scaffold the movie model</span></span>

<span data-ttu-id="aef9c-254">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="aef9c-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="aef9c-255">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-257">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="aef9c-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="aef9c-258">**Yeni klasör** **eklemek** > > *Sayfalar* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="aef9c-259">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="aef9c-259">Name the folder *Movies*</span></span>

<span data-ttu-id="aef9c-260">**Yeni yapı iskelesi öğesi** **eklemek** > > *Sayfalar/filmler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="aef9c-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="aef9c-262">**Yapı iskelesi Ekle** iletişim kutusunda **Entity Framework (CRUD)** > **Ekle**Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="aef9c-264">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="aef9c-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="aef9c-265">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="aef9c-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="aef9c-266">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="aef9c-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="aef9c-267">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-267">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="aef9c-269">*Appsettings.json* dosya yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="aef9c-271">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="aef9c-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="aef9c-272">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="aef9c-273">**Windows için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="aef9c-274">**MacOS ve Linux için**: Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-275">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="aef9c-276">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="aef9c-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="aef9c-277">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="aef9c-278">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="aef9c-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="aef9c-279">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="aef9c-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="aef9c-280">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="aef9c-280">Files created</span></span>

* <span data-ttu-id="aef9c-281">*Sayfalar/filmler*: Oluşturma, silme, ayrıntılar, düzenleme ve dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="aef9c-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="aef9c-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="aef9c-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="aef9c-283">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="aef9c-283">File updated</span></span>

* <span data-ttu-id="aef9c-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="aef9c-284">*Startup.cs*</span></span>

<span data-ttu-id="aef9c-285">Oluşturulan ve güncelleştirilen dosyalar, sonraki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="aef9c-286">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="aef9c-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aef9c-288">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="aef9c-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="aef9c-289">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-289">Add an initial migration.</span></span>
* <span data-ttu-id="aef9c-290">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="aef9c-291">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="aef9c-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="aef9c-293">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="aef9c-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-295">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="aef9c-296">Yukarıdaki komutlar aşağıdaki uyarıyı oluşturur: "' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="aef9c-297">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="aef9c-298">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açık olarak belirtin. "</span><span class="sxs-lookup"><span data-stu-id="aef9c-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="aef9c-299">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="aef9c-300">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aef9c-301">Şema, `DbContext` öğesinde belirtilen modeli temel alır ( *RazorPagesMovieContext.cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="aef9c-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="aef9c-302">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="aef9c-303">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="aef9c-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="aef9c-304">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="aef9c-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="aef9c-305">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aef9c-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="aef9c-307">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="aef9c-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="aef9c-308">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="aef9c-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="aef9c-309">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="aef9c-310">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="aef9c-311">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="aef9c-312">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="aef9c-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="aef9c-313">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="aef9c-314">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="aef9c-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="aef9c-315">`RazorPagesMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="aef9c-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="aef9c-316">Veri bağlamı (`RazorPagesMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="aef9c-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="aef9c-317">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="aef9c-318">Önceki kod, varlık kümesi [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="aef9c-319">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="aef9c-320">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="aef9c-321">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="aef9c-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="aef9c-322">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="aef9c-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aef9c-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aef9c-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="aef9c-324">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aef9c-325">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aef9c-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="aef9c-326">İnceleme `Up` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aef9c-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="aef9c-327">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aef9c-328">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="aef9c-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="aef9c-329">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="aef9c-330">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aef9c-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="aef9c-331">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="aef9c-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="aef9c-332">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aef9c-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="aef9c-333">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="aef9c-333">Test the app</span></span>

* <span data-ttu-id="aef9c-334">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="aef9c-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="aef9c-335">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="aef9c-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="aef9c-336">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="aef9c-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="aef9c-337">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="aef9c-337">Test the **Create** link.</span></span>

  ![sayfası oluşturma](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="aef9c-339">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="aef9c-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="aef9c-340">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="aef9c-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="aef9c-341">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="aef9c-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="aef9c-342">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="aef9c-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="aef9c-343">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="aef9c-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aef9c-344">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="aef9c-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aef9c-345">[Öncekini Sonraki başlangıç](xref:tutorials/razor-pages/razor-pages-start):
> [ Yapı iskelesi Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="aef9c-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
