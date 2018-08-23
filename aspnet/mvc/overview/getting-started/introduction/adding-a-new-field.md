---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Yeni alan ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7326f3d80030cdc5db1c25efc31f59f5b51594f4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755382"
---
<a name="adding-a-new-field"></a><span data-ttu-id="2ed35-102">Yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="2ed35-102">Adding a New Field</span></span>
====================
<span data-ttu-id="2ed35-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2ed35-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="2ed35-104">Bu bölümde değişiklik veritabanına uygulanır. Bu nedenle, bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First Migrations'ı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="2ed35-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="2ed35-105">Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="2ed35-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2ed35-106">Entity Framework, bunlar eşit değilse bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2ed35-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="2ed35-107">Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="2ed35-108">Code First Migrations ayarlama Model değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2ed35-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="2ed35-109">Çözüm Gezgini'ne gidin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="2ed35-110">Sağ tıklayın *Movies.mdf* seçin ve dosya **Sil** filmler veritabanını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="2ed35-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="2ed35-111">Görmüyorsanız *Movies.mdf* dosya, tıklayarak **tüm dosyaları göster** aşağıda kırmızı anahat içinde gösterilen simge.</span><span class="sxs-lookup"><span data-stu-id="2ed35-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="2ed35-112">Hiçbir hata olmadığından emin olmak için uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2ed35-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="2ed35-113">Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="2ed35-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Paketi Man Ekle](adding-a-new-field/_static/image2.png)

