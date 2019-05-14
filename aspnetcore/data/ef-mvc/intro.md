---
title: 'Öğretici: Bir ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama'
description: Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: a93d5af314f1ff679a8df636297a0d5849ebdb8d
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610204"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="84a0c-103">Öğretici: Bir ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="84a0c-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="84a0c-104">Contoso University örnek web uygulaması, Entity Framework (EF) çekirdek 2.2 ve Visual Studio 2017 veya 2019 kullanarak ASP.NET Core 2.2 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="84a0c-105">Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="84a0c-106">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="84a0c-107">Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="84a0c-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="84a0c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="84a0c-109">Bir ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-109">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="84a0c-110">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="84a0c-110">Set up the site style</span></span>
> * <span data-ttu-id="84a0c-111">EF Core NuGet paketleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="84a0c-111">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="84a0c-112">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-112">Create the data model</span></span>
> * <span data-ttu-id="84a0c-113">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="84a0c-113">Create the database context</span></span>
> * <span data-ttu-id="84a0c-114">Bağımlılık ekleme bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="84a0c-114">Register the context for dependency injection</span></span>
> * <span data-ttu-id="84a0c-115">Veritabanı test verileri ile başlatılamıyor</span><span class="sxs-lookup"><span data-stu-id="84a0c-115">Initialize the database with test data</span></span>
> * <span data-ttu-id="84a0c-116">Bir denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-116">Create a controller and views</span></span>
> * <span data-ttu-id="84a0c-117">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="84a0c-117">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84a0c-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="84a0c-118">Prerequisites</span></span>

