---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: ASP.NET MVC tarafından DropDownList Yardımcısı nasıl iskele oluşturulduğunu İnceleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 8e407d5426ed93388b3c6f7bdb125be5356c002c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837136"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>ASP.NET MVC tarafından DropDownList Yardımcısı nasıl iskele oluşturulduğunu İnceleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**. Denetleyici adı **StoreManagerController**. Seçeneklerini ayarlayın **denetleyici Ekle** aşağıdaki resimde gösterildiği gibi iletişim.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Düzen *StoreManager\Index.cshtml* görüntülemek ve kaldırmak `AlbumArtUrl`. Kaldırma `AlbumArtUrl` sunu daha okunabilir hale getirir. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Açık *Controllers\StoreManagerController.cs* dosya ve bulma `Index` yöntemi. Ekleme `OrderBy` albümleri fiyatına göre sıralanır şekilde yan tümcesi. Tüm kod aşağıda gösterilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Fiyatına göre sıralama veritabanına değişiklikleri test etmek kolaylaştırır. Düzen test yöntemleri oluşturduğunuzda, kaydedilmiş verilerle bir ilk görünecek şekilde, düşük bir fiyatla kullanabilirsiniz.

Açık *StoreManager\Edit.cshtml* dosya. Yeni gösterge etiketinden sonra şu satırı ekleyin.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Aşağıdaki kod bu değişikliği bağlamı gösterir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Bir albüm kaydı değişiklik yapmak için gereklidir.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Seçin **yönetici** bağlamak ve ardından **Yeni Oluştur** yeni albümü oluşturmak için bağlantı. Albüm bilgileri kaydedildi doğrulayın. Albüm düzenleyin ve yaptığınız değişiklikleri kalıcı doğrulayın.

### <a name="the-album-schema"></a>Albüm şeması

`StoreManager` MVC yapı iskelesi mekanizması tarafından oluşturulan denetleyici müzik deposu veritabanında albümleri CRUD (oluşturma, okuma, güncelleştirme, silme) erişim sağlar. Şema albüm bilgi için aşağıda gösterilmiştir:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Albüm tarz ve açıklama tablo depolamaz, yabancı anahtar için saklayan `Genres` tablo. `Genres` Tablo tarz ad ve açıklama içerir. Benzer şekilde, `Albums` albüm Sanatçılar adı, ancak bir yabancı anahtar tablosunu içermiyor `Artists` tablo. `Artists` Tablo sanatçının adını içerir. Verileri incelerseniz `Albums` tablo, her bir satır içeren bir yabancı anahtar görebilirsiniz `Genres` tablosunu ve yabancı anahtar `Artists` tablo. Aşağıdaki resimde, bazı tablo verilerinden Göster `Albums` tablo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML seçin etiketi

HTML `<select>` öğesi (HTML tarafından oluşturulan [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Yardımcısı) (örneğin, tür listesi) değerleri tam bir listesini görüntülemek için kullanılır. Geçerli değer bulunduğunda formları düzenleme için geçerli değer seçim listesi görüntüleyebilirsiniz. Gördüğümüz daha önce bu biz seçilen değeri ayarlandığında **Komedi**. Seçim listesi, kategori veya yabancı anahtar verileri görüntülemek için idealdir. `<select>` Öğesi türü için yabancı anahtarı için olası Tarz adları listesi görüntüler ancak formu kaydettiğinizde Tarz özelliği Tarz yabancı anahtar değeriyle görüntülenen Tarz adı değil güncelleştiriliyor. Aşağıdaki görüntüde, seçilen türe olan **DISCO** ve sanatçının **Donna yaz**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>ASP.NET MVC İnceleme iskele kurulmuş kodu

Açık *Controllers\StoreManagerController.cs* dosya ve bulma `HTTP GET Create` yöntemi.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Yöntemi iki ekler [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) nesneleri için `ViewBag`, tarz bilgilerini içeren ve diğeri sanatçının bilgileri içermelidir. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Oluşturucusu aşırı yüklemesini yukarıda kullanılan üç bağımsız değişken alır:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) listesindeki öğeleri içeren. Yukarıdaki örnekte, döndürülen tür listesi tarafından `db.Genres`.
2. *dataValueField*: özelliğin adını **IEnumerable** anahtar değeri içeren liste. Yukarıdaki örnekte `GenreId` ve `ArtistId`.
3. *dataTextField*: özelliğin adını **IEnumerable** görüntülenecek bilgileri içeren liste. Sanatçıların ve Tarz tablo `name` alanı kullanılır.

