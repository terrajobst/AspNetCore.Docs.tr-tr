---
title: 'Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma'
description: Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 3d2ae12bf8eda4f7997008758d4d29434a8371a7
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012610"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a><span data-ttu-id="53fb8-103">Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma</span><span class="sxs-lookup"><span data-stu-id="53fb8-103">Tutorial: Using the migrations feature - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="53fb8-104">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-104">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="53fb8-105">Sonraki öğreticilerde, veri modeli değiştikçe daha fazla geçişleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-105">In later tutorials, you'll add more migrations as you change the data model.</span></span>

<span data-ttu-id="53fb8-106">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="53fb8-106">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53fb8-107">Geçiş hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="53fb8-107">Learn about migrations</span></span>
> * <span data-ttu-id="53fb8-108">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="53fb8-108">Change the connection string</span></span>
> * <span data-ttu-id="53fb8-109">Bir başlangıç geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="53fb8-109">Create an initial migration</span></span>
> * <span data-ttu-id="53fb8-110">Yukarı ve aşağı yöntemleri inceleyin</span><span class="sxs-lookup"><span data-stu-id="53fb8-110">Examine Up and Down methods</span></span>
> * <span data-ttu-id="53fb8-111">Veri modeli anlık görüntü hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="53fb8-111">Learn about the data model snapshot</span></span>
> * <span data-ttu-id="53fb8-112">Geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="53fb8-112">Apply the migration</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53fb8-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="53fb8-113">Prerequisites</span></span>

* [<span data-ttu-id="53fb8-114">Sıralama, filtreleme ve sayfalama</span><span class="sxs-lookup"><span data-stu-id="53fb8-114">Sorting, filtering, and paging</span></span>](sort-filter-page.md)

## <a name="about-migrations"></a><span data-ttu-id="53fb8-115">Geçiş hakkında</span><span class="sxs-lookup"><span data-stu-id="53fb8-115">About migrations</span></span>

<span data-ttu-id="53fb8-116">Yeni bir uygulama geliştirdiğinizde, model değişiklikleri sık ve her zaman veri modelinizi değiştirir, veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="53fb8-116">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="53fb8-117">Bu öğretici, yoksa veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="53fb8-117">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="53fb8-118">Ardından, veri modeli değiştirmek--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınıza--değiştirmek, her zaman veritabanını silebilir ve EF modeli eşleşir ve test verileri ile çekirdeğini yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="53fb8-118">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="53fb8-119">Veritabanı veri modeli ile eşitlenmiş tutmak için bu yöntemi de uygulamayı üretim ortamına dağıtmadan kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="53fb8-119">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="53fb8-120">Üretim ortamında genellikle tutmak istediğiniz ve her şey her zaman kaybetmek istemediğiniz veri depolama uygulama çalıştırılırken, yeni bir sütun ekleme gibi bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-120">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="53fb8-121">EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="53fb8-121">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

<span data-ttu-id="53fb8-122">Migrations ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).</span><span class="sxs-lookup"><span data-stu-id="53fb8-122">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="53fb8-123">Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-123">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="53fb8-124">PMC hakkında bilgilerine [Bu öğreticinin sonunda](#pmc).</span><span class="sxs-lookup"><span data-stu-id="53fb8-124">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="53fb8-125">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="53fb8-125">Change the connection string</span></span>

<span data-ttu-id="53fb8-126">İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı için bağlantı dizesinde veritabanının adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="53fb8-127">Bu değişiklik, ilk geçiş yeni bir veritabanı oluşturacaksınız böylece projeyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="53fb8-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="53fb8-128">Bu migrations ile kullanmaya başlamak için gerekli değildir, ancak daha sonra neden iyi bir fikir olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="53fb8-129">Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="53fb8-130">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:</span><span class="sxs-lookup"><span data-stu-id="53fb8-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
>
> ```console
> dotnet ef database drop
> ```
>
> <span data-ttu-id="53fb8-131">Aşağıdaki bölümde, CLI komutlarını çalıştırma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="53fb8-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="53fb8-132">Bir başlangıç geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="53fb8-132">Create an initial migration</span></span>