* [<span data-ttu-id="84a0c-119">.NET core SDK'sını 2.2</span><span class="sxs-lookup"><span data-stu-id="84a0c-119">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="84a0c-120">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) aşağıdaki iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="84a0c-120">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="84a0c-121">**ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="84a0c-121">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="84a0c-122">**.NET core çoklu platform geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="84a0c-122">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="84a0c-123">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="84a0c-123">Troubleshooting</span></span>

<span data-ttu-id="84a0c-124">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="84a0c-124">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="84a0c-125">Sık karşılaşılan hatalar ve bunları çözmek nasıl bir listesi için bkz. [serinin son Öğreticisi sorun giderme bölümünü](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="84a0c-125">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="84a0c-126">Var. aradığınızı bulamazsanız, StackOverflow.com için için soru gönderebildiği [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="84a0c-126">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="84a0c-127">Önceki öğreticilerde bitti üzerinde her biri yapılar 10 öğreticiler, bir dizi budur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-127">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="84a0c-128">Her öğretici Kurulum başarıyla tamamlandıktan sonra projenin bir kopyasını kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-128">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="84a0c-129">Sorunlarla karşılaşırsanız, ardından yeniden yukarıda bahsedilen tüm dizileri başlangıcına yerine önceki öğreticiden başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-129">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="84a0c-130">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="84a0c-130">Contoso University web app</span></span>

<span data-ttu-id="84a0c-131">Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="84a0c-132">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="84a0c-133">Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-133">Here are a few of the screens you'll create.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="84a0c-136">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-136">Create web app</span></span>

* <span data-ttu-id="84a0c-137">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-137">Open Visual Studio.</span></span>

* <span data-ttu-id="84a0c-138">Gelen **dosya** menüsünde **yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-138">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="84a0c-139">Sol bölmeden **yüklü > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-139">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="84a0c-140">Seçin **ASP.NET Core Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="84a0c-140">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="84a0c-141">Girin **ContosoUniversity** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-141">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project2.png)

* <span data-ttu-id="84a0c-143">Bekle **yeni ASP.NET Core Web uygulaması** görüntülenecek iletişim.</span><span class="sxs-lookup"><span data-stu-id="84a0c-143">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="84a0c-144">Seçin **.NET Core**, **ASP.NET Core 2.2** ve **Web uygulaması (Model-View-Controller)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="84a0c-144">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="84a0c-145">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-145">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="84a0c-146">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="84a0c-146">Select **OK**</span></span>

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="84a0c-148">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="84a0c-148">Set up the site style</span></span>

<span data-ttu-id="84a0c-149">Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.</span><span class="sxs-lookup"><span data-stu-id="84a0c-149">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="84a0c-150">Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="84a0c-150">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="84a0c-151">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-151">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="84a0c-152">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-152">There are three occurrences.</span></span>

* <span data-ttu-id="84a0c-153">Menü girdileri eklemek **hakkında**, **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**, ve silme **gizlilik** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-153">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="84a0c-154">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-154">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="84a0c-155">İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="84a0c-155">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="84a0c-156">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="84a0c-156">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="84a0c-157">Aşağıdaki öğreticilerde oluşturacaksınız sayfaları için sekmeli giriş sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-157">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="84a0c-159">EF Core NuGet paketleri hakkında</span><span class="sxs-lookup"><span data-stu-id="84a0c-159">About EF Core NuGet packages</span></span>

<span data-ttu-id="84a0c-160">EF Core desteği için bir proje eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-160">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="84a0c-161">Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="84a0c-161">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="84a0c-162">Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), paket başvurusu yapmak zorunda kalmazsınız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-162">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="84a0c-163">EF SQL Server paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="84a0c-163">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="84a0c-164">Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="84a0c-164">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="84a0c-165">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="84a0c-165">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="84a0c-166">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-166">Create the data model</span></span>

<span data-ttu-id="84a0c-167">Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-167">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="84a0c-168">Aşağıdaki üç varlıklar ile başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-168">You'll start with the following three entities.</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="84a0c-170">Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84a0c-170">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="84a0c-171">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-171">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="84a0c-172">Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-172">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="84a0c-173">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="84a0c-173">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="84a0c-175">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-175">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="84a0c-176">`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="84a0c-176">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="84a0c-177">Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="84a0c-177">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="84a0c-178">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="84a0c-178">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="84a0c-179">Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun.</span><span class="sxs-lookup"><span data-stu-id="84a0c-179">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="84a0c-180">Bu durumda, `Enrollments` özelliği bir `Student entity` tümünü tutacak `Enrollment` olarak ilişkili varlıkları `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="84a0c-180">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="84a0c-181">Diğer bir deyişle, belirli bir öğrenci satır veritabanında iki kayıt satırları (birincil anahtar değeri kendi StudentID yabancı anahtar sütunu, Öğrenci içeren satırlar) ilgili olan, `Student` varlığın `Enrollments` gezinti özelliği bu içerecek iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84a0c-181">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="84a0c-182">Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-182">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="84a0c-183">Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-183">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="84a0c-184">Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="84a0c-184">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="84a0c-185">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="84a0c-185">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="84a0c-187">İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="84a0c-187">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="84a0c-188">`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="84a0c-188">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="84a0c-189">Genellikle bir düzen seçin ve veri modelinizi kullanılmakta.</span><span class="sxs-lookup"><span data-stu-id="84a0c-189">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="84a0c-190">Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-190">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="84a0c-191">İçinde bir [sonraki öğreticide](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-191">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="84a0c-192">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-192">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="84a0c-193">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-193">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="84a0c-194">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="84a0c-194">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="84a0c-195">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-195">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="84a0c-196">Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).</span><span class="sxs-lookup"><span data-stu-id="84a0c-196">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="84a0c-197">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-197">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="84a0c-198">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="84a0c-198">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="84a0c-199">Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="84a0c-199">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="84a0c-200">Yabancı anahtar özellikleri de adı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="84a0c-200">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="84a0c-201">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="84a0c-201">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="84a0c-203">İçinde *modelleri* klasör oluşturma *Course.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="84a0c-203">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="84a0c-204">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="84a0c-204">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="84a0c-205">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84a0c-205">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="84a0c-206">Daha fazla ilgili dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="84a0c-206">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="84a0c-207">Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-207">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="84a0c-208">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="84a0c-208">Create the database context</span></span>

<span data-ttu-id="84a0c-209">Verilen veri modeli için Entity Framework işlevselliği koordine eden ana veritabanı bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-209">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="84a0c-210">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="84a0c-210">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="84a0c-211">Kodunuzda hangi varlıkları veri modelinde yer alan belirtin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-211">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="84a0c-212">Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-212">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="84a0c-213">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-213">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="84a0c-214">Proje klasöründe adlı bir klasör oluşturun *veri*.</span><span class="sxs-lookup"><span data-stu-id="84a0c-214">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="84a0c-215">İçinde *veri* klasör oluşturma adlı yeni bir sınıf dosyası *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="84a0c-215">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="84a0c-216">Bu kod oluşturur bir `DbSet` her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="84a0c-216">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="84a0c-217">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-217">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="84a0c-218">Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-218">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="84a0c-219">Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="84a0c-219">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="84a0c-220">Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları.</span><span class="sxs-lookup"><span data-stu-id="84a0c-220">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="84a0c-221">Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında.</span><span class="sxs-lookup"><span data-stu-id="84a0c-221">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="84a0c-222">Bu öğreticiler için bir DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-222">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="84a0c-223">Bunu yapmak için son olan DB özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-223">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="84a0c-224">SchoolContext kaydetme</span><span class="sxs-lookup"><span data-stu-id="84a0c-224">Register the SchoolContext</span></span>

<span data-ttu-id="84a0c-225">ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="84a0c-225">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="84a0c-226">Hizmetler (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-226">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="84a0c-227">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-227">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="84a0c-228">Bu öğreticide daha sonra bir bağlam örneğini alır denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-228">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="84a0c-229">Kaydedilecek `SchoolContext` bir hizmet olarak açmak *Startup.cs*ve vurgulanan satırları ekleyin `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-229">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="84a0c-230">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir `DbContextOptionsBuilder` nesne.</span><span class="sxs-lookup"><span data-stu-id="84a0c-230">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="84a0c-231">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="84a0c-231">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="84a0c-232">Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve ardından projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-232">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="84a0c-233">Açık *appsettings.json* dosya ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-233">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="84a0c-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="84a0c-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="84a0c-235">Bir SQL Server LocalDB veritabanına bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-235">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="84a0c-236">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="84a0c-237">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="84a0c-238">Varsayılan olarak LocalDB oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-238">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="84a0c-239">DB test verileri ile başlatılamıyor</span><span class="sxs-lookup"><span data-stu-id="84a0c-239">Initialize DB with test data</span></span>

<span data-ttu-id="84a0c-240">Entity Framework sizin için boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-240">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="84a0c-241">Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-241">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="84a0c-242">Burada, kullanacağınız `EnsureCreated` otomatik olarak veritabanı oluşturmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-242">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="84a0c-243">İçinde bir [sonraki öğreticide](migrations.md) bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-243">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="84a0c-244">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veri yüklerini test Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="84a0c-244">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="84a0c-245">Kod, veritabanındaki tüm Öğrenciler vardır ve aksi durumda, veritabanını yeni ve test verileri ile sağlanmış gerekiyor, varsayar denetler.</span><span class="sxs-lookup"><span data-stu-id="84a0c-245">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="84a0c-246">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="84a0c-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="84a0c-247">İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="84a0c-247">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="84a0c-248">Veritabanı bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-248">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="84a0c-249">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-249">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="84a0c-250">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="84a0c-250">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="84a0c-251">Ekleme `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="84a0c-251">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="84a0c-252">Önceki öğreticilerde, benzer bir kod görebilirsiniz `Configure` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="84a0c-252">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="84a0c-253">Kullanmanızı öneririz `Configure` yöntemi yalnızca istek işlem hattı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-253">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="84a0c-254">Uygulama başlangıç koduna ait içinde `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-254">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="84a0c-255">Şimdi uygulamayı çalıştırdığınız ilk kez veritabanı oluşturulacak ve test verileri ile çekirdek değeri oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="84a0c-255">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="84a0c-256">Veri modelinizi değiştirdiğinizde, veritabanını silin, çekirdek yönteminizi güncelleştirin ve yeni bir veritabanı ile aynı şekilde parçalarından başlatın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-256">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="84a0c-257">Sonraki öğreticilerde, silinmesi ve yeniden oluşturulmadan değişiklik veri modelini kullanırken, veritabanı değiştirme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-257">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="84a0c-258">Denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="84a0c-258">Create controller and views</span></span>

<span data-ttu-id="84a0c-259">Ardından, bir MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio yapı iskelesi altyapısı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-259">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="84a0c-260">CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-260">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="84a0c-261">Yapı iskelesi kod oluşturma farklıdır iskele kurulan kodu ise, oluşturulan kod genellikle değiştirmeyin, kendi gereksinimlerinize uyacak şekilde değiştirebileceği bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-261">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="84a0c-262">Oluşturulan kod özelleştirmeniz gerektiğinde, kısmi sınıflar kullanın veya şeyler değiştirdiğinizde, kodu yeniden oluştur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-262">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="84a0c-263">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-263">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="84a0c-264">İçinde **İskele Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="84a0c-264">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="84a0c-265">Seçin **Entity Framework kullanarak görünümler ile MVC denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-265">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="84a0c-266">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-266">Click **Add**.</span></span> <span data-ttu-id="84a0c-267">**Ekleme Entity Framework kullanarak görünümlere sahip MVC denetleyici,** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-267">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![İskele Öğrenci](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="84a0c-269">İçinde **Model sınıfı** seçin **Öğrenci**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="84a0c-270">İçinde **veri bağlamı sınıfının** seçin **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="84a0c-271">Varsayılan değerleri kabul **StudentsController** adı.</span><span class="sxs-lookup"><span data-stu-id="84a0c-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="84a0c-272">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-272">Click **Add**.</span></span>

  <span data-ttu-id="84a0c-273">Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-273">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="84a0c-274">(El ile oluşturmayın, yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-274">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="84a0c-275">Yeni bir bağlam sınıfında belirttiğiniz **denetleyici Ekle** kutusunun sağındaki artı işaretine tıklayarak **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="84a0c-275">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="84a0c-276">Visual Studio ardından oluşturur, `DbContext` sınıf denetleyici ve görünüm yanı sıra.)</span><span class="sxs-lookup"><span data-stu-id="84a0c-276">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="84a0c-277">Denetleyici aldığını fark edeceksiniz bir `SchoolContext` Oluşturucu parametresi olarak.</span><span class="sxs-lookup"><span data-stu-id="84a0c-277">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="84a0c-278">ASP.NET Core bağımlılık ekleme örneğini geçirerek üstlenir `SchoolContext` denetleyici içinde.</span><span class="sxs-lookup"><span data-stu-id="84a0c-278">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="84a0c-279">Yapılandırdığınız *Startup.cs* daha önce dosya.</span><span class="sxs-lookup"><span data-stu-id="84a0c-279">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="84a0c-280">Denetleyici içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüleyen eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-280">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="84a0c-281">Yöntemi, okuyarak ayarlamak Öğrenciler varlıktan Öğrenciler listesini alır. `Students` veritabanı bağlam örneğinin özelliği:</span><span class="sxs-lookup"><span data-stu-id="84a0c-281">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="84a0c-282">Bu kod zaman uyumsuz programlama öğeleri öğreticinin ilerleyen bölümlerinde öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-282">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="84a0c-283">*Views/Students/Index.cshtml* bu liste bir tabloda görüntüleyen:</span><span class="sxs-lookup"><span data-stu-id="84a0c-283">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="84a0c-284">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="84a0c-284">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="84a0c-285">Test verileri görmek için Öğrenciler sekmesine tıklayın, `DbInitializer.Initialize` eklenen yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-285">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="84a0c-286">Bağlı nasıl dar tarayıcı pencerenizin, gördüğünüz `Students` Gezinti bağlantıyı görmek için sağ üst köşedeki simgeyi tıklatın sayfanın veya üst kısmındaki sekme bağlantısına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="84a0c-286">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Dar contoso University giriş sayfası](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="84a0c-289">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="84a0c-289">View the database</span></span>

<span data-ttu-id="84a0c-290">Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-290">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="84a0c-291">EF gördüğünüz veritabanı vardı ve bu nedenle oluşturulan bir sonra geri kalanında `Initialize` yöntemi kod veritabanı verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-291">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="84a0c-292">Kullanabileceğiniz **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için).</span><span class="sxs-lookup"><span data-stu-id="84a0c-292">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="84a0c-293">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-293">Close the browser.</span></span>

<span data-ttu-id="84a0c-294">SSOX pencere zaten açık değilse, seçim **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="84a0c-294">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="84a0c-295">SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları**ve ardından bağlantı dizesinde veritabanının adını girişini tıklatın, *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="84a0c-295">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="84a0c-296">Genişletin **tabloları** veritabanınızdaki tablolar görmek için düğümü.</span><span class="sxs-lookup"><span data-stu-id="84a0c-296">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX tablolarında](intro/_static/ssox-tables.png)

<span data-ttu-id="84a0c-298">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-298">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Öğrenci tabloda SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="84a0c-300">*.Mdf* ve *.ldf* veritabanı dosyalar, *C:\Users\\\<kullanıcıadınız >* klasör.</span><span class="sxs-lookup"><span data-stu-id="84a0c-300">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="84a0c-301">Aradığınız çünkü `EnsureCreated` uygulama başlatıldığında çalıştırılan Başlatıcı yönteminde artık bir değişiklik yapabileceğiniz `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması.</span><span class="sxs-lookup"><span data-stu-id="84a0c-301">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="84a0c-302">Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıfı göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablodaki sütun.</span><span class="sxs-lookup"><span data-stu-id="84a0c-302">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="84a0c-303">Kurallar</span><span class="sxs-lookup"><span data-stu-id="84a0c-303">Conventions</span></span>

<span data-ttu-id="84a0c-304">Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmanız gerekirdi kod kuralları veya Entity Framework yapan varsayımlar kullanımı nedeniyle en az olur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-304">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="84a0c-305">Adlarını `DbSet` özellikleri, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-305">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="84a0c-306">Varlık tarafından başvurulmayan bir `DbSet` özelliği, varlık sınıfı adları, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-306">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="84a0c-307">Varlık özellik adlarını sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-307">Entity property names are used for column names.</span></span>

* <span data-ttu-id="84a0c-308">Kimliği veya Classnameıd adlı varlık özellikleri birincil anahtar özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-308">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="84a0c-309">Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır  *\<gezinme özelliği adı >\<birincil anahtar özelliği adı >* (örneğin, `StudentID` için `Student` Gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="84a0c-309">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="84a0c-310">Yabancı anahtar özellikleri de adı yalnızca  *\<birincil anahtar özelliği adı >* (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="84a0c-310">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="84a0c-311">Geleneksel davranışını geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-311">Conventional behavior can be overridden.</span></span> <span data-ttu-id="84a0c-312">Örneğin, bu öğreticide daha önce bahsettiğim gibi tablo adları açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a0c-312">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="84a0c-313">Sütun adları ayarlayın ve görüşelim gibi herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="84a0c-313">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="84a0c-314">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="84a0c-314">Asynchronous code</span></span>

<span data-ttu-id="84a0c-315">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-315">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="84a0c-316">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-316">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="84a0c-317">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="84a0c-317">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="84a0c-318">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="84a0c-318">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="84a0c-319">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-319">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="84a0c-320">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-320">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="84a0c-321">Zaman uyumsuz kod çalışma zamanında az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.</span><span class="sxs-lookup"><span data-stu-id="84a0c-321">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="84a0c-322">Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="84a0c-322">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="84a0c-323">`async` Anahtar sözcüğü derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için `Task<IActionResult>` döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="84a0c-323">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="84a0c-324">Dönüş türü `Task<IActionResult>` türünün bir sonucu ile devam eden çalışmayı temsil `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-324">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="84a0c-325">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="84a0c-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="84a0c-326">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="84a0c-326">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="84a0c-327">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84a0c-327">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="84a0c-328">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84a0c-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="84a0c-329">Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="84a0c-329">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="84a0c-330">Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="84a0c-330">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="84a0c-331">Örneğin, içeren `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-331">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="84a0c-332">Bu, örneğin, yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="84a0c-332">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="84a0c-333">EF bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-333">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="84a0c-334">Herhangi bir zaman uyumsuz EF yöntemi çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="84a0c-334">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="84a0c-335">Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.</span><span class="sxs-lookup"><span data-stu-id="84a0c-335">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="84a0c-336">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="84a0c-336">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="84a0c-337">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="84a0c-337">Get the code</span></span>

[<span data-ttu-id="84a0c-338">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="84a0c-338">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="84a0c-339">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="84a0c-339">Next steps</span></span>

<span data-ttu-id="84a0c-340">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="84a0c-340">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="84a0c-341">Oluşturulan ASP.NET Core MVC web uygulaması</span><span class="sxs-lookup"><span data-stu-id="84a0c-341">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="84a0c-342">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="84a0c-342">Set up the site style</span></span>
> * <span data-ttu-id="84a0c-343">EF Core NuGet paketleri hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="84a0c-343">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="84a0c-344">Veri modeli oluşturduk</span><span class="sxs-lookup"><span data-stu-id="84a0c-344">Created the data model</span></span>
> * <span data-ttu-id="84a0c-345">Veritabanı bağlamı oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="84a0c-345">Created the database context</span></span>
> * <span data-ttu-id="84a0c-346">SchoolContext kayıtlı</span><span class="sxs-lookup"><span data-stu-id="84a0c-346">Registered the SchoolContext</span></span>
> * <span data-ttu-id="84a0c-347">Test verileri ile başlatılmış DB</span><span class="sxs-lookup"><span data-stu-id="84a0c-347">Initialized DB with test data</span></span>
> * <span data-ttu-id="84a0c-348">Oluşturulan denetleyici ve Görünüm</span><span class="sxs-lookup"><span data-stu-id="84a0c-348">Created controller and views</span></span>
> * <span data-ttu-id="84a0c-349">Veritabanı görüntülenebilir</span><span class="sxs-lookup"><span data-stu-id="84a0c-349">Viewed the database</span></span>

<span data-ttu-id="84a0c-350">Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="84a0c-350">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="84a0c-351">Temel CRUD gerçekleştirme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="84a0c-351">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="84a0c-352">Temel CRUD işlevselliği uygulama</span><span class="sxs-lookup"><span data-stu-id="84a0c-352">Implement basic CRUD functionality</span></span>](crud.md)
