---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: AJAX dinamik güncelleştirmeler sunma | Microsoft Docs
author: microsoft
description: 10. adım uygular RSVP oturum açmış kullanıcıların ilgilendiklerini bir Şimdi Akşam Yemeği ayrıntı içinde tümleşik bir Ajax tabanlı yaklaşımı kullanarak, katılan destek...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: e902881d3dab6a902cb747a197a32f317d199723
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755827"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>AJAX dinamik güncelleştirmeler sunma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 10 ücretsiz / budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 10. adım uygular RSVP oturum açmış kullanıcıların ilgilendiklerini bir Şimdi Akşam Yemeği Ayrıntıları sayfasında tümleştirilmiş bir Ajax tabanlı yaklaşımı kullanarak, katılan destekler.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner adım 10: AJAX LCV'ler etkinleştirme kabul eder.

Şimdi bir Akşam Yemeği katılan ilgilendiklerini RSVP oturum açmış kullanıcılar için destek hemen uygulayın. Biz, Şimdi Akşam Ayrıntıları sayfasında tümleştirilmiş bir AJAX tabanlı yaklaşımı kullanarak etkinleştirirsiniz.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Kullanıcı RSVP'd olup olmadığını belirten

Kullanıcılar */Dinners/Ayrıntılar / [kimliği*] belirli bir Akşam Yemeği ilgili ayrıntıları görmek için URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Eylem yöntemi gerçekleştirilir Details() şu şekilde:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

RSVP destek uygulamak için ilk adımımız, bizim Dinner nesnesiyle (daha önce oluşturduğumuz Dinner.cs kısmi sınıf) bir "IsUserRegistered(username)" yardımcı yöntem eklemek için olacaktır. True veya false kullanıcı Akşam Yemeği için geçerli olup olmadığını RSVP'd bağlı olarak bu yardımcı yöntemini döndürür:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Biz sonra aşağıdaki kodu kullanıcının kayıtlı olup olmadığını belirten bir uygun bir ileti görüntülemek için sunduğumuz Details.aspx görünüm şablonu veya olay için ekleyebilirsiniz:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Ve artık bir kullanıcı için kayıtlı bir Akşam Yemeği ziyaret ettiğinde, bu iletiyi görürsünüz:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Ve bunlar kaydedilmedi görürler için bir Akşam Yemeği ziyaret ettikleri zaman aşağıdaki ileti:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Uygulama kayıt eylemi yöntemi

Artık kullanıcılar bir akşam yemeği için RSVP için Ayrıntılar sayfasından etkinleştirmek için gereken işlevselliği ekleyelim.

Bunu gerçekleştirmek için yeni bir "RSVPController" sınıf \Controllers dizinde sağ tıklayıp Ekle - i seçerek oluşturacağız&gt;denetleyicisi menü komutu.

Biz bir "Register" eylem yöntemi için bağımsız değişken olarak bir Akşam Yemeği bir kimliği alır, oturum açan kullanıcı için kayıtlı kullanıcılar listesinde şu anda ise ve bakar uygun Dinner nesnesini alır yeni RSVPController sınıf içinde uygulayacaksınız RSVP nesne için bunları ekler değil:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Nasıl biz basit bir dize eylem yönteminin çıkışını iade ettiğiniz dikkat edin. Biz bu iletiyi bir görünüm şablonu – içindeki katıştırılmış ancak kadar küçük olduğundan yalnızca Content() yardımcı yöntem denetleyicisi temel sınıf ve return gibi bir dize iletisi yukarıda kullanacağız.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>AJAX kullanarak RSVPForEvent eylem yöntemi çağırma

Bizim Ayrıntıları görünümünde kayıt eylemi yöntemi çağırmak için AJAX kullanacağız. Bu uygulama oldukça kolaydır. İlk iki komut dosyası kitaplığı başvurularını ekleyeceğiz:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

