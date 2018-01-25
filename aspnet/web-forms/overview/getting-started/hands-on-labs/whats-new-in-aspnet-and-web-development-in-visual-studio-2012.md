---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "ASP.NET ve Web geliştirme Visual Studio 2012'de yenilikler | Microsoft Docs"
author: rick-anderson
description: "Yeni sürümü Visual Studio, bir dizi geliştirme deneyimi ve performans Web teknolojileri ile çalışırken geliştirmeye odaklanmış tanıtır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f0818cce2a82ede80556b3471cec9d965c3e987f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> Yeni sürümü Visual Studio, bir dizi geliştirme deneyimi ve performans Web teknolojileri ile çalışırken geliştirmeye odaklanmış tanıtır. IntelliSense ve otomatik girinti gibi en talep kodu yardımları çoğunu eklenecek CSS, JavaScript ve HTML için Visual Studio düzenleyicileri tamamen revamped. Performans ile ilgili paketleme ve küçültme kolayca sayfa azaltmak için yerleşik özellikleri yükleme süresi gibi şimdi tümleşiktir.
> 
> Visual Studio ile en son Web teknolojilerini çalışmanıza olanak tanır. Ayrıca Yeni HTML5 öğeleri ve özellikleri yararlanarak sırasında siteniz istemci platformundan bağımsız olarak çalıştığından emin olmak için tarayıcılar arası CSS3 parçacıkları kullanabilirsiniz.
> 
> Yazma ve JavaScript kodu profil oluşturma Visual Studio'nun bu sürümü ile daha kolay olmalıdır. IntelliSense listeleri, tümleşik XML belgeleri ve gezinti özellikleri için JavaScript kodu kullanıma sunulmuştur. Artık JavaScript katalog parmaklarınızın ucunda var. Ayrıca, komut dosyalarınızı ile ECMAScript5 uyumluluğu denetle ve erken bir aşamada sözdizimi hataları algılar.
> 
> Son ancak, bu Visual Studio sürümü yerleşik paketleme ve küçültme uygular. Komut dosyalarını ve stil sayfalarını paketlenmiş ve böylece sitenin daha hızlı gerçekleştirir sıkıştırılmış.
> 
> Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Laboratuvar üzerinde bu elinizde, öğreneceksiniz nasıl yapılır:

