---
title: 'Öğretici: Bir ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama'
description: Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: f7b557c8e560393ae886c46fad95c48ccbcc65b4
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102974"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="7b546-103">Öğretici: Bir ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7b546-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="7b546-104">Contoso University örnek web uygulaması, Entity Framework (EF) Core 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.2 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7b546-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="7b546-105">Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="7b546-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7b546-106">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7b546-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7b546-107">Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.</span><span class="sxs-lookup"><span data-stu-id="7b546-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="7b546-108">EF Core 2.0 EF en son sürümü, ancak henüz EF özelliklerinin tümünü yok 6.x.</span><span class="sxs-lookup"><span data-stu-id="7b546-108">EF Core 2.0 is the latest version of EF but doesn't yet have all the features of EF 6.x.</span></span> <span data-ttu-id="7b546-109">EF arasında seçim yapma hakkında bilgi için bkz: 6.x ve EF Core [EF Core vs. EF6.x](/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="7b546-109">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="7b546-110">EF seçerseniz 6.x, bkz: [Bu öğretici serisinin önceki sürümünü](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="7b546-110">If you choose EF 6.x, see [the previous version of this tutorial series](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> <span data-ttu-id="7b546-111">Bu öğreticide ASP.NET Core 1.1 sürümü için bkz: [VS 2017 güncelleştirme 2 sürüm PDF biçimindeki bu öğreticinin](https://webpifeed.blob.core.windows.net/webpifeed/Partners/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="7b546-111">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://webpifeed.blob.core.windows.net/webpifeed/Partners/efmvc1.1.pdf).</span></span>

<span data-ttu-id="7b546-112">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7b546-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7b546-113">ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-113">Create ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="7b546-114">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="7b546-114">Set up the site style</span></span>
> * <span data-ttu-id="7b546-115">EF Core NuGet paketleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="7b546-115">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="7b546-116">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-116">Create the data model</span></span>
> * <span data-ttu-id="7b546-117">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="7b546-117">Create the database context</span></span>
> * <span data-ttu-id="7b546-118">SchoolContext kaydetme</span><span class="sxs-lookup"><span data-stu-id="7b546-118">Register the SchoolContext</span></span>
> * <span data-ttu-id="7b546-119">DB test verileri ile başlatılamıyor</span><span class="sxs-lookup"><span data-stu-id="7b546-119">Initialize DB with test data</span></span>
> * <span data-ttu-id="7b546-120">Denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-120">Create controller and views</span></span>
> * <span data-ttu-id="7b546-121">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="7b546-121">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b546-122">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7b546-122">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a><span data-ttu-id="7b546-123">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="7b546-123">Troubleshooting</span></span>

