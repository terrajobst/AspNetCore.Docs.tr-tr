---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "ASP.NET MVC DropDownList yardımcı nasıl scaffolds inceleniyor | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>ASP.NET MVC DropDownList yardımcı nasıl scaffolds inceleniyor
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**. Denetleyici adı **StoreManagerController**. İçin seçenekleri ayarlayın **denetleyici Ekle** aşağıdaki resimde gösterildiği gibi iletişim.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Düzen *StoreManager\Index.cshtml* görüntülemek ve kaldırmak `AlbumArtUrl`. Kaldırma `AlbumArtUrl` sunu daha okunabilir hale getirir. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Açık *Controllers\StoreManagerController.cs* dosya ve Bul `Index` yöntemi. Ekleme `OrderBy` albümleri fiyatına göre sıralanır şekilde yan tümcesi. Tam kod aşağıda verilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Fiyat göre sıralama değişiklikler veritabanına test etmek daha kolay yapar. Düzen sınama ve yöntemleri oluştururken kaydedilen veriler ilk görünecek şekilde düşük fiyat kullanabilirsiniz.

Açık *StoreManager\Edit.cshtml* dosya. Yalnızca gösterge etiketinden sonra aşağıdaki satırı ekleyin.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Aşağıdaki kod bu değişikliği bağlamı gösterir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Bir albüm kaydı değişiklik yapmak için gereklidir.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. İçin Select **yönetici** bağlantı sonra seçmek **Yeni Oluştur** yeni albümü oluşturmak için bağlantı. Albüm bilgisi kaydedilmiş doğrulayın. Albüm düzenleyin ve yaptığınız değişiklikleri kalıcı doğrulayın.

### <a name="the-album-schema"></a>Albüm şeması

`StoreManager` MVC yapı iskelesi mekanizması tarafından oluşturulan denetleyicisi müzik deposu veritabanında albümleri CRUD (oluşturma, okuma, güncelleştirme, silme) erişim sağlar. Şemanın albüm bilgi için aşağıda gösterilmiştir:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Tablo albüm tarz ve açıklama saklamadığı, yabancı anahtar depolar `Genres` tablo. `Genres` Tablosu bir tarzını ad ve açıklama içerir. Benzer şekilde, `Albums` albüm Sanatçılar adı, ancak bir yabancı anahtar tablosu içermiyor `Artists` tablo. `Artists` Tablosu sanatçı adını içerir. Verileri incelerseniz `Albums` tablo, her bir satır içeren bir yabancı anahtar görebilirsiniz `Genres` tablo ve yabancı anahtar `Artists` tablo. Aşağıdaki görüntü Göster bazı tablo verileri `Albums` tablo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML Select etiketi

HTML `<select>` öğesi (HTML tarafından oluşturulan [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yardımcı) (örneğin, türler listesi) değerleri tam bir listesini görüntülemek için kullanılır. Geçerli değer bilindiğinde düzenleme formlar için geçerli değer seçim listesi görüntüleyebilirsiniz. Gördüğümüz daha önce bu biz seçili değerine ayarlandığında **Komedi**. Seçim listesi, kategori veya yabancı anahtar verileri görüntülemek için idealdir. `<select>` Öğesi Tarz yabancı anahtar için olası bir tarzını adları listesini görüntüler, ancak formun kaydettiğinizde Tarz özelliği Tarz yabancı anahtar değeriyle görüntülenen Tarz adı güncelleştirilir. Aşağıdaki resimde seçili Tarz olan **DISCO** ve sanatçı **Donna yaz**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>ASP.NET MVC inceleniyor iskele kurulmuş kodu

Açık *Controllers\StoreManagerController.cs* dosya ve Bul `HTTP GET Create` yöntemi.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Yöntemi iki ekler [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) nesneleri `ViewBag`, bir tarzını bilgiler içeren ve diğeri sanatçı bilgileri içerir. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Oluşturucusu aşırı yukarıda kullanılan üç bağımsız değişkeni alır:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) listedeki öğeleri içeren. Yukarıdaki örnekte türler listesi tarafından döndürülen `db.Genres`.
2. *dataValueField*: bir özellik adı **IEnumerable** anahtar değeri içeren liste. Yukarıdaki örnekte `GenreId` ve `ArtistId`.
3. *dataTextField*: bir özellik adı **IEnumerable** görüntülemek için bilgi içeren liste. Hem sanatçılar, hem de Tarz tablo `name` alanı kullanılır.

