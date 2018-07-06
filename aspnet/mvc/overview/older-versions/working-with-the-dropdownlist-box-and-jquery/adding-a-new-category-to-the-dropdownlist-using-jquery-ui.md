---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: JQuery kullanıcı arabirimini kullanarak DropDownList'e yeni kategori ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802532"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>JQuery kullanıcı arabirimini kullanarak DropDownList'e yeni kategori ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

HTML `Select` etikettir sabit kategori veri listesini göstermek için ideal, ancak çoğu zaman, yeni bir kategori eklemeniz gerekir. "Opera" tarzı veritabanımızdaki kategorileri eklemek istediğimiz varsayın. Bu bölümde, yeni kategori eklemek için kullanabiliriz bir iletişim kutusu eklemek için jQuery kullanıcı Arabirimi kullanacağız. Aşağıdaki görüntüde, UI tarayıcıda nasıl sunacaktır gösterilmektedir.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Bir kullanıcı seçtiğinde **ekleme yeni Tarz** bağlantı, bir açılır iletişim kutusu isteyen kullanıcı için yeni bir türe adı (ve isteğe bağlı olarak bir açıklama). Show aşağıdaki resim **ekleme Tarz** açılır iletişim kutusu.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Ne zaman yeni bir türe adı girildikten ve **Kaydet** düğmesi gönderildi, aşağıdakiler gerçekleşir:

1. AJAX çağrısı yeni Tarz veritabanına kaydeder ve yeni Tarz bilgileri (Tarz adı ve kimliği) olarak JSON döndüren Tarz denetleyicisinin, oluşturma yöntemine veri gönderir.
2. JavaScript yeni Tarz veri seçim listesine ekler.
3. JavaScript yeni Tarz seçili öğe yapar.

   Aşağıdaki görüntüde **Opera** veritabanına eklenen ve seçili **Tarz** açılan liste. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Açık *Views\StoreManager\Create.cshtml* Tarz biçimlendirme aşağıdakiyle değiştirin ve dosya aşağıdaki kodu:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Kısmi görünüm JavaScript ve jQuery Ekle yeni Tarz özelliği uygulamak için kullanılan kanca sağlayan tüm mantığı içerir. Biz sanatçının kullanıcı Arabirimi ile aynı yapmak basit kod tamamladıktan sonra.

Çözüm Gezgini'nde sağ tıklayın *Views\StoreManager* klasörü ve select **Ekle**, ardından **görünümü**. İçinde **Görünüm adı** giriş, girin `_ChooseGenre` seçip **Ekle**. Biçimlendirmeyi Değiştir *Views\StoreManager\\_ChooseGenre.cshtml* aşağıdaki dosya:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

İlk satır içinde geçiriyoruz olduğunu bildirir. bir `Album` modelimizi tam olarak aynı model oluşturma Görünümü'nde bulunan deyiminin. Sonraki birkaç satırlar **etiket** Yardımcısı biçimlendirme. Sonraki satırı **DropDownList** Yardımcısı çağrısı, tam olarak aynı özgün Oluştur görünümünün olduğu gibi. Sonraki satıra ada sahip bir bağlantı ekler `Add New Genre`, ve gibi bir düğme stilleri. İçeren satırda `ValidationMessageFor` doğrudan Oluştur görünümünden kopyalanır. Aşağıdaki satırları:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Gizli bir div kimliği ile oluşturur `genreDialog`. Bağlama için jQuery kullanacağız bizim **ekleme Tarz** Kimliğine sahip iletişim kutusu `genreDialog` bu DIV içinde Son iki komut dosyası etiketlerini Ekle yeni Tarz özelliği uygulamak için kullanacağız JavaScript dosyalarının bağlantılarını içerir. */Scripts/chooseGenre.js* dosyasıdır sağlanan, projedeki bu öğreticinin ilerleyen bölümlerinde inceleyeceğiz.

Uygulamayı çalıştırmak ve tıklayın **ekleme yeni Tarz** düğmesi. İçinde **ekleme Tarz** iletişim kutusuna **Opera** içinde **adı** giriş kutusu.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Tıklayın **Kaydet** düğmesi. AJAX çağrısı Opera kategorisi oluşturur ve ardından açılır listeden Opera ile doldurur ve Opera seçilen türe ayarlar.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Bir sanatçının, başlık ve fiyat girin ve ardından **Oluştur** düğmesi. $8.99 değerinden küçük bir fiyat girerseniz, yeni albümü Index görünümünü üst kısmında görünür. Yeni albüm girdiyi veritabanında kaydedilmiş doğrulayın.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Yalnızca bir harf ile yeni bir türe oluşturmayı deneyin. Aşağıdaki kod içinde *Models\Genre.cs* dosya Tarz adı minimum ve maksimum uzunluğunu ayarlar.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

