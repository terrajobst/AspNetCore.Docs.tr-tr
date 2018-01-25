---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "ASP.NET MVC uygulamasında sayfa denetçisi kullanarak | Microsoft Docs"
author: rick-anderson
description: "Sayfa denetçisi Visual Studio 2012'de bir tümleşik bir tarayıcı ile web geliştirme aracıdır. Herhangi bir öğe tümleşik tarayıcı ve sayfa Denetçisi'i seçin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Sayfa denetçisi ASP.NET MVC uygulamasında kullanma
====================
tarafından Tim Ammann

> Sayfa denetçisi Visual Studio 2012'de bir tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular. Herhangi bir MVC görünümüne gidin, hızlı bir şekilde oluşturulan biçimlendirmenin kaynakları bulabilir ve Visual Studio ortamında sağ tarayıcı araçları kullanın.
> 
> [Videoyu izleme](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Bu öğretici, denetleme modunu etkinleştirin ve ardından hızla bulup biçimlendirme ve CSS web projesi içinde Düzenle gösterilmektedir. MVC projesinde öğretici kullanır, ancak sayfa denetçisi için de kullanabilirsiniz [Web Forms](https://go.microsoft.com/?linkid=9802001) ve diğer ASP.NET uygulamaları.
> 
> Öğretici aşağıdaki bölümleri içerir:
> 
> - [Önkoşulları](#_1_prerequisites)
> - [Bir Web uygulaması oluşturma](#_2_creating_a)
> - [Sayfa denetçisi bir görünüme Gözat kullanın](#_3_using_page)
> - [Denetleme modunu etkinleştir](#_4_inspection_mode)
> - [Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın](#_5_using_page)
> - [Denetleme modu ve HTML penceresi](#_6_inspection_mode)
> - [Stilleri penceresinde CSS Değişiklikleri Önizle](#_7_previewing_css)
> - [CSS otomatik eşitleme](#css_auto_sync)
> - [CSS Renk Seçici kullanma](#css_color_picker)
> - [JavaScript için eşleme dinamik sayfası öğeleri](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Sayfa Denetçisi'nın en son sürümünü almak için [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Windows Azure SDK'sını yüklemek için.


Sayfa denetçisi, Microsoft Web geliştirici araçları ile gelir. 1.3 en son sürümüdür. Hangi sürümü denetlemek için sahip, Visual Studio çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Bir Web uygulaması oluşturma

İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturun. Visual Studio'da, **dosya** &gt; **yeni proje**. Sol bölmede, genişletin **Visual C#**seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.

![Yeni ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image2.png)

**Tamam**'ı tıklatın.

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama**. Bırakın **Razor** varsayılan görünüm altyapısı olarak bulunabilir.

![Yeni ASP.NET MVC proje - Internet uygulama](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Uygulama açılır **kaynak** görünümü.

![Kaynak görünümünde yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Çalışmak için bir uygulamanız varsa, incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Sayfa denetçisi bir görünüme Gözat kullanın

Visual Studio 2012'de, projenizde select herhangi bir görünümü sağ tıklayarak **sayfa denetçisi görünümünde**, sayfa denetçisi rota şekil ve sayfa görüntüler.

İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü ve ardından **giriş** klasör. Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

![Sayfa Denetçisi'nde Index.cshtml görüntüleyin](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Varsayılan olarak, Page Inspector, Visual Studio ortamı sol tarafta bir pencere olarak sabitlenmiştir. Tercih ederseniz başka bir yerde yerleştirme veya pencere çıkar. Bkz: [nasıl yapılır: pencereleri düzenleme ve yerleştirme Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Sayfa denetçisi penceresinin üst bölmesi geçerli sayfa bir tarayıcı penceresinde gösterir. Alt bölme sayfa farklı yönlerini incelemek sağlayan bazı sekmeleri birlikte HTML biçimlendirmesi sayfası gösterilir. Alt bölme benzer [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.

![Sayfa denetçisi ASP.NET MVC uygulamasındaki](using-page-inspector-in-aspnet-mvc/_static/image10.png)

Bu öğreticide, kullanacağınız **HTML** ve **stilleri** hızla gidin ve uygulamaya değişiklikler yapmak için sekmeler.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection modu

Sayfa denetçisi denetleme moduna için tıklatın **incele** düğmesi. İşlenen sayfanın herhangi bir kısmını fare işaretçisini tuttuğunuzda denetleme modunda, karşılık gelen kaynak işaretleme veya kod vurgulanır.

![Denetleme moduna geç](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Şimdi farenizi sayfa içinde sayfa denetçisi farklı kısımlarını üzerine getirin. Yaptığınız gibi büyük bir artı işareti fare işaretçisini değiştirir ve öğesinin altında vurgulanır:

![Hovering over div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Fare işaretçisini ilerlerken, Visual Studio kaynak dosyasında karşılık gelen Razor sözdizimi vurgular. HTML öğesi başka bir kaynak dosyasından geliyorsa, Visual Studio dosya otomatik olarak açılır.

Sayfa Denetçisi'nde **HTML** sekmesi Razor sözdiziminden, oluşturulan HTML gösterir. Fare işaretçisini ilerlerken, HTML öğeleri vurgulanır. **Stilleri** sekme öğesi için CSS kuralları gösterir.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın

Page Inspector, konumu belirgin olmayabilir biçimlendirmesini bulmanıza olanak sağlar. Ardından işaretleme değiştirebilir ve ortaya çıkan değişiklikleri görebilirsiniz.

Bu görmek için tıklatın **incele** ve sayfa denetçisi penceresinde sayfanın altına gidin.

Fare işaretçisini altbilgi alanına taşıdığınızda, sayfa denetçisi açılır \_Layout.cshtml dosya ve seçtiğiniz Düzen sayfasının bölümünde vurgular. Gördüğünüz gibi alt değiştirilebilir düzeni dosyasını ve görünüm kendisini tanımlanır.

![Altbilgi](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Şimdi, fare işaretçisini telif hakkı satırıyla götürün <a id="a"> </a>dikkat edin. İçinde \_Layout.cshtml sayfasında, karşılık gelen bir satır vurgulanmış.

![Vurgulanan altbilgi telif hakkı satır](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Satırın sonuna bazı metin eklemek \_Layout.cshtml dosya.

&lt;p&gt;&amp;kopyalayın; @DateTime.Now.Year -ASP.NET MVC Uygulamam Rocks! &lt;/p&gt;

Şimdi, Ctrl + Alt + Enter tuşuna basın veya sayfa denetçisi tarayıcı penceresinde sonuçları görmek için güncelleştirme Çubuğu'nu tıklatın.

![My ASP.NET uygulama Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Altbilgi Index.cshtml içinde tanımlı, ancak olduğu dönüştü zorlayıcı \_Layout.cshtml ve sayfa Denetçisi'nın sizin için bulundu.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Denetleme modu ve HTML penceresi

Ardından, HTML penceresi ve nasıl öğeleri sizin için eşleyen hızlı bir bakış sahip olur.

Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.

"Logohere," diyor sayfanın üst kısmında'ı tıklatın. Belirli bir tarayıcı penceresinde görünen artık fare işaretçisini taşımak gibi değişiklikler daha fazla ayrıntı öğesinde İncelemekte olduğunuz.

Şimdi fare işaretçisini taşıma **HTML** penceresi. Fare işaretçisini ilerlerken, öğe içinde sayfa denetçisi özetlenmektedir **HTML** penceresi ve tarayıcı penceresini karşılık gelen öğe vurgular.

![HTML penceresi](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Önce sayfa denetçisi açar gibi \_Layout.cshtml dosyayı geçici bir sekmede. Tıklatın \_Layout.cshtml geçici sekmesi ve karşılık gelen biçimlendirme vurgulanmış içinde &lt;üstbilgi&gt; , bölüm:

![Vurgulanan biçimlendirme](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Stilleri penceresinde CSS Değişiklikleri Önizle

Ardından, sayfa denetçisi kullanacağı **stilleri** CSS değişiklikleri önizleme penceresi.

Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcı penceresinde fare "Giriş sayfası" bölümüne kadar işaretçiyi üzerine **div.content sarmalayıcı** etiketi görüntülenir.

![Hovering over div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Bir kez div.content sarmalayıcı bölüm içinde tıklayın ve fare işaretçisini taşıma **stilleri** penceresi. **Syles** penceresi tüm bu öğe için CSS kuralları gösterir. Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın. Şimdi arka plan rengi özelliği için onay kutusunu temizleyin.

![Clear arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.

Onay kutusunu yeniden seçin, ardından özellik değeri çift tıklatın ve kırmızı olarak değiştirin. Değişikliği hemen gösterir:

![Kırmızı arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**Stilleri** kolay test ve CSS Önizleme değiştiğinde stiline değişiklikleri kaydetmeden önce penceresi yapar kendisini sayfa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS otomatik eşitleme

> [!NOTE]
> Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.


CSS otomatik eşitleme özelliği, bir CSS dosyasını doğrudan düzenlemeniz ve sayfa denetçisi tarayıcıda hemen değişiklikleri görmek sağlar.

Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.

Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümüne götürün **div.content sarmalayıcı** etiketi görüntülenir. Bu öğe seçmek için bir kez tıklayın.

**Syles** penceresi tüm bu öğe için CSS kuralları gösterir. Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın. ".Featured .content-sarmalayıcı üzerinde"'i tıklatın. Sayfa denetçisi bu stili (Site.css) tanımlar ve karşılık gelen CSS stil vurgular CSS dosyasını açar.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Şimdi değerini değiştirin `background-color` "red" için. Değişikliği hemen sayfa denetçisi tarayıcısında görüntülenir.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>CSS Renk Seçici kullanma

Visual Studio 2012'de CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici sahiptir. Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, RGB, RGBA, HSL ve HSLA renkleri destekler ve belgede en yakın zamanda kullandığınız renkleri listesini tutar.

Önceki bölümde değerini değiştirdiğiniz `background-color` özelliği. Renk Seçici çağrılacak ekleme noktasını özellik adı ve türü sonra yerleştirin  **#**  veya **rgb (**.

![CSS Renk Seçici çubuğu](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Seçin ya da aşağı ok tuşuna basın için bir renk tıklayın ve sonra renkleri çapraz geçiş için sol ve sağ ok tuşlarını kullanın. Bir renk ziyaret ettiğinizde, karşılık gelen onaltılık değeri önizlemesi:

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Renk çubuğu istediğiniz tam rengi yoksa, Renk Seçici pop aşağı kullanabilirsiniz. Açmak için renk çubuğunun sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşlarına basın.

![CSS Renk Seçici Pop aşağı](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Sağdaki dikey çubuk renkten'ı tıklatın. Bu, ana pencerede, renk için gradyan gösterir. Enter tuşuna basarak doğrudan dikey çubuğu'ndan bir renk seçin veya ile daha iyi kesinlik seçmek için ana penceresinde herhangi bir noktasını tıklatın.

Kullanmak istediğiniz bilgisayar ekranınızda bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olmak zorunda değildir), alt sağ tarafta Damlalık aracını kullanarak değerini yakalayabilirsiniz.

Renk Seçici altındaki kaydırıcısını hareket ettirerek bir rengin geçirgenliğini de değiştirebilirsiniz. Değişiklikleri değerleri RGBA, renk RGBA biçimi opaklık gösterebilir çünkü yapılıyor.

Bir renk seçtikten sonra Enter tuşuna basın ve arka plan rengi girişi tamamlamak için noktalı virgül yazın *Site.css* dosya.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Sayfa denetçisi güncelleştirme çubuğu

Sayfa denetçisi hemen değişikliği algılar *Site.css* dosyası ve bir güncelleştirme çubuğunda bir uyarı görüntüler.

![Güncelleştirme çubuğu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Tüm dosyaları kaydetmek ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşuna basın veya güncelleştirme Çubuğu'nu tıklatın. Vurgulama renk değişikliği tarayıcısında görüntülenir.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>JavaScript için eşleme dinamik sayfası öğeleri

Modern web uygulamalarında sayfasındaki öğeleri genellikle dinamik olarak JavaScript ile oluşturulur. Bu sayfa öğelerine karşılık gelen hiçbir static İşaretleme (HTML ya da Razor) yoktur anlamına gelir.

Sürüm 1.3 sayfa denetçisi şimdi sayfasına karşılık gelen JavaScript kodunu dinamik olarak eklenme öğeleri eşleyebilirsiniz. Bu özellik göstermek için kullanacağız [tek sayfa uygulama (SPA) şablonu](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> SPA şablonu gerektirir [ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) güncelleştirin.


Visual Studio'da, **dosya** &gt; **yeni proje**. Sol bölmede, genişletin **Visual C#**seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**. **Tamam**'ı tıklatın.

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **tek sayfa uygulaması**.

Çözüm Gezgini'nde genişletin **görünümleri** klasörünü ve ardından **giriş** klasör. Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

Sayfa denetçisi tarayıcıda görüntülenen ilk şey diğer bir deyişle, bir oturum açma sayfası olduğu. "Kaydol"'i tıklatın ve bir kullanıcı adı ve parola oluşturun. Kaydolduktan sonra uygulamanın, oturum açtığında ve bazı örnek öğeleriyle Yapılacaklar listesi oluşturur.

Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek. Sayfa denetçisi tarayıcıda, yapılacaklar öğelerini birini tıklatın. Mavi olarak vurgulanmış yerine, öğe "JS" ile turuncu yanındaki öğe adı vurgulanmış olduğundan dikkat edin. Bu öğe komut dosyası aracılığıyla dinamik olarak oluşturulduğunu gösterir.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Ayrıca, üzerinde turuncu bir alt çizginin görünür **çağrı yığını** sekmesi. Bu belirten **çağrı yığını** bölmesi olan öğe hakkında daha fazla bilgi.

Tıklayın **çağrı yığını** sekmesi. **Çağrı yığını** bölmesi öğesi oluşturulan JavaScript çağrısı için çağrı yığını gösterir. JQuery daraltılmış gibi uygulama kodunuzu çağrıları kolayca görmenize olanak tanıyan dış kitaplıklara çağırır.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Dış kitaplıklara çağrıları dahil tam yığını görmek için "Dış kitaplıklar" etiketli düğümleri genişletebilirsiniz:

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Çağrı yığını bir öğeyi tıklatın, Visual Studio kod dosyasını açar ve karşılık gelen betiği vurgular.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Ayrıca Bkz.

[Visual Studio ile ASP.NET MVC 4 giriş](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net Web sitesi)

[Sayfa denetçisi Tanıtımı](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)

[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)
