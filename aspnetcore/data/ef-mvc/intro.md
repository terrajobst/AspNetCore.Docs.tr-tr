---
title: 'Öğretici: ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama'
description: Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.
author: tdykstra
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: 1b68c20ba206a5afe36f307525879f91d03d95d1
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583334"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="474d9-103">Öğretici: ASP.NET MVC web uygulamasında EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="474d9-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="474d9-104">Contoso Üniversitesi örnek Web uygulaması, Entity Framework (EF) Core 2,2 ve Visual Studio 2017 veya 2019 kullanarak ASP.NET Core 2,2 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="474d9-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="474d9-105">Örnek uygulama, kurgusal bir Contoso Üniversitesi için bir Web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="474d9-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="474d9-106">Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="474d9-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="474d9-107">Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="474d9-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="474d9-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="474d9-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="474d9-109">ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-109">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="474d9-110">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="474d9-110">Set up the site style</span></span>
> * <span data-ttu-id="474d9-111">EF Core NuGet paketleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="474d9-111">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="474d9-112">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-112">Create the data model</span></span>
> * <span data-ttu-id="474d9-113">Veritabanı bağlamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-113">Create the database context</span></span>
> * <span data-ttu-id="474d9-114">Bağımlılık ekleme için bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="474d9-114">Register the context for dependency injection</span></span>
> * <span data-ttu-id="474d9-115">Test verileriyle veritabanını başlatma</span><span class="sxs-lookup"><span data-stu-id="474d9-115">Initialize the database with test data</span></span>
> * <span data-ttu-id="474d9-116">Denetleyici ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-116">Create a controller and views</span></span>
> * <span data-ttu-id="474d9-117">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="474d9-117">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="474d9-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="474d9-118">Prerequisites</span></span>

* [<span data-ttu-id="474d9-119">.NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="474d9-119">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="474d9-120">Aşağıdaki iş yükleriyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) :</span><span class="sxs-lookup"><span data-stu-id="474d9-120">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="474d9-121">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="474d9-121">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="474d9-122">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="474d9-122">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="474d9-123">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="474d9-123">Troubleshooting</span></span>

