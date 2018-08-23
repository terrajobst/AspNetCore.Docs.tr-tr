---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Özel HTML Yardımcıları (C#) oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir. HTML Yardımcısı yararlanarak...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a6a684e01b67c2ea139a50b568098d2dcf594272
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757016"
---
<a name="creating-custom-html-helpers-c"></a>Özel HTML Yardımcıları oluşturma (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir. HTML Yardımcıları avantajlarından yararlanarak, standart bir HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri tedious yazarak miktarını azaltabilirsiniz.


Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir. HTML Yardımcıları avantajlarından yararlanarak, standart bir HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri tedious yazarak miktarını azaltabilirsiniz.

Bu öğreticinin ilk bölümünde miyim mevcut HTML Yardımcıları ile ASP.NET MVC çerçevesi dahil bazılarını açıklar. Ardından, ben özel HTML Yardımcıları oluşturmak için iki yöntem açıklar: Ben statik bir yöntem oluşturarak ve bir genişletme yöntemi oluşturarak özel HTML Yardımcıları oluşturma işlemleri açıklanmaktadır.

## <a name="understanding-html-helpers"></a>HTML yardımcılarını anlama

Bir HTML Yardımcısı yalnızca bir dize döndüren bir yöntem var. Dize herhangi bir türde istediğiniz içerik temsil edebilir. Örneğin, HTML gibi standart HTML etiketlerini işlemek için bir HTML Yardımcıları kullanabilirsiniz `<input>` ve `<img>` etiketler. HTML Yardımcıları, sekme şeridi veya veritabanı verilerinin bir HTML tablosu gibi daha karmaşık içeriğini işlemek için de kullanabilirsiniz.

ASP.NET MVC çerçevesi aşağıdaki standart HTML Yardımcıları (Bu tam bir listesi değildir) içermektedir:

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

Örneğin, 1 listeleme biçiminde göz önünde bulundurun. Bu formu Yardımı iki standart HTML Yardımcıları (bkz. Şekil 1) ile işlenir. Bu form kullanır `Html.BeginForm()` ve `Html.TextBox()` basit bir HTML form işlemek için yardımcı yöntemler.


[![Sayfanın HTML Yardımcıları ile çizilir.](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Şekil 01**: sayfa İşlenmiş HTML Yardımcıları ile ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-html-helpers-cs/_static/image3.png))


**Kod 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() yardımcı yöntemi, açılış ve kapanış HTML oluşturmak için kullanılan `<form>` etiketler. Dikkat `Html.BeginForm()` kullanarak içinde yöntemi çağrıldığında deyimi. Using deyimi, sağlar, `<form>` etiketini kullanarak sonunda kapalı blok.

Bir kullanarak oluşturmak yerine tercih ederseniz, blok kapatmak için Html.EndForm() yardımcı yöntemini çağırabilirsiniz `<form>` etiketi. Oluşturma bir açılış ve kapanış yaklaşımı kullanmak `<form>` size en kolay görünen etiket.

`Html.TextBox()` Yardımcı yöntemler listeleme 1'de HTML oluşturmak için kullanılan `<input>` etiketler. Kaynağı Görüntüle tarayıcınıza seçerseniz, HTML kaynağını listeleme 2'de görürsünüz. Kaynak standart HTML etiketleri içerdiğine dikkat edin.

> [!IMPORTANT]
> dikkat `Html.TextBox()`-HTML Yardımcısı ile işlenir `<%= %>` yerine etiketleri `<% %>` etiketler. Ardından, eşittir işareti eklemezseniz, hiçbir şey tarayıcıda görüntülenen.

ASP.NET MVC çerçevesi Yardımcıları küçük bir kümesini içerir. Büyük olasılıkla özel HTML Yardımcıları ile MVC çerçevesi genişletmek gerekir. Bu öğreticinin geri kalanında içinde özel HTML Yardımcıları oluşturmak için iki yöntem öğrenin.

**Kod 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Statik yöntemleriyle HTML Yardımcıları oluşturma

Yeni bir HTML Yardımcısı oluşturmanın en kolay yolu, bir dize döndüren bir statik yöntem oluşturmaktır. Örneğin, bir HTML işleyen yeni bir HTML Yardımcısı oluşturmaya karar verdiğinizi düşünelim `<label>` etiketi. Sınıfı listeleme 2'de işlemek için kullanabileceğiniz bir `<label>` .

**Kod 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Özel sınıf 2 listeleme hakkında bir şey yoktur. `Label()` Yöntemi yalnızca bir dize döndürür.

Listeleme 3'te değiştirilmiş dizin görünümünün kullanan `LabelHelper` HTML oluşturmak için `<label>` etiketler. Görünüm içeren bildirimi bir `<%@ imports %>` aktarır yönergesi `Application1.Helpers` ad alanı.

**Kod 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Genişletme yöntemleri ile HTML Yardımcıları oluşturma

Çalışmaya HTML Yardımcıları oluşturmak istiyorsanız, genişletme yöntemleri oluşturmanız gerekir ASP.NET MVC çerçevesi dahil standart HTML yardımcıları gibi çalışır. Genişletme yöntemleri varolan bir sınıf için yeni yöntemler eklemenize imkan tanır. Bir HTML yardımcı yöntemi oluştururken, bir görünümün Html özelliği tarafından temsil edilen HtmlHelper sınıfı için yeni yöntemleri ekleyin.

Sınıf listesi 3'te bir genişletme yöntemi için ekler `HtmlHelper` adlı sınıfı `Label()`. Birkaç bu sınıfı hakkında fark etmişsinizdir şey vardır. İlk olarak, sınıfın statik sınıf olduğuna dikkat edin. Bir genişletme yöntemi statik sınıf ile tanımlamanız gerekir.

İkinci olarak, dikkat ilk parametresi `Label()` yöntemi anahtar sözcüğü öncesinde `this`. Genişletme yönteminin ilk parametresi genişletme yönteminin genişlettiği sınıfın gösterir.

**Kod 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Bir genişletme yöntemi oluşturma ve uygulamanızı başarıyla oluşturmak sonra Visual Studio IntelliSense gibi tüm diğer yöntemleri bir sınıfın içinde genişletme yöntemi görünür (bkz: Şekil 2). Tek fark, bu uzantı yöntemleri özel bir simge (aşağı ok simgesi) yanında görünür.


[![Html.Label() genişletme yöntemini kullanma](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Şekil 02**: Html.Label() genişletme yöntemi kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-html-helpers-cs/_static/image6.png))


Listeleme 4'te değiştirilmiş dizin görünümü, tüm işlemek için Html.Label() genişletme yöntemini kullanır. kendi `<label>` etiketler.

**4 listeleme – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Özet

Bu öğreticide, özel HTML Yardımcıları oluşturmak için iki yöntem öğrendiniz. İlk olarak, özel bir oluşturma işleminin nasıl yapılacağını öğrendiniz `Label()` HTML Yardımcısını, statik bir yöntem oluşturarak, bir dize döndürür. Ardından, özel bir oluşturma işleminin nasıl yapılacağını öğrendiniz `Label()` HTML yardımcı yöntem üzerinde bir genişletme yöntemi oluşturarak `HtmlHelper` sınıfı.

Bu öğreticide, son derece basit bir HTML yardımcı yöntem geliştirmeye odaklanır. Bir HTML Yardımcısı istediğiniz kadar karmaşık olabileceğini unutmayın. Ağaç görünümleri, menüler ya da veritabanı verilerinin tablolar gibi zengin içerik oluşturan bir HTML Yardımcıları oluşturabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](asp-net-mvc-views-overview-cs.md)
> [İleri](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
