---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Yeni bir alan film modeli ve tablo ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="e5de5-104">Yeni bir alan film modeli ve tablo ekleme</span><span class="sxs-lookup"><span data-stu-id="e5de5-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="e5de5-105">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e5de5-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="e5de5-106">Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="e5de5-107">Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="e5de5-108">Bu bölümde değişiklik veritabanına uyguladınız bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5de5-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="e5de5-109">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="e5de5-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="e5de5-110">Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5de5-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="e5de5-111">Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="e5de5-112">Model değişiklikleri için Code First Migrations kurulması</span><span class="sxs-lookup"><span data-stu-id="e5de5-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="e5de5-113">Visual Studio 2012 kullanıyorsanız, çift tıklayarak *Movies.mdf* veritabanı Aracı'nı açmak için Çözüm Gezgini'nden dosya.</span><span class="sxs-lookup"><span data-stu-id="e5de5-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="e5de5-114">Web için Visual Studio Express veritabanı Gezgini, Visual Studio 2012 Sunucu Gezgini gösterecektir gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="e5de5-115">Visual Studio 2010 kullanıyorsanız, SQL Server nesne Gezgini'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5de5-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="e5de5-116">Veritabanı Aracı'nda (veritabanı Gezgini, Sunucu Gezgini veya SQL Server Nesne Gezgini) sağ tıklayın `MovieDBContext` seçip **silmek** filmler veritabanı bırakılamadı.</span><span class="sxs-lookup"><span data-stu-id="e5de5-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="e5de5-117">Çözüm Gezgini'ne gidin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="e5de5-118">Sağ tıklayın *Movies.mdf* dosya ve seçin **silmek** filmler veritabanını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="e5de5-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="e5de5-119">Hiçbir hata bulunmadığından emin olmak için uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5de5-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="e5de5-120">Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="e5de5-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Paketi adam ekleme](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="e5de5-122">İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istemi "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext" girin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="e5de5-123">**Enable-Migrations** (yukarıda gösterilen) komut oluşturur bir *Configuration.cs* yeni bir dosyada *geçişler* klasör.</span><span class="sxs-lookup"><span data-stu-id="e5de5-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="e5de5-124">Visual Studio açılır *Configuration.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="e5de5-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="e5de5-125">Değiştir `Seed` yönteminde *Configuration.cs* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="e5de5-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="e5de5-126">Altında kırmızı kırık çizgi sağ tıklayın `Movie` seçip **gidermek** sonra **kullanarak** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="e5de5-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="e5de5-127">Bunun yapılması ekler aşağıdaki using deyimi:</span><span class="sxs-lookup"><span data-stu-id="e5de5-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="e5de5-128">Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **update-database** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya if ekler satır güncelleştirir. Bunlar henüz mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="e5de5-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="e5de5-129">**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Aşağıdaki adımları başarısız olur, bu noktada yapı yoktur.)</span><span class="sxs-lookup"><span data-stu-id="e5de5-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="e5de5-130">Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf.</span><span class="sxs-lookup"><span data-stu-id="e5de5-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="e5de5-131">Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* bir önceki adımda dosya.</span><span class="sxs-lookup"><span data-stu-id="e5de5-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="e5de5-132">İçinde **Paket Yöneticisi Konsolu** penceresinde "add-migration ilk" komutu girin ilk geçiş oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e5de5-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="e5de5-133">"Başlangıç" adı isteğe bağlıdır ve oluşturulan geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="e5de5-134">Code First geçişleri oluşturur başka bir sınıf dosyasında *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="e5de5-135">Geçiş filename damgasıyla sıralama ile yardımcı olmak için önceden düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="e5de5-136">İncelemek *{tarih damgası}\_Initial.cs* dosyası için film DB filmler tablo oluşturmak için yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="e5de5-137">Aşağıda, bu yönergeler veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şeması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5de5-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="e5de5-138">Ardından **çekirdek** yöntemi DB test verileri ile doldurmak için çalışacak.</span><span class="sxs-lookup"><span data-stu-id="e5de5-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="e5de5-139">İçinde **Paket Yöneticisi Konsolu**, komut "Güncelleştirme-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5de5-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="e5de5-140">Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, uygulama veritabanı silinmiş sonra ve, yürütülmeden önce çalışan, büyük olasılıkla çünkü `update-database`.</span><span class="sxs-lookup"><span data-stu-id="e5de5-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="e5de5-141">Bu durumda, silme *Movies.mdf* dosyasını yeniden ve yeniden deneme `update-database` komutu.</span><span class="sxs-lookup"><span data-stu-id="e5de5-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="e5de5-142">Hala bir hata alırsanız, geçişler klasörünü ve içeriğini silin sonra bu sayfanın üst kısmındaki yönergeleri ile başlayın (delete olan *Movies.mdf* dosya sonra Enable-Migrations devam).</span><span class="sxs-lookup"><span data-stu-id="e5de5-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="e5de5-143">Uygulamayı çalıştırın ve gidin */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="e5de5-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="e5de5-144">Çekirdek verileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="e5de5-145">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="e5de5-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="e5de5-146">Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e5de5-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="e5de5-147">Açık *Models\Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="e5de5-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="e5de5-148">Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:</span><span class="sxs-lookup"><span data-stu-id="e5de5-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="e5de5-149">Uygulamayı kullanarak yapı **yapı** &gt; **yapı film** menü komutunu veya CTRL-SHIFT-b tuşuna basarak</span><span class="sxs-lookup"><span data-stu-id="e5de5-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="e5de5-150">Güncelleştirdiğinizden `Model` sınıfı, ayrıca gerekir güncelleştirmek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni görüntülemekiçingörüntülemeşablonları`Rating`tarayıcı görünümünde özelliği.</span><span class="sxs-lookup"><span data-stu-id="e5de5-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="e5de5-151">Açık<em>\Views\Movies\Index.cshtml</em> dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı <strong>fiyat</strong> sütun.</span><span class="sxs-lookup"><span data-stu-id="e5de5-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="e5de5-152">Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="e5de5-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="e5de5-153">Hangi güncelleştirilmiş aşağıdadır <em>Index.cshtml</em> şablonu görüntüleme gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="e5de5-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="e5de5-154">Ardından, açık *\Views\Movies\Create.cshtml* dosyasını ve form sona aşağıdaki biçimlendirmeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="e5de5-155">Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5de5-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="e5de5-156">Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="e5de5-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="e5de5-157">Uygulamayı çalıştırın ve gidin artık */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="e5de5-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="e5de5-158">Ancak, bunu yaptığınızda, aşağıdaki hatalardan biri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="e5de5-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="e5de5-159">Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo.</span><span class="sxs-lookup"><span data-stu-id="e5de5-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="e5de5-160">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="e5de5-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="e5de5-161">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="e5de5-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="e5de5-162">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="e5de5-163">Bu yaklaşım, bir test veritabanı üzerindeki etkin geliştirme yaparken çok kullanışlı olur; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5de5-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="e5de5-164">Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="e5de5-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="e5de5-165">Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür.</span><span class="sxs-lookup"><span data-stu-id="e5de5-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="e5de5-166">Entity Framework veritabanı başlatıcıları hakkında daha fazla bilgi için bkz: zel Dykstra'nın [ASP.NET MVC/Entity Framework Öğreticisi](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="e5de5-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="e5de5-167">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="e5de5-168">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="e5de5-169">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="e5de5-170">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5de5-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="e5de5-171">Bu öğretici için Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="e5de5-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="e5de5-172">Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5de5-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="e5de5-173">Migrations\Configuration.cs dosyasını açın ve her film nesnesine bir derecelendirme alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="e5de5-174">Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="e5de5-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="e5de5-175">`add-migration` Komutu geçerli film DB şeması geçerli film modeliyle inceleyin ve yeni modele DB geçirmek için gerekli kodu oluşturmak için geçiş framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="e5de5-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="e5de5-176">AddRatingMig rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="e5de5-177">Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5de5-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="e5de5-178">Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5de5-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="e5de5-179">Çözümü derlemek ve "update-database" komutta enter **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="e5de5-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="e5de5-180">Aşağıdaki resimde çıktıda gösterir **Paket Yöneticisi Konsolu** penceresi (AddRatingMig eklenmesini tarih damgası farklı olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="e5de5-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="e5de5-181">Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="e5de5-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="e5de5-182">Yeni Derecelendirme alanı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5de5-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="e5de5-183">Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="e5de5-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="e5de5-184">Bir derecelendirme ekleyebileceğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e5de5-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="e5de5-186">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e5de5-186">Click **Create**.</span></span> <span data-ttu-id="e5de5-187">Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="e5de5-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="e5de5-189">Ayrıca eklemelisiniz `Rating` görünüm şablonları düzenleme, Ayrıntılar ve SearchIndex alan.</span><span class="sxs-lookup"><span data-stu-id="e5de5-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="e5de5-190">"Update-database" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir değişiklik yapılan, şema modeli eşleştiğinden.</span><span class="sxs-lookup"><span data-stu-id="e5de5-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="e5de5-191">Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz.</span><span class="sxs-lookup"><span data-stu-id="e5de5-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="e5de5-192">Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="e5de5-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="e5de5-193">Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="e5de5-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5de5-194">[Önceki](examining-the-edit-methods-and-edit-view.md)
> [sonraki](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="e5de5-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
