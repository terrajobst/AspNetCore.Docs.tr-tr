---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4'te Yenilikler | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4'te yenilikler nelerdir?

Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET ve .NET framework gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir. Bu yeni, mobil web uygulaması geliştirme daha kolay hale dördüncü framework sürümünü odaklanır.

Başından itibaren yeni bir ASP.NET MVC 4 proje oluşturduğunuzda var. Şimdi mobil cihazlar için özellikle bir tek başına uygulamasını oluşturmak için kullanabileceğiniz bir mobil uygulama proje şablonu Ayrıca, ASP.NET MVC 4 jQuery Mobile jQuery.Mobile.MVC NuGet paketi ile tümleşir. jQuery Mobile, Windows Phone, iPhone, Android vb. de dahil olmak üzere tüm popüler mobil cihaz platformları ile uyumlu olan web uygulamaları geliştirmek için bir HTML5 tabanlı çerçevedir. Uzmanlık ihtiyacınız varsa, ancak, ASP.NET MVC 4 ayrıca farklı görünümleri farklı aygıtlar için hizmet ve cihaza özgü iyileştirmeler sağlamanıza olanak sağlar.

Uygulamalı bu laboratuvarda, ASP.NET MVC 4 ile başlar &quot;Internet uygulama&quot; bir Fotoğraf Galerisi uygulaması oluşturmak için proje şablonu. Farklı mobil cihaz ve masaüstü web tarayıcıları ile uyumlu hale getirmek için jQuery Mobile ve ASP.NET MVC 4'ın yeni özellikleri kullanarak uygulamayı aşamalı olarak artırır. Kod oluşturma ve nasıl ASP.NET MVC 4 görev destekleyerek zaman uyumsuz eylem yöntemleri yazmanızı kolaylaştırır için yeni kod tarif hakkında bilgi edineceksiniz&lt;ActionResult&gt; dönüş türü.

