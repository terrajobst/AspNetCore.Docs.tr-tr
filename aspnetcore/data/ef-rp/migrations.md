---
title: Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile
author: rick-anderson
description: Bu öğreticide, bir ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 15e3bc57e98b249cbefc394bbe1a136a709a03a7
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089964"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="35658-103">Razor sayfalarının ASP.NET Core - Migrations - 4 8'in EF çekirdek ile</span><span class="sxs-lookup"><span data-stu-id="35658-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="35658-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35658-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="35658-105">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35658-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="35658-106">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="35658-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="35658-107">Yeni bir uygulama geliştirilmiş, veri değişikliklerini sık model.</span><span class="sxs-lookup"><span data-stu-id="35658-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="35658-108">Her model değişiklikleri model veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="35658-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="35658-109">Bu öğretici yoksa veritabanı oluşturmak için Entity Framework yapılandırma tarafından başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="35658-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="35658-110">Her veri değişiklikleri model:</span><span class="sxs-lookup"><span data-stu-id="35658-110">Each time the data model changes:</span></span>

* <span data-ttu-id="35658-111">DB bırakılır.</span><span class="sxs-lookup"><span data-stu-id="35658-111">The DB is dropped.</span></span>
* <span data-ttu-id="35658-112">EF model eşleşen yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35658-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="35658-113">Uygulamayı test verilerle DB çekirdeğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35658-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="35658-114">DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım, iyi uygulamayı üretime dağıtmak istediğiniz kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="35658-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="35658-115">Uygulama üretimde çalıştırırken, genellikle sürdürülmesi için gereken veri saklama.</span><span class="sxs-lookup"><span data-stu-id="35658-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="35658-116">Uygulama, bir test (yeni bir sütun ekleme gibi) her değişiklik yapıldığında DB ile başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="35658-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="35658-117">EF çekirdek geçişler özelliği EF yeni bir veritabanı oluşturmak yerine DB şeması güncelleştirmek çekirdek sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="35658-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="35658-118">Bırakma ve değişiklikleri veri modelini kullanırken DB yeniden oluşturma, yerine geçişler şema güncelleştirir ve mevcut verileri korur.</span><span class="sxs-lookup"><span data-stu-id="35658-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="35658-119">Veritabanı bırakma</span><span class="sxs-lookup"><span data-stu-id="35658-119">Drop the database</span></span>

<span data-ttu-id="35658-120">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` komutu:</span><span class="sxs-lookup"><span data-stu-id="35658-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35658-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35658-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="35658-122">İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="35658-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="35658-123">Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerine ulaşmak için PMC gelen.</span><span class="sxs-lookup"><span data-stu-id="35658-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="35658-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="35658-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="35658-125">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="35658-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="35658-126">Proje klasörünü içeren *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="35658-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="35658-127">Komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="35658-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="35658-128">İlk geçiş oluşturun ve DB güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="35658-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="35658-129">Projeyi derlemek ve ilk geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35658-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35658-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35658-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="35658-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="35658-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="35658-132">Yukarı inceleyin ve yöntemleri aşağı</span><span class="sxs-lookup"><span data-stu-id="35658-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="35658-133">EF çekirdek `migrations add` DB oluşturmak için oluşturulan komut kodu.</span><span class="sxs-lookup"><span data-stu-id="35658-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="35658-134">Bu geçiş kod *geçişler\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="35658-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="35658-135">`Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen DB tablolar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35658-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="35658-136">`Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="35658-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="35658-137">Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="35658-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="35658-138">Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="35658-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="35658-139">Önceki kod için ilk geçiş değil.</span><span class="sxs-lookup"><span data-stu-id="35658-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="35658-140">Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komutu çalıştırıldı.</span><span class="sxs-lookup"><span data-stu-id="35658-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="35658-141">Geçiş adı parametresi (örneğin, "InitialCreate") için dosya adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35658-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="35658-142">Geçiş adı geçerli bir dosya adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="35658-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="35658-143">Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="35658-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="35658-144">Örneğin, bir departman tablosu eklenen bir geçiş "AddDepartmentTable." adlı</span><span class="sxs-lookup"><span data-stu-id="35658-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="35658-145">İlk geçiş oluşturulur ve DB varsa:</span><span class="sxs-lookup"><span data-stu-id="35658-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="35658-146">DB oluşturma kodu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35658-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="35658-147">DB oluşturma kodu DB veri modeli eşleştiğinden çalıştırmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="35658-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="35658-148">DB oluşturma kodu çalıştırırsanız, DB veri modeli eşleştiğinden herhangi bir değişiklik yapmaz.</span><span class="sxs-lookup"><span data-stu-id="35658-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="35658-149">Uygulama için yeni bir ortam dağıtıldığında DB oluşturma kod DB oluşturmak için çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35658-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="35658-150">Daha önce DB bırakıldı ve geçişleri oluşturur şekilde yeni DB, mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="35658-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="35658-151">Veri modeli anlık görüntü</span><span class="sxs-lookup"><span data-stu-id="35658-151">The data model snapshot</span></span>

<span data-ttu-id="35658-152">Geçişler oluşturma bir *anlık görüntü* geçerli veritabanı şemasının *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="35658-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="35658-153">Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF belirler.</span><span class="sxs-lookup"><span data-stu-id="35658-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="35658-154">Bir geçiş silmek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="35658-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35658-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35658-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="35658-156">Remove-geçiş</span><span class="sxs-lookup"><span data-stu-id="35658-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="35658-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="35658-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="35658-158">Daha fazla bilgi için bkz: [dotnet ef geçişler kaldırmak](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="35658-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

------

<span data-ttu-id="35658-159">Kaldır geçişler komut geçiş siler ve anlık görüntü doğru sıfırlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="35658-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="35658-160">EnsureCreated kaldırın ve uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="35658-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="35658-161">Erken geliştirme `EnsureCreated` kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="35658-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="35658-162">Bu öğreticide, geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35658-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="35658-163">`EnsureCreated` aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="35658-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="35658-164">Geçişler atlar ve şeması ve DB oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35658-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="35658-165">Geçiş tablosu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="35658-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="35658-166">Yapabilirsiniz *değil* geçişler ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="35658-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="35658-167">İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan sınama ya da hızlı prototipi oluşturulurken.</span><span class="sxs-lookup"><span data-stu-id="35658-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="35658-168">Aşağıdaki satırı Kaldır `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="35658-168">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="35658-169">Uygulamayı çalıştırın ve DB sağlanmış doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="35658-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="35658-170">Veritabanı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="35658-170">Inspect the database</span></span>

