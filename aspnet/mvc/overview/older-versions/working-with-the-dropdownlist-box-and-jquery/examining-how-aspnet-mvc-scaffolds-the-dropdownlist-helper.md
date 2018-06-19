---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC DropDownList yardımcı nasıl scaffolds inceleniyor | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 09d2d7a0df5e8ffa14160b7d3c16b1e9da905fa1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874089"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="fc9ff-102">ASP.NET MVC DropDownList yardımcı nasıl scaffolds inceleniyor</span><span class="sxs-lookup"><span data-stu-id="fc9ff-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="fc9ff-103">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fc9ff-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="fc9ff-104">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="fc9ff-105">Denetleyici adı **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="fc9ff-106">İçin seçenekleri ayarlayın **denetleyici Ekle** aşağıdaki resimde gösterildiği gibi iletişim.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="fc9ff-107">Düzen *StoreManager\Index.cshtml* görüntülemek ve kaldırmak `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="fc9ff-108">Kaldırma `AlbumArtUrl` sunu daha okunabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="fc9ff-109">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="fc9ff-110">Açık *Controllers\StoreManagerController.cs* dosya ve Bul `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="fc9ff-111">Ekleme `OrderBy` albümleri fiyatına göre sıralanır şekilde yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="fc9ff-112">Tam kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="fc9ff-113">Fiyat göre sıralama değişiklikler veritabanına test etmek daha kolay yapar.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="fc9ff-114">Düzen sınama ve yöntemleri oluştururken kaydedilen veriler ilk görünecek şekilde düşük fiyat kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="fc9ff-115">Açık *StoreManager\Edit.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="fc9ff-116">Yalnızca gösterge etiketinden sonra aşağıdaki satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="fc9ff-117">Aşağıdaki kod bu değişikliği bağlamı gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="fc9ff-118">`AlbumId` Bir albüm kaydı değişiklik yapmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="fc9ff-119">Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="fc9ff-120">İçin Select **yönetici** bağlantı sonra seçmek **Yeni Oluştur** yeni albümü oluşturmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="fc9ff-121">Albüm bilgisi kaydedilmiş doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-121">Verify the album information was saved.</span></span> <span data-ttu-id="fc9ff-122">Albüm düzenleyin ve yaptığınız değişiklikleri kalıcı doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="fc9ff-123">Albüm şeması</span><span class="sxs-lookup"><span data-stu-id="fc9ff-123">The Album Schema</span></span>

<span data-ttu-id="fc9ff-124">`StoreManager` MVC yapı iskelesi mekanizması tarafından oluşturulan denetleyicisi müzik deposu veritabanında albümleri CRUD (oluşturma, okuma, güncelleştirme, silme) erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="fc9ff-125">Şemanın albüm bilgi için aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="fc9ff-126">`Albums` Tablo albüm tarz ve açıklama saklamadığı, yabancı anahtar depolar `Genres` tablo.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="fc9ff-127">`Genres` Tablosu bir tarzını ad ve açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="fc9ff-128">Benzer şekilde, `Albums` albüm Sanatçılar adı, ancak bir yabancı anahtar tablosu içermiyor `Artists` tablo.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="fc9ff-129">`Artists` Tablosu sanatçı adını içerir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="fc9ff-130">Verileri incelerseniz `Albums` tablo, her bir satır içeren bir yabancı anahtar görebilirsiniz `Genres` tablo ve yabancı anahtar `Artists` tablo.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="fc9ff-131">Aşağıdaki görüntü Göster bazı tablo verileri `Albums` tablo.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="fc9ff-132">HTML Select etiketi</span><span class="sxs-lookup"><span data-stu-id="fc9ff-132">The HTML Select Tag</span></span>

