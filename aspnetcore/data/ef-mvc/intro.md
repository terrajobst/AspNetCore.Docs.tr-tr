---
title: 'Öğretici: bir ASP.NET MVC web uygulamasında EF Core kullanmaya başlama'
description: Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: a8909d391ae1a35e9c8155df767ab157701c8a51
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259453"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="488ae-103">Öğretici: bir ASP.NET MVC web uygulamasında EF Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="488ae-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="488ae-104">Bu öğretici 3,0 ASP.NET Core güncelleştirilmedi.</span><span class="sxs-lookup"><span data-stu-id="488ae-104">This tutorial has **not** been updated to ASP.NET Core 3.0.</span></span> <span data-ttu-id="488ae-105">[Razor Pages sürümü](xref:data/ef-rp/intro) güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="488ae-105">The [Razor Pages version](xref:data/ef-rp/intro) has been updated.</span></span> <span data-ttu-id="488ae-106">Bunun ne zaman güncelleştirilemeyebilir hakkında bilgi edinmek için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/13920)bakın.</span><span class="sxs-lookup"><span data-stu-id="488ae-106">For information on when this might be updated, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/13920).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="488ae-107">Contoso Üniversitesi örnek Web uygulaması, Entity Framework (EF) Core 2,2 ve Visual Studio 2017 veya 2019 kullanarak ASP.NET Core 2,2 MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="488ae-107">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="488ae-108">Örnek uygulama, kurgusal bir Contoso Üniversitesi için bir Web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-108">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="488ae-109">Öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="488ae-109">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="488ae-110">Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-110">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="488ae-111">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="488ae-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="488ae-112">ASP.NET Core MVC web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-112">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="488ae-113">Site stilini ayarlayın</span><span class="sxs-lookup"><span data-stu-id="488ae-113">Set up the site style</span></span>
> * <span data-ttu-id="488ae-114">EF Core NuGet paketleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="488ae-114">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="488ae-115">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-115">Create the data model</span></span>
> * <span data-ttu-id="488ae-116">Veritabanı bağlamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-116">Create the database context</span></span>
> * <span data-ttu-id="488ae-117">Bağımlılık ekleme için bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="488ae-117">Register the context for dependency injection</span></span>
> * <span data-ttu-id="488ae-118">Test verileriyle veritabanını başlatma</span><span class="sxs-lookup"><span data-stu-id="488ae-118">Initialize the database with test data</span></span>
> * <span data-ttu-id="488ae-119">Denetleyici ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-119">Create a controller and views</span></span>
> * <span data-ttu-id="488ae-120">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="488ae-120">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="488ae-121">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="488ae-121">Prerequisites</span></span>

* [<span data-ttu-id="488ae-122">.NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="488ae-122">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="488ae-123">Aşağıdaki iş yükleriyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) :</span><span class="sxs-lookup"><span data-stu-id="488ae-123">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="488ae-124">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="488ae-124">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="488ae-125">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="488ae-125">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="488ae-126">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="488ae-126">Troubleshooting</span></span>

<span data-ttu-id="488ae-127">Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-127">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="488ae-128">Yaygın hataların bir listesi ve bunların nasıl çözüleceği için, [serideki son öğreticinin sorun giderme bölümüne](advanced.md#common-errors)bakın.</span><span class="sxs-lookup"><span data-stu-id="488ae-128">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="488ae-129">İhtiyacınız olanları bulamazsanız, [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)için bir soru gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-129">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="488ae-130">Bu, her biri daha önceki öğreticilerde gerçekleştirilen bir dizi 10 öğreticisidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-130">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="488ae-131">Her başarılı öğreticinin tamamlanmasından sonra projenin bir kopyasını kaydetmeyi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="488ae-131">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="488ae-132">Daha sonra sorunlarla karşılaşırsanız, tüm serinin başlangıcına dönmek yerine önceki öğreticiden baştan başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-132">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="488ae-133">Contoso Üniversitesi web uygulaması</span><span class="sxs-lookup"><span data-stu-id="488ae-133">Contoso University web app</span></span>

<span data-ttu-id="488ae-134">Bu öğreticilerde oluşturacağınız uygulama basit bir üniversite web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-134">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="488ae-135">Kullanıcılar öğrenci, kurs ve eğitmen bilgilerini görüntüleyebilir ve güncelleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-135">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="488ae-136">Oluşturacağınız ekranların bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="488ae-136">Here are a few of the screens you'll create.</span></span>

