---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Visual Studio 2012'de ASP.NET Web formları için sayfa denetçisi kullanarak | Microsoft Docs"
author: rick-anderson
description: "Visual Studio 2012 için sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa Denetçisi ' herhangi bir öğe seçin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Visual Studio 2012'de ASP.NET Web formları için sayfa denetçisi kullanma
====================
tarafından Tim Ammann

> Visual Studio 2012 için sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular. Herhangi bir sayfayı uygulamanızda göz atın, hızlı bir şekilde oluşturulan biçimlendirmenin kaynakları bulabilir ve Visual Studio ortamında sağ tarayıcı araçları kullanın.
> 
> Bu öğretici shwos nasıl denetleme modunu etkinleştirin ve ardından hızla bulup CSS kuralları ve web projeniz içindeki metni düzenleyin. Öğretici bir Web Forms uygulaması projesi kullanır, ancak sayfa denetçisi Web sitesi projeleri için de kullanabilirsiniz ve [MVC](https://go.microsoft.com/?linkid=9802002) uygulamalar.
> 
> Öğretici aşağıdaki bölümleri içerir:
> 
> [Önkoşulları](#_1_prerequisites)
> 
> [Bir Web uygulaması oluşturma](#_2_creating_a)
> 
> [Sayfa denetçisi uygulamayı görüntülemek için kullanın](#_3_using_page)
> 
> [Denetleme modunu etkinleştir](#_4_inspection_mode)
> 
> [Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın](#_5_using_page)
> 
> [Denetleme modu ve HTML penceresi](#_6_inspection_mode)
> 
> [Stilleri penceresinde CSS Değişiklikleri Önizle](#_7_previewing_css)
> 
> [CSS otomatik eşitleme](#css_auto_sync)
> 
> [CSS Renk Seçici kullanma](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Sayfa Denetçisi'nın en son sürümünü almak için [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Azure SDK'sını yüklemek için.


Sayfa denetçisi, Microsoft Web geliştirici araçları ile gelir. 1.3 en son sürümüdür. Hangi sürümü denetlemek için sahip, Visual Studio çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Bir Web uygulaması oluşturma

İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturacaksınız. Visual Studio'da, **dosya** &gt; **yeni proje**. Sol bölmede, genişletin **Visual C#**seçin **Web**ve ardından **ASP.NET Web Forms uygulaması**.

![Yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

**Tamam**'ı tıklatın.

Uygulama açılır **kaynak** görünümü.

![Kaynak görünümünde yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Çalışmak için bir uygulamanız varsa, incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Sayfa denetçisi uygulamayı görüntülemek için kullanın

Ardından, sayfa denetçisi ile uygulama görebilir. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **sayfa denetçisi görünümünde**.

![Sayfa denetçisi görünümünde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Varsayılan olarak, bu sayfa denetçisi ilk kez başlatıldığında dar bir pencere olarak Visual Studio ortamı sol tarafta yerleştirilir. Sol tarafta yerleşik ve sizin için rahatça ya da bir aracı alanlarının üstünde, Alttan veya sağ yerleştirme genişliği ayarlayın bırakın:

![Sayfa denetçisi takma konumları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Sayfa denetçisi penceresi yuvadan, varsa, bunu Visual Studio dışında veya ikinci monitörde bile yerleştirebilirsiniz. Sayfa denetçisi penceresi kilitli olduğunda, ancak, ALT + SEKME sayfa denetçisi ve Visual Studio arasında sırada Git **Araçları** &gt; **seçenekleri** &gt;  **Ortam** &gt; **sekmeler ve pencereler**ve altında **iyi sekmesini**, Temizle onay kutusu olarak adlandırılan **kayan araç pencereleri her zaman kalın üstünde ana penceresi**:

![ALT + SEKME Visual Studio ile kilitli sayfa denetçisi penceresi arasında için kayan araç windows onay kutusunun işaretini kaldırın](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Sayfa denetçisi penceresinin üst bölmesi geçerli sayfa bir tarayıcı penceresinde gösterir. Alt bölme sayfanın sol HTML biçimlendirmesi gösterir ve olanak tanıyan sağdaki bazı sekmeleri sayfa farklı yönlerini inceleyin. Alt bölme benzer [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da. (Ancak, geliştirici araçları, Visual Studio içinde sağ sayfa denetçisi kullanabilirsiniz.)

![Sayfa Denetçisi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Bu öğreticide, sayfa denetçisi Tarayıcısı bölmesini kullanın ve **HTML** ve **stilleri** hızla yardımcı olmak için sekmeler gidin ve uygulamaya değişiklikleri yapın.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Denetleme modunu etkinleştir

Ardından, sayfa Denetçisi'nin Denetleme modu nasıl çalıştığını görürsünüz. Sayfa denetçisi penceresinde **incele** düğmesi.

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Eylem denetleme modunda görmek için page sayfa denetçisi tarayıcı penceresi içinde farklı kısımlarını üzerinden fareyi hareket ettirin. Yaptığınız gibi büyük bir artı işareti fare işaretçisini değiştirir ve öğesinin altında vurgulanır:

![Hovering over div.content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Fareyi hareket ederken unutmayın

- İçeriği **kaynak** görüntülemek sayfada seçilen öğe için karşılık gelen biçimlendirme göstermek için değişir. İlgili biçimlendirme vurgulanır. Kaynağı başka bir dosyaya ise, bu dosya kaynak görünümünde vurgulanmış ilgili biçimlendirme ile açılır.

- Görüntülenen biçimlendirme **HTML** sayfa denetçisi sekmesinde de değişiklikler sayfada seçilen öğe karşılık gelir. İçinde **HTML** sekmesinde ilgili biçimlendirme gösterilmiştir.

- **Stilleri** sekmesi CSS kuralları geçerli seçime uygun gösterir.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın

Şimdi Bul ve biçimlendirme veya metin konumu hemen göze görünmeyebilir değişiklik sayfa denetçisi nasıl kullanabileceğiniz görürsünüz.

Sayfa denetçisi denetleme moduna geçirin ve ardından giriş sayfasının en altına gidin.

Sayfa denetçisi altbilgi alanına girdiğiniz hemen açılır *Site.Master* düzeni dosyasında **kaynak** diğer sağındaki geçici bir sekme görünümünde sekmeler ve ana bölüm vurgular, sayfa seçtiniz. Bu, sayfa denetçisi nasıl bulabilir ve gerçekte başlangıçta açtığınız olandan farklı bir dosyadan gelebilir sayfasında içeriği görüntüle gösterir.

![Denetleme modunda altbilgi vurgular](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Sayfa denetçisi tarayıcı penceresinde fare telif hakkı satırıyla üzerinden işaretçinizi <a id="a"> </a>dikkat edin.

İçinde *Site.Master* sayfasında, karşılık gelen bir satır vurgulanmış.

![Vurgulanan altbilgi telif hakkı satır](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Satırın sonuna bazı metin eklemek *Site.Master* dosya.

&lt;p&gt;&amp;kopyalayın; &lt;%: DateTime.Now.Year %&gt; -My ASP.NET uygulama Rocks!&lt; /p&gt;

Şimdi, Ctrl + Alt + Enter tuşuna basın veya sayfa denetçisi tarayıcı penceresinde sonuçları görmek için güncelleştirme Çubuğu'nu tıklatın.

![My ASP.NET uygulama Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Altbilgi üzerinde olduğunu düşündüğünüz *Default.aspx* sayfa, ancak dönüştü asıl düzeni sayfasında olması ve sayfa denetçisi bulunamadı, sizin için.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Denetleme modu ve HTML penceresi

Ardından, HTML penceresi ve nasıl öğeleri sizin için eşleyen hızlı bir bakış sahip olur.

Sayfa denetçisi denetleme moduna alın.

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

"Buraya logonuz konacak" diyor sayfanın üst kısmında'ı tıklatın. Belirli bir tarayıcı penceresinde görünen artık fare işaretçisini taşımak gibi değişiklikler daha fazla ayrıntı öğesinde İncelemekte olduğunuz.

Şimdi fare işaretçisini taşıma **HTML** penceresi. Fare işaretçisini ilerlerken, öğe içinde sayfa denetçisi özetlenmektedir **HTML** penceresi ve tarayıcı penceresini karşılık gelen öğe vurgular.

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Önce sayfa denetçisi açar gibi *Site.Master* dosyayı geçici bir sekmede. Site.Master sekmesine tıklayın ve karşılık gelen biçimlendirme vurgulanan &lt;üstbilgi&gt; bölümü:

![Vurgulanan biçimlendirme](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Stilleri penceresinde CSS Değişiklikleri Önizle

Ardından, sayfa denetçisi nasıl kullanabileceğiniz görürsünüz **stilleri** CSS değişiklikleri önizleme penceresi.

Tıklatın **incele** sayfa denetçisi denetleme moduna düğmesi.

Sayfa denetçisi tarayıcı penceresinde fare "Giriş sayfası" bölümüne kadar işaretçiyi üzerine **div.content sarmalayıcı** etiketi görüntülenir.

![Öğeleri vurgulama](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Bir kez div.content sarmalayıcı bölüm içinde tıklayın ve fare işaretçisini taşıma **stilleri** penceresi. .Featured .content sarmalayıcı sınıf seçici altında temizleyin ve arka plan rengi özelliği için onay kutusunu seçin.

![Clear arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.

Onay kutusunu yeniden seçin, ardından özellik değerini çift tıklatın ve şekilde değiştirin `red`. Değişikliği hemen gösterir:

![Kırmızı arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Stilleri** kolay test ve CSS Önizleme değiştiğinde stiline değişiklikleri kaydetmeden önce penceresi yapar kendisini sayfa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS otomatik eşitleme

> [!NOTE]
> Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.


CSS otomatik eşitleme özelliği, bir CSS dosyasını doğrudan düzenlemeniz ve sayfa denetçisi tarayıcıda hemen değişiklikleri görmek sağlar.

Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümüne götürün **div.content sarmalayıcı** etiketi görüntülenir. Bu öğe seçmek için bir kez tıklayın.

**Syles** penceresi tüm bu öğe için CSS kuralları gösterir. Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın. ".Featured .content-sarmalayıcı üzerinde"'i tıklatın. Sayfa denetçisi bu stili (Site.css) tanımlar ve karşılık gelen CSS stil vurgular CSS dosyasını açar.

![CSS dosyası](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Şimdi değerini değiştirin `background-color` "red" için. Değişikliği hemen sayfa denetçisi tarayıcısında görüntülenir.

![Sayfa denetçisi tarayıcı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>CSS Renk Seçici kullanma

Ardından, sayfa denetçisi hızla bulmak ve varsayılan uygulama vurgulanan metinde CSS değiştirmek için nasıl kullanılacağını öğreneceksiniz. Bu örnekte, yoksa mavi vurgulama ister ve başka bir rengini değiştirmek istediğiniz olduğunu karar verdiniz.

Tıklatın **incele** düğmesi.

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Sayfa denetçisi tarayıcı penceresinde fare işaretçisini vurgulanan hareket "videoları, eğitim ve örnek" metin etiketi CSS "işaretlemek" görüntülenir.

![İşareti öğenin üzerine gelerek veya onları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Metni seçmek için tıklatın. Karşılık gelen CSS işareti Seçici en altında görüntülenen **stilleri** penceresi.

![Stilleri penceresinde işareti Seçici](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

İşareti seçicisini tıklatın. Bu açılır *Site.css* web uygulaması için dosya. Site.css sekmesini tıklatın ve karşılık gelen CSS seçicisinin vurgulanır:

![Stil sayfası seçicide işaretle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Seçin ve arka plan rengi özelliği içeren satırı Kaldır.

Şimdi yeni Visual Studio 2012 CSS Renk Seçici için yeni bir renk seçmek için kullanacağınız **işaretlemek** arka plan rengi özelliği.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Visual Studio 2012 CSS Renk Seçici kullanma

Visual Studio 2012'de CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici sahiptir. Basit bir renk çubuğu ve daha hassas denetim sunar bir "aşağı pop" Seçici vardır.

Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, RGB, RGBA, HSL ve HSLA renkleri destekler ve belgede en yakın zamanda kullandığınız renkleri listesini tutar.

Satırındaki arka plan rengi özelliği olduğu "bc" yazın ve bir kez aşağı ok tuşlarına basın.

"Background-color" gibi bir tire ayrılmış özelliğinde her sözcüğün ilk karakteri yazdığınızda, IntelliSense eşleşen özelliklerini göstermek için listesini filtreler:

![IntelliSense filtre değerleri](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Şimdi iki nokta yazın. Bunu yaptığınızda, tam arka plan rengi özellik adı eklenir. Tür  **#**  veya **rgb (**, ve Renk Seçici çubuğu görüntülenir:

![CSS Renk Seçici çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Renk Seçici çubuğu nasıl çalıştığını görmek için fare işaretçisini renklerle tıklayın ya da aşağı ok tuşuna basın ve renkleri geçiş yapmak için sol ve sağ ok tuşlarını kullanın. Bir renk ziyaret ettiğinizde, arka plan rengi özelliği için karşılık gelen değer önizlemesi:

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

Bu noktada, değer ve CSS giriş tamamlamak için noktalı virgül (;) seçmek için Enter tuşuna basın. Renk Seçici pop aşağı nasıl çalıştığını görebilmeniz için şu an için sonraki bölüme geçin.

#### <a name="using-the-color-picker-pop-down"></a>Renk Seçici Pop aşağı kullanma

Renk çubuğu aradığınız tam renk atanmamışsa, Renk Seçici pop aşağı kullanabilirsiniz.

Açmak için renk çubuğunun sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşlarına basın.

![CSS Renk Seçici Pop aşağı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Sağdaki dikey çubuk renkten'ı tıklatın. Bu, ana pencerede, renk için gradyan gösterir. Enter tuşuna basarak doğrudan dikey çubuğu'ndan bir renk seçin veya ile daha iyi kesinlik seçmek için ana penceresinde herhangi bir noktasını tıklatın.

Kullanmak istediğiniz bilgisayar ekranınızda bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olmak zorunda değildir), alt sağ tarafta Damlalık aracını kullanarak değerini yakalayabilirsiniz.

Renk Seçici altındaki kaydırıcısını hareket ettirerek bir rengin geçirgenliğini de değiştirebilirsiniz. RGBA biçimi opaklık gösterebilir çünkü değişiklikleri RGBA değerleri renk yapılıyor.

Bir renk seçtikten sonra Enter tuşuna basın ve arka plan rengi girişi tamamlamak için noktalı virgül yazın *Site.css* dosya.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Sayfa denetçisi güncelleştirme çubuğu

Sayfa denetçisi hemen değişikliği algılar *Site.css* dosya (veya uygulamadaki herhangi bir dosyaya) ve bir güncelleştirme çubuğunda bir uyarı görüntüler.

![Güncelleştirme çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Tüm dosyaları kaydetmek ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşuna basın veya güncelleştirme Çubuğu'nu tıklatın. Vurgulama renk değişikliği tarayıcıda görünür:

![Değiştirilen vurgulama renk](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Sayfa denetçisi tarayıcıdan sağ Visual Studio ortamında rahat yenilenir dikkat edin. Sayfa denetçisi yerine dış tarayıcı kullanarak, web Uygulamalarınızı geliştirirken Düzenleyici'de Kal olanak sağlar.

## <a name="see-also"></a>Ayrıca Bkz.

[Sayfa denetçisi Tanıtımı](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)

[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)
