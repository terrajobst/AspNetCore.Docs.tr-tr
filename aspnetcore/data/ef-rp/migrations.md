---
title: "Razor sayfalarının EF temel - Migrations - 4 8"
author: rick-anderson
description: "Bu öğreticide, bir ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="1e2ae-103">Geçişler - Razor sayfalarının Öğreticisi (4. 8'in) EF çekirdek</span><span class="sxs-lookup"><span data-stu-id="1e2ae-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="1e2ae-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e2ae-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1e2ae-105">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="1e2ae-106">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1e2ae-107">Yeni bir uygulama geliştirilmiş, veri değişikliklerini sık model.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="1e2ae-108">Her model değişiklikleri model veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="1e2ae-109">Bu öğretici yoksa veritabanı oluşturmak için Entity Framework yapılandırma tarafından başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1e2ae-110">Her veri değişiklikleri model:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-110">Each time the data model changes:</span></span>

* <span data-ttu-id="1e2ae-111">DB bırakılır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-111">The DB is dropped.</span></span>
* <span data-ttu-id="1e2ae-112">EF model eşleşen yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="1e2ae-113">Uygulamayı test verilerle DB çekirdeğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="1e2ae-114">DB veri modeli ile eşitlenmiş tutmak için bu yaklaşım, iyi uygulamayı üretime dağıtmak istediğiniz kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="1e2ae-115">Uygulama üretimde çalıştırırken, genellikle sürdürülmesi için gereken veri saklama.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="1e2ae-116">Uygulama, bir test (yeni bir sütun ekleme gibi) her değişiklik yapıldığında DB ile başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="1e2ae-117">EF çekirdek geçişler özelliği EF yeni bir veritabanı oluşturmak yerine DB şeması güncelleştirmek çekirdek sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="1e2ae-118">Bırakma ve değişiklikleri veri modelini kullanırken DB yeniden oluşturma, yerine geçişler şema güncelleştirir ve mevcut verileri korur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="1e2ae-119">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="1e2ae-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="1e2ae-120">Geçişler ile çalışmak için kullanın **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="1e2ae-121">Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="1e2ae-122">Bilgilerine PMC hakkında [Bu öğreticide sonuna](#pmc).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="1e2ae-123">EF çekirdek Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="1e2ae-124">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* gösterildiği gibi dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="1e2ae-125">**Not:** düzenleyerek bu paketi yüklenmelidir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="1e2ae-126">`install-package` Bu paketi yüklemek için komut veya Paket Yöneticisi GUI kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="1e2ae-127">Düzen *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** ve seçerek **Düzenle ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="1e2ae-128">Aşağıdaki biçimlendirmede güncelleştirilmiş gösterir *.csproj* vurgulanmış EF çekirdek CLI araçlarını dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="1e2ae-129">Önceki örnekte sürüm numaralarını öğretici yazıldıktan sonra geçerli.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="1e2ae-130">Aynı sürüm diğer paketlerinde bulunan EF çekirdek CLI araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="1e2ae-131">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="1e2ae-131">Change the connection string</span></span>

<span data-ttu-id="1e2ae-132">İçinde *appsettings.json* dosya, ContosoUniversity2 için bağlantı dizesi DB'de adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="1e2ae-133">Bağlantı dizesindeki DB adının değiştirilmesi, yeni bir veritabanı oluşturmak ilk geçiş neden olur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="1e2ae-134">Bu ada sahip bir mevcut olmadığından yeni bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="1e2ae-135">Bağlantı dizesi değiştirme geçişler ile çalışmaya başlama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="1e2ae-136">DB adını değiştirmek için bir alternatif DB siliyor.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="1e2ae-137">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutu:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="1e2ae-138">Aşağıdaki bölümde, CLI komutları çalıştırmak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="1e2ae-139">İlk geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="1e2ae-139">Create an initial migration</span></span>

<span data-ttu-id="1e2ae-140">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-140">Build the project.</span></span>

<span data-ttu-id="1e2ae-141">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1e2ae-142">Proje klasörünü içeren *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="1e2ae-143">Komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="1e2ae-144">Komut penceresinde aşağıdakine benzer bilgiler görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="1e2ae-145">Geçiş iletisiyle başarısız olursa "*... dosyasına erişemiyor ContosoUniversity.dll çünkü başka bir işlem tarafından kullanılıyor.* "</span><span class="sxs-lookup"><span data-stu-id="1e2ae-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="1e2ae-146">görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-146">is displayed:</span></span>