<span data-ttu-id="2ed35-115">İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istem girin</span><span class="sxs-lookup"><span data-stu-id="2ed35-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="2ed35-116">-ContextTypeName MvcMovie.Models.MovieDBContext geçişleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="2ed35-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="2ed35-117">**Etkinleştir geçişleri** (yukarıda gösterilen) bir komut oluşturur bir *Configuration.cs* yeni dosya *geçişler* klasör.</span><span class="sxs-lookup"><span data-stu-id="2ed35-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="2ed35-118">Visual Studio açılır *Configuration.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="2ed35-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="2ed35-119">Değiştirin `Seed` yönteminde *Configuration.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="2ed35-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="2ed35-120">Altında kırmızı dalgalı çizgi üzerine `Movie` tıklatıp `Show Potential Fixes` ve ardından **kullanarak** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="2ed35-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="2ed35-121">Bunun yapılması ekler aşağıdaki using deyimi:</span><span class="sxs-lookup"><span data-stu-id="2ed35-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="2ed35-122">Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya varsa ekler satırları güncelleştirir. Bunlar henüz yoktur.</span><span class="sxs-lookup"><span data-stu-id="2ed35-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="2ed35-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi aşağıdaki kodda bir "upsert" işlem gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="2ed35-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="2ed35-124">Çünkü [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi, her geçiş ile çalışır, eklemeye çalıştığınız satırların zaten var. veritabanı oluşturan ilk geçişten sonra olacağından, verileri yalnızca ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="2ed35-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" işlemi zaten var olan bir satır eklemeye çalışırsanız olacağını hataları engeller, ancak bu, uygulamayı test ederken yaptığınız değişiklikler geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="2ed35-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="2ed35-126">Bazı tablolar test verileri, bunun gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi veritabanı güncelleştirmelerinden sonra kalmasını istiyor.</span><span class="sxs-lookup"><span data-stu-id="2ed35-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="2ed35-127">Bu durumda koşullu ekleme işlemi yapmak istediğiniz: yalnızca zaten mevcut değilse bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="2ed35-128">Geçirilen ilk parametre [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) özelliği bir satır zaten mevcut olup olmadığını denetlemek için kullanılacak yöntemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="2ed35-129">Sağlama, test film verileri için `Title` özelliği listedeki her başlık benzersiz olduğundan bu amaç için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2ed35-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="2ed35-130">Bu kod, başlıklar benzersiz olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="2ed35-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="2ed35-131">Yinelenen başlığa el ile eklerseniz, sonraki açışınızda bir geçiş gerçekleştirmek şu özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2ed35-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="2ed35-132">*Birden fazla öğe dizisi içeriyor*</span><span class="sxs-lookup"><span data-stu-id="2ed35-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="2ed35-133">Hakkında daha fazla bilgi için [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bkz [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="2ed35-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="2ed35-134">**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Bu noktada yapı yoksa aşağıdaki adımları başarısız olur.)</span><span class="sxs-lookup"><span data-stu-id="2ed35-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="2ed35-135">Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf.</span><span class="sxs-lookup"><span data-stu-id="2ed35-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="2ed35-136">Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* dosya önceki bir adımda.</span><span class="sxs-lookup"><span data-stu-id="2ed35-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="2ed35-137">İçinde **Paket Yöneticisi Konsolu** penceresinde komutu girin `add-migration Initial` ilk geçiş oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="2ed35-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="2ed35-138">' % S'adı "Başlangıç" isteğe bağlıdır ve oluşturulan geçiş dosyasının adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="2ed35-139">Code First geçişleri başka bir sınıf dosyasında oluşturur *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="2ed35-140">Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit.</span><span class="sxs-lookup"><span data-stu-id="2ed35-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="2ed35-141">İnceleme *{tarih damgası}\_Initial.cs* dosyasını oluşturmak için yönergeleri içeren `Movies` film DB için tablo.</span><span class="sxs-lookup"><span data-stu-id="2ed35-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="2ed35-142">Aşağıda, bu yönergeleri veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şema oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2ed35-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="2ed35-143">Ardından **çekirdek** yöntemi, bir veritabanı test verileri ile doldurmak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="2ed35-144">İçinde **Paket Yöneticisi Konsolu**, komutu girin `update-database` veritabanı oluşturmak ve çalıştırmak için `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2ed35-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="2ed35-145">Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, veritabanını ve yürüttüğünüz önce uygulamayı çalıştırdığınız için büyük olasılıkla olduğu `update-database`.</span><span class="sxs-lookup"><span data-stu-id="2ed35-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="2ed35-146">Bu durumda, silme *Movies.mdf* yeniden dosya ve yeniden deneyin `update-database` komutu.</span><span class="sxs-lookup"><span data-stu-id="2ed35-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="2ed35-147">Hata almaya devam ediyorsanız geçişleri klasörünü ve içeriğini silin daha sonra bu sayfanın üst kısmındaki yönergeleri ile başlatın (delete olan *Movies.mdf* dosya sonra Enable-geçişler için devam edin).</span><span class="sxs-lookup"><span data-stu-id="2ed35-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="2ed35-148">Bir yayımlandığında almaya devam ediyorsanız SQL Server nesne Gezgini'ni açın ve veritabanı listeden kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2ed35-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="2ed35-149">Uygulamayı çalıştırmak ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="2ed35-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2ed35-150">Çekirdek veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2ed35-151">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="2ed35-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2ed35-152">Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2ed35-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="2ed35-153">Açık *Models\Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:</span><span class="sxs-lookup"><span data-stu-id="2ed35-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="2ed35-154">Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="2ed35-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="2ed35-155">(Ctrl + Shift + B) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2ed35-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="2ed35-156">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı da ihtiyacınız bağlama güncelleştirilecek *beyaz liste* bu yeni özellik dahil edilecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="2ed35-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="2ed35-157">Güncelleştirme `bind` özniteliğini `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="2ed35-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="2ed35-158">Ayrıca görüntülemek, oluşturmak ve bunları yeni düzenleme görünümü şablonları güncelleştirmeye gerek duyduğunuz `Rating` Tarayıcı Görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="2ed35-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="2ed35-159">Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı **fiyat** sütun.</span><span class="sxs-lookup"><span data-stu-id="2ed35-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="2ed35-160">Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri.</span><span class="sxs-lookup"><span data-stu-id="2ed35-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="2ed35-161">Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* görünüm şablonu şöyle:</span><span class="sxs-lookup"><span data-stu-id="2ed35-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="2ed35-162">Ardından, açık *\Views\Movies\Create.cshtml* dosya ve ekleme `Rating` aşağıdaki highlighed biçimlendirmeye sahip alan.</span><span class="sxs-lookup"><span data-stu-id="2ed35-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="2ed35-163">Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2ed35-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="2ed35-164">Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.</span><span class="sxs-lookup"><span data-stu-id="2ed35-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="2ed35-165">Uygulamayı çalıştırmak ve gidin */Movies* URL'si.</span><span class="sxs-lookup"><span data-stu-id="2ed35-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2ed35-166">Ancak, bunu yaptığınızda, aşağıdaki hatalardan birini görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="2ed35-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="2ed35-167">Veritabanı oluşturulduktan sonra 'MovieDBContext' bağlam yedekleme modeli değişti.</span><span class="sxs-lookup"><span data-stu-id="2ed35-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="2ed35-168">Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="2ed35-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="2ed35-169">Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu.</span><span class="sxs-lookup"><span data-stu-id="2ed35-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="2ed35-170">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="2ed35-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="2ed35-171">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="2ed35-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2ed35-172">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="2ed35-173">Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2ed35-174">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!</span><span class="sxs-lookup"><span data-stu-id="2ed35-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="2ed35-175">Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="2ed35-176">Entity Framework veritabanı başlatıcılar hakkında daha fazla bilgi için bkz. [ASP.NET MVC/Entity Framework öğretici](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2ed35-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="2ed35-177">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2ed35-178">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2ed35-179">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="2ed35-180">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2ed35-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="2ed35-181">Bu öğreticide, Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2ed35-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="2ed35-182">Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ed35-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="2ed35-183">Migrations\Configuration.cs dosyasını açın ve her bir nesnenin film Derecelendirme alanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="2ed35-184">Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2ed35-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="2ed35-185">`add-migration` Komutu geçerli bir film veritabanı şeması ile geçerli film modeli inceleyin ve DB yeni modeline geçirme için gereken kodu oluşturmak için geçiş framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="2ed35-186">Adı *derecelendirme* isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2ed35-187">Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="2ed35-188">Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMigration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="2ed35-189">Çözümü derleyin ve enter `update-database` komutunu **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="2ed35-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="2ed35-190">Çıktıda aşağıdaki resimde gösterilmektedir **Paket Yöneticisi Konsolu** penceresi (tarih damgası eklenmesini *derecelendirme* farklı olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="2ed35-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="2ed35-191">Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="2ed35-192">Yeni derecesi alanını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="2ed35-193">Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2ed35-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="2ed35-194">Derecelendirme ekleyebilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2ed35-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="2ed35-196">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2ed35-196">Click **Create**.</span></span> <span data-ttu-id="2ed35-197">Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2ed35-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="2ed35-199">Proje geçişleri kullanıyorsa, veritabanını yeni bir alan eklediğinizde veya yoksa şemayı güncelleştirmenin bırakma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2ed35-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="2ed35-200">Sonraki bölümde daha fazla şema değişiklikleri yapın ve geçişleri veritabanını güncellemek için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="2ed35-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="2ed35-201">De eklemeniz gerekir `Rating` Düzenle, Ayrıntılar ve Sil görünüm şablonları alanı.</span><span class="sxs-lookup"><span data-stu-id="2ed35-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="2ed35-202">"Update-veritabanı" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir geçiş kodu çalıştırın, şema modeli ile eşleştiği için.</span><span class="sxs-lookup"><span data-stu-id="2ed35-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="2ed35-203">Bununla birlikte, "veritabanını güncelleştir" çalıştıran çalışır `Seed` yeniden yöntemi ve çekirdek veri birini değiştirdiyseniz, çünkü kaybedilir `Seed` yöntemi upsert eder veri.</span><span class="sxs-lookup"><span data-stu-id="2ed35-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="2ed35-204">Daha fazla bilgi edinebilirsiniz `Seed` yönteminde Tom Dykstra'nın popüler [ASP.NET MVC/Entity Framework öğretici](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2ed35-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="2ed35-205">Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="2ed35-206">Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="2ed35-207">Bu yalnızca hızlı bir giriş Code First için bkz: [bir ASP.NET MVC uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) konu ile ilgili daha kapsamlı bir öğretici.</span><span class="sxs-lookup"><span data-stu-id="2ed35-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="2ed35-208">Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.</span><span class="sxs-lookup"><span data-stu-id="2ed35-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ed35-209">[Önceki](adding-search.md)
> [İleri](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="2ed35-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