Açık *Views\StoreManager\Create.cshtml* dosya ve inceleyin `Html.DropDownList` bir tarzını alan için yardımcı biçimlendirme.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

İlk satırı oluştur görünümünün aldığını gösteren bir `Album` modeli. İçinde `Create` yukarıda gösterilen hiçbir model geçti, görünümü alır şekilde yöntemi bir **null** `Album` modeli. Biz yok şekilde bu noktada yeni albümü oluşturuyoruz `Album` için bu verileri.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) yukarıda gösterilen aşırı modele bağlanacak alanın adını alır. Aranacak bu ad ayrıca kullanan bir **ViewBag** nesnesini içeren bir [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) nesnesi. Bu aşırı yüklemesi'ni kullanarak, adına gereklidir **ViewBag SelectList** nesne `GenreId`. İkinci parametre (`String.Empty`) hiçbir öğe seçildiğinde görüntülenecek metin. Bu, tam olarak yeni albümü oluştururken ne istiyoruz olur. İkinci parametre kaldırıldı ve aşağıdaki kodu kullanılan varsa:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Seçim listesi ilk öğe veya Rock bizim örnek varsayılan.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

İnceleme `HTTP POST Create` yöntemi.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Bu aşırı yüklemesini `Create` metodu bir `album` gönderilen form değerleri ASP.NET MVC model bağlama sistemi tarafından oluşturulan nesne. Model durumu geçerli olduğundan ve hiçbir veritabanı hataları varsa yeni bir albümü gönderdiğinizde, yeni albümü veritabanına eklenir. Aşağıdaki resimde yeni albümü oluşturulmasını gösterir.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Kullanabileceğiniz [fiddler aracı](http://www.fiddler2.com/fiddler2/) gönderilen form değerleri incelemek için albüm nesnesi oluşturmak için ASP.NET MVC model bağlamanın kullanır.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Görünüm Paketi SelectList oluşturma yeniden düzenleme

Her iki `Edit` yöntemleri ve `HTTP POST Create` yöntemine sahip ayarlamak için aynı kodu **SelectList** içinde **ViewBag**. Ruhunu içinde [KURU](http://en.wikipedia.org/wiki/Don't_repeat_yourself), bu kod yeniden. Biz yapacağız bu kullanımını kodu daha sonra yeniden.

Bir tarzını ve sanatçı eklemek için yeni bir yöntem oluşturma **SelectList** için **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Ayar iki satır Değiştir `ViewBag` her birinde `Create` ve `Edit` çağrısıyla yöntemleri `SetGenreArtistViewBag` yöntemi. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Yeni bir albümü oluşturun ve değişiklikleri çalışma doğrulamak için bir albümü düzenleyin.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Açıkça SelectList DropDownList geçirme

Oluşturma ve düzenleme görünümleri aşağıdaki ASP.NET MVC askılama özelliğinin kullanımına oluşturulan **DropDownList** aşırı yükleme:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Oluştur görünümünün için biçimlendirme aşağıda gösterilmektedir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Çünkü `ViewBag` özelliği için `SelectList` adlı `GenreId`, **DropDownList** yardımcı kullanacağınız `GenreId` **SelectList** içinde **ViewBag** . Aşağıdaki **DropDownList** aşırı yükleme, `SelectList` açıkça geçirilen.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Açık *Views\StoreManager\Edit.cshtml* dosya ve değiştirme **DropDownList** açıkça geçirin çağrısı **SelectList**, yukarıdaki aşırı yüklemeleri kullanarak. Tarz kategorisi için bunu. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Uygulamayı çalıştırın ve tıklayın **yönetici** bağlantı sonra bir Jazz albümü gidin ve seçin **Düzenle** bağlantı.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Şu anda seçili Tarz olarak Jazz gösteren yerine Rock görüntülenir. Zaman dize bağımsız değişkeni (bağlamak için özellik) ve **SelectList** nesnesi, aynı ada sahip, seçili değer kullanılmaz. Sağlanan seçili değer olduğunda tarayıcılar varsayılan ilk öğe olarak **SelectList**(olduğu **Rock** Yukarıdaki örnekteki). Bu bir bilinen kısıtlamasıdır **DropDownList** Yardımcısı.

Açık *Controllers\StoreManagerController.cs* dosya ve değişiklik **SelectList** nesne adlarını `Genres` ve `Artists`. Tamamlanan kodu aşağıda gösterilmiştir:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Türler ve Sanatçılar adları daha fazlasını her kategori kimliği içerdiğinden kategorileri için daha iyi adlarıdır. Daha önce yaptığımız yeniden düzenleme Ücretli devre dışı. Değiştirme yerine **ViewBag** değişikliklerimizi dört yöntemleri için yalıtılmış `SetGenreArtistViewBag` yöntemi.

Değişiklik **DropDownList** Oluştur çağırın ve yeni görünümler Düzenle **SelectList** adları. Yeni markup düzenleme görünümü için aşağıda gösterilmiştir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Oluştur görünümünün SelectList listedeki ilk öğe görüntülenmesini önlemek için boş bir dize gerektirir.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Yeni bir albümü oluşturun ve değişiklikleri çalışma doğrulamak için bir albümü düzenleyin. Düzen kodu Rock dışında bir tarzını ile albüm seçerek sınayın.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Bir görünüm modeli DropDownList Yardımcısı ile kullanma

Adlı ViewModels klasöründe yeni bir sınıf oluşturun `AlbumSelectListViewModel`. Kodla `AlbumSelectListViewModel` aşağıdaki sınıfı:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Oluşturucusu albümü, Sanatçılar ve türler listesini alır ve albümü içeren bir nesne oluşturur ve bir `SelectList` türler ve tasarımcıları.

Projeyi derlemek böylece `AlbumSelectListViewModel` biz sonraki adımda bir görünüm oluşturduğunuzda kullanılabilir.

Ekleme bir `EditVM` yöntemine `StoreManagerController`. Tamamlanan kodu aşağıda gösterilmiştir.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Sağ tıklayın `AlbumSelectListViewModel`seçin **gidermek**, ardından **MvcMusicStore.ViewModels; kullanarak**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternatif olarak, aşağıdaki ekleyebilirsiniz deyimi kullanarak:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Sağ tıklayın `EditVM` seçip **Görünüm Ekle**. Aşağıda gösterilen seçenekleri kullanın.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Seçin **Ekle**, içeriğini değiştirmek *Views\StoreManager\EditVM.cshtml* aşağıdaki dosyasıyla:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Biçimlendirme asıl çok benzer `Edit` biçimlendirme aşağıdaki istisnalar dışında.

- Model özelliklerinde `Edit` görünüm şu biçimdedir `model.property`(örneğin, `model.Title` ). Model özelliklerinde `EditVm` görünüm şu biçimdedir `model.Album.property`(örneğin, `model.Album.Title`). Çünkü `EditVM` görünüm bir kapsayıcı geçirilir bir `Album`değil bir `Album` olarak `Edit` görünümü.
- **DropDownList** ikinci parametre gelen görünüm modelden değil **ViewBag**.
- **BeginForm** yardımcı `EditVM` görünüm açıkça yazılarını geri `Edit` eylem yöntemi. Göndererek başa `Edit` eylem, biz yazmak zorunda değilsiniz bir `HTTP POST EditVM` eylem ve yeniden kullanabilirsiniz `HTTP POST` `Edit` eylem.

Uygulamayı çalıştırın ve bir albüm düzenleyin. URL'sini değiştirmek `EditVM`. Bir alan değiştirin ve isabet **kaydetmek** düğmesi kodu çalıştığını doğrulayın.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Hangi yaklaşımın kullanmalısınız?

Gösterilen tüm üç yaklaşımlar acceptible. Çoğu geliştiricinin explictily geçişine tercih `SelectList` için `DropDownList` kullanarak `ViewBag`. Bu yaklaşım, koleksiyon için daha uygun bir ad kullanarak esnekliğini vermiş eklenen avantajına sahiptir. Bir uyarı olamaz adı olan `ViewBag SelectList` model özelliği adıyla aynı nesne.

Bazı geliştiriciler ViewModel yaklaşımı tercih eder. Başkalarının daha ayrıntılı göz önünde bulundurun biçimlendirme ve ViewModel oluşturulan HTML yaklaşan bir dezavantajı.

Bu bölümde biz kullanarak için üç yaklaşım öğrendiniz **DropDownList** kategori verilerle. Sonraki bölümde, yeni bir kategori ekleme göstereceğiz.

>[!div class="step-by-step"]
[Önceki](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[sonraki](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