![Öğrenciler Dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="488ae-139">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-139">Create web app</span></span>

* <span data-ttu-id="488ae-140">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="488ae-140">Open Visual Studio.</span></span>

* <span data-ttu-id="488ae-141">**Dosya** menüsünden **Yeni > proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-141">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="488ae-142">Sol bölmeden, **Visual C# > Web > yüklü**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-142">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="488ae-143">**ASP.NET Core Web uygulaması** proje şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-143">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="488ae-144">Ad olarak **Contosouniversity** yazın ve **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-144">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Yeni Proje iletişim kutusu](intro/_static/new-project2.png)

* <span data-ttu-id="488ae-146">**Yeni ASP.NET Core Web uygulaması** iletişim kutusunun görünmesini bekleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-146">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="488ae-147">**.NET Core**, **ASP.NET Core 2,2** ve **Web uygulaması (Model-View-Controller)** şablonu ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-147">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="488ae-148">**Kimlik doğrulamanın** **kimlik doğrulaması yok**olarak ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="488ae-148">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="488ae-149">**Tamam**’ı seçin</span><span class="sxs-lookup"><span data-stu-id="488ae-149">Select **OK**</span></span>

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="488ae-151">Site stilini ayarlayın</span><span class="sxs-lookup"><span data-stu-id="488ae-151">Set up the site style</span></span>

<span data-ttu-id="488ae-152">Birkaç basit değişiklik, site menüsünü, düzeni ve giriş sayfasını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="488ae-152">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="488ae-153">*Views/Shared/_Layout. cshtml* dosyasını açın ve aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="488ae-153">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="488ae-154">"ContosoUniversity" öğesinin her oluşumunu "Contoso Üniversitesi" olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="488ae-154">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="488ae-155">Üç oluşum vardır.</span><span class="sxs-lookup"><span data-stu-id="488ae-155">There are three occurrences.</span></span>

* <span data-ttu-id="488ae-156">**Hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **Gizlilik** menü girişini silin.</span><span class="sxs-lookup"><span data-stu-id="488ae-156">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="488ae-157">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="488ae-157">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="488ae-158">*Görünümler/Home/Index. cshtml*'de, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="488ae-158">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="488ae-159">Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-159">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="488ae-160">Bu öğreticilerde oluşturacağınız sayfaların sekmelerini içeren giriş sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="488ae-160">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso Üniversitesi ana sayfası](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="488ae-162">NuGet paketleri EF Core hakkında</span><span class="sxs-lookup"><span data-stu-id="488ae-162">About EF Core NuGet packages</span></span>

<span data-ttu-id="488ae-163">Bir projeye EF Core destek eklemek için hedeflemek istediğiniz veritabanı sağlayıcısını yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-163">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="488ae-164">Bu öğretici SQL Server kullanır ve sağlayıcı paketi [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)' dır.</span><span class="sxs-lookup"><span data-stu-id="488ae-164">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="488ae-165">Bu paket [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur, bu nedenle pakete başvurmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="488ae-165">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="488ae-166">EF SQL Server paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF için çalışma zamanı desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="488ae-166">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="488ae-167">[Geçiş](migrations.md) öğreticisinde daha sonra bir araç paketi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-167">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="488ae-168">Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="488ae-168">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="488ae-169">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-169">Create the data model</span></span>

<span data-ttu-id="488ae-170">Daha sonra Contoso Üniversitesi uygulaması için varlık sınıfları oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-170">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="488ae-171">Aşağıdaki üç varlıkla başlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-171">You'll start with the following three entities.</span></span>

