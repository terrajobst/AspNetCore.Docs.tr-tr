---
title: ASP.NET Core geçişleri ile EF Core Razor Pages-4/8
author: rick-anderson
description: Bu öğreticide, ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanmaya başlayabilirsiniz.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/migrations
ms.openlocfilehash: e6f44a32a473f8f3bacfa4d9608deba2f879b6b6
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259713"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="14d73-103">ASP.NET Core geçişleri ile EF Core Razor Pages-4/8</span><span class="sxs-lookup"><span data-stu-id="14d73-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="14d73-104">, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14d73-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="14d73-105">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="14d73-105">This tutorial introduces the EF Core migrations feature for managing data model changes.</span></span>

<span data-ttu-id="14d73-106">Yeni bir uygulama geliştirildiğinde, veri modeli sıklıkla değişir.</span><span class="sxs-lookup"><span data-stu-id="14d73-106">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="14d73-107">Modelin her değiştirilişinde, model veritabanıyla eşitlenmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="14d73-107">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="14d73-108">Bu öğretici serisi, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-108">This tutorial series started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="14d73-109">Veri modelinin her değiştirilişinde veritabanını bırakmalısınız.</span><span class="sxs-lookup"><span data-stu-id="14d73-109">Each time the data model changes, you have to drop the database.</span></span> <span data-ttu-id="14d73-110">Uygulamanın bir sonraki çalıştırılışında, `EnsureCreated` ' a yapılan çağrı, veritabanını yeni veri modeliyle eşleşecek şekilde yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-110">The next time the app runs, the call to `EnsureCreated` re-creates the database to match the new data model.</span></span> <span data-ttu-id="14d73-111">@No__t-0 sınıfı yeni veritabanını temel almak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="14d73-111">The `DbInitializer` class then runs to seed the new database.</span></span>

<span data-ttu-id="14d73-112">Veritabanını veri modeliyle eşitlenmiş halde tutmaya yönelik bu yaklaşım, uygulamayı üretime dağıtana kadar iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="14d73-112">This approach to keeping the database in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="14d73-113">Uygulama üretimde çalıştığında genellikle saklanması gereken verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="14d73-113">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="14d73-114">Uygulama her değişiklik yapıldığında (yeni sütun ekleme gibi) bir test veritabanıyla başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-114">The app can't start with a test database each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="14d73-115">EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine EF Core veritabanı şemasını güncelleştirmesine olanak sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="14d73-115">The EF Core Migrations feature solves this problem by enabling EF Core to update the database schema instead of creating a new database.</span></span>

<span data-ttu-id="14d73-116">Veri modeli değiştiğinde veritabanını bırakıp yeniden oluşturmak yerine, geçişler şemayı güncelleştirir ve var olan verileri korur.</span><span class="sxs-lookup"><span data-stu-id="14d73-116">Rather than dropping and recreating the database when the data model changes, migrations updates the schema and retains existing data.</span></span>

[!INCLUDE[](~/includes/sqlite-warn.md)]

## <a name="drop-the-database"></a><span data-ttu-id="14d73-117">Veritabanını bırak</span><span class="sxs-lookup"><span data-stu-id="14d73-117">Drop the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14d73-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14d73-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14d73-119">Veritabanını silmek için **SQL Server Nesne Gezgini** (ssox) kullanın veya **Paket Yöneticisi konsolunda** (PMC) şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-119">Use **SQL Server Object Explorer** (SSOX) to delete the database, or run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14d73-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14d73-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="14d73-121">EF CLı araçları 'nı yüklemek için komut isteminde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-121">Run the following command at a command prompt to install the EF CLI tools:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  ```

* <span data-ttu-id="14d73-122">Komut isteminde proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="14d73-122">In the command prompt, navigate to the project folder.</span></span> <span data-ttu-id="14d73-123">Proje klasörü *Contosouniversity. csproj* dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="14d73-123">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="14d73-124">*Cu. db* dosyasını silin veya şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-124">Delete the *CU.db* file, or run the following command:</span></span>

  ```dotnetcli
  dotnet ef database drop --force
  ```

---

