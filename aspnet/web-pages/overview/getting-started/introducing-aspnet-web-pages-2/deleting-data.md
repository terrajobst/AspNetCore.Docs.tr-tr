---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web sayfaları ile tanışın - veritabanı verilerini silme | Microsoft Docs
author: tfitzmac
description: Bu öğreticide tek veritabanı girdiyi Sil gösterilmektedir. Bu, veritabanı verilerinde güncelleştirmeyi ASP.NET Web Pa aracılığıyla serisi tamamladınız varsayar...
ms.author: aspnetcontent
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 234b5f99c5d5f580316204c88ea1ab8c1269d452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815362"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="725c1-104">ASP.NET Web sayfalarına giriş - veritabanı verilerini silme</span><span class="sxs-lookup"><span data-stu-id="725c1-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="725c1-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="725c1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="725c1-106">Bu öğreticide tek veritabanı girdiyi Sil gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="725c1-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="725c1-107">Bu seriyi aracılığıyla bitirdiğinizi [veritabanı verilerini güncelleştirme ASP.NET Web Pages'de](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="725c1-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="725c1-108">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="725c1-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="725c1-109">Tek bir kayıtta kayıtların bir listeden seçmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="725c1-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="725c1-110">Nasıl bir veritabanından tek bir kaydı silinir.</span><span class="sxs-lookup"><span data-stu-id="725c1-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="725c1-111">Belirli bir düğmeyi içinde bir formun tıklandığını nasıl kontrol edileceğini.</span><span class="sxs-lookup"><span data-stu-id="725c1-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="725c1-112">Ele alınan özelliklerin/teknolojiler:</span><span class="sxs-lookup"><span data-stu-id="725c1-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="725c1-113">`WebGrid` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="725c1-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="725c1-114">SQL `Delete` komutu.</span><span class="sxs-lookup"><span data-stu-id="725c1-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="725c1-115">`Database.Execute` SQL çalıştırılacak yöntemi `Delete` komutu.</span><span class="sxs-lookup"><span data-stu-id="725c1-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="725c1-116">Ne oluşturacaksınız</span><span class="sxs-lookup"><span data-stu-id="725c1-116">What You'll Build</span></span>

<span data-ttu-id="725c1-117">Önceki öğreticide, var olan bir veritabanı kaydını güncelleştirme öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="725c1-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="725c1-118">Kaydı güncelleştirmek yerine bunu silersiniz dışında Bu öğreticide benzerdir.</span><span class="sxs-lookup"><span data-stu-id="725c1-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="725c1-119">Bu öğreticide kısa olacak şekilde silme daha basit olması dışında işlemleri çok aynıdır.</span><span class="sxs-lookup"><span data-stu-id="725c1-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="725c1-120">İçinde *filmler* sayfasında, güncelleştirme `WebGrid` yardımcı olan görünmesi bir **Sil** eşlik eden her filmin yanındaki bağlantısını **Düzenle** daha önce eklediğiniz bağlantı.</span><span class="sxs-lookup"><span data-stu-id="725c1-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Bir silme bağlantısı için her filmin gösteren filmler sayfası](deleting-data/_static/image1.png)

<span data-ttu-id="725c1-122">Düzenleme gibi ile tıkladığınızda **Sil** bağlantı sürer, başka bir sayfaya film bilgileri zaten bir biçimde olduğu:</span><span class="sxs-lookup"><span data-stu-id="725c1-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Görüntülenen sahip bir film film sayfayı Sil](deleting-data/_static/image2.png)

<span data-ttu-id="725c1-124">Ardından, kaydı kalıcı olarak silmek için düğmeyi tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="725c1-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="725c1-125">Film listenin bir silme bağlantısı ekleme</span><span class="sxs-lookup"><span data-stu-id="725c1-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="725c1-126">Ekleyerek başlayacaksınız bir **Sil** bağlantı `WebGrid` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="725c1-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="725c1-127">Bu bağlantıyı benzer **Düzenle** bağlantı önceki bir öğreticide eklendi.</span><span class="sxs-lookup"><span data-stu-id="725c1-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="725c1-128">Açık *Movies.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="725c1-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="725c1-129">Değişiklik `WebGrid` biçimlendirme içinde bir sütun ekleyerek sayfasının gövdesi.</span><span class="sxs-lookup"><span data-stu-id="725c1-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="725c1-130">Değiştirilen biçimlendirmesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="725c1-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="725c1-131">Bu yeni bir sütun verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="725c1-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="725c1-132">Kılavuz yapılandırılmış yolu **Düzenle** kılavuzunda en soldaki sütun ve **Sil** en sağdaki sütun.</span><span class="sxs-lookup"><span data-stu-id="725c1-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="725c1-133">(Sonra bir virgül var. `Year` sütun şimdi durumda fark etmemiştir.) Bu bağlantı sütunlar nereye özel bir şey yoktur ve sizin gibi bir kolayca bunları birbirinin yanına yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="725c1-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="725c1-134">Bu durumda, bunlar karışmasına daha zor hale getirmek için ayrı.</span><span class="sxs-lookup"><span data-stu-id="725c1-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Düzenle ve ayrıntıları bağlantılarla filmler sayfası birbirinin yanına olmadıklarını olduğunu göstermek için işaretlenmiş](deleting-data/_static/image3.png)

