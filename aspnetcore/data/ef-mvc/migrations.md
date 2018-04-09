---
title: ASP.NET Core MVC EF temel - Migrations - 4 10
author: tdykstra
description: Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: f3f14d6dab1eb03e0ead5edaa9d7ba41a10b21e9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="04d09-103">ASP.NET Core MVC EF temel - Migrations - 4 10</span><span class="sxs-lookup"><span data-stu-id="04d09-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

<span data-ttu-id="04d09-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="04d09-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="04d09-105">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="04d09-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="04d09-106">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="04d09-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="04d09-107">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF çekirdek geçişler özelliği kullanmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="04d09-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="04d09-108">Sonraki öğreticileri, veri modelini değiştirme gibi daha fazla geçişler ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="04d09-109">Geçişler giriş</span><span class="sxs-lookup"><span data-stu-id="04d09-109">Introduction to migrations</span></span>

<span data-ttu-id="04d09-110">Yeni bir uygulama geliştirirken, veri modelinizi model değişikliklerini sık sık ve her zaman değiştirir, veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="04d09-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="04d09-111">Bu öğreticiler yoksa veritabanı oluşturmak için Entity Framework yapılandırarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="04d09-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="04d09-112">Ardından, veri modelini değiştirin--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınız--değiştirmek her zaman veritabanını silin ve EF modelle ve test verilerle çekirdeğini oluşturur Yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04d09-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="04d09-113">Veritabanı veri modeli ile eşitlenmiş tutmak bu yöntem, üretim uygulamayı dağıtmak iyi kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="04d09-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="04d09-114">Genellikle, korumak istediğiniz her şeyi her zaman kaybetmek istemediğiniz verilerini depolamak üretimde uygulama çalışırken yeni bir sütun ekleme gibi bir değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="04d09-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="04d09-115">EF çekirdek geçişler özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="04d09-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="04d09-116">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="04d09-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="04d09-117">Geçişler ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).</span><span class="sxs-lookup"><span data-stu-id="04d09-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="04d09-118">Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="04d09-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="04d09-119">Bilgilerine PMC hakkında [Bu öğreticide sonuna](#pmc).</span><span class="sxs-lookup"><span data-stu-id="04d09-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="04d09-120">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="04d09-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="04d09-121">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* gösterildiği gibi dosya.</span><span class="sxs-lookup"><span data-stu-id="04d09-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="04d09-122">**Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="04d09-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="04d09-123">Düzenleyebileceğiniz *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** ve seçerek **Düzenle ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="04d09-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="04d09-124">(Bu örnekte sürüm numaralarını öğretici yazıldıktan sonra geçerli.)</span><span class="sxs-lookup"><span data-stu-id="04d09-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="04d09-125">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="04d09-125">Change the connection string</span></span>

<span data-ttu-id="04d09-126">İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullandığınız parolalardan başka bir adı bağlantı dizesinde veritabanı adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="04d09-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="04d09-127">Bu değişikliğin yapılması projeyi ayarlar, böylece ilk geçiş yeni bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04d09-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="04d09-128">Bu geçiş ile çalışmaya başlamak için gerekli değildir, ancak iyi bir fikirdir neden, daha sonra göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="04d09-129">Veritabanı adının değiştirilmesi alternatif olarak, veritabanını silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="04d09-130">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutu:</span><span class="sxs-lookup"><span data-stu-id="04d09-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="04d09-131">Aşağıdaki bölümde, CLI komutları çalıştırmak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="04d09-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="04d09-132">İlk geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="04d09-132">Create an initial migration</span></span>

<span data-ttu-id="04d09-133">Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="04d09-133">Save your changes and build the project.</span></span> <span data-ttu-id="04d09-134">Ardından bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="04d09-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="04d09-135">Bunu yapmak için hızlı bir yolu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="04d09-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="04d09-136">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **dosya Gezgini'nde Aç** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="04d09-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="04d09-138">Adres çubuğunda "cmd" girin ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="04d09-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

<span data-ttu-id="04d09-140">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="04d09-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="04d09-141">Komut penceresinde aşağıdaki gibi bir çıktı bakın:</span><span class="sxs-lookup"><span data-stu-id="04d09-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="04d09-142">Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komutu "dotnet-ef"*, bkz: [bu blog gönderisine](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.</span><span class="sxs-lookup"><span data-stu-id="04d09-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="04d09-143">Bir hata iletisi görürseniz "*... dosyasına erişemiyor ContosoUniversity.dll çünkü başka bir işlem tarafından kullanılıyor.* ", IIS Express simgesini Windows sistem tepsisi bulmak ve sağ tıklatın ve ardından **ContosoUniversity > Stop Site**.</span><span class="sxs-lookup"><span data-stu-id="04d09-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="04d09-144">Yukarı inceleyin ve yöntemleri aşağı</span><span class="sxs-lookup"><span data-stu-id="04d09-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="04d09-145">Yürütülen zaman `migrations add` komutunu EF oluşturulan veritabanı sıfırdan oluşturacak kodu.</span><span class="sxs-lookup"><span data-stu-id="04d09-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="04d09-146">Bu kod *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="04d09-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="04d09-147">`Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="04d09-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="04d09-148">Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="04d09-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="04d09-149">Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="04d09-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="04d09-150">Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu.</span><span class="sxs-lookup"><span data-stu-id="04d09-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="04d09-151">Geçiş name parametresi (örneğin, "InitialCreate") için dosya adı kullanılır ve istediğiniz olabilir.</span><span class="sxs-lookup"><span data-stu-id="04d09-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="04d09-152">Bir sözcük veya tümcecik geçişte yapıldığını özetler seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="04d09-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="04d09-153">Örneğin, bir sonraki geçiş "AddDepartmentTable" olarak adlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="04d09-154">Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak veritabanı veri modeli eşleştiğinden çalıştırmak sahip değil.</span><span class="sxs-lookup"><span data-stu-id="04d09-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="04d09-155">Burada veritabanı yok henüz, veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamayı dağıttığınızda, dolayısıyla ilk test etmek için iyi bir fikir taşır.</span><span class="sxs-lookup"><span data-stu-id="04d09-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="04d09-156">İşte bu nedenle geçişler sıfırdan yeni bir tane oluşturabilmesi için daha önce--bağlantı dizesinde veritabanının adı değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="04d09-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="04d09-157">Veri modeli anlık görüntü</span><span class="sxs-lookup"><span data-stu-id="04d09-157">The data model snapshot</span></span>

<span data-ttu-id="04d09-158">Geçişler oluşturur bir *anlık görüntü* geçerli veritabanı şemasının *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="04d09-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="04d09-159">Bir geçiş eklediğinizde, anlık görüntü dosyası veri modeline karşılaştırarak değişiklikler EF belirler.</span><span class="sxs-lookup"><span data-stu-id="04d09-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="04d09-160">Bir geçiş silerken kullanmak [dotnet ef geçişler kaldırmak](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.</span><span class="sxs-lookup"><span data-stu-id="04d09-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="04d09-161">`dotnet ef migrations remove` geçiş siler ve anlık görüntü doğru sıfırlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="04d09-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="04d09-162">Bkz: [EF çekirdek geçişler takım ortamlarda](/ef/core/managing-schemas/migrations/teams) anlık görüntü dosyasının nasıl kullanıldığı hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="04d09-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="04d09-163">Veritabanına geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="04d09-163">Apply the migration to the database</span></span>

<span data-ttu-id="04d09-164">Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.</span><span class="sxs-lookup"><span data-stu-id="04d09-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="04d09-165">Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanını ayarlanan komutlar için günlüklerine bakın.</span><span class="sxs-lookup"><span data-stu-id="04d09-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="04d09-166">Aşağıdaki örnek çıktıda günlükleri çoğunu göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="04d09-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="04d09-167">Bu günlük iletilerini ayrıntı düzeyi görmek tercih ederseniz, günlük düzeyini değiştirebilirsiniz *appsettings. Development.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="04d09-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="04d09-168">Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="04d09-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="04d09-169">Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.</span><span class="sxs-lookup"><span data-stu-id="04d09-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="04d09-170">Hangi geçişleri veritabanına uygulanmış izler __EFMigrationsHistory tablo eklenmesi fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="04d09-171">Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="04d09-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="04d09-172">(Önceki CLI çıkış örnekte son günlüğü bu satırı oluşturur INSERT deyiminin gösterir.)</span><span class="sxs-lookup"><span data-stu-id="04d09-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="04d09-173">Her şeyi hala aynı önce çalışır durumda olduğunu doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="04d09-173">Run the application to verify that everything still works the same as before.</span></span>

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="04d09-175">Komut satırı arabirimi (CLI) vs. Paket Yöneticisi Konsolu (PMC)</span><span class="sxs-lookup"><span data-stu-id="04d09-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="04d09-176">Bunun için geçişler yönetme .NET Core CLI komutları veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="04d09-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="04d09-177">Bu öğretici CLI kullanmayı gösterir, ancak isterseniz PMC kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="04d09-178">EF komutlar PMC komutları için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket.</span><span class="sxs-lookup"><span data-stu-id="04d09-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="04d09-179">Bu paket zaten bulunduğundan [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) yüklemek zorunda kalmamak için metapackage.</span><span class="sxs-lookup"><span data-stu-id="04d09-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="04d09-180">**Önemli:** bu düzenleyerek için CLI yükleme biri aynı pakette değil *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="04d09-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="04d09-181">Bu ada bitiyor `Tools`, biten CLI paket adı aksine `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="04d09-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="04d09-182">CLI komutları hakkında daha fazla bilgi için bkz: [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="04d09-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="04d09-183">PMC komutları hakkında daha fazla bilgi için bkz: [Paket Yöneticisi Konsolu (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="04d09-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="04d09-184">Özet</span><span class="sxs-lookup"><span data-stu-id="04d09-184">Summary</span></span>

<span data-ttu-id="04d09-185">Bu öğreticide, oluşturma ve ilk geçişinizi uygulama öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="04d09-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="04d09-186">Sonraki öğreticide, veri modelini genişleterek daha gelişmiş konuları arayan başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="04d09-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="04d09-187">Yol boyunca oluşturun ve ek geçişleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="04d09-187">Along the way you'll create and apply additional migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04d09-188">[Önceki](sort-filter-page.md)
> [sonraki](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="04d09-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
