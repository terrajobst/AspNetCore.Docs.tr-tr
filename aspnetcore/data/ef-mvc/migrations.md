---
title: 'Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma'
description: Bu öğreticide, ASP.NET Core MVC uygulamasındaki veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 6d4ed0e95499c30417e1cfd07f57de824a8a62ed
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265527"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a><span data-ttu-id="27478-103">Öğretici: -EF çekirdekli ASP.NET MVC geçişleri özelliğini kullanma</span><span class="sxs-lookup"><span data-stu-id="27478-103">Tutorial: Using the migrations feature - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="27478-104">Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="27478-104">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="27478-105">Sonraki öğreticilerde, veri modeli değiştikçe daha fazla geçişleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27478-105">In later tutorials, you'll add more migrations as you change the data model.</span></span>

<span data-ttu-id="27478-106">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="27478-106">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27478-107">Geçiş hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="27478-107">Learn about migrations</span></span>
> * <span data-ttu-id="27478-108">NuGet geçiş paketleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="27478-108">Learn about NuGet migration packages</span></span>
> * <span data-ttu-id="27478-109">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="27478-109">Change the connection string</span></span>
> * <span data-ttu-id="27478-110">Bir başlangıç geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="27478-110">Create an initial migration</span></span>
> * <span data-ttu-id="27478-111">Yukarı ve aşağı yöntemleri inceleyin</span><span class="sxs-lookup"><span data-stu-id="27478-111">Examine Up and Down methods</span></span>
> * <span data-ttu-id="27478-112">Veri modeli anlık görüntü hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="27478-112">Learn about the data model snapshot</span></span>
> * <span data-ttu-id="27478-113">Geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="27478-113">Apply the migration</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27478-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="27478-114">Prerequisites</span></span>

* [<span data-ttu-id="27478-115">Sıralama, filtreleme ve EF Core ile bir ASP.NET Core MVC uygulamasında sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="27478-115">Add sorting, filtering, and paging with EF Core in an ASP.NET Core MVC app</span></span>](sort-filter-page.md)

## <a name="about-migrations"></a><span data-ttu-id="27478-116">Geçiş hakkında</span><span class="sxs-lookup"><span data-stu-id="27478-116">About migrations</span></span>

<span data-ttu-id="27478-117">Yeni bir uygulama geliştirdiğinizde, model değişiklikleri sık ve her zaman veri modelinizi değiştirir, veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="27478-117">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="27478-118">Bu öğretici, yoksa veritabanını oluşturmak için Entity Framework yapılandırarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="27478-118">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="27478-119">Ardından, veri modeli değiştirmek--eklemek, kaldırmak veya varlık sınıflarını değiştirmek veya DbContext sınıfınıza--değiştirmek, her zaman veritabanını silebilir ve EF modeli eşleşir ve test verileri ile çekirdeğini yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27478-119">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="27478-120">Veritabanı veri modeli ile eşitlenmiş tutmak için bu yöntemi de uygulamayı üretim ortamına dağıtmadan kadar çalışır.</span><span class="sxs-lookup"><span data-stu-id="27478-120">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="27478-121">Üretim ortamında genellikle tutmak istediğiniz ve her şey her zaman kaybetmek istemediğiniz veri depolama uygulama çalıştırılırken, yeni bir sütun ekleme gibi bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="27478-121">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="27478-122">EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine veritabanı şemasını güncelleştirmek EF sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="27478-122">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="about-nuget-migration-packages"></a><span data-ttu-id="27478-123">NuGet geçiş paketleri hakkında</span><span class="sxs-lookup"><span data-stu-id="27478-123">About NuGet migration packages</span></span>

