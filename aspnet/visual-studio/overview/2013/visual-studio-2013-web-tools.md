---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratuvar durum: Visual Studio 2013 Web Araçları | Microsoft Docs'
author: rick-anderson
description: Visual Studio, için mükemmel bir geliştirme ortamıdır. NET tabanlı Windows ve web projeleri. Kolayca için kullanılabilecek bir güçlü metin düzenleyicisi içerir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratuvar durum: Visual Studio 2013 Web Araçları
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](http://aka.ms/webcamps-training-kit)

> Visual Studio, için mükemmel bir geliştirme ortamıdır. NET tabanlı Windows ve web projeleri. Bir proje olmadan tek başına dosyalarını düzenlemek için kolaylıkla kullanılabilecek bir güçlü metin düzenleyicisi içerir.
> 
> Her dosyayı düzenlerken visual Studio tam özellikli ayrıştırma ağacı tutar. Bu, çok daha hızlı ve daha eğlenceli geliştirme deneyimi yaparken benzersiz otomatik tamamlama ve belge tabanlı eylemler sağlamak Visual Studio sağlar. Bu özellikler, HTML ve CSS belgeleri özellikle güçlüdür.
> 
> Bu güç tümünün ayrıca gereksinimlerinize uygun olarak düzenleyicileri güçlü yeni özelliklerle genişletmek basit hale getirme uzantılar, mevcut değil. Web Essentials (çoğunlukla) web ile ilgili geliştirmeler için Visual Studio koleksiyonudur. Çok sayıda yeni IntelliSense tamamlamalar (özellikle de CSS), yeni tarayıcı bağlantısı özellikleri, otomatik içeren JSHint JavaScript dosyaları, HTML, CSS ve modern web geliştirme için gerekli olan diğer birçok özellik için yeni uyarılar.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials'ta dahil yeni HTML düzenleyicisi özelliklerini kullanma
- Tarayıcı matris araç ipucu ve Renk Seçici gibi Web Essentials'ta dahil yeni CSS Düzenleyicisi özelliklerini kullanma
- Dosya ve IntelliSense Extract gibi Web Essentials için tüm HTML öğeleri dahil yeni JavaScript Düzenleyicisi özelliklerini kullanma
- Tarayıcı bağlantısı kullanarak Visual Studio ve tarayıcı arasında veri değiş tokuşu

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) veya daha büyük
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.

