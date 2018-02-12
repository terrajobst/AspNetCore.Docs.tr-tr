---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "ASP.NET MVC 4 mobil özellikleri | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici dağıtma bir ASP.NET MVC 5 mobil Web uygulamasını Azure Web sitelerindeki, kod örnekleri ile MVC 5 sürümü artık yoktur."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: f4e0e4eb558e0c7b9e94fc83ede986fa4c666739
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 mobil özellikleri
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Şimdi bu öğreticide kod örnekleri ile MVC 5 sürümü olan [bir ASP.NET MVC 5 mobil Web uygulamasını Azure Web sitelerindeki dağıtmak](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Bu öğretici bir ASP.NET MVC 4 Web uygulamasında mobil özellikleri ile nasıl çalışılacağını temellerini öğretmek. Bu öğretici için kullandığınız [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) veya Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer veya VWD&quot;). Zaten varsa, Visual Studio professional sürümünü kullanabilirsiniz.

Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recommended) or Visual Studio Web Developer Express SP1. Visual Studio 2012, ASP.NET MVC 4 içerir. Visual Web Developer 2010 kullanıyorsanız, yüklemelisiniz [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Bir mobil tarayıcı öykünücüsü de gerekir. Aşağıdakilerden birini çalışır:

- [Windows 7 Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Ekran görüntüleri çoğunda Bu öğreticide kullanılan öykünücüsü budur.)
- İPhone benzetmek için kullanıcı aracısı dizesi değiştirin. Bkz: [bu](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) blog girişi.
- [Opera Mobile öykünücüsü](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) iPhone için ayarlanmış kullanıcı aracısına sahip. Kullanıcı Aracısı, "iPhone" Safari ile ayarlama hakkında daha fazla yönerge için bkz: [IE olmasından içeriğini Safari izin vermek nasıl](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison'ın blogunda.

C# kaynak kodu ile Visual Studio projeleri bu konuya eşlik etmek kullanılabilir:

- [Başlangıç projesi indirme](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Proje indirme tamamlandı](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Ne oluşturacağınız

Bu öğretici için sağlanan Basit konferans listeleme uygulamaya mobil özellikler ekleyeceksiniz [başlangıç projesi](https://go.microsoft.com/fwlink/?LinkId=228307). Aşağıdaki ekran görüntüsünde tamamlanan uygulama etiketleri sayfasının de görüldüğü gibi gösterir [Windows 7 Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Bkz: [klavye eşleme için Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) klavye girişi basitleştirmek için.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Internet Explorer 9 veya 10, FireFox veya Chrome ayarlayarak, mobil uygulama geliştirmek için kullanabileceğiniz [kullanıcı aracısı dizesi](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Aşağıdaki resimde bir iPhone öykünen Internet Explorer'ı kullanarak tamamlanmış öğretici gösterir. Internet Explorer F-12 geliştirici araçlarını kullanabilirsiniz ve [Fiddler aracı](http://www.fiddler2.com/fiddler2/) uygulamanızda hata ayıklama yardımcı olacak.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- ASP.NET MVC 4 şablonlar HTML5 kullanma `viewport` özniteliği ve geliştirmek için Uyarlamalı işleme mobil cihazlarda görüntüler.
- Mobile özgü görünümler oluşturma
- Görünüm değiştirici sağlar'mobil bir görünüm ve uygulamanın masaüstü bir görünüm arasında geçiş yapma kullanıcılar oluşturma

### <a name="getting-started"></a>Başlarken

Aşağıdaki bağlantıyı kullanarak başlangıç projesi için konferans listeleme uygulamayı karşıdan: [karşıdan](https://go.microsoft.com/fwlink/?LinkId=228307). Windows Gezgini'nde sağ *MvcMobile.zip* dosya ve seçin **özellikleri**. İçinde **MvcMobile.zip özellikleri** iletişim kutusunda, seçin **Engellemeyi Kaldır** düğmesi. (Engellemelerini kaldırma kullanmaya çalıştığınızda oluşan bir güvenlik uyarısı engelleyen bir *.zip* Web'den indirdiğiniz dosya.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Sağ *MvcMobile.zip* dosya ve seçin **tümünü Ayıkla** dosyanın sıkıştırmasını açmak için. Visual Studio'da açın *MvcMobile.sln* dosya.

Masaüstü tarayıcınızda görüntülenir uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Mobil tarayıcı öykünücüyü başlatın, öykünücüsü konferans uygulaması URL'sini kopyalayın ve ardından **etikete göre Gözat** bağlantı. Windows Phone öykünücüsü kullanıyorsanız, URL çubuğuna tıklayın ve klavye erişmek için Duraklat tuşuna basın. Görüntünün gösterir aşağıda *AllTags* Görünüm (seçme gelen **etikete göre Gözat**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Görüntü bir mobil cihazda çok okunabilir durumdadır. ASP.NET bağlantıyı seçin.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET etiketi çok yığın görünümüdür. Örneğin, **tarih** sütundur okumak oldukça zor. Bir sürümünü oluşturmak daha sonra öğreticide *AllTags* için özellikle mobil tarayıcılar ve, yapacak görüntü okunabilir görünümü.

Not: Şu anda mobil önbelleğe alma altyapısında bir hata bulunmaktadır. Üretim uygulamaları için yüklemeniz gereken [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget paket. Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hata sabit](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için.

## <a name="css-media-queries"></a>CSS medya sorgular

[CSS medya sorguları](http://www.w3.org/TR/css3-mediaqueries/) medya türleri için CSS için bir uzantıdır. Belirli tarayıcılar (kullanıcı aracıları) için varsayılan CSS kuralları geçersiz kılma kurallar oluşturmanıza olanak sağlar. Mobil tarayıcılar hedefler CSS için ortak bir kural en yüksek ekran boyutu tanımlama. *Content\Site.css* yeni bir ASP.NET MVC 4 Internet projesi oluşturduğunuzda, oluşturduğunuz dosya aşağıdaki ortam sorgusu içerir:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Tarayıcı penceresini 850 piksel genişliğinde veya daha az ise, bu medya bloktaki CSS kuralları kullanır. Bu gibi CSS medya sorguları masaüstü tarayıcıları daha geniş görüntüler için tasarlanmış varsayılan CSS kurallarından (gibi mobil tarayıcılar) küçük tarayıcılarında HTML içeriğini daha iyi bir görünümünü sağlamak için kullanabilirsiniz.

## <a name="the-viewport-meta-tag"></a>Görünüm penceresinin Meta etiketi

Çoğu mobil tarayıcılar tanımlamak sanal tarayıcı penceresini genişliği ( *görünüm penceresinin*) mobil cihaz gerçek genişliğinden çok büyük. Bu, tüm web sayfasını sanal görüntü içine sığması mobil tarayıcılar sağlar. Kullanıcılar daha sonra ilginç içeriğine yakınlaştırabilirsiniz. Görünüm penceresinin genişliği gerçek cihaz genişliğine ayarlarsanız, mobil tarayıcıda içeriği uygun olduğundan ancak hiçbir yakınlaştırma gereklidir.

Görünüm penceresinin `<meta>` ASP.NET MVC 4 düzeni dosyasını etiketinde aygıt genişliğine görünüm penceresinin ayarlar. Görünüm penceresinin aşağıdaki satırı gösterir `<meta>` ASP.NET MVC 4 düzeni dosyasındaki etiketi.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>CSS medya sorgular ve görünüm penceresinin Meta etiketi etkisini inceleniyor

Açık *görünümler/paylaşılan\\_Layout.cshtml* dosya Düzenleyicisi ve yorum görünüm penceresinin çıkışı `<meta>` etiketi. Aşağıdaki biçimlendirmede kılınan satır gösterir.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Açık *MvcMobile\Content\Site.css* Düzenleyicisi'nde dosya ve medya sorgusunda en büyük genişliği sıfır piksel olarak değiştirin. Mobil tarayıcılardaki kullanılan bu CSS kuralları engeller. Aşağıdaki satırı değiştirilmiş bir medya sorgu gösterir:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Değişikliklerinizi kaydetmek ve konferans uygulaması bir mobil tarayıcı öykünücüsü'na göz atın. Aşağıdaki görüntüde küçük metin görünüm penceresinin kaldırma sonucudur `<meta>` etiketi. Olmayan ile `<meta>` etiketi, tarayıcı olan uzaklaştırma varsayılan görünüm penceresinin genişliğe (850 piksel veya çoğu mobil tarayıcılar için daha geniş.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Yaptığınız değişiklikleri geri almak — görünüm penceresinin açıklamadan çıkarın `<meta>` etiketi düzeni dosyasında ve ortam sorgusu 850 piksel cinsinden geri *Site.css* dosya. Değişikliklerinizi kaydetmek ve mobil dostu görünen geri yüklendiğini doğrulamak için mobil tarayıcıyı yenileyin.

Görünüm penceresinin `<meta>` etiketi ve CSS medya sorgu ASP.NET MVC 4 için belirli değildir ve herhangi bir web uygulamasında bu özelliklerden yararlanabilirsiniz. Ancak artık yeni bir ASP.NET MVC 4 proje oluşturduğunuzda, oluşturulan dosyalarıyla oluşturulmuş.

Görünüm penceresinin hakkında daha fazla bilgi için `<meta>` etiketlemek için bkz: [hikayesini iki viewports, — bölüm iki](http://www.quirksmode.org/mobile/viewports2.html).

Sonraki bölümde, mobil tarayıcı özel görünümler sağlamak nasıl görürsünüz.

## <a name="overriding-views-layouts-and-partial-views"></a>Görünümler, düzenler ve kısmi görünümler geçersiz kılma

ASP.NET MVC 4'te yeni bir önemli özellik genel olarak, tek bir mobil tarayıcı için veya belirli bir tarayıcı için mobil tarayıcılar için (düzenler ve kısmi görünümler dahil) herhangi bir görünüm geçersiz kılmanıza olanak sağlayan basit bir mekanizmadır. Mobile özgü görünüm sağlamak için Görünüm dosyasını kopyalamanız ve ekleme *. Mobil* dosya adı. Örneğin, bir mobil oluşturmak için *dizin* görüntülemek, kopyalamak *Views\Home\Index.cshtml* için *Views\Home\Index.Mobile.cshtml*.

Bu bölümde, bir mobil özgü Düzen dosyası oluşturacaksınız.

Başlatmak için kopyalama *görünümler/paylaşılan\\_Layout.cshtml* için *görünümler/paylaşılan\\_Layout.Mobile.cshtml*. Açık  *\_Layout.Mobile.cshtml* başlığını değiştirip **MVC4 konferans** için **konferans (mobil)**.

Her `Html.ActionLink` çağrısı, "her bağlantısını gözatma türü" kaldırmak *ActionLink*. Aşağıdaki kod mobil düzeni dosyasının tamamlanmış gövde bölümünü gösterir.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopya *Views\Home\AllTags.cshtml* dosya *Views\Home\AllTags.Mobile.cshtml*. Yeni dosyayı açın ve değiştirme `<h2>` için "etiketler (M)" öğesi "Etiketlerinden":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Bir masaüstü tarayıcısı kullanarak ve mobil tarayıcı öykünücüsü kullanarak etiketleri sayfasına göz atın. Mobil tarayıcı öykünücüsü iki değişikliklerinin gösterir.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Buna karşılık, masaüstü görüntü değişmemiştir.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Tarayıcı özel görünümler

Mobil ve Masaüstü özgü görünümler ek olarak, tek bir tarayıcı için görünümler oluşturabilirsiniz. Örneğin, özellikle iPhone tarayıcı için olan görünümleri oluşturabilirsiniz. Bu bölümde, iPhone tarayıcı ve bir iPhone sürümü için bir düzen oluşturacaksınız *AllTags* görünümü.

Açık *Global.asax* dosya ve aşağıdaki kodu ekleyin `Application_Start` yöntemi.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Bu kod, "her gelen istek karşı eşleşen iPhone" adlı yeni bir görüntü modu tanımlar. Gelen istek (diğer bir deyişle, kullanıcı aracısı dizesi "iPhone" içeriyorsa), tanımlanmış bir koşulu eşleşirse, ASP.NET MVC için görünümleri adında "iPhone" soneki içeren arar.

Kod içinde sağ `DefaultDisplayMode`, seçin **gidermek**ve ardından `using System.Web.WebPages;`. Bu bir başvuru ekler `System.Web.WebPages` yerdir ad alanı `DisplayModes` ve `DefaultDisplayMode` türleri tanımlanmıştır.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternatif olarak, aşağıdaki satırı yalnızca el ile ekleyebileceğiniz `using` dosyasının bölümü.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Tam içeriğini *Global.asax* dosya aşağıda gösterilmektedir.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Değişiklikleri kaydedin. Kopya *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosya *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Yeni dosyayı açın ve sonra değiştirmek `h1` gelen başlık `Conference (Mobile)` için `Conference (iPhone)`.

Kopya *MvcMobile\Views\Home\AllTags.Mobile.cshtml* dosya *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Yeni dosyasında değişiklik `<h2>` "Etiketleri (iPhone)" den "etiketler (M)" öğesine.

Uygulamayı çalıştırın. Bir mobil tarayıcı öykünücüsü çalıştırmak, kendi kullanıcı aracısı, "iPhone" ayarlandığından emin olun ve Gözat *AllTags* görünümü. Aşağıdaki ekran görüntüsü gösterildiği *AllTags* görünüm işlenen [Safari](http://www.apple.com/safari/download/) tarayıcı. Windows için Safari indirebilirsiniz [burada](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Bu bölümde mobil düzenleri ve görünümlerin nasıl oluşturulacağını ve düzenleri ve görünümler iPhone gibi belirli cihazlar için nasıl oluşturulacağını gördük. Sonraki bölümde nasıl yararlanacağınızı daha fazla ilgi çekici mobil görünüm için jQuery Mobile görürsünüz.

## <a name="using-jquery-mobile"></a>JQuery Mobile kullanma

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) kitaplığı tüm ana mobil tarayıcılar çalışır bir kullanıcı arabirimi çerçeve sağlar. jQuery Mobile geçerlidir *kademeli geliştirmeyi* CSS ve JavaScript destekleyen mobil tarayıcılar için. Kademeli geliştirmeyi daha güçlü tarayıcılar ve cihazlar daha zengin bir görüntü olmasını sağlarken temel bir web sayfası içeriğini görüntülemek tüm tarayıcılar sağlar. JQuery Mobile dahil edilen JavaScript ve CSS dosyaları biçimlendirme değişiklik yapmadan mobil tarayıcılar sığmayacak kadar çok sayıda öğesi stili.

Bu bölümde yüklersiniz *jQuery.Mobile.MVC* jQuery Mobile ve bir görünüm değiştirici pencere öğesi yükler NuGet paketi.

Başlatmak için silme *paylaşılan\\_Layout.Mobile.cshtml* ve *paylaşılan\\_Layout.iPhone.cshtml* daha önce oluşturduğunuz dosyaları.

Yeniden Adlandır *Views\Home\AllTags.Mobile.cshtml* ve *Views\Home\AllTags.iPhone.cshtml* dosyaları *Views\Home\AllTags.iPhone.cshtml.hide* ve  *Views\Home\AllTags.Mobile.cshtml.Hide*. Dosyaların artık yüklü olduğundan bir *.cshtml* uzantısını işlemek için ASP.NET MVC çalışma zamanı tarafından bunlar kullanılmayacak *AllTags* görünümü.

Yükleme *jQuery.Mobile.MVC* bunu yaparak NuGet paketi:

1. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. İçinde **Paket Yöneticisi Konsolu**, girin`Install-Package jQuery.Mobile.MVC -version 1.0.0`

Aşağıdaki resimde eklenir ve MvcMobile projesi için NuGet jQuery.Mobile.MVC paketi tarafından değiştirilen dosyaları gösterir. [Ekle] sonra dosya adını verdiğinizden eklenen dosyalar. GIF resim göstermez ve PNG dosyaları eklenen *Content\images* klasör.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

JQuery.Mobile.MVC NuGet paketi aşağıdaki yükler:

- *Uygulama\_Start\BundleMobileConfig.cs* eklenen jQuery JavaScript ve CSS dosyaları başvurmak için gereken dosya. Aşağıdaki yönergeleri izleyin ve bu dosyasında tanımlanan mobil paket başvuru gerekir.
- jQuery Mobile CSS dosyaları.
- A `ViewSwitcher` denetleyicisi pencere öğesi (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript dosyaları.
- JQuery Mobile stilde düzeni dosyasını (*görünümler/paylaşılan\\_Layout.Mobile.cshtml*).
- Görünüm değiştirici kısmi Görünüm *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) sayfanın üst kısmındaki Masaüstü görünümden mobil Görünüm ve tersi yönde geçiş yapmak için her bir bağlantı sağlar.
- Birkaç*.png* ve *.gif* görüntü dosyaları *Content\images* klasör.

Açık *Global.asax* dosya ve son satırının aşağıdaki kodu ekleyin `Application_Start` yöntemi.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Aşağıdaki kod tam gösterir *Global.asax* dosya.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Internet Explorer 9 kullanıyorsanız ve görmüyorsanız `BundleMobileConfig` içinde sarı Vurgu satır yukarıda, tıklatın [Uyumluluk Görünümü düğmesini](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![(Kapalı) Uyumluluk Görünümü düğmesinin resmi] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " (Kapalı) Uyumluluk Görünümü düğmesinin resmi") simge dosyasından bir anahattı Değiştir olun IE'de ![(Kapalı) Uyumluluk Görünümü düğmesinin resmi](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(Kapalı) Uyumluluk Görünümü düğmesinin resmi ") düz renk için ![(açık) Uyumluluk Görünümü düğmesinin resmi](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(açık) Uyumluluk Görünümü düğmesinin resmi"). Alternatif olarak, Bu öğretici FireFox veya Chrome görüntüleyebilirsiniz.


Açık *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosya ve aşağıdaki biçimlendirme doğrudan sonra Ekle `Html.Partial` çağırın:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Tam *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Uygulamayı oluşturun ve mobil tarayıcı öykünücüde gözatmak için *AllTags* görünümü. Aşağıdakilere bakın:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Mobil belirli kodu kapsamına ayıklayabilirsiniz [kullanıcı aracısı dizesi ayarı](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) IE veya Chrome iPhone ve F-12 geliştirici araçları kullanarak. Mobil tarayıcınız görüntülemiyorsa **giriş**, **Konuşmacı**, **etiketi**, ve **tarih** düğmeler olarak bağlantılar, jQuery Mobile başvuruları komut dosyaları ve CSS dosyaları doğru olmayabilir.


Stil değişikliklere ek gördüğünüz **mobil görünüm görüntüleme** ve mobil görünümünden Masaüstü görünümüne olanak sağlayan bir bağlantı. Seçin **Masaüstü görünümünde** bağlantı ve Masaüstü görünümünde görüntülenir.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Masaüstü görünümünde doğrudan mobil görünümüne gitmek için bir yol sağlamaz. Artık düzeltmesi. Açık *görünümler/paylaşılan\\_Layout.cshtml* dosya. Sayfanın altındaki yalnızca `body` öğesi, görünüm değiştirici pencere öğesi işler aşağıdaki kodu ekleyin:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Yenileme *AllTags* mobil tarayıcıda görüntüle. Şimdi, masaüstü ve mobil görünüm arasında gezinebilirsiniz.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Not hata ayıklama: görünümler/paylaşılan sonuna aşağıdaki kodu ekleyin\\tarayıcı kullanıcı aracısı dizesi kullanarak bir mobil cihazda ayarladığınızda görünümleri hata ayıklama yardımcı olmak için _ViewSwitcher.cshtml.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  ve aşağıdaki başlığı ekleyerek *görünümler/paylaşılan\\_Layout.cshtml* dosya.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Gözat *AllTags* bir masaüstü tarayıcısı sayfasında. Yalnızca mobil düzeni sayfaya eklendiğinden görünüm değiştirici pencere öğesi bir masaüstü tarayıcısı görüntülenmez. Öğreticide daha sonra Görünüm değiştirici pencere öğesi Masaüstü görünümüne nasıl ekleyebileceğiniz görürsünüz.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Konuşmacılar listesi artırma

Mobil tarayıcıda seçin **konuşmacılar** bağlantı. Mobil bir görünüm olduğundan (*AllSpeakers.Mobile.cshtml*), varsayılan konuşmacılar görüntüleme (*AllSpeakers.cshtml*) mobil düzeni görünümü kullanılarak oluşturulması ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Ayarlayarak genel işleme mobil düzeni içinde varsayılan (mobil olmayan) görünümünden devre dışı bırakabilirsiniz `RequireConsistentDisplayMode` için `true` içinde *görünümleri\\_ViewStart.cshtml* dosyası şuna benzer:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Zaman `RequireConsistentDisplayMode` ayarlanır `true`, mobil düzenini (*\_Layout.Mobile.cshtml*) yalnızca mobil görünümleri için kullanılır. (Diğer bir deyişle, görünüm dosyası biçimidir ***Görünümadı**. Mobile.cshtml*.) Ayarlamak isteyebilirsiniz `RequireConsistentDisplayMode` için `true` mobil düzeninizi iyi mobil olmayan görünümlerle işe yaramazsa. Ekran görüntüsünün altında gösterir nasıl *konuşmacılar* sayfasını işler `RequireConsistentDisplayMode` ayarlanır `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Ayarlayarak görünümünde tutarlı görüntü modu devre dışı bırakabilirsiniz `RequireConsistentDisplayMode` için `false` görünüm dosyasında. Aşağıdaki biçimlendirmede *Views\Home\AllSpeakers.cshtml* dosya kümeleri `RequireConsistentDisplayMode` için `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Bir mobil konuşmacılar görünümü oluşturma

Az önce gördüğünüz gibi *konuşmacılar* görünümdür okunabilir ancak bağlantıları küçük ve bir mobil cihazda dokunun zordur. Bu bölümde, mobil özgü oluşturacaksınız *konuşmacılar* modern mobil uygulama gibi görünüyor görünüm — büyük görüntüler, dokunun kolay bağlar ve konuşmacılar hızla bulmak için bir arama kutusu içerir.

Kopya *AllSpeakers.cshtml* için *AllSpeakers.Mobile.cshtml*. Açık *AllSpeakers.Mobile.cshtml* dosya ve kaldırma `<h2>` başlık öğesi.

İçinde `<ul>` etiketi, add `data-role` özniteliği ve değerini ayarlama `listview`. Gibi diğer [ `data-*` öznitelikleri](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` dokunmak büyük liste öğeleri kolay hale getirir. Tamamlanan biçimlendirme benzer şudur:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Mobil tarayıcıyı yenileyin. Güncelleştirilmiş görünümü şu şekildedir:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Mobil görünüm geliştirilmiştir rağmen konuşmacılar uzun listesi gidin zordur. İçinde bu sorunu düzeltmek için `<ul>` etiketi, add `data-filter` özniteliği ve ayarlamak `true`. Aşağıdaki kodu gösterir `ul` biçimlendirme.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Aşağıdaki resimde oluşur sayfanın üstündeki arama filtre kutusuna gösterilmiştir `data-filter` özniteliği.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Her harfi arama kutusuna yazarken, jQuery Mobile görüntülenen listede aşağıdaki resimde gösterildiği gibi filtreler.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Etiketler listesi artırma

Gibi varsayılan *konuşmacılar* görünümü *etiketleri* görünüm okunabilir olmakla birlikte, küçük ve bir mobil cihazda dokunun zor bağlantılardır. Bu bölümde, düzeltme *etiketleri* , sabit aynı şekilde görüntülemek *konuşmacılar* görünümü.

Kaldırma &quot;Gizle&quot; için soneki *Views\Home\AllTags.Mobile.cshtml.hide* adı olacak şekilde dosya *Views\Home\AllTags.Mobile.cshtml*. Adlandırılan dosyayı açın ve kaldırma `<h2>` öğesi.

Ekleme `data-role` ve `data-filter` özniteliklerini `<ul>` , aşağıda gösterildiği gibi etiketi:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Aşağıdaki görüntü harfi filtreleme etiketler sayfası gösterir `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Tarihleri listesi artırma

Geliştirmek *tarihleri* , geliştirilmiş gibi görüntülemek *konuşmacılar* ve *etiketleri* görünümleri böylece mobil aygıtta kullanmak daha kolaydır.

Kopya *Views\Home\AllDates.cshtml* dosya *Views\Home\AllDates.Mobile.cshtml*. Yeni dosyayı açın ve kaldırma `<h2>` öğesi.

Ekleme `data-role="listview"` için `<ul>` etiketi şuna benzer:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Aşağıdaki resimde ne görebilirsiniz **tarih** gibi sayfa görünüyor `data-role` yerinde özniteliği.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Değiştir *Views\Home\AllDates.Mobile.cshtml* aşağıdaki kod ile dosya:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Bu kod tüm oturumları günlerine göre gruplandırır. Yeni her gün için bir liste ayırıcı oluşturur ve bir ayırıcı altında her gün için tüm oturumları listeler. Bu kod çalıştığında gibi göründüğünü aşağıda verilmiştir:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>SessionsTable görünüm artırma

Bu bölümde, oturumlar mobile özgü görünümünü oluşturacaksınız. Biz yaptığınız değişiklikler oluşturduk diğer görünümlerde daha kapsamlı daha olacaktır.

Mobil tarayıcıda dokunun **Konuşmacı** düğmesine ve ardından girin `Sc` arama kutusuna.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Dokunun **Scott Hanselman** bağlantı.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Gördüğünüz gibi görünen bir mobil tarayıcı üzerinde okuma zordur. Tarih sütunu okunması zor ve etiketleri sütun dışında görüntüle. Bu sorunu gidermek için kopyalama *Views\Home\SessionsTable.cshtml* için *Views\Home\SessionsTable.Mobile.cshtml*ve ardından dosyasının içeriğini aşağıdaki kodla değiştirin:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kod yer kaldırır ve sütun etiketleri ve bu bilgileri bir mobil tarayıcı üzerinde okunabilir olmasını sağlamak başlık, konuşmacı ve tarih dikey olarak biçimlendirir. Aşağıdaki görüntü kod değişiklikleri yansıtır.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>SessionByCode görünüm artırma

Son olarak, bir mobil özgü görünümünü oluşturacaksınız *SessionByCode* görünümü. Mobil tarayıcıda dokunun **Konuşmacı** düğmesine ve ardından girin `Sc` arama kutusuna.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Dokunun **Scott Hanselman** bağlantı. Scott Hanselman'ın oturumlar görüntülenir.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Seçin **, MS Web yığını mutluluk genel bir bakış** bağlantı.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Varsayılan masaüstü görünümünde sorun yoktur, ancak bunu artırabilir.

Kopya *Views\Home\SessionByCode.cshtml* için *Views\Home\SessionByCode.Mobile.cshtml* ve içeriğini değiştirme *Views\Home\SessionByCode.Mobile.cshtml*aşağıdaki biçimlendirme dosyasıyla:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Yeni markup kullanan `data-role` görünümün düzenini geliştirmek için öznitelik.

Mobil tarayıcıyı yenileyin. Aşağıdaki resimde yaptığınız kod değişiklikleri yansıtır:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup ve gözden geçirme

Bu öğreticide, ASP.NET MVC 4 Geliştirici önizlemesi yeni mobil özelliklerini anlatılmıştır. Mobil özellikleri içerir:

- Düzen, görünümleri ve kısmi görünümler, hem genel hem de tek bir görünümü için geçersiz kılma yeteneği.
- Düzen ve kısmi geçersiz kılma zorlama kullanarak üzerinde denetim `RequireConsistentDisplayMode` özelliği.
- Görünüm değiştirici pencere öğesi Mobile de masaüstü görünümlerde görüntülenebilenden görüntüler.
- İPhone tarayıcısı gibi belirli tarayıcılar desteklemek için destek.

## <a name="see-also"></a>Ayrıca Bkz.

- [jQuery Mobile](http://jquerymobile.com) site.
- [jQuery Mobile Overview](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C öneri mobil Web uygulaması en iyi uygulamalar](http://www.w3.org/TR/mwabp/)
- [Medya sorgular için W3C adayı önerisi](http://www.w3.org/TR/css3-mediaqueries/)