<span data-ttu-id="7b546-124">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="7b546-124">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="7b546-125">Sık karşılaşılan hatalar ve bunları çözmek nasıl bir listesi için bkz. [serinin son Öğreticisi sorun giderme bölümünü](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="7b546-125">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="7b546-126">Var. aradığınızı bulamazsanız, StackOverflow.com için için soru gönderebildiği [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7b546-126">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7b546-127">Önceki öğreticilerde bitti üzerinde her biri yapılar 10 öğreticiler, bir dizi budur.</span><span class="sxs-lookup"><span data-stu-id="7b546-127">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7b546-128">Her öğretici Kurulum başarıyla tamamlandıktan sonra projenin bir kopyasını kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-128">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7b546-129">Sorunlarla karşılaşırsanız, ardından yeniden yukarıda bahsedilen tüm dizileri başlangıcına yerine önceki öğreticiden başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-129">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="7b546-130">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="7b546-130">Contoso University web app</span></span>

<span data-ttu-id="7b546-131">Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="7b546-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="7b546-132">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7b546-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7b546-133">Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7b546-133">Here are a few of the screens you'll create.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="7b546-136">Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.</span><span class="sxs-lookup"><span data-stu-id="7b546-136">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-aspnet-core-mvc-web-app"></a><span data-ttu-id="7b546-137">ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-137">Create ASP.NET Core MVC web app</span></span>

<span data-ttu-id="7b546-138">Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir ASP.NET Core C# web projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7b546-138">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="7b546-139">Gelen **dosya** menüsünde **yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="7b546-139">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="7b546-140">Sol bölmeden **yüklü > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="7b546-140">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="7b546-141">Seçin **ASP.NET Core Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="7b546-141">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="7b546-142">Girin **ContosoUniversity** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7b546-142">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project2.png)

* <span data-ttu-id="7b546-144">Bekle **yeni ASP.NET Core Web uygulaması (.NET Core)** görüntülenecek iletişim</span><span class="sxs-lookup"><span data-stu-id="7b546-144">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet2.png)

* <span data-ttu-id="7b546-146">Seçin **ASP.NET Core 2.2** ve **Web uygulaması (Model-View-Controller)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="7b546-146">Select **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="7b546-147">**Not:** Bu öğretici, ASP.NET Core 2.2 ve EF Core 2.0 veya sonraki sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7b546-147">**Note:** This tutorial requires ASP.NET Core 2.2 and EF Core 2.0 or later.</span></span>

* <span data-ttu-id="7b546-148">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="7b546-148">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="7b546-149">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7b546-149">Click **OK**</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="7b546-150">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="7b546-150">Set up the site style</span></span>

<span data-ttu-id="7b546-151">Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.</span><span class="sxs-lookup"><span data-stu-id="7b546-151">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7b546-152">Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="7b546-152">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7b546-153">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7b546-153">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="7b546-154">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="7b546-154">There are three occurrences.</span></span>

* <span data-ttu-id="7b546-155">Menü girdileri eklemek **hakkında**, **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**, ve silme **gizlilik** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="7b546-155">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="7b546-156">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="7b546-156">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,32-36,51)]

<span data-ttu-id="7b546-157">İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7b546-157">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="7b546-158">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="7b546-158">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="7b546-159">Aşağıdaki öğreticilerde oluşturacaksınız sayfaları için sekmeli giriş sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7b546-159">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="7b546-161">EF Core NuGet paketleri hakkında</span><span class="sxs-lookup"><span data-stu-id="7b546-161">About EF Core NuGet packages</span></span>

<span data-ttu-id="7b546-162">EF Core desteği için bir proje eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-162">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="7b546-163">Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7b546-163">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="7b546-164">Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa paket başvurusu yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App` paket.</span><span class="sxs-lookup"><span data-stu-id="7b546-164">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package if your app has a package reference for the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="7b546-165">Bu paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="7b546-165">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="7b546-166">Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) öğretici.</span><span class="sxs-lookup"><span data-stu-id="7b546-166">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="7b546-167">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="7b546-167">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="7b546-168">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-168">Create the data model</span></span>

<span data-ttu-id="7b546-169">Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7b546-169">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="7b546-170">Aşağıdaki üç varlıklar ile başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="7b546-170">You'll start with the following three entities.</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="7b546-172">Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="7b546-172">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7b546-173">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-173">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7b546-174">Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7b546-174">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7b546-175">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="7b546-175">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="7b546-177">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7b546-177">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7b546-178">`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="7b546-178">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="7b546-179">Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="7b546-179">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="7b546-180">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="7b546-180">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="7b546-181">Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun.</span><span class="sxs-lookup"><span data-stu-id="7b546-181">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="7b546-182">Bu durumda, `Enrollments` özelliği bir `Student entity` tümünü tutacak `Enrollment` olarak ilişkili varlıkları `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="7b546-182">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="7b546-183">Diğer bir deyişle, belirli bir öğrenci satır veritabanında iki kayıt satırları (birincil anahtar değeri kendi StudentID yabancı anahtar sütunu, Öğrenci içeren satırlar) ilgili olan, `Student` varlığın `Enrollments` gezinti özelliği bu içerecek iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="7b546-183">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="7b546-184">Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="7b546-184">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="7b546-185">Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="7b546-185">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7b546-186">Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="7b546-186">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7b546-187">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="7b546-187">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="7b546-189">İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7b546-189">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7b546-190">`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="7b546-190">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="7b546-191">Genellikle bir düzen seçin ve veri modelinizi kullanılmakta.</span><span class="sxs-lookup"><span data-stu-id="7b546-191">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="7b546-192">Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7b546-192">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="7b546-193">İçinde bir [sonraki öğreticide](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7b546-193">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7b546-194">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="7b546-194">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7b546-195">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-195">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7b546-196">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="7b546-196">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7b546-197">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b546-197">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7b546-198">Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).</span><span class="sxs-lookup"><span data-stu-id="7b546-198">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="7b546-199">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="7b546-199">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7b546-200">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="7b546-200">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7b546-201">Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="7b546-201">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7b546-202">Yabancı anahtar özellikleri de adı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="7b546-202">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7b546-203">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="7b546-203">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="7b546-205">İçinde *modelleri* klasör oluşturma *Course.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7b546-205">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7b546-206">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="7b546-206">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7b546-207">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="7b546-207">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7b546-208">Daha fazla ilgili dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="7b546-208">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="7b546-209">Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7b546-209">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="7b546-210">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="7b546-210">Create the database context</span></span>