> [!NOTE]
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET 4.5 Web formları'de](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama proje şablonu geliştirmeler yararlanın
- HTML5 görünüm penceresinin özniteliği ve CSS medya sorguları mobil cihazlarda görünen geliştirmek için kullanın
- JQuery Mobile aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın
- Mobile özgü görünümler oluşturma
- Görünüm değiştirici bileşeni uygulamasında mobil ve Masaüstü görünümleri arasında geçiş yapmak için kullanın
- Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturun

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).
- [ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 yüklemesine dahil)
- Windows Phone öykünücüsü (dahil [7.1.1 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- İsteğe bağlı - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) ile **Electric Plum iPhone benzeticisi** uzantısına (yalnızca web uygulaması ile bir iPhone benzeticisi göz atmak için kullanılan alıştırma 3)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Size kolaylık sağlamak için Visual Studio kod Visual Studio içinde el ile eklemek zorunda kalmamak için kullanabileceğiniz parçacıkları, bu kodu çoğu sağlanır.

Kod parçacıkları yüklemek için:

1. Bir Windows Explorer penceresi açın ve Gözat Laboratuvar için 's **Source\Setup** klasör.
2. Çift **Setup.cmd** Visual Studio kod parçacıkları yüklemek için bu klasörde dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek A: kullanarak kod parçacıkları](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Yeni ASP.NET MVC 4 proje şablonları](#Exercise1)
2. [Fotoğraf Galerisi Web uygulaması oluşturma](#Exercise2)
3. [Mobil cihaz desteği ekleme](#Exercise3)
4. [Zaman uyumsuz denetleyicileri kullanma](#Exercise4)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Alıştırma 1: Yeni ASP.NET MVC 4 proje şablonları

Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz. Internet uygulaması şablonu ek olarak, MVC 3'te zaten mevcut bu sürümü artık mobil uygulamalar için ayrı bir şablon içerir. İlk olarak, her bir şablonlar ilgili bazı özellikleri arar. Ardından, doğru yaklaşımı kullanarak sayfanızı farklı platformlarda düzgün işleme çalışır.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Görev 1 - Internet uygulama şablonu keşfetme

1. Açık **Visual Studio**.
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.** Proje adı **Fotografgalerisi**, bir konum seçin (veya varsayılan adı bırakın) tıklatıp **Tamam**.

    > [!NOTE]
    > Daha sonra artık oluşturmakta olduğunuz Fotografgalerisi ASP.NET MVC 4 çözümü özelleştirme.

    ![Yeni proje oluşturma](whats-new-in-aspnet-mvc-4/_static/image1.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**. Razor görüntüleme altyapısı seçtiğinizden emin olun.

    ![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")

    *Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*

    > [!NOTE]
    > Razor sözdizimi ASP.NET MVC 3'te sunulmuştur. Karakterler ve gerekli bir dosya bir hızlı ve iş akışı kodlama sıvı etkinleştirme tuş vuruşları sayısını en aza indirmek için kendi hedeftir. Razor yararlanır varolan C# / VB (veya diğer) dil becerileri ve harika bir HTML oluşturma iş akışı sağlayan bir şablon biçimlendirme sözdizimi sunar.
4. Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonları görmek için. Aşağıdaki özellikleri denetleyebilirsiniz:

    **Modern stili şablonları**

    Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.

    ![ASP.NET MVC 4 stili şablonları](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC şablonları Stili 4")

    *ASP.NET MVC 4 stili şablonları*

    ![Yeni bir kişi sayfa](whats-new-in-aspnet-mvc-4/_static/image4.png "yeni kişi sayfası")

    *Yeni ilgili kişi sayfası*

    **Uyarlamalı işleme**

    Tarayıcı penceresini yeniden boyutlandırma çıkışı denetleyin ve nasıl sayfa düzeni yeni pencere boyutunu dinamik olarak uyum dikkat edin. Bu şablonlar, hem Masaüstü hem de mobil platformları hiçbir özelleştirme gerektirmeden düzgün işlenecek Uyarlamalı işleme tekniği kullanın.

    ![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](whats-new-in-aspnet-mvc-4/_static/image5.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")

    *ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda*

    **JavaScript ile zengin kullanıcı Arabirimi**

    Varsayılan proje şablonları için başka bir geliştirme JavaScript daha etkileşimli JavaScript sağlamak için kullanılır. Şablonda kullanılan oturum açma ve kaydetme bağlantıları doğrulamaları jQuery istemci-tarafı giriş alanları doğrulamak için nasıl kullanılacağını exemplify.

    ![jQuery doğrulama](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery Validation*

    > [!NOTE]
    > Bildirim, bölümlerinde, ilk bölüm iki oturum sitesinden kayıtlı hesabı kullanarak ve google (varsayılan olarak devre dışı) gibi başka bir kimlik doğrulama hizmeti kullanarak altenativelly oturum yapabilecekleriniz ikinci kısmında oturum açabilir.
5. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
6. Dosyayı açmak **AuthConfig.cs** altında bulunan **uygulama\_Başlat** klasör.
7. Google istemci kaydetmek için son satırından açıklamayı Kaldır *OAuth* kimlik doğrulaması.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. Tuşuna **F5** çözümü çalıştırın ve oturum açma sayfasına gidin.
9. Seçin **Google** oturum açmak için hizmet.

    ![Günlük hizmetinde seçme](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Günlük hizmetinde seçme*
10. Google hesabınızı kullanarak oturum açın.
11. Google hesabı bilgilerini almak site (localhost) izin verin.
12. Son olarak, Google hesabıyla ilişkilendirmek için siteyi kaydettirmeniz gerekecektir.

   ![Google hesabınız ilişkilendirme](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Google hesabınız ilişkilendirme*
13. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
14. Şimdi proje şablonu ASP.NET MVC 4 tarafından sunulan diğer bazı yeni özellikler kullanıma için çözüm keşfedin.

   ![ASP.NET MVC 4 Internet uygulaması proje şablonu](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")

   *ASP.NET MVC 4 Internet uygulaması proje şablonu*

   - **HTML 5 biçimlendirme**

       Yeni temayı biçimlendirme bulmak için şablon görünümleri göz atın.

       ![Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu.")

       *Razor ve HTML5 biçimlendirme (About.cshtml) kullanarak yeni şablonu.*
   - **Güncelleştirilmiş JavaScript kitaplıkları**

       ASP.NET MVC 4 varsayılan şablonu artık Çakıştırmaları, zengin oluşturmanıza olanak sağlayan bir JavaScript MVVM framework ve JavaScript ve HTML kullanarak hızlı yanıt veren web uygulamaları içerir. Gibi MVC3 jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.

     > [!NOTE]
     > Bu bağlantıyı Çakıştırmaları kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Ayrıca, jQuery ve jQuery UI hakkında öğrenebilirsiniz içinde [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Görev 2 - mobil uygulama şablonu keşfetme

ASP.NET MVC 4 mobil için Web siteleri ve tablet tarayıcılar geliştirilmesini kolaylaştırır. Bu şablon Internet uygulama şablonu (bildirim denetleyicisi kodu hemen hemen aynıdır) aynı uygulama yapısını var, ancak kendi stil dokunma tabanlı mobil cihazlarda düzgün işlenecek değiştirildi.

1. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.** Proje adı **PhotoGallery.Mobile**, bir konum seçin (veya varsayılan adı bırakın), seçin &quot;eklemek için çözüm&quot; tıklatıp **Tamam**.
2. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **mobil uygulama** proje şablonu ve tıklatın **Tamam**. Razor görüntüleme altyapısı seçtiğinizden emin olun.

    ![Yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image11.png "yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma")

    *Yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma*
3. Şimdi çözümü keşfedin ve mobile için ASP.NET MVC 4 çözüm şablonu tarafından sunulan yeni özelliklerden bazıları göz atın:

    - **jQuery Mobile Library**

        Mobil uygulama proje şablonu mobil tarayıcı uyumluluğu için bir açık kaynak kitaplığı jQuery mobil kitaplık içerir. jQuery Mobile kademeli geliştirmeyi CSS ve JavaScript destekleyen mobil tarayıcılar için geçerlidir. Kademeli geliştirmeyi yalnızca Zengin içeriği görüntülemek en güçlü tarayıcılar olanak sağlarken temel bir web sayfası içeriğini görüntülemek tüm tarayıcılar sağlar. JQuery Mobile stili dahil JavaScript ve CSS dosyaları sayfası biçimlendirme içinde herhangi bir değişiklik yapmadan ekranında içeriğin sığması için mobil tarayıcılar yardımcı olur.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *şablona dahil jQuery mobil kitaplığı*
    - **Temel HTML5 biçimlendirme**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *HTML5 biçimlendirme, (Login.cshtml ve Index.cshtml) kullanılarak mobil uygulama şablonu*
4. Tuşuna **F5** çözümü çalıştırın.
5. Açık **Windows Phone 7 öykünücüsü**.
6. Telefon başlangıç ekranında da Internet Explorer'ı açın. Masaüstü uygulaması başlatıldığı URL denetleyin ve bu URL'sine gidin telefonunuzdan (örneğin `http://localhost:[PortNumber]/`).
7. Oturum açma sayfasına girin ya da kullanıma artık sayfa hakkında. Mobil için yeni Metro uygulamasını Web sitesinin stil dayalı dikkat edin. ASP.NET MVC 4 proje şablonu, sayfanın tüm öğeleri görünür ve etkin emin mobil cihazlarda düzgün görüntülenir. Üstbilgi bağlantıları tıklattınız veya dokunduğunuz büyük olduğuna dikkat edin.

    ![Proje şablonunu bir mobil cihaz sayfalarında](whats-new-in-aspnet-mvc-4/_static/image14.png "proje şablonu sayfalarında bir mobil cihaz")

    *Mobil aygıtta proje şablonu sayfaları*
8. Yeni şablon de kullanır **görünüm penceresinin meta etiketi**. Çoğu mobil tarayıcılar sanal tarayıcı penceresi için bir genişliği tanımlayın veya &quot;görünüm penceresinin&quot;, mobil cihaz gerçek genişliğinden daha büyük olduğu. Bu sanal görüntü içindeki tüm web sayfasını görüntülemek mobil tarayıcılar sağlar. **Görünüm penceresinin meta etiketi** genişlik, yükseklik ve tarayıcı alanı ölçeğini mobil cihazlarda ayarlamak web geliştiricileri sağlayan **.** ASP.NET MVC 4 şablon mobil uygulamalar için cihaz genişliğine görünüm penceresinin ayarlar (&quot;genişliği aygıt-width =&quot;) düzeni şablondaki (*görünümler/paylaşılan\_Layout.cshtml*), böylece tüm sayfalar, cihaz ekran genişliği kendi görünüm penceresinin sahip olur. Görünüm penceresinin meta etiketi varsayılan tarayıcı görünümü değiştirmez dikkat edin.
9. Açık  **\_Layout.cshtml**, bulunan **görünümleri | Paylaşılan** klasörü ve yorum görünüm penceresinin meta etiketi. Uygulamayı çalıştırın yoksa zaten açılmış ve farklılıkları denetleyin.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.
11. Görünüm penceresinin meta etiketi açıklamadan çıkarın.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Görev 3 - Uyarlamalı işleme kullanma

Bu görevde, hiçbir özelleştirme gerektirmeden aynı anda bir Web sayfasında doğru mobil cihazlar ve Web tarayıcıları işlemek için başka bir yöntem öğreneceksiniz. Görünüm penceresinin meta etiketi benzer amacı ile zaten kullandınız. Başka bir güçlü yöntemi karşılayacağını artık: *Uyarlamalı işleme*.

Uyarlamalı işleme kullanan bir teknik olduğu **CSS3 media sorguları** bir sayfaya uygulanan stil özelleştirmek için. Medya sorguları, belirli bir koşul altında CSS stilleri gruplandırma bir stil sayfası içinden koşulları tanımlayın. Yalnızca koşulun true olduğunda, stil bildirilen nesnelere uygulanır.

Farklı cihazlarda site görüntüleme için hiçbir özelleştirme Uyarlamalı işleme tekniği tarafından sağlanan esneklik sağlar. Tek bir stil sayfasında mantığı kod yazmadan stili seçmek için istediğiniz sayıda stilleri tanımlayabilirsiniz. Yinelenen kodu ve amacıyla işleme mantığı miktarını azaltır bu nedenle, sayfa stilleri uyarlama çok düzgün bir şekilde olur. CSS dosyalarınızın boyutunu fazladır büyüdükçe diğer yandan, bant genişliği tüketimi, artırır.

Uyarlamalı işleme yöntemi kullanarak, sitenizi olacaktır **düzgün bir şekilde, tarayıcı bağımsız olarak görüntülenir.** Ancak, bant genişliği ek yük ise ilgili bir sorun düşünmelisiniz.

> [!NOTE]
> Bir ortam sorgusu temel biçimi: @media \[kapsamı: tüm | el | yazdırma | projeksiyon | ekran\] ([özellik: değer] ve... [özellik: değer])


Medya sorguları örnekleri: &gt;  <strong>@media tüm ve (en fazla genişlik: 1000px) ve (min-width: 700px) {}:</strong> 700px 1000px arasındaki tüm çözümler için.

> <strong>@media ekran ve (min-width: 400px) ve (en fazla genişlik: 700px) {...}:</strong> yalnızca ekranlar için. Çözünürlüğü 400 ile 700px arasında olmalıdır.
> 
> <strong>@media taşınabilir ve (min-width: 20em), ekran ve (min-width: 20em) {...}:</strong> el bilgisayarlarında çalışmak (Mobil ve aygıtlar) ve ekranlar için. En küçük genişliği 20em büyük olmalıdır.
> 
> Bu konu hakkında daha fazla bilgi bulabilirsiniz [W3C site](http://www.w3.org/TR/css3-mediaqueries/).


ASP.NET MVC okunabilirliğini artırmak 4 varsayılan Web sitesi şablonu, Uyarlamalı işleme nasıl çalıştığını şimdi keşfedin.

1. Açık **PhotoGallery.sln** görev 1'den oluşturdunuz ve seçin çözümü **Fotografgalerisi** projesi. Tuşuna **F5** çözümü çalıştırın.
2. Tarayıcının genişlik, yarısı veya değerinden özgün boyutuna çeyreği windows ayarı yeniden boyutlandırın. Üstbilgi öğeleri ile neler dikkat edin: bazı öğeler üstbilgi görünür alanında görünmez.
3. Açık <strong>Site.css</strong> dosya bulunan Visual Studio Çözüm Gezgini'nden <strong>içerik</strong> proje klasörü. Tuşuna <strong>CTRL + F</strong> Visual Studio tümleşik arama açın ve yazmak için <strong>@media</strong> bulmak için <strong>CSS ortam sorgusu</strong>.

    Bu şablonda tanımlanan medya sorgu koşulu bu şekilde çalışır: tarayıcının pencere boyutu olduğunda aşağıda **850 piksel**, uygulanan CSS kuralları bu ortam bloğu içinde tanımlanan olanlardır.

    ![Ortam sorgusu bulma](whats-new-in-aspnet-mvc-4/_static/image16.png "ortam sorgusu bulma")

    *Ortam sorgusu bulma*
4. İçinde 850 ayarlamak en büyük genişliği öznitelik değeri değiştirin piksel ile **10px**, Uyarlamalı işleme devre dışı bırakmak için ve basın **CTRL + S** değişiklikleri kaydetmek için. Dönüş basın ve tarayıcı **CTRL + F5** yapmış olduğunuz değişiklikleri sayfasıyla yenilenecek. Her iki sayfa farklılıkları penceresinin genişliğini ayarlarken dikkat edin.

    ![Sayfanın sol, uygulama @media sağ, stil stili atlanırsa](whats-new-in-aspnet-mvc-4/_static/image17.png "sayfanın sol, uygulama @media sağ, stil stili atlanırsa")

    <em>Sayfanın sol, uygulama @media sağ, stil stili atlanırsa</em>

    Şimdi, şirketinizdeki mobil cihazlarda olanlar çıkışı denetleyin:

    ![Sayfanın sol, uygulama @media sağ, stil stili atlanırsa](whats-new-in-aspnet-mvc-4/_static/image18.png "sayfanın sol, uygulama @media sağ, stil stili atlanırsa")

    <em>Sayfanın sol, uygulama @media sağ, stil stili atlanırsa</em>

    Sayfa bir Web tarayıcısında işlendiğinde değişiklikleri bir mobil aygıtı kullanırken çok önemli olmadığını göreceksiniz rağmen farklar daha belirgin hale gelir. Görüntünün sol tarafındaki özel stili okunabilirlik geliştirilmiş görebilirsiniz.

    Uyarlamalı işleme koşullu bir Web sitesi için stil oluşturma ve masaüstünüzdeki bir yaklaşım ile ilgili ortak stil sorunları çözme uygulamak daha kolay pek çok daha fazla senaryolarda kullanılabilir.

    Bu özelliklerden herhangi bir web uygulamasında olabilmesi için görünüm penceresinin meta etiketi ve CSS medya sorgular için ASP.NET MVC 4, özel değildir.
5. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Alıştırma 2: Fotoğraf Galerisi Web uygulaması oluşturma

Bu alıştırmada, fotoğraf görüntülemek için bir Fotoğraf Galerisi uygulaması üzerinde çalışır. ASP.NET MVC 4 proje şablonu ile başlar ve ardından, bir hizmetinden fotoğraf almak ve bunları giriş sayfası görüntülemek için bir özellik ekler.

Aşağıdaki alıştırmada mobil cihazlarda görüntülenme şeklini artırmak için bu çözümün güncelleştirir.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Görev 1 - sahte fotoğraf hizmet oluşturma

Bu görevde mock galeride gösterilecek içeriği almak için fotoğraf hizmetinin oluşturur. Bunu yapmak için sadece bir JSON dosyası her fotoğraf verilerle döndürülecek yeni bir denetleyici ekleyecek.

1. Açık **Visual Studio** zaten açtıysanız.
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.** Proje adı **Fotografgalerisi**, bir konum seçin (veya varsayılan adı bırakın) tıklatıp **Tamam**. Alternatif olarak, var olan ASP.NET MVC 4'ten çalışmaya devam edebilirsiniz **Internet uygulama** çözümden **alıştırma 1** ve bir sonraki adımı atlayın.
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**. Razor görüntüleme altyapısı seçili olduğundan emin olun.
4. İçinde **Çözüm Gezgini**, sağ **uygulama\_veri** klasörü seçin ve proje **Ekle | Varolan öğeyi**. Gözat **Source\Assets\App\_veri** bu laboratuvarı klasör ekleyin **Photos.json** dosya.
5. Adlı yeni bir denetleyicisi oluşturun **PhotoController**. Bunu yapmak için sağ **denetleyicileri** klasörüne gidin **Ekle** seçip **denetleyicisi.** Denetleyici adı tamamlamak, bırakın **boş MVC denetleyicisi** şablonu ve tıklatın **Ekle**.

    ![PhotoController ekleme](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController ekleme")

    *PhotoController ekleme*
6. Değiştir **dizin** aşağıdaki yöntemiyle **galeri** eylem ve son eklediğiniz projeye JSON dosyasından içerik return.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - galeri eylem*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. Tuşuna **F5** çözümü çalıştırın ve mocked fotoğraf hizmet test etmek için aşağıdaki URL'ye gidin: `http://localhost:[port]/photo/gallery` ([bağlantı noktası] değeri uygulama burada başlatılmış geçerli bağlantı noktasına bağlıdır). Bu URL'sine yönelik istek içeriğini almak **Photos.json** dosya.

    ![Mocked fotoğraf hizmetini sınama](whats-new-in-aspnet-mvc-4/_static/image20.png "mocked fotoğraf hizmeti test etme")

    *Mocked fotoğraf hizmeti test etme*

Gerçek bir uygulamasında kullanabilirsiniz [ASP.NET Web API](../../../../web-api/index.md) Fotoğraf Galerisi hizmeti uygulama. ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Görev 2 - Fotoğraf Galerisi görüntüleme

Bu görevde, bu alıştırmada ilk görevde oluşturduğunuz mocked hizmetini kullanarak Fotoğraf Galerisi göstermek için giriş sayfası güncelleştirir. Model dosyaları ekleyin ve galeri görünümlerini güncelleştirmek.

1. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.
2. Oluşturma **fotoğraf** sınıfını **modelleri** klasör. Bunu yapmak için sağ **modelleri** klasöründe seçin **Ekle** tıklatıp **sınıfı**. Ardından, kümesinin adı **Photo.cs** tıklatıp **Ekle**.
3. Aşağıdaki üye ekleme **fotoğraf** sınıfı.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf modeli*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. Açık **HomeController.cs** dosya **denetleyicileri** klasör.
5. Aşağıdaki using deyimlerini.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - HomeController kullanımları*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. Güncelleştirme **dizin** kullanmak için eylem **HttpClient** galeri verileri almak ve daha sonra kullanmak için **JavaScriptSerializer** görünüm modeli seri durumdan çıkarılamadı.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - dizin eylem*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. Açık **Index.cshtml** altında bulunan dosya **görünümler** klasörünü ve tüm içeriğini aşağıdaki kodla değiştirin.

    Bu kod aracılığıyla hizmetinden alınan tüm fotoğrafları döngüler ve sırasız bir listesini görüntüler.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf listesi*)


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. İçinde **Çözüm Gezgini**, sağ **içerik** klasörü seçin ve proje **Ekle | Varolan öğeyi**. Gözat **Source\Assets\Content** bu laboratuvarı klasör ekleyin **Site.css** dosya. Değişimi onaylamanız gerekir. Varsa **Site.css** dosya açık, ayrıca dosyayı yeniden doğrulamak gerekir.
9. Dosya Gezgini'ni açın ve tüm kopyalama **fotoğraf** klasörünün altında **Source\Assets** bu laboratuvarı Çözüm Gezgini'nde projenizin kök klasörünün klasör.
10. Uygulamayı çalıştırın. Fotoğraf Galerisi görüntüleme giriş sayfası görmelisiniz.

    ![Fotoğraf Galerisi](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotoğraf Galerisi")

    *Fotoğraf Galerisi*
11. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Alıştırma 3: mobil cihaz desteği ekleme

ASP.NET MVC 4 anahtar güncelleştirmeleri mobil geliştirme desteği biridir. Bu alıştırmada, önceki alıştırmada oluşturduğunuz Fotografgalerisi çözüm genişleterek mobil uygulamalar için ASP.NET MVC 4 yeni özellikleri inceleyeceksiniz.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Görev 1 - bir ASP.NET MVC 4 uygulamasında yükleme jQuery Mobile

1. Açık **başlamak** çözüm bulunan **kaynak/Ex3-MobileSupport/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Paket Yöneticisi Konsolu** tıklayarak **Araçları** &gt; **kitaplık Paket Yöneticisi** &gt; **Paket Yöneticisi Konsol** menü seçeneği.

    ![NuGet Paket Yöneticisi konsolu açma](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet Paket Yöneticisi konsolunu açma")

    *NuGet Paket Yöneticisi konsolu açma*
3. Paket Yöneticisi konsolunda yüklemek için aşağıdaki komutu çalıştırın **jQuery.Mobile.MVC** paket.

    jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığıdır. JQuery Mobile'ı ASP.NET MVC 4 uygulamayla kullanmak için Yardımcıları jQuery.Mobile.MVC NuGet paketini içerir.

    > [!NOTE]
    > Aşağıdaki komutu çalıştırarak, jQuery.Mobile.MVC kitaplığını Nuget'ten indiriliyor.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Bu komut, jQuery Mobile ve aşağıdakiler de dahil olmak üzere bazı yardımcı dosyaları yükler:

    - **Görünümler/paylaşılan/\_Layout.Mobile.cshtml**: küçük bir ekran için en iyi hale getirilmiş bir jQuery Mobile tabanlı düzeni. Web sitesi mobil tarayıcıdan bir istek aldığında, özgün düzeni değiştirir (\_Layout.cshtml) Bu bir.
    - Bir görünüm değiştirici bileşeni: oluşan **görünümler/paylaşılan/\_ViewSwitcher.cshtml** kısmi Görünüm ve **ViewSwitcherController.cs** denetleyicisi. Bu bileşen, kullanıcıların sayfanın Masaüstü sürümüne geçiş mobil tarayıcılar üzerinde bir bağlantı gösterir.  
        ![Fotoğraf Galerisi proje mobil desteğiyle](whats-new-in-aspnet-mvc-4/_static/image23.png "mobil desteğiyle Fotoğraf Galerisi projesi")

        *Fotoğraf Galerisi proje mobil desteği*
4. Mobil paketleri kaydedin. Bunu yapmak için açın **Global.asax.cs** dosya ve aşağıdaki satırı ekleyin.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - kayıt mobil paketleri*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. Bir masaüstü web tarayıcısı kullanarak uygulamayı çalıştırın.
6. Açık **Windows Phone 7 öykünücüsü** bulunan **Başlat menüsü | Tüm Programlar | Windows Phone SDK 7.1 | Windows Phone öykünücüsü.**
7. Telefon başlangıç ekranında da Internet Explorer'ı açın. Uygulama başlatıldığı URL denetleyin ve telefon tarayıcı bu URL'yi gidin (örneğin `http://localhost:[PortNumber]/`).

    JQuery.Mobile.MVC projenizdeki mobil aygıtlar için en iyi duruma getirilmiş görünümleri göster yeni varlıklar oluşturan gibi uygulamanızı Windows Phone öykünücüsünde farklı göründüğünden emin fark edeceksiniz.

    Masaüstü görünümüne geçiş bağlantısını gösteren telefon üstündeki iletisini görürsünüz. Ayrıca,  **\_Layout.Mobile.cshtml** yüklediğiniz paketi tarafından oluşturulmuş düzeni uygulamada farklı bir düzen dahil.

    > [!NOTE]
    > Şu ana kadar mobil görünümüne dönmek için bağlantı yok. Sonraki sürümlerde de dahil edilir.

    ![Fotoğraf Galerisi giriş sayfasının mobil Görünüm](whats-new-in-aspnet-mvc-4/_static/image24.png "Fotoğraf Galerisi giriş sayfasının mobil Görünüm")

    *Fotoğraf Galerisi giriş sayfasının mobil Görünüm*
8. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Görev 2 - mobil görünümlerini oluşturma

Bu görevde, mobil cihazlarda iyi appareance için uyarlanmış içerikle dizin görünümünün mobil bir sürümünü oluşturur.

1. Kopya **Views\Home\Index.cshtml** görüntülemek ve bir kopya oluşturun, yeni dosyayı yeniden adlandırmak için Yapıştır **Index.Mobile.cshtml**.
2. Açık yeni oluşturulan **Index.Mobile.cshtml** görüntülemek ve varolan &lt;ul&gt; bu kodu etiketi. Bunu yaparak, güncelleştirmekte &lt;ul&gt; jQuery mobil temalardan kullanmak üzere jQuery mobil veri ek açıklamaları etiketi.


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. Tuşuna **CTRL + S** değişiklikleri kaydedin.
4. Geçiş **Windows Phone öykünücüsü** ve site yenileyin. Yeni Görünüm ve yapısını galeri listesinin yanı sıra üstte bulunan yeni arama kutusuna dikkat edin. Bir sözcük arama kutusuna yazın (örneğin, **Tulips**) Fotoğraf Galerisi bir aramayı başlatmak için.

    ![Filtre ile ListView stili kullanılarak galeri](whats-new-in-aspnet-mvc-4/_static/image25.png "filtreleme ile listview stili kullanılarak Galerisi")

    *Filtre ile ListView stili kullanılarak Galerisi*

    Özetlemek için Görünüm Mobilizer tarif dizin görünümünün bir kopyasını oluşturmak için kullandığınız &quot;mobil&quot; soneki. Bu soneki ASP.NET MVC 4 ile bir mobil cihaz aracılığıyla oluşturulan her istek dizini bu kopyası kullanacağını gösterir. Ayrıca, jQuery Mobile site görünüm mobil cihazlarda geliştirme için kullanılacak dizini görünümün mobil sürümü güncelleştirdiniz.
5. Visual Studio geri gidin ve açık **Site.Mobile.css** altında bulunan **içerik** klasör.
6. Görüntü sağ tarafında Göster yapmak için Fotoğraf Başlığı konumlandırma düzeltin. Bunu yapmak için aşağıdaki kodu ekleyin **Site.Mobile.css** dosya.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Tuşuna **CTRL + S** değişiklikleri kaydedin.
8. Dönmek **Windows Phone öykünücüsü** ve site yenileyin. Fotoğraf Başlığı düzgün şimdi konumlandırılır dikkat edin.

    ![Görüntünün sağına konumlandırılmış başlık](whats-new-in-aspnet-mvc-4/_static/image26.png "görüntünün sağına konumlandırılmış başlığı")

    *Görüntünün sağına konumlandırılmış başlığı*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Görev 3 - jQuery Mobile temaları

Her düzeni ve pencere öğesinde jQuery Mobile tam birleşik görsel tasarım tema sitelere ve uygulamalara uygulamak olanaklı kılan bir yeni nesne yönelimli CSS framework çevresinde tasarlanmıştır.

jQuery Mobile'nın varsayılan tema harf verilen 5 örnekleri içerir (e) hızlı başvuru için a b c d.

Bu görevde, varsayılandan farklı bir tema kullanılacak mobil düzenini güncelleştirir.

1. Visual Studio'ya geri çevirin.
2. Açık  **\_Layout.Mobile.cshtml** bulunan dosya **görünümler/paylaşılan**.
3. Div öğesinin ayarlamak veri rol bulur. &quot;sayfa&quot; ve güncelleştirme **veri tema** için &quot; **e**&quot;.


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. Tuşuna **CTRL + S** değişiklikleri kaydedin.
5. Siteyi Yenile **Windows Phone öykünücüsü** ve yeni renk düzenini dikkat edin.

    ![Farklı renk düzenini mobil düzeniyle](whats-new-in-aspnet-mvc-4/_static/image27.png "farklı renk düzenini mobil Düzen")

    *Farklı renk düzenini mobil Düzen*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Görev 4 - görünüm değiştirici bileşeni ve özelliklerini geçersiz kılma tarayıcı kullanma

Bir mobil iyileştirilmiş web sayfaları için metni bir şey Masaüstü görünümünde veya kullanıcıların bir masaüstü sürümüne geçiş sağlayan sitenin tam modu gibi olan bağlantı eklemek için kuralıdır. Bir örnek jQuery.Mobile.MVC paketi içeren **görünüm değiştirici** bu amaçla kullanılan bileşen  **\_Layout.Mobile.cshtml** görünümü.

![Masaüstü görünümüne geçiş yapmak için bağlantı](whats-new-in-aspnet-mvc-4/_static/image28.png "Masaüstü görünümüne geçiş yapmak için bağlantı")

*Masaüstü görünümüne geçiş yapmak için bağlantı*

Görünüm değiştirici adında yeni bir özellik kullanır **tarayıcı geçersiz kılma**. Bu özellik gerçekten geldikleri olandan farklı bir tarayıcıdan (kullanıcı aracısı) çıkıyormuş gibi istekleri kabul uygulamanızı sağlar.

Bu görevde, jQuery.Mobile.MVC ve ASP.NET MVC 4'te özelliklerini geçersiz kılma yeni tarayıcı tarafından eklenen bir görünüm değiştirici örnek uygulaması inceleyeceksiniz.

1. Visual Studio'ya geri çevirin.
2. Açık  **\_Layout.Mobile.cshtml** görünümü bulunan altında **görünümler/paylaşılan** klasör ve bir kısmi görünüm olarak başvurulan görünüm değiştirici bileşeni dikkat edin.

    ![Görünüm değiştirici bileşenini kullanarak mobil düzeni](whats-new-in-aspnet-mvc-4/_static/image29.png "görünüm değiştirici bileşenini kullanarak mobil düzeni")

    *Görünüm değiştirici bileşenini kullanarak mobil düzeni*
3. Açık  **\_ViewSwitcher.cshtml** kısmi görünüm.

    Kısmi görünüm yeni yöntemi kullanan **ViewContext.HttpContext.GetOverriddenBrowser()** web isteğini kökenini belirlemek ve Masaüstü veya mobil görünümlere ya da geçiş yapmak için karşılık gelen bağlantıyı göstermek için.

    **GetOverridenBrowser** yöntemi döndürür bir **HttpBrowserCapabilitiesBase** istek için ayarlanmış kullanıcı aracısı karşılık gelen örnek (gerçek veya geçersiz kılınan). Bu değer gibi özellikleri almak için kullanabileceğiniz **IsMobileDevice**.

    ![ViewSwitcher kısmi Görünüm](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher kısmi görünümü")

    *ViewSwitcher kısmi görünümü*
4. Açık **ViewSwitcherController.cs** sınıfı bulunan **denetleyicileri** klasör. Bu SwitchView eylemi kullanıma ViewSwitcher bileşen bağlantıyı tarafından çağrılır ve yeni HttpContext yöntemleri dikkat edin.

    - **HttpContext.ClearOverridenBrowser()** yöntemi, tüm geçerli istek için geçersiz kılınan Kullanıcı aracısını kaldırır.
    - **HttpContext.SetOverridenBrowser()** yöntemi isteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar.  
        ![ViewSwitcher denetleyicisi](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher denetleyicisi")  
*ViewSwitcher denetleyicisi*

        Tarayıcı geçersiz kılınıyor çekirdek, bir ASP.NET MVC 4, jQuery.Mobile.MVC paket yüklemeyin olsa da kullanılabilir olduğu özelliğidir. Ancak, bu özellik yalnızca görünüm, Düzen ve kısmi görünüm etkiler ve Request.Browser nesneye bağlı özelliklerinden herhangi birini etkilemez.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Görev 5 - görünüm değiştirici Masaüstü görünümünde ekleme

Bu görevde, masaüstü düzeni görünüm değiştirici içerecek şekilde güncelleştirir. Bu Masaüstü görünümünde gezinirken mobil görünüme geri dönmek mobil kullanıcılar izin verir.

1. Siteyi Yenile **Windows Phone öykünücüsü**.
2. Tıklayın **Masaüstü görünümünde** galeri üstündeki bağlantı. Görünüm değiştirici mobil görünümüne dönmek izin vermek için Masaüstü görünümünde fark.
3. Visual Studio geri gidin ve açık  **\_Layout.cshtml** görünümü.
4. Oturum açma bölümü bulun ve işlemek için bir çağrı ekleyin  **\_ViewSwitcher** kısmi görünüm aşağıdaki  **\_LogOnPartial** kısmi görünüm. Ardından, basın **CTRL + S** değişiklikleri kaydedin.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. Tuşuna **CTRL + S** değişiklikleri kaydedin.
6. Windows Phone öykünücüsü içinde sayfayı yenileyin ve yakınlaştırmak için ekran'ı çift tıklatın. Giriş sayfası artık gösterir bildirimi **mobil Görünüm** mobil masaüstü görünümüne geçer bağlantı.

    ![Masaüstü görünümünde çizilir değiştirici görüntülemek](whats-new-in-aspnet-mvc-4/_static/image32.png "Masaüstü görünümünde işlenmiş görünüm değiştirici")

    *Masaüstü görünümünde işlenmiş görünüm değiştirici*
7. Yeniden mobil görünümüne geçin ve göz <strong>hakkında</strong> sayfa (http://localhosthakkında [bağlantı noktası] / Home /). About.Mobile.cshtml görünüm oluşturmadınız olsa bile hakkında sayfa mobil düzenini kullanarak görüntülendiğini fark (\_Layout.Mobile.cshtml).

    ![Sayfayla ilgili](whats-new-in-aspnet-mvc-4/_static/image33.png "sayfa hakkında")

    *Sayfa hakkında*
8. Son olarak, site masaüstü bir Web tarayıcısında açın. Önceki güncelleştirmelerinin hiçbiri Masaüstü görünümünde etkiledi dikkat edin.

    ![Fotografgalerisi Masaüstü görünümünde](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotografgalerisi Masaüstü görünümü")

    *Fotografgalerisi Masaüstü görünümü*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Görev 6 - oluşturma yeni görüntü modları

Yeni görüntüleme modları özellik isteği oluşturma tarayıcı bağlı olarak görünümleri seçin uygulama olanak sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfası isterse, uygulama döndürülecek **Views\Home\Index.cshtml** şablonu. Bir mobil tarayıcı giriş sayfası isterse, uygulama daha sonra döndürülecek **Views\Home\Index.mobile.cshtml** şablonu.

Bu görev iPhone cihazları için özelleştirilmiş bir düzen oluşturacak ve iPhone cihazları gelen istekleri benzetimini yapmak gerekir. Bunu yapmak için ya da bir iPhone öykünücüde/benzeticide kullanabilirsiniz (gibi [elektrik mobil Simulator](http://www.electricplum.com/)) veya bir tarayıcı kullanıcı aracısı değiştirme eklentilere. İPhone benzetmek yönergeler için bir Safari tarayıcı kullanıcı aracısı dizesi ayarlamak, bkz: [IE olmasından içeriğini Safari izin vermek nasıl](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison'ın Web günlüğündeki.

**Bu görev isteğe bağlıdır ve yürütme olmadan Laboratuvar devam edebilirsiniz dikkat edin.**

1. Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.
2. Açık **Global.asax.cs** ve aşağıdaki ekleme deyimini kullanarak.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. Aşağıdaki vurgulanmış kodu uygulamasına ekleme\_Başlat yöntemi.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - iPhone DisplayMode*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. Bir kopyasını oluşturmak  **\_Layout.Mobile.cshtml** dosyasını **görünümler/paylaşılan** klasörü ve kopyalayın adlandırın &quot; **\_Layout.iPhone.csthml**&quot;.
5. Açık  **\_Layout.iPhone.csthml** önceki adımda oluşturduğunuz.
6. Div öğesinin ayarlamak veri role özniteliğini bulmak **sayfa** değiştirip **veri tema** özniteliğini &quot; **bir**&quot;.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. Tuşuna **F5** uygulamayı çalıştırın ve sitede gözatmak için **Windows Phone öykünücüsü**.
8. Açık bir **iPhone benzeticisi** (bkz [ek C](#AppendixC) yükleme ve bir iPhone benzeticisi yapılandırma hakkında yönergeler için) ve siteye çok göz atın. Her telefon belirli bir şablon kullandığını dikkat edin.

    ![Using-Different-Views-for-each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Her mobil cihaz için farklı görünümleri kullanma*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Alıştırma 4: Zaman uyumsuz denetleyicileri kullanma

Microsoft .NET Framework 4.5 C# ve Visual Basic .NET programlama asynchrony için yeni bir temel sağlamaya yönelik yeni dil özellikleri sunar. Bu yeni temel zaman uyumsuz programlama-benzer ve yaklaşık olarak kolay olarak - zaman uyumlu programlama hale getirir. Şimdi kullanarak ASP.NET MVC 4'te zaman uyumsuz eylem yöntemleri yazabilmesi **AsyncController** sınıfı. CPU olmayan istekleri bağlı, uzun süre çalışan için zaman uyumsuz eylem yöntemlerini kullanabilirsiniz. Bu istek gerçekleştirilirken iş gerçekleştirmeyi Web sunucusu engelleme önler. AsyncController sınıfı genellikle uzun süre çalışan Web hizmeti çağrıları için kullanılır.

Bu alıştırmada, ASP.NET MVC 4'te zaman uyumsuz işlem temelleri açıklanır. Daha ayrıntılı bilgi edinmek istiyorsanız, aşağıdaki makaleyi denetleyebilirsiniz: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Görev 1 - zaman uyumsuz bir denetleyici uygulama

1. Açık **başlamak** çözüm bulunan **kaynak/Ex4-zamanuyumsuz/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **HomeController.cs** sınıfıyla **denetleyicileri** klasör.
3. Aşağıdaki ekleme deyimini kullanarak.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. Güncelleştirme **HomeController** devralmak için sınıf **AsyncController**. Zaman uyumsuz isteklerini işlemek ASP.NET AsyncController türetilen denetleyicileri etkinleştirmek ve hizmet zaman uyumlu eylem yöntemleri hala yapabilir.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. Ekleme **zaman uyumsuz** anahtar **dizin** yöntemi ve dönüş türü hale **görev&lt;ActionResult&gt;**.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. Değiştir **istemci. GetAsync()** sürümünü kullanarak tam zaman uyumsuz çağrı await anahtar sözcüğü aşağıda gösterildiği gibi.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - GetAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. Aşağıda gösterildiği gibi yeni kodu ile satırları değiştirerek zaman uyumsuz bir uygulama ile devam etmek için kodu değiştirin

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - ReadAsStringAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. Uygulamayı çalıştırın. Hiçbir önemli değişiklikler fark edeceksiniz, ancak kodunuzu daha iyi bir sunucu kaynaklarının kullanımını yapma ve performansı iyileştirme iş parçacığı havuzunun bir iş parçacığından engellemez.

    > [!NOTE]
    > Laboratuvara yeni zaman uyumsuz programlama özellikler hakkında daha fazla bilgiyi &quot; **C# ve Visual Basic ile .NET 4.5 zaman uyumsuz programlama** &quot; Visual Studio eğitim Seti'nde yer.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Görev 2 - iptal belirteçlerini zaman aşımlarını işleme

Görev örnekleri döndüren zaman uyumsuz eylem yöntemleri zaman aşımlarını de destekler. Bu görevde bir iptal belirteci kullanarak bir zaman aşımı senaryoda işlemek için dizin yöntemi kod güncelleştirir.

1. Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.
2. Aşağıdaki using deyimi için **HomeController.cs** dosya.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. Güncelleştirme almak için dizin eylemi bir **CancellationToken** bağımsız değişkeni.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. Güncelleştirme **GetAsync** Çağrı iptal belirtecini geçirin.

    (Kod parçacığını - *CancellationToken ile ASP.NET MVC 4 Laboratuvar - Ex04 - SendAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. İşaretleme *dizin* yöntemiyle bir **AsyncTimeout** özniteliği için 500 milisaniye olarak ayarlanmış ve **HandleError** işlemek üzere yapılandırılmış öznitelik  **TaskCanceledException** için yönlendirerek bir **süresi sona erdi** görünümü.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - öznitelikleri*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. Açık **PhotoController** sınıfı ve güncelleştirme **galeri** uzun çalışan bir görev benzetimini yapmak için yürütme 1000 miliseconds (1 saniye) gecikme yöntemi.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. Açık **Web.config** dosya ve özel hatalar, aşağıdaki öğeyi ekleyerek etkinleştirin.


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. Yeni bir görünüm oluşturma **görünümler/paylaşılan** adlı **süresi sona erdi** ve varsayılan düzenini kullanın. Çözüm Gezgini'nde sağ **görünümler/paylaşılan** klasörü ve select **Ekle | Görünüm**.

    ![Her mobil cihaz için farklı görünümleri](whats-new-in-aspnet-mvc-4/_static/image36.png "her mobil cihaz için farklı görünümleri kullanma")

    *Her mobil cihaz için farklı görünümleri kullanma*
9. Güncelleştirme **süresi sona erdi** aşağıda gösterildiği gibi içerik görüntüleyin.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. Uygulamayı çalıştırın ve kök URL'ye gidin. Eklediğiniz gibi bir **Thread.Sleep** 1000 milisaniye olarak tarafından oluşturulan bir zaman aşımı hatası alırsınız **AsyncTimeout** özniteliği ve tarafından catch **HandleError** özniteliği.

    ![Zaman aşımı özel durumun ele](whats-new-in-aspnet-mvc-4/_static/image37.png "işlenen zaman aşımı özel durumu")

    *İşlenen zaman aşımı özel durumu*

> [!NOTE]
> Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek D: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu hands-on-laboratuvarında, bazı yeni özellikler ASP.NET MVC 4'te yerleşik gözlenen. Aşağıdaki kavramlar ele:

- ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama proje şablonu geliştirmeler yararlanın
- HTML5 görünüm penceresinin özniteliği ve CSS medya sorguları mobil cihazlarda görünen geliştirmek için kullanın
- JQuery Mobile aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın
- Mobile özgü görünümler oluşturma
- Görünüm değiştirici bileşeni uygulamasında mobil ve Masaüstü görünümleri arasında geçiş yapmak için kullanın
- Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturun

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Ek A: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-aspnet-mvc-4/_static/image38.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](whats-new-in-aspnet-mvc-4/_static/image39.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](whats-new-in-aspnet-mvc-4/_static/image40.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](whats-new-in-aspnet-mvc-4/_static/image41.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için***

1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.
2. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
3. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](whats-new-in-aspnet-mvc-4/_static/image42.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](whats-new-in-aspnet-mvc-4/_static/image43.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Ek B: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](whats-new-in-aspnet-mvc-4/_static/image44.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express Web döşemeye*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Ek C: yükleme WebMatrix 2 ve iPhone benzeticisi

Bir sanal iPhone aygıt sitenizi çalıştırmak için WebMatrix genişletmesi kullanabilirsiniz &quot;iPhone elektrik mobil simülatörü&quot;. Ayrıca, Visual Studio 2012'den simulator çalıştırmak için aynı uzantı yapılandırabilirsiniz.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Görev 1 - yükleme WebMatrix 2

1. Git [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>WebMatrix 2</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![WebMatrix 2 yükleme](whats-new-in-aspnet-mvc-4/_static/image49.png "yükleme WebMatrix 2")

    *WebMatrix 2 yükleyin*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul](whats-new-in-aspnet-mvc-4/_static/image50.png "lisans koşulları kabul ediliyor")

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image51.png "yükleme ilerleme durumu")

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image52.png "yüklemesi tamamlandı")

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Görev 2 - iPhone benzeticisi uzantı yükleme

1. Çalıştırma **WebMatrix** ve varolan bir Web sitesini açın veya yeni bir tane oluşturun.
2. Tıklatın **çalıştırmak** gelen düğmesini **giriş** Şerit ve seçin **yeni Ekle**.

    ![Yeni WebMatrix genişletmesi ekleme](whats-new-in-aspnet-mvc-4/_static/image53.png "yeni WebMatrix genişletmesi ekleme")

    *Yeni WebMatrix genişletmesi ekleme*
3. Seçin **iPhone benzeticisi** tıklatıp **yükleme**.

    ![WebMatrix uzantıları gözatma](whats-new-in-aspnet-mvc-4/_static/image54.png "gözatma WebMatrix uzantıları")

    *WebMatrix uzantıları gözatma*
4. Paket ayrıntılarını tıklatın **yükleme** uzantısını yükleme işlemine devam etmek için.

    ![iPhone benzeticisi uzantısı](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone benzeticisi uzantısı")

    *iPhone benzeticisi uzantısı*
5. Okuyun ve uzantı EULA kabul edin.

    ![WebMatrix genişletmesi EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix genişletmesi EULA")

    *WebMatrix genişletmesi EULA*
6. Şimdi, iPhone benzeticisi seçeneğini kullanarak Web sitenizi Webmatrix'ten çalıştırabilirsiniz.

    ![İPhone kullanarak çalışan](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone kullanarak çalıştırma")

    *İPhone kullanarak çalıştırma*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Görev 3 - iPhone benzeticisi çalıştırmak için Visual Studio 2012 yapılandırma

1. Açık **Visual Studio 2012** ve herhangi bir Web sitesini açın veya yeni bir proje oluşturun.
2. Çalışma düğmesinden aşağı oka tıklayın ve **ile Gözat**.

    ![İle Gözat](whats-new-in-aspnet-mvc-4/_static/image58.png "ile Gözat")

    *İle Gözat*
3. İçinde &quot;Gözat ile&quot; iletişim kutusunda, tıklatın **Ekle**.
4. İçinde &quot;Program Ekle&quot; iletişim kutusunda, aşağıdaki değerleri kullanın:

   - <strong>Program</strong>: C:\Users\*{Currentuser'a}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (yolunu uygun şekilde güncelleştirin)</em>
   - **Bağımsız değişkenler**: &quot;1&quot;
   - **Kolay ad**: iPhone benzeticisi

     ![Ekle program](whats-new-in-aspnet-mvc-4/_static/image59.png "Ekle programı")

     *İle göz atmak için program Ekle*
5. Tıklatın **Tamam** ve iletişim kutularını kapatın.
6. Şimdi, Web uygulamalarınızı iPhone benzeticisi Visual Studio 2012'den çalıştırabilir.

    ![İPhone benzeticisi ile Gözat](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone benzeticisi ile Gözat")

    *İPhone benzeticisi ile Gözat*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek D: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure portalında oturum açtığı](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açtığı*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image62.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image63.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](whats-new-in-aspnet-mvc-4/_static/image64.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](whats-new-in-aspnet-mvc-4/_static/image65.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-mvc-4/_static/image66.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](whats-new-in-aspnet-mvc-4/_static/image67.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-mvc-4/_static/image68.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Değişiklikleri onaylamak*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-aspnet-mvc-4/_static/image73.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-mvc-4/_static/image74.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-aspnet-mvc-4/_static/image75.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-aspnet-mvc-4/_static/image76.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
   - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
   - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
   - Yeni bir veritabanı adı girin: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-mvc-4/_static/image77.png "hedef bağlantı dizesi yapılandırma")

     *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-aspnet-mvc-4/_static/image78.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-aspnet-mvc-4/_static/image80.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

    ![Uygulama için Windows Azure yayımlanan](whats-new-in-aspnet-mvc-4/_static/image81.png "uygulama için Windows Azure yayımlanır")

    *Windows Azure için yayımlanan uygulama*