<span data-ttu-id="35658-171">Kullanım **SQL Server Nesne Gezgini** DB incelemek için.</span><span class="sxs-lookup"><span data-stu-id="35658-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="35658-172">Eklenmesi fark bir `__EFMigrationsHistory` tablo.</span><span class="sxs-lookup"><span data-stu-id="35658-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="35658-173">`__EFMigrationsHistory` Hangi geçişleri Veritabanına uygulanmış olan tablo izler.</span><span class="sxs-lookup"><span data-stu-id="35658-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="35658-174">Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir.</span><span class="sxs-lookup"><span data-stu-id="35658-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="35658-175">Son günlük önceki CLI çıkış örnekte bu satırı oluşturur INSERT deyiminin gösterir.</span><span class="sxs-lookup"><span data-stu-id="35658-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="35658-176">Uygulamayı çalıştırın ve her şeyi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="35658-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="35658-177">Üretim geçişleri uygulama</span><span class="sxs-lookup"><span data-stu-id="35658-177">Applying migrations in production</span></span>

<span data-ttu-id="35658-178">Üretim uygulamaları gereken öneririz **değil** çağrısı [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında.</span><span class="sxs-lookup"><span data-stu-id="35658-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="35658-179">`Migrate` sunucu grubundaki bir uygulamadan çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35658-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="35658-180">Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) genişleme ile dağıtılan bulut olması durumunda.</span><span class="sxs-lookup"><span data-stu-id="35658-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="35658-181">Veritabanı geçiş, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="35658-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="35658-182">Üretim veritabanı geçiş yaklaşımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="35658-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="35658-183">SQL komut dosyaları oluşturmak için geçişleri kullanmaya ve dağıtımda SQL komut dosyalarını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="35658-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="35658-184">Çalışan `dotnet ef database update` denetimli ortamından.</span><span class="sxs-lookup"><span data-stu-id="35658-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="35658-185">EF çekirdek kullanan `__MigrationsHistory` tüm geçişler çalıştırmak gerekip gerekmediğini görmek için tablo.</span><span class="sxs-lookup"><span data-stu-id="35658-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="35658-186">DB güncel ise, herhangi bir geçiş çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="35658-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="35658-187">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="35658-187">Troubleshooting</span></span>

<span data-ttu-id="35658-188">Karşıdan [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="35658-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="35658-189">Uygulama şu özel durum oluşturur:</span><span class="sxs-lookup"><span data-stu-id="35658-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="35658-190">Çözüm: Çalıştır `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="35658-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="35658-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="35658-191">Additional resources</span></span>

* <span data-ttu-id="35658-192">[.NET core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="35658-192">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="35658-193">Paket Yöneticisi Konsolu (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="35658-193">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="35658-194">[Önceki](xref:data/ef-rp/sort-filter-page)
> [sonraki](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="35658-194">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>