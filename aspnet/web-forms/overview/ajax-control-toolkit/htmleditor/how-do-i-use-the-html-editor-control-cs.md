---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: HTML düzenleyicisi denetimi nasıl kullanabilirim? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor bir ASP.NET AJAX kolayca oluşturun ve bir araç çubuğu düğmeleri üzerinden HTML içerik Düzenle olanak tanıyan denetimidir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7bd7c5a604c897ac6dce92123e9e7ae4157d3e0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755183"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>HTML düzenleyicisi denetimi nasıl kullanabilirim? (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

> HTMLEditor bir ASP.NET AJAX kolayca oluşturun ve bir araç çubuğu düğmeleri üzerinden HTML içerik Düzenle olanak tanıyan denetimidir.


Bu öğreticinin amacı, dahil edilen AJAX Denetim Araç Seti ile HTML düzenleyicisi denetimi genel bir bakış ile sağlamaktır. Bağlantılar, görüntüler, metin hizalamasını değiştirme ekleme ekleme yazı tipi boyutunu değiştirme, bir yazı tipi seçerek, arka plan rengini değiştirme, ön plan rengini değiştirmek için seçenekler HTML düzenleyicisi içerir ve kesme gerçekleştiriyorsa, kopyalama ve yapıştırma işlemleri (bkz. Şekil 1).


