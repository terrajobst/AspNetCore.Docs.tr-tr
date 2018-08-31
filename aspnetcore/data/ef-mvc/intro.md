---
title: Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/intro
ms.openlocfilehash: 46cc5144a289637a0fb1c1c2ac22aba2d881af52
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336113"
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a><span data-ttu-id="8820a-102">Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="8820a-102">ASP.NET Core MVC with Entity Framework Core - Tutorial 1 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8820a-103">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8820a-103">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="8820a-104">Contoso University örnek web uygulaması, Entity Framework (EF) Core 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.0 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8820a-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="8820a-105">Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="8820a-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="8820a-106">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="8820a-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="8820a-107">Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.</span><span class="sxs-lookup"><span data-stu-id="8820a-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="8820a-108">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-108">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="8820a-109">EF Core 2.0 EF en son sürümü, ancak henüz EF özelliklerinin tümünü yok 6.x.</span><span class="sxs-lookup"><span data-stu-id="8820a-109">EF Core 2.0 is the latest version of EF but doesn't yet have all the features of EF 6.x.</span></span> <span data-ttu-id="8820a-110">EF arasında seçim yapma hakkında bilgi için bkz: 6.x ve EF Core [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="8820a-110">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="8820a-111">EF seçerseniz 6.x, bkz: [Bu öğretici serisinin önceki sürümünü](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="8820a-111">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> <span data-ttu-id="8820a-112">Bu öğreticide ASP.NET Core 1.1 sürümü için bkz: [VS 2017 güncelleştirme 2 sürüm PDF biçimindeki bu öğreticinin](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="8820a-112">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8820a-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8820a-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a><span data-ttu-id="8820a-114">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="8820a-114">Troubleshooting</span></span>

<span data-ttu-id="8820a-115">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="8820a-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="8820a-116">Sık karşılaşılan hatalar ve bunları çözmek nasıl bir listesi için bkz. [serinin son Öğreticisi sorun giderme bölümünü](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="8820a-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="8820a-117">Var. aradığınızı bulamazsanız, StackOverflow.com için için soru gönderebildiği [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="8820a-117">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="8820a-118">Önceki öğreticilerde bitti üzerinde her biri yapılar 10 öğreticiler, bir dizi budur.</span><span class="sxs-lookup"><span data-stu-id="8820a-118">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="8820a-119">Her öğretici Kurulum başarıyla tamamlandıktan sonra projenin bir kopyasını kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="8820a-120">Sorunlarla karşılaşırsanız, ardından yeniden yukarıda bahsedilen tüm dizileri başlangıcına yerine önceki öğreticiden başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-120">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="8820a-121">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="8820a-121">The Contoso University web application</span></span>

<span data-ttu-id="8820a-122">Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="8820a-122">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="8820a-123">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8820a-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="8820a-124">Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8820a-124">Here are a few of the screens you'll create.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="8820a-127">Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8820a-127">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="8820a-128">Bir ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8820a-128">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="8820a-129">Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir ASP.NET Core C# web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8820a-129">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="8820a-130">Gelen **dosya** menüsünde **yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="8820a-130">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="8820a-131">Sol bölmeden **yüklü > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="8820a-131">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="8820a-132">Seçin **ASP.NET Core Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="8820a-132">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="8820a-133">Girin **ContosoUniversity** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="8820a-133">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project.png)

* <span data-ttu-id="8820a-135">Bekle **yeni ASP.NET Core Web uygulaması (.NET Core)** görüntülenecek iletişim</span><span class="sxs-lookup"><span data-stu-id="8820a-135">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="8820a-136">Seçin **ASP.NET Core 2.0** ve **Web uygulaması (Model-View-Controller)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="8820a-136">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="8820a-137">**Not:** Bu öğretici, ASP.NET Core 2.0 ve EF Core 2.0 veya sonraki--emin gerektirir **ASP.NET Core 1.1** seçilmez.</span><span class="sxs-lookup"><span data-stu-id="8820a-137">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** isn't selected.</span></span>

* <span data-ttu-id="8820a-138">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="8820a-138">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="8820a-139">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8820a-139">Click **OK**</span></span>

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="8820a-141">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="8820a-141">Set up the site style</span></span>