İlk kitaplığı çekirdek ASP.NET AJAX istemci tarafı komut dosyası kitaplığı başvuruyor. Bu dosya, yaklaşık 24 k (sıkıştırılmış) boyutu ve çekirdek istemci tarafı AJAX işlevselliği içerir. İkinci kitaplık (kısa bir süre sonra kullanacağız) ASP.NET MVC'nin yerleşik AJAX Yardımcısı yöntemleri ile tümleştirmenize yardımcı programını işlevleri içerir.

Güncelleştirme outputing bir ", bu olay için kaydedilmeyen" iletisi yerine, biz bunun yerine bir bağlantı, zamanla gönderdiğiniz işlemek için daha önce eklediğimiz kodu görüntüle şablon RSVP denetleyicimizin bizim RSVPForEvent eylem yöntemini çağıran bir AJAX çağrısı gerçekleştirir biz seçebilir ve kullanıcı RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Yukarıda kullanılan Ajax.ActionLink() yardımcı yöntem ASP.NET MVC'nda yerleşik olarak bulunan, bağlantıya tıklandığında bir standart gezinti gerçekleştirmek yerine eylem yöntemi için bir AJAX çağrısı kolaylaştırır Html.ActionLink() yardımcı yöntemine benzer. Yukarıdaki biz "RSVP" denetleyicide "Register" eylem yöntemini çağırarak ve DinnerID için "id" parametresi olarak geçirerek. Biz kaçı AjaxOptions parametresi son eylem yönteminden döndürülen içeriği alıp HTML güncelleştirme istiyoruz gösterir &lt;div&gt; sayfasında "rsvpmsg" kimliğine sahip öğe.

Ve Şimdi Akşam Yemeği için bir kullanıcı attığında için kayıtlı değil henüz RSVP onun için bir bağlantı görürler:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

"Bu olayın RSVP" Bağlantısı'na tıklarsanız, kayıt eylem yöntemi için bir AJAX çağrısı RSVP denetleyicisinde yapacaksınız ve tamamlandığında güncelleştirilmiş bir ileti görürler aşağıdaki gibi:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Bu AJAX çağrısı yaparken ilgili trafik ve ağ bant genişliği gerçekten basit. Kullanıcı "RSVP bu olay için" bağlantısına tıkladığında, küçük bir HTTP POST ağ isteği yapılan */Dinners/Register/1* kablo aşağıdaki gibi görünür URL'si:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Ve bizim kayıt eylemi yöntemi yanıttan yeterlidir:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Bu basit bir çağrı, hızlı ve daha yavaş bir ağ üzerinden çalışır.

### <a name="adding-a-jquery-animation"></a>JQuery bir animasyon ekleme

Uyguladık AJAX işlevselliği iyi ve hızlı bir şekilde çalışır. Bazen hızlı, ancak, bir kullanıcı RSVP bağlantı ile yeni metin değiştirildiğini fark etmeyebilirsiniz emin olabilir. Sonucu biraz daha belirgin hale getirmek için güncelleştirme iletisi dikkat çekmek için basit bir animasyon ekleyebilirsiniz.

' % S'varsayılan ASP.NET MVC proje şablonu, jQuery – da Microsoft tarafından desteklenen bir mükemmel (ve çok popüler) açık kaynak JavaScript kitaplığı içerir. bir dizi özellik, iyi bir HTML DOM seçimi ve etkileri kitaplığı dahil olmak üzere jQuery sağlar.

JQuery kullanmak için önce bir komut dosyası başvuru ekleyeceğiz. JQuery içinde çeşitli yerlerde sitemizi içinde kullanılmasını kullanacağız çünkü tüm sayfaları kullanabilmesi betik başvurusu bizim Site.master ana sayfa dosyası içinde ekleyeceğiz.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*İpucu: VS 2008 SP1'de JavaScript dosyaları (jQuery dahil) için daha zengin IntelliSense desteği sağlayan JavaScript IntelliSense düzeltme yüklediğinizden emin olun. Buradan indirebilirsiniz: http://tinyurl.com/vs2008javascripthotfix*