<span data-ttu-id="27478-124">Migrations ile çalışmak için kullanabileceğiniz **Paket Yöneticisi Konsolu** (PMC) veya komut satırı arabirimi (CLI).</span><span class="sxs-lookup"><span data-stu-id="27478-124">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="27478-125">Bu öğreticiler CLI komutlarının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="27478-125">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="27478-126">PMC hakkında bilgilerine [Bu öğreticinin sonunda](#pmc).</span><span class="sxs-lookup"><span data-stu-id="27478-126">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="27478-127">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="27478-127">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="27478-128">Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* gösterildiği gibi dosya.</span><span class="sxs-lookup"><span data-stu-id="27478-128">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="27478-129">**Not:** Düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI.</span><span class="sxs-lookup"><span data-stu-id="27478-129">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="27478-130">Düzenleyebileceğiniz *.csproj* proje adına sağ tıklanarak dosya **Çözüm Gezgini** seçerek **Düzenle ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="27478-130">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]

<span data-ttu-id="27478-131">(Bu örnekte sürüm numaraları, öğreticiyi yazıldıktan sonra geçerli.)</span><span class="sxs-lookup"><span data-stu-id="27478-131">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="27478-132">Bağlantı dizesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="27478-132">Change the connection string</span></span>

<span data-ttu-id="27478-133">İçinde *appsettings.json* dosya, ContosoUniversity2 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı için bağlantı dizesinde veritabanının adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="27478-133">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="27478-134">Bu değişiklik, ilk geçiş yeni bir veritabanı oluşturacaksınız böylece projeyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="27478-134">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="27478-135">Bu migrations ile kullanmaya başlamak için gerekli değildir, ancak daha sonra neden iyi bir fikir olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="27478-135">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="27478-136">Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27478-136">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="27478-137">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:</span><span class="sxs-lookup"><span data-stu-id="27478-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="27478-138">Aşağıdaki bölümde, CLI komutlarını çalıştırma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="27478-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="27478-139">Bir başlangıç geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="27478-139">Create an initial migration</span></span>

<span data-ttu-id="27478-140">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="27478-140">Save your changes and build the project.</span></span> <span data-ttu-id="27478-141">Ardından bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="27478-141">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="27478-142">Bunu yapmanın hızlı bir yolu aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="27478-142">Here's a quick way to do that:</span></span>

* <span data-ttu-id="27478-143">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **klasörü dosya Gezgini'nde Aç** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="27478-143">In **Solution Explorer**, right-click the project and choose **Open Folder in File Explorer** from the context menu.</span></span>

  ![Dosya Gezgini menü öğesini Aç](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="27478-145">Adres çubuğunda "cmd" girin ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="27478-145">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Komut penceresini aç](migrations/_static/open-command-window.png)

<span data-ttu-id="27478-147">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="27478-147">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="27478-148">Komut penceresinde aşağıdaki gibi bir çıktı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="27478-148">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="27478-149">Bir hata iletisi görürseniz *hiçbir yürütülebilir bulunan eşleşen komut "dotnet-ef"*, bakın [bu blog gönderisini](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) sorun giderme Yardımı.</span><span class="sxs-lookup"><span data-stu-id="27478-149">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="27478-150">Bir hata iletisi görürseniz "*... dosyaya erişemiyor ContosoUniversity.dll başka bir işlem tarafından kullanıldığı için.* ", IIS Express simgesi Windows Sistem tepsisinde bulun ve sağ tıklayın ve ardından tıklayın **ContosoUniversity > durdurma Site**.</span><span class="sxs-lookup"><span data-stu-id="27478-150">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-up-and-down-methods"></a><span data-ttu-id="27478-151">Yukarı ve aşağı yöntemleri inceleyin</span><span class="sxs-lookup"><span data-stu-id="27478-151">Examine Up and Down methods</span></span>

<span data-ttu-id="27478-152">Ne zaman yürütülen `migrations add` EF oluşturulan veritabanı sıfırdan oluşturacak kod komutu.</span><span class="sxs-lookup"><span data-stu-id="27478-152">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="27478-153">Bu kodu *geçişler* klasöründe adlı dosyayı  *\<zaman damgası > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="27478-153">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="27478-154">`Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi siler, bunları, aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="27478-154">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="27478-155">Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="27478-155">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="27478-156">Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="27478-156">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="27478-157">Girdiğiniz zaman, oluşturulan ilk geçiş için bu kodu `migrations add InitialCreate` komutu.</span><span class="sxs-lookup"><span data-stu-id="27478-157">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="27478-158">Geçiş name parametresi (Bu örnekte "InitialCreate"), dosya adı için kullanılır ve istediğiniz olabilir.</span><span class="sxs-lookup"><span data-stu-id="27478-158">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="27478-159">Bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçmek en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="27478-159">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="27478-160">Örneğin, bir sonraki geçiş "AddDepartmentTable" adını verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27478-160">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="27478-161">Veritabanı zaten mevcut olduğunda ilk geçiş oluşturduysanız, veritabanı oluşturma kod oluşturulur ancak bu veritabanı zaten veri modelinde eşleştiğinden çalıştırmak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="27478-161">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="27478-162">Burada veritabanı yok henüz veritabanınızı oluşturmak için bu kodu çalıştıracak başka bir ortama uygulamasını dağıttığınızda, bu nedenle, ilk test etmek için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="27478-162">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="27478-163">İşte bu geçişleri sıfırdan yeni bir tane oluşturabilirsiniz. böylece daha önce--bağlantı dizesinde veritabanının adı değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="27478-163">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="27478-164">Veri modeli anlık görüntü</span><span class="sxs-lookup"><span data-stu-id="27478-164">The data model snapshot</span></span>

<span data-ttu-id="27478-165">Geçişleri oluşturur bir *anlık görüntü* içinde geçerli veritabanı şeması *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="27478-165">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="27478-166">Bir geçiş eklediğinizde, anlık görüntü dosyası ve veri modelini karşılaştırarak değişiklikler EF belirler.</span><span class="sxs-lookup"><span data-stu-id="27478-166">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="27478-167">Bir geçiş silerken kullanın [dotnet ef geçişleri Kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) komutu.</span><span class="sxs-lookup"><span data-stu-id="27478-167">When deleting a migration, use the [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="27478-168">`dotnet ef migrations remove` geçiş siler ve anlık görüntü doğru sıfırlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="27478-168">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="27478-169">Bkz: [EF Core geçişleri ekip ortamlarında](/ef/core/managing-schemas/migrations/teams) anlık görüntü dosyası nasıl kullanıldığı hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="27478-169">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration"></a><span data-ttu-id="27478-170">Geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="27478-170">Apply the migration</span></span>

<span data-ttu-id="27478-171">Komut penceresinde, tablo ve veritabanı içinde oluşturmak için aşağıdaki komutu girin.</span><span class="sxs-lookup"><span data-stu-id="27478-171">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="27478-172">Komut çıktısı benzer `migrations add` komutu dışında SQL veritabanı ayarlama, komutları için günlüklerine bakın.</span><span class="sxs-lookup"><span data-stu-id="27478-172">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="27478-173">Aşağıdaki örnek çıktıda, günlükleri çoğunu göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="27478-173">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="27478-174">Bu günlük iletilerinin ayrıntı düzeyini görmek isterseniz, içinde günlük düzeyini değiştirmek *appsettings. Development.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="27478-174">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="27478-175">Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="27478-175">For more information, see <xref:fundamentals/logging/index>.</span></span>

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

<span data-ttu-id="27478-176">Kullanım **SQL Server Nesne Gezgini** ilk öğreticide yaptığınız gibi veritabanı incelemek için.</span><span class="sxs-lookup"><span data-stu-id="27478-176">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="27478-177">Ek fark edeceksiniz bir \_ \_EFMigrationsHistory tablo, hangi geçişleri veritabanına uygulanmış olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="27478-177">You'll notice the addition of an \_\_EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="27478-178">Bu tablodaki verileri görüntüleyebilir ve ilk geçiş için bir satır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="27478-178">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="27478-179">(Önceki CLI çıktı örneği son günlüğünde bu satırı oluşturur INSERT deyimini gösterir.)</span><span class="sxs-lookup"><span data-stu-id="27478-179">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="27478-180">Her şeyin hala önceki ile aynı çalıştığını doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="27478-180">Run the application to verify that everything still works the same as before.</span></span>

![Öğrenciler dizin sayfası](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a><span data-ttu-id="27478-182">CLI ile PMC karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="27478-182">Compare CLI and PMC</span></span>

<span data-ttu-id="27478-183">Geçişleri yönetme .NET Core CLI komutlarını veya Visual Studio'da PowerShell cmdlet'leri kullanılabilir EF tooling **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="27478-183">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="27478-184">Bu öğreticide, CLI'yı kullanma gösterilmektedir, ancak isterseniz PMC'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27478-184">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="27478-185">EF komutları PMC'yi komutlar için bulunan [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paket.</span><span class="sxs-lookup"><span data-stu-id="27478-185">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="27478-186">Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa, bir paket başvurusu ekleme yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="27478-186">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to add a package reference if your app has a package reference for `Microsoft.AspNetCore.App`.</span></span>

<span data-ttu-id="27478-187">**Önemli:** Bunun için CLI'ı yükleme düzenleyerek biri aynı pakette olmadığını *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="27478-187">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="27478-188">Bu ada içinde sona erecek `Tools`, biten CLI paket adı aksine `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="27478-188">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="27478-189">CLI komutları hakkında daha fazla bilgi için bkz. [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="27478-189">For more information about the CLI commands, see [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="27478-190">PMC komutlar hakkında daha fazla bilgi için bkz. [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="27478-190">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="27478-191">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="27478-191">Get the code</span></span>

[<span data-ttu-id="27478-192">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="27478-192">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a><span data-ttu-id="27478-193">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="27478-193">Next step</span></span>

<span data-ttu-id="27478-194">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="27478-194">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27478-195">Geçiş hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="27478-195">Learned about migrations</span></span>
> * <span data-ttu-id="27478-196">NuGet geçiş paketleri hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="27478-196">Learned about NuGet migration packages</span></span>
> * <span data-ttu-id="27478-197">Bağlantı dizesi değişti</span><span class="sxs-lookup"><span data-stu-id="27478-197">Changed the connection string</span></span>
> * <span data-ttu-id="27478-198">Bir başlangıç geçiş oluşturulan</span><span class="sxs-lookup"><span data-stu-id="27478-198">Created an initial migration</span></span>
> * <span data-ttu-id="27478-199">Yukarı ve aşağı yöntem incelenir.</span><span class="sxs-lookup"><span data-stu-id="27478-199">Examined Up and Down methods</span></span>
> * <span data-ttu-id="27478-200">Veri modeli anlık görüntü hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="27478-200">Learned about the data model snapshot</span></span>
> * <span data-ttu-id="27478-201">Geçiş uygulandı</span><span class="sxs-lookup"><span data-stu-id="27478-201">Applied the migration</span></span>

<span data-ttu-id="27478-202">Veri modelini genişletme hakkında daha ileri seviyeli konulara göz atan başlamak için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="27478-202">Advance to the next article to begin looking at more advanced topics about expanding the data model.</span></span> <span data-ttu-id="27478-203">Süreç boyunca oluşturun ve ek geçişlerin uygulayın.</span><span class="sxs-lookup"><span data-stu-id="27478-203">Along the way you'll create and apply additional migrations.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="27478-204">Oluşturun ve ek geçişlerin uygulayın</span><span class="sxs-lookup"><span data-stu-id="27478-204">Create and apply additional migrations</span></span>](complex-data-model.md)