<span data-ttu-id="8820a-142">Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8820a-142">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="8820a-143">Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="8820a-143">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="8820a-144">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8820a-144">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="8820a-145">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8820a-145">There are three occurrences.</span></span>

* <span data-ttu-id="8820a-146">Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="8820a-146">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="8820a-147">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="8820a-147">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="8820a-148">İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8820a-148">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="8820a-149">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="8820a-149">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="8820a-150">Aşağıdaki öğreticilerde oluşturacaksınız sayfaları için sekmeli giriş sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8820a-150">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="8820a-152">Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="8820a-152">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="8820a-153">EF Core desteği için bir proje eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-153">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="8820a-154">Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="8820a-154">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="8820a-155">Bu paket dahil [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8820a-155">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="8820a-156">Bu paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="8820a-156">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="8820a-157">Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="8820a-157">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="8820a-158">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](https://docs.microsoft.com/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="8820a-158">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="8820a-159">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="8820a-159">Create the data model</span></span>

<span data-ttu-id="8820a-160">Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8820a-160">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="8820a-161">Aşağıdaki üç varlıklar ile başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="8820a-161">You'll start with the following three entities.</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="8820a-163">Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8820a-163">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="8820a-164">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-164">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="8820a-165">Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8820a-165">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="8820a-166">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="8820a-166">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="8820a-168">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8820a-168">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="8820a-169">`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="8820a-169">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="8820a-170">Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="8820a-170">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="8820a-171">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="8820a-171">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="8820a-172">Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun.</span><span class="sxs-lookup"><span data-stu-id="8820a-172">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="8820a-173">Bu durumda, `Enrollments` özelliği bir `Student entity` tümünü tutacak `Enrollment` olarak ilişkili varlıkları `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="8820a-173">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="8820a-174">Diğer bir deyişle, belirli bir öğrenci satır veritabanında iki kayıt satırları (birincil anahtar değeri kendi StudentID yabancı anahtar sütunu, Öğrenci içeren satırlar) ilgili olan, `Student` varlığın `Enrollments` gezinti özelliği bu içerecek iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8820a-174">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="8820a-175">Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="8820a-175">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="8820a-176">Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="8820a-176">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="8820a-177">Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="8820a-177">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="8820a-178">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="8820a-178">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="8820a-180">İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8820a-180">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="8820a-181">`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="8820a-181">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="8820a-182">Genellikle bir düzen seçin ve veri modelinizi kullanılmakta.</span><span class="sxs-lookup"><span data-stu-id="8820a-182">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="8820a-183">Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8820a-183">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="8820a-184">İçinde bir [sonraki öğreticide](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8820a-184">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="8820a-185">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="8820a-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="8820a-186">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="8820a-187">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="8820a-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="8820a-188">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="8820a-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="8820a-189">Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).</span><span class="sxs-lookup"><span data-stu-id="8820a-189">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="8820a-190">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="8820a-190">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="8820a-191">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="8820a-191">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="8820a-192">Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="8820a-192">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="8820a-193">Yabancı anahtar özellikleri de adı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="8820a-193">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="8820a-194">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="8820a-194">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="8820a-196">İçinde *modelleri* klasör oluşturma *Course.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8820a-196">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="8820a-197">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="8820a-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="8820a-198">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8820a-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="8820a-199">Daha fazla ilgili dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="8820a-199">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="8820a-200">Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8820a-200">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="8820a-201">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="8820a-201">Create the Database Context</span></span>

<span data-ttu-id="8820a-202">Verilen veri modeli için Entity Framework işlevselliği koordine eden ana veritabanı bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="8820a-202">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="8820a-203">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="8820a-203">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="8820a-204">Kodunuzda hangi varlıkları veri modelinde yer alan belirtin.</span><span class="sxs-lookup"><span data-stu-id="8820a-204">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="8820a-205">Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-205">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="8820a-206">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="8820a-206">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="8820a-207">Proje klasöründe adlı bir klasör oluşturun *veri*.</span><span class="sxs-lookup"><span data-stu-id="8820a-207">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="8820a-208">İçinde *veri* klasör oluşturma adlı yeni bir sınıf dosyası *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8820a-208">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="8820a-209">Bu kod oluşturur bir `DbSet` her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="8820a-209">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="8820a-210">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8820a-210">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="8820a-211">Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="8820a-211">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="8820a-212">Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="8820a-212">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="8820a-213">Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları.</span><span class="sxs-lookup"><span data-stu-id="8820a-213">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="8820a-214">Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında.</span><span class="sxs-lookup"><span data-stu-id="8820a-214">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="8820a-215">Bu öğreticiler için bir DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-215">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="8820a-216">Bunu yapmak için son olan DB özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-216">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="8820a-217">Bağımlılık ekleme bağlam kaydı</span><span class="sxs-lookup"><span data-stu-id="8820a-217">Register the context with dependency injection</span></span>

<span data-ttu-id="8820a-218">ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="8820a-218">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="8820a-219">Hizmetler (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-219">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8820a-220">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="8820a-220">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="8820a-221">Bu öğreticide daha sonra bir bağlam örneğini alır denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8820a-221">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="8820a-222">Kaydedilecek `SchoolContext` bir hizmet olarak açmak *Startup.cs*ve vurgulanan satırları ekleyin `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-222">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="8820a-223">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir `DbContextOptionsBuilder` nesne.</span><span class="sxs-lookup"><span data-stu-id="8820a-223">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="8820a-224">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="8820a-224">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="8820a-225">Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve ardından projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-225">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="8820a-226">Açık *appsettings.json* dosya ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-226">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="8820a-227">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="8820a-227">SQL Server Express LocalDB</span></span>

<span data-ttu-id="8820a-228">Bir SQL Server LocalDB veritabanına bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="8820a-228">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="8820a-229">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8820a-229">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="8820a-230">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="8820a-230">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="8820a-231">Varsayılan olarak LocalDB oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="8820a-231">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="8820a-232">Veritabanı test verileri ile başlatmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="8820a-232">Add code to initialize the database with test data</span></span>

<span data-ttu-id="8820a-233">Entity Framework sizin için boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8820a-233">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="8820a-234">Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="8820a-234">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="8820a-235">Burada, kullanacağınız `EnsureCreated` otomatik olarak veritabanı oluşturmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-235">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="8820a-236">İçinde bir [sonraki öğreticide](migrations.md) bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8820a-236">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="8820a-237">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veri yüklerini test Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="8820a-237">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="8820a-238">Kod, veritabanındaki tüm Öğrenciler vardır ve aksi durumda, veritabanını yeni ve test verileri ile sağlanmış gerekiyor, varsayar denetler.</span><span class="sxs-lookup"><span data-stu-id="8820a-238">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="8820a-239">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="8820a-239">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="8820a-240">İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="8820a-240">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="8820a-241">Veritabanı bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="8820a-241">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="8820a-242">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="8820a-242">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="8820a-243">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="8820a-243">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="8820a-244">Ekleme `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="8820a-244">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="8820a-245">Önceki öğreticilerde, benzer bir kod görebilirsiniz `Configure` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8820a-245">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="8820a-246">Kullanmanızı öneririz `Configure` yöntemi yalnızca istek işlem hattı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8820a-246">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="8820a-247">Uygulama başlangıç koduna ait içinde `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-247">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="8820a-248">Şimdi uygulamayı çalıştırdığınız ilk kez veritabanı oluşturulacak ve test verileri ile çekirdek değeri oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="8820a-248">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="8820a-249">Veri modelinizi değiştirdiğinizde, veritabanını silin, çekirdek yönteminizi güncelleştirin ve yeni bir veritabanı ile aynı şekilde parçalarından başlatın.</span><span class="sxs-lookup"><span data-stu-id="8820a-249">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="8820a-250">Sonraki öğreticilerde, silinmesi ve yeniden oluşturulmadan değişiklik veri modelini kullanırken, veritabanı değiştirme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8820a-250">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="8820a-251">Bir denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="8820a-251">Create a controller and views</span></span>

<span data-ttu-id="8820a-252">Ardından, bir MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio yapı iskelesi altyapısı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8820a-252">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="8820a-253">CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="8820a-253">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="8820a-254">Yapı iskelesi kod oluşturma farklıdır iskele kurulan kodu ise, oluşturulan kod genellikle değiştirmeyin, kendi gereksinimlerinize uyacak şekilde değiştirebileceği bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="8820a-254">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="8820a-255">Oluşturulan kod özelleştirmeniz gerektiğinde, kısmi sınıflar kullanın veya şeyler değiştirdiğinizde, kodu yeniden oluştur.</span><span class="sxs-lookup"><span data-stu-id="8820a-255">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="8820a-256">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="8820a-256">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="8820a-257">Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="8820a-257">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="8820a-258">[Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8820a-258">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="8820a-259">Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="8820a-259">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="8820a-260">Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.</span><span class="sxs-lookup"><span data-stu-id="8820a-260">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="8820a-261">İçinde **İskele Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="8820a-261">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="8820a-262">Seçin **Entity Framework kullanarak görünümler ile MVC denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="8820a-262">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="8820a-263">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8820a-263">Click **Add**.</span></span>

* <span data-ttu-id="8820a-264">İçinde **denetleyici Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="8820a-264">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="8820a-265">İçinde **Model sınıfı** seçin **Öğrenci**.</span><span class="sxs-lookup"><span data-stu-id="8820a-265">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="8820a-266">İçinde **veri bağlamı sınıfının** seçin **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="8820a-266">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="8820a-267">Varsayılan değerleri kabul **StudentsController** adı.</span><span class="sxs-lookup"><span data-stu-id="8820a-267">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="8820a-268">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8820a-268">Click **Add**.</span></span>

  ![İskele Öğrenci](intro/_static/scaffold-student.png)

  <span data-ttu-id="8820a-270">Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="8820a-270">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="8820a-271">(El ile oluşturmayın, yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi.</span><span class="sxs-lookup"><span data-stu-id="8820a-271">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="8820a-272">Yeni bir bağlam sınıfında belirttiğiniz **denetleyici Ekle** kutusunun sağındaki artı işaretine tıklayarak **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="8820a-272">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="8820a-273">Visual Studio ardından oluşturur, `DbContext` sınıf denetleyici ve görünüm yanı sıra.)</span><span class="sxs-lookup"><span data-stu-id="8820a-273">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="8820a-274">Denetleyici aldığını fark edeceksiniz bir `SchoolContext` Oluşturucu parametresi olarak.</span><span class="sxs-lookup"><span data-stu-id="8820a-274">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="8820a-275">ASP.NET Core bağımlılık ekleme örneğini geçirerek üstlenir `SchoolContext` denetleyici içinde.</span><span class="sxs-lookup"><span data-stu-id="8820a-275">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="8820a-276">Yapılandırdığınız *Startup.cs* daha önce dosya.</span><span class="sxs-lookup"><span data-stu-id="8820a-276">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="8820a-277">Denetleyici içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüleyen eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-277">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="8820a-278">Yöntemi, okuyarak ayarlamak Öğrenciler varlıktan Öğrenciler listesini alır. `Students` veritabanı bağlam örneğinin özelliği:</span><span class="sxs-lookup"><span data-stu-id="8820a-278">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="8820a-279">Bu kod zaman uyumsuz programlama öğeleri öğreticinin ilerleyen bölümlerinde öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-279">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="8820a-280">*Views/Students/Index.cshtml* bu liste bir tabloda görüntüleyen:</span><span class="sxs-lookup"><span data-stu-id="8820a-280">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="8820a-281">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="8820a-281">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="8820a-282">Test verileri görmek için Öğrenciler sekmesine tıklayın, `DbInitializer.Initialize` eklenen yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-282">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="8820a-283">Bağlı nasıl dar tarayıcı pencerenizin, gördüğünüz `Student` Gezinti bağlantıyı görmek için sağ üst köşedeki simgeyi tıklatın sayfanın veya üst kısmındaki sekme bağlantısına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8820a-283">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Dar contoso University giriş sayfası](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="8820a-286">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="8820a-286">View the Database</span></span>

<span data-ttu-id="8820a-287">Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="8820a-287">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="8820a-288">EF gördüğünüz veritabanı vardı ve bu nedenle oluşturulan bir sonra geri kalanında `Initialize` yöntemi kod veritabanı verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="8820a-288">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="8820a-289">Kullanabileceğiniz **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için).</span><span class="sxs-lookup"><span data-stu-id="8820a-289">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="8820a-290">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="8820a-290">Close the browser.</span></span>

<span data-ttu-id="8820a-291">SSOX pencere zaten açık değilse, seçim **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="8820a-291">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="8820a-292">SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları**ve ardından bağlantı dizesinde veritabanının adını girişini tıklatın, *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="8820a-292">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="8820a-293">Genişletin **tabloları** veritabanınızdaki tablolar görmek için düğümü.</span><span class="sxs-lookup"><span data-stu-id="8820a-293">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX tablolarında](intro/_static/ssox-tables.png)

