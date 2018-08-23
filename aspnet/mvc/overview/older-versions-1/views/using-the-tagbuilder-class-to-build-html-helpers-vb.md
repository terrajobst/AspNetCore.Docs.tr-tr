---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: HTML Yardımcıları (VB) oluşturmak için TagBuilder sınıfı kullanarak | Microsoft Docs
author: StephenWalther
description: Stephen Walther TagBuilder sınıfı adlı ASP.NET MVC çerçevesi bir kullanışlı yardımcı sınıfta tanıtır. İçin TagBuilder sınıfı bir kolayca kullanabileceğiniz...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 783c5f73709de37f79c472e10c79e284cf25f8fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751979"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>HTML Yardımcıları (VB) oluşturmak için TagBuilder sınıfı kullanma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther TagBuilder sınıfı adlı ASP.NET MVC çerçevesi bir kullanışlı yardımcı sınıfta tanıtır. HTML etiketlerini kolayca oluşturmak için TagBuilder sınıfı kullanabilirsiniz.


ASP.NET MVC çerçevesi, HTML Yardımcıları oluşturma sırasında kullanabileceğiniz TagBuilder sınıfı adlı yararlı yardımcı program sınıfı içerir. Sınıf adından da anlaşılacağı gibi TagBuilder sınıfı, HTML etiketleri kolayca oluşturmanıza olanak sağlar. Bu kısa öğreticide TagBuilder sınıfı bir bakış sağlanır ve bu sınıf, basit bir HTML Yardımcısı oluşturma HTML oluşturulduğunda kullanmayı öğrenin &lt;img&gt; etiketler.

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder sınıfı genel bakış

TagBuilder sınıfı System.Web.Mvc ad alanında yer alır. Bu beş yöntemi vardır:

- AddCssClass() – yeni bir eklemenizi sağlar *class = ""* özniteliği için bir etiket.
- GenerateId() – bir ID özniteliği için bir etiket eklemenize olanak sağlar. Bu yöntem otomatik olarak kimliği dönemlerde değiştirir (varsayılan olarak, alt çizgi ile nokta değiştirilir)
- MergeAttribute() – etiket öznitelikleri eklemek olanak tanır. Bu yöntemin birden fazla aşırı yüklemeleri vardır.
- SetInnerText() – etiketinin iç metni olanak tanır. HTML kodlama otomatik olarak iç metindir.
- ToString() – etiket işleme olanak tanır. Normal bir etiketi, bir başlangıç etiketi, bir bitiş etiketi veya kendi kendine kapanan etiketi oluşturmak isteyip istemediğinizi belirtebilirsiniz.
  

TagBuilder sınıfı dört önemli özelliklere sahiptir:

- Öznitelikleri – tüm etiket özniteliklerini temsil eder.
- IdAttributeDotReplacement – nokta (varsayılan değer altçizgidir) değiştirileceğini GenerateId() yöntemi tarafından kullanılan karakteri temsil eder.
- InnerHTML – etiketinin iç içeriğini temsil eder. Bu özellik için bir dize atama *yok* HTML ile kodlanan dize.
- TagName – etiketin adını temsil eder.

Bu yöntemler ve özellikler, tüm temel yöntem ve bir HTML etiketi oluşturmak için gereken özellikleri sağlar. Gerçekten TagBuilder sınıfı kullanmanız gerekmez. Bunun yerine bir StringBuilder sınıfını kullanabilirsiniz. Ancak, TagBuilder sınıfı hayatınızı biraz daha kolay hale getirir.

## <a name="creating-an-image-html-helper"></a>Bir görüntü HTML Yardımcısı oluşturma

TagBuilder sınıfı örneğini oluşturduğunuzda TagBuilder oluşturucuya derlemek istediğiniz etiketi adı geçirin. Ardından, etiket özniteliklerini değiştirmek için AddCssClass ve MergeAttribute() yöntemleri gibi yöntemleri çağırabilir. Son olarak, Etiket oluşturulacak ToString() yöntemini çağırın.

Örneğin, 1 listeleyen bir görüntü HTML Yardımcısı içerir. Görüntü yardımcı bir HTML temsil eden bir TagBuilder ile dahili olarak uygulanan &lt;img&gt; etiketi.

**1 – Helpers\ImageHelper.vb listeleme**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

1 listeleme modülünde Image() adlı iki aşırı yüklenmiş yöntemler içerir. Image() yöntemi çağırdığınızda, veya bir dizi HTML özniteliklerini temsil eden bir nesne geçirebilirsiniz.

TagBuilder.MergeAttribute() yöntemi için TagBuilder src özniteliğini gibi tek tek öznitelikler eklemek için nasıl kullanıldığını dikkat edin. Ayrıca, fark TagBuilder.MergeAttributes() yöntemi nasıl bir öznitelik koleksiyonu için TagBuilder eklemek için kullanılır. Bir sözlük MergeAttributes() yöntemi kabul&lt;dize, nesne&gt; parametresi. RouteValueDictionary sınıf özniteliklerinin koleksiyonunu bir sözlükte temsil eden nesneyi dönüştürmek için kullanılan&lt;dize, nesne&gt;.

Görüntü Yardımcısı oluşturduktan sonra herhangi bir diğer standart HTML yardımcıları gibi ASP.NET MVC görünümlerinizde yardımcıyı kullanabilirsiniz. Xbox aynı görüntüsü iki kez görüntülenecek görüntü Yardımcısı 2 liste görünümünde kullanır (bkz. Şekil 1). Image() Yardımcısı, hem ile hem de bir HTML öznitelik koleksiyonu olmadan çağrılır.

**2 – Home\Index.aspx listeleme**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![Yeni Proje iletişim kutusu](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Şekil 01**: yansıma Yardımcısını kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Bildirim Index.aspx görünümün üst görüntü Yardımcısı ilişkili ad alanı içeri aktarmanız gerekir. Yardımcı, aşağıdaki yönergesi ile aktarılır:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

Bir Visual Basic uygulamasında varsayılan ad alanı uygulamanın adı ile aynıdır.

> [!div class="step-by-step"]
> [Önceki](creating-custom-html-helpers-vb.md)
> [İleri](creating-page-layouts-with-view-master-pages-vb.md)
