---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Film modeli ve tablosuna yeni alan ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 39b48c67b5264a9b3ad97389f6a5c2bf9d94d25f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576567"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="58066-104">Film modeli ve tablosuna yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="58066-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="58066-105">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="58066-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="58066-106">Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="58066-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="58066-107">Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="58066-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="58066-108">Bu bölümde değişiklik veritabanına uygulanır. Bu nedenle, bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First Migrations'ı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="58066-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="58066-109">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="58066-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="58066-110">Entity Framework, bunlar eşit değilse bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="58066-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="58066-111">Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="58066-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="58066-112">Code First Migrations ayarlama Model değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="58066-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="58066-113">Visual Studio 2012 kullanıyorsanız, çift tıklayarak *Movies.mdf* veritabanı aracını açmak için Çözüm Gezgini'nden bir dosya.</span><span class="sxs-lookup"><span data-stu-id="58066-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="58066-114">Web için Visual Studio Express veritabanı Gezgini, Visual Studio 2012 Sunucu Gezgini gösterecektir gösterilir.</span><span class="sxs-lookup"><span data-stu-id="58066-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="58066-115">Visual Studio 2010 kullanıyorsanız, SQL Server nesne Gezgini'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="58066-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="58066-116">Veritabanı Aracı'nda (veritabanı Gezgini, Sunucu Gezgini veya SQL Server Nesne Gezgini) sağ tıklayın `MovieDBContext` seçip **Sil** filmler veritabanını bırakmak için.</span><span class="sxs-lookup"><span data-stu-id="58066-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="58066-117">Çözüm Gezgini'ne geri gidin.</span><span class="sxs-lookup"><span data-stu-id="58066-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="58066-118">Sağ tıklayın *Movies.mdf* seçin ve dosya **Sil** filmler veritabanını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="58066-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="58066-119">Hiçbir hata olmadığından emin olmak için uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="58066-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="58066-120">Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="58066-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Paketi Man Ekle](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="58066-122">İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istemi "Enable-geçişleri - ContextTypeName MvcMovie.Models.MovieDBContext" girin.</span><span class="sxs-lookup"><span data-stu-id="58066-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="58066-123">**Etkinleştir geçişleri** (yukarıda gösterilen) bir komut oluşturur bir *Configuration.cs* yeni dosya *geçişler* klasör.</span><span class="sxs-lookup"><span data-stu-id="58066-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="58066-124">Visual Studio açılır *Configuration.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="58066-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="58066-125">Değiştirin `Seed` yönteminde *Configuration.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="58066-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="58066-126">Altında kırmızı dalgalı çizgi sağ tıklayın `Movie` seçip **çözmek** ardından **kullanarak** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="58066-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="58066-127">Bunun yapılması ekler aşağıdaki using deyimi:</span><span class="sxs-lookup"><span data-stu-id="58066-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="58066-128">Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya varsa ekler satırları güncelleştirir. Bunlar henüz yoktur.</span><span class="sxs-lookup"><span data-stu-id="58066-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="58066-129">**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Aşağıdaki adımları başarısız olur, bu noktada oluşturmayın.)</span><span class="sxs-lookup"><span data-stu-id="58066-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="58066-130">Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf.</span><span class="sxs-lookup"><span data-stu-id="58066-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="58066-131">Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* dosya önceki bir adımda.</span><span class="sxs-lookup"><span data-stu-id="58066-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="58066-132">İçinde **Paket Yöneticisi Konsolu** penceresinde "Ekle geçiş ilk" komutunu girin ilk geçiş oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="58066-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="58066-133">' % S'adı "Başlangıç" isteğe bağlıdır ve oluşturulan geçiş dosyasının adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58066-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="58066-134">Code First geçişleri başka bir sınıf dosyasında oluşturur *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="58066-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="58066-135">Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit.</span><span class="sxs-lookup"><span data-stu-id="58066-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="58066-136">İnceleme *{tarih damgası}\_Initial.cs* dosyası, filmler tablo için bir film veritabanı oluşturmak için yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="58066-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="58066-137">Aşağıda, bu yönergeleri veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şema oluşturun.</span><span class="sxs-lookup"><span data-stu-id="58066-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="58066-138">Ardından **çekirdek** yöntemi, bir veritabanı test verileri ile doldurmak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="58066-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="58066-139">İçinde **Paket Yöneticisi Konsolu**, komut "update-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="58066-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="58066-140">Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, veritabanını ve yürüttüğünüz önce uygulamayı çalıştırdığınız için büyük olasılıkla olduğu `update-database`.</span><span class="sxs-lookup"><span data-stu-id="58066-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="58066-141">Bu durumda, silme *Movies.mdf* yeniden dosya ve yeniden deneyin `update-database` komutu.</span><span class="sxs-lookup"><span data-stu-id="58066-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="58066-142">Hata almaya devam ediyorsanız geçişleri klasörünü ve içeriğini silin daha sonra bu sayfanın üst kısmındaki yönergeleri ile başlatın (delete olan *Movies.mdf* dosya sonra Enable-geçişler için devam edin).</span><span class="sxs-lookup"><span data-stu-id="58066-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="58066-143">Uygulamayı çalıştırmak ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="58066-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="58066-144">Çekirdek veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="58066-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="58066-145">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="58066-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="58066-146">Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="58066-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="58066-147">Açık *Models\Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:</span><span class="sxs-lookup"><span data-stu-id="58066-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="58066-148">Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="58066-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="58066-149">Kullanarak uygulama derleme **derleme** &gt; **derleme film** menü komutunu ya da CTRL-SHIFT-b tuşuna basarak</span><span class="sxs-lookup"><span data-stu-id="58066-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="58066-150">Güncelleştirdiğinize göre `Model` sınıfı da ihtiyacınız güncelleştirilecek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni görüntülemekiçingörüntülemeşablonları`Rating`Tarayıcı Görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="58066-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="58066-151">Açık<em>\Views\Movies\Index.cshtml</em> dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı <strong>fiyat</strong> sütun.</span><span class="sxs-lookup"><span data-stu-id="58066-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="58066-152">Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="58066-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="58066-153">Hangi güncelleştirilmiş aşağıdadır <em>Index.cshtml</em> görünüm şablonu şöyle:</span><span class="sxs-lookup"><span data-stu-id="58066-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="58066-154">Ardından, açık *\Views\Movies\Create.cshtml* dosya ve formun sonlarında aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="58066-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="58066-155">Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="58066-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="58066-156">Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="58066-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="58066-157">Şimdi uygulamayı çalıştırabilir ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="58066-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="58066-158">Ancak, bunu yaptığınızda, aşağıdaki hatalardan birini görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="58066-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="58066-159">Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu.</span><span class="sxs-lookup"><span data-stu-id="58066-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="58066-160">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="58066-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="58066-161">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="58066-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="58066-162">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="58066-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="58066-163">Bu yaklaşım, bir test veritabanında etkin geliştirme işi yaparken, kullanışlı olur; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="58066-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="58066-164">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="58066-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="58066-165">Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="58066-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="58066-166">Entity Framework veritabanı başlatıcılar hakkında daha fazla bilgi için bkz: Tom Dykstra'nın [ASP.NET MVC/Entity Framework öğretici](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="58066-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="58066-167">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="58066-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="58066-168">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="58066-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="58066-169">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="58066-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="58066-170">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="58066-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="58066-171">Bu öğreticide, Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="58066-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="58066-172">Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="58066-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="58066-173">Migrations\Configuration.cs dosyasını açın ve her bir nesnenin film Derecelendirme alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="58066-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="58066-174">Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="58066-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="58066-175">`add-migration` Komutu geçerli bir film veritabanı şeması ile geçerli film modeli inceleyin ve DB yeni modeline geçirme için gereken kodu oluşturmak için geçiş framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="58066-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="58066-176">AddRatingMig isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58066-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="58066-177">Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="58066-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="58066-178">Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58066-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="58066-179">Çözümü derleyin ve ardından "veritabanını güncelleştir" komut girin **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="58066-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="58066-180">Çıktıda aşağıdaki resimde gösterilmektedir **Paket Yöneticisi Konsolu** penceresi (AddRatingMig eklenmesini tarih damgası farklı olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="58066-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="58066-181">Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="58066-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="58066-182">Yeni derecesi alanını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58066-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="58066-183">Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı.</span><span class="sxs-lookup"><span data-stu-id="58066-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="58066-184">Derecelendirme ekleyebilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="58066-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="58066-186">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="58066-186">Click **Create**.</span></span> <span data-ttu-id="58066-187">Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:</span><span class="sxs-lookup"><span data-stu-id="58066-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="58066-189">De eklemeniz gerekir `Rating` alan düzenleme, ayrıntıları ve SearchIndex şablonları göster.</span><span class="sxs-lookup"><span data-stu-id="58066-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="58066-190">"Update-veritabanı" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve herhangi bir değişiklik yapılacak, şema modeli ile eşleştiği için.</span><span class="sxs-lookup"><span data-stu-id="58066-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="58066-191">Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz.</span><span class="sxs-lookup"><span data-stu-id="58066-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="58066-192">Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="58066-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="58066-193">Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="58066-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58066-194">[Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="58066-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