<span data-ttu-id="53fb8-133">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-133">Save your changes and build the project.</span></span> <span data-ttu-id="53fb8-134">Ardından bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="53fb8-135">Bunu yapmanın hızlı bir yolu aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="53fb8-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="53fb8-136">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **klasörü dosya Gezgini'nde Aç** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="53fb8-136">In **Solution Explorer**, right-click the project and choose **Open Folder in File Explorer** from the context menu.</span></span>

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="53fb8-138">Adres çubuğunda "cmd" girin ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

<span data-ttu-id="53fb8-140">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="53fb8-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="53fb8-141">Komut penceresinde aşağıdaki gibi bir çıktı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="53fb8-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="53fb8-142">Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komut "dotnet-ef"*, bakın [bu blog gönderisini](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.</span><span class="sxs-lookup"><span data-stu-id="53fb8-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="53fb8-143">Bir hata iletisi görürseniz "*... dosyaya erişemiyor ContosoUniversity.dll başka bir işlem tarafından kullanıldığı için.* ", IIS Express simgesi Windows Sistem tepsisinde bulun ve sağ tıklayın ve ardından tıklayın **ContosoUniversity > durdurma Site**.</span><span class="sxs-lookup"><span data-stu-id="53fb8-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-up-and-down-methods"></a><span data-ttu-id="53fb8-144">Yukarı ve aşağı yöntemleri inceleyin</span><span class="sxs-lookup"><span data-stu-id="53fb8-144">Examine Up and Down methods</span></span>

<span data-ttu-id="53fb8-145">Ne zaman yürütülen `migrations add` EF oluşturulan veritabanı sıfırdan oluşturacak kod komutu.</span><span class="sxs-lookup"><span data-stu-id="53fb8-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="53fb8-146">Bu kodu *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="53fb8-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="53fb8-147">`Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="53fb8-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="53fb8-148">Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53fb8-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="53fb8-149">Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53fb8-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="53fb8-150">Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu.</span><span class="sxs-lookup"><span data-stu-id="53fb8-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="53fb8-151">Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır ve istediğiniz olabilir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="53fb8-152">Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="53fb8-153">Örneğin, bir sonraki geçiş "AddDepartmentTable" adını verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="53fb8-154">Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak bu veritabanı zaten veri modelinde eşleştiğinden çalıştırmak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="53fb8-155">Burada veritabanı yok henüz veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamasını dağıttığınızda, bu nedenle, ilk test etmek için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="53fb8-156">İşte bu geçişleri sıfırdan yeni bir tane oluşturabilirsiniz. böylece daha önce--bağlantı dizesinde veritabanının adı değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="53fb8-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="53fb8-157">Veri modeli anlık görüntü</span><span class="sxs-lookup"><span data-stu-id="53fb8-157">The data model snapshot</span></span>

<span data-ttu-id="53fb8-158">Geçişleri oluşturur bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="53fb8-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="53fb8-159">Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.</span><span class="sxs-lookup"><span data-stu-id="53fb8-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="53fb8-160">Bir geçiş silerken kullanın [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.</span><span class="sxs-lookup"><span data-stu-id="53fb8-160">When deleting a migration, use the [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="53fb8-161">`dotnet ef migrations remove` geçiş siler ve anlık görüntü doğru sıfırlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="53fb8-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="53fb8-162">Bkz: [EF Core geçişleri ekip ortamlarında](/ef/core/managing-schemas/migrations/teams) anlık görüntü dosyası nasıl kullanıldığı hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="53fb8-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration"></a><span data-ttu-id="53fb8-163">Geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="53fb8-163">Apply the migration</span></span>

<span data-ttu-id="53fb8-164">Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="53fb8-165">Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanı ayarlama, komutları için günlüklerine bakın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="53fb8-166">Aşağıdaki örnek çıktıda, günlükleri çoğunu göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="53fb8-167">Bu günlük iletilerinin ayrıntı düzeyini görmek isterseniz, içinde günlük düzeyini değiştirmek *appsettings. Development.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="53fb8-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="53fb8-168">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="53fb8-168">For more information, see <xref:fundamentals/logging/index>.</span></span>

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'2.2.0-rtm-35687');
Done.
```

<span data-ttu-id="53fb8-169">Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.</span><span class="sxs-lookup"><span data-stu-id="53fb8-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="53fb8-170">Ek fark edeceksiniz bir \_ \_EFMigrationsHistory tablo, hangi geçişleri veritabanına uygulanmış olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="53fb8-170">You'll notice the addition of an \_\_EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="53fb8-171">Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="53fb8-172">(Önceki CLI çıktı örneği son günlüğünde bu satırı oluşturur INSERT deyimini gösterir.)</span><span class="sxs-lookup"><span data-stu-id="53fb8-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="53fb8-173">Her şeyin hala önceki ile aynı çalıştığını doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-173">Run the application to verify that everything still works the same as before.</span></span>

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a><span data-ttu-id="53fb8-175">CLI ile PMC karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="53fb8-175">Compare CLI and PMC</span></span>

<span data-ttu-id="53fb8-176">Geçişleri yönetme .NET Core CLI komutlarını veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="53fb8-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="53fb8-177">Bu öğreticide, CLI'yı kullanma gösterilmektedir, ancak isterseniz PMC'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="53fb8-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="53fb8-178">EF komutları PMC'yi komutlar için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket.</span><span class="sxs-lookup"><span data-stu-id="53fb8-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="53fb8-179">Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa, bir paket başvurusu ekleme yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="53fb8-179">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to add a package reference if your app has a package reference for `Microsoft.AspNetCore.App`.</span></span>

<span data-ttu-id="53fb8-180">**Önemli:** Bunun için CLI'ı yükleme düzenleyerek biri aynı pakette olmadığını *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="53fb8-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="53fb8-181">Bu ada içinde sona erecek `Tools`, biten CLI paket adı aksine `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="53fb8-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="53fb8-182">CLI komutları hakkında daha fazla bilgi için bkz. [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="53fb8-182">For more information about the CLI commands, see [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="53fb8-183">PMC komutlar hakkında daha fazla bilgi için bkz. [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="53fb8-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="53fb8-184">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="53fb8-184">Get the code</span></span>

[<span data-ttu-id="53fb8-185">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-185">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a><span data-ttu-id="53fb8-186">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="53fb8-186">Next step</span></span>

<span data-ttu-id="53fb8-187">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="53fb8-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53fb8-188">Geçiş hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="53fb8-188">Learned about migrations</span></span>
> * <span data-ttu-id="53fb8-189">NuGet geçiş paketleri hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="53fb8-189">Learned about NuGet migration packages</span></span>
> * <span data-ttu-id="53fb8-190">Bağlantı dizesi değişti</span><span class="sxs-lookup"><span data-stu-id="53fb8-190">Changed the connection string</span></span>
> * <span data-ttu-id="53fb8-191">Bir başlangıç geçiş oluşturulan</span><span class="sxs-lookup"><span data-stu-id="53fb8-191">Created an initial migration</span></span>
> * <span data-ttu-id="53fb8-192">Yukarı ve aşağı yöntem incelenir.</span><span class="sxs-lookup"><span data-stu-id="53fb8-192">Examined Up and Down methods</span></span>
> * <span data-ttu-id="53fb8-193">Veri modeli anlık görüntü hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="53fb8-193">Learned about the data model snapshot</span></span>
> * <span data-ttu-id="53fb8-194">Geçiş uygulandı</span><span class="sxs-lookup"><span data-stu-id="53fb8-194">Applied the migration</span></span>

<span data-ttu-id="53fb8-195">Veri modelini genişletme hakkında daha ileri seviyeli konulara göz atan başlamak için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="53fb8-195">Advance to the next tutorial to begin looking at more advanced topics about expanding the data model.</span></span> <span data-ttu-id="53fb8-196">Süreç boyunca oluşturun ve ek geçişlerin uygulayın.</span><span class="sxs-lookup"><span data-stu-id="53fb8-196">Along the way you'll create and apply additional migrations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="53fb8-197">Oluşturun ve ek geçişlerin uygulayın</span><span class="sxs-lookup"><span data-stu-id="53fb8-197">Create and apply additional migrations</span></span>](complex-data-model.md)