<span data-ttu-id="8820a-295">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-295">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Öğrenci tabloda SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="8820a-297"><em>.Mdf</em> ve <em>.ldf</em> veritabanı dosyalar, <em>C:\Users\\ <yourusername> </em> klasör.</span><span class="sxs-lookup"><span data-stu-id="8820a-297">The <em>.mdf</em> and <em>.ldf</em> database files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="8820a-298">Aradığınız çünkü `EnsureCreated` uygulama başlatıldığında çalıştırılan Başlatıcı yönteminde artık bir değişiklik yapabileceğiniz `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması.</span><span class="sxs-lookup"><span data-stu-id="8820a-298">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="8820a-299">Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıfı göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablodaki sütun.</span><span class="sxs-lookup"><span data-stu-id="8820a-299">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="8820a-300">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8820a-300">Conventions</span></span>

<span data-ttu-id="8820a-301">Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmanız gerekirdi kod kuralları veya Entity Framework yapan varsayımlar kullanımı nedeniyle en az olur.</span><span class="sxs-lookup"><span data-stu-id="8820a-301">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="8820a-302">Adlarını `DbSet` özellikleri, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8820a-302">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="8820a-303">Varlık tarafından başvurulmayan bir `DbSet` özelliği, varlık sınıfı adları, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8820a-303">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="8820a-304">Varlık özellik adlarını sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8820a-304">Entity property names are used for column names.</span></span>

