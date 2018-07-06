---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Çıktı önbelleğe alma (VB) ile performansı iyileştirme | Microsoft Docs
author: microsoft
description: Bu öğretici sayesinde nasıl, ASP.NET MVC web uygulamalarınızın performansını çıkış önbelleğe alma özelliğinden yararlanarak artırabilirsiniz öğrenin. ...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 72d73067f6f7dabe4644a35c8d9462bfbc7a93ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807336"
---
<a name="improving-performance-with-output-caching-vb"></a>Çıktı önbelleğe alma (VB) ile performansı iyileştirme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğretici sayesinde nasıl, ASP.NET MVC web uygulamalarınızın performansını çıkış önbelleğe alma özelliğinden yararlanarak artırabilirsiniz öğrenin. Aynı içerik, her zaman yeni bir kullanıcı eylemi çağırır oluşturulması gerekmez. böylece bir denetleyici eylemi döndürülen sonuç önbelleğe öğrenin.


Bu öğreticide nasıl, bir ASP.NET MVC uygulamasının performansını çıktı önbelleği avantajlarından yararlanarak artırabilirsiniz açıklamak için hedefidir. Çıkış önbelleğini, denetleyici eylem tarafından döndürülen içeriği önbelleğe olanak tanır. Bu şekilde, aynı içerik her zaman aynı denetleyici eylemi çağrılır oluşturulması gerekmez.

Örneğin, ASP.NET MVC uygulamanızın bir görünümde dizin adlı veritabanı kayıtlarını içeren bir liste görüntüler düşünün. Normalde, bir kullanıcı dizin görünümünün döndüren denetleyici eylemini çağırır her zaman veritabanı kayıt kümesini veritabanından bir veritabanı sorgusunun yürütülmesi tarafından alınmalıdır.

Öte yandan, çıktı önbelleği yararlanabilirsiniz, herhangi bir kullanıcı aynı denetleyici eylemini çağırır her zaman bir veritabanı sorgusunun yürütülmesi önleyebilirsiniz. Görünüm, denetleyici eylem yeniden oluşturuluyor yerine önbellekten alınabilir. Önbelleğe alma etkinleştirir, yedekli uygulamaktan kaçının sunucu üzerinde çalışır.

#### <a name="enabling-output-caching"></a>Çıkış önbelleğe almayı etkinleştirme

Çıkış ekleyerek önbelleğe almayı etkinleştirdikten bir &lt;OutputCache&gt; ayrı denetleyicisinin eylem ya da tüm denetleyicinin Sınıf özniteliği. Örneğin, denetleyici 1 listeleme İNDİS() adlı bir eylem kullanıma sunar. İNDİS() eylemin çıkışına 10 saniye boyunca önbelleğe alınır.

**1 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


