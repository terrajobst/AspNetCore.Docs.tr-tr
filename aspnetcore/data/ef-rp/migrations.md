---
title: ASP.NET core'da - geçiş - 4 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, ASP.NET Core MVC uygulaması içindeki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 406b94d74a99dcec3b2ce01a5eb3313c48819148
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517009"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="83339-103">ASP.NET core'da - geçiş - 4 8 EF çekirdekli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="83339-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="83339-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83339-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="83339-105">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="83339-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="83339-106">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="83339-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="83339-107">Yeni bir uygulama geliştirdiğinizde, veri değişikliklerini sık model.</span><span class="sxs-lookup"><span data-stu-id="83339-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="83339-108">Her model veritabanı ile eşitlenmemiş model değişiklikleri alır.</span><span class="sxs-lookup"><span data-stu-id="83339-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="83339-109">Bu öğreticide, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="83339-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="83339-110">Veri değişiklikleri model her zaman:</span><span class="sxs-lookup"><span data-stu-id="83339-110">Each time the data model changes:</span></span>

* <span data-ttu-id="83339-111">DB bırakılır.</span><span class="sxs-lookup"><span data-stu-id="83339-111">The DB is dropped.</span></span>
* <span data-ttu-id="83339-112">EF modeli eşleşen yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83339-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="83339-113">Uygulamayı test verilerle bir veritabanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="83339-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="83339-114">DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım da uygulamayı üretime dağıtma kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="83339-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="83339-115">Uygulamayı üretim ortamında çalıştırırken, genellikle sürdürülmesi için gereken verileri depoladığı.</span><span class="sxs-lookup"><span data-stu-id="83339-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="83339-116">Uygulama, bir test (yeni bir sütun eklenmesi gibi) her değişiklik yapıldığında DB ile başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="83339-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="83339-117">EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine DB şemayı güncelleştirmenin EF Core sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="83339-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="83339-118">Bırakma ve değişiklikler veri modelini kullanırken DB yeniden yerine geçişleri şemasını güncelleştirir ve mevcut verileri korur.</span><span class="sxs-lookup"><span data-stu-id="83339-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="83339-119">Veritabanını bırakma</span><span class="sxs-lookup"><span data-stu-id="83339-119">Drop the database</span></span>

<span data-ttu-id="83339-120">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` komutu:</span><span class="sxs-lookup"><span data-stu-id="83339-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83339-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83339-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="83339-122">İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="83339-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="83339-123">Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerini almak için PMC'yi öğesinden.</span><span class="sxs-lookup"><span data-stu-id="83339-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83339-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="83339-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="83339-125">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="83339-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="83339-126">Proje klasörünü içeren *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="83339-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="83339-127">Komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="83339-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="83339-128">Bir başlangıç geçiş oluşturun ve DB update</span><span class="sxs-lookup"><span data-stu-id="83339-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="83339-129">Projeyi oluşturun ve ilk geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="83339-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83339-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83339-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83339-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="83339-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="83339-132">Yöntemleri aşağı ve yukarı inceleyin</span><span class="sxs-lookup"><span data-stu-id="83339-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="83339-133">EF Core `migrations add` bir veritabanı oluşturmak için oluşturulan komut kodu.</span><span class="sxs-lookup"><span data-stu-id="83339-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="83339-134">Bu geçiş kodu *geçişler\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="83339-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="83339-135">`Up` Yöntemi `InitialCreate` sınıfı, karşılık gelen bir veri modeli varlık kümeleri DB tablolar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83339-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="83339-136">`Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="83339-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="83339-137">Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="83339-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="83339-138">Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="83339-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="83339-139">Yukarıdaki kod, ilk geçiş için ' dir.</span><span class="sxs-lookup"><span data-stu-id="83339-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="83339-140">Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komut çalıştırıldı.</span><span class="sxs-lookup"><span data-stu-id="83339-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="83339-141">Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="83339-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="83339-142">Geçiş adı geçerli bir dosya adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="83339-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="83339-143">Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="83339-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="83339-144">Örneğin, departman tablosu eklenen bir geçiş "AddDepartmentTable." olarak adlandırılabilir</span><span class="sxs-lookup"><span data-stu-id="83339-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="83339-145">İlk geçiş oluşturulur ve bir veritabanı varsa:</span><span class="sxs-lookup"><span data-stu-id="83339-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="83339-146">DB oluşturma kod oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="83339-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="83339-147">DB oluşturma kod, bir veritabanı zaten veri modeli ile eşleştiği için çalıştırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="83339-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="83339-148">DB oluşturma kodu çalıştırırsanız, bir veritabanı zaten veri modeli ile eşleştiği için herhangi bir değişiklik yapmaz.</span><span class="sxs-lookup"><span data-stu-id="83339-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="83339-149">Uygulamanın yeni bir ortama dağıtıldığında, DB oluşumun kodunu bir veritabanı oluşturmak için çalıştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="83339-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="83339-150">Daha önce DB bırakıldı ve yeni bir veritabanı geçişleri oluşturur. böylece, mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="83339-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="83339-151">Veri modeli anlık görüntü</span><span class="sxs-lookup"><span data-stu-id="83339-151">The data model snapshot</span></span>

