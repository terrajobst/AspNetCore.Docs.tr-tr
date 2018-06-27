---
title: Razor sayfalarının Entity Framework Çekirdek ASP.NET Core - 8'in Öğreticisi 1 ile
author: rick-anderson
description: Entity Framework Çekirdek kullanarak Razor sayfalarının uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 97ae8a0f268d3ca6fb2deee72128714ab6e89266
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961366"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="cb3f1-103">Razor sayfalarının Entity Framework Çekirdek ASP.NET Core - 8'in Öğreticisi 1 ile</span><span class="sxs-lookup"><span data-stu-id="cb3f1-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="cb3f1-104">Bu öğretici ASP.NET Core 2.0 sürümünü bulunabilir [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cb3f1-105">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cb3f1-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cb3f1-106">Contoso University örnek web uygulaması Entity Framework (EF) çekirdeği kullanarak bir ASP.NET Core Razor sayfalarının uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-106">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="cb3f1-107">Örnek uygulaması, kurgusal bir Contoso üniversite için bir web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="cb3f1-108">Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="cb3f1-109">Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan eğitim serileri ilk sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="cb3f1-110">İndirme veya tamamlanan uygulama görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="cb3f1-111">[Yükleme yönergeleri](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb3f1-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cb3f1-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cb3f1-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb3f1-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cb3f1-114">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="cb3f1-114">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cb3f1-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cb3f1-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cb3f1-116">[! INCLUDE [] (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="cb3f1-116">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

------

<span data-ttu-id="cb3f1-117">Aşina [Razor sayfalarının](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-117">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="cb3f1-118">Yeni programcıları tamamlamanız gereken [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) bu seri başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-118">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="cb3f1-119">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="cb3f1-119">Troubleshooting</span></span>

<span data-ttu-id="cb3f1-120">Olamaz çözmek bir sorunla çalıştırırsanız, genellikle çözümün kodunuzu karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-120">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="cb3f1-121">Bir soruyu göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF çekirdek](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-121">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="cb3f1-122">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="cb3f1-122">The Contoso University web app</span></span>

<span data-ttu-id="cb3f1-123">Bu öğreticiler oluşturulmuş uygulamaya bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="cb3f1-124">Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="cb3f1-125">Öğreticide oluşturulan ekranlar bazılarını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-125">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler Düzenle sayfası](intro/_static/student-edit.png)

<span data-ttu-id="cb3f1-128">Bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın ' dir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="cb3f1-129">Razor sayfalarının, UI EF çekirdek üzerinde öğretici odak noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="cb3f1-130">ContosoUniversity Razor sayfalarının web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb3f1-130">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cb3f1-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb3f1-131">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cb3f1-132">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="cb3f1-133">Yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="cb3f1-134">Proje adı **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="cb3f1-135">Proje adı önemlidir *ContosoUniversity* kod kopyalanıp yapıştırılmış ad alanları eşleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="cb3f1-136">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-136">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="cb3f1-137">Yukarıdaki adımları görüntüleri için bkz: [bir Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-137">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="cb3f1-138">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-138">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cb3f1-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cb3f1-139">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="cb3f1-140">Site stil ayarlama</span><span class="sxs-lookup"><span data-stu-id="cb3f1-140">Set up the site style</span></span>

<span data-ttu-id="cb3f1-141">Birkaç değişiklik sitesi menüsü, Düzen ve giriş sayfası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-141">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="cb3f1-142">Güncelleştirme *Pages/Shared/_Layout.cshtml* aşağıdaki değişiklikleri:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-142">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="cb3f1-143">Her oluşumu "ContosoUniversity", "Contoso University" değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-143">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="cb3f1-144">Üç oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-144">There are three occurrences.</span></span>

* <span data-ttu-id="cb3f1-145">Menü girişlerini eklemek **Öğrenciler**, **kurslar**, **Eğitmen**, ve **Departmanlar**ve silme **kişi** menü girişi.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="cb3f1-146">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-146">The changes are highlighted.</span></span> <span data-ttu-id="cb3f1-147">(Tüm biçimlendirme *değil* görüntülenir.)</span><span class="sxs-lookup"><span data-stu-id="cb3f1-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="cb3f1-148">İçinde *Pages/Index.cshtml*, dosyanın içeriğini bu uygulama hakkında metinle ASP.NET MVC hakkında metni değiştirmek için aşağıdaki kod ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="cb3f1-149">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb3f1-149">Create the data model</span></span>

<span data-ttu-id="cb3f1-150">Sınıflar Contoso University uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="cb3f1-151">Aşağıdaki üç varlıklarıyla başlatın:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-151">Start with the following three entities:</span></span>

![İndirmelere kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="cb3f1-153">Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="cb3f1-154">Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="cb3f1-155">Öğrencinin herhangi bir sayıda kurslar kaydedebilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="cb3f1-156">Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="cb3f1-157">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="cb3f1-158">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="cb3f1-158">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="cb3f1-160">Oluşturma bir *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-160">Create a *Models* folder.</span></span> <span data-ttu-id="cb3f1-161">İçinde *modelleri* klasörünü adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="cb3f1-162">`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu haline gelir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="cb3f1-163">Varsayılan olarak, EF çekirdek adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="cb3f1-164">İçinde `classnameID`, `classname` sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-164">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="cb3f1-165">Birincil anahtar alternatif otomatik olarak kabul edilen `StudentID` önceki örnekte.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-165">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="cb3f1-166">`Enrollments` Özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="cb3f1-167">Bu varlığa ilgili diğer varlıklar için Gezinti özellikleri bağlayın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="cb3f1-168">Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` için ilişkili olan varlıkların `Student`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="cb3f1-169">Örneğin, bir öğrenci satır varsa DB iki kayıt, ilişkili satırları sahip `Enrollments` gezinti özelliği içerdiğinden bu iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="cb3f1-170">İlgili `Enrollment` satırıdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="cb3f1-171">Örneğin, Öğrenci kimlikli varsayalım = 1 sahip iki satır `Enrollment` tablo.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="cb3f1-172">`Enrollment` Tablolu sahip iki satır `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="cb3f1-173">`StudentID` bir yabancı anahtar `Enrollment` Öğrenci belirtir tablo `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="cb3f1-174">Bir gezinme özelliği birden çok varlık tutarsanız gezinti özelliği bir liste türü gibi olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="cb3f1-175">`ICollection<T>` belirtilebilir, ya da bir türü `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="cb3f1-176">Zaman `ICollection<T>` olduğu EF çekirdek oluşturur kullanıldığında, bir `HashSet<T>` varsayılan koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="cb3f1-177">Birden çok varlık tutun Gezinti özellikleri çok- ve -çok ilişkileri gelir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="cb3f1-178">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="cb3f1-178">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="cb3f1-180">İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="cb3f1-181">`EnrollmentID` Birincil anahtarı bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="cb3f1-182">Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="cb3f1-183">Genellikle geliştiriciler bir deseni seçin ve veri modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="cb3f1-184">Sonraki öğreticide, veri modelinde devralma uygulamak daha kolay classname Kimliğini kullanarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="cb3f1-185">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="cb3f1-186">Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="cb3f1-187">Null bir düzeyde bir sıfır ataması farklı.--null anlamına gelir bir düzeyde bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="cb3f1-188">`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="cb3f1-189">Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özelliği içerecek şekilde varlık `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="cb3f1-190">`Student` Varlık farklı olarak `Student.Enrollments` birden çok içeren gezinti özelliği `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="cb3f1-191">`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="cb3f1-192">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="cb3f1-193">EF çekirdek, ise bu özellik yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="cb3f1-194">Örneğin,`StudentID` için `Student` gezinti özelliği, bu yana `Student` varlığın birincil anahtarının `ID`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="cb3f1-195">Yabancı anahtar özellikleri de adlı `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="cb3f1-196">Örneğin, `CourseID` beri `Course` varlığın birincil anahtarının `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="cb3f1-197">İndirmelere varlık</span><span class="sxs-lookup"><span data-stu-id="cb3f1-197">The Course entity</span></span>

![İndirmelere varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="cb3f1-199">İçinde *modelleri* klasörü oluşturmak *Course.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="cb3f1-200">`Enrollments` Özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="cb3f1-201">A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="cb3f1-202">`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek uygulama izin verir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="cb3f1-203">İskele Öğrenci modeli</span><span class="sxs-lookup"><span data-stu-id="cb3f1-203">Scaffold the student model</span></span>

<span data-ttu-id="cb3f1-204">Bu bölümde, Öğrenci modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-204">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="cb3f1-205">Diğer bir deyişle, yapı iskelesi aracı Öğrenci modeli için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleri için sayfaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-205">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="cb3f1-206">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-206">Build the project.</span></span>
* <span data-ttu-id="cb3f1-207">Oluşturma *sayfaları/Öğrenciler* klasör.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-207">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cb3f1-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb3f1-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cb3f1-209">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/Öğrenciler* klasörü > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-209">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="cb3f1-210">İçinde **İskele Ekle** iletişim kutusunda **Razor Entity Framework (CRUD) kullanarak sayfaları** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-210">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="cb3f1-211">Tamamlamak **Razor Entity Framework (CRUD) kullanarak Sayfa Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-211">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="cb3f1-212">İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-212">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="cb3f1-213">İçinde **veri bağlamı sınıfı** satır, select **+** (artı) oturum açın ve oluşturulan adı değiştirmek **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-213">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="cb3f1-214">İçinde **veri bağlamı sınıfı** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="cb3f1-214">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="cb3f1-215">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-215">Select **Add**.</span></span>

![CRUD iletişim](intro/_static/s1.png)

<span data-ttu-id="cb3f1-217">Bkz: [film modeli İskele](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-217">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cb3f1-218">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cb3f1-218">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cb3f1-219">Öğrenci modeli iskele için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-219">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="cb3f1-220">İskele işlemi oluşturulan ve aşağıdaki dosyaları değişti:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-220">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="cb3f1-221">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="cb3f1-221">Files created</span></span>

* <span data-ttu-id="cb3f1-222">*Sayfa/Öğrenciler* oluşturma, silme, ayrıntı, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-222">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="cb3f1-223">*Data/ContosoUniversityContext.cs*</span><span class="sxs-lookup"><span data-stu-id="cb3f1-223">*Data/ContosoUniversityContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="cb3f1-224">Güncelleştirme dosyaları</span><span class="sxs-lookup"><span data-stu-id="cb3f1-224">Files updates</span></span>

* <span data-ttu-id="cb3f1-225">*Haline* : Bu dosyada yapılan değişiklikler ayrıntılı bir sonraki bölüm.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-225">*Startup.cs* : Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="cb3f1-226">*appSettings.JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklendi.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-226">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="cb3f1-227">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="cb3f1-227">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="cb3f1-228">ASP.NET Core ile oluşturulan [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-228">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cb3f1-229">Hizmetleri (örneğin, EF çekirdek DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-229">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="cb3f1-230">Bu Hizmetleri (örneğin, Razor sayfalarının) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-230">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="cb3f1-231">Bir db bağlamı örneği alır Oluşturucusu kodu daha sonra öğreticide gösterilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-231">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="cb3f1-232">Yapı iskelesi Aracı'nı otomatik olarak bir veritabanı bağlamını oluşturulur ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-232">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="cb3f1-233">İncelemek `ConfigureServices` yönteminde *haline*.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-233">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="cb3f1-234">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-234">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="cb3f1-235">Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-235">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="cb3f1-236">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-236">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="cb3f1-237">Ana güncelleştir</span><span class="sxs-lookup"><span data-stu-id="cb3f1-237">Update main</span></span>

<span data-ttu-id="cb3f1-238">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-238">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="cb3f1-239">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcıdan alın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-239">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="cb3f1-240">Çağrı [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-240">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="cb3f1-241">Bağlamda dispose zaman `EnsureCreated` yöntemi tamamlar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-241">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="cb3f1-242">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-242">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="cb3f1-243">`EnsureCreated` Veritabanı bağlamı için varolduğunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-243">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="cb3f1-244">Varsa, hiçbir işlem yapılmadı.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-244">If it exists, no action is taken.</span></span> <span data-ttu-id="cb3f1-245">Yoksa, tüm şema ve veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-245">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="cb3f1-246">`EnsureCreated` geçişler, veritabanı oluşturmak için kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-246">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="cb3f1-247">İle oluşturulmuş bir veritabanı `EnsureCreated` daha sonra geçişler kullanılarak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-247">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="cb3f1-248">`EnsureCreated` Aşağıdaki iş akışı sağlayan uygulama Başlat'çağrılır:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-248">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="cb3f1-249">DB silin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-249">Delete the DB.</span></span>
* <span data-ttu-id="cb3f1-250">DB şeması değiştirin (örneğin, bir `EmailAddress` alan).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-250">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="cb3f1-251">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-251">Run the app.</span></span>
* <span data-ttu-id="cb3f1-252">`EnsureCreated` bir DB ile oluşturur`EmailAddress` sütun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-252">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="cb3f1-253">`EnsureCreated` Şema hızla gelişen erken geliştirme yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-253">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="cb3f1-254">Daha sonra öğreticide DB silinir ve geçişleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-254">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="cb3f1-255">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="cb3f1-255">Test the app</span></span>

<span data-ttu-id="cb3f1-256">Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-256">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="cb3f1-257">Bu uygulamayı, kişisel bilgi korumayarak.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-257">This app doesn't keep personal information.</span></span> <span data-ttu-id="cb3f1-258">Tanımlama bilgisi ilkesi hakkında bilgi edinebilirsiniz [AB genel veri koruma düzenleme (GDPR) Destek](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-258">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="cb3f1-259">Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-259">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="cb3f1-260">Düzenleme, Ayrıntılar, test ve bağlantıları silin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-260">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="cb3f1-261">SchoolContext DB bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="cb3f1-261">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="cb3f1-262">Verilen veri modeli için EF temel işlevleri koordinatları ana sınıfı DB bağlamı sınıftır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-262">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="cb3f1-263">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-263">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="cb3f1-264">Veri bağlamı hangi varlıkların veri modelinde dahil edildiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-264">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="cb3f1-265">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-265">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="cb3f1-266">Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-266">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="cb3f1-267">Vurgulanmış kodu oluşturur bir [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği her bir varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-267">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="cb3f1-268">EF çekirdek terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-268">In EF Core terminology:</span></span>

* <span data-ttu-id="cb3f1-269">Bir varlık kümesine genellikle DB tabloya karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-269">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="cb3f1-270">Bir varlık tablosunda bir satırı karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-270">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="cb3f1-271">`DbSet<Enrollment>` ve `DbSet<Course>` kullanılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-271">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="cb3f1-272">EF çekirdek içerir bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-272">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="cb3f1-273">Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-273">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="cb3f1-274">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="cb3f1-274">SQL Server Express LocalDB</span></span>

<span data-ttu-id="cb3f1-275">Bağlantı dizesi belirtir [SQL Server yerel veritabanı](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-275">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="cb3f1-276">Yerel veritabanı, SQL Server Express veritabanı Motoru'nu hafif sürümüdür ve uygulama geliştirme, üretim kullanımı için amaçlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-276">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="cb3f1-277">Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-277">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="cb3f1-278">Varsayılan olarak, yerel veritabanı oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-278">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="cb3f1-279">DB test verileri ile başlatmak için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="cb3f1-279">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="cb3f1-280">EF çekirdek boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-280">EF Core creates an empty DB.</span></span> <span data-ttu-id="cb3f1-281">Bu bölümde, bir `Initialize` yöntemi test veri ile doldurulacak yazılır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-281">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="cb3f1-282">İçinde *veri* klasörünü adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-282">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="cb3f1-283">Kod DB'de herhangi Öğrenciler olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="cb3f1-284">DB içinde hiçbir Öğrenciler varsa, DB test verilerle başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="cb3f1-285">Diziye test verileri yükler yerine `List<T>` performansı iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="cb3f1-286">`EnsureCreated` Yöntemi DB DB bağlamı için otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="cb3f1-287">Veritabanı varsa `EnsureCreated` DB değiştirmeden döndürür.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="cb3f1-288">İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="cb3f1-289">Tüm Öğrenci kayıtlarını silin ve uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="cb3f1-290">DB başlatılmadı, bir kesme noktası kümesinde `Initialize` sorunu tanılamak için.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="cb3f1-291">DB görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="cb3f1-291">View the DB</span></span>

<span data-ttu-id="cb3f1-292">Açık **SQL Server Nesne Gezgini** (SSOX) gelen **Görünüm** Visual Studio menüsünde.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="cb3f1-293">SSOX içinde tıklatın **(localdb) \MSSQLLocalDB > veritabanları > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="cb3f1-294">Genişletme **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="cb3f1-295">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunlar ve tabloya eklenen satır görmek için.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="cb3f1-296">Zaman uyumsuz kodu</span><span class="sxs-lookup"><span data-stu-id="cb3f1-296">Asynchronous code</span></span>

<span data-ttu-id="cb3f1-297">Zaman uyumsuz programlama ASP.NET Core ve EF çekirdek için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="cb3f1-298">Bir web sunucusu kullanılabilir iş parçacıklarının sınırlı sayıda sahiptir ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="cb3f1-299">Bu durum oluştuğunda sunucu yukarı serbest iş parçacığı kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="cb3f1-300">G/ç tamamlamak bekliyorsunuz çünkü bunlar herhangi bir iş gerçekte yapmamanız sırasında zaman uyumlu koduyla birçok iş parçacığı bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="cb3f1-301">Bir işlemin g/ç tamamlanması beklenirken zaman uyumsuz koduyla diğer istekleri işlemek için kullanılacak sunucuyu kaydınızı kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="cb3f1-302">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarını daha verimli bir şekilde kullanılmak üzere etkinleştirir ve sunucu gecikme olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="cb3f1-303">Zaman uyumsuz kod yükünü az miktarda çalışma zamanında tanıtır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="cb3f1-304">Trafiğin düşük durumlar için performans isabet yüksek trafik durumlarda düşünülerek, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="cb3f1-305">Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütme kodu olun.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="cb3f1-306">`async` Anahtar sözcüğü derleyiciye bildirir:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="cb3f1-307">Geri aramalar yöntemi gövde bölümlerinin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="cb3f1-308">Otomatik olarak oluştur [görev](/dotnet/api/system.threading.tasks.task) döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="cb3f1-309">Daha fazla bilgi için bkz: [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="cb3f1-310">Örtük dönüş türü `Task` devam eden iş temsil eder.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="cb3f1-311">`await` Anahtar sözcüğü yöntemi iki parçalara bölmek derleyici neden olur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="cb3f1-312">İlk bölümü ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="cb3f1-313">İkinci bölümü işlem tamamlandığında çağrılan bir geri çağırma yöntemi konur.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="cb3f1-314">`ToListAsync` zaman uyumsuz sürümü `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="cb3f1-315">EF çekirdek kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="cb3f1-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="cb3f1-316">Sorguları ya da Veritabanına gönderilen komutları neden deyimleri zaman uyumsuz olarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="cb3f1-317">İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="cb3f1-318">Yalnızca değiştirme deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="cb3f1-319">İş parçacığı içinde korumalı bir EF çekirdek bağlamı değil: paralel birden çok işlemleri yapmak denemeyin.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="cb3f1-320">Zaman uyumsuz kod performans yararlarını yararlanmak için DB sorguları göndermek EF çekirdek yöntemleri çağırırsanız paketleri kitaplığı (disk belleği ettirilmesi gibi) zaman uyumsuz kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="cb3f1-321">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz: [zaman uyumsuz genel bakış](/dotnet/articles/standard/async) ve [zaman uyumsuz programlama ile zaman uyumsuz ve bekleme](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="cb3f1-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="cb3f1-322">Sonraki öğreticide, temel CRUD (Oluştur, oku, Güncelleştir, Sil) işlemleri incelenmesini.</span><span class="sxs-lookup"><span data-stu-id="cb3f1-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="cb3f1-323">Next</span><span class="sxs-lookup"><span data-stu-id="cb3f1-323">Next</span></span>](xref:data/ef-rp/crud)