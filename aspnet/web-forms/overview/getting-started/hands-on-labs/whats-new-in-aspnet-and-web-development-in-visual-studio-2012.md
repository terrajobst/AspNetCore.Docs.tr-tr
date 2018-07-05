---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler | Microsoft Docs
author: rick-anderson
description: Visual Studio'nun yeni sürümü, performans ve deneyimini Web teknolojileri ile çalışırken geliştirmeye odaklı geliştirmeleri sunar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d037ccb62693a07ab8e2640b2d930ec7093b35da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375891"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

> Visual Studio'nun yeni sürümü, performans ve deneyimini Web teknolojileri ile çalışırken geliştirmeye odaklı geliştirmeleri tanıtır. En çok talep kod Yardımcıları, IntelliSense ve Otomatik girintili yazma gibi birçok dahil etmek için Visual Studio Düzenleyicisi, CSS, JavaScript ve HTML tamamen modernize. Sayfa kolayca azaltmak için yerleşik özellikleri yükleme süresi gibi performans, artık paketleme ve küçültme tümleşiktir.
> 
> Visual Studio, en son Web teknolojilerini ile çalışmanıza olanak sağlar. Yeni HTML5 öğeleri ve özellikleri de çekerken istemci platformları ne olursa olsun, sitenizi çalıştığından emin olmak için tarayıcılar arası CSS3 parçacıkları kullanabilirsiniz.
> 
> Yazma ve profil oluşturma JavaScript kodunu bu Visual Studio sürümü ile daha kolay olmalıdır. IntelliSense listeleri, tümleşik XML belgeleri ve gezinti özellikleri artık JavaScript kodu için kullanılabilir. JavaScript Kataloğu artık parmaklarınızın ucunda sahipsiniz. Ayrıca, betiklerinizi ile ECMAScript5 uyumluluk denetimi ve erken bir aşamada söz dizimi hataları algılayın.
> 
> Son ancak, bu Visual Studio sürümü yerleşik paketleme ve küçültme uygular. Komut dosyalarını ve stil sayfalarını paketlenmiş ve böylece sitenin daha hızlı gerçekleştirir sıkıştırılmış.
> 
> Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı Laboratuvar, öğreneceksiniz nasıl yapılır:

