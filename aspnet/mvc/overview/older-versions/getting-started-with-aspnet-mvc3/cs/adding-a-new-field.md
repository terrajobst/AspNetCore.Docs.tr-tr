---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Film modeli ve tablosuna (C#) yeni bir alan ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3408afe6665d353e9570acc2740b5f1b7ba916da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367163"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="c4d82-103">Film modeli ve tablosuna (C#) yeni bir alan ekleme</span><span class="sxs-lookup"><span data-stu-id="c4d82-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="c4d82-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c4d82-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="c4d82-105">Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="c4d82-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="c4d82-106">Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c4d82-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="c4d82-107">Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c4d82-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="c4d82-108">Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="c4d82-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="c4d82-109">Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="c4d82-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="c4d82-110">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c4d82-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="c4d82-111">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="c4d82-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="c4d82-112">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="c4d82-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="c4d82-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)</span><span class="sxs-lookup"><span data-stu-id="c4d82-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="c4d82-114">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="c4d82-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="c4d82-115">C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4d82-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="c4d82-116">[C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="c4d82-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="c4d82-117">Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="c4d82-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="c4d82-118">Bu bölümde model sınıflarına bazı değişiklikler yapmanız ve veritabanı şeması modeli değişikliklerle eşleştirmek için nasıl güncelleştirebilirsiniz öğrenin.</span><span class="sxs-lookup"><span data-stu-id="c4d82-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c4d82-119">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="c4d82-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c4d82-120">Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c4d82-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="c4d82-121">Açık *Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:</span><span class="sxs-lookup"><span data-stu-id="c4d82-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="c4d82-122">Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="c4d82-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="c4d82-123">Kullanarak uygulamayı derleyin **hata ayıklama** &gt; **derleme film** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="c4d82-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="c4d82-124">Güncelleştirdiğinize göre `Model` sınıfı da ihtiyacınız güncelleştirilecek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni desteklemekiçingörüntülemeşablonları`Rating`özelliği.</span><span class="sxs-lookup"><span data-stu-id="c4d82-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="c4d82-125">Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı **fiyat** sütun.</span><span class="sxs-lookup"><span data-stu-id="c4d82-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="c4d82-126">Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="c4d82-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="c4d82-127">Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* görünüm şablonu şöyle:</span><span class="sxs-lookup"><span data-stu-id="c4d82-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="c4d82-128">Ardından, açık *\Views\Movies\Create.cshtml* dosya ve formun sonlarında aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4d82-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="c4d82-129">Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c4d82-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="c4d82-130">Model ve veritabanı şema farklılıkları yönetme</span><span class="sxs-lookup"><span data-stu-id="c4d82-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="c4d82-131">Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c4d82-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="c4d82-132">Şimdi uygulamayı çalıştırabilir ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="c4d82-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="c4d82-133">Ancak, bunu yaptığınızda, aşağıdaki hatayı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="c4d82-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="c4d82-134">Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu.</span><span class="sxs-lookup"><span data-stu-id="c4d82-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="c4d82-135">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="c4d82-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c4d82-136">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="c4d82-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="c4d82-137">Entity Framework, bunlar eşit değilse bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c4d82-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="c4d82-138">Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c4d82-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="c4d82-139">Eşitleme denetimi özelliği, görüntülenecek hata iletisi yalnızca gördüğünüz ne neden olur.</span><span class="sxs-lookup"><span data-stu-id="c4d82-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="c4d82-140">Hatayı çözümlemek için iki yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="c4d82-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="c4d82-141">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="c4d82-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="c4d82-142">Model ve veritabanı şeması birlikte hızla geliştirilebilen izin verdiğinden bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken çok yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="c4d82-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c4d82-143">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="c4d82-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="c4d82-144">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c4d82-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c4d82-145">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="c4d82-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c4d82-146">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c4d82-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="c4d82-147">Bu öğreticide, ilk yaklaşımı kullanacağız — Entity Framework Code model değişiklikleri her zaman otomatik olarak veritabanını yeniden oluşturma First sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c4d82-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="c4d82-148">Otomatik olarak Model değişiklikleri veritabanını yeniden oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4d82-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="c4d82-149">Code First otomatik olarak bırakır ve uygulama için model değiştirme herhangi bir zamanda veritabanını yeniden oluşturur, uygulamayı güncelleştirelim.</span><span class="sxs-lookup"><span data-stu-id="c4d82-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c4d82-150">**Uyarı** otomatik olarak bırakarak ve yalnızca geliştirme veya test veritabanını kullanırken veritabanı yeniden oluşturarak bu yaklaşım etkinleştirmeniz gerekir ve *hiçbir zaman* gerçek verileri içeren bir üretim veritabanında.</span><span class="sxs-lookup"><span data-stu-id="c4d82-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="c4d82-151">Bir üretim sunucusunda kullanarak veri kaybına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c4d82-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="c4d82-152">İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="c4d82-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="c4d82-153">' % S'sınıfı "MovieInitializer" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="c4d82-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="c4d82-154">Güncelleştirme `MovieInitializer` sınıfı aşağıdaki kodu içerir:</span><span class="sxs-lookup"><span data-stu-id="c4d82-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="c4d82-155">`MovieInitializer` Sınıfı model tarafından kullanılan veritabanı bırakılacak ve model sınıfları hiç olmadığı kadar değiştirirseniz otomatik olarak yeniden oluşturulan olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="c4d82-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="c4d82-156">Kod içeren bir `Seed` yöntemi otomatik olarak herhangi bir veritabanına eklemek için bazı varsayılan veri süresi belirtmek için oluşturduğu (veya yeniden oluşturulduğunda).</span><span class="sxs-lookup"><span data-stu-id="c4d82-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="c4d82-157">Bu, bunu değiştirmek için bir model yaptığınız her zaman el ile doldurmak üzere gerek kalmadan bazı örnek verilerle bir veritabanını doldurmak için kullanışlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4d82-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="c4d82-158">Tanımladığınız göre `MovieInitializer` sınıfı, uygulama her çalıştırıldığında, bu model sınıfları veritabanında şemasından farklı olup olmadığını denetler. böylece yedekleme wire olmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c4d82-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="c4d82-159">Böyle bir durumda, model eşleşen ve ardından örnek verileriyle veritabanını doldurmak için veritabanını yeniden oluşturmak için Başlatıcı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4d82-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="c4d82-160">Açık *Global.asax* kökünde dosya `MvcMovies` proje:</span><span class="sxs-lookup"><span data-stu-id="c4d82-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="c4d82-161">*Global.asax* dosyasını içeren proje için uygulamanın tamamını tanımlar ve içeren sınıf bir `Application_Start` uygulama ilk kez başlatıldığında çalıştırılan olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="c4d82-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="c4d82-162">İki ekleyelim using deyimlerini dosyanın üstüne.</span><span class="sxs-lookup"><span data-stu-id="c4d82-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="c4d82-163">İlk Entity Framework ad alanı başvuruyor ve ikinci bir ad alanı başvuruları burada bizim `MovieInitializer` hayatını sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c4d82-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="c4d82-164">Ardından bulun `Application_Start` yöntemi ve bir çağrı ekleyin `Database.SetInitializer` aşağıda gösterildiği gibi yöntemin başında:</span><span class="sxs-lookup"><span data-stu-id="c4d82-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="c4d82-165">`Database.SetInitializer` Eklediğiniz bir ifadeyi gösterir veritabanı tarafından kullanılan `MovieDBContext` örneği otomatik olarak silinecek ve şema ve veritabanı eşleşmiyorsa yeniden oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="c4d82-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="c4d82-166">Ve gördüğünüz gibi ayrıca veritabanını belirtilen örnek verilerle doldurursunuz `MovieInitializer` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c4d82-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="c4d82-167">Kapat *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="c4d82-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="c4d82-168">Uygulamayı yeniden çalıştırın ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="c4d82-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="c4d82-169">Uygulama başladığında, model yapısını artık veritabanı şemasını eşleştiğini algılar.</span><span class="sxs-lookup"><span data-stu-id="c4d82-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="c4d82-170">Otomatik olarak yeni model yapısı için veritabanını yeniden oluşturur ve örnek filmler veritabanıyla doldurur:</span><span class="sxs-lookup"><span data-stu-id="c4d82-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="c4d82-172">Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı.</span><span class="sxs-lookup"><span data-stu-id="c4d82-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="c4d82-173">Derecelendirme ekleyebilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c4d82-173">Note that you can add a rating.</span></span>

<span data-ttu-id="c4d82-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="c4d82-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="c4d82-175">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c4d82-175">Click **Create**.</span></span> <span data-ttu-id="c4d82-176">Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c4d82-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="c4d82-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c4d82-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="c4d82-178">Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz.</span><span class="sxs-lookup"><span data-stu-id="c4d82-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="c4d82-179">Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="c4d82-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="c4d82-180">Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="c4d82-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c4d82-181">[Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="c4d82-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
