---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Ana sayfalar (VB) URL'lerinde | Microsoft Docs
author: rick-anderson
description: "Ana sayfaya URL'lerde ana sayfa dosyanın içerik sayfadan farklı göreli dizinde olması nedeniyle nasıl bölünebilir giderir. Yeniden Temellendirme adresindeki görünüyor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 8aa0ed2fbf385e4b8dbb7e7a3bdb152f1e016e67
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-vb"></a>Ana sayfalar (VB) URL'leri
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Ana sayfaya URL'lerde ana sayfa dosyanın içerik sayfadan farklı göreli dizinde olması nedeniyle nasıl bölünebilir giderir. URL'ler aracılığıyla yeniden Temellendirme adresindeki arar ~ Tanımlayıcı Sözdizimi ve ResolveUrl ve ResolveClientUrl programlı olarak kullanma. (Ayrıca bakın


## <a name="introduction"></a>Giriş

Tüm örneklerde bugüne kadarki içeriği ve ana sayfa (Web sitesinin kök klasöründe) aynı klasörde bulunan gördük. Ancak ana ve içerik sayfalarına aynı klasörde neden olmalıdır bir neden yoktur. Bu gibi durumlarda, içerik sayfaları kesinlikle alt klasörler oluşturabilirsiniz. Benzer şekilde, oluşturacağınız bir `~/MasterPages/` sitenizin ana sayfalar nereye klasör.

İçeriği ve ana sayfalar farklı klasörlerde yerleştirme ile olası bir sorunu bozuk URL'leri içerir. Ana sayfanın göreli URL köprüler, görüntüleri veya diğer öğeler içeriyorsa, bağlantı için farklı bir klasörde bulunan içerik sayfaları geçersiz olacaktır. Bu öğreticide, geçici çözümler yanı sıra bu sorunun kaynağı inceleyeceğiz.

## <a name="the-problem-with-relative-urls"></a>Göreli URL sorun

Bir URL bir web sayfasında olarak kabul edilir bir *göreli URL* gösterdiği için kaynak konumunu Web sitesinin klasör yapısını web sayfasının konuma göre ise. Önde gelen eğik çizgiyle başlamıyor herhangi bir URL'yi (`/`) veya bir protokol (gibi `http://`) URL içeren web sayfasını konumuna bağlı tarayıcı tarafından çözümlendiğinden görelidir.

Örneğin, bizim Web sitesi olan bir `~/Images/` tek bir görüntü dosyası klasörüyle `PoweredByASPNET.gif`. Ana sayfa dosyası `Site.master` sahip bir `<img>` öğesinde `footerContent` aşağıdaki biçimlendirme bölgesiyle:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

`src` Öznitelik değerinde `<img>` öğesi olduğundan göreli bir URL ile başlamıyor `/` veya `http://`. Kısacası, `src` öznitelik değeri belirten konum tarayıcıya `Images` alt klasör adında bir dosya için `PoweredByASPNET.gif`.

Bir içerik sayfasını ziyaret ederken yukarıdaki biçimlendirme doğrudan tarayıcıya gönderilir. Ziyaret etmek için bir dakikanızı ayırın `About.aspx` ve tarayıcıya gönderilen HTML kaynağını görüntüleyin. Ana sayfaya tam aynı biçimlendirmede tarayıcıya gönderilip gönderilmediğini bulacaksınız.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

İçerik sayfasını Kök klasörde ise (olduğu gibi `About.aspx`) her şeyi olduğundan beklendiği gibi çalıştığını bir `Images` kök klasörüne göreli alt klasör. İçerik sayfasını ana sayfa değerinden farklı bir klasörde ise ancak şeyler bölünme. Bunu göstermek için adlı bir alt klasör oluşturun `Admin`. Ardından, adlandırılmış bir içerik sayfasını eklemek `Default.aspx` için `Admin` klasörü, yeni sayfaya bağlamak emin `Site.master` ana sayfa.

> [!NOTE]
> İçinde [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) adlı bir özel ana sayfa sınıf oluşturduğumuz öğretici `BasePage` içerik sayfasının başlığı otomatik olarak ayarlanır (varsa, açıkça atandı değil). Öğesinden türetilen yeni oluşturulan sayfanın arka plandaki kod sınıfı unutmayın `BasePage` böylece bu işlevselliği kullanabilir.


Bu içerik sayfasını oluşturduktan sonra Çözüm Gezgini Şekil 1'e benzer görünmelidir.


![Yeni bir klasör ve ASP.NET sayfası projeye eklendi](urls-in-master-pages-vb/_static/image1.png)

**Şekil 01**: yeni bir klasör ve ASP.NET sayfası projeye eklendi


Ardından, güncelleştirme `Web.sitemap` dosyasını yeni bir içerecek şekilde `<siteMapNode>` bu ders için girişi. Aşağıdaki XML tam gösterir `Web.sitemap` şimdi üçüncü eklenmesi içeren biçimlendirme `<siteMapNode>` öğesi.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Yeni oluşturulan `Default.aspx` sayfa içinde dört ContentPlaceHolders için karşılık gelen dört içerik denetimleri olması gerektiğini `Site.master`. İçerik denetimi başvuruda bulunan bazı metin eklemek `MainContent` ContentPlaceHolder ve ardından bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 2'de görüldüğü gibi tarayıcı bulunamıyor `PoweredByASPNET.gif` görüntü dosyası. Burada neler olup bittiğini?

`~/Admin/Default.aspx` İçerik sayfası aynı HTML gönderilir `footerContent` haliyle bölge `About.aspx` sayfa:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Çünkü `<img>` öğenin `src` özniteliği bir göreli URL, aranacak tarayıcısı çalışır bir `Images` klasörüne web sayfasının klasör konumu görelidir. Tarayıcı için görüntü dosyası başka bir deyişle, arıyor `Admin/Images/PoweredByASPNET.gif`.


[![PoweredByASPNET.gif görüntü dosyası bulunamıyor](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Şekil 02**: `PoweredByASPNET.gif` görüntü dosyası bulunamadı ([tam boyutlu görüntüyü görüntülemek için tıklatın](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Göreli URL mutlak URL'ler ile değiştirme

Göreli bir URL tersidir bir *mutlak URL*, eğik çizgiyle başlayan bir olduğu (`/`) veya bir protokol gibi `http://`. Mutlak bir URL bilinen bir sabit noktasından bir kaynağın konumu belirttiğinden, aynı mutlak URL Web sitesinin klasör yapısını web sayfasının konumda bağımsız olarak herhangi bir web sayfasında geçersiz.

Şekil 2'de gösterilen kopuk resmi düzeltmek için güncelleştirmek ihtiyacımız `<img>` öğenin `src` göreli bir yerine mutlak bir URL kullanır, böylece özniteliği. Doğru mutlak URL belirlemek için Web sitenizin web sayfalarında birini ziyaret edin ve adres çubuğunu inceleyin. Şekil 2 Adres çubuğunda gösterildiği gibi web uygulamasının tam yolu olan `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Bu nedenle, güncelleştiriyoruz `<img>` öğenin `src` özniteliği ya da, aşağıdaki iki mutlak URL'ler:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Güncelleştirme için bir dakikanızı ayırın `<img>` öğenin `src` yukarıda gösterilen biçimlerden birini kullanarak mutlak URL özniteliğini ve ziyaret edip `~/Admin/Default.aspx` bir tarayıcı aracılığıyla sayfa. Bu süre tarayıcı doğru bir şekilde bulmak ve görüntülemek `PoweredByASPNET.gif` görüntü dosyası (bkz: Şekil 3).


[![Şimdi görüntülenen PoweredByASPNET.gif görüntüsüdür](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Şekil 03**: `PoweredByASPNET.gif` görüntüdür şimdi görüntülenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](urls-in-master-pages-vb/_static/image7.png))


Mutlak URL kodlamak çalışırken, sıkı bir şekilde, HTML Web sitesinin sunucu ve değişebilir klasör konumuna couples. Mutlak bir URL biçiminde kullanarak `http://localhost:3908/...` localhost önceki bağlantı noktası numarası Visual Studio'nun yerleşik ASP.NET Geliştirme Web sunucusu her başlatıldığında otomatik olarak seçilir kırılır olmasıdır. Benzer şekilde, `http://localhost` parçası olduğunda yalnızca geçerli yerel olarak test etme. Kod bir üretim ortamına dağıtıldığında, URL temel başka bir gibi değişir `http://www.yourserver.com`. Mutlak URL biçiminde `/ASPNET_MasterPages_Tutorial_04_VB/...` görmemeleri bu uygulama yolu geliştirme ve üretim sunucuları arasında değiştiğinden de aynı brittleness düşebilir.

İyi haber, ASP.NET çalışma zamanında geçerli bir göreli URL oluşturmak için bir yöntem sunar ' dir.

## <a name="usingandresolveclienturl"></a>Kullanarak`~`ve`ResolveClientUrl`

Bunun yerine mutlak bir URL sabit kod daha tilde kullanmak sayfa geliştiricilerin ASP.NET sağlar (`~`) web uygulamasının kök belirtmek için. Örneğin, bu öğreticide daha önce gösterimi kullandım `~/Admin/Default.aspx` başvurmak için metin `Default.aspx` sayfasındaki `Admin` klasör. `~` Belirten `Admin` web uygulamasının kökünün alt klasördür.

`Control` Sınıfının [ `ResolveClientUrl` yöntemi](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) bir URL alır ve denetim yer aldığı web sayfası için uygun bir göreli URL değiştirir. Örneğin, arama `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` gelen `About.aspx` döndürür `Images/PoweredByASPNET.gif`. Buradan çağırma `~/Admin/Default.aspx`, ancak döndürür `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Tüm ASP.NET sunucu denetimleri türetin çünkü `Control` sınıfı, tüm sunucu denetimleri erişiminiz `ResolveClientUrl` yöntemi. Hatta `Page` sınıfı türer `Control` sınıfı, bu yöntem, ASP.NET sayfaları arka plan kodu sınıflardan doğrudan kullanabileceğiniz anlamına gelir.


### <a name="usingin-the-declarative-markup"></a>Kullanarak`~`tanımlayıcı biçimlendirme

Birkaç ASP.NET Web denetimleri URL ile ilgili özellikleri içerir: HyperLink denetiminin bir `NavigateUrl` özellik; denetiminin görüntü bir `ImageUrl` özellik; ve benzeri. İşlendiğinde, bu denetimlerin URL ile ilgili özellik değerlerine geçirmek `ResolveClientUrl`. Sonuç olarak, bu özellikler içeriyorsa bir `~` kök web uygulamasının belirtmek için URL geçerli bir göreli URL değiştirilecek.

Yalnızca ASP.NET sunucu denetimleri dönüştürme göz önünde bulundurmanız `~` URL ile ilgili özelliklerindeki. Varsa bir `~` statik HTML biçimlendirmede gibi görünen `<img src="~/Images/PoweredByASPNET.gif" />`, ASP.NET altyapısı gönderir `~` HTML içeriğini geri kalanı ile birlikte tarayıcıya. Tarayıcı varsayar `~` URL bir parçasıdır. Örneğin, tarayıcı işaretleme alırsa `<img src="~/Images/PoweredByASPNET.gif" />` adlı bir alt klasör olduğunu varsayar `~` bir alt klasörle `Images` görüntü dosyasını içeren `PoweredByASPNET.gif`.

Görüntüyü biçimlendirme düzeltmek için `Site.master`, varolan `<img>` bir ASP.NET Web resim denetimi ile öğe. Görüntü Web denetimin ayarlamak `ID` için `PoweredByImage`, kendi `ImageUrl` özelliğine `~/Images/PoweredByASPNET.gif`ve kendi `AlternateText` "ASP.NET tarafından desteklenen!" özelliği


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Ana sayfaya bu değişikliği yaptıktan sonra yeniden ziyaret `~/Admin/Default.aspx` yeniden sayfa. Bu süre `PoweredByASPNET.gif` görüntü dosyası sayfasında görünür (bkz: Şekil 3). Ne zaman görüntü denetimi Web çizilir, kullanır `ResolveClientUrl` çözümlemeye yöntemi kendi `ImageUrl` özellik değeri. İçinde `~/Admin/Default.aspx` `ImageUrl` uygun göreli bir URL HTML kaynak gösterir aşağıdaki kod parçası dönüştürülür:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Web URL tabanlı denetim özelliklerini kullanılan yanı sıra `~` çağrılırken de kullanılabilir `Response.Redirect` ve `Server.MapPath` yöntemleri, diğerlerinin yanı sıra. Ayrıca, `ResolveClientUrl` yöntemi çağrılabilir doğrudan bir ASP.NET ya da ana sayfanın bildirim temelli biçimlendirme, gerekirse; bkz [Fritz çoklu kare](https://www.pluralsight.com/blogs/fritz/)ın blog girdisi [kullanma `ResolveClientUrl` biçimlendirmede](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Ana sayfanın göreli URL kalan çözme

Ek olarak `<img>` öğesinde `footerContent` biz yalnızca sabit, ana sayfa uygulamamızla gerektiren bir daha göreli URL içerir. `topContent` Bölge içerir "Ana sayfa için gösteren öğreticileri," bağlantı `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Bu URL göreli olduğundan, kullanıcıya göndereceğiniz `Default.aspx` ziyaret içerik sayfasının klasörde sayfa. Bu bağlantı için her zaman noktası sağlamak için `Default.aspx` ihtiyacımız değiştirmek için kök klasöründe `<a>` bir köprü Web bir öğesiyle denetim biz kullanabilirsiniz `~` gösterimi.

Kaldırma `<a>` öğesi biçimlendirme ve onun yerine bir köprü denetim ekleyin. Köprü ayarlamak `ID` için `lnkHome`, kendi `NavigateUrl` özelliğine `~/Default.aspx`ve kendi `Text` özelliğini "Ana sayfa öğreticileri."


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

İşte bu kadar! Bu noktada tüm ana sayfamızı URL'lerinde düzgün klasörleri bakılmaksızın bir içerik sayfasını tarafından ana sayfa ve içerik sayfasını işlendiğinde dayalı bulunur.

### <a name="automatic-url-resolution-in-theheadsection"></a>Otomatik URL'sini çözümlemesinde`<head>`bölümü

İçinde [ *bir Site genelinde düzenini kullanarak ana sayfalar oluşturma* ](creating-a-site-wide-layout-using-master-pages-vb.md) eklediğimiz öğretici bir `<link>` için `Styles.css` dosyasını `<head>` bölge:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Sırada `<link>` öğenin `href` özniteliği göreli, uygun bir yol çalışma zamanında otomatik olarak dönüştürülür. Biz anlatıldığı gibi [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) öğretici, `<head>` bölgedir değiştirmek için sağlayan gerçekte bir sunucu tarafı denetimi işlendiğinde iç denetimlerinden içerikleri.

Bunu doğrulamak için yeniden ziyaret `~/Admin/Default.aspx` sayfasında ve tarayıcıya gönderilen HTML kaynağını görüntülemek. Aşağıdaki kod parçacığında gösterildiği gibi `<link>` öğenin `href` öznitelik için uygun bir göreli URL, otomatik olarak da değiştirildi `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Özet

Ana sayfalar çok sık bağlantılar, görüntüler ve URL aracılığıyla belirtilmelidir diğer dış kaynaklara içerir. İçerik sayfaları ve ana sayfa aynı klasörde mevcut olmayabilir olduğundan göreli URL'ler kullanarak abstain önemlidir. Sabit kodlanmış mutlak URL'ler kullanmak mümkün olsa da, bu nedenle sıkı bir şekilde yapılması mutlak bir URL için web uygulaması couples. Taşıma veya bir web uygulaması - dağıtılırken genellikle yaptığı gibi bir mutlak URL - değişirse geri dönün ve mutlak URL'ler güncelleştirme hatırlamak zorunda kalırsınız.

İdeal yaklaşım tilde kullanmaktır (`~`) uygulama kökü belirtmek için. URL ile ilgili Özellikler içeren ASP.NET Web denetimleri eşleme `~` çalışma zamanında uygulama kök dizini. Dahili olarak, Web denetimleri kullanın `Control` sınıfının `ResolveClientUrl` geçerli bir göreli URL üretmek için yöntem. Bu yöntem ortak ve her sunucu denetiminden kullanılabilir değildir (de dahil olmak üzere `Page` sınıfı), bunu programlı olarak, arka plan kodu sınıflardan gerekirse kullanabilirsiniz.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfalar](http://www.odetocode.com/Articles/419.aspx)
- [URL bir ana sayfa yeniden Temellendirme](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Kullanarak `ResolveClientUrl` biçimlendirme](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Önceki](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
[sonraki](control-id-naming-in-content-pages-vb.md)
