---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Dinamik güncelleştirmeleri göndermek için AJAX kullanma | Microsoft Docs"
author: microsoft
description: "Adım 10 Implements oturum açan kullanıcılar için RSVP içinde Yemeği ayrıntı tümleşik bir Ajax tabanlı yaklaşımı kullanarak, bir Yemeği katılan içinde kendi ilgi destek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Dinamik güncelleştirmeleri göndermek için AJAX kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Bu 10 ücretsiz bir adımdır ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 10 Implements oturum açan kullanıcılar için RSVP Yemeği Ayrıntıları sayfasında tümleşik bir Ajax tabanlı yaklaşımı kullanarak, bir Yemeği katılan içinde kendi ilgi destekler.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 10. adım: LCV'ler etkinleştirme AJAX kabul eder

Şimdi bir Yemeği katılan içinde kendi ilgi RSVP oturum açan kullanıcılar için destek şimdi uygulayın. Biz bu Yemeği Ayrıntıları sayfasında tümleşik bir AJAX tabanlı yaklaşım kullanarak etkinleştirin.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Kullanıcı RSVP'd olup olmadığını belirten

Kullanıcıların ziyaret */Dinners/Ayrıntılar / [kimliği*] belirli Yemeği ayrıntılarını görmek için URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Eylem yöntemi uygulanır Details() sözlüğüdür:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

RSVP destek uygulamak için ilk adım, bir "IsUserRegistered(username)" yardımcı yöntem bizim Yemeği nesnesiyle (daha önce oluşturduğumuz Dinner.cs parçalı sınıf) eklemek için olacaktır. True veya false kullanıcı yemeği için geçerli olup olmadığını RSVP'd bağlı olarak bu yardımcı yöntemini döndürür:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Biz sonra aşağıdaki kodu kullanıcı kayıtlı olup olmadığını belirten bir uygun iletisi görüntülenecek bizim Details.aspx şablonu görüntüleme veya olay için ekleyebilirsiniz:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Ve artık kullanıcı için kayıtlı bir Yemeği ziyaret ettiğinde, bu iletiyi görürsünüz:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Ve bunlar kaydedilmedi bunlar görürsünüz için yemeği ziyaret ettiğinizde ileti altında:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Kayıt eylemi yöntemi uygulama

Artık kullanıcıların bir Yemeği RSVP için Ayrıntılar sayfasından etkinleştirmek için gereken işlevleri ekleyelim.

Bu uygulama için yeni bir "RSVPController" sınıf \Controllers dizinde sağ tıklayıp seçerek Ekle - oluşturacağız&gt;denetleyicisi menü komutu.

"Register" eylem yöntemi için bağımsız değişken olarak bir Yemeği kimliği alır, uygun Yemeği nesnesine oturum açma kullanıcı şu an için kayıtlı kullanıcıların listesini kullanılıyorsa ve bakar alır yeni RSVPController sınıf içinde uygulamak RSVP nesneyi bunları ekler değil:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Nasıl biz basit bir dize eylem yönteminin çıkışını iade ettiğiniz dikkat edin. Biz bu iletiyi bir görünüm şablonu içinde– katıştırılmış ancak kadar küçük olduğundan yalnızca Content() yardımcı yöntem denetleyicisi temel sınıf ve dize ileti gibi yukarıda return kullanacağız.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>AJAX kullanarak RSVPForEvent eylem yöntemini çağırma

Bizim Ayrıntıları görünümünden kayıt eylemi yöntemi çağırmak için AJAX kullanacağız. Bu uygulama oldukça kolaydır. İlk iki komut dosyası kitaplığı başvuru ekleyeceğiz:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

İlk kitaplığı çekirdek ASP.NET AJAX istemci tarafı komut dosyası kitaplığı başvurur. Bu dosya, yaklaşık 24 k (sıkıştırılmış) boyutu ve çekirdek istemci tarafı AJAX işlevselliği içerir. İkinci kitaplığı (kısa bir süre sonra kullanacağız) ASP.NET MVC'ın yerleşik AJAX yardımcı yöntemler ile tümleştirmenize yardımcı işlevlerini içerir.

Bizim RSVP denetleyicisinde bizim RSVPForEvent eylem yöntemini çağırır bir AJAX çağrısı güncelleştirme outputing ", bu olay için kayıtlı olmayan" iletisi yerine biz bunun yerine bir bağlantı, basıldığında işlemek için daha önce eklediğimiz görünüm şablonu kodu gerçekleştirir sonra geçebiliriz ve kullanıcı RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Yukarıda kullanılan Ajax.ActionLink() yardımcı yöntemi yerleşik-ASP.NET MVC ve bağlantı tıklatıldığında standart gezinti gerçekleştirmek yerine eylem yöntemi için bir AJAX çağrısı kolaylaştırır Html.ActionLink() yardımcı yöntemine benzerdir. Yukarıdaki biz "RSVP" denetleyicisinde "Register" eylem yöntemini çağırma ve "id" parametresi olarak kendisine DinnerID geçirme. Eylem yönteminden döndürülen içeriği almak ve HTML güncelleştirmek istiyoruz geçirme son AjaxOptions parametre gösterir &lt;div&gt; kimliğine sahip "rsvpmsg" sayfasında öğesi.

Ve şimdi bir kullanıcı bir Yemeği attığında için kayıtlı değil henüz RSVP onun için bir bağlantı göreceksiniz:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

