---
title: Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/intro
ms.openlocfilehash: 4e0bcffd1162681aa4d31c4fe74acac5a7e981f1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216318"
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a><span data-ttu-id="c13f5-102">Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c13f5-102">ASP.NET Core MVC with Entity Framework Core - Tutorial 1 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c13f5-103">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c13f5-103">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="c13f5-104">Contoso University örnek web uygulaması, Entity Framework (EF) Core 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.0 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="c13f5-105">Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c13f5-106">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="c13f5-107">Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="c13f5-108">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-108">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="c13f5-109">EF Core 2.0 EF en son sürümü, ancak henüz EF özelliklerinin tümünü yok 6.x.</span><span class="sxs-lookup"><span data-stu-id="c13f5-109">EF Core 2.0 is the latest version of EF but doesn't yet have all the features of EF 6.x.</span></span> <span data-ttu-id="c13f5-110">EF arasında seçim yapma hakkında bilgi için bkz: 6.x ve EF Core [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="c13f5-110">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="c13f5-111">EF seçerseniz 6.x, bkz: [Bu öğretici serisinin önceki sürümünü](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="c13f5-111">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> <span data-ttu-id="c13f5-112">Bu öğreticide ASP.NET Core 1.1 sürümü için bkz: [VS 2017 güncelleştirme 2 sürüm PDF biçimindeki bu öğreticinin](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="c13f5-112">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c13f5-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c13f5-113">Prerequisites</span></span>