## <a name="create-an-initial-migration"></a><span data-ttu-id="14d73-125">İlk geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="14d73-125">Create an initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14d73-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14d73-126">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14d73-127">PMC 'de şu komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-127">Run the following commands in the PMC:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14d73-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14d73-128">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="14d73-129">Komut isteminin proje klasöründe olduğundan emin olun ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-129">Make sure the command prompt is in the project folder, and run the following commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## <a name="up-and-down-methods"></a><span data-ttu-id="14d73-130">Yukarı ve aşağı Yöntemler</span><span class="sxs-lookup"><span data-stu-id="14d73-130">Up and Down methods</span></span>

<span data-ttu-id="14d73-131">EF Core `migrations add` komutu, veritabanını oluşturmak için kod üretti.</span><span class="sxs-lookup"><span data-stu-id="14d73-131">The EF Core `migrations add` command generated code to create the database.</span></span> <span data-ttu-id="14d73-132">Bu geçiş kodu *geçişleri @ no__t-1timestamp > _ınitialcreate. cs* dosyasında bulunur.</span><span class="sxs-lookup"><span data-stu-id="14d73-132">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="14d73-133">@No__t-1 sınıfının `Up` yöntemi, veri modeli varlık kümelerine karşılık gelen veritabanı tablolarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-133">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="14d73-134">@No__t-0 yöntemi, aşağıdaki örnekte gösterildiği gibi bunları siler:</span><span class="sxs-lookup"><span data-stu-id="14d73-134">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu30/Migrations/20190731193522_InitialCreate.cs)]

<span data-ttu-id="14d73-135">Önceki kod ilk geçişe yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="14d73-135">The preceding code is for the initial migration.</span></span> <span data-ttu-id="14d73-136">Kod:</span><span class="sxs-lookup"><span data-stu-id="14d73-136">The code:</span></span>

* <span data-ttu-id="14d73-137">@No__t-0 komutu tarafından oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="14d73-137">Was generated by the `migrations add InitialCreate` command.</span></span> 
* <span data-ttu-id="14d73-138">@No__t-0 komutu tarafından yürütülür.</span><span class="sxs-lookup"><span data-stu-id="14d73-138">Is executed by the `database update` command.</span></span>
* <span data-ttu-id="14d73-139">Veritabanı bağlamı sınıfı tarafından belirtilen veri modeli için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-139">Creates a database for the data model specified by the database context class.</span></span>

<span data-ttu-id="14d73-140">Dosya adı için geçiş adı parametresi (örnekteki "ınitialcreate") kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-140">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="14d73-141">Geçiş adı herhangi bir geçerli dosya adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="14d73-141">The migration name can be any valid file name.</span></span> <span data-ttu-id="14d73-142">Geçiş sırasında nelerin yapıldığını özetleyen bir sözcük veya tümcecik seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="14d73-142">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="14d73-143">Örneğin, bir departman tablosu ekleyen bir geçişe "AddDepartmentTable" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="14d73-143">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

## <a name="the-migrations-history-table"></a><span data-ttu-id="14d73-144">Geçişler geçmiş tablosu</span><span class="sxs-lookup"><span data-stu-id="14d73-144">The migrations history table</span></span>

