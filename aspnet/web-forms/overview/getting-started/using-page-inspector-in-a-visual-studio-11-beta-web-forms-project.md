---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Visual Studio 2012'de ASP.NET Web Forms için sayfa denetçisini kullanma | Microsoft Docs
author: rick-anderson
description: Page Inspector, Visual Studio 2012 için tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa denetçisi herhangi bir öğe seçin...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756599"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>ASP.NET Web Forms'da Visual Studio 2012 için sayfa denetçisini kullanma
====================
Tim Ammann tarafından

> Page Inspector, Visual Studio 2012 için tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular. Uygulamanızda herhangi bir sayfasında Gözat, hızlı bir şekilde biçimlendirmenin kaynaklarını bulabilir ve Visual Studio ortamının içinden tarayıcı araçları kullanın.
> 
> Bu öğretici shwos nasıl İnceleme modu etkinleştirin ve ardından hızla bulup CSS kurallarını ve web projeniz içindeki metni düzenleyin. Web Forms uygulaması projesi öğretici kullanır, ancak sayfa denetçisi Web sitesi projeleri için de kullanabilirsiniz ve [MVC](https://go.microsoft.com/?linkid=9802002) uygulamalar.
> 
> Öğretici aşağıdaki bölümleri içerir:
> 
> [Önkoşulları](#_1_prerequisites)
> 
> [Bir Web uygulaması oluşturun](#_2_creating_a)
> 
> [Uygulamayı görüntülemek için sayfa denetçisini kullanma](#_3_using_page)
> 
> [İnceleme modu etkinleştir](#_4_inspection_mode)
> 
> [İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma](#_5_using_page)
> 
> [İnceleme modu ve HTML penceresi](#_6_inspection_mode)
> 
> [Stilleri penceresinde CSS Değişiklikleri Önizle](#_7_previewing_css)
> 
> [CSS otomatik eşitleme](#css_auto_sync)
> 
> [CSS renk seçiciyi kullanarak](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Sayfa Denetçisi'nın en son sürümünü almak için kullanın [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Azure SDK'sını yüklemek için.


Page Inspector, Microsoft Web geliştirici araçları ile birlikte gelir. En son 1.3 sürümüdür. Hangi sürümünü denetlemek için sahip, Visual Studio'yu çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Bir Web uygulaması oluşturun

İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturacaksınız. Visual Studio'da **dosya** &gt; **yeni proje**. Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET Web Forms uygulaması**.

![Yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

**Tamam**'ı tıklatın.

Uygulama açılır **kaynak** görünümü.

![Kaynak Görünümü'nde yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Çalışmak için bir uygulamanız olduğuna göre incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Uygulamayı görüntülemek için sayfa denetçisini kullanma

Ardından, uygulama sayfa denetçisi ile görebilir. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **sayfa denetçisi görünümünde**.

![Sayfa denetçisi görünümünde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Varsayılan olarak, bu sayfa denetçisi ilk kez başlattığında dar bir pencere olarak Visual Studio ortamını sol tarafında yerleştirilir. Sol taraftaki yerleştirilmiş ve sizin için rahatça ya da aracı alanlarından içinde üst, alt veya sağa Yerleştir genişliği ayarlayın bırakın:

![Sayfa denetçisi yerleştirme konumları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Sayfa denetçisi penceresi çıkar, varsa, bunu Visual Studio'nun dışında ya da ikinci monitörde bile yerleştirebilirsiniz. Sayfa denetçisi penceresi yerleştirilmemiş olduğunda, ancak ALT + SEKME sayfa denetçisi ve Visual Studio arasında sırada Git **Araçları** &gt; **seçenekleri** &gt;  **Ortam** &gt; **sekmeler ve Windows**, altında **sekmesinde de**adlı onay kutusunu temizleyin **kayan araç pencereleri her zaman kalın üst kısmındaki Ana pencere**:

![Visual Studio ile yerleştirilmemiş sayfa denetçisi penceresi arasında ALT + SEKME için kayan aracı windows onay kutusunu temizleyin](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Sayfa denetçisi pencerenin en üst bölmesi, geçerli sayfada bir tarayıcı penceresinde gösterilir. Alt bölme sayfası soldaki HTML biçimlendirmeyi gösterir ve bazı sekmeler olanak tanıyan sağdaki sayfa farklı yönlerini inceleyin. Alt bölme benzer [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da. (Ancak, geliştirici araçları, Page Inspector, Visual Studio içinden kullanabilirsiniz.)

![Sayfa Denetçisi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Bu öğreticide, sayfa denetçisi tarayıcı bölmesinde kullanır ve **HTML** ve **stilleri** hızlı bir şekilde yardımcı olması için sekmeler gidin ve uygulamada değişiklik yapın.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>İnceleme modu etkinleştir

Ardından, sayfa Denetçisi'nin İnceleme modu nasıl çalıştığını görürsünüz. Sayfa denetçisi pencerede **inceleyin** düğmesi.

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

İnceleme modu iş başında görmek için Page Inspector tarayıcı penceresi içinde sayfasının farklı bölümlerini üzerinde fareyi hareket ettirin. Yaptığınız gibi fare işaretçisini büyük artı işaretine değişir ve öğesinin altında vurgulanır:

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Fare işaretçisi hareket ettikçe unutmayın

- İçeriği **kaynak** görüntüleme sayfasında seçilen öğenin karşılık gelen biçimlendirmesini gösterecek şekilde değişir. İlgili biçimlendirme vurgulanır. Kaynak başka bir dosyaya ise, bu dosyayı vurgulanmış ilgili biçimlendirme Kaynak Görünümü'nde açılır.

- Görüntülenen biçimlendirme **HTML** sayfa denetçisi sekmede sayfasında seçilen öğeye karşılık gelecek şekilde de değişir. İçinde **HTML** sekmesinde ilgili biçimlendirme ana hatlarıyla açıklanmıştır.

- **Stilleri** sekmesi CSS kurallarını geçerli seçime uygun gösterir.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma

Şimdi bulup biçimlendirme veya metin konumu hemen göze görünmeyebilir değişiklik sayfa denetçisi nasıl kullanabileceğinizi göreceksiniz.

Sayfa denetçisi İnceleme moduna alın ve ardından giriş sayfasının en alt kısma.

Sayfa denetçisi altbilgi alanı girilmez açılır *Site.Master* Düzen dosyasında **kaynak** diğer sağındaki geçici bir sekme görünümünde sekmeler ve ana bölümünü vurgular, sayfa seçtiniz. Bu, sayfa denetçisi bulabilir ve gerçekte farklı bir dosya, özgün olarak açtığınız bir nereden geldiği bir sayfada içeriklerin nasıl gösterir.

![Denetleme modunda altbilgi vurgular](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Sayfa denetçisi tarayıcı penceresinde fare işaretçinizi telif hakkı satırla üzerine getirin <a id="a"> </a>dikkat edin.

İçinde *Site.Master* sayfasında, karşılık gelen satırı vurgulanır.

![Vurgulanan alt telif hakkı satır](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Satır sonuna metin eklemek *Site.Master* dosya.

&lt;p&gt;&amp;kopyalayın; &lt;%: DateTime.Now.Year %&gt; -My ASP.NET Application Rocks!&lt; /p&gt;

Şimdi, Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğundaki sonuçları sayfa denetçisi tarayıcı penceresinde görmek için tıklayın.

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Altbilgi üzerinde olduğunu düşündüğünüz *Default.aspx* sayfa, ancak bu durumun gerçekleşmediği ana düzen sayfası içinde olmasını ve sayfa denetçisi bulunamadı, sizin için.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>İnceleme modu ve HTML penceresi

Ardından, HTML penceresi ve eşlemelerini nasıl yapar? bu öğeler, hızlı bir bakış sahip olur.

Sayfa denetçisi İnceleme moduna alın.

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

"Buraya logonuz" ifadesini içeren üst kısmında tıklayın. Fare işaretçisini getirdiğinizde tarayıcı penceresinde görüntülenmesini artık değişiklikler daha ayrıntılı olarak belirli bir öğeyle İncelemekte olduğunuz.

Artık fare işaretçisi hareket **HTML** penceresi. Fare işaretçisi hareket ettikçe öğe içinde sayfa denetçisi özetler **HTML** penceresini ve karşılık gelen öğe tarayıcı penceresinde vurgular.

![HTML penceresi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Sayfa denetçisi önce açan *Site.Master* dosyayı geçici bir sekmede. Site.Master sekmesine tıklayın ve karşılık gelen biçimlendirme vurgulanan &lt;üstbilgi&gt; bölümü:

![Vurgulanan biçimlendirme](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Stilleri penceresinde CSS Değişiklikleri Önizle

Ardından, sayfa denetçisi nasıl kullanabileceğinizi görürsünüz **stilleri** CSS değişiklikleri Önizleme için pencere.

Tıklayın **inceleyin** düğmesine sayfa denetçisi İnceleme moduna alın.

Sayfa denetçisi tarayıcı penceresinde, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.

![Öğelerin üzerine geldiğinizde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Bir kez div.content sarmalayıcı bölümündeki tıklayın ve sonra fare işaretçisi hareket **stilleri** penceresi. .Featured .content sarmalayıcı sınıfı Seçicisi altında temizleyin ve arka plan rengi özelliği için onay kutusunu işaretleyin.

![Açık Arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.

Yeniden onay kutusunu işaretleyin, sonra özellik değerini çift tıklatın ve değiştirmek için `red`. Değişikliği hemen gösterir:

![Kırmızı arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Stilleri** kendisini sayfa kolayca test edin ve CSS Önizleme değiştirirse stil ve değişiklikleri göndermeden önce penceresi yapar.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS otomatik eşitleme

> [!NOTE]
> Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.


CSS otomatik eşitleme özelliği, bir CSS dosyası doğrudan düzenlemek ve hemen sayfa denetçisi tarayıcıda değişiklikleri görmek sağlar.

Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür. Bu öğe seçmek için bir kez tıklayın.

**Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir. Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın. ".Featured .content-sarmalayıcı üzerinde"'a tıklayın. Sayfa denetçisi bu stil (Site.css) tanımlayan ve karşılık gelen CSS stil vurgular CSS dosyası açılır.

![CSS dosyası](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Artık değerini değiştirin `background-color` "kırmızı". Değişikliği hemen sayfa denetçisi tarayıcıda görüntülenir.

![Sayfa denetçisi tarayıcı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>CSS renk seçiciyi kullanarak

Ardından, sayfa denetçisi hızla bulup varsayılan uygulamada vurgulanan metni için CSS değiştirmek için nasıl kullanılacağını öğreneceksiniz. Bu örnekte, yoksa mavi Vurgu ister ve bunu başka bir renge değiştirmek istiyorsanız, karar verdiniz.

Tıklayın **inceleyin** düğmesi.

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Sayfa denetçisi tarayıcı penceresinde fare işaretçisini vurgulanan hareket "videolar, öğreticilerimiz ve örneklerimizle" metin CSS işaretle"etiket" görüntülenir.

![İşareti öğenin vurgulama](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Metni seçmek için tıklatın. Karşılık gelen CSS işareti Seçici alt kısmında görünür **stilleri** penceresi.

![Stilleri penceresinde işareti Seçici](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

İşareti Seçici'yi tıklatın. Bu açılır *Site.css* web uygulaması için dosya. Site.css sekmesine tıklayın ve ilgili CSS Seçici için vurgulanır:

![Stil sayfası seçicide işaretle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Seçin ve arka plan rengi özelliğiyle satırını kaldırır.

İçin yeni bir renk seçmek için artık yeni Visual Studio 2012 CSS renk seçicinin kullanacağı **işaretlemek** arka plan rengi özelliği.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Visual Studio 2012 CSS Renk Seçici'yi kullanma

Visual Studio 2012 CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici var. Bu, basit bir renk çubuğu ve daha hassas bir denetim sunan bir "aşağı pop" Seçici vardır.

Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, renklerin RGB, RGBA HSL ve HSLA destekler ve en son belge içinde kullandığınız renkleri listesini tutar.

Satırındaki arka plan rengi özelliği olduğu "bc" yazın ve sonra aşağı oka basın.

Çizgi ile ayrılmış bir özellik "background-color" gibi her sözcüğün ilk karakter yazdığınızda, IntelliSense, eşleşen özelliklerini göstermek listeyi filtreler:

![IntelliSense filtre değerleri](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Şimdi bir iki nokta üst üste yazın. Bunu yaptığınızda tam arka plan rengi özellik adı eklenir. Tür **#** veya **rgb (**, ve Renk Seçici çubuğu görünür:

![CSS Renk Seçici çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Renk Seçici çubuğu nasıl çalıştığını görmek için fare işaretçisi ile renklerini tıklayın veya aşağı ok tuşuna basın ve ardından renkleri geçirmek için sol ve sağ ok tuşlarını kullanın. Bir renk ziyaret ettiğinizde, arka plan rengi özelliği için karşılık gelen değer önizlemesini görebilirsiniz:

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

Bu noktada, değer ve CSS giriş tamamlamak için noktalı virgül (;) seçmek için Enter tuşuna basın. Renk Seçici pop aşağı nasıl çalıştığını görebilmeniz için şu an için sonraki bölüme geçin.

#### <a name="using-the-color-picker-pop-down"></a>Renk Seçici Pop aşağı kullanma

Renk çubuğu aradığınız tam renk sahip olmadığında, Renk Seçici pop aşağı kullanabilirsiniz.

Açmak için renk çubuğu sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşuna basın.

![CSS Renk Seçici Pop-aşağı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Sağdaki dikey çubuk renk tıklayın. Bu, renk için bir gradyan ana penceresinde gösterir. Enter tuşuna basarak doğrudan dikey çubuğundan bir renk seçin veya ile daha fazla duyarlık seçmek için ana penceresinde herhangi bir noktasına tıklayın.

Bilgisayar ekranınızda kullanmak istediğiniz bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olması gerekmez), değeri alt sağ tarafta renk damlalığı aracı kullanarak yakalayabilirsiniz.

Renk Seçici altındaki kaydırıcıyı hareket ettirerek bir rengin geçirgenliğini da değiştirebilirsiniz. RGBA biçimi opaklık temsil edebilir çünkü değişiklikler RGBA değerleri renk yapılıyor.

Bir renk seçtikten sonra Enter tuşuna basın ve ardından arka plan rengi girişte tamamlamak için noktalı virgül ekleyin *Site.css* dosya.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Sayfa denetçisi güncelleştirme çubuğu

Page Inspector, değişikliği hemen algılar *Site.css* dosya (veya uygulamadaki herhangi bir dosyaya) ve bir güncelleştirme çubuğunda bir uyarı görüntüler.

![Güncelleştirme çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Tüm dosyaları kaydedin ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğuna tıklayın. Vurgu rengi değişiklik tarayıcıda görüntülenir:

![Vurgu rengi değişir](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Sayfa denetçisi tarayıcı doğrudan Visual Studio ortamında kolayca yenilenir dikkat edin. Sayfa denetçisi yerine dış tarayıcı kullanmanız, web Uygulamalarınızı geliştirirken Düzenleyicisi'nde kalın olanak tanır.

## <a name="see-also"></a>Ayrıca Bkz.

[Sayfa denetçisi ile tanışın](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (kanal 9 videosu)

[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)
