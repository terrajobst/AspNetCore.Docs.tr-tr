---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: "HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölüm 2 ile kullanma | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar ASP.NET MV içinde çalışmak nasıl temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 271c244ab0b9e2524a33ea6ff4d41893ce22472f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölüm 2 ile kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar bir ASP.NET MVC Web uygulamasında çalışmak nasıl temellerini öğretmek.


## <a name="adding-an-automatic-datetime-template"></a>Otomatik bir DateTime şablonu ekleme

Bu öğreticinin ilk bölümü nasıl biçimlendirme açıkça belirtmek için modeli için öznitelikler ekleyebilirsiniz ve nasıl açıkça modeli işlemek için kullanılan şablonu belirtebilirsiniz gördünüz. Örneğin, [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) aşağıdaki kodu açıkça özniteliğinde belirtir için biçimlendirme `ReleaseDate` özelliği.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Aşağıdaki örnekte, [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) özniteliğini kullanarak `Date` numaralandırma, belirtir tarih şablonu modeli işlemek için kullanılmalıdır. Yerleşik tarih şablonu projenizde tarih şablon ise kullanılır.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Ancak, ASP. MVC türüyle eşleşen bir tür adıyla eşleşen bir şablon bakarak kuralı-üzerinden-Yapılandırması'nı kullanarak gerçekleştirebilirsiniz. Bu öznitelik ya da kod hiç kullanmadan verileri otomatik olarak biçimlendirir bir şablonu oluşturmanıza olanak sağlar. Model türü özelliklerine otomatik olarak uygulanan bir şablon oluşturmak için bu bölümü öğreticinin [DateTime](https://msdn.microsoft.com/en-us/library/system.datetime.aspx). Şablon türü tüm özelliklerde modeli işlemek için kullanılması gerektiğini belirtmek için bir öznitelik veya diğer yapılandırma kullanmanız gerekmez [DateTime](https://msdn.microsoft.com/en-us/library/system.datetime.aspx).

Ayrıca, tek tek özellikleri veya hatta tek tek alanların görüntüsünü özelleştirmek için bir yol da öğreneceksiniz.

Başlamak için şimdi varolan biçimlendirme bilgilerini kaldırın ve uygulamada tam tarihlerini görüntüler.

Açık *Movie.cs* çıkışı açıklamadan çıkarın ve dosya `DataType` özniteliği `ReleaseDate` özelliği:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.

Dikkat `ReleaseDate` özelliği şimdi görüntüler tarih ve saat biçimlendirme hiçbir bilgi sağlandığında, varsayılan olduğundan.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>CSS stilleri yeni şablonlar test etmek için ekleme

Tarihleri biçimlendirmek için bir şablonu oluşturmadan önce yeni şablonlar uygulayabilirsiniz birkaç CSS stil kurallarını ekleyeceksiniz. Bu, işlenen sayfa yeni şablonu kullandığından emin olun yardımcı olur.

Açık *Content\Site.cs*s dosya ve aşağıdaki CSS kuralları dosyasının en altına ekleyin:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>DateTime görüntüleme şablonları ekleme

Artık yeni şablonu oluşturabilirsiniz. İçinde *Views\Movies* klasörü oluşturmak bir *DisplayTemplates* klasör.

İçinde *görünümler/paylaşılan* klasörü oluşturmak bir *DisplayTemplates* klasör ve bir *EditorTemplates* klasör.

Ekran şablonları *views\shared\displaytemplates konumunda* klasörü tüm denetleyicileri tarafından kullanılır. Ekran şablonları *Views\Movie\DisplayTemplates* klasörü yalnızca kullanılacak `Movie` denetleyicisi. (Şablonda her iki klasörde aynı ada sahip bir şablon görünüyorsa, *Views\Movie\DisplayTemplates* klasörü — diğer bir deyişle, daha özel şablonu — tarafından döndürülen görünümler için önceliklidir `Movie` denetleyici.)

İçinde **Çözüm Gezgini**, genişletin *görünümleri* klasörünü genişletin *paylaşılan* klasörünü ve ardından sağ tıklayarak *views\shared\displaytemplates konumunda* klasör.

Tıklatın **Ekle** ve ardından **Görünüm**. **Görünüm Ekle** iletişim kutusu görüntülenir.

İçinde **Görünüm adı** kutusuna `DateTime`. (, Bu ad türünün adını eşleştirmek için kullanmanız gerekir.)

Seçin **kısmi görünüm olarak oluştur** onay kutusu. Olduğundan emin olun **düzen veya ana sayfa kullanmak** ve **kesin türü belirtilmiş görünüm oluşturmak** onay kutularının seçili değil.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

**Ekle**'yi tıklatın. A *DateTime.cshtml* şablonu oluşturulur *views\shared\displaytemplates konumunda*.

