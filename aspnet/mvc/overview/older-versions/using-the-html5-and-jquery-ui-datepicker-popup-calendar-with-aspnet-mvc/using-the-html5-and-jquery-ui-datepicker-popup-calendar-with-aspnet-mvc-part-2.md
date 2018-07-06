---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: ASP.NET MVC - bölüm 2 ile HTML5 ve jQuery UI Datepicker Popup Calendar kullanarak | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar, ASP.NET MV ile çalışmaya ilişkin temel bilgileri sağlanır...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 0dc9957bbb13394a4ee225d9966f4004de5714b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839313"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>ASP.NET MVC - bölüm 2 ile HTML5 ve jQuery UI Datepicker Popup Calendar kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar'içinde bir ASP.NET MVC Web uygulaması ile çalışmaya ilişkin temel bilgileri sağlanır.


## <a name="adding-an-automatic-datetime-template"></a>Otomatik bir DateTime şablonu ekleme

Bu öğreticinin ilk bölümünde öznitelikleri biçimlendirme açıkça belirtmek için modeli nasıl ekleyebileceğinizi ve modeli oluşturmak için kullanılan şablonu nasıl açıkça belirtebilirsiniz gördünüz. Örneğin, [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) aşağıdaki kodu açıkça özniteliği belirtir için biçimlendirme `ReleaseDate` özelliği.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Aşağıdaki örnekte, [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) özniteliği kullanarak `Date` numaralandırma tarih şablonu modeli işlemek için kullanılması gerektiğini belirtir. Projenizdeki herhangi bir tarih şablon varsa, yerleşik tarih şablonu kullanılır.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Ancak, ASP. MVC için bir tür adıyla eşleşen bir şablon bakarak kuralı-devretme-yapılandırmayı kullanarak türüyle eşleşen gerçekleştirebilirsiniz. Bu otomatik olarak herhangi bir öznitelik veya kod kullanmadan veri diskini biçimlendiren bir şablonu oluşturmanıza olanak sağlar. Öğreticinin bu bölümünde, model türü özelliklerine otomatik olarak uygulanan bir şablon oluşturursunuz [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Şablon türünün tüm model özelliklerini işlemek için kullanılması gerektiğini belirtmek için bir öznitelik veya diğer Yapılandırması'nı kullanmanız gerekmez [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Ayrıca, özellikler veya hatta ayrı ayrı alanları görüntüsünü özelleştirmek için bir yol da öğreneceksiniz.

Başlamak için şimdi mevcut biçimlendirme bilgileri kaldırın ve tam tarihleri uygulamada görüntülemek.

Açık *Movie.cs* dosya ve açıklama `DataType` özniteliği `ReleaseDate` özelliği:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.

Dikkat `ReleaseDate` özelliği artık görüntüler tarih ve saati biçimlendirme bilgisi yok sağlandığında, varsayılan olduğundan.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>CSS stilleri yeni şablonlar test etmek için ekleme

Tarihleri biçimlendirmek için bir şablon oluşturmadan önce yeni şablonlar için uygulayabileceğiniz bazı CSS stil kuralları ekleyeceksiniz. Bu, yeni şablonun işlenen sayfa kullandığından emin olun yardımcı olur.

Açık *Content\Site.cs*s dosyasını açıp aşağıdaki CSS kurallarını dosyasının alt kısmına ekleyin:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>DateTime görüntüleme şablonları ekleme

Artık yeni şablonu oluşturabilirsiniz. İçinde *Views\Movies* klasör oluşturma bir *DisplayTemplates* klasör.

İçinde *görünümler/paylaşılan* klasör oluşturma bir *DisplayTemplates* klasör ve bir *EditorTemplates* klasör.

Ekran şablonları *views\shared\displaytemplates konumunda* klasörü tüm denetleyicileri tarafından kullanılır. Ekran şablonları *Views\Movie\DisplayTemplates* klasör yalnızca kullanılacak `Movie` denetleyicisi. (Aynı ada sahip bir şablon iki klasörü, şablonda görünürse *Views\Movie\DisplayTemplates* klasör — diğer bir deyişle, daha özel şablonu — tarafından döndürülen görünümler için öncelik kazanır `Movie` denetleyici.)

İçinde **Çözüm Gezgini**, genişletme *görünümleri* klasörünü genişletin *paylaşılan* klasörünü açın ve ardından sağ *views\shared\displaytemplates konumunda* klasör.

Tıklayın **Ekle** ve ardından **görünümü**. **Görünüm Ekle** iletişim kutusu görüntülenir.

İçinde **Görünüm adı** kutusuna `DateTime`. (Bu ad tür adını eşleşmesi için kullanmanız gerekir.)

Seçin **kısmi görünüm olarak oluştur** onay kutusu. Emin olun **düzeni veya ana sayfayı kullan** ve **kesin türü belirtilmiş görünüm oluşturmak** onay kutuları seçili.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

**Ekle**'yi tıklatın. A *DateTime.cshtml* şablon oluşturuldu *views\shared\displaytemplates konumunda*.

Aşağıdaki görüntüde *görünümleri* klasöründe **Çözüm Gezgini** sonra `DateTime` görüntüleme ve düzenleyici şablonları oluşturulur.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Açık *Views\Shared\DisplayTemplates\DateTime.cshtml* kullanan aşağıdaki işaretlemeyi ekleyin ve dosya [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) özelliği bir tarih saat olmadan biçimlendirmek için yöntemi. ( `{0:d}` Biçimini kısa tarih biçimini belirtir.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Oluşturmak için bu adımı yineleyin bir `DateTime` şablonunda *Views\Movie\DisplayTemplates* klasör. Aşağıdaki kodu kullanın *Views\Movie\DisplayTemplates\DateTime.cshtml* dosya.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS sınıfı tarihi kırmızı vurgulanmış olarak ekran neden olur. Eklediğiniz `loud-1` olduğunda bu özel şablonu kullanılıyor kolayca görebilmeniz için yalnızca bir geçici önlem olarak CSS sınıfı.

Ne yaptığınızı oluşturulur ve ASP.NET tarihleri görüntülemek için kullanacağı şablonları özelleştirilebilir. Daha fazla genel şablon (içinde *views\shared\displaytemplates konumunda* klasör) basit bir kısa tarih görüntüler. Özellikle olan şablon `Movie` denetleyici (içinde *Views\Movies\DisplayTemplates* klasör) da biçimlendirilmiş tarih kırmızı kalın metin olarak görüntüler.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Tarayıcı uygulaması için Index görünümünü işler.

`ReleaseDate` Özelliği artık tarihi görüntüler kırmızı kalın yazı tipinde süresini kapsamayan işlem süresi. Böylece onaylayın `DateTime` şablonlu yardımcı *Views\Movies\DisplayTemplates* klasörü üzerinde seçili `DateTime` paylaşılan klasörde şablonlu Yardımcısı (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Şimdi Yeniden Adlandır *Views\Movies\DisplayTemplates\DateTime.cshtml* dosyasını *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.

Bu süre `ReleaseDate` özelliği bir tarih saat ve koyu kırmızı yazı tipi olmadan görüntüler. Verilerin adına sahip bir şablon türünün, bunu göstermektedir (Bu durumda `DateTime`) otomatik olarak bu türdeki tüm model özelliklerini görüntülemek için kullanılır. Sonra yeniden adlandırılmış *DateTime.cshtml* dosyasını *LoudDateTime.cshtml*, ASP.NET, artık bir şablon bulunamadı *Views\Movies\DisplayTemplates* kullanıldıkları şekilde klasörü *DateTime.cshtml* şablondan * Views\Movies\Shared\* klasör.

(Tüm büyük/küçük harf ile şablon dosyasının adı oluşturulan böylece eşleşen şablon büyük/küçük harfe duyarlı. For example, *DATETIME.chstml, datetime.cshtml*, ve *DaTeTiMe.cshtml* tüm BC `DateTime` türü.)

Gözden geçirmek için: Bu noktada, `ReleaseDate` alan görüntüleniyor kullanarak *Views\Movies\DisplayTemplates\DateTime.cshtml* kısa tarih biçimini kullanarak verileri görüntüler, ancak Aksi halde özel bir biçime ekler, şablon.

### <a name="using-uihint-to-specify-a-display-template"></a>Bir ekran şablonu belirtmek için UIHint kullanma

Web uygulamanızın birçok varsa `DateTime` alanları ve varsayılan olarak yalnızca tarih biçiminde tüm veya çoğu, görüntülemek istediğiniz *DateTime.cshtml* iyi bir yaklaşım şablonudur. Ancak, varsa ne tam tarihi ve saati görüntülemek istediğiniz birkaç tarihleri olur? Sorun değil. Ek bir şablon oluşturma ve kullanma [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) tam tarih ve saat için biçimlendirme belirtmek için özniteliği. Bu şablon daha sonra seçici olarak uygulayabilirsiniz. Kullanabileceğiniz [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) özniteliği model düzeyinde veya, şablon bir görünüm içinde belirtebilirsiniz. Bu bölümde, nasıl kullanılacağını göreceğiniz `UIHint` tarih-saat alanları için bazı örnekleri biçimlendirme seçmeli olarak değiştirmek için özniteliği.

Açık *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* dosya ve varolan kodu aşağıdakiyle değiştirin:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Bu, tam tarih ve saat görüntülenmesine neden olur ve yeşil ve büyük metni CSS sınıfı ekler.

Açık *Movie.cs* dosya ve ekleme [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) özniteliğini `ReleaseDate` aşağıdaki örnekte gösterildiği gibi özelliği:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Ne zaman bu ASP.NET MVC, bildirir `ReleaseDate` özelliği (özellikle ve yalnızca hiçbir `DateTime` nesne), kullanması gereken *LoudDateTime.cshtml* şablonu.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.

Dikkat `ReleaseDate` özelliği artık tarih ve saat büyük yeşil yazı tipiyle görüntüler.

Geri dönüp `UIHint` özniteliğini *Movie.cs* dosyası ve yorum teslim böylece *LoudDateTime.cshtml* şablon olmaz kullanılabilir. Uygulamayı yeniden çalıştırın. Yayın tarihi, büyük ve yeşil görüntülenmez. Bu doğrular *Views\Shared\DisplayTemplates\DateTime.cshtml* şablon dizini ve ayrıntıları görünümlerinde kullanılır.

Daha önce bahsedildiği gibi bir şablonda şablon bazı verileri tek bir örneğini geçerli olanak sağlayan bir görünüm de uygulayabilirsiniz. Açık *Views\Movies\Details.cshtml* görünümü. Ekleme `"LoudDateTime"` ikinci parametre olarak [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) çağrısı `ReleaseDate` alan. Tamamlanan kodu şöyle görünür:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Bu belirten `LoudDateTime` şablonu, modele hangi özniteliklerin uygulanan bağımsız olarak model özelliği görüntülemek için kullanılmalıdır.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.

Film dizin sayfası kullandığını doğrulamak *Views\Shared\DisplayTemplates\DateTime.cshtml* şablonu (koyu kırmızı) ve *Movie\Details* sayfasını kullanarak *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* şablonu (büyük ve yeşil).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Sonraki bölümde, bir karmaşık tür için bir şablon oluşturacaksınız.

> [!div class="step-by-step"]
> [Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [İleri](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
