---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: HTML Yardımcıları (C#) oluşturmak için TagBuilder sınıfını kullanma | Microsoft Docs
author: StephenWalther
description: Stephen Walther yararlı yardımcı program sınıfı TagBuilder sınıfı adlı ASP.NET MVC çerçevesi arasında tanıtır. TagBuilder sınıfına kolayca kullanabilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>HTML Yardımcıları (C#) oluşturmak için TagBuilder sınıfını kullanma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther yararlı yardımcı program sınıfı TagBuilder sınıfı adlı ASP.NET MVC çerçevesi arasında tanıtır. TagBuilder sınıfı kolayca HTML etiketleri oluşturmak için kullanabilirsiniz.


ASP.NET MVC çerçevesi HTML Yardımcıları oluştururken kullanabileceğiniz TagBuilder sınıfı adlı bir yararlı yardımcı program sınıfı içerir. Sınıfın adını da anlaşılacağı gibi TagBuilder sınıfı, HTML etiketleri kolayca oluşturmanıza olanak sağlar. Kısa Bu öğreticide, TagBuilder sınıfı bir bakış sağlanır ve basit bir HTML Yardımcısı oluşturmaya HTML işleyen olduğunda bu sınıfının nasıl kullanılacağını öğrenin &lt;img&gt; etiketler.

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder sınıfı genel bakış

TagBuilder sınıfı System.Web.Mvc ad alanında yer alır. Beş yöntemi vardır:

- AddCssClass() - yeni bir eklemenize olanak sağlayan *class = ""* özniteliği için bir etiket.
- GenerateId() - ID özniteliği için bir etiket eklemenize olanak sağlar. Bu yöntem otomatik olarak Kimliği'nde noktaları değiştiren (varsayılan olarak, alt çizgi tarafından nokta değiştirilir)
- MergeAttribute() - bir etiket için öznitelikler eklemenizi sağlar. Bu yöntemin birden çok aşırı vardır.
- SetInnerText() - etiketinin iç metni ayarlamanıza olanak sağlar. HTML kodlama otomatik olarak iç metindir.
- ToString() - etiket oluşturmak etkinleştirir. Normal bir etiketi, bir başlangıç etiketi, bir bitiş etiketi veya kendi kendine kapanan etiketi oluşturmak isteyip istemediğinizi belirtebilirsiniz.
  

TagBuilder sınıfı dört önemli özelliklere sahiptir:

- Öznitelikleri - tüm etiket özniteliklerini temsil eder.
- IdAttributeDotReplacement - GenerateId() yöntemi tarafından (varsayılan değer altçizgidir) nokta değiştirmek için kullanılan karakteri temsil eder.
- InnerHtml - etiketinin iç içeriğini temsil eder. Bu özellik bir dize atama *yok* HTML kodlama dizesi.
- TagName - etiketin adını temsil eder.

Bu yöntemlere ve özelliklere tüm temel yöntemleri ve bir HTML etiketi oluşturmak için gereken özelliklerini verin. Gerçekten TagBuilder sınıfı kullanmanız gerekmez. Bunun yerine bir StringBuilder sınıfını kullanabilirsiniz. Ancak, TagBuilder sınıfı yaşamınızı biraz daha kolay hale getirir.

## <a name="creating-an-image-html-helper"></a>Bir görüntü HTML Yardımcısı oluşturma

TagBuilder sınıfının bir örneğini oluştururken TagBuilder oluşturucuya oluşturmak istediğiniz etiketin adını geçirin. Ardından, etiket özniteliklerini değiştirmek için AddCssClass ve MergeAttribute() yöntemleri gibi yöntemleri çağırabilir. Son olarak, etiket oluşturmak için ToString() yöntemini çağırın.

Örneğin, 1 listeleyen bir görüntü HTML Yardımcısı içerir. Image helper dahili bir HTML temsil eden bir TagBuilder ile uygulanan &lt;img&gt; etiketi.

**1 - Helpers\ImageHelper.cs listeleme**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Listeleme 1 sınıfında görüntü adlı iki statik aşırı yüklenmiş yöntemler içerir. Image() yöntemini çağırdığınızda, veya bir dizi HTML özniteliklerini temsil eden bir nesne geçirebilirsiniz.

TagBuilder.MergeAttribute() yöntemi TagBuilder src özniteliğini gibi tek tek öznitelikler eklemek için nasıl kullanıldığını dikkat edin. Ayrıca, fark nasıl TagBuilder.MergeAttributes() yöntemi TagBuilder özniteliklerin bir koleksiyonu eklemek için kullanılır. Bir sözlük MergeAttributes() yöntemi kabul&lt;dize, nesne&gt; parametresi. RouteValueDictionary sınıf öznitelikleri koleksiyon bir sözlük olarak temsil eden nesneyi dönüştürmek için kullanılan&lt;dize, nesne&gt;.

Image helper oluşturduktan sonra ASP.NET MVC görünümlerinizde gibi diğer standart HTML Yardımcıları hiçbirini yardımcıyı kullanabilirsiniz. Xbox aynı görüntüsü iki kez görüntülemek için Image helper listeleme 2 görünümünde kullanır (bkz: Şekil 1). Image() Yardımcısı, hem ile hem de bir HTML öznitelik koleksiyonu olmadan adı verilir.

**Listing 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![Yeni Proje iletişim kutusu](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Şekil 01**: yansıma Yardımcısını kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Bildirim Index.aspx görünüm üstündeki Image helper ilişkili ad alanı içeri aktarmanız gerekir. Yardımcı aşağıdaki yönergesi alınır:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Önceki](creating-custom-html-helpers-cs.md)
> [sonraki](creating-page-layouts-with-view-master-pages-cs.md)
