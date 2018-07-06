---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Uygulamalı Laboratuvar: Visual Studio 2013 Web Araçları | Microsoft Docs'
author: rick-anderson
description: Visual Studio için mükemmel bir geliştirme ortamıdır. AĞ tabanlı Windows ve web projeleri. Kolayca için kullanılabilir bir güçlü metin düzenleyicisi içerdiği...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807534"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Uygulamalı Laboratuvar: Visual Studio 2013 Web Araçları
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](http://aka.ms/webcamps-training-kit)

> Visual Studio için mükemmel bir geliştirme ortamıdır. AĞ tabanlı Windows ve web projeleri. Bu kolaylıkla proje olmadan tek başına dosyalarını düzenlemek için kullanılabilecek bir güçlü metin düzenleyicisi içerir.
> 
> Her dosyayı düzenlerken visual Studio tam özellikli ayrıştırma ağacı tutar. Bu, Visual Studio geliştirme deneyimini çok daha hızlı ve daha eğlenceli yaparken eşsiz otomatik tamamlama ve belge tabanlı eylemler sağlamak sağlar. Bu özellikler, HTML ve CSS belgeleri özellikle güçlüdür.
> 
> Tüm bu gücü de yeni güçlü özellikler düzenleyicilerle gereksinimlerinize uyacak şekilde genişletmek basit hale uzantıları için mevcut değildir. Web Essentials (çoğunlukla) web ile ilgili geliştirmeler için Visual Studio koleksiyonudur. Çok sayıda yeni IntelliSense tamamlamaları (özellikle de CSS), yeni tarayıcı bağlantısı özellikleri, otomatik içerir JSHint JavaScript dosyaları, HTML, CSS ve modern web geliştirme için gerekli olan diğer birçok özellik için yeni uyarılar.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials içinde bulunan yeni HTML düzenleyicisi özelliklerini kullanma
- Renk Seçici ve tarayıcı matris araç ipucu gibi Web Essentials dahil yeni CSS Düzenleyicisi özelliklerini kullanma
- Tüm HTML öğeleri için ayıklama dosya ve IntelliSense gibi Web Essentials içindeki yeni JavaScript Düzenleyicisi özelliklerini kullanma
- Exchange verilerini tarayıcı arasındaki tarayıcı bağlantısını kullanarak Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) veya üzeri
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.

