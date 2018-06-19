---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Page Inspector Visual Studio 2012 kullanarak | Microsoft Docs
author: rick-anderson
description: Bu uygulamalı laboratuar ortamında bulmak ve Visual Studio - Page Inspector web sayfası sorunlarını düzeltmek için yeni bir aracı keşfeder. Page Inspector, yeni aracı bu b ediyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891249"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Visual Studio 2012'de sayfa denetçisi kullanma
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> Bu uygulamalı laboratuar ortamında bulmak ve Visual Studio - Page Inspector web sayfası sorunlarını düzeltmek için yeni bir aracı keşfeder.
> 
> Page Inspector, Visual Studio'ya tarayıcı tanılama araçları getirir ve tarayıcı, ASP.NET ve kaynak kodu arasında tümleşik bir deneyim sağlayan yeni bir aracıdır. Web sayfası (HTML, Web Forms, ASP.NET MVC veya Web sayfalarını) doğrudan Visual Studio IDE içinde işler ve kaynak kodu ve sonuçta çıktı inceleyin olanak tanır. Sayfa denetçisi kolayca bir Web sitesi parçalayın sayfalarını sıfırdan hızlı bir şekilde oluşturmanızı ve sorunları çabucak tanılamanızı sağlar.
> 
> Günümüzde zamanında, ASP.NET MVC ve WebForms gibi esnek ve ölçeklenebilir Web siteleri oluşturma Web çerçeveleri sayısı sahip. Öte yandan, bu daha zor IDE designer görünüm şablon tabanlı sayfaları ve dinamik içerik desteklemediğinden sayfada bulunan sorunları bulmak alır. Bu nedenle, bu Web sitelerinin bir kullanıcıya nasıl göründükleri görmek için tarayıcıda açılması gerekir.
> 
> Web geliştiricileri, düzenli olarak tarayıcıda çalışmasına sorunları bulmak için dış araçları kullanın. Ardından IDE dönün ve düzeltme başlatın. Bu geri ve İleri etkinlik IDE, tarayıcı ve profil oluşturma araçları arasında verimsiz olabilir ve bazen yeni dağıtım ve sorunu yeniden oluşturmak istediğiniz her seferinde temizleme önbellek gerektirir.
> 
> Page Inspector, birleştirilmiş bir dizi özellik kullanarak iki tarafın en iyi araya getirerek Web geliştirme (tarayıcı araçları) istemci ile sunucu (ASP.NET ve kaynak kodu) arasında bir boşluk arasında köprü.
> 
> Sayfa Denetçisi'ni kullanarak (sunucu tarafındaki kod dahil) kaynak dosyalardaki hangi öğelerin tarayıcıda işlenmek üzere HTML biçimlendirmesi ürettiğini görebilirsiniz. Page Inspector ayrıca CSS özelliklerini ve anında tarayıcıda görmenize değişiklikleri görmek için DOM öğesi özniteliklerini değiştirmenize olanak tanır.
> 
> Bu uygulamalı Laboratuvar aracılığıyla sayfa denetçisi özellikleri izlemek ve Web uygulamaları sorunlarını gidermek için bunları nasıl kullanabileceğinizi gösterir. **Bu Laboratuvar benzer akışları ancak farklı teknolojiler hedefleme iki alıştırmaları içerir. Bir ASP.NET MVC geliştiriciyseniz alıştırma birini izleyin; bir WebForms Geliştirici izleyin alıştırma iki varsa**.
> 
> Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Sayfa Denetçisi'ni kullanarak Web sitesi parçalayın
- İnceleyebilir ve önizleme CSS stilleri değişiklikleri sayfa denetçisi ile
- Algılamak ve sayfa Denetçisi'ni kullanarak web sayfalarınıza ilgili sorunları giderme

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).
- Internet Explorer 9 veya üzeri

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: Sayfa denetçisi ASP.NET MVC projelerinde kullanma.](#Exercise1)
2. [Alıştırma 2: Sayfa denetçisi WebForms projelerinde kullanma.](#Exercise2)

> [!NOTE]
> Her alıştırma diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın, başlangıç klasöründe bulunan Başlangıç bir çözüm tarafından eşlik eder. Bir alıştırma için kaynak kodunu içinde karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren bir son klasörü bulacaksınız. Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Alıştırma 1: Sayfa denetçisi ASP.NET MVC projelerinde kullanma.

Bu alıştırmada, Önizleme ve hata ayıklama hakkında bilgi edineceksiniz bir **ASP.NET MVC 4** çözümünü kullanarak **sayfa denetçisi**. İlk olarak, işlem hata ayıklama Web kolaylaştıran özellikler öğrenmek için kısa bir diz aracı geçici gerçekleştirir. Ardından, stil sorunları içeren bir web sayfasında çalışır. Sayfa denetçisi sorun oluşturur Kaynak kodu bulmak için kullanın ve giderin öğreneceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Görev 1 - Page Inspector keşfetme

Bu görevde, Fotoğraf Galerisi gösteren bir ASP.NET MVC 4 Proje bağlamında sayfa denetçisi kullanmayı öğreneceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-MVC4/başlangıç/** klasörü.

   1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Çözüm Gezgini'nde bulun **Index.cshtml** altında görüntülemek **/görünümler/giriş** proje klasörünü sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

    ![Sayfa Denetçisi'nde önizlemek için bir dosya seçmek](using-page-inspector-in-visual-studio-2012/_static/image1.png "sayfa Denetçisi'nde Önizleme için bir dosya seçme")

    *Sayfa Denetçisi'nde Önizleme için bir dosya seçme*
3. Sayfa denetçisi penceresinde gösterecektir */Home/Index* seçtiğiniz görünümünü kaynağına eşlenen URL.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Sayfa denetçisi ile ilk başvurun*

    Sayfa denetçisi aracı, Visual Studio ortamınızda tümleşiktir. Inspector güçlü bir HTML Profil Oluşturucu ile birlikte katıştırılmış bir tarayıcı içerir. Sayfalarınızın nasıl göründüğünü görmek için çözümü çalıştırmak zorunda değilsiniz dikkat edin.

    > [!NOTE]
    > Sayfa denetçisi tarayıcı genişliğini açılan sayfa genişliği az olduğunda, sayfanın düzgün görmezsiniz. Bu durumda, sayfa denetçisi genişliğini ayarla.
4. Tıklatın **dosyaları** sayfa denetçisi sekmesindedir.

    Dizin Sayfası oluşturmakta olduğunuz tüm kaynak dosyaları görürsünüz. Bu özellik, özellikle, kısmi görünümleri ve şablonları ile çalışırken bir bakışta tüm öğeleri tanımlamak için yardımcı olur. Bağlantılar'ı tıklatırsanız, ayrıca her dosya açabilmesini dikkat edin.

    ![Dosyalar sekmesi](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Dosyalar sekmesi*
5. Tıklatın **denetleme moduna geç** sekmeleri sol tarafında bulunan düğmesini.

    Bu araç, sayfanın herhangi bir öğe seçin ve HTML ve Razor kodu görmek olanak tanır.

    ![Denetleme modu düğme Değiştir](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *İki durumlu Denetleme modu düğmesi*
6. Sayfa denetçisi tarayıcıda, fare işaretçisini sayfası öğeleri taşıyın. İşlenen sayfanın herhangi bir bölümünü fare işaretçisini taşırken öğe türü görüntülenir ve karşılık gelen kaynak işaretleme veya kod Visual Studio düzenleyicisinde vurgulanır.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Denetleme modunda eylemi*

    > [!NOTE]
    > Sayfa denetçisi penceresinin ekranı kaplamasını sağlayın değil veya kaynak kodunu gösteren Önizleme sekmesi görmek mümkün olmaz. Tam ekran, sayfa denetçisi öğesinde tıklarsanız, seçim kaynak kodunu görünür, ancak sayfa denetçisi penceresi gizlenir.

    Dikkat edilmesi durumunda **Index.cshtml** dosya, seçilen öğeyi oluşturur Kaynak kodu kısmı vurgulanır görürsünüz. Bu özellik düzenleme kod erişmek için doğrudan ve hızlı bir şekilde sağlayarak uzun kaynak dosyalarını, kolaylaştırır.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Öğeleri inceleniyor*
7. Tıklatın **denetleme moduna geç** düğmesi (![sayfa denetçisi tarayıcıda işlenen HTML kodunu görüntülemek için HTML sekmesini seçin.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Sayfa denetçisi tarayıcıda işlenen HTML kodunu görüntülemek için HTML sekmesini seçin.") ) imleci devre dışı bırakmak için.
8. Seçin **HTML** sayfa denetçisi tarayıcıda işlenen HTML kodunu görüntülemek için sekme.
9. HTML biçimlendirmesi Koala bağlantı listesi öğesiyle bulup seçin.

    Kodu seçtiğinizde, karşılık gelen çıktı tarayıcıda otomatik olarak vurgulanmış dikkat edin. Bu özellik sayfasında bir HTML bloğu nasıl işlendiğine görmek kullanışlıdır.

    ![Seçme HTML öğesi sayfasında](using-page-inspector-in-visual-studio-2012/_static/image8.png "sayfasında seçerek HTML öğesi")

    *Sayfanın HTML öğesi seçme*
10. Tıklatın **denetleme moduna geç** etkinleştirmek için düğmeyi *Denetleme modu* ve Gezinti Çubuğu'nu tıklatın. Sağ tarafındaki HTML kodunda stilleri bölmesinde seçilen öğeye uygulanan CSS stilleri listesini görürsünüz.

    > [!NOTE]
    > Üst site düzenini bir parçası olduğundan, sayfa denetçisi, aynı zamanda açar \_Layout.cshtml dosyasını ve kod kesimi etkilenen Vurgula.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Stilleri ve seçili bir öğenin kaynak dosyaları bulma*
11. Etkin geçiş denetleme işaretçisi ile mavi öne çıkan çubuğunun altında fare işaretçisini ve yarı daireye tıklayın.

    ![Bir öğeyi seçerek](using-page-inspector-in-visual-studio-2012/_static/image10.png "bir öğe seçme")

    *Bir öğe seçme*
12. Stilleri bölmesinde bulun **arka plan görüntüsü** altında madde **.main içerik** grubu. **İşaretini** **arka plan görüntüsü** ve ne olduğunu gözleyin. Tarayıcı değişiklikleri hemen yansıtır ve dairenin gizli fark edeceksiniz.

    > [!NOTE]
    > Sayfa denetçisi stilleri sekmesinde uyguladığınız değişiklikler özgün stil etkilemez. Stilleri seçeneğinin işaretini kaldırın veya değerlerine sayıda istiyor, ancak bunlar sayfa yenilendikten sonra geri yüklenecek değiştirin.

    ![CSS stilleri devre dışı bırakma ve etkinleştirme](using-page-inspector-in-visual-studio-2012/_static/image11.png "etkinleştirme ve CSS stilleri devre dışı bırakma")

    *CSS stilleri devre dışı bırakma ve etkinleştirme*
13. Şimdi, '**buraya logonuz konacak**' denetleme modunu kullanarak üstbilgi metni.
14. İçinde **stilleri** sekmesinde, bulmak **yazı tipi boyutu** CSS özniteliği altında **.site-başlık** grubu. Öznitelik değeri çift tıklayın ve 2.3 em değeriyle **3 em**ve tuşuna basarak **ENTER**. Başlığı daha büyük arar dikkat edin.

    ![CSS değerleri sayfa Denetçisi'nde değiştirme](using-page-inspector-in-visual-studio-2012/_static/image12.png "değiştirme CSS değerleri sayfa Denetçisi'nde")

    *Sayfa Denetçisi'nde CSS değerlerini değiştirme*
15. Tıklatın **izleme stilleri** sayfa denetçisi sağ bölmede bulunan sekmesinden. Bu öznitelik adına göre sıralanmış seçimi, uygulanan tüm stilleri görmek için alternatif bir yoludur.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Seçili öğenin CSS stilleri izleme*
16. Başka bir sayfa denetçisi Düzen bölmesi özelliğidir. Denetleme modu kullanırken Gezinti Çubuğu'nu seçin ve ardından **düzeni** sekmesini sağ bölmede. Seçilen öğeyi tam boyutunu yanı sıra, uzaklık, kenar boşluğu, doldurma ve kenarlık boyutuna görürsünüz. Bu görünüm değerleri de değiştirebilirsiniz dikkat edin.

    ![Sayfa denetçisi öğesi düzende](using-page-inspector-in-visual-studio-2012/_static/image14.png "sayfa denetçisi öğesi düzeni")

    *Sayfa denetçisi öğesi düzeni*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Görev 2 - bulma ve Fotoğraf Galerisi stili sorunlarını giderme

Nasıl Visual Studio'nun önceki sürümleri ile Web sayfaları sorunlarını tanılamak? Hata ayıklama Internet Explorer Geliştirici Araçları veya Firebug gibi Visual Studio IDE dışında çalıştırmak araçları web ile büyük olasılıkla tanıdık olduğunuz. Tarayıcılar yalnızca anlamak HTML, komut dosyası ve temel bir çerçeve oluşturulacak HTML oluştururken stilleri. Bu nedenle, genellikle gibi web sayfaları nasıl göründüğünü görmek için tüm sitenin dağıtmanız gerekebilir.

Algılamak ve web sitenizdeki bir sorunu gidermek istediğinizde, büyük olasılıkla bu adımları uyguladığınız:

1. Visual Studio'dan çözümü çalıştırın veya web sunucusu sayfasında dağıtın.
2. Tarayıcıda, kullanma veya yalnızca kaynak kodu ve stiller açın ve sorunu eşleştirmeye geliştirici araçları açın. Dosyaları dahil edilen bulmak için kullandığınız &quot;arama&quot; veya &quot;dosyalarında arama&quot; Stil sınıfları adını özelliklerle.
3. Hata algılandığında, Web tarayıcısı ve sunucu durdurun.
4. Tarayıcı önbelleğini temizleyebilirsiniz.
5. Bir düzeltme uygulamak için Visual Studio'ya geri dönün. Test adımları yineleyin.

ASP.NET MVC 4'te hiçbir gerçek WYSIWYG gibi stil sorunların çoğunu çalıştıran veya web uygulama dağıtıldıktan sonra bir sonraki aşama üzerinde algılanır. Şimdi, sayfa denetçisi ile çözüm çalıştırmadan herhangi bir sayfayı önizlemek mümkündür.

Bu görevde, sayfa denetçisi kullanabilir ve Fotoğraf Galerisi uygulama bazı sorunları giderin.

1. Sayfa denetçisi kullanarak bulun **kaydetmek** ve **oturum** başlığının sol tarafındaki bağlantılar.

    Bağlantıları sağdaki beklenen yerde görüntülenmez ve madde işaretli bir liste gibi gösterilir dikkat edin. Şimdi bağlantıları sağa hizalayın ve buna göre restyle.

    ![Kayıt ve günlük bağlantıları bulma](using-page-inspector-in-visual-studio-2012/_static/image15.png "kayıt ve günlük bağlantıları bulma")

    *Kayıt ve günlük bağlantıları bulma*
2. İki durumlu denetleme moduyla seçili için Kapat ancak kendi kod açmak için değil kayıt bağlantı üzerindeki'ı tıklatın.

    Kaynak kodu bağlantılar bulunur bildirimi  **\_LoginPartial.cshtml** dosya, Index.cshtml veya \_ilk yerinde görünebilir yerlerdir Layout.cshtml. Doğrudan doğru kaynak dosyasında yerleştirildi.
3. İçinde **stilleri** sekmesinde, bulun ve tıklatın **<section> #login</section>** bu bağlantıları için HTML kapsayıcı öğesi.

    Dikkat **#login** stili bulunan otomatik olarak **Site.css** tıklattıktan sonra. Ayrıca, kod artık vurgulanır.

    ![CSS stilleri seçme](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS stilleri seçme")

    *CSS stilleri seçme*
4. Açıklamadan çıkarın **text-align** özniteliği vurgulanmış kodu açma ve kapatma karakterleri kaldırarak ve kaydetme **Site.css** dosya.

    Sayfa denetçisi geçerli sayfayı oluşturan tüm farklı dosyalarını farkındadır ve bu dosyalar değiştirdiğinizde algılayabilir. Geçerli sayfasını tarayıcıda kaynak dosyalarını içeren eşitlenmiş değil olduğunda sizi uyarır.
5. Sayfa denetçisi tarayıcıda sayfayı yeniden Adres çubuğunun altında bulunan Çubuğu'nu tıklatın.

    ![Sayfa yeniden yükleniyor](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Sayfa yeniden yükleniyor*

    Sağ taraftaki bağlantılardır şimdi ancak hala bir listeyi gibi görünürler. Artık kaldırdığımda ve bağlantıları yatay olarak hizalayın.

    ![Güncelleştirilmiş sayfası](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Güncelleştirilmiş sayfası*
6. Denetleme modu kullanarak, seçin herhangi birini **&lt;li&gt;** içeren öğelerini &quot;kaydetmek&quot; ve &quot;oturum&quot; bağlantılar. Ardından  **&lt;bölüm&gt; #login** öğesi erişimi **Styles.css** kodu.

    ![Stil bulma](using-page-inspector-in-visual-studio-2012/_static/image19.png "stili bulma")

    *Stil bulma*
7. İçinde **Style.css**, kodunu açıklamadan çıkarın **#login li** öğeleri. Eklediğiniz stili madde işareti gizleme ve öğeleri Yatayda görüntüleyebilirsiniz.

    ![Oturum açma bağlantılar yeniden tasarıma](using-page-inspector-in-visual-studio-2012/_static/image20.png "oturum açma bağlantılar yeniden tasarıma")

    *Oturum açma bağlantılar yeniden tasarıma*
8. Kaydet **Style.css** dosya ve sayfayı yeniden yüklemek için aşağıdaki adresi bulunan çubuğunda bir kez tıklatın. Bağlantıları doğru görüntülendiğine dikkat edin.

    ![Bağlantılar için sağ tarafı hizalı](using-page-inspector-in-visual-studio-2012/_static/image21.png "Bağlantılar sağa hizalı")

    *Bağlantılar için sağ tarafı hizalı*
9. Son olarak, üstbilgi başlık değiştirir. Tıklatın için denetleme modunu kullanması **buraya logonuz konacak** metin ve bunu oluşturan kaynak kodu get.
10. İçinde bulunduğunuz artık  **\_Layout.cshtml**, Değiştir '**buraya logonuz konacak**'text' ile**Fotoğraf Galerisi**'. Kaydet ve sayfa denetçisi tarayıcı güncelleştirin.

    ![Yeni bir başlık atama](using-page-inspector-in-visual-studio-2012/_static/image22.png "yeni bir başlık atama")

    *Yeni bir başlık atama*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Fotoğraf Galerisi sayfa güncelleştirildi*
11. Son olarak, selet **Fotografgalerisi** proje ve tuşuna **F5** uygulamayı çalıştırmak için. Tüm değişiklikleri beklendiği gibi çalışmayabilir denetleyin.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Alıştırma 2: Sayfa denetçisi WebForms projelerinde kullanma.

Bu alıştırmada, Önizleme ve sayfa denetçisi kullanarak bir WebForms çözüm hatalarını ayıklamak üzere öğreneceksiniz. İşlem hata ayıklama Web kolaylaştırmak sayfa denetçisi özellikleri öğrenmek için kısa bir diz aracı geçici ilk gerçekleştirir. Ardından, stil sorunları içeren bir web sayfasında çalışır. Sayfa denetçisi sorun oluşturur Kaynak kodu bulmak için kullanın ve giderin öğreneceksiniz.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Görev 1 - Page Inspector keşfetme

Bu görevde, sayfa denetçisi özelliklerinin bir Fotoğraf Galerisi gösterir WebForms proje bağlamında nasıl kullanılacağını öğreneceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-WebForms/başlangıç/** klasörü.

   1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Çözüm Gezgini'nde bulun **Default.aspx** sayfasında, sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

    ![Sayfa denetçisi ile default.aspx açma](using-page-inspector-in-visual-studio-2012/_static/image24.png "Default.aspx sayfa denetçisi ile açma")

    *Sayfa denetçisi ile açılış Default.aspx*
3. Sayfa denetçisi penceresi Default.aspx gösterir.

    ![Sayfa Denetçisi'nde default.aspx görüntüleme](using-page-inspector-in-visual-studio-2012/_static/image25.png "Default.aspx sayfa Denetçisi'nde görüntüleme")

    *Sayfa Denetçisi'nde default.aspx görüntüleme*

    Sayfa denetçisi aracı, Visual Studio ortamınızda tümleşiktir. Seçilen koda gösterecektir güçlü bir HTML Profil Oluşturucu ile birlikte katıştırılmış bir tarayıcı denetçisi içerir. Sayfalarınızın nasıl göründüğünü görmek için çözümü çalıştırmak zorunda değilsiniz dikkat edin.

    > [!NOTE]
    > Sayfa denetçisi tarayıcı genişliğini açılan sayfa genişliği az olduğunda, sayfanın düzgün görmezsiniz. Bu durumda, sayfa denetçisi genişliğini ayarla.
4. Tıklatın **dosyaları** sayfa denetçisi sekmesindedir.

    İşlenen varsayılan sayfa oluşturmakta olduğunuz tüm kaynak dosyaları görürsünüz. Bu, özellikle kullanıcı denetimleri ve ana sayfa ile çalışırken bir bakışta tüm öğeleri tanımlamak için yararlı bir özelliktir. Her dosya için de gidebilirsiniz dikkat edin.

    ![Dosyalar sekmesi](using-page-inspector-in-visual-studio-2012/_static/image26.png "dosyalar sekmesi")

    *Dosyalar sekmesi*
5. Tıklatın **denetleme moduna geç** sekmeleri sol tarafında bulunan düğmesini.

    Bu araç, sayfanın herhangi bir öğe seçin ve HTML kod ve .aspx kaynağına bakın olanak tanır.

    ![İki durumlu Denetleme modu düğme](using-page-inspector-in-visual-studio-2012/_static/image27.png "denetleme modunu Değiştir düğmesi")

    *İki durumlu Denetleme modu düğmesi*
6. Sayfa denetçisi tarayıcıda sayfası öğeleri fareyi hareket ettirin. İşlenen sayfanın herhangi bir bölümünü fare işaretçisini taşırken öğe türü görüntülenir ve karşılık gelen kaynak işaretleme veya kod Visual Studio düzenleyicisinde vurgulanır.

    ![Denetleme modunda eylem](using-page-inspector-in-visual-studio-2012/_static/image28.png "eylem denetleme modunda")

    *Denetleme modunda eylemi*

    > [!NOTE]
    > Sayfa denetçisi penceresinin ekranı kaplamasını sağlayın değil veya kaynak kodunu gösteren Önizleme sekmesi görmek mümkün olmaz. Tam ekran, sayfa denetçisi öğesinde tıklarsanız, seçim kaynak kodunu görünür, ancak sayfa denetçisi penceresi gizlenir.

    Dikkat edilmesi durumunda **Default.aspx** dosya, seçilen öğeyi oluşturur Kaynak kodu kısmı vurgulanır görürsünüz. Bu özellik kodu erişmek için doğrudan ve hızlı bir şekilde sağlayarak uzun kaynak dosyalarını sürümü kolaylaştırır.

    ![Öğeleri inceleniyor](using-page-inspector-in-visual-studio-2012/_static/image29.png "öğeleri inceleniyor")

    *Öğeleri inceleniyor*
7. Tıklatın **denetleme moduna geç** düğmesi (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), sayfa denetçisi sekmeleri imleci devre dışı bırakmak için bulunur.
8. Seçin **HTML** sayfa denetçisi tarayıcıda işlenen HTML kodunu görüntülemek için sekme.
9. HTML kodunda Koala bağlantı listesi öğesiyle bulup seçin.

    Karşılık gelen çıkış kodu seçtiğinizde, otomatik olarak vurgulanan tarayıcı olduğuna dikkat edin. Bu özellik sayfasında bir HTML bloğu nasıl işlendiğine görmek kullanışlıdır.

    ![Sayfanın HTML öğesini seçerek](using-page-inspector-in-visual-studio-2012/_static/image31.png "sayfasında bir HTML öğesi seçme")

    *Sayfasında bir HTML öğesi seçme*
10. Tıklatın **denetleme moduna geç** etkinleştirmek için düğmeyi *Denetleme modu* ve Gezinti Çubuğu'nu tıklatın. Sağ tarafındaki HTML kodunda stilleri bölmesinde seçilen öğeye uygulanan CSS stilleri listesini görürsünüz.

    > [!NOTE]
    > üst site düzenini bir parçası olduğundan, sayfa denetçisi ayrıca Site.Master dosyasını açın ve etkilenen kod kesimi vurgulayın.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "stilleri ve seçili bir öğenin kaynak dosyaları bulma")

    *Stilleri ve seçili bir öğenin kaynak dosyaları bulma*
11. Etkin geçiş denetleme işaretçisi ile menü çubuğunun altında fare işaretçisini ve boş yarım daireye tıklayın.

    ![Bir öğeyi seçerek](using-page-inspector-in-visual-studio-2012/_static/image33.png "bir öğe seçme")

    *Bir öğe seçme*
12. Stilleri bölmesinde bulun **arka plan görüntüsü** altında madde **.main içerik** grubu. **İşaretini** **arka plan görüntüsü** ve ne olduğunu gözleyin. Tarayıcı değişiklikleri hemen yansıtır ve dairenin gizli fark edeceksiniz.

    > [!NOTE]
    > Sayfa denetçisi stilleri sekmesinde uyguladığınız değişiklikler özgün stil etkilemez. Stilleri seçeneğinin işaretini kaldırın veya değerlerine sayıda istiyor, ancak bunlar sayfa yenilendikten sonra geri yüklenecek değiştirin.

    ![Etkinleştirme ve CSS styles2 devre dışı bırakma](using-page-inspector-in-visual-studio-2012/_static/image34.png "etkinleştirme ve CSS stilleri devre dışı bırakma")

    *CSS stilleri devre dışı bırakma ve etkinleştirme*
13. Şimdi, '**,** **burada logosu '** denetleme modunu kullanarak üstbilgi metni.
14. İçinde **stilleri** sekmesinde, bulmak **yazı tipi boyutu** CSS özniteliği altında **.site-başlık** grubu. Öznitelik bir kez değerini düzenlemek için çift tıklatın. Değiştir 2.3em değerle **3em**, ve ardından ENTER tuşuna basın. Başlığı daha büyük arar dikkat edin.

    ![Sayfa Inspector2 CSS değerleri değiştirme](using-page-inspector-in-visual-studio-2012/_static/image35.png "değiştirme CSS değerleri sayfa Denetçisi'nde")

    *Sayfa Denetçisi'nde CSS değerlerini değiştirme*
15. Tıklatın **izleme stilleri** sayfa denetçisi sağ bölmede bulunan sekmesinden. Bu öznitelik adına göre sıralanmış seçimi, uygulanan tüm stilleri görmek için alternatif bir yoludur.

    ![Seçili öğenin CSS stilleri izleme](using-page-inspector-in-visual-studio-2012/_static/image36.png "seçili öğenin CSS stilleri izleme")

    *Seçili öğenin CSS stilleri izleme*
16. Başka bir sayfa denetçisi Düzen bölmesi özelliğidir. Denetleme modu kullanırken Gezinti Çubuğu'nu seçin ve ardından **düzeni** sekmesini sağ bölmede. Seçilen öğeyi tam boyutunu yanı sıra, uzaklık, kenar boşluğu, doldurma ve kenarlık boyutuna görürsünüz. Bu görünüm değerleri de değiştirebilirsiniz dikkat edin.

    ![Sayfa denetçisi öğesi düzende](using-page-inspector-in-visual-studio-2012/_static/image37.png "sayfa denetçisi öğesi düzeni")

    *Sayfa denetçisi öğesi düzeni*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Görev 2 - bulma ve Fotoğraf Galerisi stili sorunlarını giderme

Nasıl Visual Studio'nun önceki sürümleri ile Web sayfaları sorunlarını tanılamak? Hata ayıklama Internet Explorer Geliştirici Araçları veya Firebug gibi Visual Studio IDE dışında çalıştırmak araçları web ile büyük olasılıkla tanıdık olduğunuz. Tarayıcılar yalnızca anlamak HTML, komut dosyası ve temel bir çerçeve oluşturulacak HTML oluştururken stilleri. Bu nedenle, genellikle gibi web sayfaları nasıl göründüğünü görmek için tüm sitenin dağıtmanız gerekebilir.

Algılamak ve web sitenizdeki bir sorunu gidermek istediğinizde, büyük olasılıkla bu adımları uyguladığınız:

1. Visual Studio'dan çözümü çalıştırın veya web sunucusu sayfasında dağıtın.
2. Tarayıcıda, kullanma veya yalnızca kaynak kodu ve stiller açın ve sorunu eşleştirmeye geliştirici araçları açın. Dosyaları dahil edilen bulmak için kullandığınız &quot;arama&quot; veya &quot;dosyalarında arama&quot; Stil sınıfları adını özelliklerle.
3. Hata algılandığında, Web tarayıcısı ve sunucu durdurun.
4. Tarayıcı önbelleğini temizleyebilirsiniz.
5. Bir düzeltme uygulamak için Visual Studio'ya geri dönün. Test adımları yineleyin.

Olduğundan Hayır ASP.NET WebForms gerçek WYSIWYG, bazı stil sorunlar daha sonraki bir aşamada üzerinde çalışan veya dağıtıldıktan sonra algılanır. Şimdi, sayfa denetçisi ile çözüm çalıştırmadan herhangi bir sayfayı önizlemek mümkündür.

Bu görevde, Fotoğraf Galerisi uygulama ile ilgili bazı sorunları düzeltmek için sayfa denetçisi kullanır. Aşağıdaki adımlarda algılayabilir ve hızlı bir şekilde üstbilgisinde bazı basit stil sorunu düzeltin.

1. Sayfa denetleme kullanarak bulun **kaydetmek** ve **oturum** başlığının sol tarafındaki bağlantılar.

    Bağlantıyı sağ taraftaki beklenen yerde görüntülenmez dikkat edin. Artık bağlantıyı sağa hizalayın ve buna göre restyle.

    ![Sol tarafta konumlandırılmış bağlantısında oturum](using-page-inspector-in-visual-studio-2012/_static/image38.png "sol tarafta konumlandırılmış Bağlantısı'nda oturum açın")

    *Sol tarafta konumlandırılmış oturum bağlantısı*
2. İki durumlu denetleme moduyla seçili kendi kod açmak için oturum bağlantısını seçin.

    Bağlantı kaynak kodu bulunan bildirim **Site.Master** dosyası, hangi yerdir Default.aspx sayfasında ilk yerinde görünebilir; doğrudan doğru kaynak dosyasında yerleştirildi.
3. İçinde **stilleri** sekmesinde, bulun ve tıklatın  **&lt;bölüm&gt; #login** bu bağlantıları için HTML kapsayıcı öğesi.

    Dikkat **#login** stili bulunan otomatik olarak **Site.css** tıklattıktan sonra. Ayrıca, kod artık vurgulanır.

    ![CSS stilleri seçme](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS stilleri seçme")

    *CSS stilleri seçme*
4. Açıklamadan çıkarın **text-align** özniteliği vurgulanmış kodu açma ve kapatma karakterleri kaldırarak ve kaydetme **Site.css** dosya.

    Sayfa denetçisi geçerli sayfayı oluşturan tüm farklı dosyalarını farkındadır ve bu dosyalar değiştirdiğinizde algılayabilir. Geçerli sayfasını tarayıcıda kaynak dosyalarını içeren eşitlenmiş değil olduğunda sizi uyarır.
5. Sayfa denetçisi tarayıcıda değişiklikleri kaydetmek ve sayfayı yeniden yüklemek için Adres çubuğunun altında bulunan Çubuğu'nu tıklatın.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Sayfa yeniden yükleniyor*

    Sağ taraftaki bağlantılardır şimdi ancak hala bir listeyi gibi görünürler. Artık kaldırdığımda ve bağlantıları yatay olarak hizalayın.

    ![Güncelleştirilmiş sayfası](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Güncelleştirilmiş sayfası*
6. Denetleme modu kullanarak, seçin herhangi birini **&lt;li&gt;** içeren öğelerini &quot;kaydetmek&quot; ve &quot;oturum&quot; bağlantılar. Ardından  **&lt;bölüm&gt; #login** öğesi erişimi **Styles.css** kodu.

    ![Stil bulma](using-page-inspector-in-visual-studio-2012/_static/image42.png "stili bulma")

    *Stil bulma*
7. İçinde **Style.css**, kodunu açıklamadan çıkarın **#login li** öğeleri. Eklediğiniz stili madde işareti gizleme ve öğeleri Yatayda görüntüleyebilirsiniz.

    ![Oturum açma bağlantılar yeniden tasarıma](using-page-inspector-in-visual-studio-2012/_static/image43.png "oturum açma bağlantılar yeniden tasarıma")

    *Oturum açma bağlantılar yeniden tasarıma*
8. Kaydet **Style.css** dosya ve sayfayı yeniden yüklemek için aşağıdaki adresi bulunan çubuğunda bir kez tıklatın. Bağlantıları doğru görüntülendiğine dikkat edin.

    ![Bağlantılar için sağ tarafı hizalı](using-page-inspector-in-visual-studio-2012/_static/image44.png "Bağlantılar sağa hizalı")

    *Bağlantılar için sağ tarafı hizalı*
9. Son olarak, üstbilgi başlık değiştirir. Arama yerine '**buraya logonuz konacak '** tüm dosyaları metinde metni tıklatın ve onu oluşturan kaynak koduna almak için denetleme modunu kullanın.

    ![Site başlığını bulma](using-page-inspector-in-visual-studio-2012/_static/image45.png "site başlığını bulma")

    *Site başlığını bulma*
10. İçinde bulunduğunuz artık **Site.Master**, Değiştir '**buraya logonuz konacak**'text' ile**Fotoğraf Galerisi**'. Kaydet ve sayfa denetçisi tarayıcı güncelleştirin.

    ![Fotoğraf Galerisi sayfa güncelleştirilmiş](using-page-inspector-in-visual-studio-2012/_static/image46.png "güncelleştirilmiş Fotoğraf Galerisi sayfası")

    *Fotoğraf Galerisi sayfa güncelleştirildi*
11. Son olarak basın **F5** tüm değişiklikleri iş beklendiği gibi kullanıma uygulamayı çalıştırmak için.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı Laboratuvar tamamlayarak sayfa denetçisi, Web uygulamanızı yeniden oluşturun ve Web sitesini bir tarayıcıda çalıştırmak zorunda kalmadan Önizleme için nasıl kullanılacağını learnt. Ayrıca, hızlı bir şekilde bulmak ve kaynak koduna işlenmiş çıkışı doğrudan erişerek hataları düzeltmek nasıl learnt.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](using-page-inspector-in-visual-studio-2012/_static/image47.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express Web döşemeye*