<span data-ttu-id="7b546-211">Verilen veri modeli için Entity Framework işlevselliği koordine eden ana veritabanı bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="7b546-211">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="7b546-212">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7b546-212">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="7b546-213">Kodunuzda hangi varlıkları veri modelinde yer alan belirtin.</span><span class="sxs-lookup"><span data-stu-id="7b546-213">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="7b546-214">Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-214">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="7b546-215">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7b546-215">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7b546-216">Proje klasöründe adlı bir klasör oluşturun *veri*.</span><span class="sxs-lookup"><span data-stu-id="7b546-216">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="7b546-217">İçinde *veri* klasör oluşturma adlı yeni bir sınıf dosyası *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7b546-217">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7b546-218">Bu kod oluşturur bir `DbSet` her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="7b546-218">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7b546-219">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="7b546-219">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7b546-220">Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7b546-220">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="7b546-221">Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="7b546-221">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="7b546-222">Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları.</span><span class="sxs-lookup"><span data-stu-id="7b546-222">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7b546-223">Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında.</span><span class="sxs-lookup"><span data-stu-id="7b546-223">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="7b546-224">Bu öğreticiler için bir DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-224">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="7b546-225">Bunu yapmak için son olan DB özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-225">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="7b546-226">SchoolContext kaydetme</span><span class="sxs-lookup"><span data-stu-id="7b546-226">Register the SchoolContext</span></span>