* <span data-ttu-id="1e2ae-147">Stop IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="1e2ae-148">Çıkmak ve Visual Studio'yu yeniden başlatın veya</span><span class="sxs-lookup"><span data-stu-id="1e2ae-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="1e2ae-149">IIS Express simgesini Windows Sistem tepsisinde bulun.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="1e2ae-150">IIS Express simgesine sağ tıklayın ve ardından **ContosoUniversity > Durdur Site**.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="1e2ae-151">Hata iletisi "yapılandırma başarısızsa."</span><span class="sxs-lookup"><span data-stu-id="1e2ae-151">If the error message "Build failed."</span></span> <span data-ttu-id="1e2ae-152">, komutu yeniden çalıştırın görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-152">is displayed, run the command again.</span></span> <span data-ttu-id="1e2ae-153">Bu hata alırsanız, bu öğreticinin sonunda not bırakın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1e2ae-154">Yukarı inceleyin ve yöntemleri aşağı</span><span class="sxs-lookup"><span data-stu-id="1e2ae-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="1e2ae-155">EF çekirdek komutu `migrations add` DB'den oluşturmak için kodu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="1e2ae-156">Bu geçiş kod *geçişler\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1e2ae-157">`Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen DB tablolar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="1e2ae-158">`Down` Yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="1e2ae-159">Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1e2ae-160">Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="1e2ae-161">Önceki kod için ilk geçiş değil.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="1e2ae-162">Kodun ne zaman oluşturulduğu `migrations add InitialCreate` komutu çalıştırıldı.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="1e2ae-163">Geçiş adı parametresi (örneğin, "InitialCreate") için dosya adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="1e2ae-164">Geçiş adı geçerli bir dosya adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="1e2ae-165">Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1e2ae-166">Örneğin, bir departman tablosu eklenen bir geçiş "AddDepartmentTable." adlı</span><span class="sxs-lookup"><span data-stu-id="1e2ae-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="1e2ae-167">İlk geçiş oluşturduysanız ve DB çıkar:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="1e2ae-168">DB oluşturma kodu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="1e2ae-169">DB oluşturma kodu DB veri modeli eşleştiğinden çalıştırmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="1e2ae-170">DB oluşturma kodu çalıştırırsanız, DB veri modeli eşleştiğinden herhangi bir değişiklik yapmaz.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="1e2ae-171">Uygulama için yeni bir ortam dağıtıldığında DB oluşturma kod DB oluşturmak için çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="1e2ae-172">Daha önce bağlantı dizesi DB için yeni bir ad kullanmak üzere değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="1e2ae-173">Belirtilen veritabanı yok, bu geçişler oluşturur şekilde DB.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="1e2ae-174">Veri modeli anlık görüntü inceleyin</span><span class="sxs-lookup"><span data-stu-id="1e2ae-174">Examine the data model snapshot</span></span>

