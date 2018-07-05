---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: Ana sayfalar (C#) URL'lerinde | Microsoft Docs
author: rick-anderson
description: İçerik sayfası farklı bir göreli dizine işlemlerle ana sayfa dosyası nedeniyle nasıl URL'lerde ana sayfa sonu yöneliktir. Yeniden Temellendirme sırasında görünüyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: dcbd066a55fed3dd6c79e8a091e61449f85531b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398394"
---
<a name="urls-in-master-pages-c"></a>URL'lerde ana sayfalar (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> İçerik sayfası farklı bir göreli dizine işlemlerle ana sayfa dosyası nedeniyle nasıl URL'lerde ana sayfa sonu yöneliktir. URL'ler aracılığıyla yeniden Temellendirme sırasında görünür ~ bildirim temelli söz dizimi ve ResolveUrl ve ResolveClientUrl programlı olarak kullanma. (Ayrıca bakın


## <a name="introduction"></a>Giriş

Tüm örneklerde şimdiye kadar içeriği ve ana sayfalar (Web sitesinin kök klasöründe) aynı klasörde bulunan gördük. Ancak ana ve içerik sayfaları aynı klasörde neden olmalıdır bir neden yoktur. Bu gibi durumlarda, içerik sayfalarını kesinlikle alt klasörler oluşturabilirsiniz. Benzer şekilde, oluşturacağınız bir `~/MasterPages/` sitenizin ana sayfalar yerleştirdiğiniz bir klasör.

İçeriği ve ana sayfalar farklı klasörlerde yerleştirme bir olası sorun, bozuk bir URL içerir. Ana sayfanın göreli URL'ler köprüler, görüntüleri veya diğer öğeleri içeriyorsa, bağlantı için farklı bir klasörde bulunan içerik sayfalarını geçersiz olacaktır. Bu öğreticide geçici çözümlerin yanı sıra bu sorunun kaynağını inceleyeceğiz.

## <a name="the-problem-with-relative-urls"></a>Göreli URL'ler ile ilgili sorun

Bir web sayfasındaki bir URL olduğu söylenir bir *göreli URL* gösterdiği için kaynak konumu Web sitesinin klasörü yapısı içinde web sayfasının konumu göreli ise. Önde gelen eğik çizgiyle başlamıyor herhangi bir URL (`/`) veya bir protokol (gibi `http://`) URL'sini içeren web sayfasını konumunu temel alarak tarayıcı tarafından çözümlendiğinden görelidir.

Örneğin, Web sitemizi sahip bir `~/Images/` tek bir görüntü dosyası klasörü `PoweredByASPNET.gif`. Ana sayfa dosyası `Site.master` sahip bir `<img>` öğesinde `footerContent` aşağıdaki işaretlemeyle bölgesi:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src` Öznitelik değeri `<img>` öğesi, göreli bir URL ile başlamıyor çünkü `/` veya `http://`. Kısacası, `src` öznitelik değeri, aranacak tarayıcı söyler `Images` adlı bir dosya için alt `PoweredByASPNET.gif`.

Bir içerik sayfasını ziyaret ederek, yukarıdaki biçimlendirme doğrudan tarayıcıya gönderilir. Ziyaret etmek için birkaç dakikanızı `About.aspx` ve tarayıcıya gönderilen HTML kaynağını görüntülemek. Ana sayfanın tam aynı işaretlemede tarayıcıya gönderilip gönderilmediğini de bulabilirsiniz.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

İçerik sayfası kök klasöründe ise (olduğu gibi `About.aspx`) her şey olmadığı için beklendiği gibi çalışır bir `Images` kök klasörüne göreli alt klasör. Farklı bir klasöre ana sayfadan içerik sayfası varsa ancak şeyler parçalara ayırın. Bunu açıklamak üzere; adlı bir alt klasör oluşturmak `Admin`. Ardından, bir içerik sayfasının adlandırılmış ekleme `Default.aspx` için `Admin` yeni sayfaya bağlamak sağlamaktan klasöründe `Site.master` ana sayfa.

> [!NOTE]
> İçinde [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) adlı bir özel taban sayfası sınıfı oluşturduk öğretici `BasePage` içerik sayfasının başlığı otomatik olarak ayarlanır (varsa, açıkça atanan değil). Yeni oluşturulan sayfa arka plan kod sınıfı türetilen unutmayın `BasePage` böylece bu işlevleri kullanabilir.


Bu içerik sayfası oluşturduktan sonra çözüm Gezgininizde Şekil 1'e benzer görünmelidir.


![Yeni klasör ve ASP.NET sayfası projeye eklenmiş olan](urls-in-master-pages-cs/_static/image1.png)

**Şekil 01**: yeni bir klasör ve ASP.NET sayfası projeye eklenmiş olan


Ardından, güncelleştirme `Web.sitemap` yeni bir dosyaya `<siteMapNode>` bu dersin girişi. Aşağıdaki XML tam gösterir `Web.sitemap` artık üçüncü eklenmesini de içeren bir biçimlendirme `<siteMapNode>` öğesi.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Yeni oluşturulan `Default.aspx` sayfa içinde dört ContentPlaceHolder karşılık gelen dört içerik denetimleri olmalıdır `Site.master`. İçerik denetimi başvuru için metin ekleyin `MainContent` ContentPlaceHolder ve sonra da bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 2 gösterildiği gibi tarayıcı bulunamıyor `PoweredByASPNET.gif` görüntü dosyası. Ne anlama geliyor?

`~/Admin/Default.aspx` İçerik sayfası aynı HTML gönderilir `footerContent` haliyle bölge `About.aspx` sayfası:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Çünkü `<img>` öğenin `src` özniteliği bir göreli URL, aranacak tarayıcısı çalışır bir `Images` klasörüyle ilgili web sayfasının klasör konumu. Tarayıcı için resim dosyası başka bir deyişle, arayan `Admin/Images/PoweredByASPNET.gif`.


[![PoweredByASPNET.gif resim dosyası bulunamıyor](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Şekil 02**: `PoweredByASPNET.gif` görüntü dosyası bulunamıyor. ([tam boyutlu görüntüyü görmek için tıklatın](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Mutlak URL'ler ile göreli URL'leri değiştirme

Bir göreli URL tersidir bir *mutlak URL*, eğik çizgiyle başlayan bir olduğu (`/`) veya bir protokol gibi `http://`. Bilinen bir sabit noktaya ait kaynak konumunu mutlak bir URL belirttiğinden aynı mutlak URL Web sitesinin klasörü yapısı içinde web sayfasının konumdan bağımsız olarak herhangi bir web sayfası geçersiz.

Şekil 2'de gösterilen bozuk görüntü sorunu gidermek için güncelleştirilecek ihtiyacımız `<img>` öğenin `src` göreli bir yerine mutlak bir URL kullanır, böylece öznitelik. Doğru mutlak URL belirlemek için sitenizin web sayfalarını ziyaret edin ve adres çubuğuna inceleyin. Şekil 2 Adres çubuğunda gösterildiği gibi web uygulamasının tam yolu olan `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Bu nedenle, biz güncelleştirebilir `<img>` öğenin `src` özniteliği ya da aşağıdaki iki mutlak URL'ler için:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Güncelleştirmek için bir dakikanızı ayırın `<img>` öğenin `src` yukarıda gösterilen biçimlerden birini kullanarak mutlak URL özniteliğini ve ziyaret edip `~/Admin/Default.aspx` tarayıcısından sayfası. Tarayıcı doğru bir şekilde bulmak ve görüntülemek, bu kez `PoweredByASPNET.gif` görüntü dosyası (bkz: Şekil 3).


[![Şimdi görüntülenen PoweredByASPNET.gif görüntüsüdür](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Şekil 03**: `PoweredByASPNET.gif` görüntü hizmetidir şimdi görüntülenen ([tam boyutlu görüntüyü görmek için tıklatın](urls-in-master-pages-cs/_static/image7.png))


Mutlak URL kodlamak çalışırken, sıkı bir şekilde HTML Web sitesinin sunucu ve değişebilir klasör konumu, couples. Formun mutlak bir URL kullanarak `http://localhost:3908/...` kırılır olduğu için önceki bağlantı noktası numarası `localhost` Visual Studio'nun yerleşik ASP.NET Geliştirme Web sunucusu her başlatıldığında otomatik olarak seçilir. Benzer şekilde, `http://localhost` bölümü, yalnızca geçerli yerel olarak test ederken. Kodu bir üretim sunucusuna dağıtıldıktan sonra URL'si temeli başka bir şeye gibi değişecektir `http://www.yourserver.com`. Mutlak bir URL biçiminde `/ASPNET_MasterPages_Tutorial_04_CS/...` aktardığınızda genellikle bu uygulama yolu geliştirme ve üretim sunucuları arasında değiştiğinden de aynı brittleness olumsuz etkilenir.

Güzel bir haberimiz var ASP.NET çalışma zamanı, geçerli bir göreli URL oluşturmak için bir yöntem sahip olmasıdır.

## <a name="usingandresolveclienturl"></a>Kullanarak`~`ve`ResolveClientUrl`

Bunun yerine sabit bir mutlak URL kod daha tilde kullanmak sayfa geliştiricilerin ASP.NET sağlar (`~`) web uygulamasının kökünü belirtmek için. Örneğin, bu öğreticide daha önce bu gösterim kullandım `~/Admin/Default.aspx` başvurmak için metin `Default.aspx` sayfasını `Admin` klasör. `~` Belirten `Admin` web uygulamasının kök alt klasörüdür.

`Control` Sınıfın [ `ResolveClientUrl` yöntemi](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) bir URL alır ve denetimi yer aldığı web sayfası için uygun bir göreli URL değiştirir. Örneğin, çağırma `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` gelen `About.aspx` döndürür `Images/PoweredByASPNET.gif`. Buradan çağırma `~/Admin/Default.aspx`, ancak döndürür `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Tüm ASP.NET sunucu denetimleri türetmek için `Control` sınıfı, tüm sunucu denetimleri erişiminiz `ResolveClientUrl` yöntemi. Hatta `Page` sınıf türetilir `Control` sınıfı, bu yöntem, ASP.NET sayfalarının arka plan kod sınıflardan doğrudan kullanabileceğiniz anlamına gelir.


### <a name="usingin-the-declarative-markup"></a>Kullanarak`~`tanımlayıcı biçimlendirme

Çeşitli ASP.NET Web denetimleri URL ile ilgili özellikleri içerir: HyperLink denetim sahip bir `NavigateUrl` özelliği; görüntü denetiminin bir `ImageUrl` özelliği; ve benzeri. İşlendiğinde, bu denetimler için URL ile ilgili özellik değerlerini geçirmek `ResolveClientUrl`. Sonuç olarak, bu özellikleri içeren bir `~` web uygulamasının kökünü belirtmek için URL'nin geçerli bir göreli URL değiştirilecek.

ASP.NET sunucu denetimleri dönüştüren göz önünde bulundurun `~` kendi URL ile ilgili özellikleri. Varsa bir `~` statik HTML biçimlendirmeyi aşağıdaki gibi görünür `<img src="~/Images/PoweredByASPNET.gif" />`, ASP.NET altyapısı gönderir `~` tarayıcıya HTML içeriği geri kalanıyla birlikte. Tarayıcı varsayar `~` URL'nin bir parçasıdır. Örneğin, tarayıcıyı işaretleme alırsa `<img src="~/Images/PoweredByASPNET.gif" />` adlı bir alt klasör olduğunu varsayar `~` bir alt klasörle `Images` görüntü dosyasını içeren `PoweredByASPNET.gif`.

Görüntü biçimlendirme içinde düzeltmek için `Site.master`, varolan `<img>` bir ASP.NET Web resim denetimi ile öğe. Görüntü Web denetimin ayarlamak `ID` için `PoweredByImage`, kendi `ImageUrl` özelliğini `~/Images/PoweredByASPNET.gif`ve onun `AlternateText` "ASP.NET tarafından desteklenen!" özelliği


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Ana sayfaya bu değişikliği yaptıktan sonra yeniden ziyaret `~/Admin/Default.aspx` yeniden sayfa. Bu süre `PoweredByASPNET.gif` görüntü dosyası sayfasında görünür (bkz: Şekil 3). Ne zaman Web resim denetimi işlenen, kullandığı `ResolveClientUrl` çözümlenecek yöntemi kendi `ImageUrl` özellik değeri. İçinde `~/Admin/Default.aspx` `ImageUrl` HTML kaynak programlarını aşağıdaki kod parçacığı ilgili göreli bir URL dönüştürülür:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> URL tabanlı Web denetim özelliklerini kullanılan yanı sıra `~` çağırırken de kullanılabilir `Response.Redirect` ve `Server.MapPath` yöntemleri, diğerlerinin yanı sıra. Ayrıca, `ResolveClientUrl` yöntemi çağrılabilir doğrudan bir ASP.NET ya da ana sayfanın bildirim temelli biçimlendirme, gerekirse; bkz [Fritz çoklu kare](https://www.pluralsight.com/blogs/fritz/)ın blog girişine [kullanma `ResolveClientUrl` biçimlendirmede](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Ana sayfanın göreli URL'ler kalan düzeltme

Ek olarak `<img>` öğesinde `footerContent` yalnızca düzelttik, ana sayfa uygulamamızla gerektirir daha göreli bir URL içerir. `topContent` Bölge içeren "Ana sayfa işaret öğreticiler," bağlantı `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Bu URL, göreli olduğundan, bu kullanıcıya gönderecek `Default.aspx` içerik sayfasını ziyaret klasöründe sayfası. Bu bağlantı için her zaman noktası olmasını `Default.aspx` ihtiyacımız değiştirmek için kök klasöründe `<a>` köprü Web öğesi denetim kullanabiliriz `~` gösterimi.

Kaldırma `<a>` öğenin biçimlendirmesi ve bunun yerine bir HyperLink denetimi ekleyebilirsiniz. Köprü ayarlamak `ID` için `lnkHome`, kendi `NavigateUrl` özelliğini `~/Default.aspx`ve onun `Text` özelliğini "Ana sayfa öğreticiler."


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

İşte bu kadar! Bu noktada tüm ana sayfamızı URL'lerinde düzgün klasörleri bağımsız olarak içerik sayfası ile ana sayfa ve içerik sayfa işlendiğinde dayalı olarak yer alır.

### <a name="automatic-url-resolution-in-theheadsection"></a>Otomatik URL'sini çözümlemesinde`<head>`bölümü

İçinde [ *bir Site genelinde düzenini kullanarak ana sayfa oluşturma* ](creating-a-site-wide-layout-using-master-pages-cs.md) öğreticide eklediğimiz bir `<link>` için `Styles.css` dosyası `<head>` bölgesi:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Sırada `<link>` öğenin `href` özniteliktir göreli, otomatik olarak uygun bir yolu, çalışma zamanında dönüştürülür. Açıkladığımız gibi [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) öğreticide `<head>` bölgedir değiştirmenizi sağlayan gerçekten sunucu tarafı denetimlerdir, işlendiğinde, iç denetimlerinin içeriği.

Bunu doğrulamak için yeniden ziyaret `~/Admin/Default.aspx` sayfasında ve tarayıcıya gönderilen HTML kaynağını görüntülemek. Aşağıdaki kod parçacığında gösterildiği gibi `<link>` öğenin `href` özniteliği için uygun bir göreli URL, otomatik olarak da değiştirilmiş `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Özet

Ana sayfalar, bağlantılar, resimler ve URL ile belirtilen diğer dış kaynaklara çok sık içerir. Ana sayfa ve içerik sayfalarındaki aynı klasörde mevcut olmayabilir çünkü göreli URL'ler kullanarak abstain önemlidir. Sabit kodlanmış mutlak URL'ler kullanmak mümkün olsa da, bu nedenle sıkı bir şekilde yapılması web uygulaması için mutlak URL couples. Taşıma veya bir web uygulaması - dağıtılırken sık yaptığı gibi bir mutlak URL - değişirse geri dönün ve mutlak URL'ler güncelleştirme hatırlamak zorunda kalırsınız.

İdeal yaklaşım tilde kullanmaktır (`~`) uygulama kökünü belirtmek için. Eşleme URL ile ilgili özellikleri içeren ASP.NET Web denetimleri `~` için çalışma zamanında uygulama kökü. Dahili olarak, Web denetimleri kullanın `Control` sınıfın `ResolveClientUrl` geçerli bir göreli URL üretmek için yöntemi. Bu yöntem, genel ve her sunucu denetimi kullanılabilir (dahil olmak üzere `Page` sınıfı), bu nedenle onu programlı bir şekilde, arka plan kod sınıflardan gerekirse kullanabilirsiniz.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfaları](http://www.odetocode.com/Articles/419.aspx)
- [URL bir ana sayfasında ReBase işlemi gerçekleştiriliyor.](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Kullanarak `ResolveClientUrl` biçimlendirme](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [İleri](control-id-naming-in-content-pages-cs.md)