<span data-ttu-id="7b546-227">ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="7b546-227">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="7b546-228">Hizmetler (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-228">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7b546-229">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="7b546-229">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7b546-230">Bu öğreticide daha sonra bir bağlam örneğini alır denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7b546-230">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="7b546-231">Kaydedilecek `SchoolContext` bir hizmet olarak açmak *Startup.cs*ve vurgulanan satırları ekleyin `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-231">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="7b546-232">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir `DbContextOptionsBuilder` nesne.</span><span class="sxs-lookup"><span data-stu-id="7b546-232">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7b546-233">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7b546-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7b546-234">Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve ardından projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-234">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="7b546-235">Açık *appsettings.json* dosya ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-235">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7b546-236">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7b546-236">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7b546-237">Bir SQL Server LocalDB veritabanına bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="7b546-237">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="7b546-238">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7b546-238">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="7b546-239">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="7b546-239">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7b546-240">Varsayılan olarak LocalDB oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="7b546-240">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="7b546-241">DB test verileri ile başlatılamıyor</span><span class="sxs-lookup"><span data-stu-id="7b546-241">Initialize DB with test data</span></span>

<span data-ttu-id="7b546-242">Entity Framework sizin için boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7b546-242">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="7b546-243">Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="7b546-243">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="7b546-244">Burada, kullanacağınız `EnsureCreated` otomatik olarak veritabanı oluşturmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-244">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="7b546-245">İçinde bir [sonraki öğreticide](migrations.md) bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7b546-245">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="7b546-246">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veri yüklerini test Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="7b546-246">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7b546-247">Kod, veritabanındaki tüm Öğrenciler vardır ve aksi durumda, veritabanını yeni ve test verileri ile sağlanmış gerekiyor, varsayar denetler.</span><span class="sxs-lookup"><span data-stu-id="7b546-247">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="7b546-248">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="7b546-248">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7b546-249">İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="7b546-249">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="7b546-250">Veritabanı bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="7b546-250">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7b546-251">Bağlam iletmeden seed yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="7b546-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7b546-252">Seed yöntemi tamamlandığında bağlamını siler.</span><span class="sxs-lookup"><span data-stu-id="7b546-252">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="7b546-253">Ekleme `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="7b546-253">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="7b546-254">Önceki öğreticilerde, benzer bir kod görebilirsiniz `Configure` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7b546-254">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="7b546-255">Kullanmanızı öneririz `Configure` yöntemi yalnızca istek işlem hattı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7b546-255">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="7b546-256">Uygulama başlangıç koduna ait içinde `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-256">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="7b546-257">Şimdi uygulamayı çalıştırdığınız ilk kez veritabanı oluşturulacak ve test verileri ile çekirdek değeri oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="7b546-257">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="7b546-258">Veri modelinizi değiştirdiğinizde, veritabanını silin, çekirdek yönteminizi güncelleştirin ve yeni bir veritabanı ile aynı şekilde parçalarından başlatın.</span><span class="sxs-lookup"><span data-stu-id="7b546-258">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="7b546-259">Sonraki öğreticilerde, silinmesi ve yeniden oluşturulmadan değişiklik veri modelini kullanırken, veritabanı değiştirme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7b546-259">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="7b546-260">Denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b546-260">Create controller and views</span></span>

<span data-ttu-id="7b546-261">Ardından, bir MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio yapı iskelesi altyapısı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7b546-261">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="7b546-262">CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="7b546-262">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="7b546-263">Yapı iskelesi kod oluşturma farklıdır iskele kurulan kodu ise, oluşturulan kod genellikle değiştirmeyin, kendi gereksinimlerinize uyacak şekilde değiştirebileceği bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="7b546-263">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="7b546-264">Oluşturulan kod özelleştirmeniz gerektiğinde, kısmi sınıflar kullanın veya şeyler değiştirdiğinizde, kodu yeniden oluştur.</span><span class="sxs-lookup"><span data-stu-id="7b546-264">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="7b546-265">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="7b546-265">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="7b546-266">Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7b546-266">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="7b546-267">[Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="7b546-267">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="7b546-268">Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="7b546-268">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="7b546-269">Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.</span><span class="sxs-lookup"><span data-stu-id="7b546-269">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="7b546-270">İçinde **İskele Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="7b546-270">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="7b546-271">Seçin **Entity Framework kullanarak görünümler ile MVC denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="7b546-271">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="7b546-272">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7b546-272">Click **Add**.</span></span> <span data-ttu-id="7b546-273">**Ekleme Entity Framework kullanarak görünümlere sahip MVC denetleyici,** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7b546-273">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![İskele Öğrenci](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="7b546-275">İçinde **Model sınıfı** seçin **Öğrenci**.</span><span class="sxs-lookup"><span data-stu-id="7b546-275">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="7b546-276">İçinde **veri bağlamı sınıfının** seçin **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="7b546-276">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="7b546-277">Varsayılan değerleri kabul **StudentsController** adı.</span><span class="sxs-lookup"><span data-stu-id="7b546-277">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="7b546-278">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7b546-278">Click **Add**.</span></span>

  <span data-ttu-id="7b546-279">Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="7b546-279">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="7b546-280">(El ile oluşturmayın, yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi.</span><span class="sxs-lookup"><span data-stu-id="7b546-280">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="7b546-281">Yeni bir bağlam sınıfında belirttiğiniz **denetleyici Ekle** kutusunun sağındaki artı işaretine tıklayarak **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="7b546-281">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="7b546-282">Visual Studio ardından oluşturur, `DbContext` sınıf denetleyici ve görünüm yanı sıra.)</span><span class="sxs-lookup"><span data-stu-id="7b546-282">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="7b546-283">Denetleyici aldığını fark edeceksiniz bir `SchoolContext` Oluşturucu parametresi olarak.</span><span class="sxs-lookup"><span data-stu-id="7b546-283">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="7b546-284">ASP.NET Core bağımlılık ekleme örneğini geçirerek üstlenir `SchoolContext` denetleyici içinde.</span><span class="sxs-lookup"><span data-stu-id="7b546-284">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="7b546-285">Yapılandırdığınız *Startup.cs* daha önce dosya.</span><span class="sxs-lookup"><span data-stu-id="7b546-285">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="7b546-286">Denetleyici içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüleyen eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-286">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="7b546-287">Yöntemi, okuyarak ayarlamak Öğrenciler varlıktan Öğrenciler listesini alır. `Students` veritabanı bağlam örneğinin özelliği:</span><span class="sxs-lookup"><span data-stu-id="7b546-287">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="7b546-288">Bu kod zaman uyumsuz programlama öğeleri öğreticinin ilerleyen bölümlerinde öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-288">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="7b546-289">*Views/Students/Index.cshtml* bu liste bir tabloda görüntüleyen:</span><span class="sxs-lookup"><span data-stu-id="7b546-289">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="7b546-290">Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="7b546-290">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="7b546-291">Test verileri görmek için Öğrenciler sekmesine tıklayın, `DbInitializer.Initialize` eklenen yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-291">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="7b546-292">Bağlı nasıl dar tarayıcı pencerenizin, gördüğünüz `Student` Gezinti bağlantıyı görmek için sağ üst köşedeki simgeyi tıklatın sayfanın veya üst kısmındaki sekme bağlantısına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7b546-292">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Dar contoso University giriş sayfası](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="7b546-295">Veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="7b546-295">View the database</span></span>

<span data-ttu-id="7b546-296">Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="7b546-296">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="7b546-297">EF gördüğünüz veritabanı vardı ve bu nedenle oluşturulan bir sonra geri kalanında `Initialize` yöntemi kod veritabanı verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="7b546-297">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="7b546-298">Kullanabileceğiniz **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için).</span><span class="sxs-lookup"><span data-stu-id="7b546-298">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="7b546-299">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="7b546-299">Close the browser.</span></span>

<span data-ttu-id="7b546-300">SSOX pencere zaten açık değilse, seçim **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="7b546-300">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="7b546-301">SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları**ve ardından bağlantı dizesinde veritabanının adını girişini tıklatın, *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7b546-301">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="7b546-302">Genişletin **tabloları** veritabanınızdaki tablolar görmek için düğümü.</span><span class="sxs-lookup"><span data-stu-id="7b546-302">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX tablolarında](intro/_static/ssox-tables.png)

<span data-ttu-id="7b546-304">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-304">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Öğrenci tabloda SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="7b546-306"><em>.Mdf</em> ve <em>.ldf</em> veritabanı dosyalar, <em>C:\Users\\ <yourusername> </em> klasör.</span><span class="sxs-lookup"><span data-stu-id="7b546-306">The <em>.mdf</em> and <em>.ldf</em> database files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="7b546-307">Aradığınız çünkü `EnsureCreated` uygulama başlatıldığında çalıştırılan Başlatıcı yönteminde artık bir değişiklik yapabileceğiniz `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması.</span><span class="sxs-lookup"><span data-stu-id="7b546-307">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="7b546-308">Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıfı göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablodaki sütun.</span><span class="sxs-lookup"><span data-stu-id="7b546-308">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="7b546-309">Kurallar</span><span class="sxs-lookup"><span data-stu-id="7b546-309">Conventions</span></span>

<span data-ttu-id="7b546-310">Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmanız gerekirdi kod kuralları veya Entity Framework yapan varsayımlar kullanımı nedeniyle en az olur.</span><span class="sxs-lookup"><span data-stu-id="7b546-310">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="7b546-311">Adlarını `DbSet` özellikleri, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7b546-311">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7b546-312">Varlık tarafından başvurulmayan bir `DbSet` özelliği, varlık sınıfı adları, tablo adları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7b546-312">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="7b546-313">Varlık özellik adlarını sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7b546-313">Entity property names are used for column names.</span></span>

* <span data-ttu-id="7b546-314">Kimliği veya Classnameıd adlı varlık özellikleri birincil anahtar özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-314">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="7b546-315">Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *<navigation property name> <primary key property name>* (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="7b546-315">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7b546-316">Yabancı anahtar özellikleri de adı yalnızca *<primary key property name>* (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="7b546-316">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="7b546-317">Geleneksel davranışını geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-317">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7b546-318">Örneğin, bu öğreticide daha önce bahsettiğim gibi tablo adları açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b546-318">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="7b546-319">Sütun adları ayarlayın ve görüşelim gibi herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğreticide](complex-data-model.md) bu seri.</span><span class="sxs-lookup"><span data-stu-id="7b546-319">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7b546-320">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="7b546-320">Asynchronous code</span></span>

<span data-ttu-id="7b546-321">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="7b546-321">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7b546-322">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-322">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7b546-323">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="7b546-323">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7b546-324">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="7b546-324">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7b546-325">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="7b546-325">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7b546-326">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-326">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7b546-327">Zaman uyumsuz kod çalışma zamanında az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.</span><span class="sxs-lookup"><span data-stu-id="7b546-327">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7b546-328">Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="7b546-328">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7b546-329">`async` Anahtar sözcüğü derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için `Task<IActionResult>` döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="7b546-329">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="7b546-330">Dönüş türü `Task<IActionResult>` türünün bir sonucu ile devam eden çalışmayı temsil `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="7b546-330">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="7b546-331">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="7b546-331">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7b546-332">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="7b546-332">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7b546-333">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7b546-333">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="7b546-334">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b546-334">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7b546-335">Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="7b546-335">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="7b546-336">Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="7b546-336">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="7b546-337">Örneğin, içeren `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="7b546-337">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7b546-338">Bu, örneğin, yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="7b546-338">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="7b546-339">EF bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-339">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="7b546-340">Herhangi bir zaman uyumsuz EF yöntemi çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="7b546-340">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="7b546-341">Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.</span><span class="sxs-lookup"><span data-stu-id="7b546-341">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="7b546-342">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="7b546-342">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="7b546-343">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="7b546-343">Get the code</span></span>

[<span data-ttu-id="7b546-344">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="7b546-344">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="7b546-345">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7b546-345">Next steps</span></span>

<span data-ttu-id="7b546-346">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7b546-346">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7b546-347">Oluşturulan ASP.NET Core MVC web uygulaması</span><span class="sxs-lookup"><span data-stu-id="7b546-347">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="7b546-348">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="7b546-348">Set up the site style</span></span>
> * <span data-ttu-id="7b546-349">EF Core NuGet paketleri hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="7b546-349">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="7b546-350">Veri modeli oluşturduk</span><span class="sxs-lookup"><span data-stu-id="7b546-350">Created the data model</span></span>
> * <span data-ttu-id="7b546-351">Veritabanı bağlamı oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="7b546-351">Created the database context</span></span>
> * <span data-ttu-id="7b546-352">SchoolContext kayıtlı</span><span class="sxs-lookup"><span data-stu-id="7b546-352">Registered the SchoolContext</span></span>
> * <span data-ttu-id="7b546-353">Test verileri ile başlatılmış DB</span><span class="sxs-lookup"><span data-stu-id="7b546-353">Initialized DB with test data</span></span>
> * <span data-ttu-id="7b546-354">Oluşturulan denetleyici ve Görünüm</span><span class="sxs-lookup"><span data-stu-id="7b546-354">Created controller and views</span></span>
> * <span data-ttu-id="7b546-355">Veritabanı görüntülenebilir</span><span class="sxs-lookup"><span data-stu-id="7b546-355">Viewed the database</span></span>

<span data-ttu-id="7b546-356">Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="7b546-356">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="7b546-357">Temel CRUD gerçekleştirme hakkında bilgi edinmek için sonraki makaleye ilerleyin (oluşturma, okuma, güncelleştirme ve silme) işlemleri.</span><span class="sxs-lookup"><span data-stu-id="7b546-357">Advance to the next article to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7b546-358">Temel CRUD işlevselliği uygulama</span><span class="sxs-lookup"><span data-stu-id="7b546-358">Implement basic CRUD functionality</span></span>](crud.md)