<span data-ttu-id="725c1-136">Yeni bir sütun bağlantıyı gösterir (`<a>` öğesi) metni "Sil" diyor.</span><span class="sxs-lookup"><span data-stu-id="725c1-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="725c1-137">Bağlantının hedefi (kendi `href` özniteliği) sonuçta bu URL, benzer bir şey ile çözümler kodu `id` her filmin için farklı bir değer:</span><span class="sxs-lookup"><span data-stu-id="725c1-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="725c1-138">Bu bağlantıyı adlı sayfanın çağıracağı *DeleteMovie* ve seçtiğiniz film kimliği geçirin.</span><span class="sxs-lookup"><span data-stu-id="725c1-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="725c1-139">Neredeyse aynı olduğundan, bu Öğreticide bu bağlantıyı nasıl oluşturulur, ilgili ayrıntıya gitmiyor **Düzenle** önceki öğreticide bağlantıdan ([veritabanı verilerini güncelleştirme ASP.NET Web Pages'de](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="725c1-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="725c1-140">Silme sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="725c1-140">Creating the Delete Page</span></span>

<span data-ttu-id="725c1-141">Hedefi olan sayfanın oluşturabilirsiniz artık **Sil** kılavuzunda bağlantı.</span><span class="sxs-lookup"><span data-stu-id="725c1-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="725c1-142">**Önemli** ilk kez bir kaydı silmek için seçme ve ardından işlemini onaylamak için ayrı bir sayfa ve düğmesini kullanarak bir teknik güvenliği için son derece önemlidir.</span><span class="sxs-lookup"><span data-stu-id="725c1-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="725c1-143">Önceki öğreticilerde makaleyi okudunuz, yaparak *herhangi* tür değişiklik sitenize gereken *her zaman* yapılması formu kullanarak &mdash; diğer bir deyişle, bir HTTP POST işlemi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="725c1-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="725c1-144">Site (bir alma işlemi kullanma) bir bağlantıya tıklayarak değiştirmek mümkün hale, kişilerin sitenize basit isteklerde ve verilerinizi silin.</span><span class="sxs-lookup"><span data-stu-id="725c1-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="725c1-145">Hatta sitenizi dizin bir arama motoru Gezgin, yalnızca aşağıdaki bağlantılardan yanlışlıkla veri silebilir.</span><span class="sxs-lookup"><span data-stu-id="725c1-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="725c1-146">Uygulamanızı bir kaydı değiştirme kişilere izin verdiğinde, yine de düzenleme için kullanıcıya kayıt sunmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="725c1-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="725c1-147">Ancak, bir kaydı silmek için bu adımı atlamak için fikri size cazip olabilir.</span><span class="sxs-lookup"><span data-stu-id="725c1-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="725c1-148">Bu adım, ancak atlamayın.</span><span class="sxs-lookup"><span data-stu-id="725c1-148">Don't skip that step, though.</span></span> <span data-ttu-id="725c1-149">(Bu da kaydını görmek ve bunlar kaydı silmekte olduğunuz onaylamak, kullanıcılar için yararlıdır.)</span><span class="sxs-lookup"><span data-stu-id="725c1-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="725c1-150">Bir sonraki öğretici kümesinde, bir kullanıcı bir kayıt silmeden önce oturum açmak bu nedenle oturum açma işlevselliği ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="725c1-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="725c1-151">Adlı bir sayfa oluşturun *DeleteMovie.cshtml* dosyasında aşağıdaki işaretlemeyle nedir değiştirin:</span><span class="sxs-lookup"><span data-stu-id="725c1-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="725c1-152">Bu işaretleme benzer *EditMovie* sayfaları, metin kutuları yerine hariç (`<input type="text">`), biçimlendirme içeren `<span>` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="725c1-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="725c1-153">Düzenlemek için burada şey yoktur.</span><span class="sxs-lookup"><span data-stu-id="725c1-153">There's nothing here to edit.</span></span> <span data-ttu-id="725c1-154">Tek yapmanız gereken olan kullanıcıların doğru film silmekte olduğunuz emin yapabilmeleri için film ayrıntılarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="725c1-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="725c1-155">Biçimlendirme film listesi sayfasına geri dönmek kullanıcının sağlayan bir bağlantı zaten var.</span><span class="sxs-lookup"><span data-stu-id="725c1-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="725c1-156">Olarak *EditMovie* sayfasında, seçili film kimliği, gizli bir alanında saklanır.</span><span class="sxs-lookup"><span data-stu-id="725c1-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="725c1-157">(Bu sayfaya ilk başta bir sorgu dizesi değeri geçirilir.) Var olan bir `Html.ValidationSummary` doğrulama hataları görüntüler çağrısı.</span><span class="sxs-lookup"><span data-stu-id="725c1-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="725c1-158">Bu durumda, hata film kimliği yok sayfasına geçildi veya film kimliği geçersiz olabilir.</span><span class="sxs-lookup"><span data-stu-id="725c1-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="725c1-159">Birisi bu sayfa içinde bir film seçmeden çalıştırdıysanız bu durum ortaya çıkabilir *filmler* sayfası.</span><span class="sxs-lookup"><span data-stu-id="725c1-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="725c1-160">Düğme başlık **Sil film**, ve onun name özniteliği kümesine `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="725c1-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="725c1-161">`name` Özniteliği formun gönderilen düğmeyi saptamak için kodda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="725c1-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="725c1-162">(1) film Ayrıntıları sayfası ilk görüntülendiğinde okumak için kod yazma ve kullanıcı düğmeye tıkladığında film 2) gerçekten silmek gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="725c1-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="725c1-163">Tek bir filmi okumayı kod ekleme</span><span class="sxs-lookup"><span data-stu-id="725c1-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="725c1-164">Üst kısmındaki *DeleteMovie.cshtml* sayfasında, aşağıdaki kod bloğunu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="725c1-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="725c1-165">Bu işaretleme ilgili kod içinde aynıdır *EditMovie* sayfası.</span><span class="sxs-lookup"><span data-stu-id="725c1-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="725c1-166">Sorgu dizesi dışında film Kimliğini alır ve bir kayıt veritabanından okumak için Kimliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="725c1-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="725c1-167">Doğrulama testi kodu içerir (`IsInt()` ve `row != null`) sayfasına geçirilen film Kimliğinin geçerli olduğundan emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="725c1-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="725c1-168">Bu kod yalnızca ilk kez çalıştırdığında sayfasında çalışması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="725c1-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="725c1-169">Kullanıcı tıkladığında veritabanından film kaydı yeniden okunuyorsa istemediğiniz **Sil film** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="725c1-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="725c1-170">Bu nedenle, film olduğunu bildiren bir test içinde okumayı kod `if(!IsPost)` &mdash; diğer bir deyişle, *isteği gönderme işlemi (form gönderme) değilse*.</span><span class="sxs-lookup"><span data-stu-id="725c1-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="725c1-171">Seçili film silmek için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="725c1-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="725c1-172">Kullanıcı düğmeye tıkladığında film silmek için aşağıdaki kodu yalnızca kapanış ayracı Ekle `@` engelle:</span><span class="sxs-lookup"><span data-stu-id="725c1-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="725c1-173">Bu kod, varolan bir kaydı güncelleştirmek için koda benzer, ancak daha basit değildir.</span><span class="sxs-lookup"><span data-stu-id="725c1-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="725c1-174">Kod temel olarak bir SQL çalışır `Delete` deyimi.</span><span class="sxs-lookup"><span data-stu-id="725c1-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="725c1-175">Olarak *EditMovie* kod sayfası, konusu bir `if(IsPost)` blok.</span><span class="sxs-lookup"><span data-stu-id="725c1-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="725c1-176">Bu kez, `if()` biraz daha karmaşık koşul:</span><span class="sxs-lookup"><span data-stu-id="725c1-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="725c1-177">Burada iki koşul vardır.</span><span class="sxs-lookup"><span data-stu-id="725c1-177">There are two conditions here.</span></span> <span data-ttu-id="725c1-178">Sayfa gönderiliyor olduğunu, ilk önce gördüğünüz gibi olduğu &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="725c1-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="725c1-179">İkinci koşul `!Request["buttonDelete"].IsEmpty()`, istek adlı bir nesne olduğu anlamına gelen `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="725c1-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="725c1-180">Kuşkusuz, formun hangi düğmeden gönderilen test bir dolaylı yoludur.</span><span class="sxs-lookup"><span data-stu-id="725c1-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="725c1-181">Bir form birden çok düğmeleri içeriyorsa, istekte yalnızca tıklandığını düğme adı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="725c1-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="725c1-182">Bu nedenle, mantıksal olarak, belirli bir düğmeyi adını istekte görünüp görünmeyeceğini &mdash; veya bu düğmeyi boş değilse kod içinde belirtildiği gibi &mdash; formunu düğmesi olan.</span><span class="sxs-lookup"><span data-stu-id="725c1-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="725c1-183">`&&` İşleci anlamına gelir "ve" (mantıksal ve).</span><span class="sxs-lookup"><span data-stu-id="725c1-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="725c1-184">Bu nedenle tüm `if` koşul...</span><span class="sxs-lookup"><span data-stu-id="725c1-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="725c1-185">*Bu isteği bir gönderi (ilk kez istek değil) olup*</span><span class="sxs-lookup"><span data-stu-id="725c1-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="725c1-186">AND</span><span class="sxs-lookup"><span data-stu-id="725c1-186">AND</span></span>  
  