- CSS Düzenleyicisi'nde yeni özellikler ve geliştirmeler kullanın
- HTML Düzenleyicisi'nde yeni özellikler ve geliştirmeler kullanın
- Yeni özellikler ve geliştirmeler içindeki JavaScript düzenleyicisi kullanın.
- Yapılandırma ve paketleme ve küçültme kullanma

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (için ayar betikleri - Windows 8 ve Windows Server 2008 R2 üzerinde zaten yüklü)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - veya HTML5 ile uyumlu bir tarayıcı

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?](#Exercise1)
2. [Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?](#Exercise2)
3. [Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?](#Exercise3)
4. [Alıştırma 4: Paketleme ve küçültme](#Exercise4)

Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?

Web geliştiricileri, CSS düzenlemeye ilgili zorluklarla birçoğu ile ilgili bilgi sahibi olmalıdır. CSS stil en büyük sorunlardan biri, tarayıcılar arası uyumluluktur. Genellikle, başka bir tarayıcı veya cihaz açarsanız sitenize stilleri uyguladıktan sonra farklı görünüyor dikkat edin, gerçekleşir. Bu nedenle, son olarak, bir tarayıcıda yaptığınızda, bu diğer ayrılır, hayata geçirmek için bu görsel sorunlarını düzeltmeye büyük miktarda zaman harcayabilir.

Visual Studio artık, geliştiricilerin erişim, iş ve CSS stil sayfaları etkili bir şekilde düzenlemenize yardımcı olan özellikler içerir. Bu alıştırmada yeni özellikler için etkili bir kuruluşun her büyüklükteki yanı sıra, tarayıcılar arası uyumluluk için CSS3 kod parçacıkları karşılar.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Görev 1 - yeni düzenleyici özellikleri

Bu görevde, yeni özellikler CSS Düzenleyicisi'nin keşfeder. Bu yeni düzenleyici yeni akıllı girinti, geliştirilmiş kod açıklamaları ve Gelişmiş IntelliSense listesini avantajlarından yararlanarak üretkenliğinizi artırmanıza yardımcı olur.

1. Başlangıç **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** , bu Laboratuvarın klasör.
2. Çözüm Gezgini'nde açın **Site.css** altında bulunan dosya **stilleri** klasör. Emin **metin düzenleyici** araçları araç çubuğunda görünür. Bunu yapmak için seçin **görünümü** | **araç çubukları** menü seçeneği ve onay **metin düzenleyici** seçenekleri. Bu yeni sürümün bu yana fark edeceksiniz **yorum** düğmesine (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) ve **açıklamayı Kaldır** düğmesine (![açıklamasını Kaldır düğmesine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) için CSS Düzenleyicisi de etkinleştirilir.

    ![Düzenleyici ve CSS araçları etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Düzenleyicisi ve CSS Araçları'nı etkinleştirme")

    *Düzenleyici ve CSS Araçları'nı etkinleştirme*
3. Kod kaydırın ve herhangi bir CSS sınıfı tanımını seçin. Tıklayın **yorum** (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) düğmesini seçili satırları açıklama satırı yapın. ' A tıklayarak **açıklamayı Kaldır** (![açıklamasını Kaldır düğmesine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) değişiklikleri geri düğmesi.
4. Tıklayın **Daralt** (![Daralt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) ve **genişletme** (![genişletin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) düğmeleri bulunan metnin sol kenar boşluğu. Şimdi daha net bir görünüm için kullanmayın stilleri gizleyebilirsiniz dikkat edin.

    ![CSS sınıfları daraltma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "daraltma CSS sınıfları")

    *CSS sınıfları daraltma*
5. Akıllı girintileme özelliği etkin olduğundan emin olun. Seçin **Araçları** | **seçenekleri** seçeneğine tıklayın ve ardından **metin düzenleyici** | **CSS**  |  **Biçimlendirme** ekranın sol bölmede sayfası. Denetleme **hiyerarşik girintileme** seçeneği.

    ![Hiyerarşik girintileme etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "hiyerarşik girintileme etkinleştirme")

    *Hiyerarşik girintileme etkinleştirme*
6. Ana sınıf tanımı (.main) bulun ve div öğelere stil eklemek. Kodu otomatik olarak üst bir bakışta sınıfları bulmak için kullanıcılara yardımcı olma hizalar fark edeceksiniz.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![CSS hiyerarşik hizalama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS hiyerarşik hizalama")

    *CSS hiyerarşik hizalama*
7. İçinde **.main div** sınıfı, imleç sonunda bulun **kenarlık: 0px;** basın **Enter** IntelliSense listesini görüntülemek için. Yazmaya başlayın **üst** ve siz yazarken liste nasıl filtrelenmiştir dikkat edin. Liste içeren öğeleri görüntüler **üst** word'ün herhangi bir bölümü, (Visual Studio'nun önceki sürümleri liste öğeleri tarafından filtrelenir, *başlamak* terim ile).

    ![CSS IntelliSense geliştirmeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS IntelliSense geliştirmeleri")

    *CSS IntelliSense geliştirmeleri*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Görev 2 - Renk Seçici

Bu görevde, Visual Studio IntelliSense yeni CSS Renk Seçici tümleşik keşfeder.

1. İçinde **Site.css,** üst sınıf tanımı (.header) bulun ve yanındaki imleci **arka plan rengi** arasında öznitelik &quot;:&quot; ve &quot; # &quot; kodun o satırına karakterlere **.**

    ![İmleci konumlandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "imleci konumlandırma")

    *İmleci konumlandırma*
2. Silme **iki nokta üst üste** (:) ve Renk Seçici yeniden görüntülemek içinn yazabilirsiniz. Göreceğiniz ilk renkleri sitenizin en sık kullanılan renkleri olduğuna dikkat edin. Beyaz renk tıklarsanız, HTML renk kodu (#fff) stil sayfası geçerli renk kodu yerini alır.

    ![Renk Seçici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Renk Seçici")

    *Renk Seçici*
3. Tuşuna **genişletme** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) düğmesine Renk Seçici renk gradyanı görüntülemek ve farklı bir renk seçmek için gradyan imleç sürükleyin. Bundan sonra tıklayın **damlalığı** düğmesini tıklatın ve ekranından herhangi bir renk seçin. İmleç taşırken arka plan renk değeri dinamik olarak değiştiğine dikkat edin.

    ![Renk Seçici gradyan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Renk Seçici gradyan")

    *Renk Seçici gradyan*
4. İçinde **Opaklık** kaydırıcı, seçici opaklık azaltmak için çubuğunun merkezine taşımanızı. Arka plan renk değeri RGBA için artık, ölçeği değiştiğine dikkat edin.

    ![Renk Seçici Opaklık](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "opaklık Renk Seçici")

    *Renk Seçici opaklığı*

    > [!NOTE]
    > CSS3 RGBA (kırmızı, yeşil, mavi, alfa) renk tanımı tek bir öğe renk opaklık değerini tanımlamanızı sağlar. Farklı **Opaklık -** benzer bir CSS öznitelik **-** RGBA renklerdir ayrıca en son tarayıcıları ile uyumlu.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Görev 3 - CSS uyumlu kod parçacıkları

Bu görevde, bazı özellikler siteniz uygulamak için tarayıcılar arası uyumlu CSS3 parçacıkları kullanmayı öğreneceksiniz.

1. İçinde **Site.css** bulun, dosya **üstbilgi** CSS sınıf tanımını (.header) ve aşağıdaki imleci **/ \*kenarlık RADIUS\* /** yeni bir kod parçacığı eklemek için yer tutucu. Tuşuna **Enter** türü ve IntelliSense listesini görüntülemek için **RADIUS** listeyi filtrelemek için. Seçin **border-radius** seçenek listesinden bir çift tıklamayla ve tuşuna **sekmesini** kod parçacığını eklemek için anahtar. Ardından, bir RADIUS boyutunu piksel ve ENTER tuşuna yazın **Enter**. Örneğin, türü **15px**.

    Kod parçacığı tarafından eklenen CSS3 öznitelikleri Mozilla ve WebKit tabanlı tarayıcılar dahil olmak üzere çoğu HTML5 uyumluluk tarayıcılarda yuvarlatılmış Kenarlıklar işlenir.

    ![Border-radius kod parçacığını kullanarak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "border-radius kod parçacığını kullanarak")

    *Border-radius kod parçacığını kullanarak*
2. Aynı uygulama **kenarlık** kod parçacıkları, sayfa stili (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Tuşuna **F5** çözümü çalıştırın. Her sayfanın Kenarlıklar artık yuvarlatılmış olduğuna dikkat edin.

    ![Yuvarlak köşeler](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "yuvarlatılmış köşeler")

    *Yuvarlatılmış köşeler*
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
5. Açık **Custom.css** altında bulunan dosya **stilleri** klasörü ve imleci yerleştirin **div.images ul li img** sınıf tanımını.
6. Enter tuşuna basın IntelliSense listesini görüntülemek için tür **kutusu gölge** tuşuna basın **sekmesini** iki kez sınıf tanımı içinde varsayılan gölge kod parçacığını eklemek için anahtar. Gölge değerlerini ayarlamak **10px 10px 5px #888**. Ardından yazın **border-radius** ve kod parçacığını ekleyin. Tür **15px** yarıçapı ve ENTER tuşuna ayarlanacak **ENTER**.

    ![Yuvarlak köşeler gölgeli](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "yuvarlatılmış köşelere sahip Gölge")

    *Gölgeli yuvarlatılmış köşeler*

    > [!NOTE]
    > Şu anda Webkit (Chrome, Safari Konkeror) tarayıcılar ve karşılık gelen önek (moz webkit, o) Mozilla desteklemek için ile gölge özniteliği eklenir.
7. Yeni bir sınıf oluşturun **div.images ul li img:hover** aşağıda **div.images ul li img** sınıf tanımını ve imleci ayraçlar içinde **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tür **dönüştürme** basın **sekmesini** iki kez dönüştürme kod parçacığını eklemek için anahtar. Ardından, girin **rotate(-15deg)** görüntüleri seçildiğinde döndürme açısı değeri değiştirmek için.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Tuşuna **F5** çözümü çalıştırın ve CSS3 sayfasına göz atın. Görüntüleri yuvarlak köşeler olacak ve shadows kutusunda olduğuna dikkat edin. Fare görüntülerin gelin ve döndürme izleyeceksiniz.

    ![Görüntü döndürme kod parçacığı dönüştürme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "görüntü döndürme dönüşümü kod parçacığı")

    *Görüntü döndürme kod parçacığı dönüştürme*

    > [!NOTE]
    > Internet Explorer 10 kullanıyorsanız ve shadows göremez ıe10 standartları belge modu ayarlandığından emin olun. Tuşuna **F12** Internet Explorer Geliştirici Araçları'nı açın ve tıklatın **belge modu** ıe10 standartları değiştirmek için.

    ![hakkında-ABD](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?

Visual Studio, Gelişmiş bir HTML düzenleyicisi sahiptir. Bu sürümde eklenen iyileştirmeler HTML belgeleri, HTML5 kod parçacıkları, HTML başlangıç ve bitiş etiketi ile eşleşen ve HTML doğrulama akıllı girintileme bazılarıdır. Bu alıştırmada, Web sitesi biçimlendirme içinde çalışırken bu değişiklikler, fluency nasıl geliştirmek görürsünüz.

CSS Düzenleyicisi gibi HTML düzenleyicisi de İyileştirildi. Bu geliştirmeler çoğu Web geliştiricinin kolaylaştıracak küçük olanlardır. Düzenleme ve doğrulama HTML belgesi DOCTYPE hedefleyen bu geliştirmelerin bazılarını olduğu başlangıç ve bitiş etiketleri eşleşen HTML5 için akıllı girinti daha fazla kod parçacıkları gibi şeyler.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Görev 1 - geliştirilmiş DOCTYPE doğrulama

Tanımı ana sayfasında olsa bile HTML düzenleyici artık, sayfanızın DOCTYPE denetlemek için özelliğine sahiptir. DOCTYPE sayfanızın bağlı olarak HTML düzenleyicisi doğru kuralları kümesiyle doğrulanır ve DOCTYPE öğeleri dikkate IntelliSense listesini filtreler.

Bu görevde, HTML düzenleyici davranışı buna göre nasıl değiştiğini görmek için DOCTYPE sayfasının değişecektir.

1. Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.
2. Açık **Site.Master** sayfası.
3. Hedef şema doğrulama araç dikkat edin. HTML düzenleyicisi davranır (doğrulama, IntelliSense, vb.) düzgün seçili Doctype uyacak şekilde değiştirecek.

    ![Doctype HTML kaynak düzenlemesi araç kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "kullanım Doctype HTML kaynak düzenlemesi araç çubuğu")

    *Doctype HTML kaynak düzenlemesi araç çubuğunda kullanın*
4. HTML 4.01 hedef şema değiştirin.

    ![Doctype HTML kaynak düzenlemesi araç çubuğunda değiştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "değiştirme Doctype HTML kaynak düzenlemesi araç çubuğu")

    *Doctype HTML kaynak düzenlemesi araç çubuğunda değiştirme*
5. İmlecin altındaki yerleştirmek **gövdesi** öğesi ve bir HTML5 öğenin adını yazmaya (örneğin, **video**). Öğe, IntelliSense listede olmadığına dikkat edin.

    ![HTML5 öğeleri listede](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 öğeleri listede")

    *HTML5 öğeleri listede*
6. Doğrulama için araç çubuğunda DOCTYPE çekme hedef şema değişiklikleri geri al: aşağı açılan listeden XHTML5.

    ![Doctype HTML kaynak düzenlemesi araç kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "kullanım Doctype HTML kaynak düzenlemesi araç çubuğu")

    *Doctype HTML kaynak düzenlemesi araç çubuğunda Sıfırla*
7. İmlecin altındaki yerleştirmek **gövdesi** öğesi ve bir HTML5 öğe yeniden yazmaya (örneğin, gibi **video**). HTML5 öğeleri IntelliSense listesinde kullanılabilir olduğuna dikkat edin.

    ![HTML5 öğeleri listelenmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "listelenmesini HTML5 öğeleri")

    *Listelenmesini HTML5 öğeleri*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Görev 2 - Başlangıç/bitiş etiketleri otomatik güncelleştirme

Visual Studio artık açma veya kapatma etiketleri birbirine eşleştirilecek düzenlemekte olduğunuz öğenin HTML güncelleştirir. Bu yeni özellik, HTML etiketleri düzenlerken üretkenliğinizi artırır.

1. Üzerinde **Default.aspx** sayfasında, eklemek bir **H3** öğesi ile bir başlık (örneğin, Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Değişiklik **H3** etiketi ve tür **H2** veya **H1.**

    Bitiş etiketi otomatik olarak güncelleştirildiğine dikkat edin. Bitiş etiketi başlangıç etiketiyle buna göre çok güncelleştirdiğini görmek için de değiştirebilirsiniz.

    ![Kapanış etiketinin otomatik güncelleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "bitiş etiketi otomatik güncelleştirilmesi")

    *Kapanış etiketinin otomatik güncelleştirme*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Görev 3 - Yeni HTML5 kod parçacıkları

Visual Studio artık birkaç HTML5 kod parçacıklarını içerir. Bu görevde, bu kod parçacıklarının bazılarını kullanacaksınız.

1. Adlı yeni bir klasör eklemek **ses** kök web site klasörü. Windows Gezgini'ni açın ve herhangi bir ses dosyasına kopyalayın **ses** klasörü **WhatsNewASPNET.sln** çözüm.
2. İçinde **Default.aspx** sayfasında, imleci Web11 Rocks altında bulun!! Üst bilgisi. Tür **ses** ve SEKME tuşuna basın.

    HTML düzenleyicisi kod parçacıkları HTML5 içerik içerir. HTML5 parçacıkları etkinleştirmek için uygun belge türü tanımı kullanmayı unutmayın.

    ![HTML5 kod parçacıkları ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 kod parçacıkları ekleme")

    *HTML5 kod parçacıkları ekleme*
3. Ses kaynağından mevcut bir ses dosyasına işaret edecek şekilde güncelleştirin.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Ses dosyası çözüme eklemeniz gerekir.
4. Tuşuna **F5** siteyi çalıştırmak ve ses çal.

    ![Ses denetimini çalıştıran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "ses denetimini çalıştırma")

    *Ses denetimini çalıştırma*

    > [!NOTE]
    > Visual Studio'da bulunan video, Şekil vb. gibi daha fazla sayıda parçacık de deneyebilirsiniz.
5. Bazı sayfa kısmında bir denetim eklemek şimdi deneyin. Örneğin, eklemeyi denerseniz bir **GridView** denetimi, ancak yazmayı yerine  **&lt;oyutlandır,** yazmaya başlayın  **&lt;GV**. IntelliSense listesini gösteren uyarı **asp: GridView** denetimi.

    HTML Düzenleyicisi'nde IntelliSense şimdi başlık büyük/küçük harf arama yanı sıra kısmi eşleştirme (terimini içeren tüm öğeleri alınıyor) sağlar.

    ![GridView IntelliSense listeleriyle ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense listeleriyle GridView ekleme")

    *GridView IntelliSense listeleriyle ekleme*

    Yazarsanız  **&lt;kılavuz** terimiyle eşleşen tüm öğeleri alırsınız, ancak Visual Studio önerisinde bulunacak **gridview** denetimi:

    ![GridView IntelliSense listeler ve kısmi eşleşme ile ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "GridView IntelliSense listeler ve kısmi eşleşme ile ekleme")

    *GridView IntelliSense listeler ve kısmi eşleşme ile ekleme*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Görev 4 - HTML düzenleyicisi akıllı etiketleri

Başka bir geliştirme HTML Düzenleyicisi'nde akıllı etiketler özelliğidir. Akıllı etiketler bir denetim başına temelinde ortak ve yinelenen geliştirme görevlerinin gerçekleştirilebilmek kolaylaştırır. Bu özellik zaten HTML Tasarımcısı'nda ancak olmayan HTML Düzenleyicisi'nde.

1. Açık **Site.Master** bulun **asp: menü** öğesi. Başlangıç etiketi ve dikkat öğe - alt kısmında görüntülenen küçük simge Akıllı görevler menüsünü açmak için tıklayın imleci yerleştirin. Menü denetimi ile ilgili bazı görevleri hızlı erişimi olduğunu fark edeceksiniz.

    ![Menü denetimi için görevleri akıllı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "akıllı görevleri için menü denetimi")

    *Menü denetimi için akıllı görevleri*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Görev 5 - akıllı girinti

Bir HTML en iyi kod okunabilir tutmak için iç içe öğelerin girintileme. Visual Studio 2012'de kod yazarken düzenleyicide öğeleri otomatik olarak girintiler fark edeceksiniz.

> [!NOTE]
> Akıllı girintileme önceki Visual Studio sürümünde kullanılabilir olan XML düzenleyicisinde ancak HTML düzenleyicisi içinde değil.


1. Indenting yapılandırma üzerinde HTML düzenleyicisi akıllı girintileme için ayarlanmış olduğundan emin olun. Bunu yapmak için seçin **araçları | Seçenekleri** menü seçeneğini ve ardından **metin düzenleyicisi | HTML | Sekmeleri** ekranın sol bölmede sayfası. Akıllı girintileme seçeneğini belirleyin.

    ![HTML Düzenleyici ayarları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML düzenleyicisi ayarları")

    *HTML düzenleyicisi ayarları*
2. Üzerinde **Default.aspx** sayfasında, ses öğesinin altındaki tüm içeriğini kaldırın.
3. Açılış sonunda imleci **ses** öğesi ve isabet **ENTER**.

    Yeni imleç konumu bir ek girinti düzeyi olduğuna dikkat edin.

    ![Akıllı girintileme HTML Düzenleyicisi'nde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "akıllı girintileme HTML Düzenleyicisi'nde")

    *HTML Düzenleyicisi'nde akıllı girinti*
4. Ses etiketini kaldırdıysanız veya kapatın içerikle geri **Default.aspx** değişiklikleri kaydetmeden.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Görev 6 - kullanıcı denetimi ayıklayın

Bir işlev kodu bir kısmını ayıklama gibi Visual Studio'da bulunan yeniden düzenleme araçları, geliştirme ve mevcut kodu yeniden düzenlemeye kolaylaştıran harika özellikleridir. ASP.NET sayfaları için karşılık gelen HTML kod ayıklama için bir kullanıcı denetimi olacaktır. El ile yapılması yeni bir kullanıcı denetimi oluşturma, kullanıcı denetimine kod bölümünde taşıma, kullanıcı denetim için etiket öneki kaydetme ve, son olarak, kullanıcı denetimi sayfalarında örnekleme gibi çeşitli adımları içerir. Şimdi, yeni *kullanıcı denetimine çıkar* aracı otomatik olarak gerçekleştirir bu adımların tümünü sizin için.

Bu görevde, yeni kullanıcı denetimi bağlamsal işlemi ayıklayın, seçilen kod yeni bir kullanıcı denetimi oluşturmak için kullanır.

1. Üzerinde **Default.aspx** sayfasında **H2** ve **ses** öğeleri.
2. Sağ tıklatın ve seçin **kullanıcı denetimine çıkar**.

    ![Kullanıcı denetimi menü seçeneğine ayıklamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "kullanıcı denetimi menü seçeneğine ayıklayın")

    *Kullanıcı denetimi menü seçeneğine ayıklayın*
3. Yeni kullanıcı denetimi için bir ad yazın. Örneğin, **Jukebox.ascx**ve ardından **Tamam**.

    ![Ayıklanan kullanıcı denetimine kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "ayıklanan kullanıcı denetimi kaydediliyor")

    *Ayıklanan kullanıcı denetimi kaydediliyor*
4. Seçilen koda bir kullanıcı denetimine ayıklanan ve yeni kullanıcı denetimi örneğini seçili kodunun özgün konumuna değiştirildi dikkat edin.

    ![Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "sayfası yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirildi")

    *Sayfayı yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir.*
5. Tuşuna **F5** çalıştırırsanız ve denetim çalıştığını doğrulayın.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?

Yazma veya JavaScript kod düzenleme bir kolayca görev özellikle boyutları büyürken, uygulamanız başlar ve kendiniz uzun dosyalarını uğraşmanızı ve işlevleri yüzlerce bulun, değil. Betik geliştiriciler genellikle kod Okunaklılık korumak ve dosyalar arasında gezinmek için ek iş yapması gerekir. JQuery gibi JavaScript kitaplıklarını eklenmesi ile betik Gezinti kod uzunluğu nedeniyle zor kendisini haline gelmiştir.

Visual Studio JavaScript Düzenleyicisi kod modu, erişilebilir ve düzenli hale getirmek için promise ile yeniledi. C# veya VB düzenleyicilerde zaten var olan birçok Visual Studio özellikleri artık JavaScript Düzenleyicisi'nde uygulanır: Tanıma Git, Otomatik girintili yazma, belgelere ve siz yazarken doğrulama. Yenilenen IntelliSense listesiyle parmaklarınızın ucunda JavaScript işlevi Kataloğu gerekir.

Bu alıştırmada, bazı yeni özellikler ve geliştirmeler JavaScript düzenleyicisinin öğreneceksiniz. Siz örnek dosyalara göz atın ve her biri, JavaScript programlama Visual Studio 2012 içinde daha verimli hale getirecek yeni özellikleri keşfedin.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Görev 1 - JavaScript Düzenleyicisi yeni özellikleri

Bu görev için bazı kod düzenleme ve daha iyi bir kullanıcı deneyimi getirme yeni JavaScript Düzenleyicisi özellikleri kullanıma sunacak.

1. Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.
2. Tuşuna **F5** uygulamayı çalıştırmak için Gezinti çubuğundaki JavaScript bağlantıya tıklayın. Sayfanın birkaç kez ve onay nasıl Yenile sayacını artırır.

    ![Sayfa Sayacı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "sayfa sayacı")

    *Sayfa Sayacı*
3. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
4. Açık **JavaScript.aspx** bulun ve sayfa **&lt;betik&gt;** blok (aşağıda gösterilmiştir).

    Aşağıdaki kod, saklamak için yerel HTML5 depolama kullanır. bir *pageLoadCount* sayfanın geçerli kullanıcı tarafından ziyaret kaç kez depolayan değişken. Yerel depolama ile HTML5 standart sunulan bir istemci-tarafı anahtar-değer veritabanı hizmetidir. Veriler, yerel makinede, kullanıcının tarayıcı içinde kaydedilir.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > DOCTYPE XHTML5 için sonraki adımlara devam etmeden önce ayarlandığından emin olun.
5. Kodu düzenleme ve JavaScript için IntelliSense gibi yerel depolama ve iç yöntemlerini HTML5 özelliklerini içerdiğine dikkat edin.

    ![JavaScript, HTML5, JavaScript özellikleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript özellikleri")

    *JavaScript, HTML5, JavaScript özellikleri*
6. Bir açma ayracı tıklayın (**{**) komut dosyası kodu ve köşeli ayraçlar vurgulanır dikkat edin.

    ![Köşeli ayraçlar vurgulanır](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "ayraçlar vurgulanır")

    *Köşeli ayraçlar vurgulanır*
7. İşlev açıklama durumundan çıkarın **testAutoAlign()** (üç satırı seçin ve kullanabileceğiniz **CTRL** + **K**; **CTRL** + **U**) ve işlev kodunu imleci bulun. İkinci satır eklemek için enter tuşuna basın. Artık kod olduğuna dikkat edin **hizalı** ve **otomatik girintili**.

    ![JavaScript kodudur hizalı otomatik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript kodu otomatik hizalanır.")

    *JavaScript kodu otomatik hizalanır.*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Görev 2 - doğrulama JavaScript

Bu görevde, yeni bir JavaScript doğrulama ECMAScript5 standart keşfeder. Bu özellik site dağıtımdan önce kodlama engelleme sorunları oluştu uyumlu JavaScript kod yazmanıza yardımcı olur.

> [!NOTE]
> Visual Studio 2010, Visual Studio 2012 ECMAScript5 uyum sağlarken ECMAStript3 uyumluluk uygulanır.


1. Açık **ECMA5script5.js** altında bulunan **Scripts\custom** proje klasörü. Artık doğrulama ECMAScript5 standart için test eder.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Gözden geçirin &quot; **katı kullan** &quot; ECMAScript5 sağlayan dosyanın ilk satırı yönde **katı mod**. Bu modda, son sürümünden belirsizlikleri açıklar ve nesne özellikleri alıcılar ve ayarlayıcılar, JSON ve daha eksiksiz bir yansıma için kitaplık desteği gibi bazı yeni özellikler ekler dilini alt oluşur.
2. Açık **hata listesi** zaten açtıysanız (**görünümü** menü | **Hata listesi**). Bildirim **işlevi** bildirimi çizilir. Dil yapıları standart işlevleri ECMA5 içinde yuvalanamaz olmasıdır. Hata, uyarı ayrıntıları, aşağıdaki listede görürsünüz.

    ![JavaScript doğrulama hatası iletisini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript doğrulama hata iletisi")

    *JavaScript doğrulama hata iletisi*
3. Açıklama **&quot;katı kullan&quot;** yönünü ve hataları yok, ancak uyarılar kalır dikkat edin.
4. Dosyasının son satırında gibi herhangi bir dize yazmak **&quot;test&quot;** (dize olarak belirtmek için tırnak işaretleri dahil). Bir süre IntelliSense listesini görüntülemek ve seçmek için dize yanındaki yazma **trim** seçeneği.

    ECMAScript5 standart, dize değerleri ve değişkenleri, trim, büyük harf, arama ve değiştirme gibi tanımlı dize yöntemlerini de.

    ![JavaScript IntelliSense listesinde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "JavaScript IntelliSense listesinde")

    *JavaScript IntelliSense listesinde*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Görev 3 - JavaScript XML belgeleri

Bu görevde, JavaScript XML belgelerinde için Visual Studio özellikleri inceleyeceksiniz. JavaScript IntelliSense listesini her işlevin XML belgeleri gösterdiğini görürsünüz. Ayrıca, JavaScript gezinti özelliği keşfeder.

1. Açık **XMLDoc.js** bulunan dosya **betikleri/özel** proje klasörü. Bu dosya, XML belgeleri her JavaScript işlevleri içerir.

    ![JavaScript XML belgeleri için IntelliSense tümleşik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML belgeleri için IntelliSense tümleşik")

    *IntelliSense için tümleşik JavaScript XML belgeleri*
2. Aşağıda **ekleme** işlevi **XMLDoc.js** dosya, adlı yeni bir işlev oluşturma **test**.
3. İçinde **test** işlev, çağrı **Çarp** iki parametre alan bir işlev. Araç İpucu kutuyu gösteren fark **Çarp** işlevi belgelerine.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![JavaScript işlevleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML belgeleri için JavaScript işlevleri")

    *XML belgeleri için JavaScript işlevleri*
4. İşlev çağrısı ifadesi ve türü tamamlamak bir *nokta* döndürülen değerin üzerinde IntelliSense listesini açın. Visual Studio algılama Uyarısı **dönüş** değeri bir sayı olarak davranılması belgelerinde değeri.

    ![Dönüş türleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dönüş türleri için XML belgeleri")

    *Dönüş türleri için XML belgeleri*
5. Şimdi, işlev eklemek için bir çağrı ekleyin. JavaScript Düzenleyici artık işlev aşırı yüklemelerinin içerdiğine dikkat edin. Bir işlev adı yazdığınızda, herhangi bir belgede belirtilen kullanılabilir aşırı olacaktır.

    ![XML belgeleri için aşırı yüklemeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "aşırı yüklemeler için XML belgeleri")

    *Aşırı yüklemeler için XML belgeleri*
6. Açık **GotoDefinition.js** bulun ve dosya **$().html()** işlev çağrısı. İmleç bulmak **html**.
7. Tuşuna **F12** ve tanımına gidin. Artık erişebilir ve kullanmadan JavaScript kodunuza göz atma bildirimi **Bul** aracı.
8. İmleci, kod dosyasının sonundaki imza bloğunu önce jQuery yönerge üzerinde bulun. Tuşuna **F12**. JQuery kitaplığı dosyaya gider. JQuery kullanarak dosyaları arasında da gidebilirsiniz fark **F12**.

    ![JQuery tanımları için gezinme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery tanımları için gezinme")

    *JQuery tanımları için gezinme*

> [!NOTE]
> Dosyayı kaydetmeden önce GotoDefinition.js söz dizimi hatası olduğundan emin olun.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Alıştırma 4: Paketleme ve küçültme

Kaç kez sitelerinizi birden fazla JavaScript ya da CSS dosyası dahil midir? Bu, burada paketleme ve küçültme dosya boyutunu azaltıp daha hızlı gerçekleştirin sitesini yardımcı olabilir, yaygın bir senaryodur. Yeni paket özelliği ASP.NET 4.5, tek bir öğe JS ya da CSS dosyaları paketleri ve içeriği (yani gerekli boşluk kaldırma açıklamaları kaldırma, tanımlayıcılar azaltma) küçültme tarafından boyutunu azaltır.

Paketleme ve küçültme ASP.NET 4.5 içinde gerçekleştirilir çalışma zamanında işlemi kullanıcı aracısı (örneğin, IE, Mozilla, vb.) tanımlamak ve bu nedenle, kullanıcı tarayıcı (Mozilla belirli olan örneği için kaldırma öğe hedefleyerek sıkıştırma geliştirin ne zaman istek IE gelir).

Bu alıştırmada, etkinleştirmek ve paketleme ve küçültme farklı türde ASP.NET 4.5 içinde kullanmak öğreneceksiniz.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Görev 1 - yükleme paketleme ve küçültme NuGet paketi

1. Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.
2. NuGet Paket Yöneticisi konsolunu açın. Bunu yapmak için menüyü kullanın. **görünümü** | **diğer Windows** | **Paket Yöneticisi Konsolu**.

    ![Paket Yöneticisi file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Paket Yöneticisi konsolu açma")

    *Paket Yöneticisi konsolu açma*
3. İçinde **Paket Yöneticisi Konsolu** türü **Install-Package Microsoft.Web.Optimization** basın **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Görev 2 - varsayılan paketleri

Paketleme ve küçültme kullanmanın en basit yolu, varsayılan paketleri etkinleştirmektir. Bu yöntem, bir klasördeki JS ve CSS dosyaları ile birlikte gelen ve küçültülmüş sürümü başvuru izin vermek için kuralları kullanır.

Bu görevde, etkinleştirmek ve paketlenmiş ve küçültülmüş JS ve CSS dosyalarına başvuruda ve elde edilen çıkış görüntülemeyi öğreneceksiniz.

1. Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.
2. İçinde **Çözüm Gezgini**, genişletme **stilleri**, **Scripts\custom** ve **Scripts\bundle** klasörleri.

    Uygulama birden fazla CSS ve JS dosyası kullandığını unutmayın.

    ![Uygulama birden çok stil sayfaları ve JavaScript dosyalarında](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "uygulamadaki birden çok stil sayfaları ve JavaScript dosyaları")

    *Uygulama birden çok stil sayfaları ve JavaScript dosyaları*
3. Açık **Global.asax.cs** dosya.

    Dikkat yeni **Microsoft.Web.Optimization** ad alanı dışında bırakılır dosyasının başında. Using açıklamadan çıkarın paketleme ve küçültme özelliklerinin yönergesi.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Bulun **uygulama\_Başlat** yöntemi.

    Bu yöntemde, aşağıdaki kod parçacığında gösterildiği gibi EnableDefaultBundles çağrı açıklamasını kaldırın. Bu, klasörün yolunu kullanarak bir klasördeki CSS dosyaları ile birlikte gelen koleksiyonu başvuru sağlıyor artı &quot;CSS&quot; veya &quot;JS&quot; soneki.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Açık **Optimization.aspx** dosya ve bulmak için içerik denetimi **HeadContent**.

    CSS dosyaları ve tek bir başvurulan etikete sahip JS dosyaları dikkat edin.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Bu kod, tanıtım amaçları içindir. İdeal olarak, paketleri Site.Master dosyasına başvurur. Bu örnek kodda, bazı paketlenen dosyalar da Site.Master dosyası tarafından başvurulmayan, bu son başvuru yedekli yaparak bulabilirsiniz.
6. Bağlantıları paketleme kuralları kullanmakta olduğunu fark **href** stilleri ve Scripts\custom tüm CSS ve JS dosyaları almak için öznitelik klasör sırasıyla.

    Yol kullanabilirsiniz **betikleri/özel/JS** paketleme ve küçültme içindeki tüm JS dosyaları için aşağıda gösterildiği gibi bir **betikleri/özel** klasör. Bu varsayılan paketlerle varsayılan davranıştır.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Açık **Styles\Site.css** dosya.

    Özgün CSS dosyası girintili kodu, boşluk ve dosya büyütme açıklamaları içerdiğine dikkat edin. (Ayrıca JavaScript dosyası boş alanları ve açıklama içerir).

    ![Bir özgün CSS dosyaları betikler klasörüne](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "biri özgün CSS, betikler klasörüne dosyalar")

    *Özgün Scripts klasörü CSS dosyalarından birini*
8. Tuşuna **F5** gidin ve uygulamayı çalıştırmak için **iyileştirme** sayfası.
9. Tıklayarak **CSS paket** bağlantısını indirin ve dosyayı açın.

    Küçültülmüş paketlenmiş dosyasını gözden geçirin. Daha küçük bir dosya oluşturma tüm boşluk, açıklamalar ve girinti karakterler kaldırılmış olduğunu göreceksiniz.

    ![CSS dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "paketlenmiş CSS dosyaları")

    *Paketlenmiş CSS dosyaları*
10. Artık tıklayın **JS paket** paketlenmiş bir JavaScript dosyasını açmaya yönelik bağlantı. Uyarı Gezgini güvenle yoksayabilirsiniz. JavaScript dosyaları altındaki fark **özel** klasör Ayrıca paket ve küçültülmüş.

    ![JavaScript dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "paketlenmiş JavaScript dosyaları")

    *İle birlikte gelen JavaScript dosyaları*

    CSS veya JS dosyaları için sıkıştırmanın etkinleştirilmesi ASP.NET önceki bir sürümünde çok daha karmaşıktır. Gördüğünüz gibi artık, bir satır eklemek yeterlidir *Global.asax* paketleme etkinleştirmek için dosya ve ardından sitenizdeki paketlenen dosyalar başvuru.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Görev 3 - statik paket

Statik paket yaklaşım paket, başvuru ve kullanılacak küçültme yöntemi dosya kümesini özelleştirmenizi sağlar.

Bu görevde, dosyaları paketleme ve küçültme belirli bir kümesini tanımlamak için statik bir paket yapılandıracaksınız.

1. Tarayıcıyı kapatın.
2. Açık **Global.asax.cs** bulun ve dosya **uygulama\_Başlat** yöntemi.
3. Aşağıdaki kodda gösterildiği gibi statik paket kodun açıklamasını kaldırın.

    İle başvurulan statik bir paket tanımlama &quot; **~/StaticBundle** &quot; sanal yolunu ve kullanım **JsMinify** küçültme ile belirtilen tüm dosyaların için **AddFile** yöntemi. Son olarak, için statik paket ekliyoruz **BundleTable** ve onu etkinleştirme.

    Dosyaları yerinde bulunmayan dikkat edin. Varsayılan paketleme üzerinden başka bir avantajı budur.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Açık **Optimization.aspx** dosya.

    Dikkat bağlantısını **statik JS paket** bildirilen statik paket Global.asax.cs dosyasında yapılandırıldığında yolu kullanarak: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Tuşuna **F5** uygulamayı çalıştırın ve ardından gitmek **iyileştirme** sayfası.
6. Tıklayarak **statik JS paket** dosyayı açmaya yönelik bağlantı.

    Bildirim küçültülmüş JavaScript dosyası paketlenmiş emin olan statik paket dosyasının yolu altında yapılandırılan tüm JavaScript dosyaları için çıkış &quot;/StaticBundle&quot;.

    ![Statik JavaScript dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statik JavaScript dosyaları paket")

    *JavaScript dosyaları statik paket*
7. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Görev 4 - dinamik klasörü paketler

Bu görevde, dinamik klasörü paketler yapılandırma öğreneceksiniz. Dinamik paketleme, JavaScript ile derlenen dillerde statik JavaScript yanı sıra diğer dosyaları içerir ve bu nedenle, paketleme yürütülmeden önce bazı işlem yapmanız gerekmez güçtür.

Bu örnekte, nasıl kullanılacağını öğreneceksiniz **DynamicFolderBundle** CofeeScript yazılmış olan dosyalar için dinamik bir paket oluşturmak için sınıf. CofeeScript JavaScript ile derlenen ve daha basit bir söz dizimi, JavaScript kodu yazarak, JavaScript'in kısaltma ve okunabilirliği için sağlayan bir programlama dilidir.

1. Açık **Global.asax.cs** bulun ve dosya **uygulama\_Başlat** yöntemi.
2. Aşağıdaki kodda gösterildiği gibi dinamik paket kodun açıklamasını kaldırın.

    Kullanacağı bir dinamik klasör paketi tanımladığınız **CoffeeMinify** yalnızca dosyalarla uygulanacak özel küçültme İşlemci &quot; **.coffee** &quot; uzantısı () CoffeeScript dosyaları). Gibi bir klasör içinde paket dosyalarını seçmek için arama deseni kullanabilirsiniz bildirimi '\*.coffee'.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. NuGet Paket Yöneticisi konsolunu açın. Bunu yapmak için menüyü kullanın. **görünümü** | **diğer Windows** | **Paket Yöneticisi Konsolu**.
4. İçinde **Paket Yöneticisi Konsolu** türü **Install-Package CoffeeSharp** basın **ENTER**.
5. Tıklayın **tüm dosyaları göster** düğmesine **Çözüm Gezgini** penceresi

    ![Tüm dosyaları gösteren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "tüm dosyalar gösteriliyor")

    *Tüm dosyalar gösteriliyor*
6. Sağ tıklayın **CoffeeMinify.cs** dosyası **Çözüm Gezgini** seçip **Proje Ekle**

    ![CoffeeMinify.cs dosya projeye dahil et](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs dosya projeye dahil et")

    *CoffeeMinify.cs dosya projeye dahil et*
7. Açık **CoffeeMinify.cs** dosya.

    Bu sınıf, CoffeeScript kodu derlemeden kaynaklanan JavaScript çıkışını küçültülecek JsMinify devralır. CoffeeScript derleyici ilk JavaScript kodunu oluşturmak için çağırır ve sonuç kodunu küçültülecek JsMinify.Process yöntemi, gönderir.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Açık **Script1.coffee** ve **Script2.coffee** dosyalarını **betikleri/paket** klasör.

    Bu dosyaları paketleme CoffeeMinify sınıfıyla gerçekleştirilirken derlenecek CoffeScript kod içerir.

    Kolaylık olması amacıyla sağlanan CoffeeScript dosyaları yalnızca CoffeeScript örnek kod dahil. Yorumları JsMinify işlem tarafından bırakılır.

    ![CoffeeScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript dosyaları")

    *CoffeeScript dosyaları*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) JavaScript kodu yazarak, JavaScript'in kısaltma ve okunabilirliği, hem de dizi kavrama ve desen eşleştirme gibi diğer özellikler eklemek için daha basit bir söz dizimi sağlar.
9. Açık **Optimization.aspx** dosya ve paket bağlantıları bulun.

    Dikkat bağlantısını **dinamik JS paket** başvuruyor **betikleri/paket** kullanarak klasörüne **/kahve** dinamik klasör paketi için yapılandırılmış soneki.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Tuşuna **F5** uygulamayı çalıştırın ve ardından gitmek **iyileştirme** sayfası.
11. Tıklayarak **dinamik JS paket** oluşturulan dosyasını açmaya yönelik bağlantı.

    Yalnızca bu pakete eklenen içeriği içeren bir bildirim **.coffee** dosyaları. CoffeeScript kod için JavaScript derlenen ve derleme dışı bırakılan satırlar kaldırıldı de görebilirsiniz.

    ![Dinamik JS dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dinamik JS dosyaları paket")

    *Dinamik JS dosyaları paket*

> [!NOTE]
> Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu Laboratuvar, ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler ve Visual Studio 2012'de çeşitli geliştirmeler yararlanmak nasıl anlamanıza yardımcı olur.

Bu uygulamalı laboratuvarı tamamlayarak yeni özellikler ve geliştirmeler, CSS, JavaScript ve HTML için Visual Studio 2012 düzenleyicilerde kullanma modellerin. Ayrıca, Visual Studio 2012 yerleşik paketleme ve küçültme nasıl uyguladığını modellerin.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Web kutucuğu için VS Express*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure Portal'da oturum açın](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.

    ![Yayımlama profili web sitesi indiriliyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları. Kural için bir ad girin ve tıklayın ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Değişiklikleri onaylayın*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

    ![Uygulama Windows Azure'da yayımlanan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "uygulama yayımlanan Windows Azure'a")

    *Windows Azure'da yayımlanan uygulama*
