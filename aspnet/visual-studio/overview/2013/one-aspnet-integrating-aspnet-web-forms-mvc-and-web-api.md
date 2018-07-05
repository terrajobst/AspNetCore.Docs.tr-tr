---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Uygulamalı Laboratuvar: Tek ASP.NET: ASP.NET Web formları, MVC ve Web API'yi tümleştirme | Microsoft Docs"
author: rick-anderson
description: ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir. Genişletmeyle ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382958"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Uygulamalı Laboratuvar: Tek ASP.NET: ASP.NET Web formları, MVC ve Web API'yi tümleştirme
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](http://aka.ms/webcamps-training-kit)

> ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir. Genişletmeyle oluşturulduktan sonra ASP.NET yakaladı ve ifade edilen teknolojiler tümleşik olması gerekir, doğru çalışma yeni çabalar olmuştur **tek ASP.NET**.
> 
> Visual Studio 2013, bir uygulama oluşturmak ve tüm ASP.NET teknolojileri tek bir projede kullanmanıza olanak sağlayan yeni bir birleşik proje sistemi sunuyor. Bu özellik, bir proje ve onunla Sopası başlangıcında bir teknolojisini seçin gereğini ortadan kaldırır ve bunun yerine bir projede birden çok ASP.NET çerçeve kullanımını teşvik eder.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Temel bir Web sitesi oluşturma **tek ASP.NET** proje türü
- Farklı kullanım **ASP.NET** çerçeveleri ister **MVC** ve **Web API** aynı projede
- Ana bileşenleri tanımlamak bir **ASP.NET** uygulama
- Yararlanmak **ASP.NET iskeleti oluşturma** denetleyicileri ve görünümleri CRUD işlemleri gerçekleştirmek için otomatik olarak oluşturmak için framework tabanlı model sınıflarınızı
- Her iş için doğru aracı kullanarak makine ve İnsan okunabilir biçimde bilgi aynı kümesini kullanıma sunma

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya üzeri
- [Visual Studio 2013 Güncelleştirme 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.

1. Açık Windows Gezgini ve Laboratuvar göz atın **kaynak** klasör.
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

1. [Yeni bir Web formları projesi oluşturma](#Exercise1)
2. [İskele kurma kullanarak MVC denetleyicisi oluşturma](#Exercise2)
3. [Yapı İskelesi kullanarak bir Web API denetleyicisi oluşturma](#Exercise3)

Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**

> [!NOTE]
> Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir. Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Alıştırma 1: yeni bir Web formları projesi oluşturma

Bu alıştırmada, Visual Studio 2013 kullanarak yeni Web Forms sitesi oluşturacaksınız **tek ASP.NET** birleşik Proje deneyimi, Web Forms, MVC ve Web API'si bileşenleri aynı uygulamada kolayca tümleştirmenizi sağlar. Daha sonra oluşturulan çözümü keşfedin ve onun parçalarını tanımlamak ve son olarak eylem Web sitesinde görürsünüz.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Görev 1: tek ASP.NET deneyimi kullanarak yeni bir Site oluşturma

Bu görevde, başlayacak yeni bir Web sitesi Visual Studio'da oluşturma dayalı **tek ASP.NET** proje türü. **Bir ASP.NET** tüm ASP.NET teknolojileri birleştirir ve karıştırın ve bunları istediğiniz gibi eşleşen seçeneği sunar. Canlı Web formları, MVC ve Web API farklı bileşenleri ardından yan yana ve uygulamanızdaki algılar.

1. Açık **Visual Studio Express 2013 Web** seçip **dosya | Yeni proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **ASP.NET Web uygulaması** altında **Visual C# | Web** sekmesini tıklatıp emin **.NET Framework 4.5** seçilir. Projeyi adlandırın *MyHybridSite*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni ASP.NET Web uygulaması projesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Yeni bir ASP.NET Web uygulaması projesi oluşturma*
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu seçip alt **MVC** ve **Web API** seçenekleri. Ayrıca, emin **kimlik doğrulaması** seçeneği **bireysel kullanıcı hesapları**. Devam etmek için **Tamam** 'a tıklayın.

    ![Web API ve MVC bileşenler dahil olmak üzere Web Forms şablonuyla yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Web API ve MVC bileşenler dahil olmak üzere Web Forms şablonuyla yeni proje oluşturma*
4. Şimdi oluşturulan çözüm yapısını keşfedebilirsiniz.

    ![Oluşturulan çözüm keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Oluşturulan çözüm keşfetme*

    1. **Hesap:** bu klasörü kaydetmek için oturum açın ve uygulamanın kullanıcı hesaplarını yönetmek için Web formu sayfalarını içerir. Bu klasöre eklenen **bireysel kullanıcı hesapları** kimlik doğrulaması seçeneği Web Forms proje şablonu yapılandırması sırasında belirlenir.
    2. **Modelleri:** bu klasör, uygulama verilerinizi temsil eden sınıfları içerir.
    3. **Denetleyicileri** ve **görünümleri**: Bu klasör için gerekli olan **ASP.NET MVC** ve **ASP.NET Web API** bileşenleri. Sonraki alıştırmalarda MVC ve Web API teknolojileri inceleyeceksiniz.
    4. **Default.aspx**, **Contact.aspx** ve **About.aspx** dosyalar için belirli sayfaları oluşturmak için başlangıç noktası olarak kullanabileceğiniz önceden tanımlanmış Web formu sayfaları, uygulama. Bu dosyaların programlama mantığı olarak adlandırılan ayrı bir dosyada bulunan &quot;arka plan kod&quot; olan dosya, bir &quot;. aspx.vb&quot; veya &quot;. aspx.cs&quot; uzantısı (bağlı olarak dili) kullanılır. Arka plan kod mantıksal sunucu üzerinde çalışır ve dinamik olarak sayfanız için bir HTML çıktı üretir.
    5. **Site.Master** ve **Site.Mobile.Master** sayfaları, uygulamanın görünümünü ve tüm sayfaları standart davranışını tanımlayın.
5. Çift **Default.aspx** sayfasının içeriği keşfetmek için dosya.

    ![Default.aspx sayfasında keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Default.aspx sayfasında keşfetme*

    > [!NOTE]
    > **Sayfa** yönergesi dosyanın üst Web Forms sayfası özniteliklerini tanımlar. Örneğin, **MasterPageFile** öznitelik asıl yolunu belirtir - bu durumda, sayfa *Site.Master* sayfası - ve **Inherits** öznitelik tanımlar. arka plan kod sınıfı sayfanın devralır. Bu sınıf tarafından belirlenen dosya bulunan **CodeBehind** özniteliği.
    > 
    > **Asp: Content** denetim (metin, biçimlendirme ve denetimleri) sayfanın gerçek içeriği bulunduran ve eşlenmiş bir **asp: ContentPlaceHolder** ana sayfadaki denetim. Bu durumda, sayfa içeriği içinde işlenir *MainContent* tanımlı denetim *Site.Master* sayfası.
6. Genişletin **uygulama\_Başlat** klasörü ve bildirim **WebApiConfig.cs** dosya. Web API'si ile tek ASP.NET şablon projenizi yapılandırırken içerdiğinden visual Studio bu dosyayı oluşturulan çözümde yer alan.
7. Açık **WebApiConfig.cs** dosya. İçinde *WebApiConfig* HTTP eşleştiren, Web API, ile ilişkili yapılandırma bulacaksınız sınıf yolları için **Web APİ'si denetleyicilerinin**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Açık **RouteConfig.cs** dosya. İçinde *RegisterRoutes* yöntemi için bir HTTP rotasıyla eşleştiren MVC ile ilişkili yapılandırmasını bulacaksınız **MVC denetleyicileri**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümü çalıştırma

Bu görevde oluşturulan çözümü çalıştırın, uygulama ve URL yeniden yazma ve yerleşik kimlik doğrulama gibi özelliklerinden bazılarını keşfedin.

1. Çözümü çalıştırmak için basın **F5** veya **Başlat** düğme araç çubuğunda yer alan. Tarayıcıda uygulama giriş sayfası açmanız gerekir.

    ![Çözüm çalıştırma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Web formları sayfaları çağrılan doğrulayın. Bunu yapmak için URL'ye **/contact.aspx** tuşuna basın ve adres çubuğuna URL'ye **Enter**.

    ![Kolay URL'ler](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Kolay URL'ler*

    > [!NOTE]
    > Gördüğünüz gibi URL değişir **/başvurun**. Başlangıç **ASP.NET 4**, URL yönlendirme özellikleri, Web Forms eklendi, URL'ler, yazabilmesi ister *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* yerine  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Daha fazla bilgi için [URL yönlendirme](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Şimdi uygulamaya tümleşik kimlik doğrulaması akışı inceleyeceksiniz. Bunu yapmak için tıklatın **kaydetme** sayfanın sağ üst köşesindeki içinde.

    ![Yeni bir kullanıcı kaydetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Yeni bir kullanıcı kaydetme*
4. İçinde **kaydetme** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetme**.

    ![Kayıt sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Kayıt sayfası*
5. Uygulama, yeni hesabı kaydeder ve kullanıcının kimliği doğrulanır.

    ![Kimliği doğrulanmış kullanıcı](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Kimliği doğrulanmış kullanıcı*
6. Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Alıştırma 2: İskele kurma kullanarak MVC denetleyicisi oluşturma

Bu alıştırmada, Eylemler ve Razor görünümleri, tek satırlık bir kod yazmadan CRUD işlemleri gerçekleştirmek için bir ASP.NET MVC 5 denetleyici oluşturmak için Visual Studio tarafından sağlanan ASP.NET iskeleti oluşturma çerçevesi yararlanır. Yapı iskelesi süreci Entity Framework Code First SQL veritabanı'nda veri bağlamı ve veritabanı şeması oluşturmak için kullanır.

**Entity Framework'ü ilk kod**

Entity Framework (EF) programlama ilişkisel depolama şeması kullanarak doğrudan programlama yerine kavramsal bir uygulama modeli tarafından veri erişimi uygulamaları oluşturmanızı sağlayan bir nesne ilişkisel eşleyicidir (ORM) olur.

Entity Framework Code First modelleme iş akışı sorgulama, gerçekleştirirken EF dayalı modeli temsil etmek için kendi etki alanı sınıflarını kullanmak değişiklik izleme ve güncelleştirme işlevleri sağlar. Code First geliştirme iş akışı kullanarak, bir veritabanı oluşturmak ya da bir şema belirleme uygulamanızı başlatmak gerekmez. Bunun yerine, uygulamanız için en uygun etki alanı model nesneleri tanımlayan standart .NET sınıfları yazabilirsiniz ve Entity Framework veritabanı sizin için oluşturur.

> [!NOTE]
> Entity Framework hakkında daha fazla bilgi [burada](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Görev 1 – yeni bir Model oluşturma

Artık tanımlayacak bir **kişi** sınıfı MVC denetleyicisi ve görünümleri oluşturmak için iskele kurma işlemi tarafından kullanılan model olacaktır. Oluşturarak başlayacaksınız bir **kişi** model sınıfı ve denetleyici de CRUD işlemlerini otomatik olarak oluşturulacak yapı iskelesi özelliklerini kullanma.

1. Açık **Visual Studio Express 2013 Web** ve **MyHybridSite.sln** çözüm bulunan **kaynak/Ex2-MvcScaffolding/başlangıcı** klasör. Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.
2. İçinde **Çözüm Gezgini**, sağ **modelleri** klasörü **MyHybridSite** seçin ve proje **Ekle | Sınıfı...** .

    ![Kişi model sınıfı ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Kişi model sınıfı ekleme*
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda, dosya adı *Person.cs* tıklatıp **Ekle**.

    ![Kişi model sınıfı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Kişi model sınıfı oluşturma*
4. İçeriğinin yerine geçecek **Person.cs** dosyasındaki kodu aşağıdaki kodla. Tuşuna **CTRL + S** değişiklikleri kaydedin.

    (Kod parçacığını - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. İçinde **Çözüm Gezgini**, sağ **MyHybridSite** seçin ve proje **derleme**, veya basın **CTRL + SHIFT + B** Projeyi derlemek için.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Görev 2-bir MVC denetleyicisi oluşturma

Şimdi **kişi** model oluşturulur, CRUD denetleyici eylemleri ve görünümleri oluşturmak için Entity Framework ile ASP.NET MVC yapı iskelesi kullanacağı **kişi**.

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasörü **MyHybridSite** seçin ve proje **Ekle | Yeni İskeleli öğe...** .

    ![Yeni iskele kurulmuş denetleyici oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Yeni iskele kurulmuş denetleyici oluşturma*
2. İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** ve ardından **Ekle.**

    ![Entity Framework ve görünümler ile MVC 5 denetleyici seçme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Entity Framework ve görünümler ile MVC 5 denetleyici seçme*
3. Ayarlama *MvcPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini işaretleyip **kişi (MyHybridSite.Models)**  olarak **Model sınıfı**.

    ![Yapı iskelesi ile MVC denetleyicisi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Yapı iskelesi ile MVC denetleyicisi ekleme*
4. Altında **veri bağlamı sınıfının**, tıklayın **yeni veri bağlamı...** .

    ![Yeni bir veri bağlamı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Yeni bir veri bağlamı oluşturma*
5. İçinde **yeni veri bağlamı** iletişim kutusunda, yeni veri bağlamı adı *PersonContext* tıklatıp **Ekle**.

    ![Yeni PersonContext oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Yeni PersonContext tür oluşturma*
6. Tıklayın **Ekle** için yeni denetleyicisi oluşturmak için **kişi** yapı iskelesi ile. Visual Studio ardından denetleyici eylemleri, kişi veri bağlamı ve Razor görünümleri oluşturur.

    ![Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra*
7. Açık **MvcPersonController.cs** dosyası **denetleyicileri** klasör. CRUD eylem yöntemlerine otomatik olarak oluşturulmuş dikkat edin.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Seçerek **zaman uyumsuz denetleyici eylemlerini kullanmak** iskele onay kutusundan seçenekleri önceki adımlarda, Visual Studio kişi veri bağlamı erişim gerektiren tüm eylemler için zaman uyumsuz eylem yöntemi oluşturur. Uzun süre çalışan zaman uyumsuz eylem yöntemleri kullanın, Web sunucusu isteği işlenirken iş gerçekleştirmeyi engellenmesini önlemek için istekleri CPU dışı bağlı önerilir.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3: çözümü çalıştırma

Bu görevde, görünümleri yeniden doğrulamak için çözümü çalıştıracak **kişi** beklendiği gibi çalışıyor. Veritabanı başarılı bir şekilde kaydedildiğini doğrulamak için yeni bir kişiye ekleyeceksiniz.

1. Tuşuna **F5** çözümü çalıştırın.
2. Gidin **/MvcPerson**. Kişilerin listesini gösteren iskele kurulmuş görünümü görüntülenmesi gerekir.
3. Tıklayın **Yeni Oluştur** yeni bir kişiye eklemek için.

    ![İskele kurulmuş MVC görünümlerine gezinme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *İskele kurulmuş MVC görünümlerine gezinme*
4. İçinde **Oluştur** görüntülemek için sağlayan bir **adı** ve bir **yaş** kişi ve tıklayın **Oluştur**.

    ![Yeni bir kişi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Yeni bir kişi ekleme*
5. Yeni bir kişiye listesine eklenir. Öğe listesinde **ayrıntıları** kişinin ayrıntıları görüntülemek için. Ardından **ayrıntıları** yi **listesine geri** liste görünümüne geri dönmek için.

    ![Kişi Ayrıntıları görünümü](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Kişi Ayrıntıları görünümü*
6. Tıklayın **Sil** kişi silmek için bağlantı. İçinde **Sil** yi **Sil** işlemini onaylamak için.

    ![Bir kişi siliniyor](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Bir kişi siliniyor*
7. Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Alıştırma 3: iskele kurma kullanarak bir Web API denetleyicisi oluşturma

Web API çerçevesi ASP.NET yığınının bir parçasıdır ve uygulama HTTP Hizmetleri genel veri gönderme ve JSON veya XML biçimli bir RESTful API'si aracılığıyla alma kolaylaştırmak için tasarlanmıştır.

Bu alıştırmada, ASP.NET iskeleti oluşturma yeniden bir Web API denetleyicisi oluşturmak için kullanın. Aynı kullanacağınız **kişi** ve **PersonContext** aynı kişi verilerini JSON biçiminde sağlamak için önceki alıştırmada sınıflardan. Aynı ASP.NET uygulamasından farklı yollarla aynı kaynakları nasıl getirebilir görürsünüz.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Görev 1-bir Web API denetleyicisi oluşturma

Bu görevde, yeni bir oluşturacaksınız **Web API denetleyicisi** kişi verilerini JSON gibi makine tüketilebilir biçiminde kullanıma.

1. Henüz açık değilse açın **Visual Studio Express 2013 Web** açın **MyHybridSite.sln** çözüm bulunan **kaynak/Ex3-Webapı/başlangıcı** klasör. Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.

    > [!NOTE]
    > Alıştırma 3 başlangıç çözüm ile Başlat, basın **CTRL + SHIFT + B** çözümü derlemek için.
2. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasörü **MyHybridSite** seçin ve proje **Ekle | Yeni İskeleli öğe...** .

    ![Yeni iskele kurulmuş denetleyici oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Yeni iskele kurulmuş denetleyici oluşturma*
3. İçinde **İskele Ekle** iletişim kutusunda **Web API** sol bölmesinde, ardından **Web API 2 denetleyici Entity Framework kullanarak Eylemler ile** Orta bölmede ve 'yetıklayın **Ekleyin.**

    ![Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçerek](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme")

    *Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme*
4. Ayarlama *ApiPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini işaretleyip **kişi (MyHybridSite.Models)**  ve **PersonContext (MyHybridSite.Models)** olarak **modeli** ve **veri bağlamı** sırasıyla sınıfları. Ardından **Ekle**.

    ![Yapı iskelesi ile Web API denetleyici ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "yapı iskelesi ile Web API denetleyici ekleme")

    *Yapı iskelesi ile Web API denetleyici ekleme*
5. Visual Studio ardından oluşturacağını **ApiPersonController** verilerinizle çalışmaya dört CRUD eylemleri ile sınıfı.

    ![Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra")

    *Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra*
6. Açık **ApiPersonController.cs** dosya ve incelemek *GetPeople* eylem yöntemi. Bu yöntem db alanı sorgular **PersonContext** kişiler veri alabilmek için türü.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Artık yorum yöntem tanımını yukarıda dikkat edin. Bu, sonraki görevde kullanacağınız bu eylemi kullanıma sunar. URI'yi sağlar.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Varsayılan olarak, Web API'si sorguları yakalamak için yapılandırılmış */API'si* MVC denetleyicileri ile çarpışmalardan kaçınmak için yol. Bu ayarı değiştirmek istiyorsanız, başvurmak [ASP.NET Web API'de yönlendirme](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümü çalıştırma

Bu görevde, Internet Explorer kullandığınız **F12 Geliştirici araçlarıyla** Web API denetleyicisi gelen tam yanıtı denetlemek için. Uygulama verileriniz hakkında daha fazla bilgi almak için ağ trafiğini yakalamak için nasıl görürsünüz.

> [!NOTE]
> Emin olun **Internet Explorer** seçili **Başlat** düğmesi Visual Studio araç çubuğunda yer alan.
> 
> ![Internet Explorer seçeneği](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 Geliştirici araçlarıyla** geniş Bu uygulamalı-lab içinde kapsanmayan işlevleri kümesine sahiptir. Hakkında daha fazla bilgi edinmek istiyorsanız, başvurmak [F12 geliştirici araçlarını kullanarak](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Tuşuna **F5** çözümü çalıştırın.

    > [!NOTE]
    > Bu görevi doğru takip etmek için uygulamanız verileri olmalıdır. Veritabanınızı boşsa, görev 3'te alıştırma 2 dönün ve MVC görünümleri kullanarak yeni bir kişiye oluşturma konusunda adımları izleyin.
2. Tarayıcıda basın **F12** açmak için **Geliştirici Araçları** paneli. Tuşuna **CTRL** + **4** veya **ağ** simgesine ve ardından ağ trafiğini yakalamaktan başlamak için yeşil ok düğmesi.

    ![Web API ağ yakalama başlatma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "başlatma Web API ağ yakalama")

    *Web API ağ yakalama başlatılıyor*
3. Append **API/ApiPerson** tarayıcının adres çubuğundaki URL. Şimdi gelen yanıt ayrıntılarını inceleyeceksiniz **ApiPersonController**.

    ![Web API aracılığıyla kişi verilerini alma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API aracılığıyla kişi verilerini alma")

    *Web API aracılığıyla kişi verilerini alma*

    > [!NOTE]
    > İndirme tamamlandıktan sonra indirilen dosya ile bir eylem yapmanız istenir. İletişim kutusu, yanıt içeriği geliştiriciler araç penceresi aracılığıyla izlemek için açık bırakın.
4. Artık yanıt gövdesinin inceleyeceksiniz. Bunu yapmak için tıklatın **ayrıntıları** sekmesine ve ardından **yanıt gövdesi**. İndirilen veriler özellikleriyle nesnelerin bir listesini olduğunu denetleyebilirsiniz **kimliği**, **adı** ve **yaş** karşılık gelen için **kişi** sınıf.

    ![Web API yanıt gövdesi görüntüleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "görüntüleme Web API yanıt gövdesi")

    *Web API yanıt gövdesi görüntüleme*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Görev 3: Web API Yardım sayfaları ekleme

Bir Web API'si oluşturma, böylece diğer geliştiriciler, API'nin nasıl çağrılacağını öğrenmiş olacaksınız Yardım sayfasını oluşturmak kullanışlıdır. Oluşturma ve belge sayfaları el ile güncelleştirmeniz, ancak bunları bakım işi yapmak zorunda kalmamak için otomatik olarak oluşturmak iyidir. Bu görevde Web API Yardım sayfaları çözüme otomatik olarak oluşturmak için bir Nuget paketi kullanır.

1. Gelen **Araçları** Visual Studio'da seçim menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu yürütün:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** paketi gerekli bütünleştirilmiş kodları yükler ve Yardım sayfaları'nın altında MVC görünümleri ekler **alanlar/HelpPage** klasör.

    ![HelpPage alan](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage alan")

    *HelpPage alanı*
3. Varsayılan olarak, Yardım sayfaları, belgeler için yer tutucu dizeleri vardır. XML belgeleri yorumları belgeleri oluşturmak için kullanabilirsiniz. Bu özelliği etkinleştirmek için açık **HelpPageConfig.cs** bulunan dosya **HelpPage/alanlar/uygulama\_Başlat** klasörü ve aşağıdaki satırı açıklamadan çıkarın:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. İçinde **Çözüm Gezgini**, projeye sağ tıklayın **MyHybridSite**seçin **özellikleri** tıklatıp **derleme** sekmesi.

    ![Derleme sekmesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "derleme bölümü")

    *Derleme sekmesi*
5. Altında **çıkış**seçin **XML belge dosyası**. Düzenleme kutusuna **uygulama\_Data/XmlDocument.xml**.

    ![Çıkış derleme sekmesi bölümünde](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "çıkış bölümünde derleme sekmesi")

    *Çıkış bölümünde derleme sekmesi*
6. Tuşuna **CTRL** + **S** değişiklikleri kaydedin.
7. Açık **ApiPersonController.cs** dosya **denetleyicileri** klasör.
8. Arasında yeni bir satıra girin *GetPeople* yöntem imzası ve */ / GET API/ApiPerson* açıklama satırı yapın ve ardından üç eğik yazın.

    > [!NOTE]
    > Visual Studio yöntemi belgelerine tanımlayan XML öğeleri otomatik olarak ekler.
9. Bir Özet metni ve dönüş değeri Ekle *GetPeople* yöntemi. Aşağıdaki gibi görünmelidir.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Tuşuna **F5** çözümü çalıştırın.
11. Append **/help** adres çubuğundaki URL'ye yardım sayfasına gidin.

    ![ASP.NET Web API Yardım sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Yardım sayfası")

    *ASP.NET Web API Yardım sayfası*

    > [!NOTE]
    > Ana sayfanın içeriğini API'leri, denetleyici tarafından gruplandırılmış bir tablodur. Tablo girişleri kullanarak dinamik olarak oluşturulan **IApiExplorer** arabirimi. Ekler veya güncelleştirirseniz bir API denetleyicisi, tablonun uygulamayı sonraki açışınızda otomatik olarak güncelleştirilir.
    > 
    > **API** göreli URI ve HTTP yöntemi sütununda listelenir. **Açıklama** sütun yöntemin belgelerinden ayıklanan bilgileri içerir.
12. Metot tanımına eklenen açıklama Açıklama sütununda görüntülendiğini unutmayın.

    ![API yöntemi açıklaması](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API yöntemi açıklaması")

    *API yöntemi açıklaması*
13. Örnek yanıt gövdeleri dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfaya gitmek için API yöntemleri birine tıklayın.

    ![Ayrıntı bilgi sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "ayrıntı bilgileri sayfası")

    *Ayrıntılı bilgi sayfası*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı laboratuvarı tamamlayarak nasıl yapılır:

- Visual Studio 2013'te bir ASP.NET deneyimi kullanarak yeni bir Web uygulaması oluşturma
- Tek bir projeye birden çok ASP.NET teknolojileri tümleştirme
- MVC denetleyicileri ve görünümleri kullanarak ASP.NET iskeleti oluşturma, modeli sınıfların üretileceği
- Zaman uyumsuz programlama ve Entity Framework ile veri erişimi gibi özellikleri kullanan Web APİ'si denetleyicilerinin oluştur
- Web API Yardım sayfaları denetleyicileriniz için otomatik olarak oluştur
