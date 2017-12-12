---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölümü 3 ile kullanma | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar ASP.NET MV içinde çalışmak nasıl temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: dc81961094928025e25cf62ce4d51d12bc67b80c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölümü 3 ile kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar bir ASP.NET MVC Web uygulamasında çalışmak nasıl temellerini öğretmek.


## <a name="working-with-complex-types"></a>Karmaşık türleri ile çalışma

Bu bölümde bir adres sınıfı oluşturun ve görüntülemek için bir şablon oluşturmayı öğrenin.

İçinde *modelleri* klasörünü adlı yeni bir sınıf dosyası oluşturma *Person.cs* burada yerleştirdiğiniz iki tür: bir `Person` sınıfı ve bir `Address` sınıfı. `Person` Olarak belirtilmiş bir özellik içereceği sınıfı `Address`. `Address` Türüdür değil gibi yerleşik türlerden birinde yani bir karmaşık tür `int`, `string`, veya `double`. Bunun yerine, çeşitli özelliklere sahiptir. Yeni sınıflar için kod şöyle görünür:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

İçinde `Movie` denetleyicisi aşağıdakileri ekleyin `PersonDetail` eylemin bir kişinin örneği görüntülemek için:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Ardından aşağıdaki kodu ekleyin `Movie` doldurmak için denetleyici `Person` bazı örnek verilerle model:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Açık *Views\Movies\PersonDetail.cshtml* dosya ve eklemek için aşağıdaki biçimlendirme `PersonDetail` görünümü.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Uygulamayı çalıştırın ve gitmek için CTRL + F5'e basın *filmler/PersonDetail*.

`PersonDetail` Görünüm içermiyor `Address` karmaşık tür olarak bu ekran görüntüsünde görebilirsiniz. (Herhangi bir adres gösterilir.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Karmaşık bir tür olduğundan model verileri görüntülenmez. Adres bilgilerini görüntülemek için açık *Views\Movies\PersonDetail.cshtml* dosyasını yeniden ve aşağıdaki biçimlendirmeyi ekleyin.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

İçin tam biçimlendirme `PersonDetail` artık görünümü şu şekildedir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Uygulamayı yeniden çalıştırın ve görüntüleme `PersonDetail` görünümü. Adres bilgilerini şimdi görüntülenir:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Karmaşık tür için bir şablon oluşturma

Bu bölümde işlemek için kullanılan bir şablon oluşturacaksınız `Address` karmaşık tür. İçin bir şablon oluştururken `Address` türü, ASP.NET MVC otomatik olarak kullanabilir, herhangi bir yere uygulamadaki bir adres modeli biçimlendirmek için. Bu işlenmesi denetlemek için bir yöntem sunar `Address` uygulamada yalnızca tek bir yerden türü.

İçinde *views\shared\displaytemplates konumunda* klasörünü adlı kesin türü belirtilmiş bir kısmi görünüm oluşturma **adres**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Tıklatın **Ekle**ve ardından yeni açın *Views\Shared\DisplayTemplates\Address.cshtml* dosya. Yeni Görünüm aşağıdaki oluşturulan biçimlendirme içerir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Uygulamayı çalıştırın ve görüntüleme `PersonDetail` görünümü. Bu süre, `Address` yeni oluşturduğunuz şablonu görüntülemek için kullanılan `Address` karmaşık türü, görüntü aşağıdaki gibi görünür:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Özet: yolları Model görüntüleme biçimi ve şablonu belirtin

Biçimi veya şablon için bir model özelliği aşağıdaki yaklaşımlardan kullanarak belirtebilirsiniz gördünüz:

- Uygulama `DisplayFormat` özniteliği modelinde özelliği. Örneğin, aşağıdaki kodu zaman görüntülenecek tarihi neden olur:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Uygulama bir [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) özniteliği modeli ve veri türünü belirtme özelliği. Örneğin, aşağıdaki kodu zaman görüntülenecek tarihi neden olur.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Uygulama içeriyorsa, bir *date.cshtml* şablonunda *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasörü, bu şablonu işlemek için kullanılan `DateTime` özelliği. Aksi takdirde yerleşik ASP.NET şablon sistem özelliği bir tarih olarak görüntüler.
- Bir ekran şablonu oluşturma *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasör adıyla eşleşen biçimlendirmek istediğiniz veri türü. Örneğin, gördüğünüz *Views\Shared\DisplayTemplates\DateTime.cshtml* işlemek için kullanılan `DateTime` modeli, model için bir öznitelik eklemeden ve görünümlere tüm biçimlendirme eklemeden özelliklerinde.
- Kullanarak [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) özniteliği modelin model özelliği görüntülemek için şablonu belirtin.
- Açıkça görünen şablon adına ekleme [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) bir görünümde çağırın.

Kullandığınız yaklaşım, uygulamanızda gerçekleştirmeniz gereken üzerinde bağlıdır. İhtiyacınız olan biçimlendirme tam olarak türünü almak için bu yaklaşım karışık sık karşılaşılan bir durum değil.

Sonraki bölümde, geçiş yaparsınız biraz kullanılan ve verilerin nasıl girildiğini özelleştirme için nasıl görüntüleneceğini özelleştirme taşıyın. Tarih belirtmek için tıklatmanız bir yol sağlamak üzere uygulamayı Düzenle görünümleri için jQuery datepicker bağlanacağını.

>[!div class="step-by-step"]
[Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[sonraki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