<span data-ttu-id="474d9-124">Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="474d9-124">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="474d9-125">Yaygın hataların bir listesi ve bunların nasıl çözüleceği için, [serideki son öğreticinin sorun giderme bölümüne](advanced.md#common-errors)bakın.</span><span class="sxs-lookup"><span data-stu-id="474d9-125">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="474d9-126">İhtiyacınız olanları bulamazsanız, [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)için bir soru gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-126">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="474d9-127">Bu, her biri daha önceki öğreticilerde gerçekleştirilen bir dizi 10 öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="474d9-127">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="474d9-128">Her başarılı öğreticinin tamamlanmasından sonra projenin bir kopyasını kaydetmeyi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="474d9-128">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="474d9-129">Daha sonra sorunlarla karşılaşırsanız, tüm serinin başlangıcına dönmek yerine önceki öğreticiden baştan başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-129">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="474d9-130">Contoso Üniversitesi web uygulaması</span><span class="sxs-lookup"><span data-stu-id="474d9-130">Contoso University web app</span></span>

<span data-ttu-id="474d9-131">Bu öğreticilerde oluşturacağınız uygulama basit bir üniversite web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="474d9-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="474d9-132">Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="474d9-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="474d9-133">Oluşturacağınız ekranların bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="474d9-133">Here are a few of the screens you'll create.</span></span>

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="474d9-136">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-136">Create web app</span></span>

* <span data-ttu-id="474d9-137">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="474d9-137">Open Visual Studio.</span></span>

* <span data-ttu-id="474d9-138">**Dosya** menüsünden **Yeni > proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-138">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="474d9-139">Sol bölmeden, **Visual C# > Web > yüklü**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-139">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="474d9-140">**ASP.NET Core Web uygulaması** proje şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-140">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="474d9-141">Ad olarak **Contosouniversity** yazın ve **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="474d9-141">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project2.png)

* <span data-ttu-id="474d9-143">**Yeni ASP.NET Core Web uygulaması** iletişim kutusunun görünmesini bekleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-143">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="474d9-144">**.NET Core**, **ASP.NET Core 2,2** ve **Web uygulaması (Model-View-Controller)** şablonu ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-144">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="474d9-145">**Kimlik doğrulamanın** **kimlik doğrulaması yok**olarak ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="474d9-145">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="474d9-146">**Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="474d9-146">Select **OK**</span></span>

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="474d9-148">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="474d9-148">Set up the site style</span></span>

<span data-ttu-id="474d9-149">Birkaç basit değişiklik, site menüsünü, düzeni ve giriş sayfasını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="474d9-149">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="474d9-150">*Views/Shared/_Layout. cshtml* dosyasını açın ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="474d9-150">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="474d9-151">"Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="474d9-151">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="474d9-152">Üç örnekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="474d9-152">There are three occurrences.</span></span>

* <span data-ttu-id="474d9-153">**Hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **Gizlilik** menü girişini silin.</span><span class="sxs-lookup"><span data-stu-id="474d9-153">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="474d9-154">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="474d9-154">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="474d9-155">*Görünümler/Home/Index. cshtml*'de, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="474d9-155">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="474d9-156">Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-156">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="474d9-157">Bu öğreticilerde oluşturacağınız sayfaların sekmelerini içeren giriş sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="474d9-157">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso Üniversitesi ana sayfası](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="474d9-159">NuGet paketleri EF Core hakkında</span><span class="sxs-lookup"><span data-stu-id="474d9-159">About EF Core NuGet packages</span></span>

<span data-ttu-id="474d9-160">Bir projeye EF Core destek eklemek için hedeflemek istediğiniz veritabanı sağlayıcısını yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-160">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="474d9-161">Bu öğretici SQL Server kullanır ve sağlayıcı paketi [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)' dır.</span><span class="sxs-lookup"><span data-stu-id="474d9-161">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="474d9-162">Bu paket [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur, bu nedenle pakete başvurmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="474d9-162">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="474d9-163">EF SQL Server paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF için çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="474d9-163">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="474d9-164">[Geçiş](migrations.md) öğreticisinde daha sonra bir araç paketi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-164">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="474d9-165">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="474d9-165">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="474d9-166">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-166">Create the data model</span></span>

<span data-ttu-id="474d9-167">Daha sonra Contoso Üniversitesi uygulaması için varlık sınıfları oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-167">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="474d9-168">Aşağıdaki üç varlıkla başlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-168">You'll start with the following three entities.</span></span>

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="474d9-170">Ve `Student` `Course` `Enrollment` varlıkları arasında bire çok ilişki vardır ve ile varlıklar arasında bire çok ilişki vardır. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="474d9-170">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="474d9-171">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursa kaydedilebilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-171">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="474d9-172">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-172">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="474d9-173">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="474d9-173">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="474d9-175">*Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyası oluşturun ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="474d9-175">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="474d9-176">Özelliği `ID` , bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="474d9-176">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="474d9-177">Varsayılan olarak Entity Framework, birincil anahtar olarak veya `ID` `classnameID` adında bir özelliği yorumlar.</span><span class="sxs-lookup"><span data-stu-id="474d9-177">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="474d9-178">`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="474d9-178">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="474d9-179">Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar.</span><span class="sxs-lookup"><span data-stu-id="474d9-179">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="474d9-180">Bu `Enrollments` durumda, `Student entity` öğesinin özelliği bu `Student` varlıkla ilgili `Enrollment` varlıkların tümünü tutacaktır.</span><span class="sxs-lookup"><span data-stu-id="474d9-180">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="474d9-181">Diğer bir `Student` deyişle, veritabanında verilen bir öğrenci satırında iki ilişkili kayıt satırı varsa (bu öğrencinin birincil anahtar değerini kendi studentitıd yabancı anahtar sütununda içeren satırlar) varsa, söz konusu `Enrollments` varlığın gezinti özelliği bunları içerir iki `Enrollment` varlık.</span><span class="sxs-lookup"><span data-stu-id="474d9-181">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="474d9-182">Bir gezinti özelliği birden çok varlığı tutabileceiyorsa (çok-çok veya bire çok ilişkilerde olduğu gibi), türü girişlerin eklenebileceği, silinebileceği ve güncelleştirilemeyebilir `ICollection<T>`bir liste olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="474d9-182">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="474d9-183">Veya gibi bir `ICollection<T>` tür `List<T>` `HashSet<T>`belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-183">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="474d9-184">Belirtirseniz `ICollection<T>`, EF varsayılan olarak bir `HashSet<T>` koleksiyon oluşturur.</span><span class="sxs-lookup"><span data-stu-id="474d9-184">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="474d9-185">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="474d9-185">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="474d9-187">*Modeller* klasöründe *enrollment.cs* oluşturun ve mevcut kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="474d9-187">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="474d9-188">Özelliği birincil anahtar olacaktır; bu varlık, `Student` varlıkta gördüğünüz gibi kendi `classnameID` başına bir `ID` model kullanır. `EnrollmentID`</span><span class="sxs-lookup"><span data-stu-id="474d9-188">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="474d9-189">Normalde tek bir model seçip veri modeliniz genelinde kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-189">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="474d9-190">Burada, değişim, her iki stili de kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="474d9-190">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="474d9-191">[Daha sonraki bir öğreticide](inheritance.md), ID 'yi ClassName olmadan kullanarak, veri modelinde devralma uygulamayı daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="474d9-191">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="474d9-192">`Grade` Özelliği bir `enum`.</span><span class="sxs-lookup"><span data-stu-id="474d9-192">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="474d9-193">Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-193">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="474d9-194">Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.</span><span class="sxs-lookup"><span data-stu-id="474d9-194">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="474d9-195">`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`.</span><span class="sxs-lookup"><span data-stu-id="474d9-195">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="474d9-196">`Student` `Student` `Student.Enrollments` Bir varlık bir varlıkla ilişkilendirilir, bu nedenle özellik yalnızca tek bir varlığı tutabilir (daha önce gördüğünüz gezinti özelliğinden farklı olarak, birden çok `Enrollment` varlık tutabilir). `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="474d9-196">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="474d9-197">`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`.</span><span class="sxs-lookup"><span data-stu-id="474d9-197">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="474d9-198">Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.</span><span class="sxs-lookup"><span data-stu-id="474d9-198">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="474d9-199">Entity Framework, bir özelliği bir yabancı anahtar `<navigation property name><primary key property name>` özelliği olarak (örneğin, `Student` `StudentID` varlığın birincil anahtarından bu yana `Student` gezinti `ID`özelliği için) olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="474d9-199">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="474d9-200">Yabancı anahtar `<primary key property name>` özellikleri de yalnızca adlandırılmış olabilir (örneğin, `CourseID` `Course` varlığın birincil anahtarı olduğundan `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="474d9-200">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="474d9-201">Kurs varlık</span><span class="sxs-lookup"><span data-stu-id="474d9-201">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="474d9-203">*Modeller* klasöründe *Course.cs* oluşturun ve mevcut kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="474d9-203">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="474d9-204">`Enrollments` Özelliktir bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="474d9-204">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="474d9-205">A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="474d9-205">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="474d9-206">Bu serinin `DatabaseGenerated` sonraki bir [öğreticide](complex-data-model.md) özniteliği hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-206">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="474d9-207">Temel olarak bu öznitelik, veritabanının oluşturması yerine kursa ait birincil anahtarı girmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="474d9-207">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="474d9-208">Veritabanı bağlamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-208">Create the database context</span></span>

<span data-ttu-id="474d9-209">Belirli bir veri modeli için Entity Framework işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="474d9-209">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="474d9-210">`Microsoft.EntityFrameworkCore.DbContext` Sınıfından türeterek bu sınıfı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="474d9-210">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="474d9-211">Kodunuzda, veri modeline hangi varlıkların ekleneceğini belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-211">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="474d9-212">Ayrıca, belirli Entity Framework davranışlarını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-212">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="474d9-213">Bu projede adlı sınıfı `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="474d9-213">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="474d9-214">Proje klasöründe, *veri*adlı bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="474d9-214">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="474d9-215">*Veri* klasöründe, *SchoolContext.cs*adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="474d9-215">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="474d9-216">Bu kod, her `DbSet` bir varlık kümesi için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="474d9-216">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="474d9-217">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="474d9-217">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="474d9-218">`DbSet<Enrollment>` Ve`DbSet<Course>` deyimlerini atlamış olabilirsiniz ve aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="474d9-218">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="474d9-219">Entity Framework, `Student` varlık `Enrollment` varlığa `Enrollment` başvurduğundan`Course` ve varlık varlığa başvurduğundan bunları örtülü olarak içerebilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-219">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="474d9-220">Veritabanı oluşturulduğunda EF, `DbSet` Özellik adlarıyla aynı adlara sahip tablolar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="474d9-220">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="474d9-221">Koleksiyonlar için özellik adları genellikle plural (öğrenci yerine öğrenciler), ancak geliştiriciler tablo adlarının plurulululmasını kabul etmez.</span><span class="sxs-lookup"><span data-stu-id="474d9-221">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="474d9-222">Bu öğreticiler için DbContext içinde tekil tablo adları belirterek varsayılan davranışı geçersiz kılarsınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-222">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="474d9-223">Bunu yapmak için, son DbSet özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-223">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="474d9-224">SchoolContext kaydetme</span><span class="sxs-lookup"><span data-stu-id="474d9-224">Register the SchoolContext</span></span>

<span data-ttu-id="474d9-225">ASP.NET Core, varsayılan olarak [bağımlılık ekleme](../../fundamentals/dependency-injection.md) işlemini uygular.</span><span class="sxs-lookup"><span data-stu-id="474d9-225">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="474d9-226">Hizmetler (EF veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-226">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="474d9-227">Bu hizmetleri gerektiren bileşenler (MVC denetleyicileri gibi) bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="474d9-227">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="474d9-228">Bu öğreticide daha sonra bir bağlam örneği alan denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="474d9-228">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="474d9-229">Hizmet olarak `SchoolContext` kaydetmek için *Startup.cs*açın ve `ConfigureServices` vurgulanan satırları yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-229">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="474d9-230">Bağlantı dizesinin adı bir `DbContextOptionsBuilder` nesne üzerinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-230">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="474d9-231">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="474d9-231">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="474d9-232">`ContosoUniversity.Data` Ve `using` adalanlarıiçindeyimlerekleyinveardındanprojeyiderleyin`Microsoft.EntityFrameworkCore` .</span><span class="sxs-lookup"><span data-stu-id="474d9-232">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="474d9-233">*AppSettings. JSON* dosyasını açın ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-233">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="474d9-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="474d9-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="474d9-235">Bağlantı dizesi bir SQL Server LocalDB veritabanı belirtir.</span><span class="sxs-lookup"><span data-stu-id="474d9-235">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="474d9-236">LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="474d9-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="474d9-237">LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="474d9-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="474d9-238">Varsayılan olarak, LocalDB `C:/Users/<user>` dizinde *. mdf* veritabanı dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="474d9-238">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="474d9-239">Test verileriyle VERITABANıNı başlatma</span><span class="sxs-lookup"><span data-stu-id="474d9-239">Initialize DB with test data</span></span>

<span data-ttu-id="474d9-240">Entity Framework, sizin için boş bir veritabanı oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="474d9-240">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="474d9-241">Bu bölümde, test verileriyle doldurmak için veritabanı oluşturulduktan sonra çağrılan bir yöntem yazarsınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-241">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="474d9-242">Burada, `EnsureCreated` veritabanını otomatik olarak oluşturmak için yöntemini kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-242">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="474d9-243">Sonraki bir [öğreticide](migrations.md) , veritabanını bırakıp yeniden oluşturmak yerine veritabanı şemasını değiştirmek için Code First Migrations kullanarak model değişikliklerini nasıl işleyeceğinizi göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-243">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="474d9-244">*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu, gerektiğinde bir veritabanının oluşturulmasına neden olan aşağıdaki kodla değiştirin ve test verilerini yeni veritabanına yükler.</span><span class="sxs-lookup"><span data-stu-id="474d9-244">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="474d9-245">Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler ve yoksa, veritabanının yeni olduğunu ve test verileriyle hazırlanması gerektiğini varsayar.</span><span class="sxs-lookup"><span data-stu-id="474d9-245">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="474d9-246">Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="474d9-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="474d9-247">*Program.cs*' de, uygulama `Main` başlangıcında aşağıdakini yapmak için yöntemini değiştirin:</span><span class="sxs-lookup"><span data-stu-id="474d9-247">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="474d9-248">Bağımlılık ekleme kapsayıcısından bir veritabanı bağlamı örneği alın.</span><span class="sxs-lookup"><span data-stu-id="474d9-248">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="474d9-249">Temel yöntemi çağırın ve bu yönteme geçerek bağlamı geçer.</span><span class="sxs-lookup"><span data-stu-id="474d9-249">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="474d9-250">Çekirdek yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="474d9-250">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="474d9-251">Deyim `using` ekle:</span><span class="sxs-lookup"><span data-stu-id="474d9-251">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="474d9-252">Eski öğreticilerde, `Configure` *Startup.cs*içinde yönteminde benzer bir kod görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-252">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="474d9-253">`Configure` Yöntemini yalnızca istek ardışık düzenini ayarlamak için kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="474d9-253">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="474d9-254">Uygulama başlangıç kodu `Main` yöntemine aittir.</span><span class="sxs-lookup"><span data-stu-id="474d9-254">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="474d9-255">Uygulamayı ilk kez çalıştırdığınızda veritabanı oluşturulur ve test verileriyle birlikte gösterilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-255">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="474d9-256">Veri modelinizi her değiştirdiğinizde, veritabanını silebilir, çekirdek yönteminizi güncelleştirebilir ve yeni bir veritabanıyla aynı şekilde bir baştan başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-256">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="474d9-257">Sonraki öğreticilerde, veri modeli değiştiğinde ve yeniden oluşturmadan veritabanını nasıl değiştireceğiniz hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-257">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="474d9-258">Denetleyici ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="474d9-258">Create controller and views</span></span>

<span data-ttu-id="474d9-259">Daha sonra, verileri sorgulamak ve kaydetmek için EF 'i kullanacak bir MVC denetleyicisi ve görünümleri eklemek üzere Visual Studio 'da scafkatlama altyapısını kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-259">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="474d9-260">CRUD eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="474d9-260">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="474d9-261">Yapı iskelesi, yapı oluşturma işleminden farklı olarak, kendi gereksinimlerinize uyacak şekilde değiştirebileceğiniz bir başlangıç noktası ve genellikle oluşturulan kodu değiştirmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-261">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="474d9-262">Oluşturulan kodu özelleştirmeniz gerektiğinde, kısmi sınıfları kullanırsınız veya işlemler değiştiğinde kodu yeniden oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="474d9-262">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="474d9-263">**Çözüm Gezgini** ' de **denetleyiciler** klasörüne sağ tıklayın ve **> yeni iskele öğe Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-263">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="474d9-264">**Yapı Iskelesi Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="474d9-264">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="474d9-265">**Entity Framework kullanarak, görünümlerle MVC denetleyicisi ' ni**seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-265">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="474d9-266">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="474d9-266">Click **Add**.</span></span> <span data-ttu-id="474d9-267">**Görünümler Ile MVC denetleyicisi ekleme, Entity Framework kullanma** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="474d9-267">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Yapı iskelesi öğrenci](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="474d9-269">**Model sınıfı** ' nda **öğrenci**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="474d9-270">**Veri bağlamı sınıfında** **SchoolContext**öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="474d9-271">Varsayılan **Studentscontroller** adını olarak kabul edin.</span><span class="sxs-lookup"><span data-stu-id="474d9-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="474d9-272">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="474d9-272">Click **Add**.</span></span>

  <span data-ttu-id="474d9-273">**Ekle**' ye tıkladığınızda, Visual Studio yapı iskelesi altyapısı, denetleyicisiyle birlikte çalışan bir *StudentsController.cs* dosyası ve bir dizi görünüm ( *. cshtml* dosyası) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="474d9-273">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="474d9-274">(Yapı iskelesi altyapısı, daha önce Bu öğreticide yaptığınız gibi, daha önce el ile oluşturmazsanız, sizin için veritabanı bağlamını de oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-274">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="474d9-275">**Veri bağlam sınıfının**sağ tarafındaki artı Işaretine tıklayarak **Denetleyici Ekle** kutusunda yeni bir bağlam sınıfı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-275">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="474d9-276">Daha sonra, Visual Studio `DbContext` sınıfınızın yanı sıra denetleyiciyi ve görünümleri de oluşturacaktır.)</span><span class="sxs-lookup"><span data-stu-id="474d9-276">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="474d9-277">Denetleyicinin bir `SchoolContext` Oluşturucu parametresi olarak aldığını fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-277">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="474d9-278">ASP.NET Core bağımlılığı ekleme, denetleyiciye bir örneğini `SchoolContext` geçirme işlemini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="474d9-278">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="474d9-279">Bunu *Startup.cs* dosyasında daha önce yapılandırdınız.</span><span class="sxs-lookup"><span data-stu-id="474d9-279">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="474d9-280">Denetleyici, veritabanındaki tüm `Index` öğrencileri görüntüleyen bir Action yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="474d9-280">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="474d9-281">Yöntemi, veritabanı bağlamı örneğinin `Students` özelliğini okuyarak öğrenciler varlık kümesinden öğrencilerin bir listesini alır:</span><span class="sxs-lookup"><span data-stu-id="474d9-281">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="474d9-282">Öğreticide daha sonra bu kodda zaman uyumsuz programlama öğeleri hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-282">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="474d9-283">*Views/öğrenciler/Index. cshtml* görünümü bu listeyi bir tabloda görüntüler:</span><span class="sxs-lookup"><span data-stu-id="474d9-283">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="474d9-284">Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-284">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="474d9-285">`DbInitializer.Initialize` Metodun eklendiği test verilerini görmek için öğrenciler sekmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="474d9-285">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="474d9-286">Tarayıcı pencerenizin ne kadar dar olduğuna bağlı olarak sayfanın en üstünde `Students` sekme bağlantısını görürsünüz veya bağlantıyı görmek için sağ üst köşedeki gezinti simgesine tıklamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="474d9-286">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Contoso Üniversitesi giriş sayfası dar](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="474d9-289">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="474d9-289">View the database</span></span>

<span data-ttu-id="474d9-290">Uygulamayı başlattığınızda, `DbInitializer.Initialize` yöntemi çağırır `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="474d9-290">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="474d9-291">EF, bir veritabanı olmadığını ve bu nedenle bir tane oluşturduğunu gördük, daha sonra `Initialize` Yöntem kodunun geri kalanı veritabanını verilerle doldurmuş.</span><span class="sxs-lookup"><span data-stu-id="474d9-291">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="474d9-292">Visual Studio 'da veritabanını görüntülemek için **SQL Server Nesne Gezgini** (ssox) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-292">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="474d9-293">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="474d9-293">Close the browser.</span></span>

<span data-ttu-id="474d9-294">SSOX penceresi henüz açık değilse, Visual Studio 'daki **Görünüm** menüsünden bunu seçin.</span><span class="sxs-lookup"><span data-stu-id="474d9-294">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="474d9-295">SSOX 'te **(LocalDB) \MSSQLLocalDB > veritabanları**' na tıklayın ve ardından *appSettings. JSON* dosyanızdaki bağlantı dizesinde bulunan veritabanı adı için girişe tıklayın.</span><span class="sxs-lookup"><span data-stu-id="474d9-295">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="474d9-296">Veritabanınızdaki tabloları görmek için **Tablolar** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="474d9-296">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX içindeki tablolar](intro/_static/ssox-tables.png)

<span data-ttu-id="474d9-298">Oluşturulan sütunları ve tabloya eklenmiş satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="474d9-298">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![SSOX 'te öğrenci tablosu](intro/_static/ssox-student-table.png)

<span data-ttu-id="474d9-300">*. Mdf* ve *. ldf* veritabanı dosyaları, *C:\Users\\\<YourUserName >* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="474d9-300">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="474d9-301">Uygulama başlatma sırasında çalışan `EnsureCreated` Başlatıcı metodunu çağırırken, artık `Student` sınıfta bir değişiklik yapabilir, veritabanını silebilir, uygulamayı yeniden çalıştırabilirsiniz ve veritabanı, değişikliklerinizi eşleştirmek için otomatik olarak yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="474d9-301">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="474d9-302">Örneğin, `EmailAddress` `Student` sınıfa bir özellik eklerseniz, yeniden oluşturulan tabloda yeni `EmailAddress` bir sütun görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="474d9-302">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="474d9-303">Kurallar</span><span class="sxs-lookup"><span data-stu-id="474d9-303">Conventions</span></span>

<span data-ttu-id="474d9-304">Entity Framework, kuralların kullanımı veya Entity Framework varsayımlarıyla ilgili olarak en az bir veritabanı oluşturabilmesini sağlamak için yazmanız gereken kod miktarı.</span><span class="sxs-lookup"><span data-stu-id="474d9-304">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="474d9-305">`DbSet` Özelliklerin adları tablo adı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="474d9-305">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="474d9-306">Bir `DbSet` özellik tarafından başvurulmayan varlıklar için, varlık sınıfı adları tablo adı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="474d9-306">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="474d9-307">Varlık özelliği adları, sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="474d9-307">Entity property names are used for column names.</span></span>

* <span data-ttu-id="474d9-308">ID veya Classnameıd olarak adlandırılan varlık özellikleri, birincil anahtar özellikleri olarak tanınır.</span><span class="sxs-lookup"><span data-stu-id="474d9-308">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="474d9-309">Bir özellik, bir yabancı anahtar özelliği `StudentID` `Student`  *\<olarak yorumlanır.\<* `Student`>birincilanahtarözellikadı>(örneğin,gezintiözelliğiiçinvarlığın birincil anahtarı `ID`).</span><span class="sxs-lookup"><span data-stu-id="474d9-309">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="474d9-310">Yabancı anahtar özelliklerine de yalnızca  *\<birincil anahtar özellik adı >* ( `Enrollment` Örneğin, `EnrollmentID` varlığın birincil anahtarı olduğu `EnrollmentID`için) adlandırılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-310">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="474d9-311">Geleneksel davranış geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-311">Conventional behavior can be overridden.</span></span> <span data-ttu-id="474d9-312">Örneğin, bu öğreticide daha önce gördüğünüz gibi tablo adlarını açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-312">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="474d9-313">Ayrıca, bu serinin [sonraki bir öğreticide](complex-data-model.md) göreceğiniz gibi, sütun adlarını ayarlayabilir ve herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-313">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="474d9-314">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="474d9-314">Asynchronous code</span></span>

<span data-ttu-id="474d9-315">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="474d9-315">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="474d9-316">Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-316">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="474d9-317">Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="474d9-317">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="474d9-318">G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması.</span><span class="sxs-lookup"><span data-stu-id="474d9-318">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="474d9-319">İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="474d9-319">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="474d9-320">Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-320">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="474d9-321">Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir, ancak düşük trafik durumlarında performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.</span><span class="sxs-lookup"><span data-stu-id="474d9-321">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="474d9-322">Aşağıdaki kodda `async` , anahtar sözcüğü, return değeri `Task<T>` , `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="474d9-322">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="474d9-323">Anahtar sözcüğü, derleyiciye Yöntem gövdesinin parçaları için geri çağrılar oluşturmasını ve döndürülen `Task<IActionResult>` nesneyi otomatik olarak oluşturmasını söyler. `async`</span><span class="sxs-lookup"><span data-stu-id="474d9-323">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="474d9-324">Dönüş türü `Task<IActionResult>` , türünde `IActionResult`bir sonuçla devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="474d9-324">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="474d9-325">`await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur.</span><span class="sxs-lookup"><span data-stu-id="474d9-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="474d9-326">İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="474d9-326">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="474d9-327">İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="474d9-327">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="474d9-328">`ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="474d9-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="474d9-329">Entity Framework kullanan zaman uyumsuz kod yazarken dikkat edilmesi gereken bazı şeyler:</span><span class="sxs-lookup"><span data-stu-id="474d9-329">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="474d9-330">Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="474d9-330">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="474d9-331">Bu, örneğin `ToListAsync` `SingleOrDefaultAsync`,, ve `SaveChangesAsync`içerir.</span><span class="sxs-lookup"><span data-stu-id="474d9-331">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="474d9-332">Örneğin, gibi yalnızca bir `IQueryable` `var students = context.Students.Where(s => s.LastName == "Davolio")`değiştiren deyimler içermez.</span><span class="sxs-lookup"><span data-stu-id="474d9-332">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="474d9-333">EF bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-333">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="474d9-334">Herhangi bir zaman uyumsuz EF yöntemini çağırdığınızda, her zaman `await` anahtar sözcüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="474d9-334">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="474d9-335">Zaman uyumsuz kodun performans avantajlarından yararlanmak isterseniz, kullanmakta olduğunuz tüm kitaplık paketlerinin (örneğin, sayfalama için), sorguların veritabanına gönderilmesine neden olan herhangi bir Entity Framework yöntemini çağırıyorsa, zaman uyumsuz olarak da kullanılmasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="474d9-335">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="474d9-336">.NET ' te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [Async Overview](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="474d9-336">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="474d9-337">Kodu alın</span><span class="sxs-lookup"><span data-stu-id="474d9-337">Get the code</span></span>

[<span data-ttu-id="474d9-338">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-338">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="474d9-339">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="474d9-339">Next steps</span></span>

<span data-ttu-id="474d9-340">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="474d9-340">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="474d9-341">ASP.NET Core MVC web uygulaması oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="474d9-341">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="474d9-342">Site stili Ayarla</span><span class="sxs-lookup"><span data-stu-id="474d9-342">Set up the site style</span></span>
> * <span data-ttu-id="474d9-343">EF Core NuGet paketleri hakkında bilgi edinildi</span><span class="sxs-lookup"><span data-stu-id="474d9-343">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="474d9-344">Veri modeli oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="474d9-344">Created the data model</span></span>
> * <span data-ttu-id="474d9-345">Veritabanı bağlamı oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="474d9-345">Created the database context</span></span>
> * <span data-ttu-id="474d9-346">SchoolContext kaydedildi</span><span class="sxs-lookup"><span data-stu-id="474d9-346">Registered the SchoolContext</span></span>
> * <span data-ttu-id="474d9-347">Test verileriyle VERITABANı başlatıldı</span><span class="sxs-lookup"><span data-stu-id="474d9-347">Initialized DB with test data</span></span>
> * <span data-ttu-id="474d9-348">Denetleyici ve görünümler oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="474d9-348">Created controller and views</span></span>
> * <span data-ttu-id="474d9-349">Veritabanı görüntüleniyor</span><span class="sxs-lookup"><span data-stu-id="474d9-349">Viewed the database</span></span>

<span data-ttu-id="474d9-350">Aşağıdaki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="474d9-350">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="474d9-351">Temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğrenmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="474d9-351">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="474d9-352">Temel CRUD işlevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="474d9-352">Implement basic CRUD functionality</span></span>](crud.md)