İstemci tarafı doğrulama 2 ile 20 karakter arasında bir dize girin bildirir.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Nasıl bir yeni Tarz inceleme, veritabanı ve Select LIST eklenir.

Açık *Scripts\chooseGenre.js* dosya ve kodu inceleyin.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

İkinci satır kimliği kullanan `genreDialog` div etiketinde bir iletişim kutusu oluşturmak için *Views\StoreManager\\_ChooseGenre.cshtml* dosya. Adlandırılmış parametreler kendini açıklayıcı çoğu. `autoOpen` Parametresini false olarak ayarlayın seçerek **oluşturma Tarz** düğmesi açılır iletişim kutusu açıkça (ikinci üzerinde açıklanmıştır). İletişim kutusunun iki düğme var **Kaydet** ve **iptal**. **İptal** düğmesi iletişim kutusunu kapatır. Aşağıdaki kodda gösterildiği **Kaydet** düğmesini işlevi.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Seçilir `createGenreForm` kimliği `createGenreForm` Kimliği, aşağıdaki kodda bulunan ayarlandığı *Views\Genre\\_CreateGenre.cshtml* dosya.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) Yardımcısı aşırı kullanılan *Views\Genre\\_CreateGenre.cshtml* dosyası HTML form gönderme URL'sini içeren bir eylem özniteliğiyle oluşturur. Bir tarayıcıda Oluştur albüm sayfası görüntüleme ve tarayıcıda göster kaynak seçerek görebilirsiniz. Aşağıdaki biçimlendirmede oluşturulan HTML form etiketi bulunduğu gösterir.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` satır eylem özniteliği için bir AJAX çağrısı yapar (`/StoreManager/Create`) ve verileri geçirir **oluşturma Tarz** iletişim kutusu. Verileri yeni türe ve isteğe bağlı bir açıklama için ad oluşur. AJAX çağrısı başarılı olursa yeni Tarz ad ve değer seçin işaretlemede eklenir ve yeni Tarz seçili değerine ayarlanır. Bu, dinamik olarak üretilen biçimlendirme olduğu için tarayıcıda kaynak görüntüleyerek yeni seçin seçeneğini göremiyorsunuz. Yeni HTML IE 9 F12 Geliştirici araçlarıyla görebilirsiniz. Yeni seçme seçeneği, Internet Explorer 9'da görüntülemek için F12 Geliştirici Araçları'nı başlatmak için F12 tuşuna basın. Oluşturma sayfasına gidin ve yeni Tarz Tarz seçim listesinde seçili olmaması için yeni bir türe ekleyin. F12 geliştirici araçları:

1. HTML sekmesini seçin.
2. Yenile simgesine basın.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Arama kutusuna GenreID girin.
4. Sonraki simgeyi kullanarak   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Aşağıdaki select etiketine gidin:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Son seçenek değerini genişletin.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Aşağıdaki kod *Scripts\chooseGenre.js* dosyasını nasıl gösterir **yeni Tarz Ekle** düğme tıklama olayı için bağlı ve nasıl **ekleme yeni Tarz** iletişim kutusu oluşturuldu.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Bağlı bir tıklatma işlevi ilk satırı oluşturur **ekleme yeni Tarz** düğmesi. Aşağıdaki Views\StoreManager biçimlendirmeden\\_ChooseGenre.cshtml dosya gösterir nasıl **yeni Tarz ekleme** düğmesi oluşturulur:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load yöntemi oluşturur ve Tarz Ekle iletişim kutusu açılır ve jQuery çağırır `parse` yöntemi için istemci doğrulama iletişim kutusuna girilen veriler üzerinde gerçekleşir.

Bu bölümde yeni kategori verileri bir seçim listesine eklemek için kullanılan bir iletişim kutusu oluşturulacağını öğrendiniz. Yeni bir sanatçının sanatçının seçim listesine eklemek için kullanıcı Arabirimi oluşturmak için aynı yordamı izleyebilirsiniz. Bu öğreticide, ASP.NET MVC HTML Yardımcısı ile çalışmaya genel bir bakış verdiği **DropDownList**. İle çalışma hakkında ek bilgi **DropDownList**, aşağıdaki ek başvurular bölümüne bakın. Lütfen Bu öğretici yararlı olmadığını bize bildirin.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Ek başvurular

- [ASP.NET MVC – basamaklı açılır listeler öğretici](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) tarafından [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Seçilen](http://harvesthq.github.com/chosen/) çoklu seçim ve filtreleme destekleyen bir JavaScript eklentisi.

### <a name="contributors"></a>Katkıda Bulunanlar

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson '](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Gözden geçirenler

- Jean Sébastien Goupil
- [Brad Wilson '](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Önceki](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
