---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: HTML düzenleyicisi denetim nasıl kullanabilirim? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor bir ASP.NET AJAX kolayca oluşturmak ve bir araç çubuğu düğmeleri aracılığıyla HTML içeriğini düzenlemek izin veren denetimdir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fca18948c0e4f1323f214dc0033f19fa44efad47
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-use-the-html-editor-control-c"></a>HTML düzenleyicisi denetim nasıl kullanabilirim? (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

> HTMLEditor bir ASP.NET AJAX kolayca oluşturmak ve bir araç çubuğu düğmeleri aracılığıyla HTML içeriğini düzenlemek izin veren denetimdir.


Bu öğreticinin amacı, AJAX Denetim Araç Seti HTML düzenleyicisi denetimine genel bakış sağlamaktır. Bağlantılar, görüntüler, metin hizalamasını değiştirme ekleme ekleme yazı tipi boyutunu değiştirme, bir yazı tipi seçme, arka plan rengini değiştirmek, ön plan rengini değiştirmek için seçenekler HTML Düzenleyicisi'ni içerir ve gerçekleştirme kesme, kopyalama ve yapıştırma işlemleri (bkz: Şekil 1).


[![HTML düzenleyicisi](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Şekil 01**: HTML Düzenleyicisi'ni ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


HTML Düzenleyicisi'ni bir tasarım modunda kullanarak içerik girmenizi sağlar veya HTML doğrudan girebilirsiniz. HTML içeriğinizi Önizleme seçeneğiniz de sağlanır (bkz: Şekil 2).


[![Tasarım, HTML ve önizleme düğmeleri](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Şekil 02**: tasarım, HTML ve önizleme düğmelerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


Bu öğreticide, HTML Düzenleyicisi'ni görüntülemek nasıl, HTML Düzenleyicisi'nde görüntülenen araç çubuğu düğmelerini özelleştirmeyi ve siteler arası komut dosyası saldırıları önlemek nasıl öğrenin.

## <a name="displaying-the-html-editor"></a>HTML Düzenleyicisi'ni görüntüleme

Bir ASP.NET sayfası HTML Düzenleyicisi'ni kullanmadan önce ilk sayfasına bir ScriptManager denetimi eklemeniz gerekir. ScriptManager denetimi, Visual Studio/Visual Web Developer Express araç kutusunda AJAX uzantılar sekmesi altında bulunur.

Sayfanın üst kısmındaki sayfasındaki diğer denetimlere önce ScriptManager denetimi yerleştirmeniz gerekir. Örneğin, bunu hemen açılış sunucu-tarafı yerleştireceğiniz &lt;form&gt; etiketi.

HTML düzenleyicisi Denetim araç AJAX Denetim Araç Seti denetimleri geri kalanı ile birlikte bulunur. Düzenleyici denetimi adlı (bkz: Şekil 3).


[![HTML düzenleyicisi denetimi](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Şekil 03**: HTML düzenleyicisi denetimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


HTML Düzenleyicisi'ni bir sayfaya sürüklediğinizde, özellik sayfasında özelliklerini ayarlayabilirsiniz. Örneğin, normalde genişlik ve yükseklik özelliklerini ayarlamak istediğiniz. 1 listeleyen bir HTML düzenleyicisi içeren bir ASP.NET sayfası kaynağı içerir.

**1 - SimpleEditor.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

Listeleme 1 sayfasında bir HTML düzenleyicisi denetimi, bir düğme denetimi ve bir harf denetimi içerir. Düğmeye tıkladığınızda, HTML Düzenleyicisi'nin içeriği değişmez değer denetiminde görünür (Şekil 4'e bakın).


[![Bir HTML düzenleyicisi ile form gönderme](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Şekil 04**: bir HTML düzenleyicisi ile form gönderme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


HTML düzenleyicisi içerik özellik HTML düzenleyiciye girilen HTML içeriği almak için kullanılır. Bu HTML içeriğini JavaScript içerebileceğini unutmayın. Sonraki bölümde, nasıl JavaScript ekleme saldırıları engelleyebilirsiniz tartışın.

## <a name="customizing-the-html-editor-toolbar"></a>HTML Düzenleyicisi araç çubuğunu özelleştirme

Tam olarak hangi düğmeleri özelleştirebilirsiniz Düzenleyicisi'nde görüntülenir. Örneğin, kullanıcıların HTML düzenleyicisi HTML moduna geçiş yapmasını önlemek için HTML sekmesi kaldırmak isteyebilirsiniz. Veya, kullanıcıların bir forumda aşırı büyük boyutlu metin oluşturma önlemek için yazı tipi boyutu açılır listeden kaldırmak isteyebilirsiniz (bkz. Şekil 5) posta iletisi.


[![Özelleştirilmiş bir HTML düzenleyicisi](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Şekil 05**: A özelleştirilmiş HTML Düzenleyicisi'ni ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Araç çubuğu düğmelerini temel Düzenleyicisi sınıfından yeni bir HTML düzenleyicisi türetme göre özelleştirin. Örneğin, özel Düzenleyicisi'nde listeleniyor 2 yalnızca kalın ve italik araç çubuğu düğmelerini içerir. Diğer tüm araç çubuğu düğmeleri kaldırılmıştır. Ayrıca, HTML sekmesi düzenleyicisinin alt kısmından kaldırılan (ancak tasarım ve önizleme sekmeleri hala vardır).

**2 - uygulama listeleme\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Sınıfı, uygulamanızın listeleme 2'de eklemelisiniz\_böylece sınıfı otomatik olarak derlenmiş kod klasör. Uygulama\_kod klasör, Web sitenizdeki yok sonra yalnızca klasörü ekleyebilirsiniz.

Özel bir düzenleyici oluşturduktan sonra normal HTML düzenleyicisi (3 listeleme bakın) ekledikçe, onu bir ASP.NET sayfası aynı şekilde ekleyebilirsiniz.

**Listing 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Kaçınarak siteler arası komut dosyası (XSS) saldırıları

Bir kullanıcıdan giriş kabul edin ve bu giriş, Web sitenizde yeniden görüntüleyin olduğunda, siteler arası komut dosyası (XSS) saldırılarını sitenize potansiyel olarak açın. Teorik olarak, kötü amaçlı bir korsanın giriş görüntülendiğinde, yürütülen JavaScript kodu gönderme. JavaScript kullanıcı parolaları ve diğer hassas bilgiler çalmak için kullanılabilir.

Normalde, bir web sayfasında görüntülenmeden önce bir kullanıcıdan Al ne olursa olsun giriş kodlama HTML tarafından XSS saldırılarını boşa çıkarabilir. Ancak, HTML Düzenleyicisi'nin çıkış kodlama HTML değil yalnızca kodlamak &lt;komut dosyası&gt; etiketleri, aynı zamanda tüm HTML etiketleri kodlamak. Diğer bir deyişle, tüm yazı tipi, yazı tipi boyutunu ve arka plan rengi gibi biçimlendirme kaybeder.

Ardından, kullanıcılarınızın--parolaları, kredi kartı numaraları ve sosyal güvenlik numarası - gibi hassas bilgileri topluyorsanız, bir kullanıcı ile HTML düzenleyicisi almak beklemediğiniz kodlanmış içeriği görüntülememek. Sitenizin tarafından güvenilen bir taraf HTML içeriğini yeniden görüntüleme değil veya HTML içeriğini gönderilen yalnızca durumlarda HTML Düzenleyicisi'ni kullanmanız gerekir.

Örneğin, bir blog uygulaması oluşturduğunuzu düşünün. Bu durumda, blog gönderileri oluştururken HTML Düzenleyicisi'ni kullanmak için mantıklıdır. Bir blog gönderisini gönderen tek olan ve kendinize kötü amaçlı JavaScript gönderilmemesini güven muhtemelen. Ancak, yorum göndermek anonim kullanıcılar izin verirken HTML Düzenleyicisi'ni kullanmak için anlamlı yapmaz. Kullanıcıların parolalar gibi hassas bilgileri gönderme gibi durumlarda özellikle dikkatli olmanız gerekir. Potansiyel olarak, kötü niyetli bir kullanıcının parola çalmak için doğru JavaScript içeren bir açıklama sonrasında.

## <a name="summary"></a>Özet

Bu öğreticide, AJAX Denetim araç setindeki içerdiği HTML düzenleyicisi denetimi kısa bir genel bakış sağlanmıştır. Bir kullanıcıdan zengin içerik kabul etmek ve içeriği sunucuya göndermek için HTML Düzenleyicisi'ni kullanmak öğrendiniz. HTML düzenleyicisi tarafından görüntülenen araç çubuğu düğmelerini nasıl özelleştirebileceğiniz de açıklanmıştır. Son olarak, kötü amaçlı olabilecek giriş kabul edecek şekilde HTML Düzenleyicisi'ni kullanırken, siteler arası komut dosyası saldırıları önlemek nasıl öğrendiniz.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