* <span data-ttu-id="14d73-145">Veritabanını incelemek için SSOX veya SQLite aracınızı kullanın.</span><span class="sxs-lookup"><span data-stu-id="14d73-145">Use SSOX or your SQLite tool to inspect the database.</span></span>
* <span data-ttu-id="14d73-146">@No__t-0 tablosuna eklendiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="14d73-146">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="14d73-147">@No__t-0 tablosu, veritabanına hangi geçişlerin uygulandığını izler.</span><span class="sxs-lookup"><span data-stu-id="14d73-147">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the database.</span></span>
* <span data-ttu-id="14d73-148">@No__t-0 tablosundaki verileri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="14d73-148">View the data in the `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="14d73-149">İlk geçiş için bir satır gösterir.</span><span class="sxs-lookup"><span data-stu-id="14d73-149">It shows one row for the first migration.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="14d73-150">Veri modeli anlık görüntüsü</span><span class="sxs-lookup"><span data-stu-id="14d73-150">The data model snapshot</span></span>

<span data-ttu-id="14d73-151">Geçişler, *geçiş/SchoolContextModelSnapshot. cs*içindeki geçerli veri modelinin *anlık görüntüsünü* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-151">Migrations creates a *snapshot* of the current data model in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="14d73-152">Bir geçiş eklediğinizde, EF geçerli veri modelini Snapshot dosyası ile karşılaştırarak nelerin değiştirildiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="14d73-152">When you add a migration, EF determines what changed by comparing the current data model to the snapshot file.</span></span>

<span data-ttu-id="14d73-153">Anlık görüntü dosyası veri modelinin durumunu izlediğinden, `<timestamp>_<migrationname>.cs` dosyasını silerek bir geçişi silemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="14d73-153">Because the snapshot file tracks the state of the data model, you can't delete a migration by deleting the `<timestamp>_<migrationname>.cs` file.</span></span> <span data-ttu-id="14d73-154">En son geçişi geri yüklemek için `migrations remove` komutunu kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="14d73-154">To back out the most recent migration, you have to use the `migrations remove` command.</span></span> <span data-ttu-id="14d73-155">Bu komut, geçişi siler ve anlık görüntünün doğru şekilde sıfırlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="14d73-155">That command deletes the migration and ensures the snapshot is correctly reset.</span></span> <span data-ttu-id="14d73-156">Daha fazla bilgi için bkz. [DotNet EF geçişleri kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="14d73-156">For more information, see [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="14d73-157">Yeniden oluşturulmasını kaldır</span><span class="sxs-lookup"><span data-stu-id="14d73-157">Remove EnsureCreated</span></span>

<span data-ttu-id="14d73-158">Bu öğretici serisi `EnsureCreated` kullanılarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="14d73-158">This tutorial series started by using `EnsureCreated`.</span></span> <span data-ttu-id="14d73-159">`EnsureCreated` bir geçişler geçmişi tablosu oluşturmaz ve geçişle birlikte kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-159">`EnsureCreated` doesn't create a migrations history table and so can't be used with migrations.</span></span> <span data-ttu-id="14d73-160">Bu, veritabanının düşürülme ve sıklıkla yeniden oluşturulduğu test veya hızlı prototip oluşturma için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="14d73-160">It's designed for testing or rapid prototyping where the database is dropped and re-created frequently.</span></span>

<span data-ttu-id="14d73-161">Bu noktadan sonra öğreticiler, geçişleri kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="14d73-161">From this point forward, the tutorials will use migrations.</span></span>

<span data-ttu-id="14d73-162">*Data/Dbınınitializer. cs*dosyasında aşağıdaki satırı açıklama olarak inceleyin:</span><span class="sxs-lookup"><span data-stu-id="14d73-162">In *Data/DBInitializer.cs*, comment out the following line:</span></span>

```csharp
context.Database.EnsureCreated();
```
<span data-ttu-id="14d73-163">Uygulamayı çalıştırın ve veritabanının çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="14d73-163">Run the app and verify that the database is seeded.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="14d73-164">Üretimde geçişleri uygulama</span><span class="sxs-lookup"><span data-stu-id="14d73-164">Applying migrations in production</span></span>

<span data-ttu-id="14d73-165">Uygulama başlangıcında, üretim uygulamalarının [Database. Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) **olarak çağırmalarını** öneririz.</span><span class="sxs-lookup"><span data-stu-id="14d73-165">We recommend that production apps **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="14d73-166">`Migrate` bir sunucu grubuna dağıtılan bir uygulamadan çağrılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="14d73-166">`Migrate` shouldn't be called from an app that is deployed to a server farm.</span></span> <span data-ttu-id="14d73-167">Uygulama birden çok sunucu örneğine ölçekleniyorsa, veritabanı şeması güncelleştirmelerinin birden çok sunucudan oluşmaması veya okuma/yazma erişimiyle çakışmamasını sağlamak zordur.</span><span class="sxs-lookup"><span data-stu-id="14d73-167">If the app is scaled out to multiple server instances, it's hard to ensure database schema updates don't happen from multiple servers or conflict with read/write access.</span></span>

<span data-ttu-id="14d73-168">Veritabanı geçişi, dağıtımın bir parçası olarak ve denetimli bir şekilde yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="14d73-168">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="14d73-169">Üretim veritabanı geçiş yaklaşımları şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="14d73-169">Production database migration approaches include:</span></span>

* <span data-ttu-id="14d73-170">SQL betikleri oluşturmak ve dağıtımda SQL betikleri kullanmak için geçişleri kullanma.</span><span class="sxs-lookup"><span data-stu-id="14d73-170">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="14d73-171">Denetlenen bir ortamdan `dotnet ef database update` çalıştırılıyor.</span><span class="sxs-lookup"><span data-stu-id="14d73-171">Running `dotnet ef database update` from a controlled environment.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="14d73-172">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="14d73-172">Troubleshooting</span></span>

<span data-ttu-id="14d73-173">Uygulama SQL Server LocalDB kullanıyorsa ve aşağıdaki özel durumu görüntülüyorsa:</span><span class="sxs-lookup"><span data-stu-id="14d73-173">If the app uses SQL Server LocalDB and displays the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="14d73-174">Çözüm, bir komut isteminde `dotnet ef database update` ' i çalıştırmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="14d73-174">The solution may be to run `dotnet ef database update` at a command prompt.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="14d73-175">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="14d73-175">Additional resources</span></span>

* <span data-ttu-id="14d73-176">[Clı EF Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="14d73-176">[EF Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="14d73-177">Paket Yöneticisi Konsolu (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="14d73-177">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

## <a name="next-steps"></a><span data-ttu-id="14d73-178">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="14d73-178">Next steps</span></span>

<span data-ttu-id="14d73-179">Sonraki öğreticide, veri modeli, varlık özellikleri ve yeni varlıklar eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="14d73-179">The next tutorial builds out the data model, adding entity properties and new entities.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="14d73-180">[Önceki öğretici](xref:data/ef-rp/sort-filter-page)
> [sonraki öğretici](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="14d73-180">[Previous tutorial](xref:data/ef-rp/sort-filter-page)
[Next tutorial](xref:data/ef-rp/complex-data-model)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="14d73-181">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-181">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="14d73-182">Çözemediğiniz sorunlarla karşılaşırsanız, [Tamamlanmış uygulamayı](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)indirin.</span><span class="sxs-lookup"><span data-stu-id="14d73-182">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="14d73-183">Yeni bir uygulama geliştirildiğinde, veri modeli sıklıkla değişir.</span><span class="sxs-lookup"><span data-stu-id="14d73-183">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="14d73-184">Modelin her değiştirilişinde, model veritabanıyla eşitlenmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="14d73-184">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="14d73-185">Bu öğretici, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-185">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="14d73-186">Veri modelinin her değiştirilişinde:</span><span class="sxs-lookup"><span data-stu-id="14d73-186">Each time the data model changes:</span></span>

* <span data-ttu-id="14d73-187">DB bırakılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-187">The DB is dropped.</span></span>
* <span data-ttu-id="14d73-188">EF, modelle eşleşen yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-188">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="14d73-189">Uygulama, DB 'yi test verileriyle birlikte oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-189">The app seeds the DB with test data.</span></span>

<span data-ttu-id="14d73-190">Bu yaklaşım, VERITABANıNı veri modeliyle eşitlenmiş halde tutmak, uygulamayı üretime dağıtana kadar iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="14d73-190">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="14d73-191">Uygulama üretimde çalıştığında genellikle saklanması gereken verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="14d73-191">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="14d73-192">Uygulama her değişiklik yapıldığında (yeni sütun ekleme gibi) bir test DB ile başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-192">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="14d73-193">EF Core geçişleri özelliği, yeni bir VERITABANı oluşturmak yerine EF Core DB şemasını güncelleştirmesine olanak sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="14d73-193">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="14d73-194">Veri modeli değiştiğinde VERITABANıNı bırakıp yeniden oluşturmak yerine, geçişler şemayı güncelleştirir ve mevcut verileri korur.</span><span class="sxs-lookup"><span data-stu-id="14d73-194">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="14d73-195">Veritabanını bırak</span><span class="sxs-lookup"><span data-stu-id="14d73-195">Drop the database</span></span>

<span data-ttu-id="14d73-196">**SQL Server Nesne Gezgini** (ssox) veya `database drop` komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="14d73-196">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14d73-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14d73-197">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14d73-198">**Paket Yöneticisi konsolunda** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="14d73-198">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="14d73-199">Yardım bilgileri almak için PMC 'ten `Get-Help about_EntityFrameworkCore` ' i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="14d73-199">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14d73-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14d73-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="14d73-201">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="14d73-201">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="14d73-202">Proje klasörü *Startup.cs* dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="14d73-202">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="14d73-203">Komut penceresine şunu girin:</span><span class="sxs-lookup"><span data-stu-id="14d73-203">Enter the following in the command window:</span></span>

 ```dotnetcli
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="14d73-204">İlk geçiş oluşturma ve VERITABANıNı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="14d73-204">Create an initial migration and update the DB</span></span>

