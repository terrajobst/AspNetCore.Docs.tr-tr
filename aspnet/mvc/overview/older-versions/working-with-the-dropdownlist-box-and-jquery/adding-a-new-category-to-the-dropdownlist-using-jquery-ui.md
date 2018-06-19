---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Yeni bir kategori jQuery UI kullanarak DropDownList ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871941"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Yeni bir kategori jQuery UI kullanarak DropDownList ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

HTML `Select` etiketi sabit kategori verileri listesini sunmak için ideal olmakla birlikte, çoğu zaman gereken yeni bir kategori ekleyin. Biz "Opera" Tarz Veritabanımıza kategorileri eklemek istediğiniz varsayın. Bu bölümde, yeni bir kategori ekleyin kullanırız bir iletişim kutusu eklemek için jQuery UI kullanacağız. Aşağıdaki görüntü UI tarayıcıda nasıl sunacaktır gösterir.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Bir kullanıcı seçtiğinde **ekleme yeni Tarz** bağlantı, bir açılır iletişim kutusu isteyen kullanıcı için yeni bir tarzını adı (ve isteğe bağlı olarak bir açıklama). Görüntünün Göster aşağıda **eklemek Tarz** açılan iletişim.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Yeni bir tarzını ad zaman girilir ve **kaydetmek** düğmesi gönderilir, aşağıdakiler gerçekleşir:

1. AJAX çağrısıyla Tarz yeni Tarz veritabanına kaydeder ve yeni Tarz bilgileri (Tarz adı ve kimliği) olarak JSON döndüren denetleyicisi oluşturma yöntemi verileri gönderir.
2. JavaScript yeni Tarz verileri seçme listesine ekler.
3. JavaScript yeni Tarz seçili öğe yapar.

   Aşağıdaki görüntüde **Opera** veritabanına eklenir ve seçili **Tarz** açılan liste. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Açık *Views\StoreManager\Create.cshtml* dosya ve Tarz biçimlendirme şununla değiştirin aşağıdaki kodu:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Kısmi Görünüm Ekle yeni bir tarzını özellik uygulamak için kullanılan jQuery ve JavaScript kanca mantığı içerir. Biz sanatçı UI ile aynı yapmak basit kod tamamladıktan sonra.

Çözüm Gezgini'nde sağ tıklayın *Views\StoreManager* klasörü ve select **Ekle**, ardından **Görünüm**. İçinde **Görünüm adı** giriş, girin `_ChooseGenre` seçip **Ekle**. Biçimlendirme Değiştir *Views\StoreManager\\_ChooseGenre.cshtml* aşağıdaki dosyasıyla:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Biz de geçtiğiniz ilk satırı bildiren bir `Album` modelimizi, tam olarak aynı model deyimi oluşturma görünümünde bulunamadı. Sonraki birkaç satır **etiket** Yardımcısı biçimlendirme. Bir sonraki satır **DropDownList** Yardımcısı çağrısı, tam olarak aynı olduğu gibi özgün oluşturma görünümü. Sonraki satıra ada sahip bir bağlantı ekler `Add New Genre`, ve bir düğme stilleri. Satırını içeren `ValidationMessageFor` doğrudan oluşturma görünümünden kopyalanır. Aşağıdaki satırları:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Kimliği ile gizli div oluşturur `genreDialog`. JQuery takma için kullanacağız bizim **eklemek Tarz** kimliği ile iletişim kutusu `genreDialog` bu DIV içinde Son iki komut dosyası etiketleri Ekle yeni Tarz özelliği uygulamak için kullanacağız JavaScript dosyaları bağlantılar içerir. */Scripts/chooseGenre.js* dosyasıdır sağlanan, projeye, daha sonra öğreticide inceleyeceğiz.

Uygulamayı çalıştırın ve tıklayın **ekleme yeni Tarz** düğmesi. İçinde **eklemek Tarz** iletişim kutusunda, girin **Opera** içinde **adı** giriş kutusu.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Tıklatın **kaydetmek** düğmesi. AJAX çağrısı Opera kategorisi oluşturur ve ardından açılır listeden Opera ile doldurur ve Opera seçili Tarz ayarlar.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Bir sanatçı, başlık ve fiyat girin ve ardından **oluşturma** düğmesi. $8.99 değerinden küçük bir fiyat girerseniz, yeni albümü dizin görünümünün üstünde görünür. Yeni albüm giriş veritabanına kaydedilir doğrulayın.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Yalnızca bir harf ile yeni bir tarzını oluşturmayı deneyin. Aşağıdaki kod *Models\Genre.cs* dosyasını ayarlar Tarz adı minimum ve maksimum uzunluğu.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