[![HTML düzenleyicisi](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Şekil 01**: HTML düzenleyicisi ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


HTML düzenleyicisi tasarım modu kullanarak içerik girmenizi sağlar veya HTML doğrudan girebilirsiniz. HTML içerik Önizleme seçeneği de sağlanır (bkz: Şekil 2).


[![Tasarım, HTML ve önizleme düğmeleri](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Şekil 02**: tasarım, HTML ve önizleme düğmeler ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


Bu öğreticide, HTML Düzenleyicisi'ni görüntüleme, HTML Düzenleyicisi'nde görünen araç çubuğu düğmeleri özelleştirme ve siteler arası betik saldırıları önlemek nasıl öğrenin.

## <a name="displaying-the-html-editor"></a>HTML düzenleyicisi görüntülenirken

Bir ASP.NET sayfasında HTML Düzenleyicisi'ni kullanmadan önce öncelikle bir ScriptManager denetimi sayfasına eklemeniz gerekir. ScriptManager denetimini, Visual Studio/Visual Web Developer Express araç kutusunda AJAX uzantılar sekmesi altında bulunur.

Sayfadaki diğer denetimleri önce sayfanın üst kısmındaki ScriptManager denetimini yerleştirmeniz gerekir. Örneğin, bunu hemen açılış sunucu-tarafı yerleştirebilirsiniz &lt;form&gt; etiketi.

HTML düzenleyicisi denetimi araç AJAX Denetim Araç Seti denetimlerini geri kalanı ile birlikte bulunur. Düzenleyici denetimi olarak adlandırılır (bkz: Şekil 3).


[![HTML düzenleyicisi denetimi](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Şekil 03**: HTML düzenleyicisi denetimi ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


HTML düzenleyicisi bir sayfaya sürükleyin sonra özellik sayfasında özelliklerini ayarlayabilirsiniz. Örneğin, normal genişlik ve yükseklik özelliklerini ayarlamak istiyorsunuz. 1 listeleyen bir HTML düzenleyici içeren bir ASP.NET sayfası için kaynak içerir.

**1 - SimpleEditor.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

1 listesi sayfasında bir HTML düzenleyicisi denetimi, bir düğme denetimi ve bir değişmez değer denetimi içerir. Düğmeye tıkladığınızda, HTML Düzenleyicisi'nin içeriği değişmez değer denetiminde görünür (bkz: Şekil 4).


[![Bir HTML düzenleyicisi ile form gönderme](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Şekil 04**: bir HTML düzenleyicisi ile form gönderme ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


HTML düzenleyici içeriği özelliği HTML düzenleyicide yerleşik girilen HTML içeriği almak için kullanılır. Bu HTML içeriğini JavaScript içerebileceğini unutmayın. Sonraki bölümde, JavaScript ekleme saldırılarını nasıl engelleyebilirsiniz ele alır.

## <a name="customizing-the-html-editor-toolbar"></a>HTML Düzenleyicisi araç çubuğunu özelleştirme

Tam olarak hangi düğmeleri özelleştirebilirsiniz Düzenleyicisi'nde görünür. Örneğin, kullanıcıların HTML düzenleyicisi HTML moduna geçiş yapmasını önlemek için HTML sekmesi kaldırmak isteyebilirsiniz. Veya kullanıcılar bir forumda aşırı büyük metin oluşturmasını önlemek için yazı tipi boyutu açılır listeden kaldırmak isteyebilirsiniz (bkz: Şekil 5) posta iletisi.


[![Özelleştirilmiş bir HTML düzenleyicisi](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Şekil 05**: bir özelleştirilmiş HTML düzenleyicisi ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Araç çubuğu düğmeleri, temel Düzenleyici sınıfından yeni bir HTML düzenleyici türetilerek özelleştirin. Örneğin, özel Düzenleyicisi'nde listeleme 2 yalnızca kalın ve italik araç çubuğu düğmeleri içerir. Diğer araç çubuğu düğmeleri kaldırıldı. Ayrıca, HTML sekmesi düzenleyicisinin alt kaldırıldı (ancak tasarım ve önizleme sekmelerini hala vardır).

**2 - uygulama listeleme\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Uygulamanıza listeleme 2'de sınıf eklemelisiniz\_böylece sınıf otomatik olarak derlenmiş kod klasörü. Uygulama\_kod klasörü siteniz yok sonra klasör eklemeniz yeterlidir.

Özel bir düzenleyici oluşturduktan sonra normal HTML düzenleyicisi (3 listeleme bakın) ekledikçe, bunu bir ASP.NET sayfasına yolla ekleyebilirsiniz.

**3 - ShowCustomEditor.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Kaçınarak siteler arası betik (XSS) saldırıları

Bir kullanıcıdan giriş kabul edin ve sitenizde, giriş yeniden her olası siteler arası betik (XSS) saldırılarını Web sitenize açın. Teorik olarak, kötü amaçlı bir korsana giriş görüntülendiğinde yürütülen JavaScript kodu göndermek. JavaScript, kullanıcı parolalarını veya diğer hassas bilgileri çalan için kullanılabilir.

Normalde, HTML bir web sayfasında görüntülemeden önce kullanıcıdan almak istediğiniz giriş kodlama XSS saldırılarını yemektir. Ancak HTML çıktısını HTML Düzenleyicisi'nin kodlama değil yalnızca kodlamak &lt;betik&gt; etiketleri, ayrıca tüm HTML etiketleri kodlama. Diğer bir deyişle, tüm yazı tipini, yazı tipi boyutu ve arka plan rengi gibi biçimlendirme kaybeder.

--Kullanıcılarınızın parolaları, kredi kartı numaraları ve sosyal güvenlik numaraları - gibi hassas bilgileri topluyorsanız, ardından, bir kullanıcı ile HTML düzenleyicisi almak beklemediğiniz kodlanmış içeriği görüntülemelidir değil. HTML düzenleyicisi tarafından güvenilen bir taraf Web sitenize HTML içeriği yeniden görüntüleme değil veya HTML içeriğini gönderilen yalnızca durumlarda kullanmanız gerekir.

Örneğin, bir blog uygulaması oluşturma düşünün. Bu durumda, HTML düzenleyicisi blog gönderilerini oluştururken kullanılacak mantıklıdır. Bir blog gönderisi gönderen tek olduğunuz ve kendinize kötü amaçlı JavaScript gönderilmemesini muhtemelen, güvenebileceğiniz. Ancak, anonim kullanıcıların yorumlarınızı izin verirken HTML Düzenleyicisi'ni kullanmak için mantıklı değildir. Hangi kullanıcıların parolalar gibi hassas bilgiler göndermek durumlarda özellikle dikkatli olmanız gerekir. Potansiyel olarak, kötü niyetli bir kullanıcı için bir parola hırsızlığı doğru JavaScript içeren yorum göndermek.

## <a name="summary"></a>Özet

Bu öğreticide, AJAX Denetim Araç Seti dahil HTML düzenleyicisi denetimi kısa bir genel bakış sağlanmıştır. Bir kullanıcı Zengin içerikten kabul etmek ve içerik sunucusuna göndermek için HTML Düzenleyicisi'ni kullanmayı öğrendiniz. HTML düzenleyicisi tarafından görüntülenen araç çubuğu düğmeleri nasıl özelleştirebileceğiniz da bahsedilmiştir. Son olarak, kötü amaçlı olabilecek giriş kabul etmek için HTML düzenleyici kullanırken, siteler arası betik saldırıları önlemek yapma hakkında bilgi edindiniz.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