<span data-ttu-id="fc9ff-133">HTML `<select>` öğesi (HTML tarafından oluşturulan [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yardımcı) (örneğin, türler listesi) değerleri tam bir listesini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="fc9ff-134">Geçerli değer bilindiğinde düzenleme formlar için geçerli değer seçim listesi görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="fc9ff-135">Gördüğümüz daha önce bu biz seçili değerine ayarlandığında **Komedi**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="fc9ff-136">Seçim listesi, kategori veya yabancı anahtar verileri görüntülemek için idealdir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="fc9ff-137">`<select>` Öğesi Tarz yabancı anahtar için olası bir tarzını adları listesini görüntüler, ancak formun kaydettiğinizde Tarz özelliği Tarz yabancı anahtar değeriyle görüntülenen Tarz adı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="fc9ff-138">Aşağıdaki resimde seçili Tarz olan **DISCO** ve sanatçı **Donna yaz**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="fc9ff-139">ASP.NET MVC inceleniyor iskele kurulmuş kodu</span><span class="sxs-lookup"><span data-stu-id="fc9ff-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="fc9ff-140">Açık *Controllers\StoreManagerController.cs* dosya ve Bul `HTTP GET Create` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="fc9ff-141">`Create` Yöntemi iki ekler [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) nesneleri `ViewBag`, bir tarzını bilgiler içeren ve diğeri sanatçı bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="fc9ff-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Oluşturucusu aşırı yukarıda kullanılan üç bağımsız değişkeni alır:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="fc9ff-143">*öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) listedeki öğeleri içeren.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="fc9ff-144">Yukarıdaki örnekte türler listesi tarafından döndürülen `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="fc9ff-145">*dataValueField*: bir özellik adı **IEnumerable** anahtar değeri içeren liste.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="fc9ff-146">Yukarıdaki örnekte `GenreId` ve `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="fc9ff-147">*dataTextField*: bir özellik adı **IEnumerable** görüntülemek için bilgi içeren liste.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="fc9ff-148">Hem sanatçılar, hem de Tarz tablo `name` alanı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="fc9ff-149">Açık *Views\StoreManager\Create.cshtml* dosya ve inceleyin `Html.DropDownList` bir tarzını alan için yardımcı biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="fc9ff-150">İlk satırı oluştur görünümünün aldığını gösteren bir `Album` modeli.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="fc9ff-151">İçinde `Create` yukarıda gösterilen hiçbir model geçti, görünümü alır şekilde yöntemi bir **null** `Album` modeli.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="fc9ff-152">Biz yok şekilde bu noktada yeni albümü oluşturuyoruz `Album` için bu verileri.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="fc9ff-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yukarıda gösterilen aşırı modele bağlanacak alanın adını alır.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="fc9ff-154">Aranacak bu ad ayrıca kullanan bir **ViewBag** nesnesini içeren bir [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="fc9ff-155">Bu aşırı yüklemesi'ni kullanarak, adına gereklidir **ViewBag SelectList** nesne `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="fc9ff-156">İkinci parametre (`String.Empty`) hiçbir öğe seçildiğinde görüntülenecek metin.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="fc9ff-157">Bu, tam olarak yeni albümü oluştururken ne istiyoruz olur.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="fc9ff-158">İkinci parametre kaldırıldı ve aşağıdaki kodu kullanılan varsa:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="fc9ff-159">Seçim listesi ilk öğe veya Rock bizim örnek varsayılan.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="fc9ff-160">İnceleme `HTTP POST Create` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="fc9ff-161">Bu aşırı yüklemesini `Create` metodu bir `album` gönderilen form değerleri ASP.NET MVC model bağlama sistemi tarafından oluşturulan nesne.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="fc9ff-162">Model durumu geçerli olduğundan ve hiçbir veritabanı hataları varsa yeni bir albümü gönderdiğinizde, yeni albümü veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="fc9ff-163">Aşağıdaki resimde yeni albümü oluşturulmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="fc9ff-164">Kullanabileceğiniz [fiddler aracı](http://www.fiddler2.com/fiddler2/) gönderilen form değerleri incelemek için albüm nesnesi oluşturmak için ASP.NET MVC model bağlamanın kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="fc9ff-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="fc9ff-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="fc9ff-166">Görünüm Paketi SelectList oluşturma yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="fc9ff-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="fc9ff-167">Her iki `Edit` yöntemleri ve `HTTP POST Create` yöntemine sahip ayarlamak için aynı kodu **SelectList** içinde **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="fc9ff-168">Ruhunu içinde [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself), bu kod yeniden.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="fc9ff-169">Biz yapacağız bu kullanımını kodu daha sonra yeniden.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="fc9ff-170">Bir tarzını ve sanatçı eklemek için yeni bir yöntem oluşturma **SelectList** için **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="fc9ff-171">Ayar iki satır Değiştir `ViewBag` her birinde `Create` ve `Edit` çağrısıyla yöntemleri `SetGenreArtistViewBag` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="fc9ff-172">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="fc9ff-173">Yeni bir albümü oluşturun ve değişiklikleri çalışma doğrulamak için bir albümü düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="fc9ff-174">Açıkça SelectList DropDownList geçirme</span><span class="sxs-lookup"><span data-stu-id="fc9ff-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="fc9ff-175">Oluşturma ve düzenleme görünümleri aşağıdaki ASP.NET MVC askılama özelliğinin kullanımına oluşturulan **DropDownList** aşırı yükleme:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="fc9ff-176">`DropDownList` Oluştur görünümünün için biçimlendirme aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="fc9ff-177">Çünkü `ViewBag` özelliği için `SelectList` adlı `GenreId`, **DropDownList** yardımcı kullanacağınız `GenreId` **SelectList** içinde **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="fc9ff-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="fc9ff-178">Aşağıdaki **DropDownList** aşırı yükleme, `SelectList` açıkça geçirilen.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="fc9ff-179">Açık *Views\StoreManager\Edit.cshtml* dosya ve değiştirme **DropDownList** açıkça geçirin çağrısı **SelectList**, yukarıdaki aşırı yüklemeleri kullanarak.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="fc9ff-180">Tarz kategorisi için bunu.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-180">Do this for the Genre category.</span></span> <span data-ttu-id="fc9ff-181">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="fc9ff-182">Uygulamayı çalıştırın ve tıklayın **yönetici** bağlantı sonra bir Jazz albümü gidin ve seçin **Düzenle** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="fc9ff-183">Şu anda seçili Tarz olarak Jazz gösteren yerine Rock görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="fc9ff-184">Zaman dize bağımsız değişkeni (bağlamak için özellik) ve **SelectList** nesnesi, aynı ada sahip, seçili değer kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="fc9ff-185">Sağlanan seçili değer olduğunda tarayıcılar varsayılan ilk öğe olarak **SelectList**(olduğu **Rock** Yukarıdaki örnekteki).</span><span class="sxs-lookup"><span data-stu-id="fc9ff-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="fc9ff-186">Bu bir bilinen kısıtlamasıdır **DropDownList** Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="fc9ff-187">Açık *Controllers\StoreManagerController.cs* dosya ve değişiklik **SelectList** nesne adlarını `Genres` ve `Artists`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="fc9ff-188">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="fc9ff-189">Türler ve Sanatçılar adları daha fazlasını her kategori kimliği içerdiğinden kategorileri için daha iyi adlarıdır.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="fc9ff-190">Daha önce yaptığımız yeniden düzenleme Ücretli devre dışı.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="fc9ff-191">Değiştirme yerine **ViewBag** değişikliklerimizi dört yöntemleri için yalıtılmış `SetGenreArtistViewBag` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="fc9ff-192">Değişiklik **DropDownList** Oluştur çağırın ve yeni görünümler Düzenle **SelectList** adları.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="fc9ff-193">Yeni markup düzenleme görünümü için aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="fc9ff-194">Oluştur görünümünün SelectList listedeki ilk öğe görüntülenmesini önlemek için boş bir dize gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="fc9ff-195">Yeni bir albümü oluşturun ve değişiklikleri çalışma doğrulamak için bir albümü düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="fc9ff-196">Düzen kodu Rock dışında bir tarzını ile albüm seçerek sınayın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="fc9ff-197">Bir görünüm modeli DropDownList Yardımcısı ile kullanma</span><span class="sxs-lookup"><span data-stu-id="fc9ff-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="fc9ff-198">Adlı ViewModels klasöründe yeni bir sınıf oluşturun `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="fc9ff-199">Kodla `AlbumSelectListViewModel` aşağıdaki sınıfı:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="fc9ff-200">`AlbumSelectListViewModel` Oluşturucusu albümü, Sanatçılar ve türler listesini alır ve albümü içeren bir nesne oluşturur ve bir `SelectList` türler ve tasarımcıları.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="fc9ff-201">Projeyi derlemek böylece `AlbumSelectListViewModel` biz sonraki adımda bir görünüm oluşturduğunuzda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="fc9ff-202">Ekleme bir `EditVM` yöntemine `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="fc9ff-203">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="fc9ff-204">Sağ tıklayın `AlbumSelectListViewModel`seçin **gidermek**, ardından **MvcMusicStore.ViewModels; kullanarak**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="fc9ff-205">Alternatif olarak, aşağıdaki ekleyebilirsiniz deyimi kullanarak:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="fc9ff-206">Sağ tıklayın `EditVM` seçip **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="fc9ff-207">Aşağıda gösterilen seçenekleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="fc9ff-208">Seçin **Ekle**, içeriğini değiştirmek *Views\StoreManager\EditVM.cshtml* aşağıdaki dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="fc9ff-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="fc9ff-209">`EditVM` Biçimlendirme asıl çok benzer `Edit` biçimlendirme aşağıdaki istisnalar dışında.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="fc9ff-210">Model özelliklerinde `Edit` görünüm şu biçimdedir `model.property`(örneğin, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="fc9ff-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="fc9ff-211">Model özelliklerinde `EditVm` görünüm şu biçimdedir `model.Album.property`(örneğin, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="fc9ff-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="fc9ff-212">Çünkü `EditVM` görünüm bir kapsayıcı geçirilir bir `Album`değil bir `Album` olarak `Edit` görünümü.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="fc9ff-213">**DropDownList** ikinci parametre gelen görünüm modelden değil **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="fc9ff-214">**BeginForm** yardımcı `EditVM` görünüm açıkça yazılarını geri `Edit` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="fc9ff-215">Göndererek başa `Edit` eylem, biz yazmak zorunda değilsiniz bir `HTTP POST EditVM` eylem ve yeniden kullanabilirsiniz `HTTP POST` `Edit` eylem.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="fc9ff-216">Uygulamayı çalıştırın ve bir albüm düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-216">Run the application and edit an album.</span></span> <span data-ttu-id="fc9ff-217">URL'sini değiştirmek `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="fc9ff-218">Bir alan değiştirin ve isabet **kaydetmek** düğmesi kodu çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="fc9ff-219">Hangi yaklaşımın kullanmalısınız?</span><span class="sxs-lookup"><span data-stu-id="fc9ff-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="fc9ff-220">Gösterilen tüm üç yaklaşımlar acceptible.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="fc9ff-221">Çoğu geliştiricinin explictily geçişine tercih `SelectList` için `DropDownList` kullanarak `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="fc9ff-222">Bu yaklaşım, koleksiyon için daha uygun bir ad kullanarak esnekliğini vermiş eklenen avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="fc9ff-223">Bir uyarı olamaz adı olan `ViewBag SelectList` model özelliği adıyla aynı nesne.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="fc9ff-224">Bazı geliştiriciler ViewModel yaklaşımı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="fc9ff-225">Başkalarının daha ayrıntılı biçimlendirme göz önünde bulundurun ve HTML ViewModel yaklaşımın bir dezavantajı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="fc9ff-226">Bu bölümde biz kullanarak için üç yaklaşım öğrendiniz **DropDownList** kategori verilerle.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="fc9ff-227">Sonraki bölümde, yeni bir kategori ekleme göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="fc9ff-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc9ff-228">[Önceki](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [sonraki](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="fc9ff-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
