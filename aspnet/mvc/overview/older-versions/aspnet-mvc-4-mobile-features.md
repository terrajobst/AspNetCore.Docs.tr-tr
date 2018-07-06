---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: ASP.NET MVC 4 mobil özellikler | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide bir ASP.NET MVC 5 mobil Web uygulamasını Azure Web Siteleri'nde dağıtma, kod örnekleri içeren bir MVC 5 sürümü artık yoktur.
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: c852f4a853d14badb6c9a1c2c1ddb7b069bc3441
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806593"
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 mobil özellikler
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Şimdi bir kod örneği bu öğreticiyle MVC 5 sürümü yoktur [bir ASP.NET MVC 5 mobil Web uygulamasını Azure Web Siteleri'nde dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Bu öğreticide bir ASP.NET MVC 4 Web uygulamasındaki mobil özelliklerle çalışmaya ilişkin temel bilgileri sağlanır. Bu öğretici için kullandığınız [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) veya Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer veya VWD&quot;). Bu zaten varsa, Visual Studio professional sürümü kullanabilirsiniz.

Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (önerilir) veya Visual Studio Web Developer Express SP1. Visual Studio 2012, ASP.NET MVC 4 içerir. Visual Web Developer 2010 kullanıyorsanız, yüklemelisiniz [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Bir mobil tarayıcı öykünücüsü de gerekir. Aşağıdakilerden herhangi biri çalışır:

- [Windows 7, Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Ekran görüntüleri çoğunda Bu öğreticide kullanılan öykünücünün budur.)
- İPhone benzetmek için kullanıcı aracısı dizesi değiştirin. Bkz: [bu](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) blog girişi.
- [Opera Mobile öykünücüsü](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) İphone'a ayarlamak kullanıcı aracısı. Kullanıcı Aracısı, "iPhone" için Safari'ye ayarlama konusunda yönergeler için bkz: [IE olduğu anlatabilirsiniz Safari sağlama](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David geçiş'ın blogunda.

Visual Studio projeleri C# kaynak kodu ile bu konuya eşlik etmek üzere kullanılabilir:

- [Başlangıç projesi indirme](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Proje indirme tamamlandı](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Ne oluşturacaksınız

Bu öğretici için sağlanan Basit konferans listeleme uygulamaya mobil özellikler ekleyeceksiniz [başlangıç projesini](https://go.microsoft.com/fwlink/?LinkId=228307). Aşağıdaki ekran görüntüsünde görüldüğü gibi tamamlanmış uygulamanın etiketler sayfasını gösterir [Windows 7, Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Bkz: [klavye eşleme için Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) klavye girişi basitleştirmek için.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Internet Explorer 9 veya 10, FireFox veya Chrome'un ayarlayarak mobil uygulamanızı geliştirmek için kullanabileceğiniz [kullanıcı aracısı dizesi](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). İPhone öykünen Internet Explorer'ı kullanarak tamamlanmış öğretici aşağıdaki resimde gösterilmektedir. Internet Explorer F-12 geliştirici araçlarını kullanabilirsiniz ve [Fiddler aracı](http://www.fiddler2.com/fiddler2/) uygulamanızda hata ayıklama yardımcı olmak için.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- ASP.NET MVC 4 şablonlar HTML5 kullanma `viewport` özniteliği ve geliştirmek için Uyarlamalı işleme mobil cihazlarda görüntüleme.
- Mobile özgü görünümler oluşturma
- Görünüm değiştirici sağlar'bir mobil Görünüm ve bir masaüstü uygulama görünümü arasında geçiş yapma kullanıcılar oluşturma

### <a name="getting-started"></a>Başlarken

Konferans listeleme uygulama için aşağıdaki bağlantıyı kullanarak başlangıç projesini indirin: [indirme](https://go.microsoft.com/fwlink/?LinkId=228307). Ardından Windows Gezgini'nde sağ tıklayarak *MvcMobile.zip* seçin ve dosya **özellikleri**. İçinde **MvcMobile.zip özellikleri** iletişim kutusunda **Engellemeyi Kaldır** düğmesi. (Engellemesinin kaldırılması kullanmaya çalıştığınızda oluşan bir güvenlik uyarısı engelleyen bir *.zip* Web'den indirdiğiniz dosyayı.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Sağ *MvcMobile.zip* seçin ve dosya **tümünü Ayıkla** dosyasının sıkıştırması. Visual Studio'da açın *MvcMobile.sln* dosya.

Masaüstü tarayıcınızda görüntülenir uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Mobil tarayıcı öykünücüyü başlatın, öykünücü konferans uygulama için URL'yi kopyalayın ve ardından **etikete göre tara** bağlantı. Windows Phone öykünücüsü kullanıyorsanız, URL çubuğunda ve klavye erişim elde etmek için duraklatma tuşuna basın. Gösterir aşağıdaki resim *AllTags* görünümü (seçme gelen **etikete göre tara**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Mobil cihazda çok okunabilir görüntülenir. ASP.NET bağlantısını seçin.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET etiketi çok derli toplu bir görünümdür. Örneğin, **tarih** sütundur oldukça zor okunmasına. Bir sürümünü oluşturun öğreticinin ilerleyen bölümlerinde *AllTags* için özellikle mobil tarayıcılar ve, yapacak görünen okunabilir görünümü.

Not: Şu anda, mobil önbelleğe alma altyapısı bir hata bulunmaktadır. Üretim uygulamaları için yüklemelisiniz [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget paket. Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hatası düzeltildi](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için.

## <a name="css-media-queries"></a>CSS medya sorguları

[CSS medya sorguları](http://www.w3.org/TR/css3-mediaqueries/) medya türleri için CSS için bir uzantıdır. Özel tarayıcı (kullanıcı aracısı) için varsayılan CSS kurallarını geçersiz kıl kurallar oluşturmanıza olanak sağlar. Mobil tarayıcılar hedefleyen CSS için genel bir kural, en yüksek ekran boyutu tanımlamaktır. *Content\Site.css* yeni bir ASP.NET MVC 4 Internet projesi oluşturduğunuzda oluşturulan dosyası aşağıdaki medya sorgusu içerir:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Tarayıcı penceresini 850 piksel genişliğinde ya da daha az ise, bu ortam bloğu içinde CSS kurallarını kullanır. Masaüstü tarayıcıları için daha geniş görüntüleyen tasarlanmış varsayılan CSS kurallarını daha küçük tarayıcılarda (gibi mobil tarayıcılar) daha iyi bir HTML içeriğinin görüntülenmesini sağlamak için şunun gibi CSS medya sorguları kullanabilirsiniz.

## <a name="the-viewport-meta-tag"></a>Görünüm penceresi Meta etiketi

Çoğu mobil Tarayıcı tanımlamak sanal tarayıcı penceresi genişliğini ( *Görünüm penceresi*) mobil cihazın gerçek genişliğinden çok büyük. Bu sanal görüntü içinde tüm web sayfasını uyacak şekilde mobil tarayıcılar sağlar. Kullanıcılar ardından ilginç içerikler üzerinde yakınlaştırma yapabilirsiniz. Görünüm penceresinin genişliğini gerçek cihaz genişliğine ayarlarsanız, mobil tarayıcı içeriği uygun olduğundan ancak hiçbir yakınlaştırma gereklidir.

Görünüm penceresinin `<meta>` ASP.NET MVC 4 Düzen dosyası etiketinde cihaz genişliğine görünüm penceresinin ayarlar. Görünüm penceresi aşağıdaki satırı gösterir `<meta>` ASP.NET MVC 4 Düzen dosyası etiketi.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>CSS medya sorgu ve Görünüm penceresi Meta etiketi etkisini İnceleme

Açık *görünümler/paylaşılan\\_Layout.cshtml* dosya düzenleyicide ve görünüm penceresinin yorum `<meta>` etiketi. Aşağıdaki biçimlendirmede dışı bırakılan satır gösterir.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Açık *MvcMobile\Content\Site.css* Düzenleyicisi'nde dosya ve medya sorgusu maksimum genişliği sıfır piksel olarak değiştirin. Bu, CSS kurallarını mobil tarayıcılarda kullanılmasını engeller. Aşağıdaki satırı değiştirilmiş medya sorgusu gösterilmektedir:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Yaptığınız değişiklikleri kaydedin ve bir mobil tarayıcı öykünücü konferans uygulamasına göz atın. Aşağıdaki görüntüde küçük metin görünüm penceresinin kaldırma sonucudur `<meta>` etiketi. Hiçbir Görünüm penceresi ile `<meta>` etiketi, tarayıcı olan uzaklaştırmaysa varsayılan görünüm penceresi genişliğe (850 piksel veya daha geniş çoğu mobil tarayıcılar için.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Değişikliklerinizi geri — görünüm penceresinin açıklamadan çıkarın `<meta>` etiketi Düzen dosyasında ve ortam sorgusu 850 piksel cinsinden geri *Site.css* dosya. Değişikliklerinizi kaydetmek ve mobil aygıt dostu görünen geri yüklendiğini doğrulamak için mobil tarayıcıyı yenileyin.

Görünüm penceresinin `<meta>` etiketi ve CSS medya sorgusu ASP.NET MVC 4'e özgü değildir ve herhangi bir web uygulamasında bu özelliklerden yararlanabilirsiniz. Ancak, artık yeni bir ASP.NET MVC 4 projesi oluşturduğunuzda, oluşturulan bir dosya oluşturulur.

Görünüm penceresi hakkında daha fazla bilgi için `<meta>` etiketlemek için bkz: [iki viewports'ın bir hikayesini — İkinci bölüm](http://www.quirksmode.org/mobile/viewports2.html).

Sonraki bölümde, mobil tarayıcı belirli görünümler sağlamak nasıl öğreneceksiniz.

## <a name="overriding-views-layouts-and-partial-views"></a>Kısmi görünümler görünümleri ve düzenleri geçersiz kılma

ASP.NET MVC 4'te yeni bir önemli özelliği (düzenleri ve kısmi görünümler dahil) herhangi bir görünümü geçersiz kılmak için genel olarak, tek bir mobil tarayıcı veya herhangi bir tarayıcıya mobil tarayıcılar olanak tanıyan basit bir mekanizmadır. Mobile özgü görünüm sağlamak için bir görünüm dosyası kopyalayabilir ve ekleme *. Mobil* dosya adı. Örneğin, bir mobil oluşturmak için *dizin* görüntüleme, kopyalama *Views\Home\Index.cshtml* için *Views\Home\Index.Mobile.cshtml*.

Bu bölümde, mobil özel düzen dosyası oluşturacaksınız.

Başlamak için kopyalama *görünümler/paylaşılan\\_Layout.cshtml* için *görünümler/paylaşılan\\_Layout.Mobile.cshtml*. Açık  *\_Layout.Mobile.cshtml* başlığı değiştirip **MVC4 konferans** için **konferansı (mobil)**.

Her `Html.ActionLink` çağrısı, "her bağlantısını gözatma türü" kaldırma *ActionLink*. Aşağıdaki kod, mobil Düzen dosyası tamamlanmış gövde bölümünü gösterir.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopyalama *Views\Home\AllTags.cshtml* dosyasını *Views\Home\AllTags.Mobile.cshtml*. Yeni dosyayı açın ve değiştirmek `<h2>` "Tags" öğesinden için "etiketleri (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Bir masaüstü tarayıcısı kullanarak ve mobil tarayıcı öykünücüsü'nü etiketleri sayfasına göz atın. Mobil tarayıcı öykünücü yaptığınız iki değişiklik gösterir.

[![p2m_layoutTags.Mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Buna karşılık, masaüstü görüntü değiştirilmemiştir.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Tarayıcı özel görünümler

Mobil ve Masaüstü özgü görünümler ek olarak, ayrı bir tarayıcı için görünümler oluşturabilirsiniz. Örneğin, özellikle iPhone tarayıcısı görünümlerini oluşturabilirsiniz. Bu bölümde, bir düzen iPhone tarayıcısı ve bir iPhone sürümü için oluşturursunuz *AllTags* görünümü.

Açık *Global.asax* dosyasını açıp aşağıdaki kodu ekleyin `Application_Start` yöntemi.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Bu kod, "her gelen istek karşı eşleştirilir iPhone" adlı yeni bir görüntü modu tanımlar. Gelen istek (diğer bir deyişle, kullanıcı aracısı dizesi "iPhone" içeriyorsa) tanımladığınız koşul eşleşirse, ASP.NET MVC görünümleri, adı "iPhone" son eki içeren görünecektir.

Kod içinde sağ `DefaultDisplayMode`, seçin **çözmek**ve ardından `using System.Web.WebPages;`. Bu bir başvuru ekler `System.Web.WebPages` yerdir ad alanı `DisplayModes` ve `DefaultDisplayMode` türleri tanımlanmıştır.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternatif olarak, aşağıdaki satırı yalnızca el ile ekleyebileceğiniz `using` dosyasının bölümü.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Tam içeriğini *Global.asax* dosya aşağıda gösterilmektedir.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Değişiklikleri kaydedin. Kopyalama *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosyasını *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Yeni dosyayı açın ve ardından değiştirmek `h1` gelen başlık `Conference (Mobile)` için `Conference (iPhone)`.

Kopyalama *MvcMobile\Views\Home\AllTags.Mobile.cshtml* dosyasını *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Yeni dosyasında değiştirmek `<h2>` gelen "etiketleri (M)" öğesini "Etiketleri (iPhone)".

Uygulamayı çalıştırın. Bir mobil tarayıcı öykünücüyü çalıştırmak, kullanıcı aracısı, "iPhone" için ayarlandığından emin olun ve göz atın *AllTags* görünümü. Aşağıdaki ekran görüntüsü gösterildiği *AllTags* görünümü oluşturulur [Safari](http://www.apple.com/safari/download/) tarayıcı. Safari için Windows indirebileceğiniz [burada](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Bu bölümde mobil düzenleri ve görünümlerin nasıl oluşturulacağı ve düzeni ve görünümleri iPhone gibi belirli cihazlar için nasıl oluşturulacağı gördük. Sonraki bölümde nasıl yararlanacağınızı daha fazla ilgi çekici mobil görünümler için jQuery Mobile görürsünüz.

## <a name="using-jquery-mobile"></a>JQuery Mobile'ı kullanma

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) kitaplığı tüm büyük mobil tarayıcılar çalışır bir kullanıcı arabirimi çerçevesi sağlar. jQuery Mobile geçerlidir *kademeli geliştirmeyi* CSS ve JavaScript destekleyen mobil tarayıcılar için. Kademeli geliştirmeyi daha güçlü tarayıcılar ve cihazlar için daha zengin bir görüntü verirken temel bir web sayfası içeriği görüntülemek kullanılan tüm tarayıcılar sağlar. JQuery Mobile ile birlikte JavaScript ve CSS dosyaları birçok öğe biçimlendirme değişiklik yapmadan mobil tarayıcılar uyacak şekilde stili.

Bu bölümde yüklersiniz *jQuery.Mobile.MVC* NuGet paketine, jQuery Mobile ve bir görünüm değiştirici pencere öğesi yükler.

Başlamak için silme *paylaşılan\\_Layout.Mobile.cshtml* ve *paylaşılan\\_Layout.iPhone.cshtml* daha önce oluşturduğunuz dosyaları.

Yeniden adlandırma *Views\Home\AllTags.Mobile.cshtml* ve *Views\Home\AllTags.iPhone.cshtml* dosyaları *Views\Home\AllTags.iPhone.cshtml.hide* ve  *Views\Home\AllTags.Mobile.cshtml.Hide*. Artık dosyalarınız olduğundan bir *.cshtml* uzantısını işlemek için ASP.NET MVC çalışma zamanı tarafından bunlar kullanılmayacak *AllTags* görünümü.

Yükleme *jQuery.Mobile.MVC* bunu yaparak, NuGet paketi:

1. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. İçinde **Paket Yöneticisi Konsolu**, girin `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Aşağıdaki görüntüde, eklenen ve MvcMobile projeye NuGet jQuery.Mobile.MVC paketi tarafından değiştirilen dosyaları gösterir. [Add] sonra dosya adını verdiğinizden eklenen dosyalar. GIF resim göstermez ve PNG dosyaları eklenen *Content\images* klasör.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

JQuery.Mobile.MVC NuGet paket aşağıdakileri yükler:

- *Uygulama\_Start\BundleMobileConfig.cs* eklenen jQuery JavaScript ve CSS dosyalarına başvurmak için gerekli olan bir dosya. Aşağıdaki yönergeleri izleyin ve bu dosyada tanımlanan mobil paket başvuru gerekir.
- jQuery Mobile CSS dosyaları.
- A `ViewSwitcher` denetleyicisi pencere öğesi (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript dosyaları.
- JQuery Mobile biçimlendirilmiş Düzen dosyası (*görünümler/paylaşılan\\_Layout.Mobile.cshtml*).
- Görünüm değiştirici kısmi Görünüm *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) Masaüstü görünümden mobil görünüme ve geçiş yapmak için her sayfanın üst kısmındaki bir bağlantı sağlar.
- Birkaç<em>.png</em> ve <em>.gif</em> görüntü dosyaları <em>Content\images</em> klasör.

Açık *Global.asax* son satırının aşağıdaki kodu ekleyin ve dosya `Application_Start` yöntemi.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Aşağıdaki kod tam gösterir *Global.asax* dosya.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Internet Explorer 9 kullanıyorsanız ve görmüyorsanız `BundleMobileConfig` sarı Vurgu satır yukarıda, tıklayın [uyumluluk görüntüle düğmesi](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![(Kapalı) Uyumluluk Görünümü düğmesinin resmi] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " (Kapalı) Uyumluluk Görünümü düğmesinin resmi") simge bir anahat Değiştir olun IE'de ![(Kapalı) Uyumluluk Görünümü düğmesinin resmi](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(Kapalı) Uyumluluk Görünümü düğmesinin resmi ") düz renk için ![(açık) Uyumluluk Görünümü düğmesinin resmi](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(açık) Uyumluluk Görünümü düğmesinin resmi"). Alternatif olarak Bu öğretici, FireFox veya Chrome'un içinde görüntüleyebilirsiniz.


Açık *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosyasını açıp aşağıdaki biçimlendirme doğrudan sonra Ekle `Html.Partial` çağırın:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Tam *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Uygulamayı derlemek ve mobil tarayıcı öykünücüsü'nde göz atın *AllTags* görünümü. Aşağıdakilere bakın:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Mobil belirli kodun hatalarını ayıklayabilir [kullanıcı aracısı dizesi ayarı](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) IE veya Chrome iPhone ve ardından F-12 geliştirme araçlarını kullanarak. Mobil tarayıcınızda görüntülenmiyorsa **giriş**, **Konuşmacı**, **etiketi**, ve **tarih** düğmeleri olarak bağlantılar, jQuery Mobile başvuruları betik ve CSS dosyaları doğru olmayabilir.


Stil değişikliklerin yanı sıra gördüğünüz **mobil görünümde görüntüleniyor** ve mobil görünümünden Masaüstü görünümüne olanak sağlayan bir bağlantı. Seçin **Masaüstü Görünüm** bağlantı ve Masaüstü görünümünde görüntülenir.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Masaüstü görünüm doğrudan mobil görünüme gitmek için bir yol sağlamaz. Şimdi, çözeceksiniz. Açık *görünümler/paylaşılan\\_Layout.cshtml* dosya. Yalnızca sayfanın altındaki `body` öğesi, görünüm değiştirici pencere işleyen aşağıdaki kodu ekleyin:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Yenileme *AllTags* mobil tarayıcıda görüntüle. Artık, masaüstü ve mobil görünümler arasında gezinebilirsiniz.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Hata ayıklama Not: görünümler/paylaşılan sonuna aşağıdaki kodu ekleyebilirsiniz\\_ViewSwitcher.cshtml tarayıcı kullanıcı aracısı dizesi kullanarak bir mobil cihaza ayarladığınızda görünümleri hata ayıklama yardımcı olmak için.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  ve aşağıdaki başlığı ekleme *görünümler/paylaşılan\\_Layout.cshtml* dosya.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Gözat *AllTags* Masaüstü tarayıcısında sayfa. Görünüm değiştirici pencere öğesi, yalnızca mobil Düzen sayfaya eklendiğinden Masaüstü tarayıcısında görüntülenmez. Görünüm değiştirici pencere öğesi Masaüstü görünümüne nasıl ekleyebileceğiniz, öğreticinin ilerleyen bölümlerinde görürsünüz.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Konuşmacı listesi iyileştirme

Mobil tarayıcıda seçin **konuşmacıları** bağlantı. Hiçbir mobil bir görünüm olduğundan (*AllSpeakers.Mobile.cshtml*), varsayılan konuşmacıları görüntüleme (*AllSpeakers.cshtml*) mobil düzeni görünümünde kullanılarak işlenir ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Ayarlayarak genel varsayılan (mobil olmayan) bir mobil düzen içinde işleme görünümünden devre dışı bırakabilirsiniz `RequireConsistentDisplayMode` için `true` içinde *görünümleri\\_ViewStart.cshtml* böyle bir dosya:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Zaman `RequireConsistentDisplayMode` ayarlanır `true`, mobil Düzen (<em>\_Layout.Mobile.cshtml</em>) yalnızca mobil görünümler için kullanılır. (Diğer bir deyişle, görünüm dosyası biçimindedir <em>** Görünümadı</em><em>. Mobile.cshtml</em>.) Ayarlamak isteyebilirsiniz `RequireConsistentDisplayMode` için `true` mobil düzen, mobil olmayan görünümlerle da işe yaramazsa. Gösterir aşağıdaki ekran görüntüsünde nasıl <em>konuşmacıları</em> sayfasını işler `RequireConsistentDisplayMode` ayarlanır `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Ayarlayarak bir görünümde tutarlı görüntü modu devre dışı bırakabilirsiniz `RequireConsistentDisplayMode` için `false` görünüm dosyası içinde. Aşağıdaki biçimlendirmede *Views\Home\AllSpeakers.cshtml* dosya kümelerini `RequireConsistentDisplayMode` için `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Bir mobil konuşmacıları görünümü oluşturma

Yalnızca anlatıldığı gibi *konuşmacıları* görünümü okunabilir olmakla birlikte, bağlantılar, küçük ve mobil cihazda dokunun zordur. Bu bölümde, mobil özgü oluşturacaksınız *konuşmacıları* modern mobil uygulama gibi görünüyor görünümü — büyük görüntüler, dokunun kolay bağlar ve konuşmacıları hızla bulmak için arama kutusu içerir.

Kopyalama *AllSpeakers.cshtml* için *AllSpeakers.Mobile.cshtml*. Açık *AllSpeakers.Mobile.cshtml* dosya ve kaldırma `<h2>` başlık öğesi.

İçinde `<ul>` etiketinde, ekleyin `data-role` özniteliği ve değerini ayarlamak `listview`. Gibi diğer [ `data-*` öznitelikleri](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` dokunmak büyük liste öğeleri daha kolay hale getirir. Tamamlanan biçimlendirmeye benzer budur:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Mobil tarayıcıyı yenileyin. Güncelleştirilmiş görünümü şöyle görünür:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Mobil görünüme geliştirmiştir olsa da, konuşmacı uzun listesi gitmek zordur. İçinde bu sorunu gidermek için `<ul>` etiketinde, ekleyin `data-filter` özniteliği ve değerini `true`. Aşağıdaki kod gösterir `ul` biçimlendirme.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Aşağıdaki resimde gösterilmektedir oluşur sayfanın üst kısmındaki arama filtre kutusuna `data-filter` özniteliği.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Her harf arama kutusuna yazarken, jQuery Mobile görüntülenen listeyi aşağıdaki resimde gösterildiği gibi filtreler.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Etiketler listesi iyileştirme

Gibi varsayılan *konuşmacıları* görünümü *etiketleri* görünümü okunabilir olmakla birlikte, bağlantılar, küçük ve zor bir mobil cihaza dokunun. Bu bölümde, sorunu düzeltmemiz *etiketleri* görüntülemek, sabit aynı şekilde *konuşmacıları* görünümü.

Kaldırma &quot;Gizle&quot; için soneki *Views\Home\AllTags.Mobile.cshtml.hide* adı, bu nedenle dosya *Views\Home\AllTags.Mobile.cshtml*. Yeniden adlandırılan dosyayı açın ve kaldırma `<h2>` öğesi.

Ekleme `data-role` ve `data-filter` özniteliklerini `<ul>` burada gösterildiği gibi etiketleyin:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Aşağıdaki görüntüde harfi filtreleme etiketler sayfasını gösterir `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Tarihler listesini geliştirme

Artırabilirsiniz *tarihleri* , geliştirilmiş gibi görüntülemek *konuşmacıları* ve *etiketleri* görünümleri, böylece mobil cihazda kullanmak daha kolaydır.

Kopyalama *Views\Home\AllDates.cshtml* dosyasını *Views\Home\AllDates.Mobile.cshtml*. Yeni dosyayı açın ve kaldırma `<h2>` öğesi.

Ekleme `data-role="listview"` için `<ul>` etiketine:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Ne aşağıdaki resimde gösterilmektedir **tarih** sayfanın nasıl göründüğüne ile `data-role` yerinde özniteliği.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) içeriklerini *Views\Home\AllDates.Mobile.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Bu kod tüm oturumları günlerine göre gruplandırır. Bir liste ayırıcı için yeni her gün oluşturur ve bir ayırıcı altında her gün için tüm oturumları listeler. Bu kodu çalıştırdığında gibi göründüğünü aşağıda verilmiştir:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>SessionsTable görünümü geliştirme

Bu bölümde, oturumları mobile özgü görünümünü oluşturacaksınız. Biz yaptığınız değişiklikler oluşturduk diğer görünümlerde daha kapsamlı daha olacaktır.

Mobil tarayıcı içinde dokunun **Konuşmacı** düğmesine ve ardından girin `Sc` arama kutusuna.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Dokunun **Scott Hanselman** bağlantı.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Gördüğünüz gibi görünen bir mobil tarayıcı üzerinde okuma zordur. Tarih sütunu zordur ve görünümden etiketleri sütundur. Bu sorunu gidermek için kopyalama *Views\Home\SessionsTable.cshtml* için *Views\Home\SessionsTable.Mobile.cshtml*ve sonra dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kod oda kaldırır ve sütun etiketleri ve bu bilgileri bir mobil tarayıcıda okunabilir olmasını sağlayın başlık, konuşmacı ve tarih dikey olarak biçimlendirir. Aşağıdaki resimde, kod değişiklikleri yansıtır.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>SessionByCode görünümü geliştirme

Son olarak, bir mobil özgü görünümünü oluşturacaksınız *SessionByCode* görünümü. Mobil tarayıcı içinde dokunun **Konuşmacı** düğmesine ve ardından girin `Sc` arama kutusuna.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Dokunun **Scott Hanselman** bağlantı. Scott Hanselman'ın oturumlar görüntülenir.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Seçin **bir genel bakış, MS Web yığın sevgiyle** bağlantı.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Varsayılan masaüstü görünüm bir sakınca yoktur ancak bunu artırabilir.

Kopyalama *Views\Home\SessionByCode.cshtml* için *Views\Home\SessionByCode.Mobile.cshtml* ve içeriklerini *Views\Home\SessionByCode.Mobile.cshtml*aşağıdaki işaretlemeyle dosyası:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Yeni biçimlendirme kullanan `data-role` görünüm düzenini iyileştirmek için özniteliği.

Mobil tarayıcıyı yenileyin. Aşağıdaki görüntüde, yaptığınız kod değişiklikleri yansıtır:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup ve gözden geçirme

Bu öğreticide, ASP.NET MVC 4 Geliştirici önizlemesi, yeni mobil özellikler sundu. Mobil özellikler şunlardır:

- Düzen, görünümler ve kısmi görünümler, hem genel hem de tek bir görünüm için geçersiz kılma olanağı.
- Düzen ve zorlama kısmi geçersiz kılma kullanarak denetime `RequireConsistentDisplayMode` özelliği.
- Mobil cihazlar için bir görünüm değiştirici pencere öğesi de masaüstü görünümlerde görüntülenebilenden görüntüler.
- İPhone tarayıcısı gibi belirli tarayıcı desteklemek için destek.

## <a name="see-also"></a>Ayrıca Bkz.

- [jQuery Mobile](http://jquerymobile.com) site.
- [jQuery Mobile genel bakış](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C öneri mobil Web uygulaması en iyi uygulamalar](http://www.w3.org/TR/mwabp/)
- [Medya sorgular için W3C aday önerisi](http://www.w3.org/TR/css3-mediaqueries/)
