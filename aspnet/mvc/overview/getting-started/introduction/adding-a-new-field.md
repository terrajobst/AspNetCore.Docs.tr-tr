---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Yeni bir alan ekleyerek | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a><span data-ttu-id="afd40-102">Yeni bir alan ekleme</span><span class="sxs-lookup"><span data-stu-id="afd40-102">Adding a New Field</span></span>
====================
<span data-ttu-id="afd40-103">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="afd40-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="afd40-104">Bu bölümde değişiklik veritabanına uyguladınız bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="afd40-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="afd40-105">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="afd40-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="afd40-106">Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="afd40-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="afd40-107">Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="afd40-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="afd40-108">Model değişiklikleri için Code First Migrations kurulması</span><span class="sxs-lookup"><span data-stu-id="afd40-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="afd40-109">Çözüm Gezgini'ne gidin.</span><span class="sxs-lookup"><span data-stu-id="afd40-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="afd40-110">Sağ tıklayın *Movies.mdf* dosya ve seçin **silmek** filmler veritabanını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="afd40-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="afd40-111">Görmüyorsanız, *Movies.mdf* dosya, tıklayın **tüm dosyaları göster** kırmızı hattaki aşağıda simgesi.</span><span class="sxs-lookup"><span data-stu-id="afd40-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="afd40-112">Hiçbir hata bulunmadığından emin olmak için uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="afd40-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="afd40-113">Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="afd40-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Paketi adam ekleme](adding-a-new-field/_static/image2.png)