<span data-ttu-id="83339-152">Geçiş oluşturma bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="83339-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="83339-153">Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.</span><span class="sxs-lookup"><span data-stu-id="83339-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="83339-154">Bir geçiş silmek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="83339-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83339-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83339-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="83339-156">Remove-geçiş</span><span class="sxs-lookup"><span data-stu-id="83339-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83339-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="83339-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="83339-158">Daha fazla bilgi için [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="83339-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="83339-159">Remove geçişleri komutu geçiş siler ve anlık görüntünün doğru şekilde sıfırlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="83339-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="83339-160">EnsureCreated kaldırın ve uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="83339-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="83339-161">Erken geliştirme için `EnsureCreated` kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="83339-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="83339-162">Bu öğreticide, geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="83339-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="83339-163">`EnsureCreated` aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="83339-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="83339-164">Geçişleri atlar ve DB ve şema oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83339-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="83339-165">Bir geçiş tablo oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="83339-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="83339-166">İçin *değil* migrations ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="83339-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="83339-167">İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan test veya hızlı prototip oluşturma.</span><span class="sxs-lookup"><span data-stu-id="83339-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="83339-168">Kaldırma `EnsureCreated`:</span><span class="sxs-lookup"><span data-stu-id="83339-168">Remove `EnsureCreated`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="83339-169">Uygulamayı çalıştırın ve DB Çekirdek değeri oluşturulmuş doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="83339-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="83339-170">Veritabanı inceleyin</span><span class="sxs-lookup"><span data-stu-id="83339-170">Inspect the database</span></span>

<span data-ttu-id="83339-171">Kullanım **SQL Server Nesne Gezgini** DB incelemek için.</span><span class="sxs-lookup"><span data-stu-id="83339-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="83339-172">Ek fark bir `__EFMigrationsHistory` tablo.</span><span class="sxs-lookup"><span data-stu-id="83339-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="83339-173">`__EFMigrationsHistory` Tablo, hangi geçişler için DB uygulanmış olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="83339-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="83339-174">Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir.</span><span class="sxs-lookup"><span data-stu-id="83339-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="83339-175">Son günlük önceki CLI çıktı örnekte bu satırı oluşturur INSERT deyimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="83339-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="83339-176">Uygulamayı çalıştırın ve her şeyin çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="83339-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="83339-177">Üretim geçişleri uygulanıyor</span><span class="sxs-lookup"><span data-stu-id="83339-177">Applying migrations in production</span></span>

<span data-ttu-id="83339-178">Üretim uygulamaları gereken öneririz **değil** çağrı [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında.</span><span class="sxs-lookup"><span data-stu-id="83339-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="83339-179">`Migrate` sunucu grubundaki bir uygulamadan çağrılabilir olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="83339-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="83339-180">Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) ölçeklendirme ile dağıtılan bulut olmaması durumunda.</span><span class="sxs-lookup"><span data-stu-id="83339-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="83339-181">Veritabanı geçişi, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="83339-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="83339-182">Üretim veritabanı geçiş yaklaşımını şunlardır:</span><span class="sxs-lookup"><span data-stu-id="83339-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="83339-183">SQL komut dosyaları ve dağıtımda SQL betiklerini kullanarak oluşturmaya geçişleri kullanma.</span><span class="sxs-lookup"><span data-stu-id="83339-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="83339-184">Çalışan `dotnet ef database update` denetimli bir ortamda öğesinden.</span><span class="sxs-lookup"><span data-stu-id="83339-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="83339-185">EF Core kullanan `__MigrationsHistory` tüm geçişler çalıştırma gerekip gerekmediğini görmek için tabloyu.</span><span class="sxs-lookup"><span data-stu-id="83339-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="83339-186">DB güncel ise, hiçbir geçiş çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="83339-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="83339-187">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="83339-187">Troubleshooting</span></span>

<span data-ttu-id="83339-188">İndirme [tamamlanan uygulama](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="83339-188">Download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="83339-189">Uygulama, şu özel durum oluşturur:</span><span class="sxs-lookup"><span data-stu-id="83339-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="83339-190">Çözüm: `dotnet ef database update`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="83339-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="83339-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="83339-191">Additional resources</span></span>

* [<span data-ttu-id="83339-192">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="83339-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="83339-193">[.NET core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="83339-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="83339-194">Paket Yöneticisi Konsolu (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="83339-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> <span data-ttu-id="83339-195">[Önceki](xref:data/ef-rp/sort-filter-page)
> [İleri](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="83339-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