"Bu olay için RSVP" bağlantısına tıklarsanız kayıt eylem yöntemi için bir AJAX çağrısı RSVP denetleyicisinde yaptıkları ve tamamlandığında güncelleştirilmiş bir ileti görürsünüz aşağıdaki gibi:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Ağ bant genişliği ve trafik bu AJAX çağrısı yaparak gerçekten hafif olduğunda söz konusu. Kullanıcı "RSVP bu olay için" bağlantısına tıkladığında, küçük bir HTTP POST ağ isteği yapılan */Dinners/Register/1* kablo aşağıdaki gibi görünen URL'si:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Ve bizim kayıt eylemi yöntemi yanıttan yeterlidir:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Bu basit aramayı hızlıdır ve hatta bir yavaş ağ üzerinden çalışır.

### <a name="adding-a-jquery-animation"></a>JQuery animasyon ekleme

Biz uygulanan AJAX işlevselliği, düzgün ve hızlı bir şekilde çalışır. Bazen hızlı, yine de bir kullanıcı yeni metinle RSVP bağlantı değiştirildi fark etmeyebilirsiniz emin olabilir. Sonucu biraz daha belirgin yapmak için güncelleştirme iletisi dikkat çekmek için basit bir animasyon ekleyebilirsiniz.

ASP.NET MVC proje şablonu varsayılan jQuery – ayrıca Microsoft tarafından desteklenen bir mükemmel (ve çok yaygın) açık kaynak JavaScript kitaplığı içerir. jQuery iyi bir HTML DOM seçimi ve etkileri kitaplığı gibi özellikleri de sağlar.

JQuery kullanmak için öncelikle bir komut dosyası başvuru ekleyeceğiz. JQuery yerler, çeşitli içinde sitemizi içinde kullanılmasını kalacaklarını olduğundan, tüm sayfaları kullanabilmesi komut dosyası başvurusunu bizim Site.master ana sayfa dosyası içinde ekleyeceğiz.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*İpucu: VS 2008 SP1'de, JavaScript dosyaları (jQuery dahil) için daha zengin IntelliSense desteği sağlayan JavaScript IntelliSense düzeltmesinin yüklü olup olmadığını denetleyin. Buradan indirebilirsiniz: http://tinyurl.com/vs2008javascripthotfix*

JQuery genellikle kullanılarak yazılan kodu kullanan bir genel "$ ()" bir CSS seçicisini kullanarak bir veya daha fazla HTML öğeleri alır JavaScript yöntemi. Örneğin, *$("#rsvpmsg")* rsvpmsg, kimliği herhangi bir HTML öğesi seçer sırada *$(".something")* şeyle "" CSS tüm öğeleri seçeceğiniz sınıf adı. "Tüm checked radyo düğmeleri return"gibi daha gelişmiş sorgular da yazabilirsiniz gibi bir seçici sorgu kullanarak: *$("Giriş [@typeradyo =] [@checked]")*.

Öğeleri seçtikten sonra kendilerine gizleyip gibi eyleme geçmek için yöntemleri çağırabilirsiniz: *$("#rsvpmsg").hide();*

RSVP senaryomuz için biz "" rsvpmsg"seçer AnimateRSVPMessage" adlı basit bir JavaScript işlevi tanımlarsınız &lt;div&gt; ve metin içeriğini boyutunu canlandırır. Kod, metni küçük ve daha sonra nedenleri başlatır, 400 milisaniye zaman çerçevesi içinde artırmak için:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Biz sonra kablo bizim Ajax.ActionLink() yardımcı yöntem adını geçirerek bizim AJAX çağrısı başarıyla tamamlandıktan sonra çağrılacak bu JavaScript işlevi Yukarı ("OnSuccess" AjaxOptions aracılığıyla olay özellik):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Ve şimdi "RSVP bu olay için" Bağlantı tıklatıldığında ve bizim AJAX çağrısı gönderilen içerik ileti başarıyla tamamlandığında, geri hale getirmeyi ve büyük büyütmeyi:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Bir "OnSuccess" olay sağlamanın yanı sıra, AjaxOptions nesne (çeşitli diğer özellikleri ve yararlı seçenekleri ile birlikte) işleyebilir OnBegin, OnFailure ve OnComplete olayları gösterir.

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Temizleme - RSVP kısmi görünüm çıkışı düzenleme

Bizim Ayrıntılar görünümü şablon biraz uzun almak hangi mesai anlamak biraz daha zor yapacak başlatılıyor. Kodun okunabilirliğini artırmak için şimdi tüm ayrıntıları sayfamızı RSVP görünüm kodunu kapsülleyen bir kısmi görünüm – RSVPStatus.ascx – oluşturarak sonlandırın.

Bu \Views\Dinners klasöre sağ tıklayarak ve ardından Ekle - yapabiliriz&gt;görüntülemek menü komutu. Biz bunu Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak ele sahip olacaksınız. Biz sonra kopyalayıp RSVP içerik içine bizim Details.aspx görünümünden yapıştırın.

Biz yaptıktan sonra ayrıca bizim düzenleme ve silme bağlantı Görünüm Kodu yalıtır başka bir kısmi görünüm – EditAndDeleteLinks.ascx - oluşturalım. Biz de Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak alın ve kopyalayıp yapıştırın bizim Details.aspx görünümüne, düzenleme ve silme mantığından sahip olacaksınız.

Bizim ayrıntıları şablon can görüntüleyin. ardından hemen altındaki iki Html.RenderPartial() yöntemi çağrılarını içerir:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Bu kod okuyun ve korumak için Temizleyici hale getirir.

### <a name="next-step"></a>Sonraki adım

Şimdi nasıl biz AJAX daha kullanın ve uygulamamız için etkileşimli eşleme desteği eklemek bakalım.

>[!div class="step-by-step"]
[Önceki](secure-applications-using-authentication-and-authorization.md)
[sonraki](use-ajax-to-implement-mapping-scenarios.md)
