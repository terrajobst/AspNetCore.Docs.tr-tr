---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: rick-anderson
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: a234d5fefd671d4503f6c63b79074d47c893f69c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207712"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="8650a-103">ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="8650a-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8650a-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8650a-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8650a-105">Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8650a-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="8650a-106">Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="8650a-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="8650a-107">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="8650a-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="8650a-108">Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.</span><span class="sxs-lookup"><span data-stu-id="8650a-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="8650a-109">İndirme veya tamamlanmış uygulamayı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="8650a-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="8650a-110">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="8650a-110">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8650a-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8650a-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8650a-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8650a-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8650a-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8650a-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="8650a-114">Konusunda [Razor sayfaları](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="8650a-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="8650a-115">Yeni programcılar tamamlamanız gereken [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Bu seriyi başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="8650a-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="8650a-116">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="8650a-116">Troubleshooting</span></span>

<span data-ttu-id="8650a-117">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="8650a-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="8650a-118">Soru göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="8650a-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="8650a-119">Contoso University web uygulaması</span><span class="sxs-lookup"><span data-stu-id="8650a-119">The Contoso University web app</span></span>

<span data-ttu-id="8650a-120">Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="8650a-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="8650a-121">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8650a-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="8650a-122">Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8650a-122">Here are a few of the screens created in the tutorial.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

<span data-ttu-id="8650a-125">Bu sitenin UI Stili yerleşik şablonları tarafından üretilen yakın ' dir.</span><span class="sxs-lookup"><span data-stu-id="8650a-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="8650a-126">EF çekirdekli Razor sayfaları, UI ile öğretici odağı açıktır.</span><span class="sxs-lookup"><span data-stu-id="8650a-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="8650a-127">ContosoUniversity Razor sayfaları web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8650a-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8650a-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8650a-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8650a-129">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="8650a-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8650a-130">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8650a-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="8650a-131">Projeyi adlandırın **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="8650a-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="8650a-132">Projeyi adlandırın önemlidir *ContosoUniversity* kod kopyalanıp/yapıştırılmış ad alanları eşleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="8650a-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="8650a-133">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8650a-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="8650a-134">Önceki adımlarda görüntüleri için bkz: [Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="8650a-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="8650a-135">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8650a-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8650a-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8650a-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="8650a-137">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="8650a-137">Set up the site style</span></span>

<span data-ttu-id="8650a-138">Birkaç değişiklik site menü, Düzen ve giriş sayfası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8650a-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="8650a-139">Güncelleştirme *Pages/Shared/_Layout.cshtml* aşağıdaki değişikliklerle birlikte:</span><span class="sxs-lookup"><span data-stu-id="8650a-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="8650a-140">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8650a-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="8650a-141">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8650a-141">There are three occurrences.</span></span>

* <span data-ttu-id="8650a-142">Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.</span><span class="sxs-lookup"><span data-stu-id="8650a-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="8650a-143">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="8650a-143">The changes are highlighted.</span></span> <span data-ttu-id="8650a-144">(Tüm biçimlendirme *değil* görüntülenir.)</span><span class="sxs-lookup"><span data-stu-id="8650a-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="8650a-145">İçinde *sayfalar/dizin.cshtml*, dosyanın içeriğini ASP.NET ve MVC hakkında metnin bu uygulama hakkında metinle değiştirmek için aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8650a-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="8650a-146">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="8650a-146">Create the data model</span></span>

<span data-ttu-id="8650a-147">Varlık sınıflarının Contoso University uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8650a-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="8650a-148">Aşağıdaki üç varlıklarla başlayın:</span><span class="sxs-lookup"><span data-stu-id="8650a-148">Start with the following three entities:</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="8650a-150">Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8650a-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="8650a-151">Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8650a-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="8650a-152">Bir öğrenci herhangi bir sayıda kursları kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8650a-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="8650a-153">Bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="8650a-154">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8650a-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="8650a-155">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="8650a-155">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="8650a-157">Oluşturma bir *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="8650a-157">Create a *Models* folder.</span></span> <span data-ttu-id="8650a-158">İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8650a-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="8650a-159">`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="8650a-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="8650a-160">Varsayılan olarak EF Core adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.</span><span class="sxs-lookup"><span data-stu-id="8650a-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="8650a-161">İçinde `classnameID`, `classname` sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="8650a-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="8650a-162">Alternatif birincil anahtarı otomatik olarak kabul edilen `StudentID` önceki örnekte.</span><span class="sxs-lookup"><span data-stu-id="8650a-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="8650a-163">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationship).</span><span class="sxs-lookup"><span data-stu-id="8650a-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationship).</span></span> <span data-ttu-id="8650a-164">Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın.</span><span class="sxs-lookup"><span data-stu-id="8650a-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="8650a-165">Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` olarak ilişkili varlıkları `Student`.</span><span class="sxs-lookup"><span data-stu-id="8650a-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="8650a-166">Örneğin, bir öğrenci satır DB'de iki kayıt satırları ilgili olan `Enrollments` gezinti özelliği içerir, bu iki `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8650a-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="8650a-167">İlgili `Enrollment` satırdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun.</span><span class="sxs-lookup"><span data-stu-id="8650a-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="8650a-168">Örneğin, Öğrenci kimlikli varsayalım = 1 olan iki satır `Enrollment` tablo.</span><span class="sxs-lookup"><span data-stu-id="8650a-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="8650a-169">`Enrollment` Tablosunda var olan iki satır `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="8650a-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="8650a-170">`StudentID` içinde bir yabancı anahtar `Enrollment` içinde Öğrenci belirten tablo `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="8650a-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="8650a-171">Bir gezinme özelliği birden çok varlık tutarsanız gezinme özelliğini bir liste türü gibi olmalıdır `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="8650a-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="8650a-172">`ICollection<T>` belirtilebilir, ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="8650a-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="8650a-173">Zaman `ICollection<T>` olduğu EF Core kullanıldığında, oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="8650a-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="8650a-174">Birden çok varlık tutun Gezinti özellikleri çoktan çoğa ve bire çok ilişkileri gelir.</span><span class="sxs-lookup"><span data-stu-id="8650a-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="8650a-175">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="8650a-175">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="8650a-177">İçinde *modelleri* klasör oluşturma *Enrollment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8650a-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="8650a-178">`EnrollmentID` Birincil anahtar özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="8650a-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="8650a-179">Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="8650a-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="8650a-180">Genellikle geliştiriciler bir düzen seçin ve veri modelini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8650a-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="8650a-181">Bir sonraki öğreticide, classname Kimliğini kullanarak, veri modelinde aktarma uygulamak daha kolay hale getirmek için gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="8650a-182">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="8650a-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="8650a-183">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="8650a-184">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="8650a-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="8650a-185">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="8650a-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="8650a-186">Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özellik içerecek şekilde varlık `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="8650a-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="8650a-187">`Student` Varlık farklıdır `Student.Enrollments` içeren birden çok gezinti özelliği `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8650a-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="8650a-188">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="8650a-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="8650a-189">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="8650a-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="8650a-190">EF Core adlandırılmışsa, bu özellik bir yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="8650a-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="8650a-191">Örneğin,`StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`.</span><span class="sxs-lookup"><span data-stu-id="8650a-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="8650a-192">Yabancı anahtar özellikleri de adı `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="8650a-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="8650a-193">Örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="8650a-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="8650a-194">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="8650a-194">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="8650a-196">İçinde *modelleri* klasör oluşturma *Course.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8650a-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="8650a-197">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="8650a-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="8650a-198">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8650a-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="8650a-199">`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek için uygulamayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8650a-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="8650a-200">İskele Öğrenci modeli</span><span class="sxs-lookup"><span data-stu-id="8650a-200">Scaffold the student model</span></span>

<span data-ttu-id="8650a-201">Bu bölümde, Öğrenci modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="8650a-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="8650a-202">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik Öğrenci modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8650a-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="8650a-203">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8650a-203">Build the project.</span></span>
* <span data-ttu-id="8650a-204">Oluşturma *sayfaları/Öğrenciler* klasör.</span><span class="sxs-lookup"><span data-stu-id="8650a-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8650a-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8650a-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8650a-206">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/Öğrenciler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="8650a-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="8650a-207">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="8650a-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="8650a-208">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="8650a-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="8650a-209">İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="8650a-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="8650a-210">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan bir adla değiştirin **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="8650a-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="8650a-211">İçinde **veri bağlamı sınıfının** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="8650a-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="8650a-212">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="8650a-212">Select **Add**.</span></span>

![CRUD iletişim](intro/_static/s1.png)

<span data-ttu-id="8650a-214">Bkz: [film modeli iskelesini](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.</span><span class="sxs-lookup"><span data-stu-id="8650a-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8650a-215">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8650a-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8650a-216">Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8650a-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="8650a-217">İskele işlem oluşturulur ve aşağıdaki dosya değişti:</span><span class="sxs-lookup"><span data-stu-id="8650a-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="8650a-218">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="8650a-218">Files created</span></span>

* <span data-ttu-id="8650a-219">*Sayfa/Öğrenciler* oluşturma, silme, Ayrıntılar, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="8650a-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="8650a-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="8650a-220">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="8650a-221">Dosya güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="8650a-221">File updates</span></span>

* <span data-ttu-id="8650a-222">*Startup.cs* : Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılı.</span><span class="sxs-lookup"><span data-stu-id="8650a-222">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="8650a-223">*appSettings.JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="8650a-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="8650a-224">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="8650a-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="8650a-225">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8650a-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8650a-226">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8650a-227">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="8650a-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="8650a-228">Bir db bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8650a-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="8650a-229">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="8650a-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="8650a-230">İnceleme `ConfigureServices` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8650a-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="8650a-231">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="8650a-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="8650a-232">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="8650a-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="8650a-233">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="8650a-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="8650a-234">Ana güncelleştir</span><span class="sxs-lookup"><span data-stu-id="8650a-234">Update main</span></span>

<span data-ttu-id="8650a-235">İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:</span><span class="sxs-lookup"><span data-stu-id="8650a-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="8650a-236">Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.</span><span class="sxs-lookup"><span data-stu-id="8650a-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="8650a-237">Çağrı [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="8650a-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="8650a-238">Bağlam dispose olduğunda `EnsureCreated` yöntemi tamamlar.</span><span class="sxs-lookup"><span data-stu-id="8650a-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="8650a-239">Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="8650a-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="8650a-240">`EnsureCreated` Veritabanı bağlamının var olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="8650a-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="8650a-241">Varsa, hiçbir işlem yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="8650a-241">If it exists, no action is taken.</span></span> <span data-ttu-id="8650a-242">Yoksa, veritabanı ve tüm şema oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8650a-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="8650a-243">`EnsureCreated` veritabanı oluşturmaya geçişleri kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="8650a-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="8650a-244">İle oluşturulmuş bir veritabanı `EnsureCreated` daha sonra geçişleri kullanılarak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="8650a-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="8650a-245">`EnsureCreated` Aşağıdaki iş akışı sağlayan uygulama Başlat menüsünde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="8650a-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="8650a-246">DB silin.</span><span class="sxs-lookup"><span data-stu-id="8650a-246">Delete the DB.</span></span>
* <span data-ttu-id="8650a-247">DB şema değiştirme (örneğin, bir `EmailAddress` alan).</span><span class="sxs-lookup"><span data-stu-id="8650a-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="8650a-248">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8650a-248">Run the app.</span></span>
* <span data-ttu-id="8650a-249">`EnsureCreated` bir DB ile oluşturur`EmailAddress` sütun.</span><span class="sxs-lookup"><span data-stu-id="8650a-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="8650a-250">`EnsureCreated` Şema hızla gelişirken erken geliştirme uygundur.</span><span class="sxs-lookup"><span data-stu-id="8650a-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="8650a-251">Daha sonra öğreticide DB silinir ve geçişler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8650a-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="8650a-252">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="8650a-252">Test the app</span></span>

<span data-ttu-id="8650a-253">Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="8650a-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="8650a-254">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="8650a-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="8650a-255">Tanımlama bilgisi ilkesi hakkında bilgi edinebilirsiniz [AB genel veri koruma yönetmeliği (GDPR) Destek](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="8650a-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="8650a-256">Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="8650a-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="8650a-257">Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="8650a-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="8650a-258">SchoolContext DB bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="8650a-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="8650a-259">Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="8650a-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="8650a-260">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="8650a-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="8650a-261">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="8650a-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="8650a-262">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="8650a-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="8650a-263">Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8650a-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="8650a-264">Vurgulanan kod oluşturur bir [olan DB\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) her varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="8650a-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="8650a-265">EF Core terminolojisinde:</span><span class="sxs-lookup"><span data-stu-id="8650a-265">In EF Core terminology:</span></span>

* <span data-ttu-id="8650a-266">Bir varlık kümesini genellikle DB tabloya karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8650a-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="8650a-267">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8650a-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="8650a-268">`DbSet<Enrollment>` ve `DbSet<Course>` atlanmış.</span><span class="sxs-lookup"><span data-stu-id="8650a-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="8650a-269">EF Core içeren bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlığı ve `Enrollment` varlık başvuruları `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="8650a-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="8650a-270">Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="8650a-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="8650a-271">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="8650a-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="8650a-272">Bağlantı dizesini belirtir [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="8650a-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="8650a-273">LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8650a-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="8650a-274">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="8650a-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="8650a-275">Varsayılan olarak LocalDB oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.</span><span class="sxs-lookup"><span data-stu-id="8650a-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="8650a-276">Bir veritabanı test verileri ile başlatmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="8650a-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="8650a-277">EF Core boş bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8650a-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="8650a-278">Bu bölümde, bir `Initialize` yöntemi test verileriyle doldurma işlemine yazılır.</span><span class="sxs-lookup"><span data-stu-id="8650a-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="8650a-279">İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8650a-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="8650a-280">Kod DB'de tüm Öğrenciler olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="8650a-280">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="8650a-281">DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="8650a-281">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="8650a-282">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="8650a-282">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="8650a-283">`EnsureCreated` Yöntemi DB bağlamı için bir veritabanı otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8650a-283">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="8650a-284">Veritabanı varsa, `EnsureCreated` DB değiştirmeden döndürür.</span><span class="sxs-lookup"><span data-stu-id="8650a-284">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="8650a-285">İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="8650a-285">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="8650a-286">Tüm Öğrenci kayıtlarını silin ve uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="8650a-286">Delete any student records and restart the app.</span></span> <span data-ttu-id="8650a-287">DB başlatılmadı, bir kesme noktası ayarlayın `Initialize` sorunu tanılamak için.</span><span class="sxs-lookup"><span data-stu-id="8650a-287">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="8650a-288">DB görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="8650a-288">View the DB</span></span>

<span data-ttu-id="8650a-289">Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="8650a-289">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="8650a-290">SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="8650a-290">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="8650a-291">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="8650a-291">Expand the **Tables** node.</span></span>

<span data-ttu-id="8650a-292">Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8650a-292">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="8650a-293">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="8650a-293">Asynchronous code</span></span>

<span data-ttu-id="8650a-294">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="8650a-294">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="8650a-295">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-295">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="8650a-296">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="8650a-296">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="8650a-297">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="8650a-297">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="8650a-298">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="8650a-298">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="8650a-299">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-299">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="8650a-300">Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar.</span><span class="sxs-lookup"><span data-stu-id="8650a-300">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="8650a-301">Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.</span><span class="sxs-lookup"><span data-stu-id="8650a-301">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="8650a-302">Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.</span><span class="sxs-lookup"><span data-stu-id="8650a-302">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="8650a-303">`async` Anahtar sözcüğü, derleyiciye bildirir:</span><span class="sxs-lookup"><span data-stu-id="8650a-303">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="8650a-304">Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8650a-304">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="8650a-305">Otomatik olarak oluşturmasını [görev](/dotnet/api/system.threading.tasks.task) döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="8650a-305">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="8650a-306">Daha fazla bilgi için [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="8650a-306">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="8650a-307">Örtük dönüş türü `Task` devam eden çalışmayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8650a-307">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="8650a-308">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="8650a-308">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="8650a-309">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="8650a-309">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="8650a-310">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8650a-310">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="8650a-311">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8650a-311">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="8650a-312">EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8650a-312">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="8650a-313">Sorguları veya Veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="8650a-313">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="8650a-314">İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="8650a-314">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="8650a-315">Yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="8650a-315">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="8650a-316">EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.</span><span class="sxs-lookup"><span data-stu-id="8650a-316">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="8650a-317">Zaman uyumsuz kodun performans avantajlarından yararlanmak için bunlar için bir veritabanı sorguları göndermek EF Core yöntemleri çağırırsanız kitaplığı paketlerinin (disk belleği sunamıyoruz gibi) zaman uyumsuz kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8650a-317">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="8650a-318">. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/articles/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="8650a-318">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="8650a-319">Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.</span><span class="sxs-lookup"><span data-stu-id="8650a-319">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="8650a-320">Next</span><span class="sxs-lookup"><span data-stu-id="8650a-320">Next</span></span>](xref:data/ef-rp/crud)
