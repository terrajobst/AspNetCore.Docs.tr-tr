---
title: 'Öğretici: Gelişmiş senaryoları hakkında - EF çekirdekli ASP.NET MVC ile bilgi edinin.'
description: Bu öğretici, Entity Framework Core kullanan ASP.NET Core web uygulamaları geliştirmenin temellerini ötesine geçmesini yararlı konuları tanıtır.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/advanced
ms.openlocfilehash: c6255e2b4fc67c6174bab4458ec82035b1886002
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750629"
---
# <a name="tutorial-learn-about-advanced-scenarios---aspnet-mvc-with-ef-core"></a><span data-ttu-id="edb65-103">Öğretici: Gelişmiş senaryoları hakkında - EF çekirdekli ASP.NET MVC ile bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="edb65-103">Tutorial: Learn about advanced scenarios - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="edb65-104">Önceki öğreticide tablo başına hiyerarşi devralma uygulanır.</span><span class="sxs-lookup"><span data-stu-id="edb65-104">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="edb65-105">Bu öğretici, ne zaman, Entity Framework Core kullanan ASP.NET Core web uygulamaları geliştirmenin temellerini gidin dikkat etmeniz yararlı olan çeşitli konuları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="edb65-105">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

<span data-ttu-id="edb65-106">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="edb65-106">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="edb65-107">Ham SQL sorguları gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="edb65-107">Perform raw SQL queries</span></span>
> * <span data-ttu-id="edb65-108">Bir sorguyu varlıkları arama</span><span class="sxs-lookup"><span data-stu-id="edb65-108">Call a query to return entities</span></span>
> * <span data-ttu-id="edb65-109">Bir sorguyu diğer türleri çağırın</span><span class="sxs-lookup"><span data-stu-id="edb65-109">Call a query to return other types</span></span>
> * <span data-ttu-id="edb65-110">Update sorgusu çağırın</span><span class="sxs-lookup"><span data-stu-id="edb65-110">Call an update query</span></span>
> * <span data-ttu-id="edb65-111">SQL sorguları inceleyin</span><span class="sxs-lookup"><span data-stu-id="edb65-111">Examine SQL queries</span></span>
> * <span data-ttu-id="edb65-112">Bir soyutlama katmanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="edb65-112">Create an abstraction layer</span></span>
> * <span data-ttu-id="edb65-113">Otomatik değiştirme algılama hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="edb65-113">Learn about Automatic change detection</span></span>
> * <span data-ttu-id="edb65-114">EF Core kaynak kodu ve geliştirme planlar hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="edb65-114">Learn about EF Core source code and development plans</span></span>
> * <span data-ttu-id="edb65-115">Dinamik LINQ kodu basitleştirmek için kullanmayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="edb65-115">Learn how to use dynamic LINQ to simplify code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edb65-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="edb65-116">Prerequisites</span></span>

* [<span data-ttu-id="edb65-117">Devralma uygulama</span><span class="sxs-lookup"><span data-stu-id="edb65-117">Implement Inheritance</span></span>](inheritance.md)

## <a name="perform-raw-sql-queries"></a><span data-ttu-id="edb65-118">Ham SQL sorguları gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="edb65-118">Perform raw SQL queries</span></span>