Genellikle, JQuery kullanılarak yazılmış kod bir genel "$ ()" kullanan bir CSS seçicisini kullanarak bir veya daha fazla HTML öğeleri alır bir JavaScript yöntemini. Örneğin, <em>$("#rsvpmsg")</em> herhangi bir HTML öğesi kimliği rsvpmsg, seçer sırada <em>$(".something")</em> "şey" CSS tüm öğelerle seçeceğiniz sınıf adı. Ayrıca, "tüm işaretli radyo düğmeleri return gibi" daha gelişmiş sorgular yazabilirsiniz gibi bir seçici sorgu kullanarak: <em>$("Giriş [@typeradyo =] [@checked]")</em>.

Öğeleri seçtikten sonra bunlardaki gizlemeden gibi eylemler gerçekleştiren yöntemleri çağırabilirsiniz: *$("#rsvpmsg").hide();*

RSVP senaryomuz için biz ","rsvpmsg"seçer AnimateRSVPMessage" adlı basit bir JavaScript işlevi tanımlama olasılığınız &lt;div&gt; ve metin içeriğini boyutunu canlandırın. Aşağıdaki kodu küçük metin ve ardından nedenleri başlar, bir 400 milisaniyeden zaman çerçevesi içinde artırmak için:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Biz ardından kablo bu JavaScript işlevi için sunduğumuz Ajax.ActionLink() yardımcı yöntem adını geçirerek müşterilerimizin AJAX çağrısı başarıyla tamamlandıktan sonra çağrılacak Yukarı ("OnSuccess" AjaxOptions aracılığıyla olay özellik):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Ve artık "Bu olayın RSVP" bağlantısına tıkladı ve bizim AJAX çağrısı gönderilen içerik ileti başarıyla tamamlar arka animasyon ekleme ve büyük büyütmeyi:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

"OnSuccess" olay sağlamanın yanı sıra AjaxOptions nesne (çeşitli diğer özellikleri ve kullanışlı seçenekleri ile birlikte) işleyebileceği OnBegin OnFailure ve OnComplete olayları gösterir.

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Temizlik - out RSVP kısmi görünüm yeniden düzenleme

Bizim şablonu ayrıntıları görüntüleme biraz uzun almak hangi mesai bunu anlamak biraz daha zor hale getirecek başlatılıyor. Kodun okunabilirliğini geliştirmek için şimdi tüm ayrıntıları sayfamıza RSVP görünümü kodu kapsülleyen bir kısmi görünüm – RSVPStatus.ascx – oluşturarak sonlandırın.

\Views\Dinners klasörü sağ tıklatın ve sonra Add - seçerek bunu yapabilirsiniz&gt;görüntüle menü komutu. Biz Şimdi Akşam nesnesi, kesin türü belirtilmiş ViewModel olarak kazandığını sahip olacaksınız. Biz ardından kopyalama/RSVP içeriği bizim Details.aspx görünümünden içine yapıştırma.

Biz yaptıktan sonra ayrıca bizim düzenleme ve silme bağlantısı görünümü kodu kapsülleyen başka bir kısmi görünüm – EditAndDeleteLinks.ascx - oluşturalım. Biz de bunu bir Akşam Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak yararlanın ve Kopyala/Yapıştır, bizim Details.aspx görünümüne düzenleme ve silme mantığından sahip olacaksınız.

Bizim ayrıntıları şablon can görüntüleyin. ardından hemen altındaki iki Html.RenderPartial() yöntemi çağrılarını içerir:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Bu kod okunması ve düzenlenmesi daha temiz olmasını sağlar.

### <a name="next-step"></a>Sonraki adım

Şimdi nasıl biz AJAX daha da kullanabilir ve uygulamamıza etkileşimli eşleme desteği eklendi göz atalım.

> [!div class="step-by-step"]
> [Önceki](secure-applications-using-authentication-and-authorization.md)
> [İleri](use-ajax-to-implement-mapping-scenarios.md)