1. Bir Windows Explorer penceresi açın ve Laboratuvar için Gözat **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırın ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemini başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim kutusunu gösteriliyorsa, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların etkinleştirdiğinizden emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıklarını kullanma

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan bir başlangıç çözüm eşlik **başlamak** her alıştırma diğerlerinden takip etmenize olanak tanıyan çalışma klasörü. Lütfen bir alıştırma sırasında eklenen kod parçacıkları bu çözümleri başlangıç eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Ayrıca bulabilirsiniz bir alıştırma için kaynak kod içinde bir **son** karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren klasör. Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [Tarayıcı bağlantısı ve Web Essentials ile çalışma](#Exercise1)
2. [Kod parçacıkları ve IntelliSense yararlanarak](#Exercise2)

> [!NOTE]
> Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir. Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Alıştırma 1: Tarayıcı bağlantısı ve Web Essentials ile çalışma

**Web Essentials** çeşitli çoğunlukla web geliştirme deneyimini çok daha hızlı ve daha eğlenceli hale getirmeye odaklanırken, modern web geliştirme için kullanışlı özellikler ekleyen bir Visual Studio uzantısıdır. Visual Studio uzantı Galerisi Web Essentials'ı yükleyebilirsiniz.

**Tarayıcı bağlantısı** , Visual Studio ve web uygulaması arasında veri değişimi için Visual Studio IDE ve herhangi bir açık tarayıcı arasında bir kanal sağlar Visual Studio 2013'te bulunan yeni bir özelliktir. Web Essentials DOM nesne modeli ve web sayfalarınıza doğrudan tarayıcınızdan CSS stillerini işlemek için araçları ile tarayıcı bağlantısı genişletir.

Bu alıştırmada, bazı tarafından desteklenen özellikleri inceleyeceksiniz **Web Essentials** ve **tarayıcı bağlantısı** basit bir test sayfası geliştirmek için.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Görev 1 - proje birden çok tarayıcılarında çalışan

Bu görevde, çapraz tarayıcı test etmek için yararlı olan tek bir seferde birden çok tarayıcılarda çalıştırmak için web uygulaması yapılandıracaksınız.

1. Açık **Microsoft Visual Studio**.
2. İçinde **dosya** menüsünde **açık | Proje/çözüm...**  ve **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** içinde **kaynak** laboratuvarı (C:\WebCampsTK\HOL\VSWebTooling\Source) klasörü. Seçin **Begin.sln** tıklatıp **açık**.
3. Visual Studio araç çubuğunda, tarayıcı menüsünü genişletin ve seçin **şununla Gözat...** .

    ![Şununla Gözat seçeneğine](visual-studio-2013-web-tools/_static/image1.png "ile tarayıcı menüde Gözat...")

    *Menü seçeneği ile Gözat*
4. İçinde **şununla Gözat** iletişim kutusu, select **Google Chrome** ve **Internet Explorer** basılı tutarak **CTRL** basıp tıklayın **Varsayılan olarak ayarla**.

    ![İletişim kutusu Gözat](visual-studio-2013-web-tools/_static/image2.png "iletişim kutusuyla Gözat")

    *Birden çok varsayılan tarayıcı seçme*
5. Artık varsayılan tarayıcı olarak Google Chrome hem de Internet Explorer görüntülenmesi gerekir. Tıklayın **iptal** iletişim kutusunu kapatın.

    ![Google Chrome ve Internet Explorer'ı varsayılan tarayıcı olarak](visual-studio-2013-web-tools/_static/image3.png "Google Chrome ve Internet Explorer varsayılan tarayıcı")

    *Google Chrome ve Internet Explorer'ı varsayılan tarayıcı olarak*

    > [!NOTE]
    > Varsayılan tarayıcı yapılandırdıktan sonra **birden çok tarayıcı** tarayıcı menüde seçeneğinin işaretli.
    > 
    > ![Birden çok tarayıcı](visual-studio-2013-web-tools/_static/image4.png "birden çok tarayıcı")
6. Tuşuna **CTRL** + **F5** uygulamayı hata ayıklama olmadan çalıştırın.
7. İki tarayıcı penceresini açtığınızda, biri diğerinin üstüne güncelleştirmeleri aynı anda hem tarayıcılarda görmek için yerleştirin. Tarayıcılar açık mavi dikdörtgen ile bir web sayfası görüntülemelidir.

    ![Bir tarayıcı üst üste yerleştirerek](visual-studio-2013-web-tools/_static/image5.png "üste bir tarayıcı yerleştirme")

    *Bir tarayıcı üst üste yerleştirerek*
8. Tarayıcılar kapatmayın. Bunları sonraki görevde kullanır.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Görev 2 - kullanarak Zen HTML öğeleri oluşturmak için kodlama

**Zen kodlama** eklentisi hızlı HTML, XML, XSL (veya diğer yapılandırılmış kod biçimi) için kod yazma ve düzenleme bir düzenleyicidir. Bu eklenti setinin ifadelere - CSS Seçici için benzer - HTML kodu genişletin olanak tanıyan güçlü kısaltması altyapısıdır. Zen kodlama Seçici söz dizimini kullanarak bir CSS HTML stil yazmak için hızlı bir yoludur.

Bu alıştırmada, sorunun seçenekleri temsil eden HTML düğmeleri üretmek için Web Essentials tarafından sağlanan Zen kodlama işlevini kullanır.

1. Visual Studio'ya geçiş yapın.
2. Açık **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.
3. Değiştirin **&lt;!--TODO: Buraya--seçenekleri ekleme&gt;** yorum aşağıdaki kod ve basın **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. HTML kod genişletilmiştir.

    ![HTML Genişletilmiş](visual-studio-2013-web-tools/_static/image6.png "genişletilmiş HTML")

    *Genişletilmiş HTML*

    > [!NOTE]
    > Zen kodlama sözdizimi hakkında daha fazla bilgi için aşağıdakilere bakın [makale](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Tıklayın **bağlı tarayıcıların yenilenmesine** düğmesi her iki tarayıcılar güncelleştirilecek.

    ![Bağlı tarayıcıların yenilenmesine](visual-studio-2013-web-tools/_static/image7.png "bağlı tarayıcıların yenilenmesine")

    *Bağlı tarayıcıların yenilenmesine*

    ![Internet Explorer - sayfası güncelleştirildi ile dört düğme](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - sayfası dört düğme ile güncelleştirildi")

    *Internet Explorer - sayfası ile dört düğmeleri güncelleştirildi.*

    ![Google Chrome - sayfası güncelleştirildi ile dört düğme](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - sayfası dört düğme ile güncelleştirildi")

    *Google Chrome - sayfası ile dört düğmeleri güncelleştirildi.*
6. Visual Studio'ya geçiş yapın.
7. Düğmeleri sayfaya eklenen, ancak yine de bir şablonu soru eklemeniz gerekir. Bunu yapmak için yeni bir özellik olarak adlandırılan Web Essentials kullanacağınız **Lorem Ipsum Oluşturucu**. Bulun **div** öğeyle **sınıfı** özniteliği **ön**.
8. İlk alt öğesi olarak aşağıdaki kodu ekleyin **div**basın **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. HTML kod genişletilmiştir.

    ![Lorem Ipsum otomatik olarak oluşturulan](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum dinle")

    *Lorem Ipsum dinle*

    > [!NOTE]
    > Zen kodlama işleminin bir parçası olarak artık doğrudan HTML Düzenleyicisi'nde Lorem Ipsum kod oluşturabilirsiniz. Yazmanız yeterlidir **lorem** ve isabet **sekmesini** ve bir 30 word Lorem Ipsum metin eklenir. Örneğin *lorem10* 10 Lorem Ipsum sözcük ekler.
10. Adlı Web Essentials başka bir yeni özelliği kullanarak soruyu üst kısmında bir logo ekleyeceksiniz **Lorem piksel Oluşturucu**. İlk alt öğesi olarak aşağıdaki kodu ekleyin **div** öğeyle **kapsayıcı** olarak **sınıfı** değeri ve tuşuna **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. HTML kod genişletmeniz gerekir.

    ![Lorem piksel otomatik olarak oluşturulan](visual-studio-2013-web-tools/_static/image11.png "Lorem piksel dinle")

    *Lorem piksel dinle*

    > [!NOTE]
    > Zen kodlama işleminin bir parçası olarak doğrudan HTML Düzenleyicisi'nde Lorem piksel kod oluşturabilirsiniz. Yazmanız yeterlidir **PIX 200 x 200 hayvanlar** ve isabet **sekmesini** ve **img** bir donatarak 200 x 200 görüntüsü etiketiyle eklenir. Daha fazla bilgi için [Lorem piksel](http://www.lorempixel.com).
12. Tıklayın **bağlı tarayıcıların yenilenmesine** düğmesi her iki tarayıcılar güncelleştirilecek.

    ![Internet Explorer - otomatik olarak oluşturulan resim ve metin](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - otomatik olarak oluşturulan resim ve metin")

    *Internet Explorer - otomatik olarak oluşturulan resim ve metin*

    ![Google Chrome - otomatik olarak oluşturulan resim ve metin](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - otomatik olarak oluşturulan resim ve metin")

    *Google Chrome - otomatik olarak oluşturulan resim ve metin*

    > [!NOTE]
    > Görüntü rastgele kod parçacığı eklerken seçildiğinden, tarayıcılarda gösterilen görüntüyü farklı olabilir.
13. Tarayıcılar kapatmayın. Bunları sonraki görevde kullanır.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Görev 3 - Style özelliği güncelleştiriliyor

Bu görevde, tarayıcı bağlantının kullanacağınız **İnceleme modu** özelliği belirli DOM öğesi oluşturulan burada tam konumu algılama ve ardından o öğenin kullanarak Web tarafından sağlanan bir renk seçici renk özelliğini güncelleştirin Temel bileşenler.

1. Internet Explorer tarayıcınızda basın **CTRL** + **ALT** + **miyim** denetleme modunu etkinleştirmek için.
2. Açık mavi kenarlığı taşıyın ve tıklayın.

    ![Açık mavi kenarlığı üzerinde işaretçiyi taşıma](visual-studio-2013-web-tools/_static/image14.png "açık mavi kenarlığı üzerinde işaretçiyi taşıma")

    *Açık mavi kenarlığı üzerinde işaretçiyi taşıma*
3. Visual Studio'ya geçiş yapın. Seçtiğiniz tarayıcıda HTML öğesi Visual Studio HTML Düzenleyicisi'nde de nasıl seçili dikkat edin.

    ![Seçili Visual Studio HTML düzenleyicide HTML öğesi](visual-studio-2013-web-tools/_static/image15.png "seçili Visual Studio HTML düzenleyicide HTML öğesi")

    *Seçili Visual Studio HTML düzenleyicide HTML öğesi*
4. Şimdi güncelleştirecek **ön** seçilen öğenin stilini değiştirmek için kullanılan CSS sınıfı. Bunu yapmak için basın **CTRL** + **,** açmak için **gitmek için** arama kutusu. Tür **site.css** basın **ENTER** dosyayı açmak için.

    ![Site.css dosyası açılırken](visual-studio-2013-web-tools/_static/image16.png "Site.css dosya açılıyor")

    *Dosya açılırken Site.css*
5. Tuşuna **CTRL** + **F** ve türü **.flip CONTAINTER .front** CSS Seçici bulunamadı.
6. Açık mavi bir kare kenarlık özelliğinde sınıfı renk seçiciyi açmak için tıklayın.

    ![Renk seçiciyi açmak](visual-studio-2013-web-tools/_static/image17.png "Renk Seçici açma")

    *Renk Seçici açma*
7. Köşeli çift ayraç düğmesine tıklayarak renk seçiciyi Genişlet ve yeni bir renk seçin.

    ![Renk Seçici genişletme](visual-studio-2013-web-tools/_static/image18.png "Renk Seçici genişletme")

    *Renk Seçici genişletme*
8. Tuşuna **CTRL** + **ALT** + **ENTER** bağlı tarayıcıların yenilenmesine için.
9. Internet Explorer'a geçin ve kenarlık rengini nasıl değiştiğine dikkat edin.

    ![Internet Explorer - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - kenarlık rengi güncelleştirildi")

    *Internet Explorer - kenarlık rengi güncelleştirildi*
10. Google Chrome geçin ve kenarlık rengini nasıl değiştiğine dikkat edin.

    ![Google Chrome - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - kenarlık rengi güncelleştirildi")

    *Google Chrome - kenarlık rengi güncelleştirildi*
11. Visual Studio'ya geçiş yapın.
12. Sonuna Git **Site.css** dosya ve ENTER tuşuna **CTRL** + **F** bulunacak **.btn** Seçici.
13. Dikkat **- webkit-border-radius** özelliği yeşil olarak çizilir.

    ![btn Seçici - webkit-border-radius özelliği](visual-studio-2013-web-tools/_static/image21.png "btn Seçici - webkit-border-radius özelliği")

    *-webkit-border-radius özelliği btn Seçici*
14. Giriş işaretini de yerleştirin **- webkit-border-radius** özelliği. Mavi bir çizgi, özelliğin ilk sözcüğün ilk harfini altında görünmelidir. Bu **akıllı etiket**.
15. Tuşuna **CTRL** + **.** öneriler menüsünü açın ve tıklayın **standart özelliği (border-radius) eksik Ekle**.

    ![Standart bir özellik önerisi yok Ekle](visual-studio-2013-web-tools/_static/image22.png "Ekle standart bir özellik önerisi yok")

    *Standart bir özellik önerisi eksik Ekle*
16. **Border-radius** özelliği CSS kuralı otomatik olarak eklenir.

    ![Eklenen standart özelliği eksik](visual-studio-2013-web-tools/_static/image23.png "eksik standart özelliği eklendi")

    *Eklenen standart özelliği eksik*
17. İşaretçiyi **border-radius** görüntülenecek özelliği **tarayıcı matris araç ipucu**. **Tarayıcı matris araç ipucu** her tarayıcıda özelliği kullanılabilirliğini gösterir.

    ![Tarayıcı matris araç ipucu](visual-studio-2013-web-tools/_static/image24.png "tarayıcı matris araç ipucu")

    *Tarayıcı matris araç ipucu*
18. Dikkat değerini **border-radius** yine de altı çizili özelliği. Uyarı iletilerini görmek için değerin üzerinde işaretçiyi taşıyın.

    ![Border-radius özellik değeri Uyarı](visual-studio-2013-web-tools/_static/image25.png "Border-radius özellik değeri Uyarısı")

    *Border-radius özellik değeri Uyarısı*
19. Birimini kaldırma **border-radius** araç ipucu tarafından önerilen özellik değeri.
20. Olarak **border-radius** köşeleri olan, kaldırabilirsiniz nasıl yuvarlatılmış kenarlık tanımlamak için standart özelliği **- webkit-border-radius** özelliği ve CSS kuralı değeri.
21. Giriş işaretini de yerleştirin **sözcük kaydırma** özelliği ve akıllı etiket ayrıca altında göründüğüne dikkat edin.
22. Menüsünü açın ve tıklayın **eksik satıcı Özellikleri Ekle**.

    ![Eksik satıcı özellikleri öneri ekleme](visual-studio-2013-web-tools/_static/image26.png "eksik satıcı özellikleri önerisi ekleyin")

    *Eksik satıcı özellikleri önerisi ekleyin*
23. **-Ms sözcük kaydırmayı** özelliği CSS kuralı otomatik olarak eklenir.

    ![Satıcıya özgü özelliği eklendi](visual-studio-2013-web-tools/_static/image27.png "satıcıya özgü özelliği eklendi")

    *Satıcıya özgü özelliği eklendi*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Görev 4 - tarayıcıdan HTML kodunu güncelleştirme

Bu görevde, tarayıcı bağlantının kullanacağınız **tasarım modu** tarayıcıdan DOM nesnesinde düzenlemek ve Visual Studio HTML kaynak dosyasında yapılan değişiklikleri aktarmak için özellik.

1. Google Chrome'da basın **CTRL** + **ALT** + **D** Tasarım modunu etkinleştirmek için.
2. İşaretçiyi **Lorem Ipsum dolor sit amet** etiketleyebilir ve tıklayın.

    ![Soru düzenleme](visual-studio-2013-web-tools/_static/image28.png "soru düzenleme")

    *Sorunun düzenleme*
3. Bir imleç görüntülenmesi gerekir. Orijinal metinle *ben daha uzun bir soru yazdığınızda, nasıl göründüğünü?* ve tuşuna **ESC** Tasarım modundan çıkmak için.

    ![Düzenlenen soru](visual-studio-2013-web-tools/_static/image29.png "soru düzenlendi")

    *Soru düzenlendi*
4. Visual Studio dönün ve açık anahtar **Index.cshtml**, zaten açılmış. Dikkat iç metni **&lt;p&gt;** öğe güncelleştirildi.

    ![Güncelleştirilmiş soru HTML sayfasındaki](visual-studio-2013-web-tools/_static/image30.png "güncelleştirilmiş soru-HTML sayfası")

    *HTML sayfasındaki güncelleştirilmiş soru*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Görev 5 - gözden geçirme SEO ilgili uyarılar

**Arama motoru iyileştirmesi** (SEO) olan bir arama motoru sonuçları listesinde daha yüksek bir Web sitesi derece hale getirme işlemidir. Daha yüksek site derecelendirir ve daha tutarlı bir şekilde listeleniyor, daha fazla ziyaretçiler, arama motorundan site alırsınız. Web Essentials HTML inceler analitik bir araç içerir, raporları sorunları bulundu ve sorunu çözmek için Yardım sağlar.

1. Git **görünümü** menüsüne ve ardından **hata listesi** açmak için **hata listesi** penceresi.

    ![Hata Listesi görünümünde menü](visual-studio-2013-web-tools/_static/image31.png "hata Listesi'nde Görünüm menüsü")

    *Hata listesi Görünüm menüsü*
2. Bildiren bir SEO uyarı olduğuna dikkat edin bir **&lt;meta&gt;** sayfası açıklaması eksik etiketleyin. Sorunu gidermek için SEO uyarı girişi çift tıklatın.

    ![Hata Listesi penceresi](visual-studio-2013-web-tools/_static/image32.png "Hata Listesi penceresi")

    *Hata Listesi penceresi*
3. İçinde **Web Essentials** iletişim kutusu, tıklayın **Evet** bir açıklama eklemek için &lt;meta&gt; etiketi.

    ![Web Essentials iletişim kutusu](visual-studio-2013-web-tools/_static/image33.png "Web Essentials iletişim kutusu")

    *Web Essentials iletişim kutusu*
4. Düzenleyici için  **\_Layout.cshtml** açar ve **&lt;meta&gt;** etiketi otomatik olarak eklenir **baş** bölümü HTML dosyası.

    ![_Düzen sayfasında otomatik olarak eklenen Meta etiketi](visual-studio-2013-web-tools/_static/image34.png "_düzen sayfasında otomatik olarak eklenen Meta etiketi")

    *Otomatik olarak eklenen Meta etiketi \_düzen sayfası*
5. Değiştirin **içeriği** özniteliğini *GeekQuiz* ve dosyayı kaydedin.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Alıştırma 2: IntelliSense kod parçacıkları ve yararlanma

Web Essentials ile HTML düzenleyicisi ile fazladan işlevsellik genişletilmiştir. Bu alıştırmada, web uygulamaları geliştirirken yararlı olan bazı yeni özellikler görürsünüz.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Görev 1 - HTML belgelerinde IntelliSense kullanma

Bu görevde görürsünüz ilk yeni özellik adında **dinamik IntelliSense**. Dinamik IntelliSense diğer etiketleri ve öznitelikleri kullanacağınız olası kimlikleri çıkarsanacak okur.

Bu görevde, bir etiket ve giriş alanını içeren yeni bir HTML form öğesi oluşturacaksınız. Ekleyeceksiniz sonra bir **için** özniteliği etiket girişine bağlayın ve kapsam girişlerinde kimliklerini dayalı IntelliSense önerileri görürsünüz.

1. Açık **Visual Studio Express 2013 Web** ve **Begin.sln** çözüm bulunan **kaynak/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/başlangıcı** klasör. Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.
2. İçinde **Çözüm Gezgini**açın **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.
3. İçinde aşağıdaki form ekleme **&lt;bölümü&gt;** öğesi.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *Form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Girdi etiketinin, bazı alan açıklamasını içeren bir etiket tarafından gelmelidir. Girdi etiketinin önce aşağıdaki etiketi ekleyin.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **İçin** özniteliği bir **&lt;etiket&gt;** hangi form öğesi bir etiket bağlı belirtir. Özniteliğin değeri, ilgili öğenin kimliğine eşit olmalıdır. Ekleme **için** özniteliğini **&lt;etiket&gt;** öğesi. Aşağıdaki şekilde gösterildiği gibi &quot;adı&quot; değeri açılan IntelliSense kutusuna öğelerin aynı kapsam dahilinde kimliğini temel (kapsayan  **&lt;form&gt;**).

    ![IntelliSense içinde kimliği gösteren](visual-studio-2013-web-tools/_static/image35.png "Intellisense'te kimliği gösterme")

    *IntelliSense içinde kimliği gösterme*
6. Son eklenen Sil **&lt;form&gt;** öğesi ve içeriği.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Görev 2 - HTML kod parçacıkları kullanma

HTML5, 25'ten fazla yeni anlamsal etiketleri sunmuştur. Bu etiketler için IntelliSense desteği zaten Visual Studio, ancak Visual Studio 2013 daha hızlı ve yeni kod parçacıkları ekleyerek biçimlendirme yazmak kolay hale getirir. Bu etiketler karmaşık olmasa da, bunlar için doğru codec geri dönüşler ekleme gibi birkaç küçük ıot'nin gelir *ses* etiketi. Bu görevde, ses etiket için HTML kod parçacıkları görürsünüz.

1. İçinde **Index.cshtml** dosya, tür  **&lt;aud** içinde **&lt;bölümü&gt;** aşağıdaki şekilde gösterildiği gibi bir öğe.

    ![Bir audio öğesi ekleme](visual-studio-2013-web-tools/_static/image36.png "bir ses öğesi ekleniyor")

    *Bir audio öğesi ekleme*
2. Tuşuna **sekmesini** iki kez dikkat edin sayfasında aşağıdaki kod nasıl eklenir ve imleci yerleştirildiği **src** ilk kaynak özniteliği.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Tuşuna basarak **sekmesini** anahtar iki kez, kod parçacığı eklenir. Ses parçacığı standart kullanımı gösterilir *ses* etiketiyle iki kaynak dosyaları için geliştirilmiş destek.
3. İkinci satır silin ve ilk satırın kaynak WebCampsTV Katana Göster aşağıdaki bağlantısını güncelleştirin: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Sonuç kodu aşağıda gösterilmiştir.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Kaynak dosya *KatanaProject.mp3* örnek olarak kullanılır. Tercih ederseniz, başka bir kaynak kullanabilirsiniz.
4. Tuşuna **CTRL** + **S** dosyayı kaydetmek için.
5. Tuşuna **CTRL** + **F5** uygulamayı başlatmak için.
6. Bir ses yürütücüsü uygulamaya eklendiğini unutmayın.

    ![Internet Explorer'da bir ses yürütücüsü](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer'da ses player")

    *Internet Explorer'da müzikçalar*

    ![Google Chrome ses yürütücüsü](visual-studio-2013-web-tools/_static/image38.png "ses Player'da Google Chrome")

    *Google Chrome müzikçalar*
7. Tarayıcılar kapatmayın. Bunları sonraki görevde kullanır.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Görev 3 - JavaScript belgelerde IntelliSense kullanma

Web Essentials 2013'ü, stil sayfaları ve HTML sayfalarını kimlikleri ve sınıf adları listesi oluşturur. Bu görevde, bu listeleri JavaScript IntelliSense desteği Web Essentials 2013'te nasıl artırır? öğreneceksiniz.

1. İçinde **Index.cshtml** tanımlamak için aşağıdaki kodu ekleyin bir **betik** JavaScript kodu için etiket.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Aşağıdaki kodu ekleyin **betik** hazır geri çağırma işlevi tanımlamak için etiket.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Giriş işaretini de yerleştirin **betik** etiketi ve ENTER tuşuna **CTRL** + **.** öneri menüsünü açmak için.
4. Tıklayın **dosyasını ayıklayın**.

    ![JavaScript ayıklamak için dosya öneri](visual-studio-2013-web-tools/_static/image39.png "JavaScript ayıklamak için dosya önerisi")

    *JavaScript dosyası önerinizi ayıklayın*
5. İçinde **Kaydet** penceresinde **betikleri** klasör, dosya adı **init.js** tıklatıp **Kaydet**.

    ![Farklı Kaydet penceresi](visual-studio-2013-web-tools/_static/image40.png "penceresi Kaydet")

    *Farklı Kaydet penceresi*

    > [!NOTE]
    > **İnit.js** dosyası oluşturulur ve içeriği betik dosyasına taşınır.
    > 
    > ![İçerik ile oluşturulan Init.js dosyasını](visual-studio-2013-web-tools/_static/image41.png "dahil içerik ile oluşturulan Init.js dosyası")
    > 
    > *İçerik ile oluşturulan Init.js dosyası*
6. Açık **Index.cshtml** dosya ve komut dosyası etiketi başvurusuyla değiştirildiğini denetleyin **init.js** dosya.

    ![Init.js html başvuru](visual-studio-2013-web-tools/_static/image42.png "Init.js html başvurusu")

    *Init.js html başvurusu*
7. Git **Çözüm Gezgini** dikkat **init.js** dosyası dahil otomatik olarak çözümde.

    ![Çözümde yer alan Init.js dosya](visual-studio-2013-web-tools/_static/image43.png "çözümde yer alan Init.js dosyası")

    *Çözümde yer alan Init.js dosyası*
8. Dönmek **init.js** güncelleştirmek için dosya **hazır** geri çağırma işlevi.
9. Geçirilen işlev geri çağırma tanımındaki *hazır*, belirli bir sınıf özniteliği tarafından tüm öğeleri almak için aşağıdaki kodu ekleyin.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Tuşuna **CTRL** + **alanı** içinde tırnak işaretleriyle **getElementsByClassName** işlev çağrısı.

    ![IntelliSense getElementsByClassName işlevi gösteren](visual-studio-2013-web-tools/_static/image44.png "gösteren IntelliSense getElementsByClassName işlevi")

    *IntelliSense gösteren getElementsByClassName işlevi*

    > [!NOTE]
    > IntelliSense proje stil sayfasında tanımlanan sınıflara gösterdiğine dikkat edin.
11. Aşağıdaki kod ile oluşturduğunuz satırı değiştirin.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Sonra imleci konumlandırma **au** tırnak içine **getElementsByTagName** işlevi ve tuşuna **CTRL** + **alanı**.

    ![GetElementByTagName yöntemi için IntelliSense gösteren](visual-studio-2013-web-tools/_static/image45.png "gösteren IntelliSense getElementByTagName yöntemi")

    *IntelliSense gösteren getElementsByTagName yöntemi*
13. Seçin **&quot;ses&quot;** tuşuna basın ve liste **ENTER**. Sonuç, aşağıdaki şekilde gösterilir.

    ![Ses öğeleri alınıyor](visual-studio-2013-web-tools/_static/image46.png "ses öğeleri alınıyor")

    *Ses öğeleri alınıyor*
14. İçinde **Çözüm Gezgini**, sağ tıklayın **init.js** dosyası **betikleri** klasörü ve select **küçültün JavaScript dosyaları** gelen **Web Essentials** menüsü.

    ![JavaScript dosyaları küçültün](visual-studio-2013-web-tools/_static/image47.png "küçültün JavaScript dosyaları")

    *JavaScript dosyaları küçültün*
15. Kaynak dosya değişiklikleri tıkladığınızda otomatik küçültme etkinleştirilmesi istendiğinde **Evet**.

    ![Otomatik küçültme uyarı etkinleştirme](visual-studio-2013-web-tools/_static/image48.png "otomatik küçültme uyarı etkinleştiriliyor")

    *Otomatik küçültme uyarı etkinleştiriliyor*

    > [!NOTE]
    > **İnit.min.js** oluşturulur ve bir bağımlılık olarak eklenen **init.js** dosya.
    > 
    > ![Oluşturulan Init.min.js dosyasını](visual-studio-2013-web-tools/_static/image49.png "Init.min.js dosya oluşturulduğunda")
    > 
    > *Init.min.js dosya oluşturulduğunda*
16. Açık **init.min.js** dosyasını ve dosya küçültülmüş, dikkat edin.

    ![Dosya içeriği Init.Min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js dosya içeriği")

    *Init.Min.js dosya içeriği*
17. İçinde **init.js** dosyasında, aşağıdaki kodu ekleyin **getElementsByTagName** işlev çağrısında, tüm ses öğeleri yürütülecek.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Tıklayın **CTRL** + **S** dosyayı kaydetmek için. Küçültülmüş dosya zaten açık olduğundan, dosyanın Kaynak Düzenleyici dışında değiştirilmiş belirten bir iletişim kutusu görürsünüz. **Evet**'i tıklayın.

    ![Microsoft Visual Studio uyarı](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio Uyarısı")

    *Microsoft Visual Studio Uyarısı*
19. Dönmek **init.min.js** dosyanın yeni kod iler güncelleştirildiğini doğrulamak için dosya.

    ![Güncelleştirilmiş Init.min.js dosya](visual-studio-2013-web-tools/_static/image52.png "Init.min.js dosya güncelleştirildi")

    *Init.min.js dosya güncelleştirildi*
20. Tıklayın **bağlantı tarayıcıyı yenileyin** düğmesi.
21. Her iki tarayıcılar yenilenir sonra önceki görevde gördüğünüz müzikçalar otomatik olarak oynatılmaya başlar.

    ![Görünümde ses yürütücüsü](visual-studio-2013-web-tools/_static/image53.png "görünümde ses player")

    *Görünümde ses yürütücüsü*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı laboratuvarı tamamlayarak nasıl yapılır:

- Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials içinde bulunan yeni HTML düzenleyicisi özelliklerini kullanma
- Renk Seçici ve tarayıcı matris araç ipucu gibi Web Essentials dahil yeni CSS Düzenleyicisi özelliklerini kullanma
- Tüm HTML öğeleri için ayıklama dosya ve IntelliSense gibi Web Essentials içindeki yeni JavaScript Düzenleyicisi özelliklerini kullanma
- Exchange verilerini tarayıcı arasındaki tarayıcı bağlantısını kullanarak Visual Studio