<span data-ttu-id="edb65-119">Entity Framework kullanmanın avantajlarını önler, veri depolamanın çok yakından belirli bir yöntem, kodunuzu bağlamadan biridir.</span><span class="sxs-lookup"><span data-stu-id="edb65-119">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="edb65-120">Bunu SQL sorgulara ve komutlara sizin için Ayrıca bunları kendiniz yazmak zorunda kalmanızı oluşturarak yapar.</span><span class="sxs-lookup"><span data-stu-id="edb65-120">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="edb65-121">Ancak, el ile oluşturduğunuz belirli SQL sorguları çalıştırmak ihtiyacınız olduğunda olağanüstü senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="edb65-121">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="edb65-122">Bu senaryolar için Entity Framework kod ilk API SQL komutları doğrudan veritabanına geçirilecek sağlayan yöntemler içerir.</span><span class="sxs-lookup"><span data-stu-id="edb65-122">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="edb65-123">EF Core 1.0 aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="edb65-123">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="edb65-124">Kullanım `DbSet.FromSql` varlık türleri döndüren sorgular için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="edb65-124">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="edb65-125">Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenen tarafından veritabanı bağlamı sürece, [izleme devre dışı](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="edb65-125">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="edb65-126">Kullanım `Database.ExecuteSqlCommand` sorgu dışı komutları için.</span><span class="sxs-lookup"><span data-stu-id="edb65-126">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="edb65-127">Varlık olmayan türler döndüren bir sorgu çalıştırmak ihtiyacınız varsa EF tarafından sağlanan veritabanı bağlantı ile ADO.NET kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edb65-127">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="edb65-128">Varlık türleri almak için bu yöntem kullansanız bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.</span><span class="sxs-lookup"><span data-stu-id="edb65-128">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="edb65-129">Bir web uygulamasında SQL komutları yürütme her zaman true olduğu gibi sitenizi SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="edb65-129">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="edb65-130">Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutları yorumlanamıyor emin olmak için parametreli sorgular kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="edb65-130">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="edb65-131">Bu öğreticide, kullanıcı girişi bir sorguya tümleştirdiğinizde parametreli sorgular kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="edb65-131">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-to-return-entities"></a><span data-ttu-id="edb65-132">Bir sorguyu varlıkları arama</span><span class="sxs-lookup"><span data-stu-id="edb65-132">Call a query to return entities</span></span>

<span data-ttu-id="edb65-133">`DbSet<TEntity>` Sınıf türünde bir varlık döndüren bir sorgu yürütmek için kullanabileceğiniz bir yöntem sağlar `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="edb65-133">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="edb65-134">Bu, nasıl çalıştığını görmek için kodda değiştireceksiniz `Details` departmanı denetleyicinin yöntemi.</span><span class="sxs-lookup"><span data-stu-id="edb65-134">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="edb65-135">İçinde *DepartmentsController.cs*, `Details` yöntemi, bir bölümle alan kodu değiştirin bir `FromSql` yöntemi çağrısı, vurgulanan aşağıdaki kodda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="edb65-135">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10)]

<span data-ttu-id="edb65-136">Yeni kod düzgün çalıştığını doğrulamak için **Departmanlar** sekmesini ve ardından **ayrıntıları** Departmanlar biri için.</span><span class="sxs-lookup"><span data-stu-id="edb65-136">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Departman ayrıntıları](advanced/_static/department-details.png)

## <a name="call-a-query-to-return-other-types"></a><span data-ttu-id="edb65-138">Bir sorguyu diğer türleri çağırın</span><span class="sxs-lookup"><span data-stu-id="edb65-138">Call a query to return other types</span></span>

<span data-ttu-id="edb65-139">Daha önce bir öğrenci istatistikleri kılavuz Öğrenci sayısı için her bir kayıt tarihi gösterdi hakkında sayfası için oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="edb65-139">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="edb65-140">Veri Öğrenciler varlık kümesinde var (`_context.Students`) ve sonuçları bir liste halinde projeye LINQ kullanılan `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="edb65-140">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="edb65-141">SQL kendisi yerine LINQ kullanarak yazmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="edb65-141">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="edb65-142">Bir SQL sorgusunu çalıştırmak ihtiyacınız olan şeyi için varlık nesnesi dışında bir şey döndürür.</span><span class="sxs-lookup"><span data-stu-id="edb65-142">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="edb65-143">EF Core 1.0, bunu yapmak için bir ADO.NET kod yazma ve veritabanı bağlantısını almak EF yoludur.</span><span class="sxs-lookup"><span data-stu-id="edb65-143">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="edb65-144">İçinde *HomeController.cs*, değiştirin `About` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="edb65-144">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="edb65-145">Kullanarak bir ekleme deyimi:</span><span class="sxs-lookup"><span data-stu-id="edb65-145">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="edb65-146">Uygulamayı çalıştırın ve hakkında sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="edb65-146">Run the app and go to the About page.</span></span> <span data-ttu-id="edb65-147">Bu, daha önceki işlevlerini sürdürmektedir aynı verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="edb65-147">It displays the same data it did before.</span></span>

![Sayfa hakkında](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="edb65-149">Update sorgusu çağırın</span><span class="sxs-lookup"><span data-stu-id="edb65-149">Call an update query</span></span>

<span data-ttu-id="edb65-150">Contoso University yöneticiler her kurs sonunda verilen kredi sayısı değiştirme gibi bu veritabanındaki genel değişiklikler yapmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="edb65-150">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="edb65-151">University dersleri çok sayıda varsa, bunları tüm varlıklar olarak almak ve bunları tek tek değiştirmek için verimsiz olabilir.</span><span class="sxs-lookup"><span data-stu-id="edb65-151">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="edb65-152">Bu bölümde, tüm kursları için kredi sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulayacaksınız ve SQL UPDATE deyimi yürüterek değişiklik yapacaksınız.</span><span class="sxs-lookup"><span data-stu-id="edb65-152">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="edb65-153">Web sayfasını aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="edb65-153">The web page will look like the following illustration:</span></span>

![Güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits.png)

<span data-ttu-id="edb65-155">İçinde *CoursesController.cs*, HttpGet ve HttpPost UpdateCourseCredits yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="edb65-155">In *CoursesController.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="edb65-156">Denetleyici HttpGet isteği işlerken bir şey iade `ViewData["RowsAffected"]`, ve görünüm önceki resimde gösterildiği gibi boş bir metin kutusu ve bir Gönder düğmesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="edb65-156">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="edb65-157">Zaman **güncelleştirme** çarpanı olan metin kutusuna girilen değer düğmesine tıklandığında ve HttpPost yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="edb65-157">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="edb65-158">Kodu sonra dersleri güncelleştirir ve görünümüne etkilenen satır sayısını döndüren SQL yürütür `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="edb65-158">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="edb65-159">Zaman görünümünü alır bir `RowsAffected` değeri, güncelleştirilen satır sayısını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="edb65-159">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="edb65-160">İçinde **Çözüm Gezgini**, sağ *görünümler/kursları* klasörünü ve ardından **Ekle > Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="edb65-160">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="edb65-161">İçinde **Yeni Öğe Ekle** iletişim kutusunda, tıklayın **ASP.NET Core** altında **yüklü** sol bölmesinden **Razor Görünüm**ve Yeni Görünüm adlandırın *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="edb65-161">In the **Add New Item** dialog, click **ASP.NET Core** under **Installed** in the left pane, click **Razor View**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="edb65-162">İçinde *Views/Courses/UpdateCourseCredits.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="edb65-162">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="edb65-163">Çalıştırma `UpdateCourseCredits` yöntemi seçerek **kursları** sekmesini, ardından ekleme "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğuna URL'yi (örneğin: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="edb65-163">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="edb65-164">Metin kutusuna bir sayı girin:</span><span class="sxs-lookup"><span data-stu-id="edb65-164">Enter a number in the text box:</span></span>

![Güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits.png)

<span data-ttu-id="edb65-166">Tıklayın **güncelleştirme**.</span><span class="sxs-lookup"><span data-stu-id="edb65-166">Click **Update**.</span></span> <span data-ttu-id="edb65-167">Etkilenen satır sayısını görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="edb65-167">You see the number of rows affected:</span></span>

![Satırlardan etkilenen güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="edb65-169">Tıklayın **listesine geri** kredi düzeltilmiş sayısı kurslarıyla listesini görmek için.</span><span class="sxs-lookup"><span data-stu-id="edb65-169">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="edb65-170">Üretim kodu her zaman geçerli veri sonucunda güncelleştirmelerinin sağlar unutmayın.</span><span class="sxs-lookup"><span data-stu-id="edb65-170">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="edb65-171">Burada gösterilen Basitleştirilmiş kod 5'ten büyük bir sayı elde etmek yeterli kredi sayısı çarpabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edb65-171">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="edb65-172">( `Credits` Özelliğine sahip bir `[Range(0, 5)]` özniteliği.) Güncelleştirme sorgu işe yarar, ancak geçersiz veri, kredi sayısı 5 veya daha az olduğu varsayılır diğer bölümlerinde sistemin beklenmeyen sonuçlara neden.</span><span class="sxs-lookup"><span data-stu-id="edb65-172">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="edb65-173">Ham SQL sorguları hakkında daha fazla bilgi için bkz. [ham SQL sorguları](/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="edb65-173">For more information about raw SQL queries, see [Raw SQL Queries](/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-queries"></a><span data-ttu-id="edb65-174">SQL sorguları inceleyin</span><span class="sxs-lookup"><span data-stu-id="edb65-174">Examine SQL queries</span></span>

<span data-ttu-id="edb65-175">Bazen veritabanına gönderilen gerçek SQL sorguları görebilmeniz yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="edb65-175">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="edb65-176">ASP.NET Core için yerleşik günlüğü işlevini içeren SQL sorguları ve güncelleştirmeleri günlüklerini yazma izni EF Core tarafından otomatik olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="edb65-176">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="edb65-177">Bu bölümde bazı örnekler bir SQL günlük göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="edb65-177">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="edb65-178">Açık *StudentsController.cs* ve `Details` yöntemi, üzerinde bir kesme noktası Ayarla `if (student == null)` deyimi.</span><span class="sxs-lookup"><span data-stu-id="edb65-178">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="edb65-179">Uygulamayı hata ayıklama modunda çalıştırın ve bir öğrenci için ayrıntıları sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="edb65-179">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="edb65-180">Git **çıkış** çıkış penceresinin hata ayıklama gösteren ve sorgu görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="edb65-180">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="edb65-181">Burada bir şey, sizi şaşırtabilir fark edeceksiniz: SQL 2 satırları seçer (`TOP(2)`) Kişi tablosundan.</span><span class="sxs-lookup"><span data-stu-id="edb65-181">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="edb65-182">`SingleOrDefaultAsync` Yöntemi, sunucu üzerindeki 1 satır için çözümlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="edb65-182">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="edb65-183">Bunu istememizin nedeni:</span><span class="sxs-lookup"><span data-stu-id="edb65-183">Here's why:</span></span>

* <span data-ttu-id="edb65-184">Sorgu birden çok satır döndürür, yöntem null değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="edb65-184">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="edb65-185">Sorgu birden çok satır döndürecekti olup olmadığını belirlemek için en az 2 döndürür denetlenecek EF sahiptir.</span><span class="sxs-lookup"><span data-stu-id="edb65-185">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="edb65-186">Hata ayıklama modunu kullanmak ve günlük çıktısı almak için bir kesme noktasında durdurmak yok Not **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="edb65-186">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="edb65-187">Bu günlük çıktısına görünmesini istediğiniz noktada durdurmak için yalnızca bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="edb65-187">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="edb65-188">Bunu yok ise, günlüğe kaydetmeye devam eder ve geri ilgilendiğiniz bölümleri bulmak için kaydırmak zorunda.</span><span class="sxs-lookup"><span data-stu-id="edb65-188">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="create-an-abstraction-layer"></a><span data-ttu-id="edb65-189">Bir soyutlama katmanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="edb65-189">Create an abstraction layer</span></span>

<span data-ttu-id="edb65-190">Birçok geliştiricinin depo ve iş birimi desenleri, Entity Framework ile çalışan kod çevresinde sarmalayıcı olarak uygulamak için kod yazın.</span><span class="sxs-lookup"><span data-stu-id="edb65-190">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="edb65-191">Bu desenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasında bir Soyutlama Katmanı oluşturmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="edb65-191">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="edb65-192">Bu desenleri uygulama veri deposundaki değişiklikleri uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya test odaklı geliştirme (TDD) kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="edb65-192">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="edb65-193">Ancak, bu desenleri uygulamak için ek kod yazmaya her zaman EF, çeşitli nedenlerle kullanan uygulamalar için en iyi seçenek değildir:</span><span class="sxs-lookup"><span data-stu-id="edb65-193">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="edb65-194">EF bağlam sınıfını kendi veri deposu özel kod kodunuzdan korunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="edb65-194">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="edb65-195">EF bağlam sınıfını kullanarak EF bunu veritabanı için bir iş birimi sınıfı güncelleştirmeleri olarak hareket eder.</span><span class="sxs-lookup"><span data-stu-id="edb65-195">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="edb65-196">EF depo kod yazmaya gerek kalmadan TDD uygulamak için özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="edb65-196">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="edb65-197">Depo ve iş birimi desenleri uygulama hakkında daha fazla bilgi için bkz: [Entity Framework 5 sürümü Bu öğretici serisinin](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="edb65-197">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="edb65-198">Entity Framework Core test etmek için kullanılabilecek bir bellek içi veritabanı sağlayıcısını uygular.</span><span class="sxs-lookup"><span data-stu-id="edb65-198">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="edb65-199">Daha fazla bilgi için [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="edb65-199">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="edb65-200">Otomatik değiştirme algılama</span><span class="sxs-lookup"><span data-stu-id="edb65-200">Automatic change detection</span></span>

<span data-ttu-id="edb65-201">Entity Framework, bir varlığın geçerli değerleri özgün değerlerle karşılaştırarak, varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) belirler.</span><span class="sxs-lookup"><span data-stu-id="edb65-201">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="edb65-202">Varlık sorgulanan ya da bağlı orijinal değerleri depolanır.</span><span class="sxs-lookup"><span data-stu-id="edb65-202">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="edb65-203">Otomatik değiştirme algılama neden yöntemlerden bazıları aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="edb65-203">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="edb65-204">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="edb65-204">DbContext.SaveChanges</span></span>

* <span data-ttu-id="edb65-205">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="edb65-205">DbContext.Entry</span></span>

* <span data-ttu-id="edb65-206">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="edb65-206">ChangeTracker.Entries</span></span>

<span data-ttu-id="edb65-207">Aşağıdaki yöntemlerden birini bir döngüde birçok kez çağırmak ve çok sayıda varlık izliyorsunuz, önemli performans iyileştirmeleri otomatik değişiklik algılama kullanarak geçici olarak kapatarak alabilirsiniz `ChangeTracker.AutoDetectChangesEnabled` özelliği.</span><span class="sxs-lookup"><span data-stu-id="edb65-207">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="edb65-208">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="edb65-208">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="ef-core-source-code-and-development-plans"></a><span data-ttu-id="edb65-209">EF Core kaynak kodu ve Geliştirme planları</span><span class="sxs-lookup"><span data-stu-id="edb65-209">EF Core source code and development plans</span></span>

<span data-ttu-id="edb65-210">Entity Framework Core kaynak altındadır [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="edb65-210">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="edb65-211">EF Core deposu içeren gecelik derlemeler, sorun izleme, özellik özellikleri, toplantı notları, tasarım ve [gelecekteki geliştirme için yol haritası](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="edb65-211">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="edb65-212">Dosya veya hataları bulmak ve katkıda bulunun.</span><span class="sxs-lookup"><span data-stu-id="edb65-212">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="edb65-213">Kaynak kodu açık olsa da, Entity Framework Core tam olarak bir Microsoft ürünü olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="edb65-213">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="edb65-214">Microsoft Entity Framework takım denetim üzerinde katkılarını kabul tutar ve her bir yayının kalitesini sağlamak için tüm kod değişikliklerinin test eder.</span><span class="sxs-lookup"><span data-stu-id="edb65-214">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="edb65-215">Mevcut veritabanından ters mühendislik</span><span class="sxs-lookup"><span data-stu-id="edb65-215">Reverse engineer from existing database</span></span>

<span data-ttu-id="edb65-216">Varolan bir veritabanından varlık sınıfları da dahil olmak üzere bir veri modeli tersine mühendislik için kullanın [iskele dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) komutu.</span><span class="sxs-lookup"><span data-stu-id="edb65-216">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="edb65-217">Bkz: [başlangıç Öğreticisi](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="edb65-217">See the [getting-started tutorial](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>

## <a name="use-dynamic-linq-to-simplify-code"></a><span data-ttu-id="edb65-218">Kodu basitleştirmek için dinamik LINQ kullanma</span><span class="sxs-lookup"><span data-stu-id="edb65-218">Use dynamic LINQ to simplify code</span></span>

<span data-ttu-id="edb65-219">[Bu serinin üçüncü öğreticide](sort-filter-page.md) sabit kodlama sütun adları tarafından LINQ kodunu yazma işlemi gösterilmektedir bir `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="edb65-219">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="edb65-220">Aralarından seçim yapabileceğiniz iki sütunlu, bu düzgün çalışır, ancak çok sütun varsa kod ayrıntılı alabilir.</span><span class="sxs-lookup"><span data-stu-id="edb65-220">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="edb65-221">Bu sorunu çözmek için kullanabileceğiniz `EF.Property` özellik adını bir dize olarak belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="edb65-221">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="edb65-222">Bu yaklaşım denemek için değiştirin `Index` yönteminde `StudentsController` aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="edb65-222">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="acknowledgments"></a><span data-ttu-id="edb65-223">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="edb65-223">Acknowledgments</span></span>

<span data-ttu-id="edb65-224">Tom Dykstra ve Rick Anderson (twitter @RickAndMSFT) bu Öğreticisi yazdı.</span><span class="sxs-lookup"><span data-stu-id="edb65-224">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="edb65-225">Rowan Miller, Diego Vega ve diğer Entity Framework takım üyeleri kod incelemeleriyle Yardımlı ve kod öğreticileri için yazar, çıkan sorunlarında hata ayıklama yardımcı olmuştur.</span><span class="sxs-lookup"><span data-stu-id="edb65-225">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span> <span data-ttu-id="edb65-226">John Parente ve Paul Goldman öğretici ASP.NET Core 2.2 için güncelleştirme üzerinde çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="edb65-226">John Parente and Paul Goldman worked on updating the tutorial for ASP.NET Core 2.2.</span></span>

<a id="common-errors"></a>

## <a name="troubleshoot-common-errors"></a><span data-ttu-id="edb65-227">Sık karşılaşılan sorunları giderme</span><span class="sxs-lookup"><span data-stu-id="edb65-227">Troubleshoot common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="edb65-228">Başka bir işlem tarafından kullanılan ContosoUniversity.dll</span><span class="sxs-lookup"><span data-stu-id="edb65-228">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="edb65-229">Hata iletisi:</span><span class="sxs-lookup"><span data-stu-id="edb65-229">Error message:</span></span>

> <span data-ttu-id="edb65-230">Açılamıyor '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll'--yazmak için ' işlem dosyaya erişemiyor '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' başka bir işlem tarafından kullanıldığı için.</span><span class="sxs-lookup"><span data-stu-id="edb65-230">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="edb65-231">Çözüm:</span><span class="sxs-lookup"><span data-stu-id="edb65-231">Solution:</span></span>

<span data-ttu-id="edb65-232">IIS Express bir sitedeki durdurun.</span><span class="sxs-lookup"><span data-stu-id="edb65-232">Stop the site in IIS Express.</span></span> <span data-ttu-id="edb65-233">Windows sistem tepsisine, IIS Express bulmak ve simgesini sağ tıklatın, Contoso University siteyi seçin ve ardından Git **Durdur Site**.</span><span class="sxs-lookup"><span data-stu-id="edb65-233">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="edb65-234">Hiçbir kodla yöntemleri yukarı ve aşağı iskele kurulmuş geçiş</span><span class="sxs-lookup"><span data-stu-id="edb65-234">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="edb65-235">Olası neden:</span><span class="sxs-lookup"><span data-stu-id="edb65-235">Possible cause:</span></span>

<span data-ttu-id="edb65-236">EF CLI komutları yoksa otomatik olarak kapatmak ve kod dosyaları kaydedin.</span><span class="sxs-lookup"><span data-stu-id="edb65-236">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="edb65-237">Programını çalıştırdığınızda Kaydedilmemiş değişiklikleriniz var ve varsa `migrations add` komutunu EF değişikliklerinizi bulamaz.</span><span class="sxs-lookup"><span data-stu-id="edb65-237">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="edb65-238">Çözüm:</span><span class="sxs-lookup"><span data-stu-id="edb65-238">Solution:</span></span>

<span data-ttu-id="edb65-239">Çalıştırma `migrations remove` komutu, kod değişikliklerinizi kaydedin ve yeniden `migrations add` komutu.</span><span class="sxs-lookup"><span data-stu-id="edb65-239">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="edb65-240">Çalışan veritabanı güncelleştirmesi sırasında hatalar</span><span class="sxs-lookup"><span data-stu-id="edb65-240">Errors while running database update</span></span>

<span data-ttu-id="edb65-241">Diğer hatalar mevcut veriler varsa bir veritabanında şema değişiklik yaparken almak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="edb65-241">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="edb65-242">Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="edb65-242">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="edb65-243">Yeni bir veritabanı geçirmek için veri yok ve veritabanını güncelleştir komut hatasız tamamlanması çok daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="edb65-243">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="edb65-244">Veritabanında yeniden adlandırmak için en basit yaklaşımdır *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="edb65-244">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="edb65-245">Bir sonraki çalıştırmanızda `database update`, yeni bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="edb65-245">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="edb65-246">SSOX veritabanında silmek için veritabanına sağ tıklayın, **Sil**ve ardından **veritabanını Sil** iletişim kutusu seç **var olan bağlantıları kapatın** tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="edb65-246">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="edb65-247">CLI kullanarak bir veritabanını silmek için çalıştırma `database drop` CLI komutunu:</span><span class="sxs-lookup"><span data-stu-id="edb65-247">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="edb65-248">Hata bulmayla SQL Server örneği</span><span class="sxs-lookup"><span data-stu-id="edb65-248">Error locating SQL Server instance</span></span>

<span data-ttu-id="edb65-249">Hata iletisi:</span><span class="sxs-lookup"><span data-stu-id="edb65-249">Error Message:</span></span>

> <span data-ttu-id="edb65-250">Bir SQL Server bağlantısı kurulurken ağla ilgili veya örneğe özel bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="edb65-250">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="edb65-251">Sunucu bulunamadı veya erişilebilir durumda değildi.</span><span class="sxs-lookup"><span data-stu-id="edb65-251">The server was not found or was not accessible.</span></span> <span data-ttu-id="edb65-252">Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="edb65-252">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="edb65-253">(sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örneği belirtilen bulma hatası)</span><span class="sxs-lookup"><span data-stu-id="edb65-253">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="edb65-254">Çözüm:</span><span class="sxs-lookup"><span data-stu-id="edb65-254">Solution:</span></span>

<span data-ttu-id="edb65-255">Bağlantı dizesini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="edb65-255">Check the connection string.</span></span> <span data-ttu-id="edb65-256">Veritabanı dosyasını el ile sildiyseniz, yeni bir veritabanı ile baştan başlamak yapım dizesinde veritabanının adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="edb65-256">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="edb65-257">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="edb65-257">Get the code</span></span>

[<span data-ttu-id="edb65-258">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="edb65-258">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a><span data-ttu-id="edb65-259">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="edb65-259">Additional resources</span></span>

<span data-ttu-id="edb65-260">EF Core hakkında daha fazla bilgi için bkz. [Entity Framework Core belgeleri](/ef/core).</span><span class="sxs-lookup"><span data-stu-id="edb65-260">For more information about EF Core, see the [Entity Framework Core documentation](/ef/core).</span></span> <span data-ttu-id="edb65-261">Bir kitap de kullanılabilir: [Entity Framework Core uygulamada](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="edb65-261">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="edb65-262">Bir web uygulamasının nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="edb65-262">For information on how to deploy a web app, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="edb65-263">ASP.NET Core MVC için kimlik doğrulaması ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz. <xref:index>.</span><span class="sxs-lookup"><span data-stu-id="edb65-263">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see <xref:index>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb65-264">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="edb65-264">Next steps</span></span>

<span data-ttu-id="edb65-265">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="edb65-265">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="edb65-266">Gerçekleştirilen ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="edb65-266">Performed raw SQL queries</span></span>
> * <span data-ttu-id="edb65-267">Bir sorguyu varlıkları çağırılır</span><span class="sxs-lookup"><span data-stu-id="edb65-267">Called a query to return entities</span></span>
> * <span data-ttu-id="edb65-268">Adlı bir sorguyu diğer türleri</span><span class="sxs-lookup"><span data-stu-id="edb65-268">Called a query to return other types</span></span>
> * <span data-ttu-id="edb65-269">Update sorgusu çağırılır</span><span class="sxs-lookup"><span data-stu-id="edb65-269">Called an update query</span></span>
> * <span data-ttu-id="edb65-270">Denetlenen SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="edb65-270">Examined SQL queries</span></span>
> * <span data-ttu-id="edb65-271">Oluşturulan bir Soyutlama Katmanı</span><span class="sxs-lookup"><span data-stu-id="edb65-271">Created an abstraction layer</span></span>
> * <span data-ttu-id="edb65-272">Otomatik değiştirme algılama hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="edb65-272">Learned about Automatic change detection</span></span>
> * <span data-ttu-id="edb65-273">EF Core kaynak kodu ve geliştirme planlar hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="edb65-273">Learned about EF Core source code and development plans</span></span>
> * <span data-ttu-id="edb65-274">Kodu basitleştirmek için dinamik LINQ kullanmayı öğrendiniz</span><span class="sxs-lookup"><span data-stu-id="edb65-274">Learned how to use dynamic LINQ to simplify code</span></span>

<span data-ttu-id="edb65-275">Bu, Bu öğretici serisinde, Entity Framework Core kullanarak bir ASP.NET Core MVC uygulamasındaki tamamlar.</span><span class="sxs-lookup"><span data-stu-id="edb65-275">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET Core MVC application.</span></span> <span data-ttu-id="edb65-276">Yeni bir veritabanı ile birlikte çalışarak bu seri; Varolan bir veritabanından tersine mühendislik bir model için bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="edb65-276">This series worked with a new database; an alternative is to  reverse engineer a model from an existing database.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="edb65-277">Öğretici: EF Core MVC, var olan veritabanı ile</span><span class="sxs-lookup"><span data-stu-id="edb65-277">Tutorial: EF Core with MVC, existing database</span></span>](/ef/core/get-started/aspnetcore/new-db?toc=/aspnet/core/toc.json&bc=/aspnet/core/breadcrumb/toc.json)