![Kurs-kayıt-öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

<span data-ttu-id="488ae-173">@No__t-0 ve `Enrollment` varlıkları arasında bire çok ilişki vardır ve `Course` ile `Enrollment` varlıkları arasında bire çok bir ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="488ae-173">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="488ae-174">Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursa kaydedilebilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-174">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="488ae-175">Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-175">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="488ae-176">Öğrenci varlığı</span><span class="sxs-lookup"><span data-stu-id="488ae-176">The Student entity</span></span>

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

<span data-ttu-id="488ae-178">*Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyası oluşturun ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="488ae-178">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="488ae-179">@No__t-0 özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="488ae-179">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="488ae-180">Varsayılan olarak Entity Framework, `ID` veya `classnameID` adlı bir özelliği birincil anahtar olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="488ae-180">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="488ae-181">@No__t-0 özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="488ae-181">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="488ae-182">Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar.</span><span class="sxs-lookup"><span data-stu-id="488ae-182">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="488ae-183">Bu durumda, bir `Student entity` ' in `Enrollments` özelliği, bu `Student` varlığıyla ilgili @no__t 2 varlıkların tümünü tutacaktır.</span><span class="sxs-lookup"><span data-stu-id="488ae-183">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="488ae-184">Diğer bir deyişle, veritabanında verilen bir öğrenci satırı, iki ilişkili kayıt satırına (bu öğrencinin birincil anahtar değerini kendi StudentID yabancı anahtar sütununda içeren satırlar) sahipse, bu `Student` varlığının `Enrollments` gezinti özelliği bu iki `Enrollment` ' yi içerir varlıklar.</span><span class="sxs-lookup"><span data-stu-id="488ae-184">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="488ae-185">Bir gezinti özelliği birden çok varlığı tutabilecek (çoktan çoğa veya bire çok ilişkilerde olduğu gibi), türü `ICollection<T>` gibi girişlerin eklenebileceği, silinebileceği ve güncelleştirilebilen bir liste olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="488ae-185">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="488ae-186">@No__t-0 veya `List<T>` veya `HashSet<T>` gibi bir tür belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-186">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="488ae-187">@No__t-0 belirtirseniz, EF varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="488ae-187">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="488ae-188">Kayıt varlığı</span><span class="sxs-lookup"><span data-stu-id="488ae-188">The Enrollment entity</span></span>

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

<span data-ttu-id="488ae-190">*Modeller* klasöründe *enrollment.cs* oluşturun ve mevcut kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="488ae-190">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="488ae-191">@No__t-0 özelliği birincil anahtar olacaktır; Bu varlık `Student` varlığında gördüğünüz gibi, `ID` yerine `classnameID` modelini kullanır.</span><span class="sxs-lookup"><span data-stu-id="488ae-191">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="488ae-192">Normalde tek bir model seçip veri modeliniz genelinde kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-192">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="488ae-193">Burada, değişim, her iki stili de kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="488ae-193">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="488ae-194">[Daha sonraki bir öğreticide](inheritance.md), ID 'yi ClassName olmadan kullanarak, veri modelinde devralma uygulamayı daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="488ae-194">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="488ae-195">@No__t-0 özelliği bir `enum` ' dir.</span><span class="sxs-lookup"><span data-stu-id="488ae-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="488ae-196">@No__t-0 tür bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="488ae-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="488ae-197">Null olan bir sınıf sıfır bir sınıf ile farklıdır--null, henüz bir sınıf bilinmediğini veya henüz atanmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="488ae-197">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="488ae-198">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Student` ' dir.</span><span class="sxs-lookup"><span data-stu-id="488ae-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="488ae-199">@No__t-0 bir varlık bir `Student` varlıkla ilişkilendirilir, bu nedenle özellik yalnızca tek bir `Student` varlığı tutabilir (daha önce gördüğünüz `Student.Enrollments` gezinti özelliğinden farklı olarak, birden çok `Enrollment` varlığı tutabilir).</span><span class="sxs-lookup"><span data-stu-id="488ae-199">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="488ae-200">@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Course` ' dir.</span><span class="sxs-lookup"><span data-stu-id="488ae-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="488ae-201">@No__t-0 varlığı, bir `Course` varlığıyla ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="488ae-202">Entity Framework, bir özelliği bir yabancı anahtar @no__t özelliği olarak Yorumlar (örneğin, `Student` varlığının birincil anahtarı `ID` olduğundan `Student` gezinti özelliği için `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="488ae-202">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="488ae-203">Yabancı anahtar özellikleri de yalnızca `<primary key property name>` (örneğin @no__t, `Course` varlığının birincil anahtarı `CourseID`) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="488ae-203">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="488ae-204">Kurs varlığı</span><span class="sxs-lookup"><span data-stu-id="488ae-204">The Course entity</span></span>

![Kurs varlık diyagramı](intro/_static/course-entity.png)

<span data-ttu-id="488ae-206">*Modeller* klasöründe *Course.cs* oluşturun ve mevcut kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="488ae-206">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="488ae-207">@No__t-0 özelliği bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-207">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="488ae-208">@No__t-0 varlığı, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-208">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="488ae-209">Bu serinin [sonraki bir öğreticide](complex-data-model.md) `DatabaseGenerated` özniteliği hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-209">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="488ae-210">Temel olarak bu öznitelik, veritabanının oluşturması yerine kursa ait birincil anahtarı girmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="488ae-210">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="488ae-211">Veritabanı bağlamını oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-211">Create the database context</span></span>

<span data-ttu-id="488ae-212">Belirli bir veri modeli için Entity Framework işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="488ae-212">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="488ae-213">Bu sınıfı `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="488ae-213">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="488ae-214">Kodunuzda, veri modeline hangi varlıkların ekleneceğini belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-214">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="488ae-215">Ayrıca, belirli Entity Framework davranışlarını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-215">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="488ae-216">Bu projede, sınıfı `SchoolContext` olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="488ae-216">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="488ae-217">Proje klasöründe, *veri*adlı bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="488ae-217">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="488ae-218">*Veri* klasöründe, *SchoolContext.cs*adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="488ae-218">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="488ae-219">Bu kod, her varlık kümesi için bir `DbSet` özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="488ae-219">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="488ae-220">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="488ae-220">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="488ae-221">@No__t-0 ve `DbSet<Course>` deyimlerini atlarsanız aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="488ae-221">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="488ae-222">Entity Framework, `Student` varlığı `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan, bunları örtülü olarak içerebilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-222">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="488ae-223">Veritabanı oluşturulduğunda EF, `DbSet` özellik adlarıyla aynı adlara sahip tablolar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="488ae-223">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="488ae-224">Koleksiyonlar için özellik adları genellikle plural (öğrenci yerine öğrenciler), ancak geliştiriciler tablo adlarının plurulululmasını kabul etmez.</span><span class="sxs-lookup"><span data-stu-id="488ae-224">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="488ae-225">Bu öğreticiler için DbContext içinde tekil tablo adları belirterek varsayılan davranışı geçersiz kılarsınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-225">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="488ae-226">Bunu yapmak için, son DbSet özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-226">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="488ae-227">SchoolContext kaydetme</span><span class="sxs-lookup"><span data-stu-id="488ae-227">Register the SchoolContext</span></span>

<span data-ttu-id="488ae-228">ASP.NET Core, varsayılan olarak [bağımlılık ekleme](../../fundamentals/dependency-injection.md) işlemini uygular.</span><span class="sxs-lookup"><span data-stu-id="488ae-228">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="488ae-229">Hizmetler (EF veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-229">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="488ae-230">Bu hizmetleri gerektiren bileşenler (MVC denetleyicileri gibi) bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="488ae-230">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="488ae-231">Bu öğreticide daha sonra bir bağlam örneği alan denetleyici Oluşturucu kodunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="488ae-231">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="488ae-232">@No__t-0 ' ı bir hizmet olarak kaydetmek için *Startup.cs*açın ve vurgulanan satırları `ConfigureServices` yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-232">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="488ae-233">@No__t-0 nesnesi üzerinde bir yöntem çağırarak bağlantı dizesinin adı bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-233">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="488ae-234">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="488ae-234">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="488ae-235">@No__t-1 ve `Microsoft.EntityFrameworkCore` ad alanları için `using` deyimleri ekleyin ve ardından projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-235">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="488ae-236">*AppSettings. JSON* dosyasını açın ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-236">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="488ae-237">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="488ae-237">SQL Server Express LocalDB</span></span>

<span data-ttu-id="488ae-238">Bağlantı dizesi bir SQL Server LocalDB veritabanı belirtir.</span><span class="sxs-lookup"><span data-stu-id="488ae-238">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="488ae-239">LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="488ae-239">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="488ae-240">LocalDB, istek üzerine başlar ve kullanıcı modunda çalışır, bu nedenle karmaşık bir yapılandırma yoktur.</span><span class="sxs-lookup"><span data-stu-id="488ae-240">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="488ae-241">Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* veritabanı dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="488ae-241">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="488ae-242">Test verileriyle VERITABANıNı başlatma</span><span class="sxs-lookup"><span data-stu-id="488ae-242">Initialize DB with test data</span></span>

<span data-ttu-id="488ae-243">Entity Framework, sizin için boş bir veritabanı oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="488ae-243">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="488ae-244">Bu bölümde, test verileriyle doldurmak için veritabanı oluşturulduktan sonra çağrılan bir yöntem yazarsınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-244">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="488ae-245">Burada, veritabanını otomatik olarak oluşturmak için `EnsureCreated` yöntemini kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-245">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="488ae-246">Sonraki bir [öğreticide](migrations.md) , veritabanını bırakıp yeniden oluşturmak yerine veritabanı şemasını değiştirmek için Code First Migrations kullanarak model değişikliklerini nasıl işleyeceğinizi göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-246">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="488ae-247">*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu, gerektiğinde bir veritabanının oluşturulmasına neden olan aşağıdaki kodla değiştirin ve test verilerini yeni veritabanına yükler.</span><span class="sxs-lookup"><span data-stu-id="488ae-247">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="488ae-248">Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler ve yoksa, veritabanının yeni olduğunu ve test verileriyle hazırlanması gerektiğini varsayar.</span><span class="sxs-lookup"><span data-stu-id="488ae-248">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="488ae-249">Performansı iyileştirmek için, test verilerini `List<T>` koleksiyonları yerine dizilere yükler.</span><span class="sxs-lookup"><span data-stu-id="488ae-249">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="488ae-250">*Program.cs*' de, uygulama başlangıcında aşağıdakini yapmak için `Main` yöntemini değiştirin:</span><span class="sxs-lookup"><span data-stu-id="488ae-250">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="488ae-251">Bağımlılık ekleme kapsayıcısından bir veritabanı bağlamı örneği alın.</span><span class="sxs-lookup"><span data-stu-id="488ae-251">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="488ae-252">Temel yöntemi çağırın ve bu yönteme geçerek bağlamı geçer.</span><span class="sxs-lookup"><span data-stu-id="488ae-252">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="488ae-253">Çekirdek yöntemi tamamlandığında bağlamı atın.</span><span class="sxs-lookup"><span data-stu-id="488ae-253">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="488ae-254">@No__t-0 deyimleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="488ae-254">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="488ae-255">Daha eski öğreticilerde, *Startup.cs*içinde `Configure` yönteminde benzer bir kod görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-255">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="488ae-256">Yalnızca istek ardışık düzenini ayarlamak için `Configure` yöntemini kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="488ae-256">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="488ae-257">Uygulama başlangıç kodu `Main` yöntemine aittir.</span><span class="sxs-lookup"><span data-stu-id="488ae-257">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="488ae-258">Uygulamayı ilk kez çalıştırdığınızda veritabanı oluşturulur ve test verileriyle birlikte gösterilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-258">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="488ae-259">Veri modelinizi her değiştirdiğinizde, veritabanını silebilir, çekirdek yönteminizi güncelleştirebilir ve yeni bir veritabanıyla aynı şekilde bir baştan başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-259">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="488ae-260">Sonraki öğreticilerde, veri modeli değiştiğinde ve yeniden oluşturmadan veritabanını nasıl değiştireceğiniz hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-260">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="488ae-261">Denetleyici ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="488ae-261">Create controller and views</span></span>

<span data-ttu-id="488ae-262">Daha sonra, verileri sorgulamak ve kaydetmek için EF 'i kullanacak bir MVC denetleyicisi ve görünümleri eklemek üzere Visual Studio 'da scafkatlama altyapısını kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-262">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="488ae-263">CRUD eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, yapı iskelesi olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="488ae-263">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="488ae-264">Yapı iskelesi, yapı oluşturma işleminden farklı olarak, kendi gereksinimlerinize uyacak şekilde değiştirebileceğiniz bir başlangıç noktası ve genellikle oluşturulan kodu değiştirmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-264">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="488ae-265">Oluşturulan kodu özelleştirmeniz gerektiğinde, kısmi sınıfları kullanırsınız veya işlemler değiştiğinde kodu yeniden oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="488ae-265">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="488ae-266">**Çözüm Gezgini** ' de **denetleyiciler** klasörüne sağ tıklayın ve **> yeni iskele öğe Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-266">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="488ae-267">**Yapı Iskelesi Ekle** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="488ae-267">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="488ae-268">**Entity Framework kullanarak, görünümlerle MVC denetleyicisi ' ni**seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-268">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="488ae-269">**Ekle**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-269">Click **Add**.</span></span> <span data-ttu-id="488ae-270">**Görünümler Ile MVC denetleyicisi ekleme, Entity Framework kullanma** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="488ae-270">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Yapı iskelesi öğrenci](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="488ae-272">**Model sınıfı** ' nda **öğrenci**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-272">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="488ae-273">**Veri bağlamı sınıfında** **SchoolContext**öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-273">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="488ae-274">Varsayılan **Studentscontroller** adını olarak kabul edin.</span><span class="sxs-lookup"><span data-stu-id="488ae-274">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="488ae-275">**Ekle**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-275">Click **Add**.</span></span>

  <span data-ttu-id="488ae-276">**Ekle**' ye tıkladığınızda, Visual Studio yapı iskelesi altyapısı, denetleyicisiyle birlikte çalışan bir *StudentsController.cs* dosyası ve bir dizi görünüm ( *. cshtml* dosyası) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="488ae-276">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="488ae-277">(Yapı iskelesi altyapısı, daha önce Bu öğreticide yaptığınız gibi, daha önce el ile oluşturmazsanız, sizin için veritabanı bağlamını de oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-277">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="488ae-278">**Veri bağlam sınıfının**sağ tarafındaki artı Işaretine tıklayarak **Denetleyici Ekle** kutusunda yeni bir bağlam sınıfı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-278">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="488ae-279">Daha sonra Visual Studio `DbContext` sınıfınızın yanı sıra denetleyiciyi ve görünümleri de oluşturacaktır.)</span><span class="sxs-lookup"><span data-stu-id="488ae-279">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="488ae-280">Denetleyicinin bir oluşturucu parametresi olarak `SchoolContext` aldığını fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-280">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="488ae-281">ASP.NET Core bağımlılığı ekleme, denetleyiciye @no__t bir örneğini geçirmekten yararlanır.</span><span class="sxs-lookup"><span data-stu-id="488ae-281">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="488ae-282">Bunu *Startup.cs* dosyasında daha önce yapılandırdınız.</span><span class="sxs-lookup"><span data-stu-id="488ae-282">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="488ae-283">Denetleyici, veritabanındaki tüm öğrencileri görüntüleyen bir `Index` eylem yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="488ae-283">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="488ae-284">Yöntemi, veritabanı bağlamı örneğinin `Students` özelliğini okuyarak öğrenciler varlık kümesinden öğrencilerin bir listesini alır:</span><span class="sxs-lookup"><span data-stu-id="488ae-284">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="488ae-285">Öğreticide daha sonra bu kodda zaman uyumsuz programlama öğeleri hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-285">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="488ae-286">*Views/öğrenciler/Index. cshtml* görünümü bu listeyi bir tabloda görüntüler:</span><span class="sxs-lookup"><span data-stu-id="488ae-286">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="488ae-287">Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-287">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="488ae-288">@No__t-0 yönteminin eklendiği test verilerini görmek için öğrenciler sekmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-288">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="488ae-289">Tarayıcı pencerenizin ne kadar dar olduğuna bağlı olarak, sayfanın üst kısmında `Students` sekme bağlantısını görürsünüz veya bağlantıyı görmek için sağ üst köşedeki gezinti simgesine tıklamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="488ae-289">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Contoso Üniversitesi giriş sayfası dar](intro/_static/home-page-narrow.png)

![Öğrenciler Dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="488ae-292">Veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="488ae-292">View the database</span></span>

<span data-ttu-id="488ae-293">Uygulamayı başlattığınızda `DbInitializer.Initialize` yöntemi `EnsureCreated` ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="488ae-293">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="488ae-294">EF, bir veritabanı olmadığını ve bu nedenle bir tane oluşturduğunu gördük, `Initialize` Yöntem kodunun geri kalanı veritabanını verilerle doldurmuş.</span><span class="sxs-lookup"><span data-stu-id="488ae-294">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="488ae-295">Visual Studio 'da veritabanını görüntülemek için **SQL Server Nesne Gezgini** (ssox) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-295">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="488ae-296">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="488ae-296">Close the browser.</span></span>

<span data-ttu-id="488ae-297">SSOX penceresi henüz açık değilse, Visual Studio 'daki **Görünüm** menüsünden bunu seçin.</span><span class="sxs-lookup"><span data-stu-id="488ae-297">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="488ae-298">SSOX 'te **(LocalDB) \MSSQLLocalDB > veritabanları**' na tıklayın ve ardından *appSettings. JSON* dosyanızdaki bağlantı dizesinde bulunan veritabanı adı için girişe tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-298">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="488ae-299">Veritabanınızdaki tabloları görmek için **Tablolar** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="488ae-299">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX içindeki tablolar](intro/_static/ssox-tables.png)

<span data-ttu-id="488ae-301">Oluşturulan sütunları ve tabloya eklenmiş satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-301">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![SSOX 'te öğrenci tablosu](intro/_static/ssox-student-table.png)

<span data-ttu-id="488ae-303">*. Mdf* ve *. ldf* veritabanı dosyaları, *C:\Users @ no__t-3 @ no__t-4yourusername >* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="488ae-303">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="488ae-304">Uygulama başlatma sırasında çalışan Başlatıcı yönteminde `EnsureCreated` ' ı çağırırken, artık `Student` sınıfında bir değişiklik yapabilir, veritabanını silebilir, uygulamayı yeniden çalıştırabilirsiniz ve veritabanı, değişikliklerinizi eşleştirmek için otomatik olarak yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="488ae-304">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="488ae-305">Örneğin, `Student` sınıfına bir `EmailAddress` özelliği eklerseniz, yeniden oluşturulan tabloda yeni bir `EmailAddress` sütunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="488ae-305">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="488ae-306">Kurallar</span><span class="sxs-lookup"><span data-stu-id="488ae-306">Conventions</span></span>

<span data-ttu-id="488ae-307">Entity Framework, kuralların kullanımı veya Entity Framework varsayımlarıyla ilgili olarak en az bir veritabanı oluşturabilmesini sağlamak için yazmanız gereken kod miktarı.</span><span class="sxs-lookup"><span data-stu-id="488ae-307">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="488ae-308">@No__t-0 özelliklerinin adları tablo adı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="488ae-308">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="488ae-309">@No__t-0 özelliği tarafından başvurulmayan varlıklar için, varlık sınıfı adları tablo adı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="488ae-309">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="488ae-310">Varlık özelliği adları, sütun adları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="488ae-310">Entity property names are used for column names.</span></span>

* <span data-ttu-id="488ae-311">ID veya Classnameıd olarak adlandırılan varlık özellikleri, birincil anahtar özellikleri olarak tanınır.</span><span class="sxs-lookup"><span data-stu-id="488ae-311">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="488ae-312">Bir özellik, *\<gezinti özelliği adı > \<birincil anahtar özellik adı >* (örneğin, `Student` ' in birincil anahtarı olduğundan `Student` gezinti özelliği için `StudentID`) olarak adlandırılmışsa, yabancı anahtar özelliği olarak yorumlanır. t-6).</span><span class="sxs-lookup"><span data-stu-id="488ae-312">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="488ae-313">Yabancı anahtar özelliklerine de yalnızca *\<birincil anahtar özellik adı >* adlandırılabilir (örneğin, `Enrollment` varlığının birincil anahtarı `EnrollmentID` olduğundan, `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="488ae-313">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="488ae-314">Geleneksel davranış geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-314">Conventional behavior can be overridden.</span></span> <span data-ttu-id="488ae-315">Örneğin, bu öğreticide daha önce gördüğünüz gibi tablo adlarını açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-315">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="488ae-316">Ayrıca, bu serinin [sonraki bir öğreticide](complex-data-model.md) göreceğiniz gibi, sütun adlarını ayarlayabilir ve herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-316">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="488ae-317">Zaman uyumsuz kod</span><span class="sxs-lookup"><span data-stu-id="488ae-317">Asynchronous code</span></span>

<span data-ttu-id="488ae-318">Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.</span><span class="sxs-lookup"><span data-stu-id="488ae-318">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="488ae-319">Web sunucusunda sınırlı sayıda iş parçacığı bulunur ve yüksek yük durumlarında tüm kullanılabilir iş parçacıkları kullanımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-319">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="488ae-320">Bu durumda, sunucu, iş parçacıkları boşaltılana kadar yeni istekleri işleyemez.</span><span class="sxs-lookup"><span data-stu-id="488ae-320">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="488ae-321">Zaman uyumlu kodla, çok sayıda iş parçacığı, g/ç 'nin tamamlanmasını beklediği için aslında herhangi bir iş yapmadıklarında bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-321">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="488ae-322">Zaman uyumsuz kod ile, bir işlem g/ç 'yi tamamlanmayı beklerken, sunucunun diğer istekleri işlemek için kullanması için iş parçacığı serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="488ae-322">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="488ae-323">Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu, gecikme olmadan daha fazla trafiği işlemeye etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="488ae-323">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="488ae-324">Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir, ancak düşük trafik durumlarında performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.</span><span class="sxs-lookup"><span data-stu-id="488ae-324">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="488ae-325">Aşağıdaki kodda `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="488ae-325">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="488ae-326">@No__t-0 anahtar sözcüğü, derleyiciye Yöntem gövdesinin parçaları için geri çağrılar oluşturmasını ve döndürülen `Task<IActionResult>` nesnesini otomatik olarak oluşturmasını söyler.</span><span class="sxs-lookup"><span data-stu-id="488ae-326">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="488ae-327">@No__t-0 dönüş türü, `IActionResult` türünde bir sonuçla devam eden işi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="488ae-327">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="488ae-328">@No__t-0 anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="488ae-328">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="488ae-329">İlk bölüm, zaman uyumsuz olarak başlatılan işlemle biter.</span><span class="sxs-lookup"><span data-stu-id="488ae-329">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="488ae-330">İkinci bölüm, işlem tamamlandığında çağrılan bir geri çağırma yöntemine konur.</span><span class="sxs-lookup"><span data-stu-id="488ae-330">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="488ae-331">`ToListAsync`, `ToList` Genişletme yönteminin zaman uyumsuz sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="488ae-331">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="488ae-332">Entity Framework kullanan zaman uyumsuz kod yazarken dikkat edilmesi gereken bazı şeyler:</span><span class="sxs-lookup"><span data-stu-id="488ae-332">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="488ae-333">Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="488ae-333">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="488ae-334">Bu, örneğin, `ToListAsync`, `SingleOrDefaultAsync` ve `SaveChangesAsync` içerir.</span><span class="sxs-lookup"><span data-stu-id="488ae-334">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="488ae-335">Örneğin, `var students = context.Students.Where(s => s.LastName == "Davolio")` gibi yalnızca `IQueryable` değiştiren deyimler dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="488ae-335">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="488ae-336">EF bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-336">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="488ae-337">Herhangi bir zaman uyumsuz EF yöntemini çağırdığınızda her zaman `await` anahtar sözcüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="488ae-337">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="488ae-338">Zaman uyumsuz kodun performans avantajlarından yararlanmak isterseniz, kullanmakta olduğunuz tüm kitaplık paketlerinin (örneğin, sayfalama için), sorguların veritabanına gönderilmesine neden olan herhangi bir Entity Framework yöntemini çağırıyorsa, zaman uyumsuz olarak da kullanılmasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="488ae-338">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="488ae-339">.NET ' te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [Async Overview](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="488ae-339">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="488ae-340">Kodu edinin</span><span class="sxs-lookup"><span data-stu-id="488ae-340">Get the code</span></span>

[<span data-ttu-id="488ae-341">Tamamlanmış uygulamayı indirin veya görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-341">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="488ae-342">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="488ae-342">Next steps</span></span>

<span data-ttu-id="488ae-343">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="488ae-343">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="488ae-344">ASP.NET Core MVC web uygulaması oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="488ae-344">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="488ae-345">Site stilini ayarlayın</span><span class="sxs-lookup"><span data-stu-id="488ae-345">Set up the site style</span></span>
> * <span data-ttu-id="488ae-346">EF Core NuGet paketleri hakkında bilgi edinildi</span><span class="sxs-lookup"><span data-stu-id="488ae-346">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="488ae-347">Veri modeli oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="488ae-347">Created the data model</span></span>
> * <span data-ttu-id="488ae-348">Veritabanı bağlamı oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="488ae-348">Created the database context</span></span>
> * <span data-ttu-id="488ae-349">SchoolContext kaydedildi</span><span class="sxs-lookup"><span data-stu-id="488ae-349">Registered the SchoolContext</span></span>
> * <span data-ttu-id="488ae-350">Test verileriyle VERITABANı başlatıldı</span><span class="sxs-lookup"><span data-stu-id="488ae-350">Initialized DB with test data</span></span>
> * <span data-ttu-id="488ae-351">Denetleyici ve görünümler oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="488ae-351">Created controller and views</span></span>
> * <span data-ttu-id="488ae-352">Veritabanı görüntüleniyor</span><span class="sxs-lookup"><span data-stu-id="488ae-352">Viewed the database</span></span>

<span data-ttu-id="488ae-353">Aşağıdaki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="488ae-353">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="488ae-354">Temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğrenmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="488ae-354">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="488ae-355">Temel CRUD işlevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="488ae-355">Implement basic CRUD functionality</span></span>](crud.md)

::: moniker-end
