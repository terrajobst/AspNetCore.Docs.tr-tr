---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012'de sayfa denetçisini kullanma | Microsoft Docs
author: rick-anderson
description: Bu uygulamalı laboratuvarda, Visual Studio - sayfa denetçisi web sayfası sorunlarını bulmak ve yeni bir aracı keşfeder. Page Inspector, yeni bir aracı bu b ediyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 236b739abb8c9073535361040dd7d921da9dba6e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365730"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Visual Studio 2012'de sayfa denetçisini kullanma
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

> Bu uygulamalı laboratuvarda, Visual Studio - sayfa denetçisi web sayfası sorunlarını bulmak ve yeni bir aracı keşfeder.
> 
> Page Inspector, Visual Studio'ya tarayıcı tanılama araçları getirir ve tarayıcı, ASP.NET ve kaynak kodu arasında tümleşik bir deneyim sağlayan yeni bir aracıdır. Visual Studio IDE içinden doğrudan bir web sayfası (HTML, Web Forms, ASP.NET MVC veya Web sayfaları) oluşturur ve kaynak kodu hem de elde edilen çıkış incelemenize olanak sağlar. Page Inspector, Web sitesi bir kolayca ayırmak, sayfalarını sıfırdan hızlı bir şekilde oluşturmak ve sorunları çabucak tanılamanızı sağlar.
> 
> Günümüzde birçok zamanında, ASP.NET MVC ve WebForms gibi esnek ve ölçeklenebilir Web siteleri oluşturmak Web çerçeveleri sahibiz. Öte yandan, bu daha zor IDE şablon tabanlı sayfaları ve dinamik içerik Tasarımcı görünümü desteklemediği için sayfalarında sorunları bulmak alır. Bu nedenle, bu Web sitelerinin bir kullanıcıya görünme görmek için bir tarayıcıda açılması gerekir.
> 
> Web geliştiricileri, dış araçların düzenli olarak bir tarayıcıda çalışan sorunları bulmak için kullanın. Ardından, IDE'ye dönün ve düzeltme başlatın. Bu geri ve İleri etkinliği IDE, tarayıcı ve profil oluşturma araçları arasında verimsiz olabilir ve bazen yeni dağıtım ve önbelleği temizleme, bir sorunu yeniden oluşturmak istediğiniz her zaman gerektirir.
> 
> Page Inspector, birleştirilmiş bir dizi özellik kullanarak her iki platformdan da araya getirerek Web geliştirme istemci (tarayıcı araçları) ve sunucu (ASP.NET ve kaynak kodu) arasında bir boşluk arasında köprü.
> 
> Sayfa Denetçisi'ni kullanarak, hangi öğelerin (sunucu tarafındaki kod dahil) kaynak dosyalarında tarayıcıda işlenmek üzere HTML biçimlendirmesi ürettiğini görebilirsiniz. Page Inspector ayrıca CSS özelliklerini ve hemen tarayıcıda görmenize değişiklikleri görmek için DOM öğesi özniteliklerini değiştirmenize olanak sağlar.
> 
> Bu uygulamalı laboratuvarı, sayfa denetçisi özellikleriyle yol ve Web uygulamalarınızda sorunları gidermek için bunları nasıl kullanacağınızı gösterir. **Bu Laboratuvar benzer akışlarını kullanarak ancak farklı teknolojilerini hedefleyen iki alıştırmaları içerir. Bir ASP.NET MVC geliştiriciyseniz alıştırma birini izleyin. Eğer bir WebForms Geliştirici izleyin alıştırma iki**.
> 
> Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Sayfa Denetçisi'ni kullanarak bir Web sitesi parçalara ayırın
- İnceleyin ve sayfa denetçisi ile CSS stilleri Değişiklikleri Önizle
- Sayfa Denetçisi'ni kullanarak web sayfalarınıza sorunlarını algılamak ve

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).
- Internet Explorer 9 veya üzeri

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [Alıştırma 1: ASP.NET MVC projeleri sayfa denetçisini kullanma](#Exercise1)
2. [Alıştırma 2: WebForms projelerinde sayfa denetçisini kullanma](#Exercise2)

> [!NOTE]
> Her alıştırma her alıştırma diğerlerinden takip etmenize olanak tanıyan alıştırmada başlangıç klasöründe yer alan bir başlangıç çözümü vardır. Bir alıştırma için kaynak kodu içinde karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren bir son klasörü bulabilirsiniz. Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Alıştırma 1: ASP.NET MVC projeleri sayfa denetçisini kullanma

Bu alıştırmada, Önizleme ve hata ayıklama hakkında bilgi edineceksiniz bir **ASP.NET MVC 4** çözümünü kullanarak **sayfa denetçisi**. İlk olarak, işlemin hatalarını ayıklamaya Web kolaylaştırmak özellikleri öğrenmek için bir kısa turu aracı gerçekleştirir. Ardından, stil sorunlarını içeren bir web sayfasında çalışır. Sorun oluşturan kaynak kodunu bulmak için sayfa Denetçisi'ni kullanın ve Düzelt öğreneceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Görev 1 - sayfa Denetçisi'ni keşfetme

Bu görevde bir Fotoğraf Galerisi gösteren bir ASP.NET MVC 4 Proje bağlamında sayfa denetçisi kullanmayı öğreneceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-MVC4/başlangıç/** klasör.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Çözüm Gezgini'nde bulun **Index.cshtml** görünümüne **/görünümler/giriş** proje klasörü sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

    ![Sayfa denetçisi içinde önizlemek için bir dosya seçmek](using-page-inspector-in-visual-studio-2012/_static/image1.png "içinde sayfa denetçisi önizlemek için bir dosya seçme")

    *Page Inspector Önizleme için bir dosya seçme*
3. Sayfa denetçisi penceresi */Home/Index* eşlenen seçtiğiniz görünümü kaynak URL'si.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Sayfa denetçisi ile ilk iletişim*

    Sayfa denetçisi aracı Visual Studio ortamınıza tümleşiktir. Inspector, güçlü bir HTML Profil Oluşturucu ile birlikte katıştırılmış bir tarayıcı içerir. Sayfalarınızın nasıl göründüğünü görmek için çözümü çalıştırmak zorunda değilsiniz dikkat edin.

    > [!NOTE]
    > Sayfa denetçisi tarayıcı genişliğini açılan sayfanın genişliğine'dan olduğunda, sayfanın düzgün tarafından görülmez. Bu durumda, sayfa denetçisi genişliğini ayarla.
4. Tıklayın **dosyaları** sayfa denetçisi sekmesindedir.

    Dizin Sayfası oluşturmakta olduğunuz tüm kaynak dosyaları görürsünüz. Bu özellik, özellikle kısmi görünümleri ve şablonlar ile çalışırken, bir bakışta tüm öğeleri tanımlamak için yardımcı olur. Bağlantıları tıklarsanız ayrıca her dosya açabileceğiniz dikkat edin.

    ![Dosyalar sekmesi](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Dosyalar sekmesi*
5. Tıklayın **denetleme modunu Aç/Kapat** sekmelerin sol tarafında bulunan düğmesini.

    Bu araç, sayfanın herhangi bir öğe seçin ve HTML ve Razor kodunu görmek olanak tanır.

    ![İnceleme modu düğmesi Aç/Kapat](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *İki durumlu İnceleme modu düğme*
6. Sayfa denetçisi tarayıcıda sayfa öğelerin üzerine fare işaretçisini taşıyın. İşlenen sayfanın herhangi bir bölümü fare işaretçisini taşıyın, ancak öğe türü görüntülenir ve Visual Studio Düzenleyicisi'nde, karşılık gelen kaynak biçimlendirmeyi veya kodu vurgulanır.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Denetleme modunda, eylem*

    > [!NOTE]
    > Sayfa denetçisi pencerenin en üst düzeye çıkarabilirim veya kaynak kodunu gösteren bir önizleme sekmesinde görmeniz mümkün olmayacaktır. Sayfa denetçisi öğesinde tıklarsanız, büyütüldüğünde seçimin kaynak kodu görünür ancak sayfa denetçisi penceresini gizler.

    Dikkatini ödeme yaparsanız **Index.cshtml** dosya, seçili öğenin oluşturduğu kaynak kod bölümünü vurgulanır görürsünüz. Bu özellik, kodu erişmek için doğrudan ve hızlı bir şekilde sağlayarak uzun kaynak dosyalarını, düzenleme kolaylaştırır.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Öğeleri inceleniyor*
7. Tıklayın **denetleme modunu Aç/Kapat** düğmesine (![sayfa denetçisi tarayıcıda İşlenmiş HTML kodunu görüntülemek için HTML sekmesini seçin.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Sayfa denetçisi tarayıcıda İşlenmiş HTML kodunu görüntülemek için HTML sekmesini seçin.") ) imleci devre dışı bırakmak için.
8. Seçin **HTML** sayfa denetçisi tarayıcıda İşlenmiş HTML kodunu görüntülemek için sekmesinde.
9. HTML biçimlendirmede Koala bağlantısını içeren bir liste öğesi bulun ve seçin.

    Kod seçtiğinizde, ilgili çıkış tarayıcıda otomatik olarak vurgulanan dikkat edin. Bu özellik, bir HTML bloğunu sayfada nasıl oluşturulacağını görmek kullanışlıdır.

    ![Seçme HTML öğesi sayfasında](using-page-inspector-in-visual-studio-2012/_static/image8.png "sayfasında seçerek HTML öğesi")

    *Sayfanın HTML öğesi seçme*
10. Tıklayın **denetleme modunu Aç/Kapat** etkinleştirmek için düğmeye *İnceleme modu* ve gezinti çubuğuna tıklayın. Sağ tarafındaki HTML kod stilleri bölmesinde seçilen öğeye uygulanan CSS stilleri ile bir listesini görürsünüz.

    > [!NOTE]
    > Üst site düzenini bir parçası olduğundan, Page Inspector ayrıca açılır \_Layout.cshtml dosyasını ve kod kesiminin etkilenen Vurgula.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Stiller ve seçilen öğenin kaynak dosyaları bulma*
11. Etkin geçiş İnceleme işaretçisiyle mavi öne çıkan çubuğunun altındaki fare işaretçisini ve Yarım Daire tıklayın.

    ![Bir öğenin seçilmesi](using-page-inspector-in-visual-studio-2012/_static/image10.png "öğe seçme")

    *Bir öğe seçme*
12. Stilleri bölmesinde bulun **arka plan resmi** altında madde **.main içeriği** grubu. **Onay kutusunu temizleyin** **arka plan resmi** ne olacağına bakalım. Tarayıcı hemen değişiklikleri yansıtır ve dairenin gizli fark edeceksiniz.

    > [!NOTE]
    > Sayfa denetçisi stilleri sekmesinde uyguladığınız değişiklikler, özgün stil etkilemez. Stilleri işaretini kaldırın veya sayıda istediğiniz, ancak sayfa yenilendikten sonra kurulacaktır değerlerini değiştirin.

    ![CSS stilleri devre dışı bırakma ve etkinleştirme](using-page-inspector-in-visual-studio-2012/_static/image11.png "etkinleştirme ve CSS stilleri devre dışı bırakma")

    *CSS stilleri devre dışı bırakma ve etkinleştirme*
13. Şimdi tıklayın '**buraya logonuz**' İnceleme modu kullanarak üstbilgi metni.
14. İçinde **stilleri** sekmesinde, bulmak **yazı tipi boyutu** CSS özniteliği altında **.site başlık** grubu. Öznitelik değeri çift tıklayın ve 2.3 em değeriyle **3 em**ve tuşuna **ENTER**. Başlık büyük göründüğünü dikkat edin.

    ![Sayfa denetçisi CSS değerlerini değiştirmelerini](using-page-inspector-in-visual-studio-2012/_static/image12.png "değiştirme CSS değerleri, sayfa denetçisi")

    *Sayfa denetçisi CSS değerleri değiştirme*
15. Tıklayın **stilleri İzle** sayfa denetçisi sağ bölmede bulunan sekmesinden. Bu öznitelik ada göre sıralanmış seçim, uygulanan stiller görmek için alternatif bir yoludur.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Seçili öğenin CSS stilleri izleme*
16. Başka bir sayfa denetçisi Düzen bölmesi özelliğidir. Gezinti çubuğunu seçin ve ardından İnceleme modu kullanırken **Düzen** sağ bölmede sekme. Seçili öğenin tam boyutunu yanı sıra, uzaklığı, kenar boşluğu, doldurma ve kenarlık boyutuna görürsünüz. Bu görünümden değerlerini de değiştirebilirsiniz dikkat edin.

    ![Sayfa denetçisi öğesi düzende](using-page-inspector-in-visual-studio-2012/_static/image14.png "sayfa denetçisi öğesi düzeni")

    *Sayfa denetçisi öğesi düzeni*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Görev 2 - bulma ve stil sorunlarını Fotoğraf Galerisi

Nasıl Visual Studio'nun önceki sürümleri ile Web sayfaları sorunları tanılamak? Hata ayıklama araçları, Internet Explorer Geliştirici Araçları veya Firebug gibi Visual Studio IDE dışında çalışan web büyük olasılıkla bilmiyorsanız olursunuz. Tarayıcılar, yalnızca HTML, anlamak komut dosyası ve temel alınan bir framework oluşturulacak HTML oluştururken stilleri. Bu nedenle, genellikle tüm sitenin nasıl web sayfaları gibi bakmak için dağıtmanız gerekir.

Algılamak ve web sitenizi bir sorunu gidermek istediğinizde bu adımları izlediğiniz büyük olasılıkla:

1. Visual Studio'dan çözümü çalıştırın veya sayfa üzerinde web sunucusu dağıtın.
2. Geliştirici Araçları, kullanma veya yalnızca açık kaynak kodu ve stilleri ve sorunu eşleştirmeye tarayıcıda açın. Dosyaları dahil bulmak için kullandığınız &quot;arama&quot; veya &quot;dosyalarında arama&quot; adıyla style sınıflarının özellikleri.
3. Hata algılandığında, Web tarayıcı ve sunucu durdurun.
4. Tarayıcı önbelleğini temizleyin.
5. Bir düzeltme uygulamak için Visual Studio'ya geri dönün. Test adımları yineleyin.

ASP.NET MVC 4'te hiçbir gerçek WYSIWYG gibi sonraki bir aşamasında üzerinde çalışan veya web uygulaması dağıtma sonra stili sorunların çoğu algılanır. Şimdi, sayfa denetçisi ile herhangi bir sayfa çözümü çalıştırmadan önizlemek mümkündür.

Bu görevde, sayfa Denetçisi'ni kullanın ve Fotoğraf Galerisi uygulama bazı sorunları giderin.

1. Sayfa Denetçisi'ni kullanarak **kaydetme** ve **oturum** başlığının sol tarafında bağlantılar.

    Bağlantılar, sağdaki beklenen yerde görüntülenmez ve bir madde işaretli liste gibi gösterilen dikkat edin. Şimdi sağdaki bağlantılar Hizala ve buna göre bunları bilgisinin stilini değiştirme.

    ![Kaydolun ve günlük bağlantıları bulma](using-page-inspector-in-visual-studio-2012/_static/image15.png "kaydı ve günlük bağlantıları bulma")

    *Kaydolun ve günlük bağlantıları bulma*
2. Seçili geçiş İnceleme modu ile yakın ancak üzerinde değil, kodunu açmak için Kaydet bağlantısına tıklayın.

    Kaynak kodu bağlantıları bulunan bildirimi  **\_LoginPartial.cshtml** Index.cshtml dosyası ya da \_ilk yerde görünebilir yerlerdir Layout.cshtml,. Doğru kaynak dosyasına doğrudan yerleştirildi.
3. İçinde **stilleri** sekmesinde bulun ve tıklayın **<section> #login</section>** bu bağlantıların HTML kapsayıcı öğe.

    Dikkat **#login** stil bulunan otomatik olarak **Site.css** tıkladıktan sonra. Ayrıca, kodu artık vurgulanır.

    ![CSS stilleri seçerek](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS stilleri seçme")

    *CSS stilleri seçme*
4. Açıklamadan çıkarın **text-align** özniteliği vurgulanmış kodu açma ve kapatma karakterleri kaldırarak ve Kaydet **Site.css** dosya.

    Geçerli sayfayı oluşturan tüm farklı dosyalarını sayfa denetçisi farkındadır ve bu dosyalardan herhangi biri değiştiğinde algılayabilir. Geçerli sayfasını tarayıcıda kaynak dosyaları ile eşitlenmemiş olduğunda sizi uyarır.
5. Sayfa denetçisi tarayıcıda, sayfayı yeniden yüklemek için Adres çubuğunun altında bulunan çubuğu tıklayın.

    ![Sayfayı yeniden yüklemeyi](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Sayfayı yeniden yüklemeyi*

    Sağ taraftaki bağlantıları sunulmuştur, ancak yine de bir madde işaretli liste gibi görünürler. Şimdi, madde işaretlerini kaldırın ve bağlantıları yatay Hizala.

    ![Güncelleştirilmiş sayfası](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Güncelleştirilmiş sayfası*
6. İnceleme modu kullanarak, seçin herhangi birini **&lt;li&gt;** içeren öğelerini &quot;kaydetme&quot; ve &quot;oturum&quot; bağlantıları. ' A tıklayarak  **&lt;bölümü&gt; #login** öğe erişimi **Styles.css** kod.

    ![Stilini bulma](using-page-inspector-in-visual-studio-2012/_static/image19.png "stilini bulma")

    *Stilini bulma*
7. İçinde **Style.css**, için kodun açıklamasını kaldırın **#login li** öğeleri. Eklemekte olduğunuz stil madde işareti gizleme ve öğeleri yatay olarak görüntüler.

    ![Oturum açma bağlantıları yeniden tasarıma](using-page-inspector-in-visual-studio-2012/_static/image20.png "oturum açma bağlantıları yeniden tasarıma")

    *Oturum açma bağlantıları yeniden tasarıma*
8. Kaydet **Style.css** dosya ve sayfayı yeniden yüklemek için aşağıdaki adresi bulunan çubuğu kez tıklayın. Bağlantıları doğru bir şekilde görüntülendiğine dikkat edin.

    ![Bağlantılar için sağ tarafı hizalı](using-page-inspector-in-visual-studio-2012/_static/image21.png "bağlantıları sağa hizalı")

    *Sağa hizalı bağlantıları*
9. Son olarak, üst bilgi başlığı değişecektir. İnceleme modu reklamlara **buraya logonuz** metin ve get, oluşturduğu kaynak koduna.
10. Bulunduğunuz artık  **\_Layout.cshtml**, Değiştir '**buraya logonuz**'metin'**Fotoğraf Galerisi**'. Kaydet ve sayfa denetçisi tarayıcı güncelleştirin.

    ![Yeni bir başlık atama](using-page-inspector-in-visual-studio-2012/_static/image22.png "yeni bir başlık atama")

    *Yeni bir başlık atama*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Fotoğraf Galerisi sayfası güncelleştirildi*
11. Son olarak, doldurduktan **Fotografgalerisi** proje ve ENTER tuşuna **F5** uygulamayı çalıştırmak için. Tüm değişikliklerin istendiği gibi çalışması denetleyin.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Alıştırma 2: WebForms projelerinde sayfa denetçisini kullanma

Bu alıştırmada, Önizleme ve sayfa Denetçisi'ni kullanarak bir WebForms çözüme hata ayıklaması öğreneceksiniz. İlk işlemin hatalarını ayıklamaya Web kolaylaştırmak sayfa denetçisi özellikleri öğrenmek için bir kısa turu aracı gerçekleştirir. Ardından, stil sorunlarını içeren bir web sayfasında çalışır. Sorun oluşturan kaynak kodunu bulmak için sayfa Denetçisi'ni kullanın ve Düzelt öğreneceksiniz.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Görev 1 - sayfa Denetçisi'ni keşfetme

Bu görevde bir Fotoğraf Galerisi gösteren bir WebForms proje bağlamında sayfa denetçisi özelliklerini kullanmayı öğreneceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-WebForms/başlangıç/** klasör.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Çözüm Gezgini'nde bulun **Default.aspx** sayfasında sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.

    ![Sayfa denetçisi ile default.aspx açma](using-page-inspector-in-visual-studio-2012/_static/image24.png "Default.aspx sayfa denetçisi ile açma")

    *Sayfa denetçisi ile açılış Default.aspx*
3. Sayfa denetçisi penceresi Default.aspx gösterir.

    ![Sayfa denetçisi içinde default.aspx görüntüleme](using-page-inspector-in-visual-studio-2012/_static/image25.png "sayfa denetçisi içinde Default.aspx görüntüleme")

    *Sayfa denetçisi içinde default.aspx görüntüleme*

    Sayfa denetçisi aracı Visual Studio ortamınıza tümleşiktir. Seçilen kod gösterilir güçlü bir HTML Profil Oluşturucu ile birlikte katıştırılmış bir tarayıcı Inspector'ı içerir. Sayfalarınızın nasıl göründüğünü görmek için çözümü çalıştırmak zorunda değilsiniz dikkat edin.

    > [!NOTE]
    > Sayfa denetçisi tarayıcı genişliğini açılan sayfanın genişliğine'dan olduğunda, sayfanın düzgün tarafından görülmez. Bu durumda, sayfa denetçisi genişliğini ayarla.
4. Tıklayın **dosyaları** sayfa denetçisi sekmesindedir.

    İşlenen varsayılan sayfa oluşturmakta olduğunuz tüm kaynak dosyaları görürsünüz. Bu, özellikle de kullanıcı denetimleri ve ana sayfalar ile çalışırken, bir bakışta tüm öğeleri tanımlamak için yararlı bir özelliktir. Her dosya için da gidebilirsiniz dikkat edin.

    ![Dosyalar sekmesi](using-page-inspector-in-visual-studio-2012/_static/image26.png "dosyalar sekmesi")

    *Dosyalar sekmesi*
5. Tıklayın **denetleme modunu Aç/Kapat** sekmelerin sol tarafında bulunan düğmesini.

    Bu araç, sayfanın herhangi bir öğe seçin ve HTML kodu ve .aspx kaynağına bakın olanak tanır.

    ![İki durumlu İnceleme modu düğme](using-page-inspector-in-visual-studio-2012/_static/image27.png "denetleme modunu Aç/Kapat düğmesi")

    *İki durumlu İnceleme modu düğme*
6. Sayfa denetçisi tarayıcıda sayfa öğeleri üzerinde fareyi hareket ettirin. İşlenen sayfanın herhangi bir bölümü fare işaretçisini taşıyın, ancak öğe türü görüntülenir ve Visual Studio Düzenleyicisi'nde, karşılık gelen kaynak biçimlendirmeyi veya kodu vurgulanır.

    ![Denetleme modunda, eylem](using-page-inspector-in-visual-studio-2012/_static/image28.png "uygulamada İnceleme modu")

    *Denetleme modunda, eylem*

    > [!NOTE]
    > Sayfa denetçisi pencerenin en üst düzeye çıkarabilirim veya kaynak kodunu gösteren bir önizleme sekmesinde görmeniz mümkün olmayacaktır. Sayfa denetçisi öğesinde tıklarsanız, büyütüldüğünde seçimin kaynak kodu görünür ancak sayfa denetçisi penceresini gizler.

    Dikkatini ödeme yaparsanız **Default.aspx** dosya, seçili öğenin oluşturduğu kaynak kod bölümünü vurgulanır görürsünüz. Bu özellik, kodu erişmek için doğrudan ve hızlı bir şekilde sağlayarak uzun kaynak dosyalarının sürümü kolaylaştırır.

    ![Öğeleri inceleyerek](using-page-inspector-in-visual-studio-2012/_static/image29.png "öğeleri inceleniyor")

    *Öğeleri inceleniyor*
7. Tıklayın **denetleme modunu Aç/Kapat** düğmesine (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), sayfa denetçisi sekmeleri imleç devre dışı bırakmak için bulunur.
8. Seçin **HTML** sayfa denetçisi tarayıcıda İşlenmiş HTML kodunu görüntülemek için sekmesinde.
9. HTML kodunda Koala bağlantısını içeren bir liste öğesi bulun ve seçin.

    Kod seçtiğinizde, ilgili çıkış otomatik olarak vurgulanan tarayıcı olduğuna dikkat edin. Bu özellik, bir HTML bloğunu sayfada nasıl oluşturulacağını görmek kullanışlıdır.

    ![Sayfanın HTML öğesini seçerek](using-page-inspector-in-visual-studio-2012/_static/image31.png "sayfasında bir HTML öğesi seçme")

    *Sayfanın HTML öğesi seçme*
10. Tıklayın **denetleme modunu Aç/Kapat** etkinleştirmek için düğmeye *İnceleme modu* ve gezinti çubuğuna tıklayın. Sağ tarafındaki HTML kod stilleri bölmesinde seçilen öğeye uygulanan CSS stilleri ile bir listesini görürsünüz.

    > [!NOTE]
    > üst site düzenini bir parçası olduğundan, Page Inspector ayrıca Site.Master dosyasını açın ve etkilenen kod kesiminin vurgulayın.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "stilleri ve seçilen öğenin kaynak dosyaları bulma")

    *Stiller ve seçilen öğenin kaynak dosyaları bulma*
11. Etkin geçiş İnceleme işaretçisiyle fare işaretçisini menü çubuğunun altında ve boş Yarım Daire tıklayın.

    ![Bir öğenin seçilmesi](using-page-inspector-in-visual-studio-2012/_static/image33.png "öğe seçme")

    *Bir öğe seçme*
12. Stilleri bölmesinde bulun **arka plan resmi** altında madde **.main içeriği** grubu. **Onay kutusunu temizleyin** **arka plan resmi** ne olacağına bakalım. Tarayıcı hemen değişiklikleri yansıtır ve dairenin gizli fark edeceksiniz.

    > [!NOTE]
    > Sayfa denetçisi stilleri sekmesinde uyguladığınız değişiklikler, özgün stil etkilemez. Stilleri işaretini kaldırın veya sayıda istediğiniz, ancak sayfa yenilendikten sonra kurulacaktır değerlerini değiştirin.

    ![CSS styles2 devre dışı bırakma ve etkinleştirme](using-page-inspector-in-visual-studio-2012/_static/image34.png "etkinleştirme ve CSS stilleri devre dışı bırakma")

    *CSS stilleri devre dışı bırakma ve etkinleştirme*
13. Şimdi tıklayın '**,** **logosu burada '** İnceleme modu kullanarak üstbilgi metni.
14. İçinde **stilleri** sekmesinde, bulmak **yazı tipi boyutu** CSS özniteliği altında **.site başlık** grubu. Öznitelik bir kez değerini düzenlemek için çift tıklayın. Değeri ile değiştirin 2.3em **3em**, ve ardından ENTER tuşuna basın. Başlık büyük göründüğünü dikkat edin.

    ![Sayfa Inspector2 CSS değerlerini değiştirmelerini](using-page-inspector-in-visual-studio-2012/_static/image35.png "değiştirme CSS değerleri, sayfa denetçisi")

    *Sayfa denetçisi CSS değerleri değiştirme*
15. Tıklayın **stilleri İzle** sayfa denetçisi sağ bölmede bulunan sekmesinden. Bu öznitelik ada göre sıralanmış seçim, uygulanan stiller görmek için alternatif bir yoludur.

    ![CSS stilleri izleme seçili öğenin](using-page-inspector-in-visual-studio-2012/_static/image36.png "seçili öğenin CSS stilleri izleme")

    *Seçili öğenin CSS stilleri izleme*
16. Başka bir sayfa denetçisi Düzen bölmesi özelliğidir. Gezinti çubuğunu seçin ve ardından İnceleme modu kullanırken **Düzen** sağ bölmede sekme. Seçili öğenin tam boyutunu yanı sıra, uzaklığı, kenar boşluğu, doldurma ve kenarlık boyutuna görürsünüz. Bu görünümden değerlerini de değiştirebilirsiniz dikkat edin.

    ![Sayfa denetçisi öğesi düzende](using-page-inspector-in-visual-studio-2012/_static/image37.png "sayfa denetçisi öğesi düzeni")

    *Sayfa denetçisi öğesi düzeni*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Görev 2 - bulma ve stil sorunlarını Fotoğraf Galerisi

Nasıl Visual Studio'nun önceki sürümleri ile Web sayfaları sorunları tanılamak? Hata ayıklama araçları, Internet Explorer Geliştirici Araçları veya Firebug gibi Visual Studio IDE dışında çalışan web büyük olasılıkla bilmiyorsanız olursunuz. Tarayıcılar, yalnızca HTML, anlamak komut dosyası ve temel alınan bir framework oluşturulacak HTML oluştururken stilleri. Bu nedenle, genellikle tüm sitenin nasıl web sayfaları gibi bakmak için dağıtmanız gerekir.

Algılamak ve web sitenizi bir sorunu gidermek istediğinizde bu adımları izlediğiniz büyük olasılıkla:

1. Visual Studio'dan çözümü çalıştırın veya sayfa üzerinde web sunucusu dağıtın.
2. Geliştirici Araçları, kullanma veya yalnızca açık kaynak kodu ve stilleri ve sorunu eşleştirmeye tarayıcıda açın. Dosyaları dahil bulmak için kullandığınız &quot;arama&quot; veya &quot;dosyalarında arama&quot; adıyla style sınıflarının özellikleri.
3. Hata algılandığında, Web tarayıcı ve sunucu durdurun.
4. Tarayıcı önbelleğini temizleyin.
5. Bir düzeltme uygulamak için Visual Studio'ya geri dönün. Test adımları yineleyin.

Bazı stil sorunlarını olduğundan Hayır ASP.NET WebForms gerçek WYSIWYG, sonraki bir aşamasında üzerinde çalışan veya dağıtma sonra algılanır. Şimdi, sayfa denetçisi ile herhangi bir sayfa çözümü çalıştırmadan önizlemek mümkündür.

Bu görevde, sayfa denetçisi Fotoğraf Galerisi uygulama ile ilgili bazı sorunları düzeltmek için kullanır. Aşağıdaki adımlarda, algılayın ve hızlı bir şekilde üst bilgisinde bazı basit stil sorun giderin.

1. Sayfa İnceleme kullanarak **kaydetme** ve **oturum** başlığının sol tarafında bağlantılar.

    Bağlantıyı sağ taraftaki beklenen yerde görüntülenmez dikkat edin. Şimdi bağlantı Sağa Hizala ve bilgisinin stilini değiştirme, buna göre.

    ![Sol taraftaki konumlandırılmış bağlantısında oturum](using-page-inspector-in-visual-studio-2012/_static/image38.png "soldaki konumlandırılmış bağlantısında oturum")

    *Sol taraftaki konumlandırılmış oturum bağlantısı*
2. Seçili geçiş İnceleme modu ile kodunu açmak için oturum aç bağlantısını seçin.

    Bağlantı kaynak kodu bulunan bildirimi **Site.Master** dosya, nelerin Default.aspx sayfasında ilk yerde görünebilir; doğrudan doğru kaynak dosyada yerleştirildi.
3. İçinde **stilleri** sekmesinde bulun ve tıklayın  **&lt;bölümü&gt; #login** bu bağlantıların HTML kapsayıcı öğe.

    Dikkat **#login** stil bulunan otomatik olarak **Site.css** tıkladıktan sonra. Ayrıca, kodu artık vurgulanır.

    ![CSS stilleri seçerek](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS stilleri seçme")

    *CSS stilleri seçme*
4. Açıklamadan çıkarın **text-align** özniteliği vurgulanmış kodu açma ve kapatma karakterleri kaldırarak ve Kaydet **Site.css** dosya.

    Geçerli sayfayı oluşturan tüm farklı dosyalarını sayfa denetçisi farkındadır ve bu dosyalardan herhangi biri değiştiğinde algılayabilir. Geçerli sayfasını tarayıcıda kaynak dosyaları ile eşitlenmemiş olduğunda sizi uyarır.
5. Sayfa denetçisi tarayıcıda, değişiklikleri kaydetmek ve sayfayı yeniden yüklemek için Adres çubuğunun altında bulunan çubuğu tıklayın.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Sayfayı yeniden yüklemeyi*

    Sağ taraftaki bağlantıları sunulmuştur, ancak yine de bir madde işaretli liste gibi görünürler. Şimdi, madde işaretlerini kaldırın ve bağlantıları yatay Hizala.

    ![Güncelleştirilmiş sayfası](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Güncelleştirilmiş sayfası*
6. İnceleme modu kullanarak, seçin herhangi birini **&lt;li&gt;** içeren öğelerini &quot;kaydetme&quot; ve &quot;oturum&quot; bağlantıları. ' A tıklayarak  **&lt;bölümü&gt; #login** öğe erişimi **Styles.css** kod.

    ![Stilini bulma](using-page-inspector-in-visual-studio-2012/_static/image42.png "stilini bulma")

    *Stilini bulma*
7. İçinde **Style.css**, için kodun açıklamasını kaldırın **#login li** öğeleri. Eklemekte olduğunuz stil madde işareti gizleme ve öğeleri yatay olarak görüntüler.

    ![Oturum açma bağlantıları yeniden tasarıma](using-page-inspector-in-visual-studio-2012/_static/image43.png "oturum açma bağlantıları yeniden tasarıma")

    *Oturum açma bağlantıları yeniden tasarıma*
8. Kaydet **Style.css** dosya ve sayfayı yeniden yüklemek için aşağıdaki adresi bulunan çubuğu kez tıklayın. Bağlantıları doğru bir şekilde görüntülendiğine dikkat edin.

    ![Bağlantılar için sağ tarafı hizalı](using-page-inspector-in-visual-studio-2012/_static/image44.png "bağlantıları sağa hizalı")

    *Sağa hizalı bağlantıları*
9. Son olarak, üst bilgi başlığı değişecektir. Arama yerine '**logonuz buraya gelir '** dosyalardaki metni metin tıklayın ve onu oluşturan kaynak koduna almak için denetleme modunu kullan.

    ![Site başlığı bulma](using-page-inspector-in-visual-studio-2012/_static/image45.png "site başlığı bulma")

    *Site başlığı bulma*
10. Bulunduğunuz artık **Site.Master**, Değiştir '**buraya logonuz**'metin'**Fotoğraf Galerisi**'. Kaydet ve sayfa denetçisi tarayıcı güncelleştirin.

    ![Fotoğraf Galerisi sayfası güncelleştirildi](using-page-inspector-in-visual-studio-2012/_static/image46.png "Fotoğraf Galerisi sayfası güncelleştirildi")

    *Fotoğraf Galerisi sayfası güncelleştirildi*
11. Son olarak basın **F5** tüm değişiklikleri beklendiği gibi çalışması kullanıma uygulamayı çalıştırmak için.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarı tamamlayarak yeniden derleyin ve Web sitesi bir tarayıcıda çalıştırmak zorunda kalmadan, Web uygulamanızın önizlemesini görüntülemek için sayfa denetçisini kullanmak nasıl modellerin. Ayrıca, nasıl hızlı bir şekilde hataları bulmalarına ve kaynak koduna işlenmiş çıkışı doğrudan erişerek modellerin.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Web kutucuğu için VS Express*
