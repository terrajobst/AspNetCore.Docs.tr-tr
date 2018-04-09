---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: İle performansı iyileştirme çıkış önbelleğe alma (C#) | Microsoft Docs
author: microsoft
description: Bu öğreticide, nasıl, önemli ölçüde ASP.NET MVC web uygulamalarınızın performansını çıkış önbelleğe alma yararlanarak artırabilir öğrenin. ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 8958caa5a0ccad669ca861bed261102625be5cb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="improving-performance-with-output-caching-c"></a>Çıktı önbelleği (C#) ile performansı iyileştirme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğreticide, nasıl, önemli ölçüde ASP.NET MVC web uygulamalarınızın performansını çıkış önbelleğe alma yararlanarak artırabilir öğrenin. Böylece aynı içerik her zaman yeni bir kullanıcı eylemi çağırır oluşturulması gerekmez denetleyicisi eylemden döndürülen sonuç önbelleğe öğrenin.


Bu öğreticinin nasıl önemli ölçüde performansı ASP.NET MVC uygulamasının çıktı önbelleği yararlanarak artırabilirsiniz açıklamak için hedeftir. Çıktı önbelleği, denetleyici eylemi tarafından döndürülen içeriği önbelleğe olanak sağlar. Böylece, aynı içerik her zaman aynı denetleyici eylem çağrılan oluşturulması gerekmez.

Örneğin, ASP.NET MVC uygulamanızın dizin adlı bir görünümde veritabanı kayıtlarının listesini görüntüler düşünün. Normalde, bir kullanıcı dizini görünümü döndüren denetleyici eylemi çağırır her zaman veritabanı kayıt kümesi veritabanından veritabanı sorgusunun yürütülmesi tarafından alınması gerekir.

Öte yandan, çıktı önbelleği yararlanmak, herhangi bir kullanıcı aynı denetleyici eylemi çağırır her zaman bir veritabanı sorgusunun yürütülmesi önleyebilirsiniz. Görünüm, denetleyici eylem yeniden yerine önbellekten alınabilir. Etkinleştirir önbelleğe alma, yedekli gerçekleştirme önlemek için sunucu üzerinde çalışır.

## <a name="enabling-output-caching"></a>Çıkış önbelleğe almayı etkinleştirme

Tek tek denetleyici eylem ya da bir tüm denetleyicinin sınıf için bir [OutputCache] özniteliği ekleyerek çıktı önbelleği etkinleştirin. Örneğin, denetleyici listeleme 1 İNDİS() adlı bir eylem kullanıma sunar. İNDİS() eylem çıktısını 10 saniye için önbelleğe alınır.

**1 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

ASP.NET MVC Beta sürümlerinde, çıktı önbelleği için bir URL gibi çalışmaz [ http://www.MySite.com/ ](http://www.mysite.com/). Bunun yerine, gibi bir URL girmelisiniz [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Listeleme 1'de İNDİS() eylem çıktısını 10 saniye için önbelleğe alınır. İsterseniz, daha uzun bir önbellek süresi belirtebilirsiniz. Bir denetleyici eylemi bir gün çıkışını önbelleğe almak istiyorsanız, örneğin, ardından 86400 saniye cinsinden önbellek süresi belirleyebilirsiniz (60 saniye \* 60 dakika \* 24 saat).

Var olan içeriğin garanti, belirttiğiniz süre miktarı için önbelleğe alınır. Bellek kaynakları düşük olduğunda, önbellek çıkarılırken içeriği otomatik olarak başlar.

Listeleme 1 giriş denetleyicisi listeleme 2'de dizin görünümünün döndürür. Bu görünüm hakkında özel bir şey yoktur. Dizin görünümünün yalnızca geçerli saati görüntüler (bkz: Şekil 1).

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Şekil 1 – önbelleğe alınan dizini görünümü**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Tarayıcınızın adres çubuğuna URL'yi /Home/dizin girerek ve art arda sonra dizini görünüm tarafından görüntülenen zaman tarayıcınızda yenileme/yeniden yükle düğmesine basarsa birden çok kez İNDİS() eylemi çağırmak, 10 saniye değiştirmez. Aynı zamanda, görünümü önbelleğe çünkü görüntülenir.

Aynı görünüm uygulamanızı ziyaret herkes için önbelleğe alma anlamak önemlidir. İNDİS() eylemi çağırır herkes dizin görünümünün aynı önbelleğe alınmış sürümünü alır. Başka bir deyişle, web sunucusu dizin görünümünün sunmak için gerçekleştirmeniz gereken çalışma miktarını önemli ölçüde azalır.

Çok basit bir şey yapması gerektiğini listeleme 2 görünümünde olur. Görünüm yalnızca geçerli saati görüntüler. Ancak, yalnızca kolayca önbelleği olarak bir dizi veritabanı kayıtlarını görüntüleyen bir görünüm verebilir. Bu durumda, veritabanı kayıt kümesi görünümü döndüren denetleyici eylem çağrılan her zaman veritabanından alınacak ihtiyaç duymaz. Önbelleğe alma, web sunucusu ve veritabanı sunucusu gerçekleştirmelisiniz çalışma miktarını azaltır.

Sayfa kullanmayan &lt;% @ OutputCache %&gt; MVC görünümündeki yönergesi. Bu yönerge, Web Forms dünyadan üzerinden taşmasını ve bir ASP.NET MVC uygulamasındaki kullanılmamalıdır.

## <a name="where-content-is-cached"></a>Burada içerik önbelleğe

[OutputCache] özniteliğini kullandığınızda varsayılan olarak, üç konumda içeriği önbelleğe alınır: web sunucusu, herhangi bir proxy sunucusu ve web tarayıcısı. Tam olarak burada içerik konum özelliği [OutputCache] özniteliğinin değiştirerek önbelleğe kontrol edebilirsiniz.

Konum özelliği aşağıdaki değerlerden birine ayarlayabilirsiniz:

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


Varsayılan olarak, konum özelliği herhangi değerine sahip. Ancak, yalnızca tarayıcı veya sadece sunucuda önbelleğe içinde isteyebilirsiniz durumlar vardır. Her kullanıcı için kişiselleştirilmiş bilgileri önbelleğe alma, örneğin, daha sonra sunucudaki bilgileri önbelleğe kaydetmelisiniz. Farklı kullanıcılara farklı bilgiler görüntülüyorsanız bilgilerin yalnızca istemcide önbelleğe.

Örneğin, geçerli kullanıcı adını döndürür GetName() adlı bir eylem listeleme 3'te denetleyicisi sunar. Prizine Web sitesine oturum ve GetName() eylemi çağırır eylem "Hi prizine" dizesini döndürür. Sonuç olarak, Jill Web sitesinde oturum günlüğe kaydeder ve GetName() eylemi çağırır, ardından kendisi de "Hi prizine" dize alırsınız. Prizine başlangıçta denetleyici eylemini çağırır sonra dize tüm kullanıcılar için web sunucusunda önbelleğe alınır.

**3 – Controllers\BadUserController.cs listeleme**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Büyük olasılıkla listeleme 3'te denetleyicisi istediğiniz şekilde çalışmıyor. "Hi prizine" iletisi Jill için görüntülemek istediğiniz yok.

Hiçbir zaman sunucu önbelleğindeki kişiselleştirilmiş içerik önbelleğe. Ancak, kişiselleştirilmiş içerik performansını artırmak için tarayıcı önbelleğinde önbellek isteyebilirsiniz. Tarayıcıda içeriği önbelleğe ve bir kullanıcı birden çok kez aynı denetleyici eylemi çağırır, içerik sunucusu yerine tarayıcı önbelleğinden alınabilir.

Değiştirilen denetleyicisi listeleme 4'te GetName() eylem çıktısını önbelleğe alır. Ancak, içeriği, yalnızca tarayıcı ve sunucuda önbelleğe alınır. Birden çok kullanıcı GetName() yöntemi çağırdığınızda bu şekilde, her birinin kendi kullanıcı adı ve değil başka bir kişinin kullanıcı adını alır.

**4 – Controllers\UserController.cs listeleme**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

4 Listeleme [OutputCache] özniteliğinde konum özelliği OutputCacheLocation.Client değerine ayarlanmış içerdiğine dikkat edin. [OutputCache] özniteliği de NoStore özelliği içerir. NoStore özelliği, önbelleğe alınmış içeriği kalıcı bir kopyasını depolanmamalıdır proxy sunucular ve tarayıcı bildirmek için kullanılır.

## <a name="varying-the-output-cache"></a>Çıktı önbelleği değişen

Bazı durumlarda, çok aynı içeriği önbelleğe alınmış farklı sürümlerini isteyebilirsiniz. Örneğin, bir ana öğe/ayrıntı sayfası oluşturmakta olduğunuz düşünün. Ana sayfaya başlık listesini görüntüler. Başlık tıklattığınızda, seçilen film için ayrıntıları alın.

Ayrıntılar sayfası önbelleğe kaydederseniz, aynı film ayrıntılarını hangi film olsun tıklattığınız görüntülenir. İlk kullanıcı tarafından seçilen ilk film gelecekteki tüm kullanıcılar için görüntülenir.

[OutputCache] özniteliği VaryByParam özelliğinin yararlanarak bu sorunu düzeltebilirsiniz. Bu özellik, bir form parametre çok aynı içeriği önbelleğe alınmış farklı sürümlerini oluşturmanıza olanak sağlar veya sorgu dizesi parametresi değişir.

Örneğin, denetleyici listeleme 5'te Master() ve Details() adlı iki eylem kullanıma sunar. Master() eylem film adlarının bir listesini döndürür ve Details() eylem seçili film ayrıntılarını döndürür.

**5 – Controllers\MoviesController.cs listeleme**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Değer VaryByParam özelliğiyle Master() eylem içeriyorsa "none". Eylem çağrılır, Master() ana aynı önbelleğe alınmış sürümünü görüntülediğinizde döndürülür. Tüm form parametreleri veya sorgu dizesi parametreleri (bkz. Şekil 2) yoksayıldı.

**Şekil 2 – /Movies/Master görünümü**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Şekil 3 – / filmler/Ayrıntılar görünümü**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Details() eylem "Id" değerine sahip bir VaryByParam özelliği içerir. Ayrıntılar görünümü önbelleğe alınmış farklı sürümlerini farklı değerler kimliği parametresinin denetleyici eylemi geçirildiğinde üretilir.

Daha fazla önbelleğe alma işleminde VaryByParam özelliği sonuçları kullanan anlamak önemli ve daha az değil. Ayrıntılar görünümü farklı bir önbelleğe alınan sürümü ID parametresi için her farklı sürüm oluşturulur.

Aşağıdaki değerlere VaryByParam özelliği de ayarlayabilirsiniz:

> \* = Bir form veya sorgu dizesi parametresi değişir her farklı bir önbelleğe alınan sürüm oluşturun.
> 
> Hiçbiri = hiçbir zaman farklı önbelleğe alınmış sürümlerini oluşturun
> 
> Parametrelerin noktalı listesi oluşturma farklı önbelleğe alınmış sürümleri listesinde form veya sorgu dizesi parametrelerinden herhangi birini değişir her =


## <a name="creating-a-cache-profile"></a>Önbellek profili oluşturma

Çıktı önbelleği özellikleri [OutputCache] özniteliği özelliklerini değiştirerek yapılandırma alternatif olarak, web (web.config) yapılandırma dosyasında bir önbellek profili oluşturabilirsiniz. Web yapılandırma dosyasında bir önbellek profili oluşturma birkaç önemli avantajlar sunar.

İlk olarak, web yapılandırma dosyasında çıktı önbelleği yapılandırarak, denetleyici eylemleri içeriği tek bir merkezi konumda nasıl önbelleğe kontrol edebilirsiniz. Bir önbellek profili oluşturun ve birkaç denetleyicileri veya denetleyici eylemleri profili uygulayın.

İkinci olarak, uygulamanızın derlemeden web yapılandırma dosyasını değiştirebilirsiniz. Üretim için zaten dağıtılmış bir uygulama için önbelleğe almayı devre dışı bırakmanız gerekirse web yapılandırma dosyasında tanımlanmış önbellek profilleri yalnızca değiştirebilirsiniz. Web yapılandırma dosyası değişiklikleri otomatik olarak algılanır ve uygulanır.

Örneğin, &lt;önbelleğe alma&gt; web yapılandırması bölümü listeleme 6 Cache1Hour adlı bir önbellek profili tanımlar. &lt;Önbelleğe alma&gt; bölüm içinde görünmelidir &lt;system.web&gt; web yapılandırma dosyası bölümünü.

**6 – web.config için önbelleğe alma bölümüne listeleme**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Denetleyici listeleme 7'de nasıl bir denetleyici eylemi [OutputCache] özniteliğiyle Cache1Hour profili uygulayabilirsiniz gösterilmektedir.

**7 – Controllers\ProfileController.cs listeleme**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Listeleme 7 denetleyicisi tarafından sunulan İNDİS() eylemi çağırmak olursa aynı anda 1 saat boyunca döndürülür.

## <a name="summary"></a>Özet

Çıktı önbelleği, ASP.NET MVC uygulamalarınızın performansını önemli ölçüde artırmak çok kolay bir yöntem sağlar. Bu öğreticide, denetleyici eylemleri çıkışını önbelleğe almak için [OutputCache] özniteliğini kullanın öğrendiniz. Ayrıca nasıl içeriği önbelleğe değiştirmek için süre ve VaryByParam özellikler gibi [OutputCache] öznitelik özelliklerini değiştirmek öğrendiniz. Son olarak, web yapılandırma dosyasında önbellek profilleri tanımlamak nasıl öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](understanding-action-filters-cs.md)
> [sonraki](adding-dynamic-content-to-a-cached-page-cs.md)