- Yeni özellikleri ve geliştirmeleri CSS Düzenleyicisi'nde kullanın
- Yeni özellikleri ve geliştirmeleri HTML Düzenleyicisi'nde kullanın
- JavaScript Düzenleyicisi'nde yeni özellikleri ve geliştirmeleri kullanın
- Yapılandırma ve paketleme ve küçültme kullanma

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (için Kurulum betikleri - Windows 8 ve Windows Server 2008 R2 üzerinde zaten yüklü)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - veya HTML5 ile uyumlu bir tarayıcı

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Laboratuvar bu durum aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?](#Exercise1)
2. [Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?](#Exercise2)
3. [Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?](#Exercise3)
4. [Alıştırma 4: Paketleme ve küçültme](#Exercise4)

Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?

Web geliştiricileri CSS düzenlemeye ilgili sorunlar çoğunu tanımanız gerekir. En büyük sorunları CSS stil oluşturma, tarayıcılar arası uyumluluk biridir. Genellikle, başka bir tarayıcı veya aygıt açarsanız stilleri sitenize uyguladıktan sonra, farklı göründüğüne dikkat edin, olur. Bu nedenle, son olarak, bir tarayıcıda çalışması yaptığınızda, onu diğer ayrılır, hayata geçirmek için bu görsel sorunlarını giderme önemli ölçüde zaman harcayabilir.

Visual Studio şimdi erişmek, iş ve CSS stil sayfaları etkili bir şekilde düzenlemek, geliştiricilerin özellikler içerir. Bu alıştırmada etkili bir kuruluş ve sürüm için yeni özellikler yanı sıra, tarayıcılar arası uyumluluk için CSS3 kod parçacıkları karşılar.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Görev 1 - yeni Düzenleyicisi özellikleri

Bu görevde, yeni özellikler CSS Düzenleyicisi'nin keşfeder. Bu yeni düzenleyici yeni akıllı girinti, geliştirilmiş kod açıklamaları ve Gelişmiş IntelliSense listesi yararlanarak üretkenliğinizi yardımcı olur.

1. Başlat **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.
2. Çözüm Gezgini'nde açık **Site.css** altında bulunan dosya **stilleri** klasör. Emin olun **metin düzenleyici** araçları araç çubuğunda görünür. Bunu yapmak için seçin **Görünüm** | **araç çubukları** menü seçeneğini ve onay **metin düzenleyici** seçenekleri. Bu yeni sürümün bu yana fark edeceksiniz **açıklama** düğmesi (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) ve **açıklamasını kaldırın** düğmesi (![açıklamadan çıkarın düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) CSS Düzenleyicisi de etkinleştirilir.

    ![Düzenleyicisi ve CSS araçları etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Düzenleyicisi ve CSS araçları etkinleştirme")

    *Etkinleştirme Düzenleyicisi ve CSS araçları*
3. Kod kaydırın ve herhangi bir CSS sınıfı tanımının seçin. Tıklatın **açıklama** (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) seçili satırların Açıklama düğmesi. Ardından **açıklamasını kaldırın** (![açıklamadan çıkarın düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) değişiklikleri geri almak için düğmesi.
4. Tıklatın **Daralt** (![Daralt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) ve **genişletme** (![genişletin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) düğmeleri bulunan metnin sol kenar boşluğu. Temizleyici bir görünüm için kullanmayın stilleri şimdi gizleyebilirsiniz dikkat edin.

    ![CSS sınıfları daraltma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "daraltma CSS sınıfları")

    *Daraltılan CSS sınıfları*
5. Akıllı girinti özelliği etkin olduğundan emin olun. Seçin **Araçları** | **seçenekleri** menü seçeneğini ve ardından **metin düzenleyici** | **CSS**  |  **Biçimlendirme** ekranın sol bölmede sayfası. Denetleme **hiyerarşik girinti** seçeneği.

    ![Hiyerarşik girinti etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "hiyerarşik girinti etkinleştirme")

    *Hiyerarşik girinti etkinleştirme*
6. Ana sınıf tanımını (.main) bulun ve div öğelerine stil ekleyin. Kodu otomatik olarak üst bir bakışta sınıfları bulmak için kullanıcılara yardımcı olma hizalar fark edeceksiniz.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![CSS'de hiyerarşik hizalama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS'de hiyerarşik hizalama")

    *CSS'de hiyerarşik hizalama*
7. İçinde **.main div** sınıfı, imleci sonunda bulun **kenarlık: 0px;** ve basın **Enter** IntelliSense listesini görüntülemek için. Yazmaya başlayın **üst** ve yazarken listeye nasıl filtre dikkat edin. Listede içeren öğeler görüntülenir **üst** herhangi bir bölümünü word'ün en (Visual Studio'nun önceki sürümleri öğeleri listesi filtrelenir, *başlamak* terimi ile).

    ![CSS IntelliSense geliştirmeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS IntelliSense geliştirmeleri")

    *CSS IntelliSense geliştirmeleri*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Görev 2 - Renk Seçici

Bu görevde, Visual Studio IntelliSense yeni CSS Renk Seçici tümleşik keşfeder.

1. İçinde **Site.css,** üstbilgi sınıf tanımını (.header) bulun ve imleci yanına yerleştirin **arka plan rengi** özniteliği, arasında &quot;:&quot; ve &quot; # &quot; kod satırını karakterlere **.**

    ![İmleç bulma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "imleci bulma")

    *İmleç bulma*
2. Silme **iki nokta üst üste** (:) ve renk seçici tekrar görüntülemek için yazma. Göreceğiniz ilk renkler, sitenizin en sık rastlanan renkleri olduğuna dikkat edin. Beyaz renk tıklatırsanız, HTML Renk kodunu (#fff) stil geçerli renk kodda yerini alır.

    ![Renk Seçici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Renk Seçici")

    *Renk Seçici*
3. Tuşuna **genişletme** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) düğmesine Renk Seçici renk gradyan görüntülemek ve farklı bir renk seçmek için gradyan imleci sürükleyin. Bundan sonra tıklatın **Damlalık** düğmesini tıklatın ve herhangi bir renk ekranından seçin. İmleç taşırken arka plan rengi değeri dinamik olarak değiştiğine dikkat edin.

    ![Renk Seçici gradyan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Renk Seçici gradyan")

    *Renk Seçici gradyan*
4. İçinde **Opaklık** kaydırıcı, seçici opaklık azaltmak için çubuğunda merkezine taşıyın. Arka plan rengi değeri şimdi bunun ölçeğini RGBA için değiştiğine dikkat edin.

    ![Renk Seçici Opaklık](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Renk Seçici opaklık")

    *Renk Seçici opaklık*

    > [!NOTE]
    > CSS3 RGBA (kırmızı, yeşil, mavi, alfa) renk tanımında tek bir öğe için renk geçirgenlik değeri tanımlamanızı sağlar. Farklı **Opaklık -** benzer bir CSS öznitelik  **-**  RGBA renkleri son tarayıcılarla uyumlu de.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Görev 3 - CSS uyumlu kod parçacıkları

Bu görevde, bazı özelliklerin, Web sitenizdeki uygulamak için tarayıcılar arası uyumlu CSS3 parçacıkları kullanmayı öğreneceksiniz.

1. İçinde **Site.css** dosya, bulun **üstbilgi** CSS sınıf tanımının (.header) ve imleci  **/ \*kenarlık RADIUS\* /**  yeni bir kod parçacığında eklemek için yer tutucu. Tuşuna **Enter** türü ve IntelliSense listesini görüntülemek için **RADIUS** listeyi filtrelemek için. Seçin **border-radius** seçeneği çift tıklatmayla listesinden ve tuşuna basarak **sekmesini** kod parçacığını eklemek için anahtar. Sonra bir RADIUS boyutu piksel yazıp tuşuna basın **Enter**. Örneğin, yazın **15px**.

    Kod parçacığını tarafından eklenen CSS3 öznitelikler Mozilla ve WebKit tabanlı tarayıcılar dahil olmak üzere çoğu HTML5 uyumluluk tarayıcılarda yuvarlatılmış Kenarlıklar işlemez.

    ![Border-radius parçacık kullanarak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "border-radius parçacık kullanma")

    *Border-radius parçacık kullanma*
2. Aynı uygulama **kenarlık** parçacıkları sayfa stilde (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Tuşuna **F5** çözümü çalıştırın. Her bir sayfa Kenarlıklar şimdi yuvarlanmasını dikkat edin.

    ![Yuvarlanmış köşeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "yuvarlanmış köşeleri")

    *Yuvarlak köşeleri*
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
5. Açık **Custom.css** altında bulunan dosya **stilleri** klasör ve içinde imleci **div.images ul li img** sınıf tanımının.
6. IntelliSense listesini görüntülemek için enter tuşuna basın türü **kutusunu gölge** ve basın **sekmesini** iki kez sınıf tanımının içinde varsayılan gölge kod parçacığını eklemek için anahtar. Gölge değerleri ayarlamak **10px 10px 5px #888**. Ardından, yazın **border-radius** ve kod parçacığını ekleyin. Tür **15px** RADIUS boyutu ve basın ayarlanacak **ENTER**.

    ![Yuvarlanmış köşeleri gölgeli](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "yuvarlanmış köşeleri gölgeli")

    *Gölgeli yuvarlak köşeleri*

    > [!NOTE]
    > Şu anda gölge öznitelik (moz webkit, o) Mozilla desteklemek için karşılık gelen öneki ve Webkit (Chrome, Safari Konkeror) tarayıcılar eklenir.
7. Yeni bir sınıf oluşturun **div.images ul li img:hover** aşağıda **div.images ul li img** sınıf tanımının ve imleci ayraçlar içinde **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Türü **dönüştürme** ve basın **sekmesini** iki kez dönüştürme parçacığını eklemek için anahtar. Ardından, girin **rotate(-15deg)** görüntüleri vurgulanan zaman döndürme açısı değeri değiştirmek için.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Tuşuna **F5** çözümü çalıştırın ve CSS3 sayfasına göz atın. Görüntüleri yuvarlanmış köşeleri ve gölgeleri kutusuna dikkat edin. Fare görüntüleri getirin ve bunları döndürme izleyin.

    ![Görüntüyü döndürme parçacığı dönüştürme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "dönüştürme parçacığı görüntüyü döndürme")

    *Görüntüyü döndürme parçacığı dönüştürme*

    > [!NOTE]
    > Internet Explorer 10 kullanıyorsanız ve gölgeleri göremiyorum ıe10 standartları belge modunda ayarlandığından emin olun. Tuşuna **F12** Internet Explorer Geliştirici Araçları açın ve tıklatın **belge modu** ıe10 standartları değiştirmek için.

    ![hakkında-ABD](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?

Visual Studio geliştirilmiş bir HTML düzenleyicisi vardır. Bu sürümde dahil iyileştirmeler HTML belgeler, HTML5 parçacıkları, HTML başlangıç ve bitiş etiketi eşleşen ve HTML doğrulama akıllı girinti bazılarıdır. Bu alıştırmada Web sitesi biçimlendirmede çalışırken bu değişiklikler, fluency nasıl artırmak görürsünüz.

CSS Düzenleyicisi gibi HTML Düzenleyicisi'ni de geliştirildi. Bu geliştirmeler çoğunu Web geliştirici yaşam kolaylaştırmak küçük olanlardır. Öğeleri düzenleme ve doğrulama HTML belgesi DOCTYPE hedefleme bu geliştirmelerin bazıları olduğunda başlangıç ve bitiş etiketleri eşleşen HTML5 için akıllı girinti, daha fazla kod parçacıkları ister.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Görev 1 - geliştirilmiş DOCTYPE doğrulama

Tanımı ana sayfasında olsa bile HTML düzenleyicisi artık sayfanızı, DOCTYPE denetlemek için özelliğine sahiptir. DOCTYPE sayfanızın bağlı olarak HTML düzenleyicisi doğru kuralları kümesi ile doğrular ve DOCTYPE öğeleri dikkate IntelliSense listesini filtreler.

Bu görevde, HTML düzenleyicisinin davranışı uygun şekilde nasıl değiştiğini görmek için sayfanın DOCTYPE değiştirir.

1. Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.
2. Açık **Site.Master** sayfası.
3. Hedef şema doğrulama araç dikkat edin. HTML Düzenleyicisi'ni (doğrulama, IntelliSense, vb.) davranışını düzgün seçili Doctype uyacak şekilde değişir.

    ![Kaynak HTML düzenleme araç çubuğunda doctype kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "kullanım Doctype kaynak HTML düzenleme araç çubuğu")

    *Kaynak HTML düzenleme araç çubuğunda doctype kullanın*
4. Hedef şemanın HTML 4.01 değiştirin.

    ![Kaynak HTML düzenleme araç çubuğunda doctype değiştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "değiştirme Doctype kaynak HTML düzenleme araç çubuğu")

    *Kaynak HTML düzenleme araç çubuğunda doctype değiştirme*
5. İmleci altında **gövde** öğesi ve bir HTML5 öğesinin adını yazarak başlangıç (örneğin, **video**). Öğe IntelliSense listesinde kullanılabilir olmadığına dikkat edin.

    ![Listelenmeyen HTML5 öğeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "listelenmeyen HTML5 öğeleri")

    *Listelenmeyen HTML5 öğeleri*
6. Doğrulama için araç, DOCTYPE çekme hedef şema değişiklikleri geri: aşağı açılan listeden XHTML5.

    ![Kaynak HTML düzenleme araç çubuğunda doctype kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "kullanım Doctype kaynak HTML düzenleme araç çubuğu")

    *Kaynak HTML düzenleme araç çubuğunda doctype Sıfırla*
7. İmleci altında **gövde** öğesi ve HTML5 öğeyi yeniden yazmaya başlayın (örneğin, ister **video**). HTML5 öğeleri IntelliSense listesinde kullanılabilir olduğuna dikkat edin.

    ![HTML5 öğeleri listelenmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "listelenmesini HTML5 öğeleri")

    *Listelenmesini HTML5 öğeleri*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Görev 2 - Başlangıç/bitiş etiketleri otomatik güncelleştirme

Visual Studio şimdi açma veya kapatma birleriyle eşleşmesi için düzenleme öğesi etiketleri HTML güncelleştirir. Bu yeni özellik, HTML etiketleri düzenlerken üretkenliğinizi artırır.

1. Üzerinde **Default.aspx** sayfasında, eklemek bir **H3** öğesi ile bir başlık (örneğin, Visual Studio 2012 Rocks!).


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Değişiklik **H3** etiketi ve türü **H2** veya **H1.**

    Bitiş etiketi otomatik olarak güncelleştirir dikkat edin. Bitiş etiketi başlangıç etiketiyle buna göre çok güncelleştirdiğini görmek için de değiştirebilirsiniz.

    ![Bitiş etiketi otomatik güncelleştirilmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "bitiş etiketi otomatik güncelleştirilmesi")

    *Bitiş etiketi otomatik güncelleştirilmesi*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Görev 3 - Yeni HTML5 kod parçacıkları

Visual Studio artık birkaç HTML5 kod parçacıkları içerir. Bu görevde, bu parçacıkları bazılarını kullanacaksınız.

1. Adlı yeni bir klasör ekleyin **ses** kök web sitesi klasörünün. Windows Gezgini'ni açın ve tüm ses dosyaya kopyalayın **ses** klasöründe **WhatsNewASPNET.sln** çözümü.
2. İçinde **Default.aspx** sayfasında, imleci Web11 Rocks altında bulun!! Üstbilgi. Tür **ses** ve SEKME tuşuna basın.

    Kod parçacıkları HTML5 içerik için yeni HTML Düzenleyicisi'ni içerir. HTML5 parçacıkları etkinleştirmek için uygun DOCTYPE tanımı kullanmayı unutmayın.

    ![HTML5 kod parçacıkları ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 kod parçacıkları ekleme")

    *HTML5 kod parçacıkları ekleme*
3. Ses kaynağı varolan ses dosyasına işaret edecek şekilde güncelleştirin.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Çözüme ses dosyası eklemeniz gerekir.
4. Tuşuna **F5** siteyi çalıştırın ve ses yürütmek için.

    ![Ses denetimi çalıştıran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "ses denetimini çalıştırma")

    *Ses denetimini çalıştırma*

    > [!NOTE]
    > Visual Studio'da bulunan video, şekil, vb. gibi daha fazla parçacıkları da deneyebilirsiniz.
5. Sayfanın bazı parçası bir denetim eklemek şimdi deneyin. Örneğin, INSERT çalıştığınızda bir **GridView** denetim, ancak yazmak yerine  **&lt;Kıla,** yazmaya başlayın  **&lt;GV**. IntelliSense listesini gösteren bildirim **asp: GridView** denetim.

    IntelliSense HTML Düzenleyicisi'nde şimdi başlık büyük/küçük harf arama yanı sıra kısmi eşleşen (terimi içeren tüm öğeleri alınıyor) sağlar.

    ![GridView IntelliSense listeleriyle ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense listeleriyle GridView ekleme")

    *GridView IntelliSense listeleriyle ekleme*

    Yazarsanız  **&lt;kılavuz** terimiyle eşleşen tüm öğeleri alırsınız, ancak Visual Studio önermek **gridview** denetimi:

    ![IntelliSense listeler ve kısmi eşleşen bir GridView ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense listeler ve kısmi eşleşen bir GridView ekleme")

    *IntelliSense listeler ve kısmi eşleşen bir GridView ekleme*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Görev 4 - HTML düzenleyicisi akıllı etiketler

Başka bir geliştirme HTML Düzenleyicisi'nde akıllı etiketler özelliğidir. Akıllı etiketler bir denetim başına temelinde ortak ve yinelenen geliştirme görevleri gerçekleştirmek kolaylaştırır. Bu özellik zaten HTML Tasarımcısı'nda, ancak olmayan HTML Düzenleyicisi'nde.

1. Açık **Site.Master** ve bulun **asp: menü** öğesi. İmleci başlangıç etiketi ve öğe - alt kısmında görüntülenen küçük karakter akıllı görevler menüsünü açmak için tıklatın dikkat yerleştirin. Menü denetimi ile ilgili bazı görevler hızlı erişimi olmasını dikkat edin.

    ![Akıllı menü denetimi görevlerde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "akıllı menü denetimi için görevler")

    *Menü denetimi için akıllı görevleri*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Görev 5 - akıllı girinti

Bir en iyi uygulamaları HTML kod okunabilir tutmak için iç içe öğelerin girintileme. Visual Studio 2012'de kod yazarken Düzenleyicisi öğeleri otomatik olarak girinti fark edeceksiniz.

> [!NOTE]
> Visual Studio'nun önceki sürümü akıllı girinti kullanılabilir XML düzenleyicisinde ancak HTML Düzenleyicisi'nde.


1. Indenting yapılandırma üzerinde HTML düzenleyicisi akıllı girinti ayarlandığından emin olun. Bunu yapmak için seçin **Araçlar | Seçenekler** menü seçeneğini ve ardından **metin düzenleyici | HTML | Sekmeleri** ekranın sol bölmede sayfası. Akıllı girinti seçeneğini belirleyin.

    ![HTML düzenleyicisi ayarları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML düzenleyicisi ayarları")

    *HTML düzenleyicisi ayarları*
2. Üzerinde **Default.aspx** sayfasında, audio öğesi altında tüm içeriğini kaldırın.
3. İmleç açılış sonuna yerleştirin **ses** öğesi ve isabet **ENTER**.

    İmleç yeni konumunu bir ek girinti düzeyi olduğuna dikkat edin.

    ![Akıllı girinti HTML Düzenleyicisi'nde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "akıllı girinti HTML Düzenleyicisi'nde")

    *Akıllı girinti HTML Düzenleyicisi'nde*
4. Ses etiketi kaldırdıysanız veya kapatın içerik ile geri **Default.aspx** değişiklikleri kaydetmeden.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Görev 6 - kullanıcı denetimi için Extract

Bir işlev kodu bir kısmı ayıklanıyor gibi Visual Studio'da bulunan Refactoring geliştirme ve var olan kodu yeniden düzenleme kolaylaştıran harika özellikler araçlardır. ASP.NET sayfaları için karşılık gelen HTML kod ayıklama bir kullanıcı denetimi için olacaktır. El ile yapılması yeni bir kullanıcı denetimi oluşturma, kod bölümünde belirtilen kullanıcı denetimine taşıma, kullanıcı denetimi için etiket öneki kaydetme ve, son olarak, kullanıcı denetimi sayfalarında başlatmasını gibi çeşitli adımları içerir. Artık, yeni *kullanıcı denetimine çıkar* aracı otomatik olarak gerçekleştirir bu adımların tümünü sizin için.

Bu görevde, kullanıcı denetimi bağlamsal işlemi için yeni Ayıkla seçili koddan yeni bir kullanıcı denetimi oluşturmak için kullanın.

1. Üzerinde **Default.aspx** sayfasında, **H2** ve **ses** öğeleri.
2. Sağ tıklayın ve **kullanıcı denetimine çıkar**.

    ![Kullanıcı denetimi menü seçeneğine ayıklamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "kullanıcı denetimi menü seçeneğine Ayıkla")

    *Kullanıcı denetimi menü seçeneğine Ayıkla*
3. Yeni kullanıcı denetimi için bir ad yazın. Örneğin, **Jukebox.ascx**ve ardından **Tamam**.

    ![Ayıklanan kullanıcı denetimi kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "ayıklanan kullanıcı denetimi kaydetme")

    *Ayıklanan kullanıcı denetimi kaydetme*
4. Seçilen koda bir kullanıcı denetimi ayıklandı ve seçili kod özgün konumuna yeni kullanıcı denetimi örneği ile değiştirilen dikkat edin.

    ![Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir")

    *Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir.*
5. Tuşuna **F5** sayfayı çalıştırın ve denetim çalıştığını doğrulayın.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?

Yazma veya JavaScript kod düzenleme kolay bir görev özellikle uygulamanızı boyutları büyürken başlar ve kendiniz uzun dosyalarını postalarla ve işlevleri yüzlerce bulabilirsiniz değil. Komut dosyası geliştiriciler genellikle kod okunabilirliği korumak ve dosyalar arasında gezinmek için bazı ek çalışmalar yapmanız gerekir. JavaScript kitaplıklarını jQuery gibi ekleme ile bir sınama kendisini kod uzunluğu nedeniyle betik Gezinti haline gelmiştir.

Visual Studio, erişilebilir ve düzenli kod modu yapma taahhüdü JavaScript düzenleyicisiyle yeniledi. C# veya VB düzenleyicilerde zaten var olan birçok Visual Studio özellikleri artık JavaScript Düzenleyicisi'nde uygulanır: Tanıma Git, otomatik girinti, belgelerine ve yazarken doğrulama. Yenilenen IntelliSense listesiyle parmaklarınızın ucunda JavaScript işlevi katalog sahip olur.

Bu alıştırmada, bazı yeni özellikler ve geliştirmeler JavaScript Düzenleyicisi öğreneceksiniz. Örnek dosyalara gözatın ve her biri, JavaScript programlama Visual Studio 2012 içinde daha verimli hale getirir yeni özellikleri keşfedin.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Görev 1 - JavaScript Düzenleyicisi yeni özellikler

Bu görev, kodunuzun organize ve daha iyi bir kullanıcı deneyimi getiren üzerinde odaklanmak yeni JavaScript Düzenleyicisi özelliklerinden bazıları için tanıtılacaktır.

1. Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.
2. Tuşuna **F5** uygulamayı çalıştırmak için ardından Gezinti çubuğundaki JavaScript bağlantısını tıklatın. Sayfa birkaç kez onay nasıl yenileyin ve sayaç artırır.

    ![Sayfa Sayacı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "sayfa sayacı")

    *Sayfa Sayacı*
3. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.
4. Açık **JavaScript.aspx** sayfasında ve bulun  **&lt;betik&gt;**  blok (aşağıda gösterilen).

    Aşağıdaki kod depolamak için HTML5 yerel depolama kullanan bir *pageLoadCount* sayfanın geçerli kullanıcı tarafından ziyaret edildiğini sayısı depolar değişkeni. Yerel depolama, HTML5 standart sunulan istemci-tarafı bir anahtar-değer veritabanıdır. Veriler, yerel makinede kullanıcının tarayıcıda kaydedilir.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > DOCTYPE XHTML5 için sonraki adımlara devam etmeden önce ayarlandığından emin olun.
5. Kod düzenleyin ve JavaScript için IntelliSense yerel depolama ve kendi iç yöntemlerini gibi HTML5 özellikleri içerdiğine dikkat edin.

    ![JavaScript HTML5 JavaScript özellikleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript özellikleri")

    *JavaScript HTML5 JavaScript özellikleri*
6. Tüm ayraç tıklayın (**{**) komut dosyası kodu ve köşeli ayraçlar vurgulanmıştır dikkat edin.

    ![Köşeli ayraçlar vurgulanır](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "köşeli vurgulanır")

    *Köşeli ayraçlar vurgulanmış*
7. İşlev açıklamadan çıkarın **testAutoAlign()** (üç satırları seçin ve kullanabileceğiniz **CTRL** + **K**; **CTRL** + **U**) ve imleci işlev kodu içinde bulun. İkinci satır eklemek için enter tuşuna basın. Kod artık olduğuna dikkat edin **hizalı** ve **otomatik girintili**.

    ![JavaScript kodudur hizalı otomatik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript kodu hizalı otomatik.")

    *JavaScript kodu hizalı otomatik.*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Görev 2 - doğrulama JavaScript

Bu görevde, yeni bir JavaScript doğrulama ECMAScript5 standart keşfeder. Bu özellik site dağıtmadan önce önleme kodlama sorunları çalışırken uyumlu JavaScript kodu yazma yardımcı olur.

> [!NOTE]
> Visual Studio 2012 ECMAScript5 uyumluluk sağlarken visual Studio 2010 ECMAStript3 uyumluluk uygulanmadı.


1. Açık **ECMA5script5.js** altında bulunan **Scripts\custom** proje klasörü. Standart ECMAScript5 için doğrulama sınayacak.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Bakabilirsiniz &quot; **katı kullan** &quot; ECMAScript5 sağlayan dosyasının ilk satırı yönde **katı mod**. Bu mod, geçmiş sürümünden belirsizlikleri açıklar ve nesne özellikleri alıcılar ve ayarlayıcılar ve JSON ve daha kapsamlı yansıma için kitaplık desteği gibi bazı yeni özellikler ekler dilini bir alt kümesindeki oluşur.
2. Açık **hata listesi** zaten açtıysanız (**Görünüm** menü | **Hata listesi**). Bildirim **işlevi** bildirimi altı çizilir. ECMA5 standart işlev dil yapıları içinde iç içe geçirilemez olmasıdır. Hata listesi aşağıdaki uyarı ayrıntılarını görürsünüz.

    ![JavaScript doğrulama hatası iletisini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript doğrulama hata iletisi")

    *JavaScript doğrulama hata iletisi*
3. Out açıklama  **&quot;katı kullan&quot;**  yön ve hataları görünmez, ancak uyarılar kalır dikkat edin.
4. Dosyanın son satırının gibi herhangi bir dize yazma  **&quot;test&quot;**  (dize olarak belirtmek için tırnak işaretlerini kullanın). Bir süre IntelliSense listesini görüntülemek ve seçmek için dize yanındaki yazma **kırpma** seçeneği.

    ECMAScript5 standart dize değerleri ve değişkenleri Ayrıca, kırpma, büyük harf, arama ve değiştirme gibi tanımlanan dize yöntemleri vardır.

    ![JavaScript IntelliSense listesinde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "JavaScript IntelliSense listesinde")

    *JavaScript IntelliSense listesinde*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Görev 3 - JavaScript için XML belgeleri

Bu görevde, JavaScript XML belgelerinde için Visual Studio özellikleri inceleyeceksiniz. JavaScript IntelliSense listesi şimdi her işlevin XML belgeleri gösterir görürsünüz. Ayrıca, JavaScript Gezinti özelliğinde keşfeder.

1. Açık **XMLDoc.js** bulunan dosya **betikleri/özel** proje klasörü. Bu dosya, XML belgeleri her JavaScript işlevleri içerir.

    ![JavaScript XML belgeleri için IntelliSense tümleşik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML belgeleri tümleşik için IntelliSense")

    *IntelliSense için tümleşik JavaScript XML belgeleri*
2. Aşağıda **ekleme** işlevi **XMLDoc.js** dosya, adlı yeni bir işlev oluşturun **test**.
3. İçinde **test** işlev, çağrı **Çarp** iki parametre alan işlevi. Araç İpucu kutusunu gösteren fark **Çarp** işlev belgeleri.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![JavaScript işlevleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript işlevleri için XML belgeleri")

    *JavaScript işlevleri için XML belgeleri*
4. Tür ve işlevi call deyimi tamamlamak bir *nokta* döndürülen değer IntelliSense Listesi'ni açın. Visual Studio algılama bildirimi **dönmek** değeri bir sayı olarak davranma belgeleri değeri.

    ![Dönüş türleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dönüş türleri için XML belgeleri")

    *Dönüş türleri için XML belgeleri*
5. Şimdi, işlev eklemek için bir çağrı ekleyin. JavaScript Düzenleyicisi artık işlev aşırı yüklemelerinin destekler dikkat edin. İşlev adı yazdığınızda, belgede belirtilen kullanılabilir aşırı birini seçin kuramaz.

    ![XML belgeleri aşırı yüklemeleri için](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "aşırı yüklemeleri için XML belgeleri")

    *Aşırı yüklemeler için XML belgeleri*
6. Açık **GotoDefinition.js** dosya ve bulun **$().html()** işlev çağrısı. İmleç bulmak **html**.
7. Tuşuna **F12** ve tanımına gidin. Şimdi erişmek ve JavaScript kodu kullanmadan Gözat bildirimi **Bul** aracı.
8. Kod dosyası sonundaki imza bloğunu önce jQuery yönerge üzerinde imleci bulun. Tuşuna **F12**. JQuery kitaplığı dosyasına gider. De gezinmenizi kullanarak jQuery dosyalardaki fark **F12**.

    ![JQuery tanımları için gezinme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery tanımları için gezinme")

    *JQuery tanımları için gezinme*

> [!NOTE]
> Dosyayı kaydetmeden önce GotoDefinition.js sözdizimi hatası olduğundan emin olun.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Alıştırma 4: Paketleme ve küçültme

Kaç kez sitelerinizi birden fazla JavaScript ya da CSS dosyası dahil mi? Paketleme ve küçültme dosya boyutunu azaltıp daha hızlı gerçekleştirmek site yapmak için burada sağlayabilir çok yaygın bir senaryo budur. Yeni paket özelliği ASP.NET 4.5 tek bir öğeye JS ya da CSS dosyaları kümesini paketleri ve boyutuna (yani gerekli boşluk kaldırma, açıklamaları kaldırma, tanımlayıcılar azaltma) içerik küçültülmesine tarafından azaltır.

Paketleme ve küçültme ASP.NET 4.5 içinde gerçekleştirilir çalışma zamanında, böylece işlemi kullanıcı aracısı (örneğin IE, Mozilla, vb.) tanımlamak ve bu nedenle, kullanıcı tarayıcı (Mozilla belirli olan örneği için kaldırma şeyler hedefleyerek sıkıştırma geliştirmek ne zaman istek IE gelir).

Bu alıştırmada, ASP.NET 4.5 paketleme ve küçültme farklı türde etkinleştirmek ve nasıl kullanılacağını öğreneceksiniz.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Görev 1 - paketleme yükleme ve küçültme NuGet paketi

1. Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.
2. NuGet Paket Yöneticisi Konsolu'nu açın. Bunu yapmak için menüyü kullanın **Görünüm** | **diğer pencereler** | **Paket Yöneticisi Konsolu**.

    ![Paket Yöneticisi file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Paket Yöneticisi konsolu açma")

    *Paket Yöneticisi konsolu açma*
3. İçinde **Paket Yöneticisi Konsolu** türü **Install-Package Microsoft.Web.Optimization** ve basın **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Görev 2 - varsayılan paketleri

Paketleme ve küçültme kullanmanın en basit yolu, varsayılan paketleri sağlamaktır. Bu yöntem, bir klasördeki JS ve CSS dosyaları ile birlikte gelen ve küçültülmüş sürümü başvuru olanak kuralları kullanır.

Bu görevde, etkinleştirmek ve ile birlikte gelen ve küçültülmüş JS ve CSS dosyaları başvuru ve sonuçta çıktı görüntülemek öğreneceksiniz.

1. Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.
2. İçinde **Çözüm Gezgini**, genişletin **stilleri**, **Scripts\custom** ve **Scripts\bundle** klasörler.

    Uygulama birden fazla CSS ve JS dosyası kullanıyor dikkat edin.

    ![Uygulama içinde birden çok stil sayfaları ve JavaScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "uygulamada birden çok stil sayfaları ve JavaScript dosyaları")

    *Uygulama içinde birden çok stil sayfaları ve JavaScript dosyaları*
3. Açık **Global.asax.cs** dosya.

    Dikkat yeni **Microsoft.Web.Optimization** ad alanı geçersiz kılınan çıkış dosyasının başında. Using açıklamadan çıkarın paketleme ve küçültme özellikleri içerecek şekilde yönergesi.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Bulun **uygulama\_Başlat** yöntemi.

    Bu yöntemde EnableDefaultBundles çağrısı parçacığında gösterildiği gibi açıklamadan çıkarın. Bize bu klasöre yol kullanılarak bir klasördeki CSS dosyaları ile birlikte gelen koleksiyonu başvurmak böylece artı &quot;CSS&quot; veya &quot;JS&quot; soneki.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Açık **Optimization.aspx** dosya ve içerik denetimi için bulun **HeadContent**.

    CSS dosyaları ve tek bir başvurulan etiketine sahip JS dosyaları dikkat edin.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Bu kod, tanıtım amacıyla kullanılır. İdeal olarak, paketleri Site.Master dosyasına başvurur. Bu örnek kod, bazı paketlenen dosyalar da Site.Master dosyası tarafından başvurulduğundan son bu başvuruyu yedekli yapmadan bulacaksınız.
6. Bağlantıları paket kuralları kullanmakta olduğunu fark **href** stilleri ve Scripts\custom tüm CSS veya JS dosyaları almak için öznitelik klasörü sırasıyla.

    Yolun kullanabilirsiniz **özel/betikleri/JS** paketini ve içindeki tüm JS dosyaları minify için aşağıda gösterildiği gibi bir **betikleri/özel** klasör. Varsayılan paketlerle varsayılan davranış budur.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Açık **Styles\Site.css** dosya.

    Özgün CSS dosyası girintili kodu, boşluk ve dosyayı büyütmek açıklamaları içerdiğine dikkat edin. (Ayrıca JavaScript dosyası boş alanları ve açıklamaları içerir).

    ![Komut dosyaları klasördeki dosyaları özgün CSS birini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "özgün CSS birini Scripts klasöründeki dosyaları")

    *Betikler klasörüne özgün CSS dosyalarında biri*
8. Tuşuna **F5** uygulamayı çalıştırın ve gitmek için **en iyi duruma getirme** sayfası.
9. Tıklayın **CSS paket** indirin ve dosyayı açmak için bağlantı.

    Küçültülmüş ile birlikte gelen dosyalarını inceleyin. Daha küçük bir dosya oluşturma tüm boş alanları, açıklamalar ve girinti karakterler kaldırılmış olduğunu göreceksiniz.

    ![CSS dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "paketlenmiş CSS dosyaları")

    *İle birlikte gelen CSS dosyaları*
10. Şimdi **JS paket** paketlenmiş JavaScript dosyasını açmak için bağlantı. Uyarı explorer güvenle yoksayabilirsiniz. JavaScript dosyaları altında fark **özel** klasörü Ayrıca paket ve küçültülmüş.

    ![JavaScript dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "paketlenmiş JavaScript dosyaları")

    *İle birlikte gelen JavaScript dosyaları*

    CSS veya JS dosyaları için sıkıştırmayı etkinleştirme önceki ASP.NET sürümünde çok daha karmaşıktır. Gördüğünüz gibi şimdi bir satır eklemek yeterlidir *Global.asax* paketleme etkinleştirmek için dosya ve ardından paketlenen dosyalar sitenizden başvuru.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Görev 3 - statik paketleri

Statik paket yaklaşım, paket, başvuru ve kullanılacak küçültme yöntemi dosyaları kümesini özelleştirmenize olanak tanır.

Bu görevde, belirli bir paket ve minify dosyaları kümesini tanımlamak için statik bir paket yapılandırır.

1. Tarayıcıyı kapatın.
2. Açık **Global.asax.cs** dosya ve bulun **uygulama\_Başlat** yöntemi.
3. Aşağıdaki kodda gösterildiği gibi statik paket kodun açıklamasını kaldırın.

    İle başvurulan statik bir paket tanımlama &quot; **~/StaticBundle** &quot; sanal yolunu ve kullanım **JsMinify** ile belirtilen tüm dosyaların küçültme için **AddFile** yöntemi. Son olarak, statik paket ekleme **BundleTable** ve onu etkinleştirme.

    Dosyaları aynı yerde bulunmayan dikkat edin. Varsayılan paketleme üzerinden başka bir avantajı budur.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Açık **Optimization.aspx** dosya.

    Dikkat bağlantısını **statik JS paket** bildirilen Global.asax.cs dosyasında statik paket yapılandırıldığında yolu kullanarak: **/StaticBundle**.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Tuşuna **F5** uygulamayı çalıştırın ve ardından gidin **en iyi duruma getirme** sayfası.
6. Tıklayın **statik JS paket** bağlantı dosyasını açın.

    Küçültülmüş JavaScript dosyası paketlenmiş olduğunu bildiriminin geldiği yolunda statik paket dosyasında yapılandırılmış tüm JavaScript dosyaları için çıktıyı &quot;/StaticBundle&quot;.

    ![Statik JavaScript dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statik JavaScript dosyaları paket")

    *Statik JavaScript dosyaları paket*
7. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Görev 4 - dinamik klasörü paketleri

Bu görevde, dinamik klasörü paketleri yapılandırma konusunda bilgi edineceksiniz. Dinamik paketleme, güç JavaScript ile derlenen dillerde statik JavaScript gibi diğer dosyaları içerir ve bu nedenle, paketleme yürütülmeden önce bazı işleme gerektiren ' dir.

Bu örnekte, nasıl kullanılacağını öğreneceksiniz **DynamicFolderBundle** CofeeScript içinde yazılmış dosyaları için dinamik bir paket oluşturmak için sınıfı. CofeeScript JavaScript ile derlenen ve JavaScript kodu yazma, JavaScript'in kısaltma ve okunabilirliği artırma için daha basit bir söz dizimi sağlayan bir programlama dilidir.

1. Açık **Global.asax.cs** dosya ve bulun **uygulama\_Başlat** yöntemi.
2. Aşağıdaki kodda gösterildiği gibi dinamik paket kodun açıklamasını kaldırın.

    Kullanacağınız bir dinamik klasör paketi tanımlama **CoffeeMinify** ile dosyaları yalnızca uygulanacağı özel küçültme İşlemci &quot; **.coffee** &quot; uzantısı ( CoffeeScript dosyaları). Bir klasör içinde gibi gruplanacağını dosyalarını seçmek için bir arama deseniyle kullanabilirsiniz bildirimi '\*.coffee'.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. NuGet Paket Yöneticisi Konsolu'nu açın. Bunu yapmak için menüyü kullanın **Görünüm** | **diğer pencereler** | **Paket Yöneticisi Konsolu**.
4. İçinde **Paket Yöneticisi Konsolu** türü **Install-Package CoffeeSharp** ve basın **ENTER**.
5. Tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** penceresi

    ![Tüm dosyaları gösteren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "tüm dosyaları gösterme")

    *Tüm dosyaları gösterme*
6. Sağ tıklayın **CoffeeMinify.cs** dosyasını **Çözüm Gezgini** seçip **Proje Ekle**

    ![CoffeeMinify.cs dosyasını projeye dahil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs dosya projeye ekleyin")

    *Projede CoffeeMinify.cs dosya ekleyin*
7. Açık **CoffeeMinify.cs** dosya.

    Bu sınıf CoffeeScript kod derlemeden kaynaklanan JavaScript çıkış küçültülecek JsMinify devralır. JavaScript kodu ilk oluşturmak için CoffeeScript derleyici çağırır ve ortaya çıkan kodu küçültülecek JsMinify.Process yöntemi o gönderir.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Açık **Script1.coffee** ve **Script2.coffee** dosyaları buradan **betikleri/paket** klasör.

    Bu dosyalar CoffeeMinify sınıfıyla paketleme gerçekleştirilirken derlenecek CoffeScript kodu içerir.

    Kolaylık olması amacıyla, sağlanan CoffeeScript dosyaları yalnızca CoffeeScript örnek kod dahil. Açıklamaları JsMinify işlem tarafından bırakılır.

    ![CoffeeScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript dosyaları")

    *CoffeeScript dosyaları*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) JavaScript kodu yazma, JavaScript'in kısaltma ve okunabilirliği artırma yanı dizi kavrama ve desen eşleştirme gibi diğer özellikleri eklemek için daha basit bir sözdizimi sağlar.
9. Açık **Optimization.aspx** dosya ve paket bağlantıları bulun.

    Dikkat bağlantısını **dinamik JS paket** başvuruyor **betikleri/paket** kullanarak klasörüne **/kahve** dinamik klasör paketi için yapılandırılmış soneki.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Tuşuna **F5** uygulamayı çalıştırın ve ardından gidin **en iyi duruma getirme** sayfası.
11. Tıklayın **dinamik JS paket** bağlantı oluşturulan dosyasını açın.

    Bu pakete eklenen içeriğin yalnızca bulunduğu bildirimi **.coffee** dosyaları. CoffeeScript kodu JavaScript için derlenmiş ve geçersiz kılınan satır kaldırılmıştır de görebilirsiniz.

    ![Paketi dinamik JS dosyalarının](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dinamik JS dosyaları paket")

    *Dinamik JS dosyaları paket*

> [!NOTE]
> Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu Laboratuvar, yeni ASP.NET ve Visual Studio 2012 Web geliştirme nedir ve Visual Studio 2012'de çeşitli geliştirmeler yararlanmak nasıl anlamanıza yardımcı olur.

Bu uygulamalı Laboratuvar tamamlayarak yeni özellikleri ve geliştirmeleri Visual Studio 2012 düzenleyicilerde CSS, JavaScript ve HTML için nasıl kullanılacağını learnt. Ayrıca, Visual Studio 2012 yerleşik paketleme ve küçültme nasıl uyguladığını learnt.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express Web döşemeye*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure portalında oturum açtığı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açtığı*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları. Kural için bir ad girin ve tıklayın ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) düğmesi.

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Değişiklikleri onaylamak*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

    - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
    - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
    - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
    - Yeni bir veritabanı adı girin: *MVC4SampleDB*.

    ![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "hedef bağlantı dizesi yapılandırma")

    *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

    ![Uygulama için Windows Azure yayımlanan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "uygulama için Windows Azure yayımlanır")

    *Windows Azure için yayımlanan uygulama*