<span data-ttu-id="c13f5-114">[! [] (~/İncludes/net-core-prereqs.md) içerir [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="c13f5-114">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c13f5-115">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="c13f5-115">Troubleshooting</span></span>

<span data-ttu-id="c13f5-116">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="c13f5-116">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="c13f5-117">Sık karşılaşılan hatalar ve bunları çözmek nasıl bir listesi için bkz. [serinin son Öğreticisi sorun giderme bölümünü](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="c13f5-117">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="c13f5-118">Var. aradığınızı bulamazsanız, StackOverflow.com için için soru gönderebildiği [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c13f5-118">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="c13f5-119">Önceki öğreticilerde bitti üzerinde her biri yapılar 10 öğreticiler, bir dizi budur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-119">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="c13f5-120">Her öğretici Kurulum başarıyla tamamlandıktan sonra projenin bir kopyasını kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-120">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="c13f5-121">Sorunlarla karşılaşırsanız, ardından yeniden yukarıda bahsedilen tüm dizileri başlangıcına yerine önceki öğreticiden başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-121">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="c13f5-122">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="c13f5-122">The Contoso University web application</span></span>

<span data-ttu-id="c13f5-123">Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-123">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="c13f5-124">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c13f5-125">Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-125">Here are a few of the screens you'll create.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="c13f5-128">Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-128">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="c13f5-129">Bir ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="c13f5-129">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="c13f5-130">Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir ASP.NET Core C# web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c13f5-130">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="c13f5-131">Gelen **dosya** menüsünde **yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-131">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="c13f5-132">Sol bölmeden **yüklü > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-132">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="c13f5-133">Seçin **ASP.NET Core Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="c13f5-133">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="c13f5-134">Girin **ContosoUniversity** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-134">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project.png)

* <span data-ttu-id="c13f5-136">Bekle **yeni ASP.NET Core Web uygulaması (.NET Core)** görüntülenecek iletişim</span><span class="sxs-lookup"><span data-stu-id="c13f5-136">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="c13f5-137">Seçin **ASP.NET Core 2.0** ve **Web uygulaması (Model-View-Controller)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="c13f5-137">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="c13f5-138">**Not:** Bu öğretici, ASP.NET Core 2.0 ve EF Core 2.0 veya sonraki--emin gerektirir **ASP.NET Core 1.1** seçilmez.</span><span class="sxs-lookup"><span data-stu-id="c13f5-138">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** isn't selected.</span></span>

* <span data-ttu-id="c13f5-139">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-139">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="c13f5-140">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-140">Click **OK**</span></span>

  ![Yeni ASP.NET projesi iletişim kutusu](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="c13f5-142">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="c13f5-142">Set up the site style</span></span>

<span data-ttu-id="c13f5-143">Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c13f5-143">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="c13f5-144">Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="c13f5-144">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="c13f5-145">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-145">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="c13f5-146">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-146">There are three occurrences.</span></span>

* <span data-ttu-id="c13f5-147">Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-147">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="c13f5-148">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-148">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="c13f5-149">İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c13f5-149">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="c13f5-150">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="c13f5-150">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="c13f5-151">Aşağıdaki öğreticilerde oluşturacaksınız sayfaları için sekmeli giriş sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-151">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="c13f5-153">Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="c13f5-153">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="c13f5-154">EF Core desteği için bir proje eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-154">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="c13f5-155">Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="c13f5-155">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="c13f5-156">Bu paket dahil [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c13f5-156">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="c13f5-157">Bu paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="c13f5-157">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="c13f5-158">Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="c13f5-158">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="c13f5-159">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](https://docs.microsoft.com/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="c13f5-159">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="c13f5-160">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="c13f5-160">Create the data model</span></span>

<span data-ttu-id="c13f5-161">Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c13f5-161">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="c13f5-162">Aşağıdaki üç varlıklar ile başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="c13f5-162">You'll start with the following three entities.</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="c13f5-164">Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="c13f5-164">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="c13f5-165">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-165">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="c13f5-166">Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c13f5-166">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="c13f5-167">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="c13f5-167">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="c13f5-169">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-169">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="c13f5-170">`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="c13f5-170">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="c13f5-171">Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="c13f5-171">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="c13f5-172">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="c13f5-172">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c13f5-173">Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun.</span><span class="sxs-lookup"><span data-stu-id="c13f5-173">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="c13f5-174">Bu durumda, `Enrollments` özelliği bir `Student entity` tümünü tutacak `Enrollment` olarak ilişkili varlıkları `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="c13f5-174">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="c13f5-175">Diğer bir deyişle, belirli bir öğrenci satır veritabanında iki kayıt satırları (birincil anahtar değeri kendi StudentID yabancı anahtar sütunu, Öğrenci içeren satırlar) ilgili olan, `Student` varlığın `Enrollments` gezinti özelliği bu içerecek iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="c13f5-175">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="c13f5-176">Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-176">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="c13f5-177">Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-177">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="c13f5-178">Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="c13f5-178">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="c13f5-179">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="c13f5-179">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="c13f5-181">İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c13f5-181">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="c13f5-182">`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="c13f5-182">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="c13f5-183">Genellikle bir düzen seçin ve veri modelinizi kullanılmakta.</span><span class="sxs-lookup"><span data-stu-id="c13f5-183">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="c13f5-184">Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-184">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="c13f5-185">İçinde bir [sonraki öğreticide](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-185">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="c13f5-186">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c13f5-187">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="c13f5-188">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="c13f5-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c13f5-189">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c13f5-190">Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).</span><span class="sxs-lookup"><span data-stu-id="c13f5-190">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="c13f5-191">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c13f5-192">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="c13f5-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c13f5-193">Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="c13f5-193">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="c13f5-194">Yabancı anahtar özellikleri de adı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="c13f5-194">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="c13f5-195">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="c13f5-195">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="c13f5-197">İçinde *modelleri* klasör oluşturma *Course.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c13f5-197">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="c13f5-198">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="c13f5-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c13f5-199">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="c13f5-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c13f5-200">Daha fazla ilgili dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="c13f5-200">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="c13f5-201">Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-201">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="c13f5-202">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="c13f5-202">Create the Database Context</span></span>

<span data-ttu-id="c13f5-203">Verilen veri modeli için Entity Framework işlevselliği koordine eden ana veritabanı bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-203">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="c13f5-204">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c13f5-204">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="c13f5-205">Kodunuzda hangi varlıkları veri modelinde yer alan belirtin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-205">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="c13f5-206">Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-206">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="c13f5-207">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c13f5-208">Proje klasöründe adlı bir klasör oluşturun *veri*.</span><span class="sxs-lookup"><span data-stu-id="c13f5-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="c13f5-209">İçinde *veri* klasör oluşturma adlı yeni bir sınıf dosyası *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c13f5-209">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="c13f5-210">Bu kod oluşturur bir `DbSet` her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="c13f5-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="c13f5-211">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-211">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c13f5-212">Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-212">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="c13f5-213">Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="c13f5-213">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="c13f5-214">Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları.</span><span class="sxs-lookup"><span data-stu-id="c13f5-214">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="c13f5-215">Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında.</span><span class="sxs-lookup"><span data-stu-id="c13f5-215">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="c13f5-216">Bu öğreticiler için bir DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-216">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="c13f5-217">Bunu yapmak için son olan DB özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-217">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="c13f5-218">Bağımlılık ekleme bağlam kaydı</span><span class="sxs-lookup"><span data-stu-id="c13f5-218">Register the context with dependency injection</span></span>

<span data-ttu-id="c13f5-219">ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="c13f5-219">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="c13f5-220">Hizmetler (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-220">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c13f5-221">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-221">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c13f5-222">Bu öğreticide daha sonra bir bağlam örneğini alır denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-222">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="c13f5-223">Kaydedilecek `SchoolContext` bir hizmet olarak açmak *Startup.cs*ve vurgulanan satırları ekleyin `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-223">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="c13f5-224">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir `DbContextOptionsBuilder` nesne.</span><span class="sxs-lookup"><span data-stu-id="c13f5-224">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="c13f5-225">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="c13f5-225">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="c13f5-226">Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve ardından projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-226">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="c13f5-227">Açık *appsettings.json* dosya ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-227">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="c13f5-228">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c13f5-228">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c13f5-229">Bir SQL Server LocalDB veritabanına bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-229">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="c13f5-230">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-230">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="c13f5-231">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-231">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c13f5-232">Varsayılan olarak LocalDB oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-232">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="c13f5-233">Veritabanı test verileri ile başlatmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="c13f5-233">Add code to initialize the database with test data</span></span>

<span data-ttu-id="c13f5-234">Entity Framework sizin için boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-234">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="c13f5-235">Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-235">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="c13f5-236">Burada, kullanacağınız `EnsureCreated` otomatik olarak veritabanı oluşturmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-236">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="c13f5-237">İçinde bir [sonraki öğreticide](migrations.md) bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-237">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="c13f5-238">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veri yüklerini test Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="c13f5-238">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="c13f5-239">Kod, veritabanındaki tüm Öğrenciler vardır ve aksi durumda, veritabanını yeni ve test verileri ile sağlanmış gerekiyor, varsayar denetler.</span><span class="sxs-lookup"><span data-stu-id="c13f5-239">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="c13f5-240">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="c13f5-240">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="c13f5-241">İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="c13f5-241">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="c13f5-242">Veritabanı bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-242">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="c13f5-243">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-243">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="c13f5-244">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="c13f5-244">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="c13f5-245">Ekleme `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="c13f5-245">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="c13f5-246">Önceki öğreticilerde, benzer bir kod görebilirsiniz `Configure` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c13f5-246">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="c13f5-247">Kullanmanızı öneririz `Configure` yöntemi yalnızca istek işlem hattı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-247">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="c13f5-248">Uygulama başlangıç koduna ait içinde `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-248">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="c13f5-249">Şimdi uygulamayı çalıştırdığınız ilk kez veritabanı oluşturulacak ve test verileri ile çekirdek değeri oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="c13f5-249">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="c13f5-250">Veri modelinizi değiştirdiğinizde, veritabanını silin, çekirdek yönteminizi güncelleştirin ve yeni bir veritabanı ile aynı şekilde parçalarından başlatın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-250">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="c13f5-251">Sonraki öğreticilerde, silinmesi ve yeniden oluşturulmadan değişiklik veri modelini kullanırken, veritabanı değiştirme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-251">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="c13f5-252">Bir denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="c13f5-252">Create a controller and views</span></span>

<span data-ttu-id="c13f5-253">Ardından, bir MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio yapı iskelesi altyapısı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c13f5-253">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="c13f5-254">CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-254">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="c13f5-255">Yapı iskelesi kod oluşturma farklıdır iskele kurulan kodu ise, oluşturulan kod genellikle değiştirmeyin, kendi gereksinimlerinize uyacak şekilde değiştirebileceği bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-255">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="c13f5-256">Oluşturulan kod özelleştirmeniz gerektiğinde, kısmi sınıflar kullanın veya şeyler değiştirdiğinizde, kodu yeniden oluştur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-256">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="c13f5-257">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-257">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="c13f5-258">Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c13f5-258">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="c13f5-259">[Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c13f5-259">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="c13f5-260">Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="c13f5-260">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="c13f5-261">Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-261">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="c13f5-262">İçinde **İskele Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="c13f5-262">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="c13f5-263">Seçin **Entity Framework kullanarak görünümler ile MVC denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-263">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="c13f5-264">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-264">Click **Add**.</span></span>

* <span data-ttu-id="c13f5-265">İçinde **denetleyici Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="c13f5-265">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="c13f5-266">İçinde **Model sınıfı** seçin **Öğrenci**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-266">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="c13f5-267">İçinde **veri bağlamı sınıfının** seçin **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-267">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="c13f5-268">Varsayılan değerleri kabul **StudentsController** adı.</span><span class="sxs-lookup"><span data-stu-id="c13f5-268">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="c13f5-269">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-269">Click **Add**.</span></span>

  ![İskele Öğrenci](intro/_static/scaffold-student.png)

  <span data-ttu-id="c13f5-271">Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-271">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="c13f5-272">(El ile oluşturmayın, yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-272">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="c13f5-273">Yeni bir bağlam sınıfında belirttiğiniz **denetleyici Ekle** kutusunun sağındaki artı işaretine tıklayarak **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="c13f5-273">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="c13f5-274">Visual Studio ardından oluşturur, `DbContext` sınıf denetleyici ve görünüm yanı sıra.)</span><span class="sxs-lookup"><span data-stu-id="c13f5-274">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="c13f5-275">Denetleyici aldığını fark edeceksiniz bir `SchoolContext` Oluşturucu parametresi olarak.</span><span class="sxs-lookup"><span data-stu-id="c13f5-275">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="c13f5-276">ASP.NET bağımlılık ekleme halleder örneğini geçirerek `SchoolContext` denetleyici içinde.</span><span class="sxs-lookup"><span data-stu-id="c13f5-276">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="c13f5-277">Yapılandırdığınız *Startup.cs* daha önce dosya.</span><span class="sxs-lookup"><span data-stu-id="c13f5-277">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="c13f5-278">Denetleyici içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüleyen eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-278">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="c13f5-279">Yöntemi, okuyarak ayarlamak Öğrenciler varlıktan Öğrenciler listesini alır. `Students` veritabanı bağlam örneğinin özelliği:</span><span class="sxs-lookup"><span data-stu-id="c13f5-279">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="c13f5-280">Bu kod zaman uyumsuz programlama öğeleri öğreticinin ilerleyen bölümlerinde öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-280">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="c13f5-281">*Views/Students/Index.cshtml* bu liste bir tabloda görüntüleyen:</span><span class="sxs-lookup"><span data-stu-id="c13f5-281">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="c13f5-282">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="c13f5-282">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="c13f5-283">Test verileri görmek için Öğrenciler sekmesine tıklayın, `DbInitializer.Initialize` eklenen yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-283">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="c13f5-284">Bağlı nasıl dar tarayıcı pencerenizin, gördüğünüz `Student` Gezinti bağlantıyı görmek için sağ üst köşedeki simgeyi tıklatın sayfanın veya üst kısmındaki sekme bağlantısına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c13f5-284">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Dar contoso University giriş sayfası](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="c13f5-287">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="c13f5-287">View the Database</span></span>

<span data-ttu-id="c13f5-288">Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-288">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="c13f5-289">EF gördüğünüz veritabanı vardı ve bu nedenle oluşturulan bir sonra geri kalanında `Initialize` yöntemi kod veritabanı verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-289">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="c13f5-290">Kullanabileceğiniz **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için).</span><span class="sxs-lookup"><span data-stu-id="c13f5-290">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="c13f5-291">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-291">Close the browser.</span></span>

<span data-ttu-id="c13f5-292">SSOX pencere zaten açık değilse, seçim **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="c13f5-292">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="c13f5-293">SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları**ve ardından bağlantı dizesinde veritabanının adını girişini tıklatın, *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="c13f5-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="c13f5-294">Genişletin **tabloları** veritabanınızdaki tablolar görmek için düğümü.</span><span class="sxs-lookup"><span data-stu-id="c13f5-294">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX tablolarında](intro/_static/ssox-tables.png)

<span data-ttu-id="c13f5-296">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-296">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Öğrenci tabloda SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="c13f5-298"><em>.Mdf</em> ve <em>.ldf</em> veritabanı dosyalar, <em>C:\Users\\ <yourusername> </em> klasör.</span><span class="sxs-lookup"><span data-stu-id="c13f5-298">The <em>.mdf</em> and <em>.ldf</em> database files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="c13f5-299">Aradığınız çünkü `EnsureCreated` uygulama başlatıldığında çalıştırılan Başlatıcı yönteminde artık bir değişiklik yapabileceğiniz `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması.</span><span class="sxs-lookup"><span data-stu-id="c13f5-299">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="c13f5-300">Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıfı göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablodaki sütun.</span><span class="sxs-lookup"><span data-stu-id="c13f5-300">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="c13f5-301">Kurallar</span><span class="sxs-lookup"><span data-stu-id="c13f5-301">Conventions</span></span>

<span data-ttu-id="c13f5-302">Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmanız gerekirdi kod kuralları veya Entity Framework yapan varsayımlar kullanımı nedeniyle en az olur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-302">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="c13f5-303">Adlarını `DbSet` özellikleri, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-303">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="c13f5-304">Varlık tarafından başvurulmayan bir `DbSet` özelliği, varlık sınıfı adları, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-304">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="c13f5-305">Varlık özellik adlarını sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-305">Entity property names are used for column names.</span></span>

* <span data-ttu-id="c13f5-306">Kimliği veya Classnameıd adlı varlık özellikleri birincil anahtar özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-306">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="c13f5-307">Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *<navigation property name> <primary key property name>* (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="c13f5-307">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="c13f5-308">Yabancı anahtar özellikleri de adı yalnızca *<primary key property name>* (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="c13f5-308">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="c13f5-309">Geleneksel davranışını geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-309">Conventional behavior can be overridden.</span></span> <span data-ttu-id="c13f5-310">Örneğin, bu öğreticide daha önce bahsettiğim gibi tablo adları açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-310">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="c13f5-311">Sütun adları ayarlayın ve görüşelim gibi herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="c13f5-311">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="c13f5-312">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="c13f5-312">Asynchronous code</span></span>

<span data-ttu-id="c13f5-313">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-313">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c13f5-314">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-314">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c13f5-315">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="c13f5-315">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c13f5-316">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="c13f5-316">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c13f5-317">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-317">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c13f5-318">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-318">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="c13f5-319">Zaman uyumsuz kod çalışma zamanında az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.</span><span class="sxs-lookup"><span data-stu-id="c13f5-319">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c13f5-320">Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="c13f5-320">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="c13f5-321">`async` Anahtar sözcüğü derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için `Task<IActionResult>` döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="c13f5-321">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="c13f5-322">Dönüş türü `Task<IActionResult>` türünün bir sonucu ile devam eden çalışmayı temsil `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-322">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="c13f5-323">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="c13f5-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c13f5-324">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="c13f5-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c13f5-325">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c13f5-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="c13f5-326">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c13f5-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c13f5-327">Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="c13f5-327">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="c13f5-328">Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c13f5-328">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="c13f5-329">Örneğin, içeren `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-329">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c13f5-330">Bu, örneğin, yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c13f5-330">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="c13f5-331">EF bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="c13f5-331">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="c13f5-332">Herhangi bir zaman uyumsuz EF yöntemi çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="c13f5-332">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="c13f5-333">Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.</span><span class="sxs-lookup"><span data-stu-id="c13f5-333">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="c13f5-334">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="c13f5-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="c13f5-335">Özet</span><span class="sxs-lookup"><span data-stu-id="c13f5-335">Summary</span></span>

<span data-ttu-id="c13f5-336">Depolamak ve verileri görüntülemek için Entity Framework Core ve SQL Server Express LocalDB kullanan basit bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="c13f5-336">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="c13f5-337">Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="c13f5-337">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="c13f5-338">Next</span><span class="sxs-lookup"><span data-stu-id="c13f5-338">Next</span></span>](crud.md)
