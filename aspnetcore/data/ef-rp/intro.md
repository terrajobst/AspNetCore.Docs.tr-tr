---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: rick-anderson
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 94783aa9014aef4c5f775fc8f36a2c3a7715e4b6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656823"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="d4037-103">ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="d4037-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="d4037-104">, [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="d4037-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d4037-105">Bu, bir [ASP.NET Core Razor Pages](xref:razor-pages/index) uygulamasında ENTITY Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="d4037-106">Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d4037-107">Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="d4037-108">Öğretici ilk kod yaklaşımını kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-108">The tutorial uses the code first approach.</span></span> <span data-ttu-id="d4037-109">Bu öğreticiyi veritabanı ilk yaklaşımı kullanarak takip eden bilgiler için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/16897)bakın.</span><span class="sxs-lookup"><span data-stu-id="d4037-109">For information on following this tutorial using the database first approach, see [this Github issue](https://github.com/dotnet/AspNetCore.Docs/issues/16897).</span></span>

[<span data-ttu-id="d4037-110">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-110">Download or view the completed app.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d4037-111">[Yönergeleri indirin](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d4037-111">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4037-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d4037-112">Prerequisites</span></span>

* <span data-ttu-id="d4037-113">Razor Pages yeni başladıysanız, bu duruma başlamadan önce Razor Pages öğretici serisini [kullanmaya](xref:tutorials/razor-pages/razor-pages-start) başlayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-113">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="d4037-116">Veritabanı altyapıları</span><span class="sxs-lookup"><span data-stu-id="d4037-116">Database engines</span></span>

<span data-ttu-id="d4037-117">Visual Studio yönergeleri, yalnızca Windows üzerinde çalışan bir SQL Server Express sürümü olan [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-117">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="d4037-118">Visual Studio Code yönergeler, platformlar arası bir veritabanı altyapısı olan [SQLite](https://www.sqlite.org/)kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-118">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="d4037-119">SQLite kullanmayı seçerseniz, SQLite [Için DB tarayıcısı](https://sqlitebrowser.org/)gibi bir SQLite veritabanını yönetmek ve görüntülemek için üçüncü taraf bir araç indirip yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-119">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d4037-120">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="d4037-120">Troubleshooting</span></span>

<span data-ttu-id="d4037-121">Giderebileceğiniz bir sorunla karşılaşırsanız, kodunuzu [Tamamlanan projeyle](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-121">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="d4037-122">Yardım almanın iyi bir yolu, [ASP.NET Core etiketi](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core etiketi](https://stackoverflow.com/questions/tagged/entity-framework-core)kullanılarak StackOverflow.com 'e bir soru göndererek.</span><span class="sxs-lookup"><span data-stu-id="d4037-122">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="d4037-123">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="d4037-123">The sample app</span></span>

<span data-ttu-id="d4037-124">Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-124">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="d4037-125">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d4037-126">Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d4037-126">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

<span data-ttu-id="d4037-129">Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-129">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="d4037-130">Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.</span><span class="sxs-lookup"><span data-stu-id="d4037-130">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="d4037-131">Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-131">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="d4037-132">*Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-132">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="d4037-133">1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-133">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-134">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d4037-135">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="d4037-135">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="d4037-136">Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-136">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="d4037-137">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-137">Build the project.</span></span>
* <span data-ttu-id="d4037-138">Paket Yöneticisi konsolu 'nda (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-138">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="d4037-139">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-139">Run the project to seed the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-140">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-140">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d4037-141">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="d4037-141">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="d4037-142">*Contosouniversity. csproj*öğesini silin ve *Contosoüniversıtysqlite. csproj* öğesini *contosouniversity. csproj*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-142">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="d4037-143">*Startup.cs*silin ve *StartupSQLite.cs* öğesini *Startup.cs*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-143">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="d4037-144">*AppSettings. JSON*öğesini silin ve *Appsettingssqlite. JSON* öğesini *appSettings. JSON*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-144">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="d4037-145">*Geçişler* klasörünü silin ve *migrationssql* öğesini *geçişlerle*yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-145">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="d4037-146">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-146">Build the project.</span></span>
* <span data-ttu-id="d4037-147">Proje klasöründeki bir komut isteminde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-147">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* <span data-ttu-id="d4037-148">SQLite aracında şu SQL ifadesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-148">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="d4037-149">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-149">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="d4037-150">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-150">Create the web app project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-152">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d4037-153">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d4037-154">Projeyi *Contosouniversity*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-154">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="d4037-155">Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d4037-155">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="d4037-156">Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-156">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4037-158">Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.</span><span class="sxs-lookup"><span data-stu-id="d4037-158">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="d4037-159">Razor Pages bir proje oluşturmak ve yeni proje klasörüne `cd` için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-159">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="d4037-160">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="d4037-160">Set up the site style</span></span>

<span data-ttu-id="d4037-161">*Sayfa/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d4037-161">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="d4037-162">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-162">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="d4037-163">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-163">There are three occurrences.</span></span>

* <span data-ttu-id="d4037-164">**Giriş** ve **Gizlilik** menü girişlerini silin ve **hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için girişler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-164">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="d4037-165">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-165">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="d4037-166">*Pages/Index. cshtml*dosyasında, ASP.NET Core hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-166">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="d4037-167">Giriş sayfasının göründüğünü doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-167">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="d4037-168">Veri modeli</span><span class="sxs-lookup"><span data-stu-id="d4037-168">The data model</span></span>

<span data-ttu-id="d4037-169">Aşağıdaki bölümler bir veri modeli oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d4037-169">The following sections create a data model:</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="d4037-171">Bir öğrenci herhangi bir sayıda kursa kaydolabilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-171">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="d4037-172">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-172">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

* <span data-ttu-id="d4037-174">Proje klasöründe bir *modeller* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-174">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="d4037-175">Aşağıdaki kodla *modeller/öğrenci. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-175">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="d4037-176">`ID` özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur.</span><span class="sxs-lookup"><span data-stu-id="d4037-176">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="d4037-177">Varsayılan olarak, EF Core `ID` veya `classnameID` adında bir özelliği birincil anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-177">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="d4037-178">Bu nedenle, `Student` sınıfı birincil anahtarı için otomatik olarak tanınan ad `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d4037-178">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="d4037-179">`Enrollments` özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="d4037-179">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="d4037-180">Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar.</span><span class="sxs-lookup"><span data-stu-id="d4037-180">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="d4037-181">Bu durumda, bir `Student` varlığının `Enrollments` özelliği söz konusu öğrenci ile ilgili `Enrollment` varlıkların tümünü barındırır.</span><span class="sxs-lookup"><span data-stu-id="d4037-181">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="d4037-182">Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-182">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="d4037-183">Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-183">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="d4037-184">Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="d4037-184">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="d4037-185">İlgili kayıt satırları Studentitıd = 1 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d4037-185">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="d4037-186">Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* .</span><span class="sxs-lookup"><span data-stu-id="d4037-186">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="d4037-187">Birden çok ilgili kayıt varlığı olabileceğinden `Enrollments` özelliği `ICollection<Enrollment>` olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-187">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="d4037-188">`List<Enrollment>` veya `HashSet<Enrollment>`gibi başka koleksiyon türleri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-188">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="d4037-189">`ICollection<Enrollment>` kullanıldığında EF Core varsayılan olarak bir `HashSet<Enrollment>` koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-189">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="d4037-190">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-190">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="d4037-192">Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-192">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="d4037-193">`EnrollmentID` özelliği birincil anahtardır; Bu varlık, `ID` yerine `classnameID` modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-193">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="d4037-194">Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4037-194">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="d4037-195">Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-195">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="d4037-196">`classname` olmadan `ID` kullanmak, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="d4037-196">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="d4037-197">`Grade` özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="d4037-197">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d4037-198">`Grade` türü bildiriminden sonraki soru işareti, `Grade` özelliğinin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4037-198">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="d4037-199">Null olan bir sınıf sıfır bir sınıfa göre farklılık gösterir&mdash;null, henüz bir sınıf bilinmediğini veya henüz atanmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4037-199">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d4037-200">`StudentID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Student`.</span><span class="sxs-lookup"><span data-stu-id="d4037-200">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d4037-201">`Enrollment` varlık bir `Student` varlığıyla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-201">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="d4037-202">`CourseID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Course`.</span><span class="sxs-lookup"><span data-stu-id="d4037-202">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d4037-203">Bir `Enrollment` varlığı bir `Course` varlığıyla ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-203">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d4037-204">EF Core, `<navigation property name><primary key property name>`adında bir özelliği yabancı anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-204">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d4037-205">Örneğin, `Student` varlığın birincil anahtarı `ID`olduğundan, `Student` gezinti özelliği için`StudentID` yabancı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-205">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d4037-206">Yabancı anahtar özelliklerine de `<primary key property name>`adı verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-206">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d4037-207">Örneğin, `Course` varlığın birincil anahtarı `CourseID`olduğundan `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d4037-207">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="d4037-208">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-208">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="d4037-210">Aşağıdaki kodla *modeller/kurs. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-210">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="d4037-211">`Enrollments` özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-211">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d4037-212">`Course` bir varlık, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-212">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d4037-213">`DatabaseGenerated` özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-213">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="d4037-214">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-214">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="d4037-215">Yapı iskelesi öğrenci sayfaları</span><span class="sxs-lookup"><span data-stu-id="d4037-215">Scaffold Student pages</span></span>

<span data-ttu-id="d4037-216">Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:</span><span class="sxs-lookup"><span data-stu-id="d4037-216">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="d4037-217">EF Core *bağlamı* sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d4037-217">An EF Core *context* class.</span></span> <span data-ttu-id="d4037-218">Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="d4037-218">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d4037-219">`Microsoft.EntityFrameworkCore.DbContext` sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-219">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="d4037-220">`Student` varlık için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="d4037-220">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-222">*Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-222">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="d4037-223">**Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve > **yeni yapı iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-223">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d4037-224">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-224">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="d4037-225">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="d4037-225">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="d4037-226">**Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-226">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="d4037-227">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-227">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="d4037-228">*Contosouniversity. modeller. Contosoüniversıtycontext* olan veri bağlamı adını *Contosouniversity. Data. SchoolContext*olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-228">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="d4037-229">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-229">Select **Add**.</span></span>

<span data-ttu-id="d4037-230">Aşağıdaki paketler otomatik olarak yüklenir:</span><span class="sxs-lookup"><span data-stu-id="d4037-230">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4037-232">Gerekli NuGet paketlerini yüklemek için aşağıdaki .NET Core CLI komutlarını çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-232">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  <span data-ttu-id="d4037-233">Yapı iskelesi için Microsoft. VisualStudio. Web. CodeGeneration. Design paketi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-233">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="d4037-234">Uygulama SQL Server kullanmayabilse de, scafkatlama aracı SQL Server paketine ihtiyaç duyuyor.</span><span class="sxs-lookup"><span data-stu-id="d4037-234">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="d4037-235">*Sayfalar/öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-235">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="d4037-236">[ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator)'nı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-236">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="d4037-237">Fkatlama öğrenci sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-237">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="d4037-238">**Windows 'da**</span><span class="sxs-lookup"><span data-stu-id="d4037-238">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="d4037-239">**MacOS veya Linux üzerinde**</span><span class="sxs-lookup"><span data-stu-id="d4037-239">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="d4037-240">Önceki adımla ilgili bir sorununuz varsa, projeyi derleyin ve yapı iskelesi adımını yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-240">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="d4037-241">Yapı iskelesi işlemi:</span><span class="sxs-lookup"><span data-stu-id="d4037-241">The scaffolding process:</span></span>

* <span data-ttu-id="d4037-242">*Sayfalar/öğrenciler* klasöründe Razor sayfaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d4037-242">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="d4037-243">*. Cshtml* ve *Create.cshtml.cs* oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-243">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="d4037-244">*Delete. cshtml* ve *delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d4037-244">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="d4037-245">*Details. cshtml* ve *details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d4037-245">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="d4037-246">*. Cshtml* ve *Edit.cshtml.cs* Düzenle</span><span class="sxs-lookup"><span data-stu-id="d4037-246">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="d4037-247">*Index. cshtml* ve *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d4037-247">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="d4037-248">*Data/SchoolContext. cs*oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-248">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="d4037-249">*Startup.cs*içinde bağımlılık eklenmesine bağlam ekler.</span><span class="sxs-lookup"><span data-stu-id="d4037-249">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="d4037-250">*AppSettings. JSON*öğesine bir veritabanı bağlantı dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="d4037-250">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="d4037-251">Veritabanı bağlantı dizesi</span><span class="sxs-lookup"><span data-stu-id="d4037-251">Database connection string</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d4037-253">Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4037-253">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="d4037-254">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d4037-254">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d4037-255">Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-255">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-256">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-256">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d4037-257">Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-257">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="d4037-258">Veritabanı bağlam sınıfını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="d4037-258">Update the database context class</span></span>

<span data-ttu-id="d4037-259">Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-259">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="d4037-260">Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-260">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d4037-261">Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4037-261">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d4037-262">Bu projede, sınıfı `SchoolContext`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d4037-263">Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="d4037-264">Vurgulanan kod, her bir varlık kümesi için bir [Dbset\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="d4037-265">EF Core terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="d4037-265">In EF Core terminology:</span></span>

* <span data-ttu-id="d4037-266">Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d4037-266">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="d4037-267">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d4037-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d4037-268">Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-268">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="d4037-269">Scafkatlama aracı bir`Student` DBSet oluşturduğundan, bu adım bunu plural `Students`olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="d4037-269">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="d4037-270">Razor Pages kodun yeni DBSet adıyla eşleşmesini sağlamak için, tüm `_context.Student` projesi genelinde `_context.Students`için genel bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="d4037-270">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="d4037-271">8 oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-271">There are 8 occurrences.</span></span>

<span data-ttu-id="d4037-272">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-272">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="d4037-273">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d4037-273">Startup.cs</span></span>

<span data-ttu-id="d4037-274">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d4037-274">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d4037-275">Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-275">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d4037-276">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-276">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d4037-277">Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d4037-277">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d4037-278">Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.</span><span class="sxs-lookup"><span data-stu-id="d4037-278">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-279">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-279">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-280">`ConfigureServices`, vurgulanan satırlar scaffolder tarafından eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="d4037-280">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-281">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-281">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4037-282">`ConfigureServices`, desteği tarafından eklenen kodun `UseSqlite`çağırdığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d4037-282">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="d4037-283">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-283">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d4037-284">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="d4037-284">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d4037-285">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-285">Create the database</span></span>

<span data-ttu-id="d4037-286">Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-286">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="d4037-287">Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="d4037-287">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="d4037-288">Veritabanı yoksa, veritabanını ve şemayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-288">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="d4037-289">`EnsureCreated`, veri modeli değişikliklerini işlemek için aşağıdaki iş akışına izin vermez:</span><span class="sxs-lookup"><span data-stu-id="d4037-289">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="d4037-290">Veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-290">Delete the database.</span></span> <span data-ttu-id="d4037-291">Mevcut veriler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="d4037-291">Any existing data is lost.</span></span>
* <span data-ttu-id="d4037-292">Veri modelini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-292">Change the data model.</span></span> <span data-ttu-id="d4037-293">Örneğin, bir `EmailAddress` alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-293">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="d4037-294">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-294">Run the app.</span></span>
* <span data-ttu-id="d4037-295">`EnsureCreated` yeni şemaya sahip bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-295">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="d4037-296">Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-296">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="d4037-297">Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-297">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="d4037-298">Bu durumda, geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4037-298">When that is the case, use migrations.</span></span>

<span data-ttu-id="d4037-299">Öğretici serisinde daha sonra, `EnsureCreated` tarafından oluşturulan veritabanını silin ve bunun yerine geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4037-299">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="d4037-300">`EnsureCreated` tarafından oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="d4037-300">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d4037-301">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="d4037-301">Test the app</span></span>

* <span data-ttu-id="d4037-302">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-302">Run the app.</span></span>
* <span data-ttu-id="d4037-303">**Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d4037-303">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="d4037-304">Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-304">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="d4037-305">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-305">Seed the database</span></span>

<span data-ttu-id="d4037-306">`EnsureCreated` yöntemi boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-306">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="d4037-307">Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.</span><span class="sxs-lookup"><span data-stu-id="d4037-307">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="d4037-308">Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-308">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="d4037-309">Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d4037-309">The code checks if there are any students in the database.</span></span> <span data-ttu-id="d4037-310">Öğrenci yoksa, veritabanına test verileri ekler.</span><span class="sxs-lookup"><span data-stu-id="d4037-310">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="d4037-311">Performansı iyileştirmek için `List<T>` koleksiyonları yerine diziler halinde test verileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-311">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="d4037-312">*Program.cs*' de `EnsureCreated` çağrısını `DbInitializer.Initialize` çağrısıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-312">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ```

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-313">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-313">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d4037-314">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-314">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4037-316">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-316">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="d4037-317">Uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="d4037-317">Restart the app.</span></span>

* <span data-ttu-id="d4037-318">Sağlanan verileri görmek için öğrenciler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-318">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="d4037-319">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="d4037-319">View the database</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-320">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-320">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-321">Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="d4037-321">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="d4037-322">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-322">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="d4037-323">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d4037-323">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="d4037-324">**Tables** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="d4037-324">Expand the **Tables** node.</span></span>
* <span data-ttu-id="d4037-325">Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-325">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="d4037-326">`Student` modelinin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-326">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-327">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-327">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d4037-328">Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4037-328">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="d4037-329">Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="d4037-329">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="d4037-330">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="d4037-330">Asynchronous code</span></span>

<span data-ttu-id="d4037-331">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="d4037-331">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d4037-332">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-332">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d4037-333">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="d4037-333">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d4037-334">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="d4037-334">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d4037-335">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-335">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d4037-336">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-336">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="d4037-337">Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar.</span><span class="sxs-lookup"><span data-stu-id="d4037-337">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d4037-338">Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="d4037-338">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d4037-339">Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` metodu kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="d4037-339">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="d4037-340">`async` anahtar sözcüğü derleyiciye şunu söyler:</span><span class="sxs-lookup"><span data-stu-id="d4037-340">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="d4037-341">Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-341">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d4037-342">Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-342">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="d4037-343">`Task<T>` dönüş türü devam eden çalışmayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d4037-343">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="d4037-344">`await` anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d4037-344">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d4037-345">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="d4037-345">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="d4037-346">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-346">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="d4037-347">`ToListAsync`, `ToList` uzantısı yönteminin zaman uyumsuz sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="d4037-347">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d4037-348">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d4037-348">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d4037-349">Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d4037-349">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="d4037-350">Bu `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`ve `SaveChangesAsync`içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-350">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d4037-351">`var students = context.Students.Where(s => s.LastName == "Davolio")`gibi yalnızca bir `IQueryable`değiştiren deyimler içermez.</span><span class="sxs-lookup"><span data-stu-id="d4037-351">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="d4037-352">EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-352">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="d4037-353">Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-353">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="d4037-354">.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama</span><span class="sxs-lookup"><span data-stu-id="d4037-354">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4037-355">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d4037-355">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d4037-356">Sonraki öğretici</span><span class="sxs-lookup"><span data-stu-id="d4037-356">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d4037-357">Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4037-357">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="d4037-358">Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-358">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d4037-359">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-359">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="d4037-360">Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.</span><span class="sxs-lookup"><span data-stu-id="d4037-360">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="d4037-361">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-361">Download or view the completed app.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d4037-362">[Yönergeleri indirin](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d4037-362">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4037-363">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d4037-363">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-364">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-364">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-365">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-365">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="d4037-366">[Razor Pages](xref:razor-pages/index)hakkında benzerlik.</span><span class="sxs-lookup"><span data-stu-id="d4037-366">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="d4037-367">Yeni programcılar, bu seriyi başlatmadan önce [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) ' i tamamlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-367">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d4037-368">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="d4037-368">Troubleshooting</span></span>

<span data-ttu-id="d4037-369">Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-369">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="d4037-370">[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) [için bir](https://stackoverflow.com/questions/tagged/asp.net-core) soru göndererek yardım almanın iyi bir yolu.</span><span class="sxs-lookup"><span data-stu-id="d4037-370">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="d4037-371">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="d4037-371">The Contoso University web app</span></span>

<span data-ttu-id="d4037-372">Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-372">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="d4037-373">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-373">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d4037-374">Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d4037-374">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="d4037-377">Bu sitenin UI Stili yerleşik şablonları tarafından üretilen yakın ' dir.</span><span class="sxs-lookup"><span data-stu-id="d4037-377">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="d4037-378">EF çekirdekli Razor sayfaları, UI ile öğretici odağı açıktır.</span><span class="sxs-lookup"><span data-stu-id="d4037-378">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="d4037-379">ContosoUniversity Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-379">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-380">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-380">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-381">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-381">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d4037-382">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-382">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d4037-383">Projeyi **Contosouniversity**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-383">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="d4037-384">Kod kopyalama/yapıştırma olduğunda, ad alanlarının eşleşmesi için *Contosouniversity* projesini adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-384">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="d4037-385">Açılan listede **ASP.NET Core 2,1** ' i seçin ve ardından **Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-385">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="d4037-386">Yukarıdaki adımların görüntüleri için bkz. [Razor Web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="d4037-386">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="d4037-387">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-387">Run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="d4037-389">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="d4037-389">Set up the site style</span></span>

<span data-ttu-id="d4037-390">Birkaç değişiklik site menü, Düzen ve giriş sayfası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-390">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="d4037-391">*Sayfaları/paylaşılan/_Layout. cshtml* 'yi aşağıdaki değişikliklerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-391">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="d4037-392">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-392">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="d4037-393">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-393">There are three occurrences.</span></span>

* <span data-ttu-id="d4037-394">**Öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **kişi** menü girişini silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-394">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="d4037-395">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-395">The changes are highlighted.</span></span> <span data-ttu-id="d4037-396">(Tüm *biçimlendirme gösterilmez.* )</span><span class="sxs-lookup"><span data-stu-id="d4037-396">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="d4037-397">*Pages/Index. cshtml*dosyasında, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-397">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="d4037-398">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="d4037-398">Create the data model</span></span>

<span data-ttu-id="d4037-399">Varlık sınıflarının Contoso University uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-399">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="d4037-400">Aşağıdaki üç varlıklarla başlayın:</span><span class="sxs-lookup"><span data-stu-id="d4037-400">Start with the following three entities:</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="d4037-402">`Student` ve `Enrollment` varlıkları arasında bire çok ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-402">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="d4037-403">`Course` ve `Enrollment` varlıkları arasında bire çok ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-403">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="d4037-404">Bir öğrenci herhangi bir sayıda kursları kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-404">A student can enroll in any number of courses.</span></span> <span data-ttu-id="d4037-405">Bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-405">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="d4037-406">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d4037-406">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="d4037-407">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-407">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="d4037-409">*Modeller* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-409">Create a *Models* folder.</span></span> <span data-ttu-id="d4037-410">*Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyasını aşağıdaki kodla oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-410">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="d4037-411">`ID` özelliği, bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu olur.</span><span class="sxs-lookup"><span data-stu-id="d4037-411">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="d4037-412">Varsayılan olarak, EF Core `ID` veya `classnameID` adında bir özelliği birincil anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-412">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="d4037-413">`classnameID`, `classname` sınıfın adıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-413">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="d4037-414">Diğer otomatik olarak tanınan birincil anahtar, önceki örnekte `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d4037-414">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="d4037-415">`Enrollments` özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="d4037-415">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="d4037-416">Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-416">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="d4037-417">Bu durumda, bir `Student entity` `Enrollments` özelliği, bu `Student`ilgili `Enrollment` varlıkların tümünü barındırır.</span><span class="sxs-lookup"><span data-stu-id="d4037-417">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="d4037-418">Örneğin, VERITABANıNDAKI bir öğrenci satırında iki ilişkili kayıt satırı varsa `Enrollments` gezinti özelliği bu iki `Enrollment` varlığını içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-418">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="d4037-419">İlgili `Enrollment` satırı, `StudentID` sütununda öğrencinin birincil anahtar değerini içeren bir satırdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-419">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="d4037-420">Örneğin, ID = 1 olan öğrencinin `Enrollment` tablosunda iki satır olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="d4037-420">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="d4037-421">`Enrollment` tablosu `StudentID` = 1 olan iki satıra sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d4037-421">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="d4037-422">`StudentID`, `Student` tablosundaki öğrencisi belirten `Enrollment` tablosundaki bir yabancı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-422">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="d4037-423">Bir gezinti özelliği birden çok varlık tutabileceğinden, gezinti özelliği `ICollection<T>`gibi bir liste türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-423">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="d4037-424">`ICollection<T>` belirtilebilir veya `List<T>` veya `HashSet<T>`gibi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-424">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="d4037-425">`ICollection<T>` kullanıldığında EF Core varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-425">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="d4037-426">Birden çok varlık tutun Gezinti özellikleri çoktan çoğa ve bire çok ilişkileri gelir.</span><span class="sxs-lookup"><span data-stu-id="d4037-426">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="d4037-427">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-427">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="d4037-429">*Modeller* klasöründe, aşağıdaki kodla *enrollment.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-429">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="d4037-430">`EnrollmentID` özelliği birincil anahtardır.</span><span class="sxs-lookup"><span data-stu-id="d4037-430">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="d4037-431">Bu varlık `Student` varlığı gibi `ID` yerine `classnameID` modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-431">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="d4037-432">Genellikle geliştiriciler bir düzen seçin ve veri modelini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4037-432">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="d4037-433">Bir sonraki öğreticide, classname Kimliğini kullanarak, veri modelinde aktarma uygulamak daha kolay hale getirmek için gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-433">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="d4037-434">`Grade` özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="d4037-434">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d4037-435">`Grade` türü bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4037-435">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="d4037-436">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="d4037-436">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d4037-437">`StudentID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Student`.</span><span class="sxs-lookup"><span data-stu-id="d4037-437">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d4037-438">`Enrollment` varlık bir `Student` varlığıyla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-438">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="d4037-439">`Student` varlığı, birden çok `Enrollment` varlığı içeren `Student.Enrollments` gezinti özelliğinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-439">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="d4037-440">`CourseID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Course`.</span><span class="sxs-lookup"><span data-stu-id="d4037-440">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d4037-441">Bir `Enrollment` varlığı bir `Course` varlığıyla ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-441">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d4037-442">EF Core, `<navigation property name><primary key property name>`adında bir özelliği yabancı anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-442">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d4037-443">Örneğin, `Student` varlığının birincil anahtarı `ID`olduğundan, `Student` gezinti özelliği için`StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d4037-443">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d4037-444">Yabancı anahtar özelliklerine de `<primary key property name>`adı verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-444">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d4037-445">Örneğin, `Course` varlığın birincil anahtarı `CourseID`olduğundan `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d4037-445">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="d4037-446">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="d4037-446">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="d4037-448">*Modeller* klasöründe, aşağıdaki kodla *Course.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d4037-448">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="d4037-449">`Enrollments` özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="d4037-449">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d4037-450">`Course` bir varlık, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-450">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d4037-451">`DatabaseGenerated` özniteliği, uygulamanın DB oluşturmak yerine birincil anahtarı belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-451">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="d4037-452">İskele Öğrenci modeli</span><span class="sxs-lookup"><span data-stu-id="d4037-452">Scaffold the student model</span></span>

<span data-ttu-id="d4037-453">Bu bölümde, Öğrenci modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="d4037-453">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="d4037-454">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik Öğrenci modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-454">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="d4037-455">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-455">Build the project.</span></span>
* <span data-ttu-id="d4037-456">*Sayfalar/öğrenciler* klasörünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-456">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4037-458">**Çözüm Gezgini**, *sayfalar/öğrenciler* > klasörüne sağ tıklayıp **Yeni > Scalankatan öğe** **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-458">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d4037-459">**Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-459">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="d4037-460">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="d4037-460">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="d4037-461">**Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-461">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="d4037-462">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve üretilen adı **Contosouniversity. modeller. SchoolContext**olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d4037-462">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="d4037-463">**Veri bağlamı sınıfı** açılır penceresinde **Contosouniversity. modeller. SchoolContext** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d4037-463">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="d4037-464">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-464">Select **Add**.</span></span>

![CRUD iletişim](intro/_static/s1.png)

<span data-ttu-id="d4037-466">Önceki adımla ilgili bir sorununuz varsa [Film modeli](xref:tutorials/razor-pages/model#scaffold-the-movie-model) ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="d4037-466">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-467">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-467">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d4037-468">Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-468">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="d4037-469">İskele işlem oluşturulur ve aşağıdaki dosya değişti:</span><span class="sxs-lookup"><span data-stu-id="d4037-469">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="d4037-470">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="d4037-470">Files created</span></span>

* <span data-ttu-id="d4037-471">*Sayfalar/öğrenciler* Oluşturma, silme, ayrıntılar, düzenleme, dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d4037-471">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="d4037-472">*Data/SchoolContext. cs*</span><span class="sxs-lookup"><span data-stu-id="d4037-472">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="d4037-473">Dosya güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="d4037-473">File updates</span></span>

* <span data-ttu-id="d4037-474">*Startup.cs* : Bu dosyadaki değişiklikler sonraki bölümde ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-474">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="d4037-475">*appSettings. JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="d4037-475">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="d4037-476">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="d4037-476">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="d4037-477">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d4037-477">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d4037-478">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-478">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d4037-479">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-479">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d4037-480">Bir db bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d4037-480">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d4037-481">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="d4037-481">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="d4037-482">*Startup.cs*içinde `ConfigureServices` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-482">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="d4037-483">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="d4037-483">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="d4037-484">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-484">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d4037-485">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="d4037-485">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="d4037-486">Ana güncelleştir</span><span class="sxs-lookup"><span data-stu-id="d4037-486">Update main</span></span>

<span data-ttu-id="d4037-487">*Program.cs*' de, aşağıdakileri yapmak için `Main` yöntemini değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-487">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="d4037-488">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="d4037-488">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="d4037-489">Yeniden [oluşturulmasını](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)çağırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-489">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="d4037-490">`EnsureCreated` yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="d4037-490">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="d4037-491">Aşağıdaki kod güncelleştirilmiş *program.cs* dosyasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4037-491">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="d4037-492">`EnsureCreated`, bağlam veritabanının mevcut olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4037-492">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="d4037-493">Varsa, hiçbir işlem yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="d4037-493">If it exists, no action is taken.</span></span> <span data-ttu-id="d4037-494">Yoksa, veritabanı ve tüm şema oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d4037-494">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="d4037-495">`EnsureCreated`, veritabanını oluşturmak için geçişleri kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="d4037-495">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="d4037-496">`EnsureCreated` ile oluşturulan bir veritabanı daha sonra geçişler kullanılarak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="d4037-496">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="d4037-497">`EnsureCreated`, aşağıdaki iş akışına izin veren uygulama başlatma sırasında çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d4037-497">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="d4037-498">DB silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-498">Delete the DB.</span></span>
* <span data-ttu-id="d4037-499">DB şemasını değiştirin (örneğin, bir `EmailAddress` alanı ekleyin).</span><span class="sxs-lookup"><span data-stu-id="d4037-499">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="d4037-500">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4037-500">Run the app.</span></span>
* <span data-ttu-id="d4037-501">`EnsureCreated`,`EmailAddress` sütunuyla bir VERITABANı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-501">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="d4037-502">şema hızlı bir şekilde gelişen zaman geliştirme sürecinde `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="d4037-502">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="d4037-503">Daha sonra öğreticide DB silinir ve geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-503">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d4037-504">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="d4037-504">Test the app</span></span>

<span data-ttu-id="d4037-505">Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="d4037-505">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="d4037-506">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="d4037-506">This app doesn't keep personal information.</span></span> <span data-ttu-id="d4037-507">[Ab genel veri koruma yönetmeliği (GDPR) desteğiyle](xref:security/gdpr)ilgili tanımlama bilgisi İlkesi hakkında bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4037-507">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="d4037-508">**Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d4037-508">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="d4037-509">Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-509">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="d4037-510">SchoolContext DB bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="d4037-510">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="d4037-511">Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="d4037-511">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="d4037-512">Veri bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-512">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d4037-513">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4037-513">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d4037-514">Bu projede, sınıfı `SchoolContext`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-514">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d4037-515">Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-515">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="d4037-516">Vurgulanan kod, her bir varlık kümesi için bir [Dbset\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-516">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="d4037-517">EF Core terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="d4037-517">In EF Core terminology:</span></span>

* <span data-ttu-id="d4037-518">Bir varlık kümesini genellikle DB tabloya karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d4037-518">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="d4037-519">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d4037-519">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d4037-520">`DbSet<Enrollment>` ve `DbSet<Course>` atlanamaz.</span><span class="sxs-lookup"><span data-stu-id="d4037-520">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="d4037-521">EF Core, `Student` varlık `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan bunları örtülü olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-521">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="d4037-522">Bu öğretici için `SchoolContext``DbSet<Enrollment>` ve `DbSet<Course>` tutun.</span><span class="sxs-lookup"><span data-stu-id="d4037-522">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="d4037-523">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d4037-523">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d4037-524">Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4037-524">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="d4037-525">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d4037-525">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d4037-526">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="d4037-526">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="d4037-527">Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* DB dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-527">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="d4037-528">Bir veritabanı test verileri ile başlatmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="d4037-528">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="d4037-529">EF Core boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-529">EF Core creates an empty DB.</span></span> <span data-ttu-id="d4037-530">Bu bölümde, test verileriyle doldurmak için bir `Initialize` yöntemi yazılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-530">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="d4037-531">*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d4037-531">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="d4037-532">Note: Yukarıdaki kod `Data`yerine ad alanı (`namespace ContosoUniversity.Models`) için `Models` kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4037-532">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="d4037-533">`Models`, scaffolder tarafından oluşturulan kodla tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="d4037-533">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="d4037-534">Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="d4037-534">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="d4037-535">Kod DB'de tüm Öğrenciler olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d4037-535">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="d4037-536">DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-536">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="d4037-537">Performansı iyileştirmek için test verilerini `List<T>` koleksiyonlar yerine dizilere yükler.</span><span class="sxs-lookup"><span data-stu-id="d4037-537">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="d4037-538">`EnsureCreated` yöntemi DB bağlamı için otomatik olarak DB oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-538">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="d4037-539">VERITABANı varsa, `EnsureCreated` DB 'yi değiştirmeden döndürür.</span><span class="sxs-lookup"><span data-stu-id="d4037-539">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="d4037-540">*Program.cs*içinde, `Main` yöntemini `Initialize`çağrısı olacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d4037-540">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studio"></a>[<span data-ttu-id="d4037-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4037-541">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d4037-542">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d4037-542">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4037-543">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4037-543">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4037-544">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="d4037-544">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="d4037-545">DB görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="d4037-545">View the DB</span></span>

<span data-ttu-id="d4037-546">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d4037-546">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="d4037-547">Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d4037-547">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="d4037-548">GUID her kullanıcı için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d4037-548">The GUID will be different for each user.</span></span>
<span data-ttu-id="d4037-549">Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="d4037-549">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="d4037-550">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-550">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="d4037-551">**Tables** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="d4037-551">Expand the **Tables** node.</span></span>

<span data-ttu-id="d4037-552">Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-552">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="d4037-553">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="d4037-553">Asynchronous code</span></span>

<span data-ttu-id="d4037-554">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="d4037-554">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d4037-555">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-555">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d4037-556">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="d4037-556">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d4037-557">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="d4037-557">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d4037-558">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="d4037-558">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d4037-559">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-559">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="d4037-560">Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar.</span><span class="sxs-lookup"><span data-stu-id="d4037-560">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d4037-561">Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="d4037-561">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d4037-562">Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` metodu kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="d4037-562">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="d4037-563">`async` anahtar sözcüğü derleyiciye şunu söyler:</span><span class="sxs-lookup"><span data-stu-id="d4037-563">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="d4037-564">Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4037-564">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d4037-565">Döndürülen [görev](/dotnet/api/system.threading.tasks.task) nesnesini otomatik olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d4037-565">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="d4037-566">Daha fazla bilgi için bkz. [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="d4037-566">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="d4037-567">Örtük dönüş türü `Task` devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d4037-567">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="d4037-568">`await` anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d4037-568">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d4037-569">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="d4037-569">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="d4037-570">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4037-570">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="d4037-571">`ToListAsync`, `ToList` uzantısı yönteminin zaman uyumsuz sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="d4037-571">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d4037-572">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d4037-572">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d4037-573">Sorguları veya Veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d4037-573">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="d4037-574">Bu, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`ve `SaveChangesAsync`içerir.</span><span class="sxs-lookup"><span data-stu-id="d4037-574">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d4037-575">`var students = context.Students.Where(s => s.LastName == "Davolio")`gibi yalnızca bir `IQueryable`değiştiren deyimler içermez.</span><span class="sxs-lookup"><span data-stu-id="d4037-575">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="d4037-576">EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="d4037-576">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="d4037-577">Zaman uyumsuz kodun performans avantajlarından yararlanmak için bunlar için bir veritabanı sorguları göndermek EF Core yöntemleri çağırırsanız kitaplığı paketlerinin (disk belleği sunamıyoruz gibi) zaman uyumsuz kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d4037-577">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="d4037-578">.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama</span><span class="sxs-lookup"><span data-stu-id="d4037-578">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="d4037-579">Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.</span><span class="sxs-lookup"><span data-stu-id="d4037-579">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="d4037-580">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d4037-580">Additional resources</span></span>

* [<span data-ttu-id="d4037-581">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="d4037-581">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="d4037-582">Next</span><span class="sxs-lookup"><span data-stu-id="d4037-582">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