Aşağıdaki resimde gösterildiği *görünümleri* klasöründe **Çözüm Gezgini** sonra `DateTime` görüntüleme ve düzenleyici şablonları oluşturulur.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Açık *Views\Shared\DisplayTemplates\DateTime.cshtml* dosya ve kullandığı aşağıdaki biçimlendirmeleri eklemek [String.Format](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) özelliği bir tarih saat olmadan olarak biçimlendirmek için yöntem. ( `{0:d}` Biçimi kısa tarih biçimi belirtir.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Oluşturmak için bu adımı yineleyin bir `DateTime` şablonunda *Views\Movie\DisplayTemplates* klasör. Aşağıdaki kodda kullanmak *Views\Movie\DisplayTemplates\DateTime.cshtml* dosya.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS sınıfı tarihi kırmızı kalın metin olarak görünen neden olur. Eklediğiniz `loud-1` ne zaman bu özel şablonu kullanılıyor kolayca görebilmek için yalnızca bir geçici önlemi olarak CSS sınıfı.

Neler yaptığınızı oluşturulur ve ASP.NET tarihleri görüntülemek için kullanacağı şablonları özelleştirilebilir. Daha fazla genel şablonu (içinde *views\shared\displaytemplates konumunda* klasörü) basit bir kısa tarih görüntüler. Özellikle olan şablon `Movie` denetleyici (içinde *Views\Movies\DisplayTemplates* klasörü) de biçimlendirilen tarih kırmızı kalın metin olarak görüntüler.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Tarayıcı uygulaması için Index görünümünü işler.

`ReleaseDate` Özelliği artık tarihi görüntüler süresi olmadan koyu kırmızı yazı tipi. Bu, onaylayın yardımcı olur `DateTime` şablonlu yardımcı *Views\Movies\DisplayTemplates* klasörü üzerinde seçili `DateTime` paylaşılan klasörde şablonlu Yardımcısı (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Şimdi yeniden adlandırmak *Views\Movies\DisplayTemplates\DateTime.cshtml* dosya *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.

Bu süre `ReleaseDate` özelliği bir tarih saat ve kalın kırmızı yazı tipi olmadan görüntüler. Veri adına sahip bir şablon türünün, bu gösterir (Bu durumda `DateTime`) otomatik olarak bu türdeki tüm model özelliklerini görüntülemek için kullanılır. Sonra yeniden adlandırılmış *DateTime.cshtml* dosya *LoudDateTime.cshtml*, ASP.NET artık bulunan bir şablonda *Views\Movies\DisplayTemplates* kullanılabilmesi için klasör *DateTime.cshtml* şablondan * Views\Movies\Shared\* klasör.

(Tüm büyük/küçük harf ile şablon dosyası adı oluşturulan şablon eşleştirme büyük küçük harf duyarsız olduğundan. For example, *DATETIME.chstml, datetime.cshtml*, ve *DaTeTiMe.cshtml* tüm eşleşir `DateTime` türü.)

Gözden geçirmek için: Bu noktada, `ReleaseDate` alanı görüntülenir kullanarak *Views\Movies\DisplayTemplates\DateTime.cshtml* verileri kısa tarih biçimini kullanarak görüntüler, ancak Aksi durumda özel bir biçime ekler şablonu.

### <a name="using-uihint-to-specify-a-display-template"></a>Bir görüntü şablonu belirtmek için UIHint kullanma

Çoğu web uygulamanız varsa, `DateTime` alanları ve varsayılan olarak tüm veya çoğu salt tarih biçiminde görüntülemek istediğiniz *DateTime.cshtml* iyi bir yaklaşım şablonudur. Ancak, birkaç tarihleri tam tarihi ve saati görüntülemek istediğiniz yoksa ne? Sorun değil. Ek bir şablon oluşturma ve kullanma [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) tam tarih ve saat için biçimlendirme belirtmek için özniteliği. Bu şablon seçerek geçerli olabilir. Kullanabileceğiniz [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) özniteliği model düzeyinde veya, şablon bir görünüm içindeki belirtebilirsiniz. Bu bölümde, nasıl kullanılacağını görürsünüz `UIHint` tarih-saat alanları için bazı örnekler biçimlendirme seçmeli olarak değiştirmek için öznitelik.

Açık *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* dosya ve var olan kodu aşağıdakilerle değiştirin:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Bu tam tarih ve saat görüntülenmesine neden olur ve yeşil ve büyük metni CSS sınıfı ekler.

Açık *Movie.cs* dosya ve ekleme [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) özniteliğini `ReleaseDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Ne zaman bu ASP.NET MVC, bildirir `ReleaseDate` özelliği (özellikle ve yalnızca hiçbir `DateTime` nesnesi), kullanması gereken *LoudDateTime.cshtml* şablonu.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.

Dikkat `ReleaseDate` özelliği şimdi tarih ve saat büyük yeşil yazı tipiyle görüntüler.

Dönün `UIHint` özniteliğini *Movie.cs* dosyası ve yorum teslim böylece *LoudDateTime.cshtml* şablonu olmaz kullanılabilir. Uygulamayı yeniden çalıştırın. Yayın tarihi büyük ve yeşil görüntülenmez. Bu doğrular *Views\Shared\DisplayTemplates\DateTime.cshtml* şablon dizini ve ayrıntıları görünümlerinde kullanılır.

Daha önce belirtildiği gibi aynı zamanda bazı verileri tek bir örneğini Şablonu Uygula olanak sağlayan bir görünümde bir şablon uygulanabilir. Açık *Views\Movies\Details.cshtml* görünümü. Ekleme `"LoudDateTime"` ikinci parametre olarak [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) çağrısı `ReleaseDate` alan. Tamamlanan kodu şöyle görünür:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Bu belirleyen `LoudDateTime` şablonu, hangi özeliklerin modeline uygulanan bağımsız olarak model özelliği görüntülemek için kullanılmalıdır.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.

Film dizin sayfası kullandığını doğrulayın *Views\Shared\DisplayTemplates\DateTime.cshtml* şablonu (kırmızı kalın) ve *Movie\Details* sayfasını kullanarak *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* şablonu (büyük ve yeşil).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Sonraki bölümde, karmaşık bir tür için bir şablon oluşturacaksınız.

>[!div class="step-by-step"]
[Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[sonraki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
