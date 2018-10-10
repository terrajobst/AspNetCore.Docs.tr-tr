---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 sürümündeki yenilikler | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçeve olan ve...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9d5a51a5887ecbbc96fce1416b88aa849bc3674e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912728"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4 sürümündeki yenilikler

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4, tanınmış tasarım desenleri ve ASP.NET ve .NET framework gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçevedir. Bu yeni, dördüncü framework sürümü, mobil web uygulaması geliştirmeyi daha kolay hale odaklanmıştır.

Başlangıç olarak, yeni bir ASP.NET MVC 4 projesi oluşturduğunuzda var. Şimdi mobil cihazlar için özel olarak tek başına uygulama derlemek için kullanabileceğiniz bir mobil uygulaması proje şablonu Buna ek olarak, ASP.NET MVC 4 jQuery.Mobile.MVC NuGet paketi aracılığıyla jQuery Mobile ile entegre olur. jQuery Mobile, Windows Phone, iPhone, Android vb. dahil olmak üzere tüm popüler mobil cihaz platformları, uyumlu olan web uygulamaları geliştirmek için bir HTML5 tabanlı çerçevedir. Özelleştirmesi gerekiyorsa, Bununla birlikte, ASP.NET MVC 4 ayrıca farklı cihazlar için farklı görünümleri hizmet ve cihaza özgü iyileştirmeleri sağlamanıza olanak sağlar.

Bu uygulamalı bir laboratuvarda, ASP.NET MVC 4 ile başlar &quot;Internet uygulaması&quot; bir Fotoğraf Galerisi uygulaması oluşturmak için proje şablonu. Farklı mobil cihaz ve masaüstü web tarayıcıları ile uyumlu hale getirmek için jQuery Mobile ve ASP.NET MVC 4'ün yeni özelliklerini kullanarak uygulamayı kademeli olarak artırır. Kod oluşturma ve nasıl ASP.NET MVC 4 görev destekleyerek zaman uyumsuz eylem yöntemi yazmanızı kolaylaştırır için yeni kod tarifleri hakkında bilgi edineceksiniz&lt;ActionResult&gt; dönüş türü.

> [!NOTE]
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET 4.5 Web Forms yenilikleri](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama projesi şablon geliştirmeleri avantajlarından yararlanın
- Mobil cihazlarda görünen geliştirmek için CSS medya sorgular ve HTML5 Görünüm penceresi özniteliği kullanın
- JQuery Mobile için aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın.
- Mobile özel görünümlerini oluşturma
- Uygulamasında mobil ve Masaüstü görünüm arasında geçiş yapmak için Görünüm değiştirici bileşenini kullan
- Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturma

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).
- [ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 yüklemesine dahil)
- Windows Phone öykünücüsü'nü (dahil [7.1.1 Windows Phone SDK'sı](https://www.microsoft.com/download/details.aspx?id=29233))
- İsteğe bağlı - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) ile **Electric Plum iPhone simülatörü** uzatma (yalnızca web uygulaması ile bir iPhone benzeticisi göz atmak için kullanılan alıştırma 3)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Kolaylık olması için bu kodu çoğu Visual Studio kodu Visual Studio içinden el ile eklemek zorunda kalmamak için kullanabileceğiniz kod parçacıkları sağlanır.

Kod parçacıkları yüklemek için:

1. Bir Windows Explorer penceresi açın ve Laboratuvar için Gözat **Source\Setup** klasör.
2. Çift **Setup.cmd** Visual Studio kod parçacıkları yüklemek için bu klasördeki bir dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek A: kullanarak kod parçacıklarına](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [Yeni ASP.NET MVC 4 proje şablonları](#Exercise1)
2. [Fotoğraf Galerisi Web uygulaması oluşturma](#Exercise2)
3. [Mobil cihazlar için destek ekleme](#Exercise3)
4. [Zaman uyumsuz denetleyicilerini kullanma](#Exercise4)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Alıştırma 1: Yeni ASP.NET MVC 4 proje şablonları

Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz. Internet uygulaması şablonu ek olarak, MVC 3, zaten var. bu sürümü artık mobil uygulamalar için ayrı bir şablon içerir. İlk olarak, ilgili bazı özellikleri şablonlarının her biri, görünecektir. Ardından, sayfanız farklı platformlarda düzgün bir şekilde doğru yaklaşımı kullanarak işleme üzerinde çalışır.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Görev 1 - Internet uygulama şablonu keşfetme

1. Açık **Visual Studio**.
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.** Projeyi adlandırın **Fotografgalerisi**, bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam**.

    > [!NOTE]
    > Daha sonra artık oluşturmakta olduğunuz Fotografgalerisi ASP.NET MVC 4 çözüm özelleştireceksiniz.

    ![Yeni proje oluşturma](whats-new-in-aspnet-mvc-4/_static/image1.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**. Razor görünüm altyapısı seçtiğinizden emin olun.

    ![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")

    *Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*

    > [!NOTE]
    > Razor sözdizimi, ASP.NET MVC 3'te sunulmuştur. Karakterler ve tuş vuruşları gerekli bir dosya içinde hızlı ve iş akışı kodlama sıvı etkinleştirme sayısını en aza indirmek için hedefi sağlamaktır. Razor yararlanır mevcut C# / VB (veya diğer) dil becerilerine ve harika bir HTML oluşturma iş akışı sağlayan bir şablon işaretleme söz dizimi sağlar.
4. Tuşuna **F5** yenilenen şablonları görmek ve çözümü çalıştırın. Aşağıdaki özellikleri kontrol edebilirsiniz:

    **Modern stili şablonları**

    Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.

    ![ASP.NET MVC 4 restyled şablonları](whats-new-in-aspnet-mvc-4/_static/image3.png "şablonları restyled MVC 4")

    *ASP.NET MVC 4 restyled şablonları*

    ![Yeni kişi sayfası](whats-new-in-aspnet-mvc-4/_static/image4.png "yeni kişi sayfası")

    *Yeni kişi sayfası*

    **Uyarlamalı işleme**

    Tarayıcı penceresini yeniden boyutlandırmadan kullanıma denetleyin ve sayfa düzeni için yeni pencere boyutunu dinamik olarak Uyarlanır nasıl dikkat edin. Bu şablonlar, hem Masaüstü hem de mobil platformlarda özelleştirme olmadan düzgün bir şekilde işlemek için Uyarlamalı işleme tekniği kullanın.

    ![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](whats-new-in-aspnet-mvc-4/_static/image5.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")

    *ASP.NET MVC 4 Proje şablonunda farklı tarayıcı boyutları*

    **JavaScript ile daha zengin kullanıcı Arabirimi**

    Varsayılan proje şablonları için başka bir geliştirme, JavaScript daha etkileşimli bir JavaScript sağlamak için kullanılır. Şablonda kullanılan oturum açma ve kaydetme bağlantıları nasıl doğrulamaları jQuery istemci-tarafı giriş alanları doğrulamada exemplify.

    ![jQuery doğrulaması](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery doğrulaması*

    > [!NOTE]
    > Bildirim, ilk bölümü olarak bölümlerde iki günlük sitesinden kayıtlı hesabıyla ve google (varsayılan olarak devre dışı) gibi başka bir kimlik doğrulama hizmeti kullanarak altenativelly oturum ikinci bölümü oturum açabilirsiniz.
5. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
6. Dosyayı açmak **AuthConfig.cs** altında bulunan **uygulama\_Başlat** klasör.
7. İçin Google istemcisini kaydetmek için son satırından açıklamayı kaldırın *OAuth* kimlik doğrulaması.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Herhangi bir Openıd veya OAuth hizmeti Facebook, Twitter, Microsoft, vb. kullanarak kimlik doğrulamasını kolayca etkinleştirebilirsiniz dikkat edin.
8. Tuşuna **F5** çözümü çalıştırın ve oturum açma sayfasına gidin.
9. Seçin **Google** oturum açmak için hizmet.

    ![Günlük hizmetini seçme](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Günlük hizmetini seçme*
10. Google hesabınızı kullanarak oturum açın.
11. Google hesabı bilgilerini almak site (localhost) sağlar.
12. Son olarak, Google hesabıyla ilişkilendirmek için sitesine kaydetmek gerekir.

   ![Google hesabınızı ilişkilendirin](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Google hesabınızı ilişkilendirme*
13. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
14. ASP.NET MVC 4 Proje şablonunda sunulan diğer bazı yeni özellikleri kullanıma çözümü şimdi keşfedin.

   ![ASP.NET MVC 4 Internet uygulaması proje şablonu](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")

   *ASP.NET MVC 4 Internet uygulaması proje şablonu*

   - **HTML 5 biçimlendirme**

       Yeni tema biçimlendirme bulabilmek için şablon görünümleri göz atın.

       ![Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu.")

       *Razor ve HTML5 biçimlendirme (About.cshtml) kullanarak yeni şablon.*
   - **Güncelleştirilmiş JavaScript kitaplıkları**

       ASP.NET MVC 4 varsayılan şablonu artık KnockoutJS, zengin oluşturmanızı sağlayan bir JavaScript MVVM çerçeve ve JavaScript ve HTML kullanarak yüksek derecede yanıt veren web uygulamaları içerir. Gibi MVC3 jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.

     > [!NOTE]
     > Bu bağlantıda KnockOutJS Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Ayrıca, jQuery ve jQuery kullanıcı Arabirimi hakkında bilgi edinebilirsiniz, [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Görev 2 - mobil uygulama şablonu keşfetme

ASP.NET MVC 4 Web siteleri için mobil ve tablet tarayıcılar geliştirilmesini kolaylaştırır. Bu şablon Internet uygulaması şablonu (denetleyici kodlarının neredeyse aynı olduğuna dikkat edin) aynı uygulama yapısını var, ancak kendi stili dokunmatik tabanlı mobil cihazlarda düzgün işlenecek değiştirildi.

1. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.** Projeyi adlandırın **PhotoGallery.Mobile**, bir konum seçin (veya varsayılan değeri bırakın), seçin &quot;eklemek için çözüm&quot; tıklatıp **Tamam**.
2. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **mobil uygulama** proje şablonu ve tıklatın **Tamam**. Razor görünüm altyapısı seçtiğinizden emin olun.

    ![Yeni bir ASP.NET MVC 4 mobil uygulama oluşturma](whats-new-in-aspnet-mvc-4/_static/image11.png "yeni bir ASP.NET MVC 4 mobil uygulama oluşturma")

    *Yeni bir ASP.NET MVC 4 mobil uygulama oluşturma*
3. Artık çözümü keşfedin ve bazı mobil cihazlar için ASP.NET MVC 4 çözüm şablonu tarafından sunulan yeni özellikleri kullanıma şunlardır:

    - **jQuery Mobile kitaplığı**

        Mobil uygulaması proje şablonu, mobil tarayıcı uyumluluğu için bir açık kaynak kitaplığı jQuery Mobile kitaplığı içerir. jQuery Mobile kademeli geliştirmeyi, CSS ve JavaScript destekleyen mobil tarayıcılar için geçerlidir. Kademeli geliştirmeyi yalnızca Zengin içeriği görüntülemek en güçlü tarayıcılar olanak sağlarken temel bir web sayfası içeriği görüntülemek kullanılan tüm tarayıcılar sağlar. JQuery Mobile stili içinde bulunan JavaScript ve CSS dosyalarına içeriği, sayfa biçimlendirme içinde herhangi bir değişiklik yapmadan ekrana sığacak şekilde mobil tarayıcılar yardımcı olur.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *jQuery mobile kitaplığı şablona dahil*
    - **HTML5 tabanlı biçimlendirme**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *HTML5 biçimlendirme, (Login.cshtml ve Index.cshtml) mobil uygulama şablonu*
4. Tuşuna **F5** çözümü çalıştırın.
5. Açık **Windows Phone 7 öykünücüsü**.
6. Telefonunuzun Başlangıç ekranına Internet Explorer'ı açın. Masaüstü uygulaması başlatıldığı URL denetleyin ve telefonunuzdan bu URL'ye gidin (örn `http://localhost:[PortNumber]/`).
7. Oturum açma sayfasına girin ya da kullanıma göre sayfa hakkında. Mobil cihazlar için yeni Metro uygulaması Web sitesinin stili dayalı dikkat edin. ASP.NET MVC 4 proje şablonu, tüm öğelerin görünür ve etkin olduğundan emin yapma, mobil cihazlarda düzgün şekilde görüntülenir. Üst bağlantılar tıklandığında veya dokunulduğunda büyük olduğuna dikkat edin.

    ![Proje şablonunu bir mobil cihaz sayfalarında](whats-new-in-aspnet-mvc-4/_static/image14.png "proje şablonu sayfalarında bir mobil cihaz")

    *Proje şablonu sayfalarından bir mobil cihaz*
8. Yeni şablonu da kullanır **Görünüm penceresi meta etiketi**. Çoğu mobil Tarayıcı tanımlamak sanal tarayıcı penceresi için bir genişlik veya &quot;Görünüm penceresi&quot;, mobil cihazın gerçek genişliğinden daha büyük olduğu. Bu sanal görüntü içinde tüm web sayfasını görüntülemek mobil tarayıcılar sağlar. **Görünüm penceresi meta etiketi** genişlik, yükseklik ve tarayıcı alanının ölçeği, mobil cihazlarda ayarlamak, web geliştiricilerinin sağlayan **.** Mobil uygulamalar için ASP.NET MVC 4 şablon cihaz genişliğine görünüm penceresinin ayarlar (&quot;genişliği cihaz width =&quot;) düzeni şablondaki (*görünümler/paylaşılan\_Layout.cshtml*), böylece tüm cihaz ekranı genişliğine ayarlayın, Görünüm penceresi sayfaları olacaktır. Görünüm penceresi meta etiketi varsayılan tarayıcı görünümü değiştirmez dikkat edin.
9. Açık  **\_Layout.cshtml**, bulunan **görünümleri | Paylaşılan** klasörü ve açıklama Görünüm penceresi meta etiketi. Uygulamayı çalıştırmak yoksa zaten açılmış ve farklılıkları denetleyin.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site](whats-new-in-aspnet-mvc-4/_static/image15.png "Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site")

*Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site*
10. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.
11. Görünüm penceresi meta etiketi açıklamasını kaldırın.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Görev 3 - Uyarlamalı işleme kullanma

Bu görevde, herhangi bir özelleştirme olmadan aynı anda bir Web sayfası mobil cihazlar ve tarayıcılar üzerinde doğru şekilde işlemek için başka bir yöntem öğreneceksiniz. Görünüm penceresi meta etiketi ile benzer bir amaç zaten kullandınız. Artık başka bir güçlü yöntemi karşılar: *Uyarlamalı işleme*.

Uyarlamalı işleme kullanan bir teknik olduğu **CSS3 media sorguları** bir sayfaya uygulanan stilini özelleştirmek için. Medya sorgular, belirli bir koşul altında CSS stilleri gruplandırma bir stil sayfası içinde koşulları tanımlayın. Yalnızca koşul true olduğunda, stil bildirilen nesnelere uygulanır.

Site farklı cihazlarda görüntülemek için herhangi bir özelleştirme Uyarlamalı işleme teknikleri tarafından sağlanan esneklik sağlar. Tek bir stil sayfasında mantığı kod yazmaya gerek kalmadan stili seçmek için istediğiniz sayıda stilleri tanımlayabilirsiniz. Yinelenen kod ve amacıyla işlemek için mantığı miktarını azaltır olarak bu nedenle, sayfa stilleri uyarlama bir çok masaüstünüzdeki yoludur. CSS dosyaları boyutunu fazladır büyüyebilir gibi diğer taraftan, bant genişliği tüketimi, artırır.

Uyarlamalı oluşturma tekniği kullanarak, siteniz olacaktır **düzgün bir şekilde bağımsız olarak tarayıcı görüntülenir.** Ancak, ek bant genişliği yüklerseniz önemli olduğu düşünmelisiniz.

> [!NOTE]
> Medya sorgusu temel biçimi: @media \[kapsamı: tüm | el | yazdırma | projeksiyon | ekran\] ([özellik: değer] ve... [özellik: değer])


Medya sorgularının örnekleri: &gt;  <strong>@media tüm ve (max-width: 1000px) ve (en az Genişlik: 700px) {}:</strong> 700px 1000px arasındaki tüm çözümler için.

> <strong>@media ekran ve (min-width: 400px) ve (max-width: 700px) {…}:</strong> yalnızca ekranlar için. Çözüm 700px ile 400 arasında olmalıdır.
> 
> <strong>@media taşınabilir ve (en az Genişlik: 20em), ekranı ve (en az Genişlik: 20em) {…}:</strong> el bilgisayarlarında çalışmak (Mobil ve cihazlar) ve ekranlar için. Minimum genişliğini 20em büyük olmalıdır.
> 
> Bu konu hakkında daha fazla bilgi bulabilirsiniz [W3C site](http://www.w3.org/TR/css3-mediaqueries/).


Okunurluğunu ASP.NET MVC 4 Web sitesi şablonu varsayılan, Uyarlamalı işleme nasıl çalıştığını şimdi keşfedin.

1. Açık **PhotoGallery.sln** görev 1'den oluşturdunuz ve seçin çözüm **Fotografgalerisi** proje. Tuşuna **F5** çözümü çalıştırın.
2. Tarayıcının genişliği windows yarısı veya küçüktür özgün boyutuna çeyreği ayarlama, yeniden boyutlandırın. Üstbilgi öğeleri ile neler olduğuna dikkat edin: bazı öğeleri üstbilgi görünür alanında görünmez.
3. Açık <strong>Site.css</strong> bulunan Visual Studio Çözüm Gezgini'nde, bir dosyadan <strong>içerik</strong> proje klasörü. Tuşuna <strong>CTRL + F</strong> Visual Studio tümleşik arama açın ve yazmak için <strong>@media</strong> bulunacak <strong>CSS medya sorgusu</strong>.

    Bu şablonda tanımlanan medya sorgu koşulu bu şekilde çalışır: tarayıcının pencere boyutunu olduğunda aşağıda **850 px**, uygulanan CSS kurallarını bu medya blok içinde tanımlanan olanlardır.

    ![Medya sorgusu bulma](whats-new-in-aspnet-mvc-4/_static/image16.png "medya sorgusu bulma")

    *Medya sorgusu bulma*
4. Max-width öznitelik değeri içinde 850 ayarlamak yerine piksel ile **10px**, Uyarlamalı işleme devre dışı bırakmak için basın **CTRL + S** değişiklikleri kaydetmek için. Dönüş tuşuna basın ve tarayıcı **CTRL + F5 tuşlarına basarak** yaptığınız değişikliklerle sayfayı yenilemek için. Her iki sayfa farklılıkları pencerenin genişliğini ayarlarken dikkat edin.

    ![Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa](whats-new-in-aspnet-mvc-4/_static/image17.png "sol, sayfanın uyguluyor @media stilde stil sağındaki atlanırsa")

    <em>Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa</em>

    Şimdi, şimdi mobil cihazlarda neler olduğunu kontrol edin:

    ![Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa](whats-new-in-aspnet-mvc-4/_static/image18.png "sol, sayfanın uyguluyor @media stilde stil sağındaki atlanırsa")

    <em>Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa</em>

    Bir Web tarayıcısında sayfa işlendiğinde değişiklikleri bir mobil cihazı kullanırken çok önemli olmadığını fark edeceksiniz rağmen farkları daha belirgin hale gelir. Özel bir stil okunabilirliği geliştirildi görüntü sol tarafında görebiliriz.

    Uyarlamalı işleme koşullu bir Web sitesi için stil oluşturma ve net bir yaklaşım ile ortak stil sorunlarını çözme uygulamak kolaylaştıran birçok daha fazla senaryoda kullanılabilir.

    Bu özelliklerden herhangi bir web uygulamasına olabilmesi için CSS medya sorgu ve Görünüm penceresi meta etiketi ASP.NET MVC 4'e özgü değildir.
5. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Alıştırma 2: Fotoğraf Galerisi Web uygulaması oluşturma

Bu alıştırmada, fotoğrafları görüntülemek için bir Fotoğraf Galerisi uygulamasında çalışmaz. ASP.NET MVC 4 proje şablonu ile başlar ve ardından bir hizmetten fotoğraf almak ve bunları giriş sayfasında görüntülemek için bir özellik ekleyeceksiniz.

Aşağıdaki alıştırmada, mobil cihazlarda görüntülenme biçimini geliştirmek için bu çözüm güncelleştirir.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Görev 1 - sahte fotoğraf hizmet oluşturma

Bu görevde, sahte galeride görüntülenecek içeriği almak için fotoğraf hizmeti oluşturacaksınız. Bunu yapmak için her fotoğraf verileri içeren bir JSON dosyası yalnızca döndüreceği yeni bir denetleyici ekleyeceksiniz.

1. Açık **Visual Studio** zaten açtıysanız.
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.** Projeyi adlandırın **Fotografgalerisi**, bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam**. Alternatif olarak, mevcut ASP.NET MVC 4 ile çalışmaya devam edebilirsiniz **Internet uygulaması** çözümünden **alıştırma 1** ve bir sonraki adımı atlayın.
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**. Razor görünüm altyapısı seçili olduğundan emin olun.
4. İçinde **Çözüm Gezgini**, sağ **uygulama\_veri** klasörü seçin ve proje **Ekle | Var olan öğe**. Göz atın **Source\Assets\App\_veri** bu Laboratuvar, klasör ve eklemek **Photos.json** dosya.
5. Adlı bir yeni denetleyici oluşturun **PhotoController**. Bunu yapmak için sağ **denetleyicileri** klasörüne gidin **Ekle** seçip **denetleyicisi.** Denetleyici adı tamamlamak, bırakın **boş MVC denetleyicisi** şablonu ve tıklatın **Ekle**.

    ![PhotoController ekleme](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController ekleme")

    *PhotoController ekleme*
6. Değiştirin **dizin** yöntemi aşağıdaki **galeri** eylem ve son eklediğiniz projeye JSON dosyasından içerik döndürülecek.

    (Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar galeri eylem*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Tuşuna **F5** çözümü çalıştırın ve ardından sahte fotoğraf hizmeti test etmek için aşağıdaki URL'ye gidin: `http://localhost:[port]/photo/gallery` (uygulamanın nerede başlatıldı geçerli bir bağlantı noktasında [bağlantı noktası] değeri bağlıdır). Bu istek içeriğini almak **Photos.json** dosya.

    ![Sahte fotoğraf hizmeti test](whats-new-in-aspnet-mvc-4/_static/image20.png "sahte fotoğraf hizmeti test etme")

    *Sahte fotoğraf hizmeti test etme*

Gerçek bir uygulamada kullanabileceğinizi [ASP.NET Web API](../../../../web-api/index.md) Fotoğraf Galerisi hizmeti uygulamak için. ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API'si, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Görev 2 - Fotoğraf Galerisi görüntüleme

Bu görevde, giriş sayfası, bu alıştırmada ilk görevde oluşturduğunuz sahte hizmetini kullanarak Fotoğraf Galerisi göstermek için güncelleştirilir. Model dosyaları ekleme ve galeri görünümleri güncelleştirme.

1. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.
2. Oluşturma **fotoğraf** sınıfını **modelleri** klasör. Bunu yapmak için sağ **modelleri** klasörüne **Ekle** tıklatıp **sınıfı**. Ardından, kümesinin adı **Photo.cs** tıklatıp **Ekle**.
3. Aşağıdaki üye ekleme **fotoğraf** sınıfı.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf modeli*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Açık **HomeController.cs** dosya **denetleyicileri** klasör.
5. Aşağıdaki using deyimlerini.

    (Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar HomeController kullanımları*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Güncelleştirme **dizin** eylemin kullanmasını **HttpClient** galeri verileri almak ve ardından **JavaScriptSerializer** görünüm modeli için seri durumdan çıkarılamıyor.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - dizin eylem*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Açık **Index.cshtml** altında bulunan dosya **görünümler/giriş** klasörünü ve tüm içeriğini aşağıdaki kodla değiştirin.

    Bu kodu hizmetten alınan tüm fotoğraflar aracılığıyla döngüye girer ve bunları sırasız bir listesini görüntüler.

    (Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar fotoğraf listesi*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. İçinde **Çözüm Gezgini**, sağ **içerik** klasörü seçin ve proje **Ekle | Var olan öğe**. Göz atın **Source\Assets\Content** bu Laboratuvar, klasör ve eklemek **Site.css** dosya. Değişimi onaylamanız gerekir. Varsa **Site.css** dosya açık, ayrıca dosyayı yeniden doğrulamak gerekir.
9. Dosya Gezgini'ni açın ve tamamını **fotoğraf** klasörünün altında **Source\Assets** klasör, bu Laboratuvarın Çözüm Gezgini'nde projenizin kök klasörüne.
10. Uygulamayı çalıştırın. Şimdi, galeride fotoğrafları görüntülemek giriş sayfası görmeniz gerekir.

    ![Fotoğraf Galerisi](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotoğraf Galerisi")

    *Fotoğraf Galerisi*
11. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Alıştırma 3: mobil cihazlar için destek ekleme

ASP.NET MVC 4'te anahtar güncelleştirmelerden biri mobil geliştirme desteğidir. Bu alıştırmada önceki alıştırmada oluşturduğunuz Fotografgalerisi çözüm genişleterek, ASP.NET MVC 4 mobil uygulamalar için yeni özellikleri inceleyeceksiniz.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Görev 1 - bir ASP.NET MVC 4 uygulamasında yükleme jQuery Mobile

1. Açık **başlamak** çözüm bulunan **kaynak/Ex3-MobileSupport/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Paket Yöneticisi Konsolu** tıklayarak **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**  menü seçeneği.

    ![NuGet Paket Yöneticisi konsolu açma](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet Paket Yöneticisi konsolu açma")

    *NuGet Paket Yöneticisi konsolu açma*
3. Paket Yöneticisi Konsolu'nda yüklemek için aşağıdaki komutu çalıştırın **jQuery.Mobile.MVC** paket.

    jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı arabirimini oluşturmaya yönelik bir açık kaynak kitaplığıdır. JQuery Mobile bir ASP.NET MVC 4 uygulama ile kullanma Yardımcıları jQuery.Mobile.MVC NuGet paketini içerir.

    > [!NOTE]
    > Aşağıdaki komutu çalıştırarak jQuery.Mobile.MVC kitaplığı Nuget'ten indirdiğiniz.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Bu komut, jQuery Mobile ve aşağıdakiler dahil olmak üzere bazı yardımcı dosyalar yüklenir:

    - **Görünümler/paylaşılan/\_Layout.Mobile.cshtml**: bir küçük ekranlar için optimize edilmiş bir jQuery Mobile tabanlı düzeni. Web sitesi, bir mobil tarayıcıda bir istek aldığında, orijinal düzenini değiştirir (\_Layout.cshtml) Bu bir.
    - Görünüm değiştirici bileşeni: oluşan **görünümler/paylaşılan/\_ViewSwitcher.cshtml** kısmi Görünüm ve **ViewSwitcherController.cs** denetleyicisi. Bu bileşen, kullanıcıların masaüstü sürümüne geçiş sağlamak için mobil tarayıcılarda bir bağlantı gösterilir.  
        ![Fotoğraf Galerisi proje mobil desteğiyle](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotoğraf Galerisi proje mobil desteği")

        *Fotoğraf Galerisi proje mobil desteği*
4. Mobil paketleri kaydedin. Bunu yapmak için açık **Global.asax.cs** dosyasını açıp aşağıdaki satırı ekleyin.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - kayıt mobil paketleri*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Bir masaüstü web tarayıcısını kullanarak uygulamayı çalıştırın.
6. Açık **Windows Phone 7 öykünücüsü'nü** bulunan **Başlat menüsündeki | Tüm Programlar | Windows Phone SDK 7.1 | Windows Phone öykünücüsü.**
7. Telefonunuzun Başlangıç ekranına Internet Explorer'ı açın. Uygulama başlatıldığı URL denetleyin ve söz konusu URL ile telefon tarayıcı göz atın (örneğin `http://localhost:[PortNumber]/`).

    JQuery.Mobile.MVC mobil cihazlar için iyileştirilmiş görünümlerini Göster, projenizdeki yeni varlıklar oluşturan gibi uygulamanızı Windows Phone öykünücüsü'nde, farklı görünür olduğunu fark edeceksiniz.

    İletinin üst kısmında telefon Masaüstü görünümüne geçer bağlantıyı gösteren dikkat edin. Ayrıca,  **\_Layout.Mobile.cshtml** düzenini yüklediğiniz paketi tarafından oluşturulan uygulamada farklı bir düzene dahil.

    > [!NOTE]
    > Şu ana kadar mobil görünüme geri dönmek için bağlantı yok. Sonraki sürümlerde eklenecektir.

    ![Fotoğraf Galerisi giriş sayfası mobil görünüme](whats-new-in-aspnet-mvc-4/_static/image24.png "mobil görünüme Fotoğraf Galerisi giriş sayfası")

    *Fotoğraf Galerisi giriş sayfası mobil Görünüm*
8. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Görev 2 - mobil görünüm oluşturma

Bu görevde, mobil cihazlarda iyi appareance için uyarlanmış içerikle dizin görünümünün mobil bir sürümünü oluşturur.

1. Kopyalama **Views\Home\Index.cshtml** görüntülemek ve bir kopya oluşturmak için yeni dosyayı yeniden adlandır yapıştırın **Index.Mobile.cshtml**.
2. Açık yeni oluşturulan **Index.Mobile.cshtml** görüntüleyin ve değiştirin &lt;ul&gt; bu kodla etiketi. Bunu yaparak, güncelleştiriliyor &lt;ul&gt; jQuery mobile temalarından kullanmak için jQuery Mobile veri ek açıklamaları etiketi.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Şunlara dikkat edin:
    > 
    > - **Veri rolü** özniteliğini **listview** listview stilleri kullanarak listesinin işleyecek.
    > 
    > - **Veri dışarı** yuvarlatılmış kenarlık ve kenar boşluğu listesiyle özniteliği true olarak ayarlanırsa gösterir.
    > 
    > - **Veri filtresi** özniteliğini **true** bir arama kutusu oluşturur.
    > 
    > Proje belgelerinde jQuery Mobile kuralları hakkında daha fazla bilgi edinebilirsiniz: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Tuşuna **CTRL + S** değişiklikleri kaydedin.
4. Geçiş **Windows Phone öykünücüsü** ve sitesini yenileyin. Yeni Azure görünümü ve deneyimini galeri listesinin yanı sıra üstte bulunan yeni arama kutusuna dikkat edin. Ardından, arama kutusuna bir sözcük yazın (örneğin, **Tulips**) bir arama Fotoğraf Galerisi başlatmak için.

    ![ListView stili filtreleme ile kullanarak galeri](whats-new-in-aspnet-mvc-4/_static/image25.png "filtreleme ile listview stil kullanarak Galerisi")

    *Galeri ListView stili filtreleme ile kullanma*

    Özetlemek gerekirse, görünüm Mobilizer tarif şekilde dizin görünümünün bir kopyasını oluşturmak için kullandığınız &quot;mobil&quot; soneki. Bu sonekin ASP.NET MVC 4'e bir mobil CİHAZDAN oluşturulan her bir istek bu kopyası dizini kullanacağını gösterir. Ayrıca, mobil cihazlarda site görünümü ve deneyimini geliştirmek için jQuery Mobile'ı kullanmak için dizin görünümünün mobil sürümü güncelleştirdik.
5. Visual Studio geri gidin ve açık **Site.Mobile.css** altında bulunan **içerik** klasör.
6. Resmin sağ tarafında Göster hale Fotoğraf Başlığı konumlandırma düzeltin. Bunu yapmak için aşağıdaki kodu ekleyin. **Site.Mobile.css** dosya.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Tuşuna **CTRL + S** değişiklikleri kaydedin.
8. Dönmek **Windows Phone öykünücüsü** ve sitesini yenileyin. Fotoğraf Başlığı düzgün artık konumlandırıldı dikkat edin.

    ![Resmin sağ tarafında konumlandırılan başlık](whats-new-in-aspnet-mvc-4/_static/image26.png "resmin sağ tarafında konumlandırılan başlığı")

    *Resmin sağ tarafında konumlandırılan başlığı*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Görev 3 - jQuery Mobile temalar

Her düzen ve pencere öğesinde jQuery Mobile tam birleşik görsel tasarım tema sitelere ve uygulamalara uygulamak mümkün kılan bir yeni nesne yönelimli CSS framework geçici olarak tasarlanmıştır.

jQuery Mobile'nın varsayılan tema harf verildiğinde 5 örneklerini içerir (e) hızlı başvuru için b, c, d.

Bu görevde, varsayılandan farklı bir tema mobil düzenin güncelleştirir.

1. Visual Studio'ya geçiş yapın.
2. Açık  **\_Layout.Mobile.cshtml** bulunan dosya **görünümler/paylaşılan**.
3. Div öğesinin ayarlamak veri rolüne sahip bulma &quot;sayfa&quot; ve güncelleştirme **data-theme** için &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Tuşuna **CTRL + S** değişiklikleri kaydedin.
5. Siteyi Yenile **Windows Phone öykünücüsü** ve yeni renk düzeni dikkat edin.

    ![Farklı renk şeması bir mobil düzenin](whats-new-in-aspnet-mvc-4/_static/image27.png "farklı renk şeması ile mobil Düzen")

    *Farklı renk şeması ile mobil Düzen*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Görev 4 - görünüm değiştirici bileşen ve tarayıcı geçersiz kılma özellikleri kullanma

Mobil için iyileştirilmiş web sayfaları için bir kuralı metni bir şey Masaüstü görünüm veya kullanıcıların bir masaüstü sürümüne geçiş olanak sağlayan tam site modu gibi olan bir bağlantı eklemektir. Bir örnek jQuery.Mobile.MVC paket içerir **görünüm değiştirici** bu amaçla kullanılan bileşen  **\_Layout.Mobile.cshtml** görünümü.

![Masaüstü görünümüne geçmek için bağlantı](whats-new-in-aspnet-mvc-4/_static/image28.png "Masaüstü görünümüne geçmek için bağlantı")

*Masaüstü görünümüne geç bağlama*

Görünüm değiştirici adlı yeni bir özellik kullanır **tarayıcı geçersiz kılma**. Bu özellik, bunlar gerçekten geldikleri olandan farklı bir tarayıcıdan (kullanıcı aracısı) çıkıyormuş gibi istekleri kabul uygulamanızı sağlar.

Bu görevde, örnek uygulaması jQuery.Mobile.MVC ve geçersiz kılma özellikleri ASP.NET MVC 4'te yeni tarayıcı tarafından eklenen bir görünüm değiştirici inceleyeceksiniz.

1. Visual Studio'ya geçiş yapın.
2. Açık  **\_Layout.Mobile.cshtml** görünümü altında yer **görünümler/paylaşılan** klasör ve bir kısmi görünüm olarak başvurulan görünüm değiştirici bileşen dikkat edin.

    ![Görünüm değiştirici bileşenini kullanarak mobil Düzen](whats-new-in-aspnet-mvc-4/_static/image29.png "görünüm değiştirici bileşenini kullanarak mobil Düzen")

    *Görünüm değiştirici bileşenini kullanarak mobil Düzen*
3. Açık  **\_ViewSwitcher.cshtml** kısmi görünüm.

    Kısmi görünüm yeni kullanmaktadır **ViewContext.HttpContext.GetOverriddenBrowser()** web isteğinin kaynağını belirlemek ve Masaüstü veya mobil görünümler için ya da geçiş yapmak için ilgili bağlantıyı göstermek için.

    **GetOverridenBrowser** yöntemi döndürür bir **HttpBrowserCapabilitiesBase** istek için ayarlanan kullanıcı aracısı karşılık gelen bir örneği (gerçek ya da geçersiz kılınan). Bu değer gibi özellikleri almak için kullanabileceğiniz **IsMobileDevice**.

    ![Kısmi görünüm ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher kısmi Görünüm")

    *ViewSwitcher kısmi Görünüm*
4. Açık **ViewSwitcherController.cs** sınıfı bulunan **denetleyicileri** klasör. Bu SwitchView eylemi kullanıma bağlantıya ViewSwitcher bileşen tarafından çağrılır ve yeni HttpContext yöntemleri dikkat edin.

    - **HttpContext.ClearOverridenBrowser()** yöntemi, geçerli istek için herhangi bir geçersiz kılınan Kullanıcı aracısını kaldırır.
    - **HttpContext.SetOverridenBrowser()** yöntemi isteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar.  
        ![ViewSwitcher denetleyicisi](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher denetleyicisi")  
*ViewSwitcher denetleyicisi*

        Tarayıcı geçersiz kılmak çekirdek ASP.NET MVC 4 jQuery.Mobile.MVC paketi yüklemezseniz bile aynı zamanda kullanılabilir olan özelliğidir. Ancak, bu özellik yalnızca görüntüleme, Düzen ve kısmi görünüm etkiler ve Request.Browser nesneye bağlı özelliklerinden herhangi birinin etkilemez.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Görev 5 - görünüm değiştirici Masaüstü görünüm ekleme

Bu görevde, Masaüstü düzenini görünüm değiştirici içerecek şekilde güncelleştirir. Bu Masaüstü görünümünde gezinirken mobil görünüme geri dönmek mobil kullanıcılar izin verir.

1. Siteyi Yenile **Windows Phone öykünücüsü**.
2. Tıklayarak **Masaüstü Görünüm** galerinin üstündeki bağlantısı. Görünüm değiştirici mobil görünüme dönmek izin vermek için Masaüstü görünümünde dikkat edin.
3. Visual Studio geri gidin ve açık  **\_Layout.cshtml** görünümü.
4. Oturum açma bölümü bulun ve işlemek için bir çağrı ekleyin  **\_ViewSwitcher** kısmi görünüm aşağıdaki  **\_LogOnPartial** kısmi görünüm. Ardından, basın **CTRL + S** değişiklikleri kaydedin.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Tuşuna **CTRL + S** değişiklikleri kaydedin.
6. Windows Phone öykünücüsü sayfayı yeniler ve ekran yakınlaştırmak için çift tıklayın. Giriş sayfası gösterdiğini bildirimi **mobil görünüme** mobil masaüstü görünümüne geçer bağlantı.

    ![Masaüstü görünümünde işlenen değiştirici görüntülemek](whats-new-in-aspnet-mvc-4/_static/image32.png "Masaüstü görünümünde oluşturulması görünüm değiştirici")

    *Masaüstü görünümünde oluşturulması görünüm değiştirici*
7. Göz atın ve mobil görünüme geçiş yeniden <strong>hakkında</strong> sayfa (http://localhosthakkında [bağlantı noktası] / Home /). About.Mobile.cshtml görünümü oluşturmadıysanız bile hakkında sayfası mobil düzen kullanılarak görüntülenir, dikkat edin (\_Layout.Mobile.cshtml).

    ![Sayfa hakkında](whats-new-in-aspnet-mvc-4/_static/image33.png "sayfa hakkında")

    *Sayfa hakkında*
8. Son olarak, site bir Masaüstü Web tarayıcısında açın. Önceki güncelleştirmelerinin hiçbiri Masaüstü görünüm etkiledi dikkat edin.

    ![Masaüstü görünüm Fotografgalerisi](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotografgalerisi Masaüstü Görünüm")

    *Fotografgalerisi Masaüstü Görünüm*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Görev 6 - oluşturma, yeni görüntü modları

Yeni görüntü modu özelliğini isteği oluşturma tarayıcı bağlı olarak görünümleri seçin bir uygulama sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfasını isterse, uygulama döndüreceği **Views\Home\Index.cshtml** şablonu. Daha sonra uygulama giriş sayfası mobil tarayıcı isteğinde bulunursa döndürür **Views\Home\Index.mobile.cshtml** şablonu.

Bu görevde, iPhone cihazları için özel bir düzen oluşturacak ve iPhone cihazları gelen istekleri benzetimini yapmak gerekir. Bunu yapmak için ya da bir iPhone öykünücüsü/simülatör kullanabilirsiniz (gibi [elektrik mobil simülatör](http://www.electricplum.com/)) veya bir tarayıcı kullanıcı aracısı değiştirme eklentiler ile. İPhone benzetmek bir Safari tarayıcı kullanıcı aracısı dizesi ayarlama konusunda yönergeler için [IE olduğu anlatabilirsiniz Safari sağlama](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David geçiş'ın blogunda.

**Bu görev isteğe bağlı olduğuna dikkat edin ve Laboratuvar yürütme olmadan devam edebilirsiniz.**

1. Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.
2. Açık **Global.asax.cs** ve aşağıdaki deyimi kullanarak.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Uygulamaya aşağıdaki vurgulanmış kodu ekleyin\_yöntemi başlatın.

    (Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Yeni kaydettiğiniz **adlı DefaultDisplayMode &quot;iPhone&quot;**, statik içinde **DisplayModeProvider.Instance.Modes** karşı eşleşen statik listesi her gelen istek. Gelen istek dize içeriyorsa &quot;iPhone&quot;, ASP.NET MVC görünümleri adını içeren bulacaksınız &quot;iPhone&quot; soneki. 0 parametresi, yeni modu nasıl özel olduğunu belirtir; Örneğin, bu görünümü genel ayrıntılı &quot;.mobile&quot; mobil cihazlardan gelen isteklere eşleşen kural.

Bir iPhone tarayıcısı, isteği oluşturduğunda bu kod çalıştırıldıktan sonra uygulamanızın kullanacağı **görünümler/paylaşılan\\_Layout.iPhone.cshtml** sonraki adımda oluşturacağınız düzeni.

> [!NOTE]
> İPhone için Tanıtım amaçlı Basitleştirilmiş ve (örneğin test büyük küçük harfe duyarlı olması nedeniyle), her iPhone kullanıcı aracısı dizesi beklendiği gibi çalışmayabilir, istek testinin bu şekilde.

4. Bir kopyasını oluşturmak  **\_Layout.Mobile.cshtml** dosyası **görünümler/paylaşılan** klasörü ve kopyalayın adlandırın &quot; **\_Layout.iPhone.csthml**&quot;.
5. Açık  **\_Layout.iPhone.csthml** önceki adımda oluşturduğunuz.
6. Div öğesinin ayarlanmış veri role özniteliğini bulmak **sayfa** değiştirip **data-theme** özniteliğini &quot; **bir**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Artık ASP.NET MVC 4 uygulamanızı 3 düzenlere sahip:

1. **\_Layout.cshtml**: masaüstü tarayıcıları için kullanılan varsayılan düzen.
2. **\_Layout.Mobile.cshtml**: mobil cihazlar için kullanılan varsayılan düzen.
3. **\_Layout.iPhone.cshtml**: ayırmak farklı renk düzenini kullanarak, iPhone cihazları için özel düzen \_Layout.mobile.cshtml.
7. Tuşuna **F5** uygulamayı çalıştırın ve site için göz atmayı **Windows Phone öykünücüsü**.
8. Açık bir **iPhone benzeticisi** (bkz [ek C](#AppendixC) yükleme ve bir iPhone benzeticisi yapılandırma konusunda yönergeler için) ve çok siteye göz atın. Belirli bir şablon her telefon kullandığını dikkat edin.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Tüm mobil cihazlar için farklı görünümleri kullanma*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Alıştırma 4: Zaman uyumsuz denetleyicilerini kullanma

Microsoft .NET Framework 4.5, C# ve Visual Basic .NET programlama zaman uyumsuzluk için yeni bir temel sağlayacak yeni dil özellikleri tanıtır. Bu yeni foundation zaman uyumsuz programlama-benzer ve yaklaşık olarak basit olarak - eşzamanlı programlama yapar. Şimdi kullanarak ASP.NET MVC 4'te zaman uyumsuz eylem yöntemleri yazabilmesi **AsyncController** sınıfı. Uzun süre çalışan zaman uyumsuz eylem yöntemlerini kullanabilirsiniz, CPU dışı istekleri bağlı. Bu, Web sunucusu isteği işlenirken iş gerçekleştirmeyi engelleme önler. AsyncController sınıf genellikle uzun süre çalışan Web hizmeti çağrıları için kullanılır.

Bu alıştırmada, ASP.NET MVC 4'te zaman uyumsuz işlem temelleri açıklanır. Bir daha ayrıntılı bilgi edinmek istiyorsanız aşağıdaki makaleye denetleyebilirsiniz: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Görev 1 - zaman uyumsuz bir denetleyiciyi uygulama

1. Açık **başlamak** çözüm bulunan **kaynak/Ex4-Async/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **HomeController.cs** gelen sınıfı **denetleyicileri** klasör.
3. Aşağıdaki using deyimi.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Güncelleştirme **HomeController** devralınacak sınıfı **AsyncController**. Zaman uyumsuz istek işlemeye ASP.NET AsyncController türetilen denetleyicileri etkinleştirin ve hala hizmeti zaman uyumlu eylem yöntemleri yapabilirler.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Ekleme **zaman uyumsuz** anahtar **dizin** yöntemi ve dönüş türü hale **görev&lt;actionresult öğesini&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **Zaman uyumsuz** anahtar sözcüğü, .NET Framework 4.5 sağlayan yeni anahtar sözcüklerle biridir; bu yöntemini zaman uyumsuz kod içeren derleyiciye bildirir. A **görev** nesne belirli bir noktada gelecekte tamamlanabilir bir zaman uyumsuz işlemi temsil eder.
6. Değiştirin **istemci. GetAsync()** aşağıda gösterildiği gibi kullanarak tam zaman uyumsuz sürümü ile çağrı await anahtar sözcüğü.

    (Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Önceki sürümde, kullanmakta olduğunuz **sonucu** özelliğinden **görev** sonucu (eşitleme sürüm) döndürülünceye kadar iş parçacığını engellemek için nesne.
    > 
    > Ekleme **await** anahtar sözcüğü, derleyicinin zaman uyumsuz yöntemin çağrısından döndürülen göreve bekleyin söyler. Bu, kodun geri kalanını yalnızca beklenen yöntemi tamamlandıktan sonra geri arama olarak yürütülecek anlamına gelir. Başka bir şey fark, try-catch bloğu Bunun çalışmasını sağlamak için değiştirmeniz gerekmez olduğu: ön plan veya arka planda gerçekleşen özel durumları hala framework tarafından sağlanan bir işleyici kullanarak herhangi bir ek çalışma yapmadan yakalanır.
7. Aşağıda gösterildiği gibi yeni kod iler satırları değiştirerek zaman uyumsuz bir uygulama ile devam etmek için kodu değiştirin

    (Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Uygulamayı çalıştırın. Hiçbir önemli değişiklikler fark edeceksiniz, ancak bir iş parçacığından yapmak daha iyi bir sunucu kaynak kullanımı ve performansı geliştirme iş parçacığı havuzu kodunuzu engellemez.

    > [!NOTE]
    > Laboratuvardaki yeni zaman uyumsuz programlama özellikleri hakkında daha fazla bilgi &quot; **C# ve Visual Basic ile .NET 4.5 içinde zaman uyumsuz programlama** &quot; Visual Studio eğitim Seti dahildir.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Görev 2 - işleme zaman aşımı ile iptal belirteçleri

Görev örnekleri döndüren zaman uyumsuz eylem yöntemleri de zaman aşımlarını destekler. Bu görevde, bir iptal belirteci kullanarak bir zaman aşımı senaryonun işlenmesi için dizin yöntemi kodu güncelleştirir.

1. Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.
2. Aşağıdaki deyimi kullanarak **HomeController.cs** dosya.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Güncelleştirme almak için dizin eylemi bir **CancellationToken** bağımsız değişken.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Güncelleştirme **GetAsync** Çağrı iptal belirteci geçirmek.

    (Kod parçacığını - *CancellationToken ile ASP.NET MVC 4 - Ex04 - Laboratuvar SendAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Süslemek *dizin* yöntemi ile bir **AsyncTimeout** özniteliği ayarlanmış için aralığını 500 milisaniyenin ve **HandleError** işlemek için yapılandırılmış öznitelik  **TaskCanceledException** yönlendirmek için bir **zaman aşımına uğradı** görünümü.

    (Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar öznitelikleri*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Açık **PhotoController** sınıfı ve güncelleştirme **galeri** yürütme 1000 milisaniye (1 saniye) uzun süre çalışan bir görevin benzetimini yapmak için gecikme yöntemi.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Açık **Web.config** dosya ve özel hataları öğesi ekleyerek etkinleştirin.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Yeni bir görünüm oluşturma **görünümler/paylaşılan** adlı **zaman aşımına uğradı** ve varsayılan düzenini kullanın. Çözüm Gezgini'nde sağ **görünümler/paylaşılan** klasörü ve select **Ekle | Görünüm**.

    ![Tüm mobil cihazlar için farklı görünümleri kullanma](whats-new-in-aspnet-mvc-4/_static/image36.png "tüm mobil cihazlar için farklı görünümleri kullanma")

    *Tüm mobil cihazlar için farklı görünümleri kullanma*
9. Güncelleştirme **zaman aşımına uğradı** aşağıda gösterildiği gibi içeriği görüntüle.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Uygulamayı çalıştırmak ve kök URL'ye gidin. Eklediğiniz gibi bir **net_thread_sleep** 1000 milisaniye olarak tarafından oluşturulan bir zaman aşımı hatası alırsınız **AsyncTimeout** özniteliği ve tarafından catch **HandleError** özniteliği.

    ![Zaman aşımı özel durumu işlenmiş](whats-new-in-aspnet-mvc-4/_static/image37.png "işlenen zaman aşımı özel durumu")

    *Zaman aşımı özel durumu işlenmiş*

> [!NOTE]
> Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek D: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı-lab içinde size bazı yeni özellikleri ASP.NET MVC 4'te yerleşik gözlemlenen. Aşağıdaki kavramları açıklanmıştır:

- ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama projesi şablon geliştirmeleri avantajlarından yararlanın
- Mobil cihazlarda görünen geliştirmek için CSS medya sorgular ve HTML5 Görünüm penceresi özniteliği kullanın
- JQuery Mobile için aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın.
- Mobile özel görünümlerini oluşturma
- Uygulamasında mobil ve Masaüstü görünüm arasında geçiş yapmak için Görünüm değiştirici bileşenini kullan
- Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturma

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Ek A: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-aspnet-mvc-4/_static/image38.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](whats-new-in-aspnet-mvc-4/_static/image39.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](whats-new-in-aspnet-mvc-4/_static/image40.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](whats-new-in-aspnet-mvc-4/_static/image41.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için***

1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.
2. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
3. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](whats-new-in-aspnet-mvc-4/_static/image42.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](whats-new-in-aspnet-mvc-4/_static/image43.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Ek B: yükleme Visual Studio 2012 Web için Express

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Web kutucuğu için VS Express*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Ek C: yükleme WebMatrix 2 ve iPhone simülatörü

Sitenizi iPhone sanal cihaz çalıştırmak için WebMatrix genişletmesi kullanabilirsiniz &quot;mobil iPhone simülatörü elektrik&quot;. Ayrıca, Visual Studio 2012'den simülatörünü çalıştırmak için aynı uzantı yapılandırabilirsiniz.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Görev 1 - yükleme WebMatrix 2

1. Git [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>WebMatrix 2</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![WebMatrix 2 Yükle](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2'yi yükleme")

    *WebMatrix 2 yükle*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul](whats-new-in-aspnet-mvc-4/_static/image50.png "lisans koşullarını kabul etme")

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image51.png "yükleme ilerleme durumu")

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image52.png "yükleme tamamlandı")

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Görev 2 - iPhone simülatörü uzantısını yükleme

1. Çalıştırma **WebMatrix** ve varolan bir Web sitesini açın veya yeni bir tane oluşturun.
2. Tıklayın **çalıştırma** düğmesini **giriş** seçin ve Şerit **yeni Ekle**.

    ![Yeni bir WebMatrix genişletmesi ekleme](whats-new-in-aspnet-mvc-4/_static/image53.png "yeni bir WebMatrix genişletmesi ekleme")

    *Yeni bir WebMatrix genişletmesi ekleme*
3. Seçin **iPhone simülatörü** tıklatıp **yükleme**.

    ![WebMatrix uzantıları gözatma](whats-new-in-aspnet-mvc-4/_static/image54.png "gözatma WebMatrix uzantıları")

    *WebMatrix uzantıları gözatma*
4. Paket Ayrıntıları tıklatın **yükleme** uzantısını yükleme işlemine devam etmek için.

    ![iPhone simülatörü uzantısı](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone simülatörü uzantısı")

    *iPhone simülatörü uzantısı*
5. Okuyun ve uzantı EULA'yı kabul edin.

    ![WebMatrix genişletmesi EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix genişletmesi EULA")

    *WebMatrix genişletmesi EULA*
6. Artık, Web sitenizi iPhone simülatörü seçeneği kullanarak Webmatrix'ten çalıştırabilirsiniz.

    ![İPhone kullanarak çalıştırma](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone kullanarak çalıştırma")

    *İPhone kullanarak çalıştırma*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Görev 3 - iPhone simülatörü çalıştırmak için Visual Studio 2012 yapılandırma

1. Açık **Visual Studio 2012** ve herhangi bir Web sitesini açın veya yeni bir proje oluşturun.
2. Çalıştırma düğmesi aşağı oka tıklayın ve **birlikte Gözat**.

    ![Birlikte Gözat](whats-new-in-aspnet-mvc-4/_static/image58.png "birlikte Gözat")

    *Birlikte Gözat*
3. İçinde &quot;şununla Gözat&quot; iletişim kutusunda, tıklayın **Ekle**.
4. İçinde &quot;Program Ekle&quot; iletişim kutusunda aşağıdaki değerleri kullanın:

   - <strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (yolunu uygun şekilde güncelleştirin)</em>
   - **Bağımsız değişkenler**: &quot;1&quot;
   - **Kolay ad**: iPhone simülatörü

     ![Ekleme programı](whats-new-in-aspnet-mvc-4/_static/image59.png "ekleme programı")

     *İle göz atmak için program ekleyin*
5. Tıklayın **Tamam** ve iletişim kutularını kapatın.
6. Artık Web uygulamalarınızı iPhone simülatör'de Visual Studio 2012'den çalıştırabilir.

    ![İPhone simülatörü ile Gözat](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone simülatörü ile Gözat")

    *İPhone simülatörü ile Gözat*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek D: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure Portal'da oturum açın](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image62.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image63.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](whats-new-in-aspnet-mvc-4/_static/image64.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](whats-new-in-aspnet-mvc-4/_static/image65.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-mvc-4/_static/image66.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.

    ![Yayımlama profili web sitesi indiriliyor](whats-new-in-aspnet-mvc-4/_static/image67.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-mvc-4/_static/image68.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Değişiklikleri onaylayın*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-aspnet-mvc-4/_static/image73.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-mvc-4/_static/image74.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-aspnet-mvc-4/_static/image75.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-aspnet-mvc-4/_static/image76.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-mvc-4/_static/image77.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-aspnet-mvc-4/_static/image78.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-aspnet-mvc-4/_static/image80.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

    ![Uygulama Windows Azure'da yayımlanan](whats-new-in-aspnet-mvc-4/_static/image81.png "uygulama yayımlanan Windows Azure'a")

    *Windows Azure'da yayımlanan uygulama*