<span data-ttu-id="14d73-205">Projeyi derleyin ve ilk geçişi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="14d73-205">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14d73-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14d73-206">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14d73-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14d73-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="14d73-208">Yukarı ve aşağı yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="14d73-208">Examine the Up and Down methods</span></span>

<span data-ttu-id="14d73-209">EF Core `migrations add` komutu, DB oluşturmak için kod oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="14d73-209">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="14d73-210">Bu geçiş kodu *geçişleri @ no__t-1timestamp > _ınitialcreate. cs* dosyasında bulunur.</span><span class="sxs-lookup"><span data-stu-id="14d73-210">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="14d73-211">@No__t-1 sınıfının `Up` yöntemi, veri modeli varlık kümelerine karşılık gelen VERITABANı tablolarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-211">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="14d73-212">@No__t-0 yöntemi, aşağıdaki örnekte gösterildiği gibi bunları siler:</span><span class="sxs-lookup"><span data-stu-id="14d73-212">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="14d73-213">Geçişler, bir geçiş için veri modeli değişikliklerini uygulamak üzere `Up` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="14d73-213">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="14d73-214">Güncelleştirmeyi geri almak için bir komut girdiğinizde, geçişler `Down` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="14d73-214">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="14d73-215">Önceki kod ilk geçişe yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="14d73-215">The preceding code is for the initial migration.</span></span> <span data-ttu-id="14d73-216">Bu kod, `migrations add InitialCreate` komutu çalıştırıldığında oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="14d73-216">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="14d73-217">Dosya adı için geçiş adı parametresi (örnekteki "ınitialcreate") kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-217">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="14d73-218">Geçiş adı herhangi bir geçerli dosya adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="14d73-218">The migration name can be any valid file name.</span></span> <span data-ttu-id="14d73-219">Geçiş sırasında nelerin yapıldığını özetleyen bir sözcük veya tümcecik seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="14d73-219">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="14d73-220">Örneğin, bir departman tablosu ekleyen bir geçişe "AddDepartmentTable" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="14d73-220">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="14d73-221">İlk geçiş oluşturulur ve VERITABANı varsa:</span><span class="sxs-lookup"><span data-stu-id="14d73-221">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="14d73-222">DB oluşturma kodu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="14d73-222">The DB creation code is generated.</span></span>
* <span data-ttu-id="14d73-223">DB, veri modeliyle zaten eşleştiğinden, DB oluşturma kodunun çalıştırılması gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="14d73-223">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="14d73-224">DB oluşturma kodu çalıştırılsa, VERITABANı veri modeliyle zaten eşleştiğinden hiçbir değişiklik yapmaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-224">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="14d73-225">Uygulama yeni bir ortama dağıtıldığında, DB oluşturmak için DB oluşturma kodunun çalıştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="14d73-225">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="14d73-226">Daha önce VERITABANı bırakılmıştı ve mevcut olmadığından geçişler yeni DB 'yi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-226">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="14d73-227">Veri modeli anlık görüntüsü</span><span class="sxs-lookup"><span data-stu-id="14d73-227">The data model snapshot</span></span>