<span data-ttu-id="725c1-187">`buttonDelete`*Düğmesi formun gönderilen düğmesi oluştu.*</span><span class="sxs-lookup"><span data-stu-id="725c1-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="725c1-188">Bu formu (aslında bu sayfayı) yalnızca bir düğme şekilde içeren ek test için `buttonDelete` teknik olarak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="725c1-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="725c1-189">Yine de verileri kalıcı olarak kaldırır bir işlem gerçekleştirmek üzere olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="725c1-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="725c1-190">Bu nedenle olabildiğince yalnızca kullanıcının açıkça, istediği zaman işlemi gerçekleştiriyorsunuz emin olmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="725c1-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="725c1-191">Örneğin, daha sonra bu sayfayı genişletilmiş ve diğer düğmeleri eklenmiş olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="725c1-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="725c1-192">Film silen kod yalnızca aşağıdaki durumlarda bile çalışır `buttonDelete` düğmeye tıkladı.</span><span class="sxs-lookup"><span data-stu-id="725c1-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="725c1-193">Olarak *EditMovie* sayfasından gizli alanı Kimliğini alın ve ardından SQL komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="725c1-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="725c1-194">Sözdizimi `Delete` deyimidir:</span><span class="sxs-lookup"><span data-stu-id="725c1-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="725c1-195">Dahil etmek için çok önemli `WHERE` yan tümcesi ve kimliği.</span><span class="sxs-lookup"><span data-stu-id="725c1-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="725c1-196">WHERE yan tümcesi bırakırsanız *tablodaki tüm kayıtları silinen*.</span><span class="sxs-lookup"><span data-stu-id="725c1-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="725c1-197">Sizin gördüğünüz gibi kimlik değeri SQL komutu bir yer tutucu kullanarak geçirin.</span><span class="sxs-lookup"><span data-stu-id="725c1-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="725c1-198">Film silme işlemi test etme</span><span class="sxs-lookup"><span data-stu-id="725c1-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="725c1-199">Artık test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="725c1-199">Now you can test.</span></span> <span data-ttu-id="725c1-200">Çalıştırma *filmler* sayfasında ve tıklayın **Sil** film yanında.</span><span class="sxs-lookup"><span data-stu-id="725c1-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="725c1-201">Zaman *DeleteMovie* sayfası görüntülendikten sonra **Sil film**.</span><span class="sxs-lookup"><span data-stu-id="725c1-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Film Sil düğmesi vurgulanmış film sayfayı Sil](deleting-data/_static/image4.png)