<span data-ttu-id="afd40-115">İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istem girin</span><span class="sxs-lookup"><span data-stu-id="afd40-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="afd40-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="afd40-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="afd40-117">**Enable-Migrations** (yukarıda gösterilen) komut oluşturur bir *Configuration.cs* yeni bir dosyada *geçişler* klasör.</span><span class="sxs-lookup"><span data-stu-id="afd40-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="afd40-118">Visual Studio açılır *Configuration.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="afd40-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="afd40-119">Değiştir `Seed` yönteminde *Configuration.cs* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="afd40-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="afd40-120">Altında kırmızı kırık çizgi üzerine gelerek `Movie` tıklatıp `Show Potential Fixes` ve ardından **kullanarak** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="afd40-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="afd40-121">Bunun yapılması ekler aşağıdaki using deyimi:</span><span class="sxs-lookup"><span data-stu-id="afd40-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="afd40-122">Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **update-database** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya if ekler satır güncelleştirir. Bunlar henüz mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="afd40-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="afd40-123">[Örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi aşağıdaki kodda bir "upsert" işlemi gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="afd40-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="afd40-124">Çünkü [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi her geçiş ile çalışır, eklemeye çalıştığınız satır zaten var. bir veritabanı oluşturur ilk geçişten sonra olacağından, verileri, yalnızca ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="afd40-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="afd40-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" işlemi zaten bir satır eklemeye çalışırsanız, olacağını hataları engeller, ancak uygulamayı test ederken yapmış olabileceğiniz veri değişiklikleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="afd40-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="afd40-126">Bazı tablolardaki test verilerle, gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi sonra veritabanı güncelleştirmeleri kalmasını istiyor.</span><span class="sxs-lookup"><span data-stu-id="afd40-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="afd40-127">Bu durumda bir koşullu ekleme işlemi yapmak istiyor: yalnızca zaten yoksa bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="afd40-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="afd40-128">Geçirilen ilk parametre [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bir satır zaten var olup olmadığını denetlemek için kullanılacak özellik belirtir.</span><span class="sxs-lookup"><span data-stu-id="afd40-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="afd40-129">Sağlama, test film veriler için `Title` özellik listesinde her başlık benzersiz olduğundan bu amaç için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="afd40-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="afd40-130">Bu kod başlıkları benzersiz olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="afd40-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="afd40-131">Yinelenen bir başlık el ile eklerseniz, şu özel bir geçiş gerçekleştirmek sonraki açışınızda elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="afd40-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="afd40-132">*Sıra birden fazla öğe içeriyor*</span><span class="sxs-lookup"><span data-stu-id="afd40-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="afd40-133">Hakkında daha fazla bilgi için [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi, bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="afd40-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="afd40-134">**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Bu noktada yapı yok aşağıdaki adımları başarısız olur.)</span><span class="sxs-lookup"><span data-stu-id="afd40-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="afd40-135">Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf.</span><span class="sxs-lookup"><span data-stu-id="afd40-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="afd40-136">Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* bir önceki adımda dosya.</span><span class="sxs-lookup"><span data-stu-id="afd40-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="afd40-137">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin `add-migration Initial` ilk geçiş oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="afd40-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="afd40-138">"Başlangıç" adı isteğe bağlıdır ve oluşturulan geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afd40-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="afd40-139">Code First geçişleri oluşturur başka bir sınıf dosyasında *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="afd40-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="afd40-140">Geçiş filename damgasıyla sıralama ile yardımcı olmak için önceden düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="afd40-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="afd40-141">İncelemek *{tarih damgası}\_Initial.cs* dosyasını oluşturmaya ilişkin yönergeleri içeren `Movies` film DB tablo.</span><span class="sxs-lookup"><span data-stu-id="afd40-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="afd40-142">Aşağıda, bu yönergeler veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şeması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="afd40-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="afd40-143">Ardından **çekirdek** yöntemi DB test verileri ile doldurmak için çalışacak.</span><span class="sxs-lookup"><span data-stu-id="afd40-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="afd40-144">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin `update-database` veritabanını oluşturmak ve çalıştırmak için `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="afd40-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="afd40-145">Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, uygulama veritabanı silinmiş sonra ve, yürütülmeden önce çalışan, büyük olasılıkla çünkü `update-database`.</span><span class="sxs-lookup"><span data-stu-id="afd40-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="afd40-146">Bu durumda, silme *Movies.mdf* dosyasını yeniden ve yeniden deneme `update-database` komutu.</span><span class="sxs-lookup"><span data-stu-id="afd40-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="afd40-147">Hala bir hata alırsanız, geçişler klasörünü ve içeriğini silin sonra bu sayfanın üst kısmındaki yönergeleri ile başlayın (delete olan *Movies.mdf* dosya sonra Enable-Migrations devam).</span><span class="sxs-lookup"><span data-stu-id="afd40-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="afd40-148">Bir eror hala alırsanız, SQL Server nesne Gezgini'ni açın ve veritabanını listeden kaldırın.</span><span class="sxs-lookup"><span data-stu-id="afd40-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="afd40-149">Uygulamayı çalıştırın ve gidin */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="afd40-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="afd40-150">Çekirdek verileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="afd40-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="afd40-151">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="afd40-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="afd40-152">Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="afd40-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="afd40-153">Açık *Models\Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="afd40-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="afd40-154">Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:</span><span class="sxs-lookup"><span data-stu-id="afd40-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="afd40-155">Uygulama (Ctrl + Shift + B) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="afd40-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="afd40-156">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı, ayrıca gerekir bağlama güncelleştirmek *beyaz liste* bu yeni özelliği eklenecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="afd40-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="afd40-157">Güncelleştirme `bind` için öznitelik `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="afd40-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="afd40-158">Ayrıca görüntülemek, oluşturmak ve yeni düzenlemek için görünümü şablonlarını güncelleştirmek gereken `Rating` tarayıcı görünümünde özelliği.</span><span class="sxs-lookup"><span data-stu-id="afd40-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="afd40-159">Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı **fiyat** sütun.</span><span class="sxs-lookup"><span data-stu-id="afd40-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="afd40-160">Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="afd40-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="afd40-161">Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* şablonu görüntüleme gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="afd40-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="afd40-162">Ardından, açık *\Views\Movies\Create.cshtml* dosya ve ekleme `Rating` aşağıdaki highlighed biçimlendirme içeren alan.</span><span class="sxs-lookup"><span data-stu-id="afd40-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="afd40-163">Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="afd40-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="afd40-164">Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="afd40-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="afd40-165">Uygulamayı çalıştırın ve gidin */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="afd40-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="afd40-166">Ancak, bunu yaptığınızda, aşağıdaki hatalardan biri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="afd40-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="afd40-167">Veritabanının oluşturulmasından 'MovieDBContext' bağlamını destekleyen model değişti.</span><span class="sxs-lookup"><span data-stu-id="afd40-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="afd40-168">Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="afd40-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="afd40-169">Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo.</span><span class="sxs-lookup"><span data-stu-id="afd40-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="afd40-170">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="afd40-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="afd40-171">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="afd40-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="afd40-172">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="afd40-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="afd40-173">Bu yaklaşım erken zaman, bir test veritabanı üzerindeki etkin geliştirme yapmakta olduğunuz geliştirme döngüsü çok kolaydır; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="afd40-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="afd40-174">Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="afd40-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="afd40-175">Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür.</span><span class="sxs-lookup"><span data-stu-id="afd40-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="afd40-176">Entity Framework veritabanı başlatıcıları hakkında daha fazla bilgi için bkz: [ASP.NET MVC/Entity Framework Öğreticisi](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="afd40-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="afd40-177">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="afd40-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="afd40-178">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="afd40-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="afd40-179">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="afd40-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="afd40-180">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="afd40-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="afd40-181">Bu öğretici için Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="afd40-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="afd40-182">Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="afd40-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="afd40-183">Migrations\Configuration.cs dosyasını açın ve her film nesnesine bir derecelendirme alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="afd40-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="afd40-184">Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="afd40-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="afd40-185">`add-migration` Komutu geçerli film DB şeması geçerli film modeliyle inceleyin ve yeni modele DB geçirmek için gerekli kodu oluşturmak için geçiş framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="afd40-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="afd40-186">Adı *derecelendirme* rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afd40-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="afd40-187">Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="afd40-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="afd40-188">Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMigration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afd40-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="afd40-189">Çözümü derlemek ve enter `update-database` komutunu **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="afd40-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="afd40-190">Aşağıdaki resimde çıktıda gösterir **Paket Yöneticisi Konsolu** penceresi (tarih damgası eklenmesini *derecelendirme* farklı olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="afd40-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="afd40-191">Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="afd40-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="afd40-192">Yeni Derecelendirme alanı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afd40-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="afd40-193">Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="afd40-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="afd40-194">Bir derecelendirme ekleyebileceğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="afd40-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="afd40-196">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="afd40-196">Click **Create**.</span></span> <span data-ttu-id="afd40-197">Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="afd40-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="afd40-199">Proje geçişler kullanarak, yeni bir alan eklediğinizde veya aksi halde güncelleştirme şeması veritabanını bırakın gerekmez.</span><span class="sxs-lookup"><span data-stu-id="afd40-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="afd40-200">Sonraki bölümde, biz daha fazla şema değişiklikleri yapın ve veritabanını güncelleştirmek için geçişler kullanın.</span><span class="sxs-lookup"><span data-stu-id="afd40-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="afd40-201">Ayrıca eklemelisiniz `Rating` düzenleme, Ayrıntılar ve Delete görünüm şablonları alanı.</span><span class="sxs-lookup"><span data-stu-id="afd40-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="afd40-202">"Update-database" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir geçiş kodu çalışıyor, şema modeli eşleştiğinden.</span><span class="sxs-lookup"><span data-stu-id="afd40-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="afd40-203">Bununla birlikte, "update-database" çalıştıran çalışır `Seed` yeniden, yöntemi ve çekirdek veri birini değiştirdiyseniz, çünkü kaybedilir `Seed` yöntemi upserts veri.</span><span class="sxs-lookup"><span data-stu-id="afd40-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="afd40-204">Daha fazla bilgi edinebilirsiniz `Seed` yönteminde zel Dykstra'nın popüler [ASP.NET MVC/Entity Framework Öğreticisi](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="afd40-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="afd40-205">Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz.</span><span class="sxs-lookup"><span data-stu-id="afd40-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="afd40-206">Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="afd40-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="afd40-207">Bu yalnızca bir giriş Code First, bkz: [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) konu hakkında daha kapsamlı bir öğretici.</span><span class="sxs-lookup"><span data-stu-id="afd40-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="afd40-208">Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="afd40-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="afd40-209">[Önceki](adding-search.md)
> [sonraki](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="afd40-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