1. Bir Windows Explorer penceresi açın ve Gözat Laboratuvar için 's **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırmanıza ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemi başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim kutusu gösterilirse, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların işaretli olduğundan emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıkları

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Size kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan Başlangıç bir çözüm tarafından eşlik **başlamak** diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın klasör. Lütfen bir alıştırma sırasında eklenen kod parçacıkları çözümleri başlangıç bunlardan eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Bir alıştırma için kaynak kodunu içinde da bulacaksınız bir **son** karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren klasör. Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Tarayıcı bağlantısı ve Web Essentials ile çalışma](#Exercise1)
2. [Kod parçacıkları ve IntelliSense yararlanarak](#Exercise2)

> [!NOTE]
> Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir. Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Alıştırma 1: Tarayıcı bağlantısı ve Web Essentials ile çalışma

**Web Essentials** çeşitli çoğunlukla web geliştirme deneyimi çok daha hızlı ve daha eğlenceli yapan odaklanmış modern web geliştirme için kullanışlı özellikler ekleyen bir Visual Studio uzantısıdır. Visual Studio uzantısı galerisinden Web Essentials yükleyebilirsiniz.

**Tarayıcı bağlantısı** Visual Studio IDE ve herhangi bir açık tarayıcıda web uygulamasını Visual Studio arasında veri değişimi için arasında bir kanal sağlar Visual Studio 2013'te bulunan yeni bir özelliktir. Web Essentials DOM nesne modeli ve web sayfalarınıza doğrudan tarayıcıdan CSS stillerini işlemek için araçları ile tarayıcı bağlantısı genişletir.

Bu alıştırmada, bazı tarafından desteklenen özellikleri inceleyeceksiniz **Web Essentials** ve **tarayıcı bağlantısı** basit test sayfası geliştirmek için.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Görev 1 - proje birden çok tarayıcılarında çalışan

Bu görevde, tarayıcılar arası test etmek için yararlı olan birden çok tarayıcılarda aynı anda çalıştırmak için web uygulamanızın yapılandırır.

1. Açık **Microsoft Visual Studio**.
2. İçinde **dosya** menüsünde, select **açık | Proje/çözüm...**  ve **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** içinde **kaynak** klasörü laboratuvarı (C:\WebCampsTK\HOL\VSWebTooling\Source). Seçin **Begin.sln** tıklatıp **açık**.
3. Visual Studio araç çubuğunda, tarayıcı menüsünü genişletin ve seçin **Gözat ile...** .

    ![Gözat ile menü seçeneği](visual-studio-2013-web-tools/_static/image1.png "ile tarayıcı menüde Gözat...")

    *Gözat ile menü seçeneği*
4. İçinde **Gözat ile** iletişim kutusu, select **Google Chrome** ve **Internet Explorer** basılı tutarak **CTRL** anahtar ve 'ıtıklatın **Varsayılan olarak ayarla**.

    ![İletişim kutusuyla Gözat](visual-studio-2013-web-tools/_static/image2.png "iletişim kutusuyla Gözat")

    *Birden çok varsayılan tarayıcı seçme*
5. Artık Google Chrome ve Internet Explorer varsayılan tarayıcı olarak görünmelidir. Tıklatın **iptal** iletişim kutusunu kapatın.

    ![Google Chrome ve varsayılan tarayıcı olarak Internet Explorer](visual-studio-2013-web-tools/_static/image3.png "Google Chrome ve Internet Explorer varsayılan tarayıcı")

    *Google Chrome ve varsayılan tarayıcı olarak Internet Explorer*

    > [!NOTE]
    > Varsayılan tarayıcı yapılandırdıktan sonra **birden çok tarayıcı** seçeneği tarayıcı menüde.
    > 
    > ![Birden çok tarayıcı](visual-studio-2013-web-tools/_static/image4.png "birden çok tarayıcı")
6. Tuşuna **CTRL** + **F5** hata ayıklama olmadan uygulamayı çalıştırın.
7. Her iki tarayıcı pencerelerini açtığınızda, aynı anda hem tarayıcılarda güncelleştirmeleri görmek için bunlardan birini birbirinin üzerine yerleştirin. Tarayıcılar açık mavi dikdörtgen bir web sayfası görüntülemelidir.

    ![Bir tarayıcı birbirinin üzerine yerleştirme](visual-studio-2013-web-tools/_static/image5.png "birbirinin üzerine bir tarayıcı yerleştirme")

    *Bir tarayıcı birbirinin üzerine yerleştirme*
8. Tarayıcılar kapatmayın. Bunları işlemin sonraki görev kullanır.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Görev 2 - kullanarak Zen HTML öğeleri oluşturmak için kodlama

**Zen kodlama** yüksek hızlı HTML, XML, XSL (veya başka bir yapılandırılmış kod biçimi) için eklentisi kodlama ve düzenleme bir düzenleyicisidir. Bu eklenti çekirdek ifadelere - CSS Seçici benzer - HTML kod genişletmenize olanak tanıyan güçlü kısaltması altyapısıdır. Zen kodlama Seçici söz dizimi CSS kullanarak HTML stil yazmak için hızlı bir yoludur.

Bu alıştırmada, soru seçenekleri temsil eden HTML düğmeleri oluşturmak için Web Essentials tarafından sağlanan Zen kodlama özelliğini kullanır.

1. Visual Studio'ya geri çevirin.
2. Açık **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.
3. Değiştir **&lt;!--TODO: seçenekler burada--eklemek&gt;** açıklama aşağıdaki kodu ve basın **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. HTML kod genişletilmiş.

    ![HTML Genişletilmiş](visual-studio-2013-web-tools/_static/image6.png "genişletilmiş HTML")

    *Genişletilmiş HTML*

    > [!NOTE]
    > Zen kodlama sözdizimi hakkında daha fazla bilgi için aşağıdakilere bakın [makale](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Tıklatın **bağlantılı tarayıcılar yenileme** hem tarayıcılar güncelleştirmek için düğmesini.

    ![Bağlantılı tarayıcılar yenileme](visual-studio-2013-web-tools/_static/image7.png "bağlantılı tarayıcılar Yenile")

    *Bağlantılı tarayıcılar Yenile*

    ![Internet Explorer - sayfası güncelleştirilmiş dört düğmeleriyle](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - sayfası dört düğmeleriyle güncelleştiriliyor")

    *Internet Explorer - sayfası dört düğmeleriyle güncelleştiriliyor.*

    ![Google Chrome - sayfası güncelleştirilmiş dört düğmeleriyle](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - sayfası dört düğmeleriyle güncelleştiriliyor")

    *Google Chrome - sayfası dört düğmeleriyle güncelleştiriliyor.*
6. Visual Studio'ya geri çevirin.
7. Düğmeleri sayfasına eklediğiniz, ancak hala bir şablon soru eklemeniz gerekir. Bunu yapmak için yeni bir özellik olarak adlandırılan Web Essentials'ta kullanacağınız **Lorem Ipsum Oluşturucu**. Bulun **div** öğeyle **sınıfı** özniteliği **ön**.
8. İlk alt öğesi aşağıdaki kodu ekleyin **div**ve basın **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. HTML kod genişletilmiş.

    ![Lorem Ipsum otomatik olarak oluşturulur](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum otomatik olarak oluşturulur")

    *Lorem Ipsum otomatik olarak oluşturulur*

    > [!NOTE]
    > Kapsamında Zen kodlama, doğrudan HTML Düzenleyicisi'nde Lorem Ipsum kodu şimdi oluşturabilirsiniz. Yazmanız yeterlidir **lorem** ve isabet **SEKMESİ** ve bir 30 Lorem metin eklenecek Ipsum word. Örneğin *lorem10* 10 Lorem Ipsum sözcük ekler.
10. Adlı Web Essentials'ta başka bir yeni özelliği kullanarak soru en üstte bir logo ekleyecek **Lorem piksel Oluşturucu**. İlk alt öğesi aşağıdaki kodu ekleyin **div** öğeyle **kapsayıcı** olarak **sınıfı** değeri ve tuşuna **sekmesini**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kod HTML genişletmeniz gerekir.

    ![Lorem piksel otomatik olarak oluşturulur](visual-studio-2013-web-tools/_static/image11.png "Lorem piksel otomatik olarak oluşturulur")

    *Lorem piksel otomatik olarak oluşturulur*

    > [!NOTE]
    > Zen kodlama bir parçası olarak Lorem piksel kodu doğrudan HTML Düzenleyicisi'nde de oluşturabilirsiniz. Yazmanız yeterlidir **PIX 200 x 200 hayvanlar** ve isabet **sekmesini** ve bir **img** bir hayvan 200 x 200 görüntüsü etiketiyle eklenecek. Daha fazla bilgi için bkz [Lorem piksel](http://www.lorempixel.com).
12. Tıklatın **bağlantılı tarayıcılar yenileme** hem tarayıcılar güncelleştirmek için düğmesini.

    ![Internet Explorer - otomatik olarak oluşturulur resim ve metin](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - otomatik olarak oluşturulur resim ve metin")

    *Internet Explorer - otomatik olarak oluşturulur resim ve metin*

    ![Google Chrome - otomatik olarak oluşturulur resim ve metin](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - otomatik olarak oluşturulur resim ve metin")

    *Google Chrome - otomatik olarak oluşturulur resim ve metin*

    > [!NOTE]
    > Görüntü rastgele kod parçacığını eklerken seçildiğinden, tarayıcılarda gösterilen görüntünün farklı olabilir.
13. Tarayıcılar kapatmayın. Bunları işlemin sonraki görev kullanır.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Görev 3 - bir stil özelliğini güncelleştiriliyor

Bu görevde, tarayıcı bağlantının kullanacağı **Denetleme modu** belirli DOM öğesi burada oluşturulur tam konumu algılamak ve Web tarafından sağlanan bir renk seçici kullanarak o öğenin color özelliği güncelleştirmek için özellik Essentials.

1. Internet Explorer tarayıcınızda basın **CTRL** + **ALT** + **ı** denetleme modunu etkinleştirmek için.
2. Açık mavi kenarlığı taşıyın ve'ı tıklatın.

    ![İşaretçi açık mavi kenarlık taşıma](visual-studio-2013-web-tools/_static/image14.png "işaretçiyi açık mavi kenarlık taşıma")

    *İşaretçi açık mavi kenarlık taşıma*
3. Visual Studio'ya geri çevirin. Tarayıcıda seçili HTML öğesi Visual Studio HTML Düzenleyicisi'nde de nasıl seçili dikkat edin.

    ![Visual Studio HTML Düzenleyicisi'nde seçili HTML öğesi](visual-studio-2013-web-tools/_static/image15.png "Visual Studio HTML Düzenleyicisi'nde seçili HTML öğesi")

    *Visual Studio HTML Düzenleyicisi'nde seçili HTML öğesi*
4. Şimdi güncelleştirecek **ön** seçilen öğeyi stilini değiştirmek için CSS sınıfı. Bunu yapmak için basın **CTRL** + **,** açmak için **gitmek için** arama kutusu. Türü **site.css** ve basın **ENTER** dosyası açılamıyor.

    ![Site.css dosyası açılırken](visual-studio-2013-web-tools/_static/image16.png "Site.css dosya açılırken")

    *Dosya açılırken Site.css*
5. Tuşuna **CTRL** + **F** ve türü **.flip kapsayıcısında .front** CSS Seçici bulunamıyor.
6. Renk Seçici'yi açmak için sınıf kenarlık özelliğinde açık mavi kareyi tıklatın.

    ![Renk Seçici açma](visual-studio-2013-web-tools/_static/image17.png "Renk Seçici açma")

    *Renk Seçici açma*
7. Renk Seçici Köşeli Çift Ayraca düğmesine tıklayarak genişletin ve yeni bir renk seçin.

    ![Renk Seçici genişletme](visual-studio-2013-web-tools/_static/image18.png "Renk Seçici genişletme")

    *Renk Seçici genişletme*
8. Tuşuna **CTRL** + **ALT** + **ENTER** bağlantılı tarayıcılar yenilenecek.
9. Internet Explorer'a geçin ve kenarlık rengi nasıl değiştiğini dikkat edin.

    ![Internet Explorer - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - güncelleştirilmiş kenarlık rengi")

    *Internet Explorer - güncelleştirilmiş kenarlık rengi*
10. Google Chrome geçin ve kenarlık rengi nasıl değiştiğini dikkat edin.

    ![Google Chrome - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - güncelleştirilmiş kenarlık rengi")

    *Google Chrome - güncelleştirilmiş kenarlık rengi*
11. Visual Studio'ya geri çevirin.
12. Sonuna gidin **Site.css** dosya ve tuşuna **CTRL** + **F** bulmak için **.btn** Seçici.
13. Dikkat **- webkit-border-radius** özelliği yeşil olarak çizilir.

    ![-webkit-border-radius özelliği btn Seçici](visual-studio-2013-web-tools/_static/image21.png "btn Seçici - webkit-border-radius özelliği")

    *-webkit-border-radius özelliği btn Seçici*
14. İçinde düzeltme işareti koyun **- webkit-border-radius** özelliği. Mavi bir çizgi özelliğinin ilk sözcüğün ilk harfi altında görünmelidir. Bu **akıllı etiket**.
15. Tuşuna **CTRL** + **.** öneriler menüsünü açın ve tıklatın **standart özelliği (border-radius) eksik Ekle**.

    ![Standart özellik öneri eksik Ekle](visual-studio-2013-web-tools/_static/image22.png "standart özellik öneri eksik Ekle")

    *Standart özellik öneri eksik Ekle*
16. **Border-radius** özelliği CSS kuralı otomatik olarak eklenir.

    ![Eklenen standart özellik eksik](visual-studio-2013-web-tools/_static/image23.png "eklenen standart özelliği eksik")

    *Eklenen standart özelliği eksik*
17. İşaretçinin taşıyamazsınız **border-radius** görüntülenecek özellik **tarayıcı matris araç ipucu**. **Tarayıcı matris araç ipucu** her tarayıcıda özelliği kullanılabilirliğini gösterir.

    ![Tarayıcı matris araç ipucu](visual-studio-2013-web-tools/_static/image24.png "tarayıcı matris araç ipucu")

    *Tarayıcı matris araç ipucu*
18. Dikkat değerini **border-radius** özelliktir hala altı çizili. İşaretçinin uyarı iletisini görmek için değerin taşıyın.

    ![Border-radius özellik değeri Uyarı](visual-studio-2013-web-tools/_static/image25.png "Border-radius özellik değeri Uyarı")

    *Border-radius özellik değeri Uyarı*
19. Ölçü kaldırmak **border-radius** araç ipucu tarafından önerilen özellik değeri.
20. Olarak **border-radius** köşeleri olan, kaldırabilirsiniz nasıl yuvarlatılmış kenarlık tanımlamak için standart özelliği **- webkit-border-radius** özellik ve CSS kuraldan değer.
21. İçinde düzeltme işareti koyun **sözcük kaydırma** özelliği ve akıllı etiket aşağıda da görünür dikkat edin.
22. Menüsünü açın ve tıklatın **eksik satıcı özellikleri eklemek**.

    ![Eksik satıcı özellikleri öneri eklemek](visual-studio-2013-web-tools/_static/image26.png "eksik satıcı özellikleri öneri ekleme")

    *Eksik satıcı özellikleri öneri ekleme*
23. **-Ms-word-wrap** özelliği CSS kuralı otomatik olarak eklenir.

    ![Satıcıya özgü özellik eklenen](visual-studio-2013-web-tools/_static/image27.png "satıcı belirli özelliği eklendi")

    *Satıcı belirli özelliği eklendi*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Görev 4 - tarayıcı HTML kodundan güncelleştiriliyor

Bu görevde, tarayıcı bağlantının kullanacağı **Tasarım modunda** tarayıcıdan DOM nesnesi düzenlemek ve Visual Studio HTML kaynak dosyasında yapılan değişiklikleri aktarmak için özellik.

1. Google Chrome'tuşuna basın **CTRL** + **ALT** + **D** Tasarım modunu etkinleştirmek için.
2. İşaretçinin taşıyamazsınız **Lorem Ipsum dolor sit amet** etiket ve'ı tıklatın.

    ![Soru düzenleme](visual-studio-2013-web-tools/_static/image28.png "soru düzenleme")

    *Soru düzenleme*
3. Bir imleç görüntülenmesi gerekir. Özgün metinle *daha uzun bir soru yazdığınızda, nasıl göründüğünü?*ve tuşuna basarak **ESC** Tasarım modundan çıkmak için.

    ![Düzenlenen soru](visual-studio-2013-web-tools/_static/image29.png "düzenlenebilir soru")

    *Düzenlenen soru*
4. Anahtar Visual Studio'ya geri dönün ve açık **Index.cshtml**, zaten açtıysanız. Dikkat iç metni **&lt;p&gt;** öğesi güncelleştirilmiştir.

    ![HTML sayfasındaki güncelleştirilmiş soru](visual-studio-2013-web-tools/_static/image30.png "güncelleştirilmiş soru HTML sayfasındaki")

    *Güncelleştirilmiş sorusuna HTML sayfası*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Görev 5 - gözden geçirme SEO ilgili uyarıları

**Arama motoru iyileştirme** (SEO), sistemi, daha yüksek bir Web sitesi derece sonuçlarının bir arama motoru listesi hale getirme işlemidir. Daha yüksek site derecelendirir ve daha tutarlı bir şekilde listelenir, daha fazla ziyaretçileri site bu arama altyapısı alırsınız. Web Essentials HTML inceler analitik bir aracı içerir, raporları sorunlar bulundu ve bunları düzeltmek için Yardım sağlar.

1. Git **Görünüm** menüsüne ve ardından **hata listesi** açmak için **hata listesi** penceresi.

    ![Hata Listesi görünümünde menü](visual-studio-2013-web-tools/_static/image31.png "hata listesinde Görünüm menüsü")

    *Hata Listesi görünümünde menüsü*
2. Bildiren bir SEO uyarı fark bir **&lt;meta&gt;** sayfası açıklaması eksik etiketi. Sorunu gidermek için SEO uyarı girişi çift tıklatın.

    ![Hata Listesi penceresi](visual-studio-2013-web-tools/_static/image32.png "Hata Listesi penceresi")

    *Hata Listesi penceresi*
3. İçinde **Web Essentials** iletişim kutusu, tıklatın **Evet** bir açıklama eklemek için &lt;meta&gt; etiketi.

    ![Web Essentials iletişim kutusu](visual-studio-2013-web-tools/_static/image33.png "Web Essentials iletişim kutusu")

    *Web Essentials iletişim kutusu*
4. Düzenleyicisi  **\_Layout.cshtml** açar ve **&lt;meta&gt;** etiketi otomatik olarak eklenir **head** bölümü HTML dosyası.

    ![Otomatik olarak _Layout sayfasında eklenen Meta etiketi](visual-studio-2013-web-tools/_static/image34.png "_Layout sayfasında otomatik olarak eklenen Meta etiketi")

    *Otomatik olarak eklenen Meta etiketi \_düzen sayfası*
5. Değerini değiştirme **içerik** özniteliğini *GeekQuiz* ve dosyayı kaydedin.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Alıştırma 2: Kod parçacıkları ve IntelliSense yararlanarak

Web Essentials ile HTML Düzenleyicisi ek işlevsellik genişletilmiştir. Bu alıştırmada, web uygulamaları geliştirmeye yardımcı olan bazı yeni özellikler görürsünüz.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Görev 1 - HTML belgelerinde IntelliSense kullanma

Bu görevde göreceğiniz ilk yeni özellik adında **dinamik IntelliSense**. Dinamik IntelliSense diğer etiketler ve kullanacağınız olası kimlikleri anlamak için öznitelikler okur.

Bu görevde bir etiket ve giriş alanını içeren yeni bir HTML form öğesi oluşturur. Ekleyeceksiniz sonra bir **için** özniteliği girişine bağlamak için etiket ve kapsamdaki girişleri kimliklerini göre IntelliSense öneriler göreceksiniz.

1. Açık **için Visual Studio Express 2013 Web** ve **Begin.sln** çözüm bulunan **kaynak/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/başlangıç** klasör. Alternatif olarak, önceki alıştırmada elde çözümüyle devam edebilirsiniz.
2. İçinde **Çözüm Gezgini**, açık **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.
3. Aşağıdaki form içinde eklemek **&lt;bölüm&gt;** öğesi.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *Form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Giriş etiketi, alanın bazı açıklaması etiketle tarafından gelmelidir. Giriş etiketi önce aşağıdaki etiketi ekleyin.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **İçin** özniteliği bir **&lt;etiket&gt;** hangi form öğesi bir etiket bağlı belirtir. Özniteliğin değeri ilgili öğe kimliğine eşit olması gerekir. Ekleme **için** özniteliğini **&lt;etiket&gt;** öğesi. Aşağıdaki şekilde gösterildiği gibi &quot;adı&quot; değeri açılır IntelliSense kutusunda aynı kapsamdaki öğelerin kimliğini göre (kapsayan  **&lt;form&gt;**).

    ![IntelliSense içinde kimliği gösteren](visual-studio-2013-web-tools/_static/image35.png "IntelliSense içinde kimliği gösterme")

    *IntelliSense içinde kimliği gösterme*
6. Son eklenen silme **&lt;form&gt;** öğesi ve içeriği.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Görev 2 - kullanarak HTML kod parçacıkları

HTML5 25'ten fazla yeni anlamsal etiketler sunmuştur. Visual Studio bu etiketler için IntelliSense desteği zaten, ancak daha hızlı ve kolay yeni kod parçacıkları ekleyerek biçimlendirme yazmak Visual Studio 2013 yapar. Bu etiketler karmaşık olmasa da, bunlar için doğru codec geri dönüşler ekleme gibi birkaç küçük subtleties gelir *ses* etiketi. Bu görevde, ses etiketi HTML kod parçacıkları görürsünüz.

1. İçinde **Index.cshtml** dosya, yazın  **&lt;aud** içinde **&lt;bölüm&gt;** aşağıdaki resimde gösterildiği gibi öğesi.

    ![Bir audio öğesi ekleme](visual-studio-2013-web-tools/_static/image36.png "bir audio öğesi ekleniyor")

    *Bir audio öğesi ekleniyor*
2. Tuşuna **sekmesini** iki kez ve aşağıdaki kodu sayfada nasıl eklenir dikkat edin ve imleci yerleştirildiği **src** ilk kaynak özniteliği.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Basarak **sekmesini** anahtar iki kez, kod parçacığında eklenir. Ses parçacığı standart kullanımını gösterir *ses* etiketiyle iki kaynak dosyaları için geliştirilmiş destek.
3. İkinci satır silme ve aşağıdaki bağlantı WebCampsTV Katana Göster ile ilk satırının kaynağı güncelleştirme: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Sonuçta elde edilen kod aşağıda verilmiştir.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Kaynak dosya *KatanaProject.mp3* bir örnek olarak kullanılır. Tercih ederseniz başka bir kaynağı kullanabilirsiniz.
4. Tuşuna **CTRL** + **S** dosyayı kaydetmek için.
5. Tuşuna **CTRL** + **F5** uygulamayı başlatmak için.
6. Ses player uygulamaya eklendiğini dikkat edin.

    ![Internet Explorer'da ses player](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer'da ses player")

    *Internet Explorer'da ses player*

    ![Google Chrome ses player](visual-studio-2013-web-tools/_static/image38.png "Google Chrome ses player")

    *Google Chrome ses player*
7. Tarayıcılar kapatmayın. Bunları işlemin sonraki görev kullanır.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Görev 3 - JavaScript belgelerde IntelliSense kullanma

Web Essentials 2013 ile stil sayfaları ve HTML sayfaları kimlikleri ve sınıf adları listesini oluşturur. Bu görevin nasıl JavaScript IntelliSense desteği Web Essentials 2013'te bu listeleri artırmak öğreneceksiniz.

1. İçinde **Index.cshtml** dosya, tanımlamak için aşağıdaki kodu ekleyin bir **betik** JavaScript kodu etiketi.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Aşağıdaki kodu ekleyin **betik** hazır geri çağırma işlevini tanımlamanızı etiketi.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. İçinde düzeltme işareti koyun **betik** etiketi ve tuşuna **CTRL** + **.** öneri menüsünü açmak için.
4. Tıklatın **dosyasını ayıklayın**.

    ![JavaScript ayıklamak için dosya öneri](visual-studio-2013-web-tools/_static/image39.png "JavaScript ayıklamak için dosya önerisi")

    *JavaScript ayıklamak için dosya önerisi*
5. İçinde **Kaydet** penceresinde, seçin **betikleri** adı dosya klasörü **init.js** tıklatıp **kaydetmek**.

    ![Farklı Kaydet penceresi](visual-studio-2013-web-tools/_static/image40.png "Kaydet penceresi")

    *Farklı Kaydet penceresi*

    > [!NOTE]
    > **İnit.js** dosya oluşturulur ve komut dosyası içeriğini dosyasına taşınır.
    > 
    > ![Dahil edilen içeriği ile oluşturulan Init.js dosyasını](visual-studio-2013-web-tools/_static/image41.png "dahil içerik ile oluşturulan Init.js dosyası")
    > 
    > *Dahil edilen içeriği ile oluşturulan Init.js dosyası*
6. Açık **Index.cshtml** dosya ve komut dosyası etiketinin başvuru değiştirildi denetleyin **init.js** dosya.

    ![Init.js html başvuru](visual-studio-2013-web-tools/_static/image42.png "Init.js html başvurusu")

    *Init.js html başvurusu*
7. Git **Çözüm Gezgini** ve dikkat **init.js** dosyası dahil otomatik olarak çözümde.

    ![Çözümdeki Init.js dosya](visual-studio-2013-web-tools/_static/image43.png "çözümdeki Init.js dosyası")

    *Çözümdeki Init.js dosyası*
8. Dönmek **init.js** güncelleştirmek için dosya **hazır** geri çağırma işlevi.
9. Geçirilir işlevi geri çağırma tanımı içinde *hazır*, belirli bir sınıf özniteliği tarafından tüm öğeleri almak için aşağıdaki kodu ekleyin.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Tuşuna **CTRL** + **alanı** tırnak işaretleri arasında **getElementsByClassName** işlev çağrısı.

    ![GetElementsByClassName işlevi için IntelliSense gösteren](visual-studio-2013-web-tools/_static/image44.png "gösteren IntelliSense getElementsByClassName işlevi")

    *GetElementsByClassName işlevi için IntelliSense gösterme*

    > [!NOTE]
    > IntelliSense proje stil sayfasında tanımlanan sınıflar gösterdiğine dikkat edin.
11. Aşağıdaki kod ile oluşturduğunuz satırı değiştirin.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Sonra imleç Konumlandır **au** tırnak işareti içinde **getElementsByTagName** işlevi ve tuşuna **CTRL** + **alanı**.

    ![GetElementByTagName yöntemi için IntelliSense gösteren](visual-studio-2013-web-tools/_static/image45.png "gösteren IntelliSense getElementByTagName yöntemi")

    *GetElementsByTagName yöntemini gösteren IntelliSense*
13. Seçin **&quot;ses&quot;** basın ve liste **ENTER**. Sonuç aşağıdaki çizimde gösterilmiştir.

    ![Ses öğeleri alınıyor](visual-studio-2013-web-tools/_static/image46.png "ses öğeleri alınıyor")

    *Ses öğeleri alınıyor*
14. İçinde **Çözüm Gezgini**, sağ **init.js** dosyasını **betikleri** klasörü ve seçin **Minify JavaScript dosyaları** gelen **Web Essentials** menüsü.

    ![JavaScript dosyaları minify](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript dosyaları")

    *JavaScript dosyaları minify*
15. Kaynak dosya değişiklikleri tıklattığınızda otomatik küçültme etkinleştirmek isteyip istemediğiniz sorulduğunda **Evet**.

    ![Otomatik küçültme uyarı etkinleştirme](visual-studio-2013-web-tools/_static/image48.png "otomatik küçültme uyarı etkinleştirme")

    *Otomatik küçültme uyarı etkinleştirme*

    > [!NOTE]
    > **İnit.min.js** oluşturulur ve bir bağımlılık olarak eklenen **init.js** dosya.
    > 
    > ![Oluşturulan Init.min.js dosyasını](visual-studio-2013-web-tools/_static/image49.png "oluşturulan Init.min.js dosyası")
    > 
    > *Oluşturulan Init.min.js dosyası*
16. Açık **init.min.js** dosya ve dosya küçültülmüş olduğunu dikkat edin.

    ![Init.Min.js dosya içeriğini](visual-studio-2013-web-tools/_static/image50.png "Init.min.js dosya içeriği")

    *Init.Min.js dosya içeriği*
17. İçinde **init.js** dosya, aşağıdaki kodu ekleyin **getElementsByTagName** işlev çağrısı ses tüm öğeleri yürütmek için.

    (Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Tıklatın **CTRL** + **S** dosyayı kaydetmek için. Küçültülmüş dosya zaten açık olduğundan, dosya kaynak Düzenleyicisi dışında değiştirildi belirten bir iletişim kutusu görürsünüz. **Evet**'i tıklayın.

    ![Microsoft Visual Studio uyarı](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio uyarı")

    *Microsoft Visual Studio uyarı*
19. Dönmek **init.min.js** dosyanın yeni kodu ile güncelleştirildi doğrulamak için dosya.

    ![Güncelleştirilmiş Init.min.js dosya](visual-studio-2013-web-tools/_static/image52.png "güncelleştirilmiş Init.min.js dosya")

    *Güncelleştirilmiş Init.min.js dosya*
20. Tıklatın **bağlantı tarayıcıyı yenilemek** düğmesi.
21. Her iki tarayıcılar yenilenir sonra otomatik olarak çalma önceki görevde gördüğünüz ses oynatıcıları başlatılacak.

    ![Görünümde bir ses yürütücüsü](visual-studio-2013-web-tools/_static/image53.png "görünümde ses player")

    *Görünümde bir ses yürütücüsü*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı Laboratuvar tamamlayarak nasıl yapılır:

- Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials'ta dahil yeni HTML düzenleyicisi özelliklerini kullanma
- Tarayıcı matris araç ipucu ve Renk Seçici gibi Web Essentials'ta dahil yeni CSS Düzenleyicisi özelliklerini kullanma
- Dosya ve IntelliSense Extract gibi Web Essentials için tüm HTML öğeleri dahil yeni JavaScript Düzenleyicisi özelliklerini kullanma
- Tarayıcı bağlantısı kullanarak Visual Studio ve tarayıcı arasında veri değiş tokuşu