ASP.NET MVC Beta sürümleri, çıktı önbelleği gibi bir URL için çalışmaz [ http://www.MySite.com/ ](http://www.mysite.com/). Bunun yerine, gibi bir URL girmelisiniz [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).


Listeleme 1'de, çıktı İNDİS() eylemin 10 saniye boyunca önbelleğe alınır. İsterseniz, daha uzun bir önbellek süresi belirtebilirsiniz. Bir gün için bir denetleyici eylemi çıkışını önbelleğe almak istiyorsanız, ardından bir 86400 saniye cinsinden önbellek süresi belirtebilirsiniz (60 saniye \* 60 dakika \* 24 saat).

Yoktur, içerik garantisi, belirttiğiniz süre miktarı önbelleğe alınır. Bellek kaynakları düşük olduğunda, önbellek çıkarılırken içeriği otomatik olarak başlar.

Giriş denetleyicisine listeleme 1 listeleme 2'de dizin görünümünün döndürür. Bu görünüm hakkında özel bir şey yoktur. Index görünümünü yalnızca geçerli zamanı görüntüler (bkz. Şekil 1).

**2 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Şekil 1: önbelleğe alınan dizini görünümü**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Tarayıcınızın adres çubuğuna URL'yi /Home/dizin girerek ve art arda sonra dizini görünüm tarafından görüntülenen zaman tarayıcınızda yenileme/yeniden yükle düğmesine basarak birden çok kez İNDİS() eylemini çağır, 10 saniye boyunca değiştirmez. Görünüm önbelleğe alındığından, aynı zamanda görüntülenir.

Aynı görünümde, uygulamanızın ziyaret eden herkes için önbelleğe alınmış anlamak önemlidir. İNDİS() eylemi çağırır herkes dizin görünümünün aynı önbelleğe alınmış sürümünü alırsınız. Başka bir deyişle, web sunucusu Index görünümünü sunmak için gerçekleştirmeniz gereken iş miktarını önemli ölçüde azaltılır.

Gerçekten basit bir şey yapmak için 2 liste görünümünde gerçekleşir. Görünüm, yalnızca geçerli zamanı görüntüler. Ancak, yalnızca kolayca önbelleği olarak bir dizi veritabanı kayıtlarını görüntüleyen bir görünüm oluşturulamadı. Bu durumda, veritabanı kayıt kümesini görünümü döndürür denetleyicisi eylemi çağrıldı her zaman veritabanından alınacak ihtiyaç duymaz. Önbelleğe alma, web sunucusu ve veritabanı sunucusu gerçekleştirmesi gereken iş miktarını azaltabilirsiniz.

Sayfa kullanmayın &lt;% @ OutputCache %&gt; MVC görünümündeki yönergesi. Bu yönerge, Web Forms ile tüm dünyaya üzerinden taşmasını ve bir ASP.NET MVC uygulamasındaki kullanılmamalıdır. 

#### <a name="where-content-is-cached"></a>Burada içerik önbelleğe alınır

Varsayılan olarak kullandığınızda, &lt;OutputCache&gt; özniteliği, içerik üç konumda önbelleğe: web sunucusu, herhangi bir proxy sunucusu ve web tarayıcısı. Tam olarak nerede içerik konumu özelliğini değiştirerek önbelleğe denetleyebilirsiniz &lt;OutputCache&gt; özniteliği.

Konum özelliği şu değerlerden birini ayarlayabilirsiniz:

> · Tüm
> 
> · İstemci
> 
> · Aşağı Akış
> 
> · Sunucu
> 
> · Yok
> 
> · ServerAndClient


Varsayılan olarak, konum özelliği herhangi bir değere sahip. Ancak, önbelleğe yalnızca tarayıcı veya yalnızca sunucuda isteyebileceğiniz durumlar vardır. Her kullanıcı için kişiselleştirilmiş bilgiler önbelleğe, örneğin, ardından, sunucudaki bilgileri önbelleğe. Farklı kullanıcılar için farklı bilgi görüntülüyorsanız bilgileri yalnızca istemci önbellek.

Örneğin, geçerli kullanıcı adını döndüren GetName() adlı bir eylem denetleyicisi listeleme 3'te kullanıma sunar. Jack bir Web sitesi günlüklerini ve GetName() eylemi çağırır, eylemi "Hi Jack" dizesini döndürür. Sonuç olarak, Jill Web sitesine günlüğe kaydeder ve GetName() eylemi çağırır, ardından kendisi de "Hi Jack" dize alırsınız. Jack başlangıçta denetleyici eylemini çağırır sonra dize tüm kullanıcılar için web sunucusunda önbelleğe alınır.

**3 – Controllers\BadUserController.vb listeleme**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Büyük olasılıkla denetleyici listeleme 3'te, istediğiniz şekilde çalışmaz. "Merhaba Jack" iletisi için Jill görüntülemek istediğiniz yok.

Hiçbir zaman, kişiselleştirilmiş içerik sunucusu önbelleğinde önbellek. Ancak, performansı artırmak için tarayıcı önbelleğini kişiselleştirilmiş içeriği önbelleğe almak isteyebilirsiniz. Tarayıcıda içerikleri önbelleğe almak ve bir kullanıcı birden çok kez aynı denetleyici eylemini çağırır, içerik sunucusu yerine tarayıcı önbelleğinden alınabilir.

Listeleme 4'te değiştirilmiş denetleyicisi GetName() eylemin çıkış önbelleğe alır. Ancak, içeriği yalnızca tarayıcı ve sunucuda önbelleğe alınır. Birden çok kullanıcı GetName() yöntemi çağırdığınızda bu şekilde, her kişi, kendi kullanıcı adı ve değil başka bir kişinin kullanıcı adını alır.

**4 – Controllers\UserController.vb listeleme**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Dikkat &lt;OutputCache&gt; öznitelik listesi 4'te OutputCacheLocation.Client değerine ayarlanırsa bir Location özelliği içerir. &lt;OutputCache&gt; özniteliği NoStore özelliğini de içerir. NoStore özelliği, Ara sunucuları ve tarayıcılar önbelleğe alınmış içeriği kalıcı bir kopyasını depolanmamalıdır bildirmek için kullanılır.

#### <a name="varying-the-output-cache"></a>Çıkış önbelleğini Çeşitleme uygulanıyor

Bazı durumlarda, aynı çok içerik önbelleğe alınmış farklı sürümlerini isteyebilirsiniz. Örneğin, bir ana/ayrıntı sayfasında oluşturduğunuz düşünün. Ana sayfaya başlık listesini görüntüler. Bir başlık tıkladığınızda, seçili film ayrıntıları alın.

Ayrıntılar sayfasını önbelleğe alma, ilgili ayrıntıları aynı film hangi film ne olursa olsun tıkladığınız görüntülenir. İlk kullanıcı tarafından seçilen ilk film gelecekteki tüm kullanıcılara görüntülenir.

VaryByParam özelliği avantajlarından yararlanarak bu sorunu düzeltebilirsiniz &lt;OutputCache&gt; özniteliği. Bu özellik bir form parametre çok aynı içeriği önbelleğe alınmış farklı sürümleri oluşturmanıza olanak tanır veya sorgu dizesi parametresi olarak değişir.

Örneğin, denetleyici listeleme 5'te Master() ve Details() adlı iki eylem kullanıma sunar. Başlık listesi Master() eylem döndürür ve Details() eylem seçili film ayrıntılarını döndürür.

**5-Controllers\MoviesController.vb listeleme**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

Değerine sahip bir VaryByParam özelliği Master() eylem içeriyorsa "none". Eylem çağrılır, ana aynı önbelleğe alınmış sürümünü Master() görüntülediğinizde döndürülür. Tüm form parametreleri veya sorgu dizesi parametreleri (bkz: Şekil 2) yoksayıldı.

**Şekil 2 – /Movies/Master görüntüle**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Şekil 3 – / filmler/Ayrıntılar görünümü**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

"Id" değeri VaryByParam özelliğiyle Details() eylemi içerir. Denetleyici eylemi için farklı değerleri ID parametresine geçirilir, Ayrıntılar görünümünü önbelleğe alınmış farklı sürümlerini üretilir.

Bu, daha fazla önbellek VaryByParam özelliği sonuçları kullanarak anlamak önemlidir ve değil daha az olur. Ayrıntılar görünümü farklı önbelleğe alınmış bir sürümü ID parametresi için her farklı sürüm oluşturulur.

Aşağıdaki değerlere VaryByParam özelliği ayarlayabilirsiniz:

> \* = Bir form veya sorgu dizesi parametresi değişir her farklı bir önbelleğe alınmış sürümünü oluşturur.
> 
> Hiçbiri hiçbir zaman = önbelleğe alınan farklı sürümlerini oluşturun
> 
> Parametrelerin noktalı virgül listesi oluştur farklı önbelleğe alınan sürümleri herhangi bir form veya sorgu dizesi parametreleri listedeki değişir her =


#### <a name="creating-a-cache-profile"></a>Önbellek profili oluşturma

Çıktı önbellek özelliklerini özelliklerini değiştirerek yapılandırma alternatif olarak &lt;OutputCache&gt; özniteliği, web yapılandırma (web.config) dosyasında bir önbellek profili oluşturabilirsiniz. Web yapılandırma dosyasında bir önbellek profili oluşturuluyor, birkaç önemli avantaj sunar.

İlk olarak, web yapılandırma dosyasında çıktı önbelleği yapılandırarak, denetleyici eylemlerini içeriği tek bir merkezi konumda nasıl önbelleğe kontrol edebilirsiniz. Bir önbellek profili oluşturabilir ve profil birkaç denetleyicileri veya denetleyici eylemleri için geçerlidir.

İkinci olarak, web yapılandırma dosyası, uygulamanızı yeniden derlemeye gerek kalmadan değiştirebilirsiniz. Üretim için zaten dağıtılmış bir uygulama için önbelleğe alma devre dışı bırakmanız gerekirse, web yapılandırma dosyasında tanımlanmış önbellek profilleri yalnızca değiştirebilirsiniz. Değişiklikleri web yapılandırma dosyasına otomatik olarak algılandı ve uygulanır.

Örneğin, &lt;önbelleğe alma&gt; listeleme 6'web yapılandırma bölümü Cache1Hour adlı bir önbellek profili tanımlar. &lt;Önbelleğe alma&gt; bölümü içinde görünmelidir &lt;system.web&gt; web yapılandırma dosyası bölümünü.

**6 – web.config için önbelleğe alma bölümüne listeleme**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Denetleyici listeleme 7'de nasıl Cache1Hour profil ile bir denetleyici eylemi uygulayabileceğiniz gösterir &lt;OutputCache&gt; özniteliği.

**7 – Controllers\ProfileController.vb listeleme**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

7'de listeleme denetleyici tarafından kullanıma sunulan İNDİS() eylemin çağırması durumunda aynı anda 1 saat boyunca döndürülür.

#### <a name="summary"></a>Özet

Çıktı önbelleği, ASP.NET MVC uygulamalarınızın performansını önemli ölçüde geliştirmeye, çok kolay bir yöntem sağlar. Bu öğretici sayesinde nasıl kullanacağınızı öğrendiniz &lt;OutputCache&gt; denetleyici eylemleri çıkışını önbelleğe almak için özniteliği. Ayrıca özelliklerini değiştirmek nasıl öğrendiniz &lt;OutputCache&gt; nasıl içeriği önbelleğe değiştirmek için süresi ve VaryByParam özellikleri gibi özniteliği. Son olarak, önbellek web yapılandırma dosyasına profilleri hakkında bilgi edindiniz.

> [!div class="step-by-step"]
> [Önceki](understanding-action-filters-vb.md)
> [İleri](adding-dynamic-content-to-a-cached-page-vb.md)
