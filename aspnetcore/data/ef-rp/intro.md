---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: tdykstra
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: e261ccd30dc9ef0929e74fa44a5ed752d515b707
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317679"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="a8db6-103">ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="a8db6-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="a8db6-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a8db6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8db6-105">Bu, bir [ASP.NET Core Razor Pages](xref:razor-pages/index) uygulamasında ENTITY Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="a8db6-106">Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a8db6-107">Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="a8db6-108">İndirme veya tamamlanmış uygulamayı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a8db6-109">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a8db6-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8db6-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a8db6-110">Prerequisites</span></span>

* <span data-ttu-id="a8db6-111">Razor Pages yeni başladıysanız, bu duruma başlamadan önce Razor Pages öğretici serisini [kullanmaya](xref:tutorials/razor-pages/razor-pages-start) başlayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="a8db6-114">Veritabanı altyapıları</span><span class="sxs-lookup"><span data-stu-id="a8db6-114">Database engines</span></span>

<span data-ttu-id="a8db6-115">Visual Studio yönergeleri, yalnızca Windows üzerinde çalışan bir SQL Server Express sürümü olan [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="a8db6-116">Visual Studio Code yönergeler, platformlar arası bir veritabanı altyapısı olan [SQLite](https://www.sqlite.org/)kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="a8db6-117">SQLite kullanmayı seçerseniz, SQLite [Için DB tarayıcısı](https://sqlitebrowser.org/)gibi bir SQLite veritabanını yönetmek ve görüntülemek için üçüncü taraf bir araç indirip yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a8db6-118">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="a8db6-118">Troubleshooting</span></span>

<span data-ttu-id="a8db6-119">Giderebileceğiniz bir sorunla karşılaşırsanız, kodunuzu [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="a8db6-120">Yardım almanın iyi bir yolu, [ASP.NET Core etiketi](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core etiketi](https://stackoverflow.com/questions/tagged/entity-framework-core)kullanılarak StackOverflow.com 'e bir soru göndererek.</span><span class="sxs-lookup"><span data-stu-id="a8db6-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="a8db6-121">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="a8db6-121">The sample app</span></span>

<span data-ttu-id="a8db6-122">Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="a8db6-123">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a8db6-124">Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-124">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

<span data-ttu-id="a8db6-127">Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="a8db6-128">Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="a8db6-129">Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="a8db6-130">*Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="a8db6-131">1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8db6-133">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="a8db6-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="a8db6-134">Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="a8db6-135">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-135">Build the project.</span></span>
* <span data-ttu-id="a8db6-136">Paket Yöneticisi konsolu 'nda (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="a8db6-137">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8db6-139">Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="a8db6-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="a8db6-140">*Contosouniversity. csproj*öğesini silin ve *Contosoüniversıtysqlite. csproj* öğesini *contosouniversity. csproj*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="a8db6-141">*Startup.cs*silin ve *StartupSQLite.cs* öğesini *Startup.cs*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="a8db6-142">*AppSettings. JSON*öğesini silin ve *Appsettingssqlite. JSON* öğesini *appSettings. JSON*olarak yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="a8db6-143">*Geçişler* klasörünü silin ve *migrationssql* öğesini *geçişlerle*yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="a8db6-144">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-144">Build the project.</span></span>
* <span data-ttu-id="a8db6-145">Proje klasöründeki bir komut isteminde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* <span data-ttu-id="a8db6-146">SQLite aracında şu SQL ifadesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="a8db6-147">Veritabanını temel alarak projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="a8db6-148">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-150">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a8db6-151">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a8db6-152">Projeyi adlandırın *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="a8db6-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="a8db6-153">Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="a8db6-154">Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8db6-156">Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="a8db6-157">Bir Razor Pages projesi ve `cd` yeni proje klasörü oluşturmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="a8db6-158">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="a8db6-158">Set up the site style</span></span>

<span data-ttu-id="a8db6-159">*Sayfaları/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="a8db6-160">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="a8db6-161">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-161">There are three occurrences.</span></span>

* <span data-ttu-id="a8db6-162">**Giriş** ve **Gizlilik** menü girişlerini silin ve **hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için girişler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="a8db6-163">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="a8db6-164">*Pages/Index. cshtml*dosyasında, ASP.NET Core hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="a8db6-165">Giriş sayfasının göründüğünü doğrulamak için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="a8db6-166">Veri modeli</span><span class="sxs-lookup"><span data-stu-id="a8db6-166">The data model</span></span>

<span data-ttu-id="a8db6-167">Aşağıdaki bölümler bir veri modeli oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a8db6-167">The following sections create a data model:</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="a8db6-169">Bir öğrenci herhangi bir sayıda kursa kaydolabilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="a8db6-170">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-170">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

* <span data-ttu-id="a8db6-172">Proje klasöründe bir *modeller* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="a8db6-173">Aşağıdaki kodla *modeller/öğrenci. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a8db6-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="a8db6-174">Özelliği `ID` , bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="a8db6-175">Varsayılan olarak EF Core adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="a8db6-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a8db6-176">Bu nedenle birincil anahtar `Student` sınıfı için alternatif otomatik olarak tanınan ad olur. `StudentID`</span><span class="sxs-lookup"><span data-stu-id="a8db6-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="a8db6-177">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="a8db6-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="a8db6-178">Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="a8db6-179">Bu durumda, `Enrollments` bir `Student` varlığın özelliği söz konusu öğrenciye ilişkin tüm varlıklarıbarındırır.`Enrollment`</span><span class="sxs-lookup"><span data-stu-id="a8db6-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="a8db6-180">Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="a8db6-181">Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="a8db6-182">Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="a8db6-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="a8db6-183">İlgili kayıt satırları Studentitıd = 1 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="a8db6-184">Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* .</span><span class="sxs-lookup"><span data-stu-id="a8db6-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="a8db6-185">Özelliği, birden çok ilgili `ICollection<Enrollment>` kayıt varlığı olabileceğinden, olarak tanımlanır. `Enrollments`</span><span class="sxs-lookup"><span data-stu-id="a8db6-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="a8db6-186">`List<Enrollment>` Veya`HashSet<Enrollment>`gibi diğer koleksiyon türlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="a8db6-187">Zaman `ICollection<Enrollment>` olduğu EF Core kullanıldığında, oluşturur bir `HashSet<Enrollment>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="a8db6-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="a8db6-188">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-188">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="a8db6-190">Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a8db6-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="a8db6-191">Bu özellik birincil anahtardır; bu varlık kendi başına değil, `classnameID` kendi modelini `ID` kullanır. `EnrollmentID`</span><span class="sxs-lookup"><span data-stu-id="a8db6-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="a8db6-192">Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="a8db6-193">Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="a8db6-194">`ID` Olmadan`classname` kullanmak, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="a8db6-195">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a8db6-196">`Grade` Tür bildiriminden sonraki soru işareti, `Grade` özelliğin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="a8db6-197">Null olan bir sınıf sıfır&mdash;bir sınıf null değerinden farklıdır veya henüz atanmamış olur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a8db6-198">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a8db6-199">Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özellik içerecek şekilde varlık `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="a8db6-200">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a8db6-201">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a8db6-202">EF Core adlandırılmışsa, bu özellik bir yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a8db6-203">Örneğin,`StudentID` nesnenin birincil anahtarı olduğundan `Student` `Student` `ID`, gezinti özelliği için yabancı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a8db6-204">Yabancı anahtar özellikleri de adı `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a8db6-205">Örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="a8db6-206">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-206">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="a8db6-208">Aşağıdaki kodla *modeller/kurs. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a8db6-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="a8db6-209">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8db6-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a8db6-210">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a8db6-211">`DatabaseGenerated` Özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="a8db6-212">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="a8db6-213">Yapı iskelesi öğrenci sayfaları</span><span class="sxs-lookup"><span data-stu-id="a8db6-213">Scaffold Student pages</span></span>

<span data-ttu-id="a8db6-214">Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:</span><span class="sxs-lookup"><span data-stu-id="a8db6-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="a8db6-215">EF Core *bağlamı* sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a8db6-215">An EF Core *context* class.</span></span> <span data-ttu-id="a8db6-216">Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a8db6-217">`Microsoft.EntityFrameworkCore.DbContext` Sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="a8db6-218">`Student` Varlık için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="a8db6-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-220">*Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="a8db6-221">**Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve **yeni yapı iskelesi** **Ekle** > öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a8db6-222">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="a8db6-223">**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="a8db6-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="a8db6-224">İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a8db6-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="a8db6-225">**Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="a8db6-226">*Contosouniversity. modeller. Contosoüniversıtycontext* olan veri bağlamı adını *Contosouniversity. Data. SchoolContext*olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="a8db6-227">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-227">Select **Add**.</span></span>

<span data-ttu-id="a8db6-228">Aşağıdaki paketler otomatik olarak yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a8db6-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8db6-230">Gerekli NuGet paketlerini yüklemek için aşağıdaki .NET Core CLI komutlarını çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
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

  <span data-ttu-id="a8db6-231">Yapı iskelesi için Microsoft. VisualStudio. Web. CodeGeneration. Design paketi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="a8db6-232">Uygulama SQL Server kullanmayabilse de, scafkatlama aracı SQL Server paketine ihtiyaç duyuyor.</span><span class="sxs-lookup"><span data-stu-id="a8db6-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="a8db6-233">*Sayfalar/öğrenciler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="a8db6-234">[ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator)'nı yüklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="a8db6-235">Fkatlama öğrenci sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="a8db6-236">**Windows 'da**</span><span class="sxs-lookup"><span data-stu-id="a8db6-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="a8db6-237">**MacOS veya Linux üzerinde**</span><span class="sxs-lookup"><span data-stu-id="a8db6-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="a8db6-238">Önceki adımla ilgili bir sorununuz varsa, projeyi derleyin ve yapı iskelesi adımını yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="a8db6-239">Yapı iskelesi işlemi:</span><span class="sxs-lookup"><span data-stu-id="a8db6-239">The scaffolding process:</span></span>

* <span data-ttu-id="a8db6-240">*Sayfalar/öğrenciler* klasöründe Razor sayfaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a8db6-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="a8db6-241">*. Cshtml* ve *Create.cshtml.cs* oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="a8db6-242">*Delete. cshtml* ve *delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a8db6-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="a8db6-243">*Details. cshtml* ve *details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a8db6-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="a8db6-244">*. Cshtml* ve *Edit.cshtml.cs* Düzenle</span><span class="sxs-lookup"><span data-stu-id="a8db6-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="a8db6-245">*Index. cshtml* ve *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a8db6-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="a8db6-246">*Data/SchoolContext. cs*oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="a8db6-247">*Startup.cs*içinde bağımlılık eklenmesine bağlam ekler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="a8db6-248">*AppSettings. JSON*öğesine bir veritabanı bağlantı dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="a8db6-249">Veritabanı bağlantı dizesi</span><span class="sxs-lookup"><span data-stu-id="a8db6-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8db6-251">Bağlantı dizesini belirtir [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="a8db6-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="a8db6-252">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a8db6-253">Varsayılan olarak, LocalDB `C:/Users/<user>` dizinde *. mdf* dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8db6-255">Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="a8db6-256">Veritabanı bağlam sınıfını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="a8db6-256">Update the database context class</span></span>

<span data-ttu-id="a8db6-257">Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="a8db6-258">Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a8db6-259">Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a8db6-260">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a8db6-261">Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a8db6-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="a8db6-262">Vurgulanan kod oluşturur bir [olan DB\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8db6-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="a8db6-263">EF Core terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="a8db6-263">In EF Core terminology:</span></span>

* <span data-ttu-id="a8db6-264">Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="a8db6-265">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a8db6-266">Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="a8db6-267">Yapı iskelesi aracı bir`Student` dbset oluşturduğundan, bu adım çoğul `Students`olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="a8db6-268">Razor Pages kodun yeni dbset adıyla eşleşmesini sağlamak için, tüm projesi `_context.Student` `_context.Students`genelinde genel bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="a8db6-269">8 oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-269">There are 8 occurrences.</span></span>

<span data-ttu-id="a8db6-270">Derleyici hatası olmadığını doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="a8db6-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a8db6-271">Startup.cs</span></span>

<span data-ttu-id="a8db6-272">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a8db6-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a8db6-273">Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a8db6-274">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a8db6-275">Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a8db6-276">Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.</span><span class="sxs-lookup"><span data-stu-id="a8db6-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-278">' `ConfigureServices`De, vurgulanan satırlar scaffolder tarafından eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="a8db6-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8db6-280">' `ConfigureServices`De, desteği tarafından eklenen kodun çağrılarından `UseSqlite`emin olun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="a8db6-281">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="a8db6-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a8db6-282">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a8db6-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a8db6-283">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-283">Create the database</span></span>

<span data-ttu-id="a8db6-284">Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="a8db6-285">Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="a8db6-286">Veritabanı yoksa, veritabanını ve şemayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="a8db6-287">`EnsureCreated`veri modeli değişikliklerini işlemek için aşağıdaki iş akışını sunar:</span><span class="sxs-lookup"><span data-stu-id="a8db6-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="a8db6-288">Veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-288">Delete the database.</span></span> <span data-ttu-id="a8db6-289">Mevcut veriler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-289">Any existing data is lost.</span></span>
* <span data-ttu-id="a8db6-290">Veri modelini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-290">Change the data model.</span></span> <span data-ttu-id="a8db6-291">Örneğin, bir `EmailAddress` alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="a8db6-292">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-292">Run the app.</span></span>
* <span data-ttu-id="a8db6-293">`EnsureCreated`yeni şemaya sahip bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="a8db6-294">Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="a8db6-295">Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="a8db6-296">Bu durumda, geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="a8db6-297">Öğretici serisinde daha sonra tarafından `EnsureCreated` oluşturulan veritabanını siler ve bunun yerine geçişleri kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="a8db6-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="a8db6-298">Tarafından `EnsureCreated` oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="a8db6-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="a8db6-299">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a8db6-299">Test the app</span></span>

* <span data-ttu-id="a8db6-300">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-300">Run the app.</span></span>
* <span data-ttu-id="a8db6-301">Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="a8db6-302">Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="a8db6-303">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-303">Seed the database</span></span>

<span data-ttu-id="a8db6-304">`EnsureCreated` Yöntemi boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="a8db6-305">Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="a8db6-306">Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a8db6-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="a8db6-307">Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="a8db6-308">Öğrenci yoksa, veritabanına test verileri ekler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="a8db6-309">Performansı iyileştirmek için `List<T>` koleksiyonlar yerine diziler halinde test verileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="a8db6-310">*Program.cs*içinde, `EnsureCreated` çağrıyı bir `DbInitializer.Initialize` çağrı ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8db6-312">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8db6-314">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="a8db6-315">Uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-315">Restart the app.</span></span>

* <span data-ttu-id="a8db6-316">Sağlanan verileri görmek için öğrenciler sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="a8db6-317">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="a8db6-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-319">Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="a8db6-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="a8db6-320">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="a8db6-321">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="a8db6-322">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="a8db6-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="a8db6-323">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="a8db6-324">`Student` Modelin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8db6-326">Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="a8db6-327">Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="a8db6-328">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="a8db6-328">Asynchronous code</span></span>

<span data-ttu-id="a8db6-329">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a8db6-330">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a8db6-331">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="a8db6-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a8db6-332">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="a8db6-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a8db6-333">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a8db6-334">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="a8db6-335">Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a8db6-336">Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="a8db6-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a8db6-337">Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="a8db6-338">`async` Anahtar sözcüğü, derleyiciye bildirir:</span><span class="sxs-lookup"><span data-stu-id="a8db6-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="a8db6-339">Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a8db6-340">Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="a8db6-341">Dönüş `Task<T>` türü, devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a8db6-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="a8db6-342">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a8db6-343">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a8db6-344">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="a8db6-345">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a8db6-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a8db6-346">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a8db6-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a8db6-347">Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a8db6-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="a8db6-348">`ToListAsync` ,`SingleOrDefaultAsync`,, Ve`SaveChangesAsync`içerir. `FirstOrDefaultAsync`</span><span class="sxs-lookup"><span data-stu-id="a8db6-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a8db6-349">Yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="a8db6-350">EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="a8db6-351">Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="a8db6-352">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="a8db6-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8db6-353">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="a8db6-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a8db6-354">Sonraki öğretici</span><span class="sxs-lookup"><span data-stu-id="a8db6-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8db6-355">Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="a8db6-356">Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a8db6-357">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="a8db6-358">Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="a8db6-359">İndirme veya tamamlanmış uygulamayı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a8db6-360">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a8db6-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8db6-361">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a8db6-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="a8db6-364">Konusunda [Razor sayfaları](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="a8db6-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="a8db6-365">Yeni programcılar tamamlamanız gereken [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Bu seriyi başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="a8db6-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a8db6-366">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="a8db6-366">Troubleshooting</span></span>

<span data-ttu-id="a8db6-367">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="a8db6-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="a8db6-368">Soru göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="a8db6-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="a8db6-369">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="a8db6-369">The Contoso University web app</span></span>

<span data-ttu-id="a8db6-370">Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="a8db6-371">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a8db6-372">Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-372">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="a8db6-375">Bu sitenin UI Stili yerleşik şablonları tarafından üretilen yakın ' dir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="a8db6-376">EF çekirdekli Razor sayfaları, UI ile öğretici odağı açıktır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="a8db6-377">ContosoUniversity Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-379">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a8db6-380">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="a8db6-381">Projeyi adlandırın **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="a8db6-382">Projeyi adlandırın önemlidir *ContosoUniversity* kod kopyalanıp/yapıştırılmış ad alanları eşleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="a8db6-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="a8db6-383">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="a8db6-384">Önceki adımlarda görüntüleri için bkz: [Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="a8db6-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="a8db6-385">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="a8db6-387">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="a8db6-387">Set up the site style</span></span>

<span data-ttu-id="a8db6-388">Birkaç değişiklik site menü, Düzen ve giriş sayfası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="a8db6-389">Güncelleştirme *Pages/Shared/_Layout.cshtml* aşağıdaki değişikliklerle birlikte:</span><span class="sxs-lookup"><span data-stu-id="a8db6-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="a8db6-390">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="a8db6-391">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-391">There are three occurrences.</span></span>

* <span data-ttu-id="a8db6-392">Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="a8db6-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="a8db6-393">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-393">The changes are highlighted.</span></span> <span data-ttu-id="a8db6-394">(Tüm biçimlendirme *değil* görüntülenir.)</span><span class="sxs-lookup"><span data-stu-id="a8db6-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="a8db6-395">İçinde *sayfalar/dizin.cshtml*, dosyanın içeriğini ASP.NET ve MVC hakkında metnin bu uygulama hakkında metinle değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="a8db6-396">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="a8db6-396">Create the data model</span></span>

<span data-ttu-id="a8db6-397">Varlık sınıflarının Contoso University uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="a8db6-398">Aşağıdaki üç varlıklarla başlayın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-398">Start with the following three entities:</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="a8db6-400">Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="a8db6-401">Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="a8db6-402">Bir öğrenci herhangi bir sayıda kursları kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="a8db6-403">Bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="a8db6-404">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="a8db6-405">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-405">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="a8db6-407">Oluşturma bir *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a8db6-407">Create a *Models* folder.</span></span> <span data-ttu-id="a8db6-408">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a8db6-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="a8db6-409">`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="a8db6-410">Varsayılan olarak EF Core adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="a8db6-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a8db6-411">İçinde `classnameID`, `classname` sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="a8db6-412">Alternatif birincil anahtarı otomatik olarak kabul edilen `StudentID` önceki örnekte.</span><span class="sxs-lookup"><span data-stu-id="a8db6-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="a8db6-413">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="a8db6-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="a8db6-414">Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="a8db6-415">Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` olarak ilişkili varlıkları `Student`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="a8db6-416">Örneğin, bir öğrenci satır DB'de iki kayıt satırları ilgili olan `Enrollments` gezinti özelliği içerir, bu iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="a8db6-417">İlgili `Enrollment` satırdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="a8db6-418">Örneğin, Öğrenci kimlikli varsayalım = 1 olan iki satır `Enrollment` tablo.</span><span class="sxs-lookup"><span data-stu-id="a8db6-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="a8db6-419">`Enrollment` Tablosunda var olan iki satır `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="a8db6-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="a8db6-420">`StudentID` içinde bir yabancı anahtar `Enrollment` içinde Öğrenci belirten tablo `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="a8db6-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="a8db6-421">Bir gezinme özelliği birden çok varlık tutarsanız gezinme özelliğini bir liste türü gibi olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="a8db6-422">`ICollection<T>` belirtilebilir, ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="a8db6-423">Zaman `ICollection<T>` olduğu EF Core kullanıldığında, oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="a8db6-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="a8db6-424">Birden çok varlık tutun Gezinti özellikleri çoktan çoğa ve bire çok ilişkileri gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="a8db6-425">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-425">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="a8db6-427">İçinde *modelleri* klasör oluşturma *Enrollment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a8db6-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="a8db6-428">`EnrollmentID` Birincil anahtar özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="a8db6-429">Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="a8db6-430">Genellikle geliştiriciler bir düzen seçin ve veri modelini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="a8db6-431">Bir sonraki öğreticide, classname Kimliğini kullanarak, veri modelinde aktarma uygulamak daha kolay hale getirmek için gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="a8db6-432">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a8db6-433">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="a8db6-434">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="a8db6-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a8db6-435">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a8db6-436">Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özellik içerecek şekilde varlık `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="a8db6-437">`Student` Varlık farklıdır `Student.Enrollments` içeren birden çok gezinti özelliği `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="a8db6-438">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a8db6-439">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a8db6-440">EF Core adlandırılmışsa, bu özellik bir yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a8db6-441">Örneğin,`StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a8db6-442">Yabancı anahtar özellikleri de adı `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a8db6-443">Örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="a8db6-444">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="a8db6-444">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="a8db6-446">İçinde *modelleri* klasör oluşturma *Course.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a8db6-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="a8db6-447">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8db6-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a8db6-448">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a8db6-449">`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek için uygulamayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="a8db6-450">İskele Öğrenci modeli</span><span class="sxs-lookup"><span data-stu-id="a8db6-450">Scaffold the student model</span></span>

<span data-ttu-id="a8db6-451">Bu bölümde, Öğrenci modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="a8db6-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="a8db6-452">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik Öğrenci modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="a8db6-453">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-453">Build the project.</span></span>
* <span data-ttu-id="a8db6-454">Oluşturma *sayfaları/Öğrenciler* klasör.</span><span class="sxs-lookup"><span data-stu-id="a8db6-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8db6-456">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/Öğrenciler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a8db6-457">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="a8db6-458">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="a8db6-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a8db6-459">İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a8db6-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="a8db6-460">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan bir adla değiştirin **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="a8db6-461">İçinde **veri bağlamı sınıfının** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="a8db6-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="a8db6-462">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-462">Select **Add**.</span></span>

![CRUD iletişim](intro/_static/s1.png)

<span data-ttu-id="a8db6-464">Bkz: [film modeli iskelesini](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.</span><span class="sxs-lookup"><span data-stu-id="a8db6-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8db6-466">Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="a8db6-467">İskele işlem oluşturulur ve aşağıdaki dosya değişti:</span><span class="sxs-lookup"><span data-stu-id="a8db6-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a8db6-468">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="a8db6-468">Files created</span></span>

* <span data-ttu-id="a8db6-469">*Sayfa/Öğrenciler* oluşturma, silme, Ayrıntılar, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="a8db6-470">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a8db6-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="a8db6-471">Dosya güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="a8db6-471">File updates</span></span>

* <span data-ttu-id="a8db6-472">*Startup.cs* : Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="a8db6-473">*appSettings. JSON* : Yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a8db6-474">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="a8db6-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a8db6-475">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a8db6-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a8db6-476">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a8db6-477">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a8db6-478">Bir db bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a8db6-479">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="a8db6-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a8db6-480">İnceleme `ConfigureServices` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a8db6-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="a8db6-481">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="a8db6-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="a8db6-482">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="a8db6-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a8db6-483">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a8db6-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="a8db6-484">Ana güncelleştir</span><span class="sxs-lookup"><span data-stu-id="a8db6-484">Update main</span></span>

<span data-ttu-id="a8db6-485">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="a8db6-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="a8db6-486">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="a8db6-487">Çağrı [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="a8db6-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="a8db6-488">Bağlam dispose olduğunda `EnsureCreated` yöntemi tamamlar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="a8db6-489">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="a8db6-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="a8db6-490">`EnsureCreated` Veritabanı bağlamının var olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="a8db6-491">Varsa, hiçbir işlem yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-491">If it exists, no action is taken.</span></span> <span data-ttu-id="a8db6-492">Yoksa, veritabanı ve tüm şema oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="a8db6-493">`EnsureCreated` veritabanı oluşturmaya geçişleri kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="a8db6-494">İle oluşturulmuş bir veritabanı `EnsureCreated` daha sonra geçişleri kullanılarak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="a8db6-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="a8db6-495">`EnsureCreated` Aşağıdaki iş akışı sağlayan uygulama Başlat menüsünde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a8db6-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="a8db6-496">DB silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-496">Delete the DB.</span></span>
* <span data-ttu-id="a8db6-497">DB şema değiştirme (örneğin, bir `EmailAddress` alan).</span><span class="sxs-lookup"><span data-stu-id="a8db6-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="a8db6-498">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-498">Run the app.</span></span>
* <span data-ttu-id="a8db6-499">`EnsureCreated` bir DB ile oluşturur`EmailAddress` sütun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="a8db6-500">`EnsureCreated` Şema hızla gelişirken erken geliştirme uygundur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="a8db6-501">Daha sonra öğreticide DB silinir ve geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="a8db6-502">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a8db6-502">Test the app</span></span>

<span data-ttu-id="a8db6-503">Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="a8db6-504">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="a8db6-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="a8db6-505">Tanımlama bilgisi ilkesi hakkında bilgi edinebilirsiniz [AB genel veri koruma yönetmeliği (GDPR) Destek](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="a8db6-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="a8db6-506">Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="a8db6-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="a8db6-507">Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="a8db6-508">SchoolContext DB bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="a8db6-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="a8db6-509">Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="a8db6-510">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a8db6-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a8db6-511">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a8db6-512">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a8db6-513">Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="a8db6-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="a8db6-514">Vurgulanan kod oluşturur bir [olan DB\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8db6-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="a8db6-515">EF Core terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="a8db6-515">In EF Core terminology:</span></span>

* <span data-ttu-id="a8db6-516">Bir varlık kümesini genellikle DB tabloya karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="a8db6-517">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a8db6-518">`DbSet<Enrollment>` ve `DbSet<Course>` atlanmış.</span><span class="sxs-lookup"><span data-stu-id="a8db6-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="a8db6-519">EF Core içeren bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlığı ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="a8db6-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="a8db6-520">Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="a8db6-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="a8db6-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="a8db6-522">Bağlantı dizesini belirtir [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="a8db6-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="a8db6-523">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a8db6-524">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="a8db6-525">Varsayılan olarak LocalDB oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="a8db6-526">Bir veritabanı test verileri ile başlatmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="a8db6-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="a8db6-527">EF Core boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="a8db6-528">Bu bölümde, bir `Initialize` yöntemi test verileriyle doldurma işlemine yazılır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="a8db6-529">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a8db6-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="a8db6-530">Not: Önceki kod, yerine `Models` adalanı`namespace ContosoUniversity.Models`() için kullanır `Data`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="a8db6-531">`Models`, scaffolder tarafından oluşturulan kodla tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="a8db6-532">Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="a8db6-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="a8db6-533">Kod DB'de tüm Öğrenciler olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="a8db6-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="a8db6-534">DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="a8db6-535">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="a8db6-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="a8db6-536">`EnsureCreated` Yöntemi DB bağlamı için bir veritabanı otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="a8db6-537">Veritabanı varsa, `EnsureCreated` DB değiştirmeden döndürür.</span><span class="sxs-lookup"><span data-stu-id="a8db6-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="a8db6-538">İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="a8db6-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8db6-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8db6-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8db6-540">Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a8db6-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8db6-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8db6-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8db6-542">Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="a8db6-543">DB görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="a8db6-543">View the DB</span></span>

<span data-ttu-id="a8db6-544">Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="a8db6-545">Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="a8db6-546">GUID her kullanıcı için farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="a8db6-547">Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="a8db6-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="a8db6-548">SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="a8db6-549">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="a8db6-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="a8db6-550">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8db6-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="a8db6-551">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="a8db6-551">Asynchronous code</span></span>

<span data-ttu-id="a8db6-552">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a8db6-553">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a8db6-554">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="a8db6-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a8db6-555">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="a8db6-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a8db6-556">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a8db6-557">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="a8db6-558">Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar.</span><span class="sxs-lookup"><span data-stu-id="a8db6-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a8db6-559">Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="a8db6-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a8db6-560">Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="a8db6-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="a8db6-561">`async` Anahtar sözcüğü, derleyiciye bildirir:</span><span class="sxs-lookup"><span data-stu-id="a8db6-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="a8db6-562">Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a8db6-563">Otomatik olarak oluşturmasını [görev](/dotnet/api/system.threading.tasks.task) döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="a8db6-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="a8db6-564">Daha fazla bilgi için [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="a8db6-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="a8db6-565">Örtük dönüş türü `Task` devam eden çalışmayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a8db6-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="a8db6-566">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="a8db6-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a8db6-567">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a8db6-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a8db6-568">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="a8db6-569">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a8db6-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a8db6-570">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a8db6-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a8db6-571">Sorguları veya Veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a8db6-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="a8db6-572">İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a8db6-573">Yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="a8db6-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="a8db6-574">EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="a8db6-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="a8db6-575">Zaman uyumsuz kodun performans avantajlarından yararlanmak için bunlar için bir veritabanı sorguları göndermek EF Core yöntemleri çağırırsanız kitaplığı paketlerinin (disk belleği sunamıyoruz gibi) zaman uyumsuz kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a8db6-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="a8db6-576">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="a8db6-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="a8db6-577">Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.</span><span class="sxs-lookup"><span data-stu-id="a8db6-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="a8db6-578">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a8db6-578">Additional resources</span></span>

* [<span data-ttu-id="a8db6-579">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="a8db6-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="a8db6-580">Next</span><span class="sxs-lookup"><span data-stu-id="a8db6-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