* <span data-ttu-id="8820a-305">Kimliği veya Classnameıd adlı varlık özellikleri birincil anahtar özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-305">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="8820a-306">Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *<navigation property name> <primary key property name>* (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="8820a-306">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="8820a-307">Yabancı anahtar özellikleri de adı yalnızca *<primary key property name>* (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="8820a-307">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="8820a-308">Geleneksel davranışını geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-308">Conventional behavior can be overridden.</span></span> <span data-ttu-id="8820a-309">Örneğin, bu öğreticide daha önce bahsettiğim gibi tablo adları açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8820a-309">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="8820a-310">Sütun adları ayarlayın ve görüşelim gibi herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="8820a-310">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="8820a-311">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="8820a-311">Asynchronous code</span></span>

<span data-ttu-id="8820a-312">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="8820a-312">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="8820a-313">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-313">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="8820a-314">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="8820a-314">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="8820a-315">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="8820a-315">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="8820a-316">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="8820a-316">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="8820a-317">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-317">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="8820a-318">Zaman uyumsuz kod çalışma zamanında az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.</span><span class="sxs-lookup"><span data-stu-id="8820a-318">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="8820a-319">Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="8820a-319">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="8820a-320">`async` Anahtar sözcüğü derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için `Task<IActionResult>` döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="8820a-320">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="8820a-321">Dönüş türü `Task<IActionResult>` türünün bir sonucu ile devam eden çalışmayı temsil `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="8820a-321">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="8820a-322">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="8820a-322">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="8820a-323">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="8820a-323">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="8820a-324">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8820a-324">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="8820a-325">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8820a-325">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="8820a-326">Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="8820a-326">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="8820a-327">Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="8820a-327">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="8820a-328">Örneğin, içeren `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="8820a-328">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="8820a-329">Bu, örneğin, yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="8820a-329">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="8820a-330">EF bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="8820a-330">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="8820a-331">Herhangi bir zaman uyumsuz EF yöntemi çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="8820a-331">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="8820a-332">Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.</span><span class="sxs-lookup"><span data-stu-id="8820a-332">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="8820a-333">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="8820a-333">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="8820a-334">Özet</span><span class="sxs-lookup"><span data-stu-id="8820a-334">Summary</span></span>

<span data-ttu-id="8820a-335">Depolamak ve verileri görüntülemek için Entity Framework Core ve SQL Server Express LocalDB kullanan basit bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="8820a-335">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="8820a-336">Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="8820a-336">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="8820a-337">Next</span><span class="sxs-lookup"><span data-stu-id="8820a-337">Next</span></span>](crud.md)
