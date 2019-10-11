---
title: ASP.NET Core Entity Framework Core ile Razor Pages-1-8 öğretici
author: rick-anderson
description: Entity Framework Core kullanarak Razor Pages uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 01e507326ddd57057aa178ad3909fd4027a013fd
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259368"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="de0a1-103">ASP.NET Core Entity Framework Core ile Razor Pages-1-8 öğretici</span><span class="sxs-lookup"><span data-stu-id="de0a1-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="de0a1-104">, [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="de0a1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="de0a1-105">Bu, bir [ASP.NET Core Razor Pages](xref:razor-pages/index) uygulamasında ENTITY Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="de0a1-106">Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="de0a1-107">Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="de0a1-108">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="de0a1-109">[Yönergeleri indirin](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="de0a1-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de0a1-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="de0a1-110">Prerequisites</span></span>

* <span data-ttu-id="de0a1-111">Razor Pages yeni başladıysanız, bu duruma başlamadan önce Razor Pages öğretici serisini [kullanmaya](xref:tutorials/razor-pages/razor-pages-start) başlayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="de0a1-114">Veritabanı altyapıları</span><span class="sxs-lookup"><span data-stu-id="de0a1-114">Database engines</span></span>

<span data-ttu-id="de0a1-115">Visual Studio yönergeleri, yalnızca Windows üzerinde çalışan bir SQL Server Express sürümü olan [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="de0a1-116">Visual Studio Code yönergeler, platformlar arası bir veritabanı altyapısı olan [SQLite](https://www.sqlite.org/)kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="de0a1-117">SQLite kullanmayı seçerseniz, SQLite [Için DB tarayıcısı](https://sqlitebrowser.org/)gibi bir SQLite veritabanını yönetmek ve görüntülemek için üçüncü taraf bir araç indirip yükleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="de0a1-118">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="de0a1-118">Troubleshooting</span></span>

<span data-ttu-id="de0a1-119">Giderebileceğiniz bir sorunla karşılaşırsanız, kodunuzu [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="de0a1-120">Yardım almanın iyi bir yolu, [ASP.NET Core etiketi](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core etiketi](https://stackoverflow.com/questions/tagged/entity-framework-core)kullanılarak StackOverflow.com 'e bir soru göndererek.</span><span class="sxs-lookup"><span data-stu-id="de0a1-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="de0a1-121">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="de0a1-121">The sample app</span></span>

<span data-ttu-id="de0a1-122">Bu öğreticilerde oluşturulan uygulama, temel bir üniversite web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="de0a1-123">Kullanıcılar öğrenci, kurs ve eğitmen bilgilerini görüntüleyebilir ve güncelleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="de0a1-124">Öğreticide oluşturulan ekranlardan bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-124">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler Dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

<span data-ttu-id="de0a1-127">Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="de0a1-128">Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="de0a1-129">Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="de0a1-130">*Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="de0a1-131">1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de0a1-133">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="de0a1-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="de0a1-134">Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="de0a1-135">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-135">Build the project.</span></span>
* <span data-ttu-id="de0a1-136">Paket Yöneticisi konsolu 'nda (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="de0a1-137">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de0a1-139">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="de0a1-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="de0a1-140">*Contosouniversity. csproj*öğesini silin ve *Contosoüniversıtysqlite. csproj* öğesini *contosouniversity. csproj*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="de0a1-141">*Startup.cs*silin ve *StartupSQLite.cs* öğesini *Startup.cs*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="de0a1-142">*AppSettings. JSON*öğesini silin ve *Appsettingssqlite. JSON* öğesini *appSettings. JSON*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="de0a1-143">*Geçişler* klasörünü silin ve *migrationssql* öğesini *geçişlerle*yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="de0a1-144">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-144">Build the project.</span></span>
* <span data-ttu-id="de0a1-145">Proje klasöründeki bir komut isteminde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* <span data-ttu-id="de0a1-146">SQLite aracında şu SQL ifadesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="de0a1-147">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="de0a1-148">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="de0a1-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-150">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="de0a1-151">**ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="de0a1-152">Projeyi *Contosouniversity*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="de0a1-153">Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="de0a1-154">Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de0a1-156">Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="de0a1-157">Bir Razor Pages projesi oluşturmak için aşağıdaki komutları çalıştırın ve yeni proje klasörüne `cd`:</span><span class="sxs-lookup"><span data-stu-id="de0a1-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="de0a1-158">Site stilini ayarlayın</span><span class="sxs-lookup"><span data-stu-id="de0a1-158">Set up the site style</span></span>

<span data-ttu-id="de0a1-159">*Sayfaları/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="de0a1-160">"ContosoUniversity" öğesinin her oluşumunu "Contoso Üniversitesi" olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="de0a1-161">Üç oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-161">There are three occurrences.</span></span>

* <span data-ttu-id="de0a1-162">**Giriş** ve **Gizlilik** menü girişlerini silin ve **hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için girişler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="de0a1-163">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="de0a1-164">*Pages/Index. cshtml*dosyasında, ASP.NET Core hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="de0a1-165">Giriş sayfasının göründüğünü doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="de0a1-166">Veri modeli</span><span class="sxs-lookup"><span data-stu-id="de0a1-166">The data model</span></span>

<span data-ttu-id="de0a1-167">Aşağıdaki bölümler bir veri modeli oluşturur:</span><span class="sxs-lookup"><span data-stu-id="de0a1-167">The following sections create a data model:</span></span>

![Kurs-kayıt-öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="de0a1-169">Bir öğrenci herhangi bir sayıda kursa kaydolabilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="de0a1-170">Öğrenci varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-170">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

* <span data-ttu-id="de0a1-172">Proje klasöründe bir *modeller* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="de0a1-173">Aşağıdaki kodla *modeller/öğrenci. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="de0a1-174">@No__t-0 özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="de0a1-175">Varsayılan olarak, EF Core `ID` veya `classnameID` adlı bir özelliği birincil anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="de0a1-176">Bu nedenle, `Student` sınıfı birincil anahtarı için otomatik olarak tanınan ad `StudentID` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="de0a1-177">@No__t-0 özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="de0a1-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="de0a1-178">Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="de0a1-179">Bu durumda, bir `Student` varlığının `Enrollments` özelliği, söz konusu öğrenciye ilişkin tüm @no__t 2 varlıklarını barındırır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="de0a1-180">Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="de0a1-181">Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="de0a1-182">Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="de0a1-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="de0a1-183">İlgili kayıt satırları Studentitıd = 1 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="de0a1-184">Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* .</span><span class="sxs-lookup"><span data-stu-id="de0a1-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="de0a1-185">@No__t-0 özelliği, birden çok ilgili kayıt varlığı olabileceğinden, `ICollection<Enrollment>` olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="de0a1-186">@No__t-0 veya `HashSet<Enrollment>` gibi diğer koleksiyon türlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="de0a1-187">@No__t-0 kullanıldığında, EF Core varsayılan olarak bir `HashSet<Enrollment>` koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="de0a1-188">Kayıt varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-188">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="de0a1-190">Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="de0a1-191">@No__t-0 özelliği birincil anahtardır; Bu varlık, `ID` yerine `classnameID` modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="de0a1-192">Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="de0a1-193">Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="de0a1-194">@No__t-1 olmadan @no__t kullanılması, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="de0a1-195">@No__t-0 özelliği bir `enum` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="de0a1-196">@No__t-0 tür bildiriminden sonraki soru işareti, `Grade` özelliğinin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="de0a1-197">Null olan bir sınıf, 0 ' dan farklı bir şekilde ayarlanır @ no__t-0null, bir sınıf bilinmiyor veya henüz atanmamış anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="de0a1-198">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Student` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="de0a1-199">@No__t-0 bir varlık bir `Student` varlıkla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="de0a1-200">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Course` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="de0a1-201">@No__t-0 varlığı, bir `Course` varlığıyla ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="de0a1-202">EF Core, `<navigation property name><primary key property name>` olarak adlandırılmışsa bir özelliği yabancı anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="de0a1-203">Örneğin, `StudentID`, `Student` varlığının birincil anahtarı `ID` olduğundan, `Student` gezinti özelliği için yabancı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="de0a1-204">Yabancı anahtar özellikleri, `<primary key property name>` olarak da adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="de0a1-205">Örneğin, `Course` varlığının birincil anahtarı `CourseID` olduğundan `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="de0a1-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="de0a1-206">Kurs varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-206">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="de0a1-208">Aşağıdaki kodla *modeller/kurs. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="de0a1-209">@No__t-0 özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="de0a1-210">@No__t-0 varlığı, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="de0a1-211">@No__t-0 özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="de0a1-212">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="de0a1-213">Yapı iskelesi öğrenci sayfaları</span><span class="sxs-lookup"><span data-stu-id="de0a1-213">Scaffold Student pages</span></span>

<span data-ttu-id="de0a1-214">Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:</span><span class="sxs-lookup"><span data-stu-id="de0a1-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="de0a1-215">EF Core *bağlamı* sınıfı.</span><span class="sxs-lookup"><span data-stu-id="de0a1-215">An EF Core *context* class.</span></span> <span data-ttu-id="de0a1-216">Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="de0a1-217">@No__t-0 sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="de0a1-218">@No__t-0 varlığı için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="de0a1-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-220">*Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="de0a1-221">**Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve **Ekle** > **yeni yapı iskelesi öğesini**seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="de0a1-222">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="de0a1-223">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="de0a1-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="de0a1-224">**Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="de0a1-225">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="de0a1-226">*Contosouniversity. modeller. Contosoüniversıtycontext* olan veri bağlamı adını *Contosouniversity. Data. SchoolContext*olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="de0a1-227">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-227">Select **Add**.</span></span>

<span data-ttu-id="de0a1-228">Aşağıdaki paketler otomatik olarak yüklenir:</span><span class="sxs-lookup"><span data-stu-id="de0a1-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de0a1-230">Gerekli NuGet paketlerini yüklemek için aşağıdaki .NET Core CLI komutlarını çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
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

  <span data-ttu-id="de0a1-231">Yapı iskelesi için Microsoft. VisualStudio. Web. CodeGeneration. Design paketi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="de0a1-232">Uygulama SQL Server kullanmayabilse de, scafkatlama aracı SQL Server paketine ihtiyaç duyuyor.</span><span class="sxs-lookup"><span data-stu-id="de0a1-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="de0a1-233">*Sayfalar/öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="de0a1-234">[ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator)'nı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="de0a1-235">Fkatlama öğrenci sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="de0a1-236">**Windows 'da**</span><span class="sxs-lookup"><span data-stu-id="de0a1-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="de0a1-237">**MacOS veya Linux üzerinde**</span><span class="sxs-lookup"><span data-stu-id="de0a1-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="de0a1-238">Önceki adımla ilgili bir sorununuz varsa, projeyi derleyin ve yapı iskelesi adımını yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="de0a1-239">Yapı iskelesi işlemi:</span><span class="sxs-lookup"><span data-stu-id="de0a1-239">The scaffolding process:</span></span>

* <span data-ttu-id="de0a1-240">*Sayfalar/öğrenciler* klasöründe Razor sayfaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="de0a1-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="de0a1-241">*. Cshtml* ve *Create.cshtml.cs* oluşturma</span><span class="sxs-lookup"><span data-stu-id="de0a1-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="de0a1-242">*Delete. cshtml* ve *delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="de0a1-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="de0a1-243">*Details. cshtml* ve *details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="de0a1-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="de0a1-244">*. Cshtml* ve *Edit.cshtml.cs* Düzenle</span><span class="sxs-lookup"><span data-stu-id="de0a1-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="de0a1-245">*Index. cshtml* ve *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="de0a1-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="de0a1-246">*Data/SchoolContext. cs*oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="de0a1-247">*Startup.cs*içinde bağımlılık eklenmesine bağlam ekler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="de0a1-248">*AppSettings. JSON*öğesine bir veritabanı bağlantı dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="de0a1-249">Veritabanı bağlantı dizesi</span><span class="sxs-lookup"><span data-stu-id="de0a1-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de0a1-251">Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="de0a1-252">LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="de0a1-253">Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de0a1-255">Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="de0a1-256">Veritabanı bağlam sınıfını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="de0a1-256">Update the database context class</span></span>

<span data-ttu-id="de0a1-257">Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="de0a1-258">Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="de0a1-259">Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="de0a1-260">Bu projede, sınıfı `SchoolContext` olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="de0a1-261">Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="de0a1-262">Vurgulanan kod, her bir varlık kümesi için bir [Dbset @ no__t-1TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="de0a1-263">EF Core terminoloji:</span><span class="sxs-lookup"><span data-stu-id="de0a1-263">In EF Core terminology:</span></span>

* <span data-ttu-id="de0a1-264">Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="de0a1-265">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="de0a1-266">Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="de0a1-267">Scafkatlama aracı bir @ no__t-0 DBSet oluşturduğundan, bu adım bunu plural `Students` olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="de0a1-268">Razor Pages kodun yeni DBSet adıyla eşleşmesini sağlamak için, `_context.Student` ' ın tüm projesi genelinde bir global değişiklik yapın `_context.Students`.</span><span class="sxs-lookup"><span data-stu-id="de0a1-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="de0a1-269">8 oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-269">There are 8 occurrences.</span></span>

<span data-ttu-id="de0a1-270">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="de0a1-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="de0a1-271">Startup.cs</span></span>

<span data-ttu-id="de0a1-272">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="de0a1-273">Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="de0a1-274">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="de0a1-275">Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="de0a1-276">Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.</span><span class="sxs-lookup"><span data-stu-id="de0a1-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-278">@No__t-0 ' da, vurgulanan satırlar scaffolder tarafından eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="de0a1-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de0a1-280">@No__t-0 ' da, desteği tarafından eklenen kodun `UseSqlite` ' i çağırıyor olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="de0a1-281">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="de0a1-282">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="de0a1-283">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="de0a1-283">Create the database</span></span>

<span data-ttu-id="de0a1-284">Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="de0a1-285">Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="de0a1-286">Veritabanı yoksa, veritabanını ve şemayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="de0a1-287">`EnsureCreated`, veri modeli değişikliklerini işlemek için aşağıdaki iş akışına izin vermez:</span><span class="sxs-lookup"><span data-stu-id="de0a1-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="de0a1-288">Veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-288">Delete the database.</span></span> <span data-ttu-id="de0a1-289">Mevcut veriler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-289">Any existing data is lost.</span></span>
* <span data-ttu-id="de0a1-290">Veri modelini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-290">Change the data model.</span></span> <span data-ttu-id="de0a1-291">Örneğin, `EmailAddress` alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="de0a1-292">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-292">Run the app.</span></span>
* <span data-ttu-id="de0a1-293">`EnsureCreated`, yeni şemaya sahip bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="de0a1-294">Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="de0a1-295">Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="de0a1-296">Bu durumda, geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="de0a1-297">Öğretici serisinde daha sonra, `EnsureCreated` tarafından oluşturulan veritabanını silin ve bunun yerine geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="de0a1-298">@No__t-0 tarafından oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="de0a1-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="de0a1-299">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="de0a1-299">Test the app</span></span>

* <span data-ttu-id="de0a1-300">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-300">Run the app.</span></span>
* <span data-ttu-id="de0a1-301">**Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.</span><span class="sxs-lookup"><span data-stu-id="de0a1-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="de0a1-302">Düzenleme, Ayrıntılar ve silme bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="de0a1-303">Veritabanını çekirdek</span><span class="sxs-lookup"><span data-stu-id="de0a1-303">Seed the database</span></span>

<span data-ttu-id="de0a1-304">@No__t-0 yöntemi boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="de0a1-305">Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="de0a1-306">Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="de0a1-307">Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="de0a1-308">Öğrenci yoksa, veritabanına test verileri ekler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="de0a1-309">Performansı iyileştirmek için `List<T>` koleksiyonları yerine diziler halinde test verileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="de0a1-310">*Program.cs*' de `EnsureCreated` çağrısını `DbInitializer.Initialize` çağrısıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de0a1-312">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de0a1-314">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="de0a1-315">Uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-315">Restart the app.</span></span>

* <span data-ttu-id="de0a1-316">Sağlanan verileri görmek için öğrenciler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="de0a1-317">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="de0a1-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-319">Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="de0a1-320">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="de0a1-321">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="de0a1-322">**Tables** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="de0a1-323">Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="de0a1-324">@No__t-2 modelinin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de0a1-326">Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="de0a1-327">Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="de0a1-328">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="de0a1-328">Asynchronous code</span></span>

<span data-ttu-id="de0a1-329">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="de0a1-330">Web sunucusunda sınırlı sayıda iş parçacığı bulunur ve yüksek yük durumlarında tüm kullanılabilir iş parçacıkları kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="de0a1-331">Bu durumda, sunucu, iş parçacıkları boşaltılana kadar yeni istekleri işleyemez.</span><span class="sxs-lookup"><span data-stu-id="de0a1-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="de0a1-332">Zaman uyumlu kodla, çok sayıda iş parçacığı, g/ç 'nin tamamlanmasını beklediği için aslında herhangi bir iş yapmadıklarında bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="de0a1-333">Zaman uyumsuz kod ile, bir işlem g/ç 'yi tamamlanmayı beklerken, sunucunun diğer istekleri işlemek için kullanması için iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="de0a1-334">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="de0a1-335">Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="de0a1-336">Düşük trafik durumlarında, performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="de0a1-337">Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="de0a1-338">@No__t-0 anahtar sözcüğü derleyiciye şunu söyler:</span><span class="sxs-lookup"><span data-stu-id="de0a1-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="de0a1-339">Yöntem gövdesinin parçaları için geri çağrılar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="de0a1-340">Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="de0a1-341">@No__t-0 dönüş türü devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="de0a1-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="de0a1-342">@No__t-0 anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="de0a1-343">İlk bölüm, zaman uyumsuz olarak başlatılan işlemle biter.</span><span class="sxs-lookup"><span data-stu-id="de0a1-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="de0a1-344">İkinci bölüm, işlem tamamlandığında çağrılan bir geri çağırma yöntemine konur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="de0a1-345">`ToListAsync`, `ToList` Genişletme yönteminin zaman uyumsuz sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="de0a1-346">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı şeyler:</span><span class="sxs-lookup"><span data-stu-id="de0a1-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="de0a1-347">Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="de0a1-348">Bu `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` ve `SaveChangesAsync` içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="de0a1-349">Yalnızca `var students = context.Students.Where(s => s.LastName == "Davolio")` gibi @no__t (0) değiştiren deyimler içermez.</span><span class="sxs-lookup"><span data-stu-id="de0a1-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="de0a1-350">EF Core bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="de0a1-351">Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="de0a1-352">.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama</span><span class="sxs-lookup"><span data-stu-id="de0a1-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="de0a1-353">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="de0a1-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="de0a1-354">Sonraki öğretici</span><span class="sxs-lookup"><span data-stu-id="de0a1-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="de0a1-355">Contoso Üniversitesi örnek Web uygulaması, Entity Framework (EF) çekirdeğini kullanarak bir ASP.NET Core Razor Pages uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="de0a1-356">Örnek uygulama, kurgusal bir Contoso Üniversitesi için bir Web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="de0a1-357">Öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="de0a1-358">Bu sayfa, Contoso Üniversitesi örnek uygulamasının nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="de0a1-359">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="de0a1-360">[Yönergeleri indirin](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="de0a1-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de0a1-361">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="de0a1-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="de0a1-364">[Razor Pages](xref:razor-pages/index)hakkında benzerlik.</span><span class="sxs-lookup"><span data-stu-id="de0a1-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="de0a1-365">Yeni programcılar, bu seriyi başlatmadan önce [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) ' i tamamlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="de0a1-366">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="de0a1-366">Troubleshooting</span></span>

<span data-ttu-id="de0a1-367">Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="de0a1-368">[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) [için bir](https://stackoverflow.com/questions/tagged/asp.net-core) soru göndererek yardım almanın iyi bir yolu.</span><span class="sxs-lookup"><span data-stu-id="de0a1-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="de0a1-369">Contoso Üniversitesi web uygulaması</span><span class="sxs-lookup"><span data-stu-id="de0a1-369">The Contoso University web app</span></span>

<span data-ttu-id="de0a1-370">Bu öğreticilerde oluşturulan uygulama, temel bir üniversite web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="de0a1-371">Kullanıcılar öğrenci, kurs ve eğitmen bilgilerini görüntüleyebilir ve güncelleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="de0a1-372">Öğreticide oluşturulan ekranlardan bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-372">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler Dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="de0a1-375">Bu sitenin kullanıcı arabirimi stili yerleşik şablonlar tarafından üretilme kadar yakın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="de0a1-376">Öğretici odağı, kullanıcı arabiriminden değil, Razor Pages EF Core.</span><span class="sxs-lookup"><span data-stu-id="de0a1-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="de0a1-377">ContosoUniversity Razor Pages Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="de0a1-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-379">Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="de0a1-380">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="de0a1-381">Projeyi **Contosouniversity**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="de0a1-382">Kod kopyalama/yapıştırma olduğunda, ad alanlarının eşleşmesi için *Contosouniversity* projesini adlandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="de0a1-383">Açılan listede **ASP.NET Core 2,1** ' i seçin ve ardından **Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="de0a1-384">Yukarıdaki adımların görüntüleri için bkz. [Razor Web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="de0a1-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="de0a1-385">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="de0a1-387">Site stilini ayarlayın</span><span class="sxs-lookup"><span data-stu-id="de0a1-387">Set up the site style</span></span>

<span data-ttu-id="de0a1-388">Site menüsünü, düzeni ve giriş sayfasını birkaç değişiklik ayarlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="de0a1-389">*Pages/Shared/_Layout. cshtml* dosyasını aşağıdaki değişikliklerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="de0a1-390">"ContosoUniversity" öğesinin her oluşumunu "Contoso Üniversitesi" olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="de0a1-391">Üç oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-391">There are three occurrences.</span></span>

* <span data-ttu-id="de0a1-392">**Öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **kişi** menü girişini silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="de0a1-393">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-393">The changes are highlighted.</span></span> <span data-ttu-id="de0a1-394">(Tüm *biçimlendirme gösterilmez.* )</span><span class="sxs-lookup"><span data-stu-id="de0a1-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="de0a1-395">*Pages/Index. cshtml*dosyasında, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="de0a1-396">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="de0a1-396">Create the data model</span></span>

<span data-ttu-id="de0a1-397">Contoso Üniversitesi uygulaması için varlık sınıfları oluşturma.</span><span class="sxs-lookup"><span data-stu-id="de0a1-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="de0a1-398">Aşağıdaki üç varlıkla başlayın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-398">Start with the following three entities:</span></span>

![Kurs-kayıt-öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="de0a1-400">@No__t-0 ve `Enrollment` varlıkları arasında bire çok ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="de0a1-401">@No__t-0 ve `Enrollment` varlıkları arasında bire çok ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="de0a1-402">Bir öğrenci herhangi bir sayıda kursa kaydolabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="de0a1-403">Bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="de0a1-404">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="de0a1-405">Öğrenci varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-405">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="de0a1-407">*Modeller* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-407">Create a *Models* folder.</span></span> <span data-ttu-id="de0a1-408">*Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyasını aşağıdaki kodla oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="de0a1-409">@No__t-0 özelliği, bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu olur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="de0a1-410">Varsayılan olarak, EF Core `ID` veya `classnameID` adlı bir özelliği birincil anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="de0a1-411">@No__t-0 ' da, `classname` ' in sınıfın adıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="de0a1-412">Diğer otomatik olarak tanınan birincil anahtar, önceki örnekte `StudentID` ' dır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="de0a1-413">@No__t-0 özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="de0a1-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="de0a1-414">Gezinti özellikleri bu varlıkla ilgili diğer varlıkların bağlantısını sağlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="de0a1-415">Bu durumda, bir `Student entity` ' in `Enrollments` özelliği, bu `Student` ile ilgili @no__t 2 varlıkların tümünü barındırır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="de0a1-416">Örneğin, VERITABANıNDAKI bir öğrenci satırında iki ilişkili kayıt satırı varsa `Enrollments` gezinti özelliği bu iki `Enrollment` varlığını içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="de0a1-417">İlgili `Enrollment` satırı, `StudentID` sütununda öğrencinin birincil anahtar değerini içeren bir satırdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="de0a1-418">Örneğin, ID = 1 olan öğrencinin `Enrollment` tablosunda iki satıra sahip olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="de0a1-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="de0a1-419">@No__t-0 tablosunda `StudentID` = 1 olan iki satır vardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="de0a1-420">`StudentID`, `Student` tablosunda öğrenci belirten `Enrollment` tablosundaki bir yabancı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="de0a1-421">Bir gezinti özelliği birden çok varlık tutabileceğinden, gezinti özelliği `ICollection<T>` gibi bir liste türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="de0a1-422">`ICollection<T>` veya `List<T>` veya `HashSet<T>` gibi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="de0a1-423">@No__t-0 kullanıldığında, EF Core varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="de0a1-424">Birden çok varlığı tutan gezinti özellikleri, çoktan çoğa ve bire çok ilişkilerden gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="de0a1-425">Kayıt varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-425">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="de0a1-427">*Modeller* klasöründe, aşağıdaki kodla *enrollment.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="de0a1-428">@No__t-0 özelliği birincil anahtardır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="de0a1-429">Bu varlık `Student` varlığı gibi `ID` yerine `classnameID` modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="de0a1-430">Genellikle geliştiriciler bir model seçer ve bunu veri modeli boyunca kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="de0a1-431">Daha sonraki bir öğreticide, veri modelinde devralmayı daha kolay hale getirmek için ClassName olmadan ID kullanımı gösterilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="de0a1-432">@No__t-0 özelliği bir `enum` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="de0a1-433">@No__t-0 tür bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="de0a1-434">Null olan bir sınıf sıfır bir sınıf ile farklıdır--null, henüz bir sınıf bilinmediğini veya henüz atanmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="de0a1-435">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Student` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="de0a1-436">@No__t-0 bir varlık bir `Student` varlıkla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="de0a1-437">@No__t-0 varlığı, birden çok `Enrollment` varlık içeren `Student.Enrollments` gezinti özelliğinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="de0a1-438">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Course` ' dir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="de0a1-439">@No__t-0 varlığı, bir `Course` varlığıyla ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="de0a1-440">EF Core, `<navigation property name><primary key property name>` olarak adlandırılmışsa bir özelliği yabancı anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="de0a1-441">Örneğin, `Student` varlığının birincil anahtarı `ID` olduğundan, `Student` gezinti özelliği için `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="de0a1-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="de0a1-442">Yabancı anahtar özellikleri, `<primary key property name>` olarak da adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="de0a1-443">Örneğin, `Course` varlığının birincil anahtarı `CourseID` olduğundan `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="de0a1-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="de0a1-444">Kurs varlığı</span><span class="sxs-lookup"><span data-stu-id="de0a1-444">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="de0a1-446">*Modeller* klasöründe, aşağıdaki kodla *Course.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="de0a1-447">@No__t-0 özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="de0a1-448">@No__t-0 varlığı, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="de0a1-449">@No__t-0 özniteliği, uygulamanın DB 'nin oluşturmasını sağlamak yerine birincil anahtarı belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="de0a1-450">Öğrenci modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="de0a1-450">Scaffold the student model</span></span>

<span data-ttu-id="de0a1-451">Bu bölümde öğrenci modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="de0a1-452">Diğer bir deyişle, scafkatlama aracı öğrenci modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="de0a1-453">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-453">Build the project.</span></span>
* <span data-ttu-id="de0a1-454">*Sayfalar/öğrenciler* klasörünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de0a1-456">**Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayarak > **yeni yapı iskelesi** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="de0a1-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="de0a1-457">**Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="de0a1-458">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="de0a1-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="de0a1-459">**Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="de0a1-460">**Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve üretilen adı **Contosouniversity. modeller. SchoolContext**olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="de0a1-461">**Veri bağlamı sınıfı** açılır penceresinde **Contosouniversity. modeller. SchoolContext** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="de0a1-462">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-462">Select **Add**.</span></span>

![CRUD iletişim kutusu](intro/_static/s1.png)

<span data-ttu-id="de0a1-464">Önceki adımla ilgili bir sorununuz varsa [Film modeli](xref:tutorials/razor-pages/model#scaffold-the-movie-model) ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de0a1-466">Öğrenci modelini iskele almak için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="de0a1-467">Yapı iskelesi işlemi oluşturulur ve aşağıdaki dosyaları değiştirdi:</span><span class="sxs-lookup"><span data-stu-id="de0a1-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="de0a1-468">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="de0a1-468">Files created</span></span>

* <span data-ttu-id="de0a1-469">*Sayfalar/öğrenciler* Oluşturma, silme, ayrıntılar, düzenleme, dizin oluşturma.</span><span class="sxs-lookup"><span data-stu-id="de0a1-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="de0a1-470">*Data/SchoolContext. cs*</span><span class="sxs-lookup"><span data-stu-id="de0a1-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="de0a1-471">Dosya güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="de0a1-471">File updates</span></span>

* <span data-ttu-id="de0a1-472">*Startup.cs* : Bu dosyadaki değişiklikler sonraki bölümde ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="de0a1-473">*appSettings. JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="de0a1-474">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="de0a1-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="de0a1-475">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="de0a1-476">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="de0a1-477">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="de0a1-478">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="de0a1-479">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.</span><span class="sxs-lookup"><span data-stu-id="de0a1-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="de0a1-480">*Startup.cs*içinde `ConfigureServices` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="de0a1-481">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="de0a1-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="de0a1-482">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="de0a1-483">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="de0a1-484">Ana güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="de0a1-484">Update main</span></span>

<span data-ttu-id="de0a1-485">*Program.cs*' de `Main` yöntemini aşağıdaki şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="de0a1-486">Bağımlılık ekleme kapsayıcısından bir DB bağlam örneği alın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="de0a1-487">Yeniden [oluşturulmasını](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)çağırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="de0a1-488">@No__t-0 yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="de0a1-489">Aşağıdaki kod güncelleştirilmiş *program.cs* dosyasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="de0a1-490">`EnsureCreated`, bağlam veritabanının mevcut olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="de0a1-491">Varsa, hiçbir eylem yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-491">If it exists, no action is taken.</span></span> <span data-ttu-id="de0a1-492">Yoksa, veritabanı ve tüm şeması oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="de0a1-493">`EnsureCreated`, veritabanını oluşturmak için geçişleri kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="de0a1-494">@No__t-0 ile oluşturulan bir veritabanı daha sonra geçişler kullanılarak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="de0a1-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="de0a1-495">`EnsureCreated`, uygulama başlatma sırasında çağrılır ve bu, aşağıdaki iş akışına izin verir:</span><span class="sxs-lookup"><span data-stu-id="de0a1-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="de0a1-496">VERITABANıNı silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-496">Delete the DB.</span></span>
* <span data-ttu-id="de0a1-497">DB şemasını değiştirin (örneğin, `EmailAddress` alanı ekleyin).</span><span class="sxs-lookup"><span data-stu-id="de0a1-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="de0a1-498">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-498">Run the app.</span></span>
* <span data-ttu-id="de0a1-499">`EnsureCreated`, @ no__t-1 sütunuyla bir VERITABANı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="de0a1-500">şema hızlı bir şekilde gelişmede `EnsureCreated`, geliştirmede daha erken bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="de0a1-501">Öğreticide daha sonra DB silinir ve geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="de0a1-502">Uygulamayı test edin</span><span class="sxs-lookup"><span data-stu-id="de0a1-502">Test the app</span></span>

<span data-ttu-id="de0a1-503">Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="de0a1-504">Bu uygulama, kişisel bilgileri saklar.</span><span class="sxs-lookup"><span data-stu-id="de0a1-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="de0a1-505">[Ab genel veri koruma yönetmeliği (GDPR) desteğiyle](xref:security/gdpr)ilgili tanımlama bilgisi İlkesi hakkında bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="de0a1-506">**Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.</span><span class="sxs-lookup"><span data-stu-id="de0a1-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="de0a1-507">Düzenleme, Ayrıntılar ve silme bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="de0a1-508">SchoolContext DB bağlamını inceleyin</span><span class="sxs-lookup"><span data-stu-id="de0a1-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="de0a1-509">Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf DB bağlam sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="de0a1-510">Veri bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="de0a1-511">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="de0a1-512">Bu projede, sınıfı `SchoolContext` olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="de0a1-513">Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="de0a1-514">Vurgulanan kod, her bir varlık kümesi için bir [Dbset @ no__t-1TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="de0a1-515">EF Core terminoloji:</span><span class="sxs-lookup"><span data-stu-id="de0a1-515">In EF Core terminology:</span></span>

* <span data-ttu-id="de0a1-516">Bir varlık kümesi, genellikle bir DB tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="de0a1-517">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="de0a1-518">`DbSet<Enrollment>` ve `DbSet<Course>` atlanamaz.</span><span class="sxs-lookup"><span data-stu-id="de0a1-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="de0a1-519">EF Core, `Student` varlığı `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan bunları örtülü olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="de0a1-520">Bu öğreticide `DbSet<Enrollment>` ve `DbSet<Course>` ' i `SchoolContext` ' de tutun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="de0a1-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="de0a1-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="de0a1-522">Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="de0a1-523">LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="de0a1-524">LocalDB, istek üzerine başlar ve kullanıcı modunda çalışır, bu nedenle karmaşık bir yapılandırma yoktur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="de0a1-525">Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* DB dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="de0a1-526">Test verileriyle VERITABANıNı başlatmak için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="de0a1-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="de0a1-527">EF Core boş bir VERITABANı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="de0a1-528">Bu bölümde, test verileriyle doldurmak için bir `Initialize` yöntemi yazılır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="de0a1-529">*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="de0a1-530">Note: Yukarıdaki kod `Data` yerine ad alanı için (`namespace ContosoUniversity.Models`) `Models` kullanır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="de0a1-531">`Models`, scaffolder tarafından oluşturulan kodla tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="de0a1-532">Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="de0a1-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="de0a1-533">Kod, VERITABANıNDA herhangi bir öğrenci olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="de0a1-534">Veritabanında hiç öğrenci yoksa, DB test verileriyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="de0a1-535">Performansı iyileştirmek için, test verilerini `List<T>` koleksiyonları yerine dizilere yükler.</span><span class="sxs-lookup"><span data-stu-id="de0a1-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="de0a1-536">@No__t-0 yöntemi DB bağlamı için otomatik olarak DB oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="de0a1-537">VERITABANı varsa, DB 'yi değiştirmeden `EnsureCreated` döndürür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="de0a1-538">*Program.cs*' de `Main` yöntemini `Initialize` ' i çağırmak üzere değiştirin:</span><span class="sxs-lookup"><span data-stu-id="de0a1-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de0a1-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de0a1-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de0a1-540">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de0a1-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de0a1-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de0a1-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de0a1-542">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="de0a1-543">VERITABANıNı görüntüleme</span><span class="sxs-lookup"><span data-stu-id="de0a1-543">View the DB</span></span>

<span data-ttu-id="de0a1-544">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="de0a1-545">Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="de0a1-546">GUID her kullanıcı için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="de0a1-547">Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="de0a1-548">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="de0a1-549">**Tables** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="de0a1-550">Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="de0a1-551">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="de0a1-551">Asynchronous code</span></span>

<span data-ttu-id="de0a1-552">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="de0a1-553">Web sunucusunda sınırlı sayıda iş parçacığı bulunur ve yüksek yük durumlarında tüm kullanılabilir iş parçacıkları kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="de0a1-554">Bu durumda, sunucu, iş parçacıkları boşaltılana kadar yeni istekleri işleyemez.</span><span class="sxs-lookup"><span data-stu-id="de0a1-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="de0a1-555">Zaman uyumlu kodla, çok sayıda iş parçacığı, g/ç 'nin tamamlanmasını beklediği için aslında herhangi bir iş yapmadıklarında bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="de0a1-556">Zaman uyumsuz kod ile, bir işlem g/ç 'yi tamamlanmayı beklerken, sunucunun diğer istekleri işlemek için kullanması için iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="de0a1-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="de0a1-557">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu, gecikme olmadan daha fazla trafiği işlemeye etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="de0a1-558">Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="de0a1-559">Düşük trafik durumlarında, performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="de0a1-560">Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="de0a1-561">@No__t-0 anahtar sözcüğü derleyiciye şunu söyler:</span><span class="sxs-lookup"><span data-stu-id="de0a1-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="de0a1-562">Yöntem gövdesinin parçaları için geri çağrılar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="de0a1-563">Döndürülen [görev](/dotnet/api/system.threading.tasks.task) nesnesini otomatik olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="de0a1-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="de0a1-564">Daha fazla bilgi için bkz. [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="de0a1-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="de0a1-565">@No__t-0 örtülü dönüş türü, devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="de0a1-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="de0a1-566">@No__t-0 anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="de0a1-567">İlk bölüm, zaman uyumsuz olarak başlatılan işlemle biter.</span><span class="sxs-lookup"><span data-stu-id="de0a1-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="de0a1-568">İkinci bölüm, işlem tamamlandığında çağrılan bir geri çağırma yöntemine konur.</span><span class="sxs-lookup"><span data-stu-id="de0a1-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="de0a1-569">`ToListAsync`, `ToList` Genişletme yönteminin zaman uyumsuz sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="de0a1-570">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı şeyler:</span><span class="sxs-lookup"><span data-stu-id="de0a1-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="de0a1-571">Yalnızca sorguları veya komutlarının VERITABANıNA gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="de0a1-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="de0a1-572">Bu, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` ve `SaveChangesAsync` içerir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="de0a1-573">Yalnızca `var students = context.Students.Where(s => s.LastName == "Davolio")` gibi @no__t (0) değiştiren deyimler içermez.</span><span class="sxs-lookup"><span data-stu-id="de0a1-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="de0a1-574">EF Core bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.</span><span class="sxs-lookup"><span data-stu-id="de0a1-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="de0a1-575">Zaman uyumsuz kodun performans avantajlarından yararlanmak için, VERITABANıNA sorgu gönderen EF Core yöntemlerini çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="de0a1-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="de0a1-576">.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama</span><span class="sxs-lookup"><span data-stu-id="de0a1-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="de0a1-577">Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemleri incelenir.</span><span class="sxs-lookup"><span data-stu-id="de0a1-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="de0a1-578">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="de0a1-578">Additional resources</span></span>

* [<span data-ttu-id="de0a1-579">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="de0a1-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="de0a1-580">İleri</span><span class="sxs-lookup"><span data-stu-id="de0a1-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
