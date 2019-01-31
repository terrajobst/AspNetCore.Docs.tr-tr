---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: ASP.NET MVC'de sayfa denetçisini kullanma | Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa Denetçisi'i herhangi bir öğe seçin...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 0dea8b077878139a3f513cb51447b86a93fe55b8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236542"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>ASP.NET MVC'de Sayfa Denetçisini Kullanma
====================
Tim Ammann tarafından

> Visual Studio 2012 sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular. Herhangi bir MVC görünümü göz atabilir, hızlı bir şekilde biçimlendirmenin kaynaklarını bulun ve Visual Studio ortamının içinden tarayıcı araçları kullanın.
> 
> [Videoyu izleyin](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Bu öğreticide, İnceleme modu etkinleştirin ve ardından hızla bulup işaretleme ve CSS web projeniz Düzenle gösterilir. Bir MVC projesi öğretici kullanır, ancak sayfa denetçisi için de kullanabilirsiniz [Web Forms](https://go.microsoft.com/?linkid=9802001) ve diğer ASP.NET uygulamalarını.
> 
> Öğretici aşağıdaki bölümleri içerir:
> 
> - [Önkoşullar](#_1_prerequisites)
> - [Bir Web uygulaması oluşturun](#_2_creating_a)
> - [Görünüme Gözat sayfa denetçisini kullanma](#_3_using_page)
> - [İnceleme modu etkinleştir](#_4_inspection_mode)
> - [İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma](#_5_using_page)
> - [İnceleme modu ve HTML penceresi](#_6_inspection_mode)
> - [Stilleri penceresinde CSS Değişiklikleri Önizle](#_7_previewing_css)
> - [CSS otomatik eşitleme](#css_auto_sync)
> - [CSS renk seçiciyi kullanarak](#css_color_picker)
> - [Dinamik sayfa öğeleri için JavaScript eşleme](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Sayfa Denetçisi'nın en son sürümünü almak için kullanın [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Windows Azure SDK'sını yüklemek için.


Page Inspector, Microsoft Web geliştirici araçları ile birlikte gelir. En son 1.3 sürümüdür. Hangi sürümünü denetlemek için sahip, Visual Studio'yu çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Bir Web uygulaması oluşturun

İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturun. Visual Studio'da **dosya** &gt; **yeni proje**. Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.

![Yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image2.png)

**Tamam**'ı tıklatın.

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması**. Bırakın **Razor** varsayılan görünüm altyapısı olarak.

![Yeni ASP.NET MVC projesi - Internet uygulaması](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Uygulama açılır **kaynak** görünümü.

![Kaynak Görünümü'nde yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Çalışmak için bir uygulamanız olduğuna göre incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Görünüme Gözat sayfa denetçisini kullanma

Visual Studio 2012'de, projenizde select herhangi bir görünümü sağ tıklayabilirsiniz **sayfa denetçisi görünümünde**, sayfa denetçisi yol şekil ve sayfayı görüntüleme.

İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü ve ardından **giriş** klasör. Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

![Sayfa Denetçisi'nde Index.cshtml görüntüleyin](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Varsayılan olarak, Page Inspector, Visual Studio ortamını sol tarafında bir pencere olarak sabitlenmiştir. Tercih ederseniz, başka bir yerde sabitlemek veya pencere çıkar. Bkz: [nasıl yapılır: Pencereleri düzenleme ve yerleştirme Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Sayfa denetçisi pencerenin en üst bölmesi, geçerli sayfada bir tarayıcı penceresinde gösterilir. Alt bölme sayfası, sayfayı farklı yönlerini incelemenize olanak bazı sekmeler yanı sıra HTML biçimlendirmesi olarak gösterir. Alt bölme benzer [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.

![ASP.NET MVC uygulamasındaki sayfa denetçisi](using-page-inspector-in-aspnet-mvc/_static/image10.png)

Bu öğreticide kullanacağınız **HTML** ve **stilleri** uygulamada değişiklik yapın ve hızlıca gezinmek için sekmeleri.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection modu

Sayfa denetçisi İnceleme moduna almanız tıklayın **inceleyin** düğmesi. İşlenen sayfanın herhangi bir bölümü fare işaretçisini tuttuğunuzda denetleme modunda, karşılık gelen kaynak biçimlendirmeyi veya kodu vurgulanır.

![İnceleme modunu Aç/Kapat](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Artık sayfa içinde sayfa denetçisi farklı bölümleri üzerinde fareyi hareket ettirin. Yaptığınız gibi fare işaretçisini büyük artı işaretine değişir ve öğesinin altında vurgulanır:

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Fare işaretçisi hareket ettikçe Visual Studio kaynak dosyasında karşılık gelen Razor söz dizimi vurgulanır. HTML öğesi, başka bir kaynak dosyasından geliyorsa, Visual Studio dosyayı otomatik olarak açılır.

Sayfa denetçisi içinde **HTML** Razor sözdiziminden, oluşturulan HTML sekmesi gösterir. Fare işaretçisini getirdiğinizde, HTML öğeleri vurgulanır. **Stilleri** sekme öğesi için CSS kurallarını gösterir.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma

Sayfa denetçisi konumu belirgin olmayabilecek biçimlendirme bulmanıza olanak tanır. Ardından biçimlendirme değiştirebilir ve sonuçta elde edilen değişiklikleri görebilirsiniz.

Bunu görmek için tıklayın **inceleyin** ve sayfa denetçisi penceresinde sayfanın alt kısmına kaydırın.

Alt bilgi alanına fare işaretçisini getirdiğinizde, sayfa denetçisi açılır \_Layout.cshtml dosya ve seçmiş olduğunuz Düzen sayfasının bölümünde vurgular. Gördüğünüz gibi alt değiştirilebilir Düzen dosyası ve görünüm kendisi içinde tanımlanır.

![Alt bilgi](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Telif Hakkı satırla fare işaretçinizi artık üzerine getirin <a id="a"> </a>dikkat edin. İçinde \_Layout.cshtml sayfasında, karşılık gelen satırı vurgulanır.

![Vurgulanan alt telif hakkı satır](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Satır sonuna metin eklemek \_Layout.cshtml dosya.

&lt;p&gt;&amp;kopyalayın; @DateTime.Now.Year -ASP.NET MVC Uygulamam Rocks! &lt;/p&gt;

Şimdi, Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğundaki sonuçları sayfa denetçisi tarayıcı penceresinde görmek için tıklayın.

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Altbilgi Index.cshtml içinde tanımlı, ancak olması dönüştü düşündüğünüz \_Layout.cshtml ve sayfa Denetçisi'nın sizin için bulunamadı.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>İnceleme modu ve HTML penceresi

Ardından, HTML penceresi ve eşlemelerini nasıl yapar? bu öğeler, hızlı bir bakış sahip olur.

Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.

Üst kısmında "logohere" ifadesini içeren tıklayın. Fare işaretçisini getirdiğinizde tarayıcı penceresinde görüntülenmesini artık değişiklikler daha ayrıntılı olarak belirli bir öğeyle İncelemekte olduğunuz.

Artık fare işaretçisi hareket **HTML** penceresi. Fare işaretçisi hareket ettikçe öğe içinde sayfa denetçisi özetler **HTML** penceresini ve karşılık gelen öğe tarayıcı penceresinde vurgular.

![HTML penceresi](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Sayfa denetçisi önce açan \_Layout.cshtml dosyayı geçici bir sekmede. Tıklayın \_Layout.cshtml geçici sekmesinde ve karşılık gelen biçimlendirme vurgulanmış &lt;üstbilgi&gt; , bölüm:

![Vurgulanan biçimlendirme](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Stilleri penceresinde CSS Değişiklikleri Önizle

Ardından, sayfa denetçisi kullanacağınız **stilleri** CSS değişiklikleri Önizleme için pencere.

Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcı penceresinde, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Bir kez div.content sarmalayıcı bölümündeki tıklayın ve sonra fare işaretçisi hareket **stilleri** penceresi. **Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir. Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın. Artık arka plan rengi özelliği için onay kutusunu temizleyin.

![Açık Arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.

Yeniden onay kutusunu işaretleyin, sonra özellik değerini çift tıklatın ve kırmızı olarak değiştirin. Değişikliği hemen gösterir:

![Kırmızı arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**Stilleri** kendisini sayfa kolayca test edin ve CSS Önizleme değiştirirse stil ve değişiklikleri göndermeden önce penceresi yapar.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS Auto Sync

> [!NOTE]
> Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.


CSS otomatik eşitleme özelliği, bir CSS dosyası doğrudan düzenlemek ve hemen sayfa denetçisi tarayıcıda değişiklikleri görmek sağlar.

Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür. Bu öğe seçmek için bir kez tıklayın.

**Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir. Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın. ".Featured .content-sarmalayıcı üzerinde"'a tıklayın. Sayfa denetçisi bu stil (Site.css) tanımlayan ve karşılık gelen CSS stil vurgular CSS dosyası açılır.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Artık değerini değiştirin `background-color` "kırmızı". Değişikliği hemen sayfa denetçisi tarayıcıda görüntülenir.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>CSS renk seçiciyi kullanarak

Visual Studio 2012 CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici var. Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, renklerin RGB, RGBA HSL ve HSLA destekler ve en son belge içinde kullandığınız renkleri listesini tutar.

Değerini önceki bölümde değiştirdiğiniz `background-color` özelliği. Renk Seçici çağırmak için ekleme noktasını türü ve özellik adını sonra yerleştirin **#** veya **rgb (**.

![CSS Renk Seçici çubuğu](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Bir rengi seçin ya da aşağı ok tuşuna tıklayın ve ardından renkleri geçirmek için sol ve sağ ok tuşlarını kullanın. Bir renk ziyaret ettiğinizde, karşılık gelen bir onaltılık değer önizlemesini görebilirsiniz:

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Renk çubuğu tam rengi yoksa, Renk Seçici pop aşağı kullanabilirsiniz. Açmak için renk çubuğu sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşuna basın.

![CSS Renk Seçici Pop-aşağı](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Sağdaki dikey çubuk renk tıklayın. Bu, renk için bir gradyan ana penceresinde gösterir. Enter tuşuna basarak doğrudan dikey çubuğundan bir renk seçin veya ile daha fazla duyarlık seçmek için ana penceresinde herhangi bir noktasına tıklayın.

Bilgisayar ekranınızda kullanmak istediğiniz bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olması gerekmez), değeri alt sağ tarafta renk damlalığı aracı kullanarak yakalayabilirsiniz.

Renk Seçici altındaki kaydırıcıyı hareket ettirerek bir rengin geçirgenliğini da değiştirebilirsiniz. Değişiklikleri renk değerleri RGBA, RGBA biçimi opaklık temsil edebilir çünkü yapılıyor.

Bir renk seçtikten sonra Enter tuşuna basın ve ardından arka plan rengi girişte tamamlamak için noktalı virgül ekleyin *Site.css* dosya.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Sayfa denetçisi güncelleştirme çubuğu

Page Inspector, değişikliği hemen algılar *Site.css* dosyası ve bir güncelleştirme çubuğunda bir uyarı görüntüler.

![Güncelleştirme çubuğu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Tüm dosyaları kaydedin ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğuna tıklayın. Vurgu rengi değişiklik tarayıcıda görüntülenir.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Dinamik sayfa öğeleri için JavaScript eşleme

Modern web uygulamaları, sayfadaki öğeleri genellikle dinamik olarak JavaScript ile oluşturulur. Bu sayfa öğeleri için karşılık gelen statik biçimlendirme yok (HTML veya Razor) anlamına gelir.

Sürüm 1.3 sayfa denetçisi sayfasına karşılık gelen JavaScript kodunu dinamik olarak eklenen öğeleri artık eşleyebilirsiniz. Bu özellik göstermek için kullanacağız [tek sayfa uygulama (SPA) şablonu](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> SPA şablon için gerekli [ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) güncelleştirin.


Visual Studio'da **dosya** &gt; **yeni proje**. Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**. **Tamam**'ı tıklatın.

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **tek sayfalı uygulama**.

Çözüm Gezgini'nde **görünümleri** klasörünü ve ardından **giriş** klasör. Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

Sayfa denetçisi tarayıcıda görüntülenen olan ilk şey bir oturum açma sayfası olduğu. "Kaydol" tıklayın ve bir kullanıcı adı ve parola oluşturun. Kaydolduktan sonra uygulama oturum açması ve bazı örnek öğeleri ile bir Yapılacaklar listesi oluşturur.

Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek. Sayfa denetçisi tarayıcıda Yapılacaklar öğelerini birine tıklayın. Mavi renkle vurgulanmış yerine, öğe "JS" ile bir turuncu öğesi adının yanındaki vurgulandığından emin olun. Bu öğe, komut dosyası aracılığıyla dinamik olarak oluşturulduğunu gösterir.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Ayrıca, üzerinde turuncu bir çizgi görünür **çağrı yığını** sekmesi. Gösterir **çağrı yığını** bölmesi olan öğeyle ilgili daha fazla bilgi.

Tıklayarak **çağrı yığını** sekmesi. **Çağrı yığını** bölmesi öğesi oluşturulan JavaScript çağrısı için çağrı yığınını gösterir. JQuery daraltılmış gibi uygulama kodunuzu çağrıları kolayca görmenize olanak tanıyan dış kitaplıklara çağırır.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Dış kitaplıklarında, çağrılar dahil olmak üzere tam bir yığın görmek için "Dış kitaplıkları" etiketli düğümleri genişletebilirsiniz:

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Çağrı yığınındaki bir öğe tıklarsanız, Visual Studio kod dosyasını açar ve karşılık gelen betiği vurgular.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Ayrıca Bkz.

[ASP.NET MVC 4 ile Visual Studio giriş](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net Web sitesi)

[Sayfa denetçisi ile tanışın](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (kanal 9 videosu)

[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)