<span data-ttu-id="14d73-228">Geçişler *geçişlerde/SchoolContextModelSnapshot. cs*' de geçerli veritabanı şemasının *anlık görüntüsünü* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-228">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="14d73-229">Bir geçiş eklediğinizde EF, veri modeli Snapshot dosyası ile karşılaştırılarak nelerin değiştirildiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="14d73-229">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="14d73-230">Bir geçişi silmek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="14d73-230">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14d73-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14d73-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14d73-232">Geçişi Kaldır</span><span class="sxs-lookup"><span data-stu-id="14d73-232">Remove-Migration</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14d73-233">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14d73-233">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations remove
```

<span data-ttu-id="14d73-234">Daha fazla bilgi için bkz. [DotNet EF geçişleri kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="14d73-234">For more information, see [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="14d73-235">Geçişleri Kaldır komutu geçişi siler ve anlık görüntünün doğru şekilde sıfırlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="14d73-235">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="14d73-236">Uygulamayı kaldırın ve uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="14d73-236">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="14d73-237">Erken geliştirme için `EnsureCreated` kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="14d73-237">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="14d73-238">Bu öğreticide geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14d73-238">In this tutorial, migrations are used.</span></span> <span data-ttu-id="14d73-239">`EnsureCreated` aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="14d73-239">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="14d73-240">Geçişleri atlar ve DB ve şema oluşturur.</span><span class="sxs-lookup"><span data-stu-id="14d73-240">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="14d73-241">Geçişler tablosu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-241">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="14d73-242">Geçişlerle *kullanılamaz.*</span><span class="sxs-lookup"><span data-stu-id="14d73-242">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="14d73-243">, DB 'nin bıraktığı ve sıklıkla yeniden oluşturulduğu test veya hızlı prototipleme için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="14d73-243">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="14d73-244">@No__t kaldır-0:</span><span class="sxs-lookup"><span data-stu-id="14d73-244">Remove `EnsureCreated`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="14d73-245">Uygulamayı çalıştırın ve DB 'nin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="14d73-245">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="14d73-246">Veritabanını inceleyin</span><span class="sxs-lookup"><span data-stu-id="14d73-246">Inspect the database</span></span>

<span data-ttu-id="14d73-247">DB 'yi denetlemek için **SQL Server Nesne Gezgini** kullanın.</span><span class="sxs-lookup"><span data-stu-id="14d73-247">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="14d73-248">@No__t-0 tablosuna eklendiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="14d73-248">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="14d73-249">@No__t-0 tablosu, hangi geçişlerin VERITABANıNA uygulandığını izler.</span><span class="sxs-lookup"><span data-stu-id="14d73-249">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="14d73-250">@No__t-0 tablosundaki verileri görüntüleyin, ilk geçiş için bir satır gösterir.</span><span class="sxs-lookup"><span data-stu-id="14d73-250">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="14d73-251">Önceki CLı çıkış örneğinde yer alan son oturum, bu satırı oluşturan INSERT ifadesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="14d73-251">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="14d73-252">Uygulamayı çalıştırın ve her şeyin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="14d73-252">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="14d73-253">Üretimde geçişleri uygulama</span><span class="sxs-lookup"><span data-stu-id="14d73-253">Applying migrations in production</span></span>

<span data-ttu-id="14d73-254">Uygulama başlangıcında, üretim uygulamalarının [Database. Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) **çağrısını yapmanızı** öneririz.</span><span class="sxs-lookup"><span data-stu-id="14d73-254">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="14d73-255">`Migrate` sunucu grubundaki bir uygulamadan çağrılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="14d73-255">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="14d73-256">Örneğin, uygulama bulutu genişleme ile dağıtılmışsa (uygulamanın birden çok örneği çalışır).</span><span class="sxs-lookup"><span data-stu-id="14d73-256">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="14d73-257">Veritabanı geçişi, dağıtımın bir parçası olarak ve denetimli bir şekilde yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="14d73-257">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="14d73-258">Üretim veritabanı geçiş yaklaşımları şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="14d73-258">Production database migration approaches include:</span></span>

* <span data-ttu-id="14d73-259">SQL betikleri oluşturmak ve dağıtımda SQL betikleri kullanmak için geçişleri kullanma.</span><span class="sxs-lookup"><span data-stu-id="14d73-259">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="14d73-260">Denetlenen bir ortamdan `dotnet ef database update` çalıştırılıyor.</span><span class="sxs-lookup"><span data-stu-id="14d73-260">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="14d73-261">EF Core, herhangi bir geçişin çalıştırılması gerektiğini görmek için `__MigrationsHistory` tablosunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="14d73-261">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="14d73-262">DB güncel değilse, hiçbir geçiş çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="14d73-262">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="14d73-263">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="14d73-263">Troubleshooting</span></span>

<span data-ttu-id="14d73-264">[Tamamlanmış uygulamayı](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21snapshots/cu-part4-migrations)indirin.</span><span class="sxs-lookup"><span data-stu-id="14d73-264">Download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21snapshots/cu-part4-migrations).</span></span>

<span data-ttu-id="14d73-265">Uygulama aşağıdaki özel durumu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="14d73-265">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="14d73-266">Çözüm: Run `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="14d73-266">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="14d73-267">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="14d73-267">Additional resources</span></span>

* [<span data-ttu-id="14d73-268">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="14d73-268">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="14d73-269">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="14d73-269">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="14d73-270">Paket Yöneticisi Konsolu (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="14d73-270">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> <span data-ttu-id="14d73-271">[Önceki](xref:data/ef-rp/sort-filter-page)
> [İleri](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="14d73-271">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>

::: moniker-end

