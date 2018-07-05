---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - 3. Kısım kullanarak | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar, ASP.NET MV ile çalışmaya ilişkin temel bilgileri sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 11f9e0a8602e9ee1feda9d0e7d0d319add7c440c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400777"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - 3. Kısım kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar'içinde bir ASP.NET MVC Web uygulaması ile çalışmaya ilişkin temel bilgileri sağlanır.


## <a name="working-with-complex-types"></a>Karmaşık türleri ile çalışma

Bu bölümde bir adres sınıfı oluşturmak ve görüntülemek için bir şablon oluşturmayı öğrenin.

İçinde *modelleri* klasöründe adlı yeni bir sınıf dosyası oluşturma *Person.cs* nerede yerleştirdiğiniz iki tür: bir `Person` sınıfı ve bir `Address` sınıfı. `Person` Sınıfı olarak belirlenmiş bir özellik içerecek `Address`. `Address` Türü olup olmadığı gibi yerleşik türlerden birinde yani bir karmaşık tür `int`, `string`, veya `double`. Bunun yerine, birçok özelliğe sahiptir. Yeni sınıflar için kod şöyle görünür:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

İçinde `Movie` denetleyici ekleyin aşağıdaki `PersonDetail` eylemi bir kişi örneği görüntülemek için:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Ardından aşağıdaki kodu ekleyin `Movie` doldurmak için denetleyici `Person` model bazı örnek verilerle:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Açık *Views\Movies\PersonDetail.cshtml* dosya ve eklemek için aşağıdaki biçimlendirme `PersonDetail` görünümü.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Uygulamayı çalıştırmak ve gitmek için CTRL + F5 tuşlarına basın *filmler/PersonDetail*.

`PersonDetail` Görünüm içermiyor `Address` karmaşık tür olarak bu ekran görüntüsünde görebilirsiniz. (Adres gösterilen.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Karmaşık bir tür olduğundan model verileri görüntülenmez. Adres bilgileri görüntülemek için Aç *Views\Movies\PersonDetail.cshtml* yeniden dosyasını açıp aşağıdaki işaretlemeyi ekleyin.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

İçin tam biçimlendirme `PersonDetail` artık görünümü şu şekildedir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Uygulamayı yeniden çalıştırın ve görüntüleme `PersonDetail` görünümü. Adres bilgileri artık görüntülenir:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Karmaşık tür için bir şablon oluşturma

Bu bölümde, işlemek için kullanılan bir şablon oluşturursunuz `Address` karmaşık tür. Bir şablon oluştururken `Address` türü, ASP.NET MVC otomatik olarak kullanabilir, uygulama başka bir yerindeki bir adresi modeli biçimlendirmek için. Bu, işlenmesi denetlemek için bir yol sağlar `Address` uygulamada tek yerden türü.

İçinde *views\shared\displaytemplates konumunda* klasöründe adlı kesin türü belirtilmiş bir kısmi görünüm oluşturma **adresi**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Tıklayın **Ekle**ve ardından yeni açın *Views\Shared\DisplayTemplates\Address.cshtml* dosya. Yeni Görünüm aşağıdaki oluşturulan biçimlendirme içeriyor:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Uygulamayı çalıştırmak ve görüntüleme `PersonDetail` görünümü. Bu kez, `Address` yeni oluşturduğunuz şablonu görüntülemek için kullanılan `Address` karmaşık tür ekran aşağıdaki gibi görünür:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Özet: yolları modeli görüntüleme biçimi ve şablonu belirtin

Biçimi veya şablon için bir model özelliğine aşağıdaki yaklaşımlardan kullanarak belirtebilirsiniz, öğrendiniz:

- Uygulama `DisplayFormat` modelinde bir özellik için özniteliği. Örneğin, aşağıdaki kod, tarih saat görüntülenecek neden olur:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Uygulama bir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) modeli ve veri türünü belirten bir özelliği özniteliği. Örneğin, aşağıdaki kod, tarih saat görüntülenecek neden olur.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Uygulama içeriyorsa, bir *date.cshtml* şablonunda *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasör, bu şablonu işlemek için kullanılan `DateTime` özelliği. Aksi takdirde yerleşik ASP.NET şablon oluşturma sistem özelliği olarak bir tarih görüntüler.
- Bir ekran şablonu oluşturma *views\shared\displaytemplates konumunda* klasör veya *Views\Movies\DisplayTemplates* klasör adıyla eşleşen biçimlendirmek istediğiniz veri türü. Gördüğünüz gibi *Views\Shared\DisplayTemplates\DateTime.cshtml* işlemek için kullanılan `DateTime` modeline bir öznitelik ekleyerek ve tüm biçimlendirme görünümlere ekleme olmadan bir model özellikleri.
- Kullanarak [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) modelin model özelliği görüntülemek için şablon belirtmek için özniteliği.
- Açıkça görünen şablon adının ekleme [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) Görünümü'nde arayın.

Kullandığınız yaklaşım, uygulamanızda yapmanız gerekenler üzerinde bağlıdır. İhtiyacınız olan biçimlendirme tam olarak türünü almak için bu yaklaşımları karıştırmak durumdur.

Sonraki bölümde, geçiş biraz gears ve verilerin nasıl girildiğini özelleştirme için nasıl görüntüleneceğini özelleştirmesini taşıyın. JQuery datepicker tarihlerini belirtmek için bahsettiniz bir yol sağlamak üzere uygulamayı düzenleme görünümle denetime.

> [!div class="step-by-step"]
> [Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [İleri](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