<span data-ttu-id="725c1-203">Düğmeye tıkladığınızda, kod filmler siler ve film listeye döndürür.</span><span class="sxs-lookup"><span data-stu-id="725c1-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="725c1-204">Silinen film arama vardır ve silinmiş onaylayın.</span><span class="sxs-lookup"><span data-stu-id="725c1-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="725c1-205">Sıradaki gelen</span><span class="sxs-lookup"><span data-stu-id="725c1-205">Coming Up Next</span></span>

<span data-ttu-id="725c1-206">Sonraki öğreticiye genel bir görünüm ve Düzen sitenizdeki tüm sayfaları bildirimde bulunma konusunda gösterir.</span><span class="sxs-lookup"><span data-stu-id="725c1-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="725c1-207">Tam listesi için (Sil bağlantılarla güncelleştirilmiş) film sayfası</span><span class="sxs-lookup"><span data-stu-id="725c1-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="725c1-208">Tam listesi için DeleteMovie sayfası</span><span class="sxs-lookup"><span data-stu-id="725c1-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="725c1-209">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="725c1-209">Additional Resources</span></span>

- [<span data-ttu-id="725c1-210">ASP.NET Web programlama Razor söz dizimini kullanarak giriş</span><span class="sxs-lookup"><span data-stu-id="725c1-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="725c1-211">[SQL DELETE deyimi](http://www.w3schools.com/sql/sql_delete.asp) W3Schools sitesinde</span><span class="sxs-lookup"><span data-stu-id="725c1-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="725c1-212">[Önceki](updating-data.md)
> [İleri](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="725c1-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
