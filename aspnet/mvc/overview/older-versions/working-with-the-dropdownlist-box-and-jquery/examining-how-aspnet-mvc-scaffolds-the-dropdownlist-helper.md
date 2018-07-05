---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC tarafından DropDownList Yardımcısı nasıl iskele oluşturulduğunu İnceleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ece3645f2b37550058a5c93bdd9badbc088ae11c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382971"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="34fd0-102">ASP.NET MVC tarafından DropDownList Yardımcısı nasıl iskele oluşturulduğunu İnceleme</span><span class="sxs-lookup"><span data-stu-id="34fd0-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="34fd0-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="34fd0-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="34fd0-104">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="34fd0-105">Denetleyici adı **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="34fd0-106">Seçeneklerini ayarlayın **denetleyici Ekle** aşağıdaki resimde gösterildiği gibi iletişim.</span><span class="sxs-lookup"><span data-stu-id="34fd0-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="34fd0-107">Düzen *StoreManager\Index.cshtml* görüntülemek ve kaldırmak `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="34fd0-108">Kaldırma `AlbumArtUrl` sunu daha okunabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="34fd0-109">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="34fd0-110">Açık *Controllers\StoreManagerController.cs* dosya ve bulma `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="34fd0-111">Ekleme `OrderBy` albümleri fiyatına göre sıralanır şekilde yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="34fd0-112">Tüm kod aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="34fd0-113">Fiyatına göre sıralama veritabanına değişiklikleri test etmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="34fd0-114">Düzen test yöntemleri oluşturduğunuzda, kaydedilmiş verilerle bir ilk görünecek şekilde, düşük bir fiyatla kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34fd0-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="34fd0-115">Açık *StoreManager\Edit.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="34fd0-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="34fd0-116">Yeni gösterge etiketinden sonra şu satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="34fd0-117">Aşağıdaki kod bu değişikliği bağlamı gösterir:</span><span class="sxs-lookup"><span data-stu-id="34fd0-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="34fd0-118">`AlbumId` Bir albüm kaydı değişiklik yapmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="34fd0-119">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="34fd0-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="34fd0-120">Seçin **yönetici** bağlamak ve ardından **Yeni Oluştur** yeni albümü oluşturmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="34fd0-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="34fd0-121">Albüm bilgileri kaydedildi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="34fd0-121">Verify the album information was saved.</span></span> <span data-ttu-id="34fd0-122">Albüm düzenleyin ve yaptığınız değişiklikleri kalıcı doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="34fd0-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="34fd0-123">Albüm şeması</span><span class="sxs-lookup"><span data-stu-id="34fd0-123">The Album Schema</span></span>

<span data-ttu-id="34fd0-124">`StoreManager` MVC yapı iskelesi mekanizması tarafından oluşturulan denetleyici müzik deposu veritabanında albümleri CRUD (oluşturma, okuma, güncelleştirme, silme) erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="34fd0-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="34fd0-125">Şema albüm bilgi için aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="34fd0-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="34fd0-126">`Albums` Albüm tarz ve açıklama tablo depolamaz, yabancı anahtar için saklayan `Genres` tablo.</span><span class="sxs-lookup"><span data-stu-id="34fd0-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="34fd0-127">`Genres` Tablo tarz ad ve açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="34fd0-128">Benzer şekilde, `Albums` albüm Sanatçılar adı, ancak bir yabancı anahtar tablosunu içermiyor `Artists` tablo.</span><span class="sxs-lookup"><span data-stu-id="34fd0-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="34fd0-129">`Artists` Tablo sanatçının adını içerir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="34fd0-130">Verileri incelerseniz `Albums` tablo, her bir satır içeren bir yabancı anahtar görebilirsiniz `Genres` tablosunu ve yabancı anahtar `Artists` tablo.</span><span class="sxs-lookup"><span data-stu-id="34fd0-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="34fd0-131">Aşağıdaki resimde, bazı tablo verilerinden Göster `Albums` tablo.</span><span class="sxs-lookup"><span data-stu-id="34fd0-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="34fd0-132">HTML seçin etiketi</span><span class="sxs-lookup"><span data-stu-id="34fd0-132">The HTML Select Tag</span></span>

