---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: "Özel HTML Yardımcıları (C#) oluşturma | Microsoft Docs"
author: microsoft
description: "Bu öğreticinin amacı, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturabileceğinizi göstermektir. HTML Yardımcısı yararlanarak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b6d67eb7aab51ba2b422fab0788e34255f2c8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-c"></a>Oluşturma özel HTML Yardımcıları (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Bu öğreticinin amacı, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturabileceğinizi göstermektir. HTML Yardımcıları yararlanarak, can sıkıcı bir standart HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri yazarak miktarını azaltabilir.


Bu öğreticinin amacı, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturabileceğinizi göstermektir. HTML Yardımcıları yararlanarak, can sıkıcı bir standart HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri yazarak miktarını azaltabilir.

Bu öğreticinin ilk bölümü ı ASP.NET MVC çerçevesiyle dahil olan HTML Yardımcıları bazıları açıklanmaktadır. Ardından, t özel HTML Yardımcıları oluşturmak için iki yöntem açıklanmaktadır: özel HTML Yardımcıları bir statik yöntem oluşturarak ve bir genişletme yöntemi oluşturarak nasıl oluşturulacağını açıklar.

## <a name="understanding-html-helpers"></a>HTML Yardımcıları anlama

Bir HTML Yardımcısı dizesi döndüren yalnızca bir yöntemdir. Dize istediğiniz içerik herhangi bir türde temsil edebilir. Örneğin, HTML gibi standart HTML etiketlerini işlemek için bir HTML Yardımcıları kullanabilirsiniz `<input>` ve `<img>` etiketler. HTML Yardımcıları sekmesini Şerit veya veritabanı verilerinin bir HTML tablosu gibi daha karmaşık içerik işlemek için de kullanabilirsiniz.

ASP.NET MVC çerçevesi aşağıdaki standart HTML Yardımcıları (Bu tam bir listesi değildir) kümesini içerir:

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Örneğin, 1 listeleme formunda göz önünde bulundurun. Bu form Yardımı iki standart HTML Yardımcıları (bkz. Şekil 1 ') ile işlenir. Bu form kullanır `Html.BeginForm()` ve `Html.TextBox()` basit bir HTML form işlemek için yardımcı yöntemler.


[![Sayfanın HTML Yardımcıları ile çizilir](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Şekil 01**: sayfa işlenen HTML Yardımcıları ile ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-custom-html-helpers-cs/_static/image3.png))


**Kod 1 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Açma ve kapatma HTML oluşturmak için kullanılan Html.BeginForm() yardımcı yöntem `<form>` etiketler. Dikkat `Html.BeginForm()` yöntemi kullanarak bir içinde çağrılır deyimi. Sağlar deyimiyle `<form>` etiketi kullanarak sonunda kapalı bloğu.

Kullanarak bir oluşturmak yerine tercih ederseniz bloğu kapatmak için Html.EndForm() yardımcı yöntemini çağırabilir `<form>` etiketi. Bir açma oluşturma ve kapatma için yaklaşımı kullanmak `<form>` size en sezgisel görünen etiket.

`Html.TextBox()` Yardımcı yöntemler listeleme 1'de HTML oluşturmak için kullanılan `<input>` etiketler. Tarayıcınızda kaynağı görüntüle seçeneğini belirlerseniz, listeleme 2 HTML kaynağında görürsünüz. Kaynak standart HTML etiketleri içerdiğine dikkat edin.

> [!IMPORTANT]
> dikkat `Html.TextBox()`-HTML Yardımcısı ile işlenir `<%= %>` yerine etiketler `<% %>` etiketler. Sonra eşittir işareti eklemezseniz, hiçbir şey tarayıcıda görüntülenen.

ASP.NET MVC çerçevesi Yardımcıları, küçük bir kümesini içerir. Büyük olasılıkla, MVC çerçevesi özel HTML Yardımcıları ile genişletmek gerekir. Bu öğreticinin geri kalanında içinde özel HTML Yardımcıları oluşturma iki yöntemleri öğrenin.

**Kod 2 –`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>HTML Yardımcıları statik yöntemleriyle oluşturma

Yeni bir HTML Yardımcısı oluşturmanın en kolay yolu, bir dize döndürür bir statik yöntem oluşturmaktır. Örneğin, bir HTML işleyen yeni bir HTML Yardımcısı oluşturmaya karar düşünün `<label>` etiketi. Sınıfı listeleme 2'de oluşturmak için kullanabileceğiniz bir `<label>` .

**Kod 2 –`Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

2. listeleme sınıfı hakkında özel bir şey yoktur. `Label()` Yöntemi yalnızca bir dize döndürür.

Listeleme 3 değiştirilmiş dizin görünümünde kullanan `LabelHelper` HTML oluşturmak için `<label>` etiketler. Görünüm içerir bildirimi bir `<%@ imports %>` alır yönergesi `Application1.Helpers` ad alanı.

**Kod 2 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>HTML Yardımcıları genişletme yöntemleri ile oluşturma

Oluşturmak istiyorsanız çalışmaya HTML Yardımcıları genişletme yöntemleri oluşturmanıza gerek sonra ASP.NET MVC çerçevesi dahil standart HTML Yardımcıları ister. Genişletme yöntemleri, varolan bir sınıfa yeni yöntemler eklemenize olanak tanır. Bir HTML yardımcı yöntem oluştururken, bir görünümün Html özelliği tarafından temsil edilen HtmlHelper sınıfı için yeni yöntemler ekleyin.

Bir genişletme yöntemi listeleme 3'te sınıfı ekler `HtmlHelper` adlı sınıf `Label()`. Bu sınıfı hakkında dikkat etmelidir şunları vardır. İlk olarak, sınıfın statik sınıf olduğuna dikkat edin. Statik sınıf uzantısı yöntemiyle tanımlamanız gerekir.

İkinci olarak, dikkat ilk parametresi `Label()` yöntemi anahtar sözcüğüyle öncesinde `this`. Bir genişletme yöntemi ilk parametresi uzantısı yöntemin genişlettiği sınıfı gösterir.

**Kod 3 –`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Bir genişletme yöntemi oluşturun ve başarılı bir şekilde uygulamanızı sonra genişletme yöntemi Visual Studio IntelliSense tüm bir sınıfın diğer yöntemleri gibi görünür. (bkz: Şekil 2). Tek fark, bunları (aşağı ok simgesi) yanındaki özel simgesiyle yöntemleri görünür bu uzantısıdır.


[![Html.Label() genişletme yöntemi kullanma](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Şekil 02**: Html.Label() genişletme yöntemi kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-custom-html-helpers-cs/_static/image6.png))


Listeleme 4 değiştirilmiş dizin görünümünde tüm işlemek için Html.Label() genişletme yöntemi kullanan kendi `<label>` etiketler.

**4 listeleme –`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Özet

Bu öğreticide, özel HTML Yardımcıları oluşturmak için iki yöntem öğrendiniz. Özel bir oluşturmak öğrendiniz ilk olarak, `Label()` bir statik yöntem oluşturarak HTML Yardımcısı bir dize döndürür. Ardından, özel bir oluşturma öğrenilen `Label()` üzerinde bir genişletme yöntemi oluşturarak HTML yardımcı yöntem `HtmlHelper` sınıfı.

Bu öğreticide, oldukça basit bir HTML yardımcı yöntemi oluşturma üzerine odaklanır. Bir HTML Yardımcısı istediğiniz kadar karmaşık olabileceğini unutmayın. Ağaç görünümleri, menüler veya veritabanı veri tabloları gibi zengin içerik işlemek HTML Yardımcıları oluşturabilirsiniz.

>[!div class="step-by-step"]
[Önceki](asp-net-mvc-views-overview-cs.md)
[sonraki](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
