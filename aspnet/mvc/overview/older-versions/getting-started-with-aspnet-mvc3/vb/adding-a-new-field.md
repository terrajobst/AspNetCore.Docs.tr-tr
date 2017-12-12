---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: "Film modeli ve veritabanı tablosu (VB) için yeni bir alan ekleyerek | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 377c667a56bb5c0d58ecef5c3550ca510ec52546
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="1bcb5-103">Film modeli ve veritabanı tablosu (VB) yeni bir alan ekleme</span><span class="sxs-lookup"><span data-stu-id="1bcb5-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="1bcb5-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1bcb5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="1bcb5-105">Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="1bcb5-106">Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="1bcb5-107">Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1bcb5-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="1bcb5-108">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="1bcb5-109">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="1bcb5-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="1bcb5-110">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="1bcb5-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="1bcb5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)</span><span class="sxs-lookup"><span data-stu-id="1bcb5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="1bcb5-112">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1bcb5-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="1bcb5-113">VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="1bcb5-114">[VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="1bcb5-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="1bcb5-115">C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-new-field.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="1bcb5-116">Bu bölümde model sınıflarına bazı değişiklikler yapın ve veritabanı şeması model değişiklikleri eşleşecek şekilde nasıl güncelleştirebileceğinizi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="1bcb5-117">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="1bcb5-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="1bcb5-118">Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="1bcb5-119">Açık *Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="1bcb5-120">Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="1bcb5-121">Kullanarak uygulamayı derleyin **hata ayıklama** &gt; **yapı film** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="1bcb5-122">Güncelleştirdiğinizden `Model` sınıfı, ayrıca gerekir güncelleştirmek *\Views\Movies\Index.vbhtml* ve *\Views\Movies\Create.vbhtml* yeni desteklemekiçingörüntülemeşablonları`Rating`özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="1bcb5-123">Açık*\Views\Movies\Index.vbhtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı **fiyat** sütun.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-123">Open the*\Views\Movies\Index.vbhtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="1bcb5-124">Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="1bcb5-125">Hangi güncelleştirilmiş aşağıdadır *Index.vbhtml* şablonu görüntüleme gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-125">Below is what the updated *Index.vbhtml* view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="1bcb5-126">Ardından, açık *\Views\Movies\Create.vbhtml* dosyasını ve form sona aşağıdaki biçimlendirmeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="1bcb5-127">Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="1bcb5-128">Model ve veritabanı şeması farklar yönetme</span><span class="sxs-lookup"><span data-stu-id="1bcb5-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="1bcb5-129">Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="1bcb5-130">Uygulamayı çalıştırın ve gidin artık */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="1bcb5-131">Ancak, bunu yaptığınızda, aşağıdaki hatayı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="1bcb5-132">Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="1bcb5-133">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="1bcb5-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="1bcb5-134">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="1bcb5-135">Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="1bcb5-136">Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="1bcb5-137">Eşitleme denetimi özelliği görüntülenecek hata iletisi yalnızca gördüğünüz neyin neden olur.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="1bcb5-138">Hatayı çözümlemek için iki yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="1bcb5-139">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="1bcb5-140">Hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi izin verdiği için bu yaklaşım bir test veritabanı üzerindeki etkin geliştirme yaparken çok kullanışlı bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="1bcb5-141">Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="1bcb5-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="1bcb5-142">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="1bcb5-143">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="1bcb5-144">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="1bcb5-145">Bu öğretici için ilk yaklaşım kullanacağız — Entity Framework kod model değişiklikleri zaman otomatik olarak veritabanını yeniden oluşturmak ilk sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="1bcb5-146">Otomatik olarak Model değişiklikleri veritabanını yeniden oluşturma</span><span class="sxs-lookup"><span data-stu-id="1bcb5-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="1bcb5-147">Code First otomatik olarak bırakır ve uygulama için model değiştirmek zaman veritabanını yeniden oluşturur şekilde şimdi uygulama güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1bcb5-148">**Uyarı** otomatik olarak bırakarak ve yalnızca geliştirme veya test veritabanını kullanırken veritabanı yeniden oluşturarak bu yaklaşım etkinleştirmeniz gerekir ve *hiçbir zaman* gerçek verileri içeren bir üretim veritabanında.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="1bcb5-149">Bir üretim sunucusunda kullanarak veri kaybına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="1bcb5-150">İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="1bcb5-151">Sınıf adını &quot;MovieInitializer&quot;.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="1bcb5-152">Güncelleştirme `MovieInitializer` sınıfı aşağıdaki kod içerir:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="1bcb5-153">`MovieInitializer` Sınıf modeli tarafından kullanılan veritabanı bırakılacak ve modeli sınıfları herhangi bir zamanda değiştirmeniz durumunda otomatik olarak yeniden oluşturulması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="1bcb5-154">Kodu içeren bir `Seed` otomatik olarak herhangi bir veritabanına eklemek için bazı varsayılan veri süresi belirtmek için yöntemi oluşturduğu (veya yeniden oluşturulur).</span><span class="sxs-lookup"><span data-stu-id="1bcb5-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="1bcb5-155">Bu, el ile değiştirmek model yaptığınızda doldurmak gerek kalmadan veritabanını bazı örnek verilerle doldurmak için kullanışlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="1bcb5-156">Tanımladığınız göre `MovieInitializer` sınıfı, istemeniz yukarı, kablo böylece uygulama her çalıştırıldığında, modeli sınıfları veritabanındaki şemasından farklı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="1bcb5-157">Bunlar Başlatıcı modelle eşleşecek ve veritabanı örnek verileri ile doldurmak için veritabanını yeniden oluşturmak için çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="1bcb5-158">Açık *Global.asax* kökünde dosya `MvcMovies` proje:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="1bcb5-159">*Global.asax* dosyayı içeren tüm uygulama projesi için tanımlar ve içeren sınıf bir `Application_Start` uygulamayı ilk kez başlatıldığında çalıştırılan olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="1bcb5-160">Bul `Application_Start` yöntemi ve bir çağrı ekleyin `Database.SetInitializer` aşağıda gösterildiği gibi yöntem başında:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="1bcb5-161">`Database.SetInitializer` Eklediğiniz deyimi gösterir veritabanı tarafından kullanılan `MovieDBContext` örneği otomatik olarak silinecek ve şema ve veritabanı eşleşmiyorsa yeniden oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="1bcb5-162">Ve gördüğünüz gibi aynı zamanda belirtilen örnek verileri veritabanıyla dolduracaktır `MovieInitializer` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="1bcb5-163">Kapat *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="1bcb5-164">Uygulamayı yeniden çalıştırın ve gidin */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="1bcb5-165">Uygulama başladığında model yapısını artık veritabanı şeması eşleştiğini algılar.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="1bcb5-166">Otomatik olarak yeni model yapısını eşleştirmek üzere veritabanını yeniden oluşturur ve örnek filmler veritabanıyla doldurur:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="1bcb5-168">Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="1bcb5-169">Bir derecelendirme ekleyebileceğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-169">Note that you can add a rating.</span></span>

<span data-ttu-id="1bcb5-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1bcb5-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="1bcb5-171">
              **Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-171">Click **Create**.</span></span> <span data-ttu-id="1bcb5-172">Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="1bcb5-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="1bcb5-174">Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="1bcb5-175">Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="1bcb5-176">Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="1bcb5-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1bcb5-177">[Önceki](examining-the-edit-methods-and-edit-view.md)
[sonraki](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="1bcb5-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