<span data-ttu-id="1e2ae-175">Geçişler oluşturur bir *anlık görüntü* geçerli DB şemasının *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="1e2ae-176">Geçerli DB şeması kodda gösterilir çünkü EF çekirdek geçişler oluşturmak için DB ile etkileşim kurmak sahip değil.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="1e2ae-177">Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF çekirdek belirler.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="1e2ae-178">Yalnızca DB güncelleştirmek sahip olduğu EF çekirdek DB ile etkileşime girer.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="1e2ae-179">Anlık görüntü dosyasının oluşturulduğu geçişler ile eşitlenmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="1e2ae-180">Bir geçiş adlı dosyayı silerek kaldırılamaz  *\<zaman damgası > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="1e2ae-181">Bu dosya silinirse, kalan geçişler DB anlık görüntü dosyası ile eşitlenmemiş.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="1e2ae-182">Eklenen son geçiş silmek için kullanın [dotnet ef geçişler kaldırmak](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="1e2ae-183">Remove EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="1e2ae-183">Remove EnsureCreated</span></span>

<span data-ttu-id="1e2ae-184">Erken geliştirme `EnsureCreated` komutu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="1e2ae-185">Bu öğreticide, geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="1e2ae-186">`EnsureCreated`aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-186">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="1e2ae-187">Geçişler atlar ve şeması ve DB oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="1e2ae-188">Geçiş tablosu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-188">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="1e2ae-189">Yapabilirsiniz *değil* geçişler ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="1e2ae-190">İçin tasarlanmış burada DB bırakılan ve sık sık yeniden oluşturulan sınama ya da hızlı prototipi oluşturulurken.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="1e2ae-191">Aşağıdaki satırı Kaldır `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="1e2ae-192">Geliştirme DB'de geçiş uygulamak</span><span class="sxs-lookup"><span data-stu-id="1e2ae-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="1e2ae-193">Komut penceresinde DB ve tablolar oluşturmak için aşağıdakileri girin.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="1e2ae-194">Not: Varsa `update` komut, "oluşturma başarısız oldu." hatasını döndürür:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="1e2ae-195">Komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-195">Run the command again.</span></span>
* <span data-ttu-id="1e2ae-196">Yine başarısız olursa, Visual Studio'dan çıkın ve ardından çalıştırın `update` komutu.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="1e2ae-197">Sayfanın altındaki bir ileti bırakın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="1e2ae-198">Komut çıktısı benzer `migrations add` komut çıktı.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="1e2ae-199">Önceki komutta DB'yi yedekleyin ayarlamak SQL komutlarını günlüklerinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="1e2ae-200">Aşağıdaki örnek çıktıda günlükleri çoğunu göz ardı edilir:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-200">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="1e2ae-201">Günlük iletileri, ayrıntı düzeyi azaltmak için günlük düzeyleri değiştirebilirsiniz *appsettings. Development.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="1e2ae-202">Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="1e2ae-203">Kullanım **SQL Server Nesne Gezgini** DB incelemek için.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="1e2ae-204">Eklenmesi fark bir `__EFMigrationsHistory` tablo.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="1e2ae-205">`__EFMigrationsHistory` Hangi geçişleri Veritabanına uygulanmış olan tablo izler.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="1e2ae-206">Verileri görüntüleme `__EFMigrationsHistory` tablo, ilk geçiş için bir satır gösterir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="1e2ae-207">Son günlük önceki CLI çıkış örnekte bu satırı oluşturur INSERT deyiminin gösterir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="1e2ae-208">Uygulamayı çalıştırın ve her şeyi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="1e2ae-209">Üretim appling geçişleri</span><span class="sxs-lookup"><span data-stu-id="1e2ae-209">Appling migrations in production</span></span>

<span data-ttu-id="1e2ae-210">Üretim uygulamaları gereken öneririz **değil** çağrısı [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) uygulama başlangıcında.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="1e2ae-211">`Migrate`sunucu grubundaki bir uygulamadan çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-211">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="1e2ae-212">Örneğin, uygulama (uygulama birden çok örneğini çalıştıran) genişleme ile dağıtılan bulut olması durumunda.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="1e2ae-213">Veritabanı geçiş, dağıtım ve denetimli bir şekilde bir parçası olarak yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="1e2ae-214">Üretim veritabanı geçiş yaklaşımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="1e2ae-215">SQL komut dosyaları oluşturmak için geçişleri kullanmaya ve dağıtımda SQL komut dosyalarını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="1e2ae-216">Çalışan `dotnet ef database update` denetimli ortamından.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="1e2ae-217">EF çekirdek kullanan `__MigrationsHistory` tüm geçişler çalıştırmak gerekip gerekmediğini görmek için tablo.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="1e2ae-218">DB güncel ise, herhangi bir geçiş çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="1e2ae-219">Komut satırı arabirimi (CLI) vs. Paket Yöneticisi Konsolu (PMC)</span><span class="sxs-lookup"><span data-stu-id="1e2ae-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="1e2ae-220">EF geçişler yönetmek için tooling çekirdek kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="1e2ae-221">.NET core CLI komutları.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="1e2ae-222">Visual Studio'da PowerShell cmdlet'leri **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="1e2ae-223">Bu öğretici CLI kullanmayı gösterir, bazı geliştiriciler PMC kullanmayı tercih.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="1e2ae-224">PMC EF çekirdek komutlarında bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="1e2ae-225">Bu paket dahil [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) yüklemek zorunda kalmamak için metapackage.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="1e2ae-226">**Önemli:** bu düzenleyerek için CLI yükleme biri aynı pakette değil *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-226">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="1e2ae-227">Bu ada bitiyor `Tools`, biten CLI paket adı aksine `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="1e2ae-228">CLI komutları hakkında daha fazla bilgi için bkz: [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="1e2ae-229">PMC komutları hakkında daha fazla bilgi için bkz: [Paket Yöneticisi Konsolu (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1e2ae-230">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="1e2ae-230">Troubleshooting</span></span>

<span data-ttu-id="1e2ae-231">Karşıdan [Bu aşama için tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1e2ae-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1e2ae-232">Uygulama şu özel durum oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="1e2ae-233">Çözüm: Çalıştır`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="1e2ae-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="1e2ae-234">Varsa `update` komut, "oluşturma başarısız oldu." hatasını döndürür:</span><span class="sxs-lookup"><span data-stu-id="1e2ae-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="1e2ae-235">Komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-235">Run the command again.</span></span>
* <span data-ttu-id="1e2ae-236">Sayfanın altındaki bir ileti bırakın.</span><span class="sxs-lookup"><span data-stu-id="1e2ae-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1e2ae-237">[Önceki](xref:data/ef-rp/sort-filter-page)
[sonraki](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="1e2ae-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