<span data-ttu-id="34fd0-133">HTML `<select>` öğesi (HTML tarafından oluşturulan [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Yardımcısı) (örneğin, tür listesi) değerleri tam bir listesini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="34fd0-134">Geçerli değer bulunduğunda formları düzenleme için geçerli değer seçim listesi görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34fd0-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="34fd0-135">Gördüğümüz daha önce bu biz seçilen değeri ayarlandığında **Komedi**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="34fd0-136">Seçim listesi, kategori veya yabancı anahtar verileri görüntülemek için idealdir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="34fd0-137">`<select>` Öğesi türü için yabancı anahtarı için olası Tarz adları listesi görüntüler ancak formu kaydettiğinizde Tarz özelliği Tarz yabancı anahtar değeriyle görüntülenen Tarz adı değil güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="34fd0-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="34fd0-138">Aşağıdaki görüntüde, seçilen türe olan **DISCO** ve sanatçının **Donna yaz**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="34fd0-139">ASP.NET MVC İnceleme iskele kurulmuş kodu</span><span class="sxs-lookup"><span data-stu-id="34fd0-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="34fd0-140">Açık *Controllers\StoreManagerController.cs* dosya ve bulma `HTTP GET Create` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="34fd0-141">`Create` Yöntemi iki ekler [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) nesneleri için `ViewBag`, tarz bilgilerini içeren ve diğeri sanatçının bilgileri içermelidir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="34fd0-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Oluşturucusu aşırı yüklemesini yukarıda kullanılan üç bağımsız değişken alır:</span><span class="sxs-lookup"><span data-stu-id="34fd0-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="34fd0-143">*öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) listesindeki öğeleri içeren.</span><span class="sxs-lookup"><span data-stu-id="34fd0-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="34fd0-144">Yukarıdaki örnekte, döndürülen tür listesi tarafından `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="34fd0-145">*dataValueField*: özelliğin adını **IEnumerable** anahtar değeri içeren liste.</span><span class="sxs-lookup"><span data-stu-id="34fd0-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="34fd0-146">Yukarıdaki örnekte `GenreId` ve `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="34fd0-147">*dataTextField*: özelliğin adını **IEnumerable** görüntülenecek bilgileri içeren liste.</span><span class="sxs-lookup"><span data-stu-id="34fd0-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="34fd0-148">Sanatçıların ve Tarz tablo `name` alanı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="34fd0-149">Açık *Views\StoreManager\Create.cshtml* inceleyin ve dosya `Html.DropDownList` Tarz alanın Yardımcısı biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="34fd0-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="34fd0-150">İlk satırı oluştur görünümünün aldığını gösteren bir `Album` modeli.</span><span class="sxs-lookup"><span data-stu-id="34fd0-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="34fd0-151">İçinde `Create` yukarıda gösterilen modeli bulunmayan geçirildi görünümünü alır, böylece yöntemi bir **null** `Album` modeli.</span><span class="sxs-lookup"><span data-stu-id="34fd0-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="34fd0-152">Yok, dolayısıyla bu noktada yeni bir albümü oluşturuyoruz `Album` , verileri.</span><span class="sxs-lookup"><span data-stu-id="34fd0-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="34fd0-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yukarıda gösterilen aşırı modele bağlanacak alanın adını alır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="34fd0-154">Aramak için de kullanır bu ada bir **ViewBag** nesne içeren bir [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) nesne.</span><span class="sxs-lookup"><span data-stu-id="34fd0-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="34fd0-155">Bu aşırı yüklemesini kullanarak, adına gereklidir **ViewBag SelectList** nesne `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="34fd0-156">İkinci parametre (`String.Empty`) hiçbir öğe seçili olduğunda görüntülenecek metin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="34fd0-157">Yeni bir albümü oluştururken istediğimiz tam olarak budur.</span><span class="sxs-lookup"><span data-stu-id="34fd0-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="34fd0-158">İkinci parametresi kaldırıldı ve aşağıdaki kodu kullanılan varsa:</span><span class="sxs-lookup"><span data-stu-id="34fd0-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="34fd0-159">Seçim listesi ilk öğe veya Rock örneğimizde varsayılan.</span><span class="sxs-lookup"><span data-stu-id="34fd0-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="34fd0-160">İnceleme `HTTP POST Create` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="34fd0-161">Bu aşırı yüklemesini `Create` yöntemi bir `album` gönderilen form değerleri ASP.NET MVC model bağlama sistem tarafından oluşturulan nesne.</span><span class="sxs-lookup"><span data-stu-id="34fd0-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="34fd0-162">Model durumu geçerli olduğundan ve hiçbir veritabanı hataları varsa yeni bir albümü gönderdiğinizde, yeni albümü veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="34fd0-163">Aşağıdaki görüntüde, yeni albümü oluşturulmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="34fd0-164">Kullanabileceğiniz [fiddler aracı](http://www.fiddler2.com/fiddler2/) gönderilen form değerlerini incelemek için albüm nesnesi oluşturmak için ASP.NET MVC, model bağlama kullanır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="34fd0-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="34fd0-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="34fd0-166">Görünüm paketini SelectList oluşturmayı yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="34fd0-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="34fd0-167">Her iki `Edit` yöntemleri ve `HTTP POST Create` yöntemine sahip ayarlamak için aynı kodu **SelectList** içinde **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="34fd0-168">Ruhunu içinde [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself), biz bu kodu yeniden düzenleme.</span><span class="sxs-lookup"><span data-stu-id="34fd0-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="34fd0-169">Oluşturacağız kullanımı bu kodu daha sonra yeniden düzenlenen.</span><span class="sxs-lookup"><span data-stu-id="34fd0-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="34fd0-170">Bir türe ve sanatçının eklemek için yeni bir yöntem oluşturma **SelectList** için **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="34fd0-171">Aşağıdaki iki satırı ayarı değiştirin `ViewBag` her birinde `Create` ve `Edit` yöntem çağrısı ile `SetGenreArtistViewBag` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="34fd0-172">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="34fd0-173">Yeni albümü oluşturmak ve iş değişiklikleri doğrulamak için albüm düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="34fd0-174">Açıkça SelectList DropDownList'e geçirme</span><span class="sxs-lookup"><span data-stu-id="34fd0-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="34fd0-175">Oluşturma ve düzenleme görünümleri, aşağıdaki ASP.NET MVC yapı iskelesi kullanımı tarafından oluşturulan **DropDownList** aşırı yükleme:</span><span class="sxs-lookup"><span data-stu-id="34fd0-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="34fd0-176">`DropDownList` Oluştur görünümünün için biçimlendirme aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="34fd0-177">Çünkü `ViewBag` özelliği `SelectList` adlı `GenreId`, **DropDownList** Yardımcısı kullanacağı `GenreId` **SelectList** içinde **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="34fd0-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="34fd0-178">Aşağıdaki **DropDownList** aşırı yükleme, `SelectList` açıkça geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="34fd0-179">Açık *Views\StoreManager\Edit.cshtml* dosyasını bulun ve değiştirin **DropDownList** açıkça geçirin aranacak **SelectList**, yukarıdaki aşırı yüklemesi kullanma.</span><span class="sxs-lookup"><span data-stu-id="34fd0-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="34fd0-180">Bu tarz kategorisi için yapın.</span><span class="sxs-lookup"><span data-stu-id="34fd0-180">Do this for the Genre category.</span></span> <span data-ttu-id="34fd0-181">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="34fd0-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="34fd0-182">Uygulamayı çalıştırmak ve tıklayın **yönetici** bağlantı sonra Caz albümü gidip seçmek **Düzenle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="34fd0-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="34fd0-183">Şu anda seçili Tarz olarak Caz göstermek yerine Rock görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="34fd0-184">Zaman dize bağımsız değişkeni (bağlanacak özelliğin) ve **SelectList** nesnesi aynı ada sahip, seçilen değeri kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="34fd0-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="34fd0-185">Seçilen değer sağlanmadı olduğunda tarayıcılar içindeki ilk öğeye varsayılan **SelectList**(olduğu **Rock** Yukarıdaki örnekteki).</span><span class="sxs-lookup"><span data-stu-id="34fd0-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="34fd0-186">Bu bilinen bir sınırlaması, **DropDownList** Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="34fd0-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="34fd0-187">Açık *Controllers\StoreManagerController.cs* dosya ve değiştirme **SelectList** nesne adları `Genres` ve `Artists`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="34fd0-188">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="34fd0-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="34fd0-189">Adları, türleri ve sanatçıların daha fazlasını her kategori kimliği içerdikleri gibi kategorileri için daha iyi adlarıdır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="34fd0-190">Daha önce yaptığımız yeniden düzenleme Ücretli devre dışı.</span><span class="sxs-lookup"><span data-stu-id="34fd0-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="34fd0-191">Değiştirme yerine **ViewBag** dört yöntemleri, yaptığımız değişiklikleri için yalıtılmış `SetGenreArtistViewBag` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="34fd0-192">Değişiklik **DropDownList** Oluştur çağırın ve yeni görünümler Düzenle **SelectList** adları.</span><span class="sxs-lookup"><span data-stu-id="34fd0-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="34fd0-193">Düzenleme görünümü için yeni biçimlendirme aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="34fd0-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="34fd0-194">Oluştur görünümünün SelectList ilk öğesinde görüntülenmesini engellemek için boş bir dize gerektirir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="34fd0-195">Yeni albümü oluşturmak ve iş değişiklikleri doğrulamak için albüm düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="34fd0-196">Albüm Rock dışında bir türe sahip seçerek düzenleme kodu test edin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="34fd0-197">Bir görünüm modeli ile DropDownList Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="34fd0-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="34fd0-198">Adlı Viewmodel'lar klasörde yeni bir sınıf oluşturun `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="34fd0-199">Değiştirin `AlbumSelectListViewModel` aşağıdaki sınıfı:</span><span class="sxs-lookup"><span data-stu-id="34fd0-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="34fd0-200">`AlbumSelectListViewModel` Oluşturucu albümü, sanatçıların ve türleri listesini alır ve albümü içeren bir nesne oluşturur ve bir `SelectList` türleri ve tasarımcıları.</span><span class="sxs-lookup"><span data-stu-id="34fd0-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="34fd0-201">Proje derleme böylece `AlbumSelectListViewModel` sonraki adımda bir görünüm oluşturacağız olduğunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="34fd0-202">Ekleme bir `EditVM` yönteme `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="34fd0-203">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="34fd0-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="34fd0-204">Sağ tıklayın `AlbumSelectListViewModel`seçin **çözmek**, ardından **MvcMusicStore.ViewModels; kullanarak**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="34fd0-205">Alternatif olarak, aşağıdaki ekleyebilirsiniz using deyimi:</span><span class="sxs-lookup"><span data-stu-id="34fd0-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="34fd0-206">Sağ tıklayın `EditVM` seçip **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="34fd0-207">Aşağıda gösterilen seçenekleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="34fd0-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="34fd0-208">Seçin **Ekle**, ardından içeriğini değiştirin *Views\StoreManager\EditVM.cshtml* aşağıdaki dosya:</span><span class="sxs-lookup"><span data-stu-id="34fd0-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="34fd0-209">`EditVM` Biçimlendirme özgün çok benzer `Edit` biçimlendirme aşağıdaki istisnalar dışında.</span><span class="sxs-lookup"><span data-stu-id="34fd0-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="34fd0-210">Model özelliklerinde `Edit` görünümü şu biçimdedir `model.property`(örneğin, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="34fd0-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="34fd0-211">Model özelliklerinde `EditVm` görünümü şu biçimdedir `model.Album.property`(örneğin, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="34fd0-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="34fd0-212">Çünkü `EditVM` görünümü bir kapsayıcı geçirilir bir `Album`değil bir `Album` olarak `Edit` görünümü.</span><span class="sxs-lookup"><span data-stu-id="34fd0-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="34fd0-213">**DropDownList** ikinci parametre görünümü modelden değil gelir **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="34fd0-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="34fd0-214">**BeginForm** yardımcı `EditVM` görünümü açıkça gönderileri geri `Edit` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="34fd0-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="34fd0-215">Geri göndererek `Edit` eylemi, biz yazmak zorunda değilsiniz bir `HTTP POST EditVM` eylem ve yeniden `HTTP POST` `Edit` eylem.</span><span class="sxs-lookup"><span data-stu-id="34fd0-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="34fd0-216">Uygulamayı çalıştırmak ve albüm düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="34fd0-216">Run the application and edit an album.</span></span> <span data-ttu-id="34fd0-217">Kullanılacak URL değiştirme `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="34fd0-218">Bir alanı değiştirmek ve isabet **Kaydet** kod çalıştığından emin olmak için düğme.</span><span class="sxs-lookup"><span data-stu-id="34fd0-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="34fd0-219">Hangi yaklaşımın kullanmalısınız?</span><span class="sxs-lookup"><span data-stu-id="34fd0-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="34fd0-220">Gösterilen tüm üç yaklaşımları birinin dahil edilmemesi kabul.</span><span class="sxs-lookup"><span data-stu-id="34fd0-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="34fd0-221">Birçok geliştiricinin tercih explictily geçişine `SelectList` için `DropDownList` kullanarak `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="34fd0-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="34fd0-222">Bu yaklaşım, koleksiyon için daha uygun bir ad kullanarak esnekliğini fayda vardır.</span><span class="sxs-lookup"><span data-stu-id="34fd0-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="34fd0-223">Bir uyarı olamaz adı olan `ViewBag SelectList` model özelliği aynı ada nesne.</span><span class="sxs-lookup"><span data-stu-id="34fd0-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="34fd0-224">Bazı geliştiriciler ViewModel yaklaşımı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="34fd0-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="34fd0-225">Başkalarının daha ayrıntılı biçimlendirmeyi göz önünde bulundurun ve HTML ViewModel yaklaşımın bir dezavantajı oluşturan.</span><span class="sxs-lookup"><span data-stu-id="34fd0-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="34fd0-226">Bu bölümde üç yaklaşımları kullanarak edindiğimiz **DropDownList** kategori verilerle.</span><span class="sxs-lookup"><span data-stu-id="34fd0-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="34fd0-227">Sonraki bölümde, yeni kategori eklemek nasıl göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="34fd0-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34fd0-228">[Önceki](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [İleri](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="34fd0-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