İstemci tarafı doğrulama 2 ile 20 karakter arasında bir dize girin bildirir.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Nasıl yeni bir tarzını incelemek için veritabanı ve seçin listesine eklenir.

Açık *Scripts\chooseGenre.js* dosya ve kodu inceleyin.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

İkinci satır kimliği kullanan `genreDialog` bir iletişim kutusu div etiketi oluşturmak için *Views\StoreManager\\_ChooseGenre.cshtml* dosya. Adlandırılmış parametreleri açıklayıcı self çoğu. `autoOpen` Parametresi false olarak ayarlanırsa seçme **oluşturma Tarz** düğmesi açılır iletişim kutusu açıkça (üzerinde ikinci açıklanan). İki düğmelerini iletişim sahip **kaydetmek** ve **iptal**. **İptal** düğmesi iletişim kutusunu kapatır. Aşağıdaki kodda gösterildiği **kaydetmek** düğmesini işlevi.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Seçilir `createGenreForm` kimliği `createGenreForm` Kimliği bulunan aşağıdaki kodda ayarlandığı *Views\Genre\\_CreateGenre.cshtml* dosya.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) yardımcı aşırı kullanılan *Views\Genre\\_CreateGenre.cshtml* dosyası HTML form gönderme URL'sini içeren bir eylem özniteliğiyle oluşturur. Bir tarayıcıda oluşturma albüm sayfası görüntüleme ve tarayıcıda göster kaynak seçerek görebilirsiniz. Aşağıdaki biçimlendirmede form etiketi içeren oluşturulan HTML gösterir.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` satır action özniteliği için bir AJAX çağrısı yapar (`/StoreManager/Create`) ve geçirir verilerden içinde **oluşturma Tarz** iletişim kutusu. Yeni bir tarzını ve isteğe bağlı bir açıklama için ad, verileri oluşur. AJAX çağrısı başarılı olursa, yeni bir tarzını ad ve değer seçin biçimlendirme eklenir ve yeni bir tarzını seçili değerine ayarlanır. Bu dinamik olarak üretilen biçimlendirme olduğundan, tarayıcıda kaynağını görüntüleyerek yeni seçme seçeneğini göremiyorsunuz. Yeni HTML IE 9 F12 geliştirici araçları ile görebilirsiniz. Internet Explorer 9'yeni Seç seçeneği görüntülemek için F12 geliştirici araçları başlatmak için F12 tuşuna basın. Oluştur sayfasına gidin ve yeni bir tarzını Tarz seçim listesinde seçili olmaması için yeni bir tarzını ekleyin. F12 geliştirici araçları:

1. HTML sekmesini seçin.
2. Yenile simgesine basın.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Arama kutusuna GenreID girin.
4. Sonraki simgesini kullanarak,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Aşağıdaki select etiketine gidin:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Son seçenek değerini genişletin.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Aşağıdaki kod *Scripts\chooseGenre.js* dosya nasıl gösterir **ekleme yeni Tarz** düğmesini tıklatın olaya bağlı ve nasıl **ekleme yeni Tarz** iletişim kutusu oluşturulur.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

İlk satırı bağlı bir tıklatın işlev oluşturur **ekleme yeni Tarz** düğmesi. Views\StoreManager aşağıdaki biçimlendirmeden\\_ChooseGenre.cshtml dosya gösterir nasıl **ekleme yeni Tarz** düğmesi oluşturulur:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load yöntemi oluşturur ve Tarz Ekle iletişim kutusu açılır ve jQuery çağırır `parse` istemci doğrulama iletişim kutusuna girilen verileri oluşmaz yöntemi.

Bu bölümde yeni kategori verileri seçme listesine eklemek için kullanılan bir iletişim kutusu oluşturma öğrendiniz. Yeni bir sanatçı sanatçı seçme listesine eklemek için kullanıcı Arabirimi oluşturmak için aynı yordamı kullanabilirsiniz. Bu öğreticide ASP.NET MVC HTML Yardımcısı ile çalışmaya genel bir bakış verdiği **DropDownList**. İle çalışma hakkında ek bilgi için **DropDownList**, aşağıdaki ek başvurular bölümüne bakın. Lütfen Bu öğretici yararlı olup olmadığını bize bildirin.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Ek başvurular

- [ASP.NET MVC – basamaklı açılan listeleri öğretici](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) tarafından [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Seçilen](http://harvesthq.github.com/chosen/) çoklu seçim ve filtreleme destekleyen bir JavaScript eklentisi.

### <a name="contributors"></a>Katkıda Bulunanlar

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Gözden geçirenler

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Önceki](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