Açık *Views\StoreManager\Create.cshtml* inceleyin ve dosya `Html.DropDownList` Tarz alanın Yardımcısı biçimlendirme.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

İlk satırı oluştur görünümünün aldığını gösteren bir `Album` modeli. İçinde `Create` yukarıda gösterilen modeli bulunmayan geçirildi görünümünü alır, böylece yöntemi bir **null** `Album` modeli. Yok, dolayısıyla bu noktada yeni bir albümü oluşturuyoruz `Album` , verileri.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yukarıda gösterilen aşırı modele bağlanacak alanın adını alır. Aramak için de kullanır bu ada bir **ViewBag** nesne içeren bir [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) nesne. Bu aşırı yüklemesini kullanarak, adına gereklidir **ViewBag SelectList** nesne `GenreId`. İkinci parametre (`String.Empty`) hiçbir öğe seçili olduğunda görüntülenecek metin. Yeni bir albümü oluştururken istediğimiz tam olarak budur. İkinci parametresi kaldırıldı ve aşağıdaki kodu kullanılan varsa:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Seçim listesi ilk öğe veya Rock örneğimizde varsayılan.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

İnceleme `HTTP POST Create` yöntemi.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Bu aşırı yüklemesini `Create` yöntemi bir `album` gönderilen form değerleri ASP.NET MVC model bağlama sistem tarafından oluşturulan nesne. Model durumu geçerli olduğundan ve hiçbir veritabanı hataları varsa yeni bir albümü gönderdiğinizde, yeni albümü veritabanına eklenir. Aşağıdaki görüntüde, yeni albümü oluşturulmasını gösterir.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Kullanabileceğiniz [fiddler aracı](http://www.fiddler2.com/fiddler2/) gönderilen form değerlerini incelemek için albüm nesnesi oluşturmak için ASP.NET MVC, model bağlama kullanır.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Görünüm paketini SelectList oluşturmayı yeniden düzenleme

Her iki `Edit` yöntemleri ve `HTTP POST Create` yöntemine sahip ayarlamak için aynı kodu **SelectList** içinde **ViewBag**. Ruhunu içinde [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself), biz bu kodu yeniden düzenleme. Oluşturacağız kullanımı bu kodu daha sonra yeniden düzenlenen.

Bir türe ve sanatçının eklemek için yeni bir yöntem oluşturma **SelectList** için **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Aşağıdaki iki satırı ayarı değiştirin `ViewBag` her birinde `Create` ve `Edit` yöntem çağrısı ile `SetGenreArtistViewBag` yöntemi. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Yeni albümü oluşturmak ve iş değişiklikleri doğrulamak için albüm düzenleyin.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Açıkça SelectList DropDownList'e geçirme

Oluşturma ve düzenleme görünümleri, aşağıdaki ASP.NET MVC yapı iskelesi kullanımı tarafından oluşturulan **DropDownList** aşırı yükleme:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Oluştur görünümünün için biçimlendirme aşağıda gösterilmektedir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Çünkü `ViewBag` özelliği `SelectList` adlı `GenreId`, **DropDownList** Yardımcısı kullanacağı `GenreId` **SelectList** içinde **ViewBag** . Aşağıdaki **DropDownList** aşırı yükleme, `SelectList` açıkça geçirilir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Açık *Views\StoreManager\Edit.cshtml* dosyasını bulun ve değiştirin **DropDownList** açıkça geçirin aranacak **SelectList**, yukarıdaki aşırı yüklemesi kullanma. Bu tarz kategorisi için yapın. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Uygulamayı çalıştırmak ve tıklayın **yönetici** bağlantı sonra Caz albümü gidip seçmek **Düzenle** bağlantı.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Şu anda seçili Tarz olarak Caz göstermek yerine Rock görüntülenir. Zaman dize bağımsız değişkeni (bağlanacak özelliğin) ve **SelectList** nesnesi aynı ada sahip, seçilen değeri kullanılmaz. Seçilen değer sağlanmadı olduğunda tarayıcılar içindeki ilk öğeye varsayılan **SelectList**(olduğu **Rock** Yukarıdaki örnekteki). Bu bilinen bir sınırlaması, **DropDownList** Yardımcısı.

Açık *Controllers\StoreManagerController.cs* dosya ve değiştirme **SelectList** nesne adları `Genres` ve `Artists`. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Adları, türleri ve sanatçıların daha fazlasını her kategori kimliği içerdikleri gibi kategorileri için daha iyi adlarıdır. Daha önce yaptığımız yeniden düzenleme Ücretli devre dışı. Değiştirme yerine **ViewBag** dört yöntemleri, yaptığımız değişiklikleri için yalıtılmış `SetGenreArtistViewBag` yöntemi.

Değişiklik **DropDownList** Oluştur çağırın ve yeni görünümler Düzenle **SelectList** adları. Düzenleme görünümü için yeni biçimlendirme aşağıda gösterilmiştir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Oluştur görünümünün SelectList ilk öğesinde görüntülenmesini engellemek için boş bir dize gerektirir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Yeni albümü oluşturmak ve iş değişiklikleri doğrulamak için albüm düzenleyin. Albüm Rock dışında bir türe sahip seçerek düzenleme kodu test edin.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Bir görünüm modeli ile DropDownList Yardımcısını kullanma

Adlı Viewmodel'lar klasörde yeni bir sınıf oluşturun `AlbumSelectListViewModel`. Değiştirin `AlbumSelectListViewModel` aşağıdaki sınıfı:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Oluşturucu albümü, sanatçıların ve türleri listesini alır ve albümü içeren bir nesne oluşturur ve bir `SelectList` türleri ve tasarımcıları.

Proje derleme böylece `AlbumSelectListViewModel` sonraki adımda bir görünüm oluşturacağız olduğunda kullanılabilir.

Ekleme bir `EditVM` yönteme `StoreManagerController`. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Sağ tıklayın `AlbumSelectListViewModel`seçin **çözmek**, ardından **MvcMusicStore.ViewModels; kullanarak**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternatif olarak, aşağıdaki ekleyebilirsiniz using deyimi:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Sağ tıklayın `EditVM` seçip **Görünüm Ekle**. Aşağıda gösterilen seçenekleri kullanın.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Seçin **Ekle**, ardından içeriğini değiştirin *Views\StoreManager\EditVM.cshtml* aşağıdaki dosya:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Biçimlendirme özgün çok benzer `Edit` biçimlendirme aşağıdaki istisnalar dışında.

- Model özelliklerinde `Edit` görünümü şu biçimdedir `model.property`(örneğin, `model.Title` ). Model özelliklerinde `EditVm` görünümü şu biçimdedir `model.Album.property`(örneğin, `model.Album.Title`). Çünkü `EditVM` görünümü bir kapsayıcı geçirilir bir `Album`değil bir `Album` olarak `Edit` görünümü.
- **DropDownList** ikinci parametre görünümü modelden değil gelir **ViewBag**.
- **BeginForm** yardımcı `EditVM` görünümü açıkça gönderileri geri `Edit` eylem yöntemi. Geri göndererek `Edit` eylemi, biz yazmak zorunda değilsiniz bir `HTTP POST EditVM` eylem ve yeniden `HTTP POST` `Edit` eylem.

Uygulamayı çalıştırmak ve albüm düzenleyin. Kullanılacak URL değiştirme `EditVM`. Bir alanı değiştirmek ve isabet **Kaydet** kod çalıştığından emin olmak için düğme.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Hangi yaklaşımın kullanmalısınız?

Gösterilen tüm üç yaklaşımları birinin dahil edilmemesi kabul. Birçok geliştiricinin tercih explictily geçişine `SelectList` için `DropDownList` kullanarak `ViewBag`. Bu yaklaşım, koleksiyon için daha uygun bir ad kullanarak esnekliğini fayda vardır. Bir uyarı olamaz adı olan `ViewBag SelectList` model özelliği aynı ada nesne.

Bazı geliştiriciler ViewModel yaklaşımı tercih eder. Başkalarının daha ayrıntılı biçimlendirmeyi göz önünde bulundurun ve HTML ViewModel yaklaşımın bir dezavantajı oluşturan.

Bu bölümde üç yaklaşımları kullanarak edindiğimiz **DropDownList** kategori verilerle. Sonraki bölümde, yeni kategori eklemek nasıl göstereceğiz.

> [!div class="step-by-step"]
> [Önceki](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [İleri](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
