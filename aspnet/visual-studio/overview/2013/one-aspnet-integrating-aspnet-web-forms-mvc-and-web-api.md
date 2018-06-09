---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratuvar durum: Bir ASP.NET: ASP.NET Web Forms, MVC ve Web API tümleştirme | Microsoft Docs'
author: rick-anderson
description: ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir. Genişletmeyle ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566442"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratuvar durum: Bir ASP.NET: ASP.NET Web Forms, MVC ve Web API tümleştirme
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](http://aka.ms/webcamps-training-kit)

> ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir. Genişletmeyle oluşturulduktan sonra ASP.NET görüldüğü ve ifade edilen tümleşik bu teknolojiler olması gerekir, doğru çalışma yeni çabalar olmuştur **bir ASP.NET**.
> 
> Visual Studio 2013'ün bir uygulama oluşturmak ve bir projede tüm ASP.NET teknolojileri kullanmanıza olanak sağlayan yeni bir birleşik proje sistemi tanıtır. Bu özellik proje ve onunla çubuğu başlangıcında bir teknoloji çekme gereğini ortadan kaldırır ve bunun yerine bir projede birden çok ASP.NET çerçeveyi kullanılmasını önerir.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Temel alan bir Web sitesi oluşturma **bir ASP.NET** proje türü
- Kullanım farklı **ASP.NET** çerçeveleri ister **MVC** ve **Web API** aynı projede
- Ana bileşenlerinin tanımlamak bir **ASP.NET** uygulama
- Yararlanmak **ASP.NET yapı İskelesi** çerçevesi denetleyicileri ve görünümler CRUD işlemleri gerçekleştirmek için otomatik olarak oluşturmak için model sınıflarınızı dayalı
- Her iş için doğru aracı kullanarak makine ve kullanıcı okunabilen biçimlerde bilgileri aynı kümesini

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük
- [Visual Studio 2013 Güncelleştirme 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.

1. Açık Windows Gezgini ve Laboratuvar 's gözatın **kaynak** klasör.
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

1. [Yeni bir Web Forms projesi oluşturma](#Exercise1)
2. [Yapı İskelesi kullanarak bir MVC denetleyicisi oluşturma](#Exercise2)
3. [Yapı İskelesi kullanan bir Web API denetleyicisi oluşturma](#Exercise3)

Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**

> [!NOTE]
> Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir. Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Alıştırma 1: yeni bir Web Forms projesi oluşturma

Bu alıştırmada, Visual Studio 2013 kullanarak yeni bir Web Forms site oluşturacak **bir ASP.NET** kolayca Web Forms, MVC ve Web API bileşenleri aynı uygulamada tümleştirmenize olanak tanır Proje deneyimi birleşik. Daha sonra oluşturulan çözüm keşfedin ve parçalarının tanımlamak ve son olarak eylem Web sitesini göreceksiniz.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Görev 1 – bir ASP.NET deneyimi kullanarak yeni bir Site oluşturma

Bu görevde, başlayacak Visual Studio'da yeni bir Web sitesi oluşturma temel **bir ASP.NET** proje türü. **Bir ASP.NET** tüm ASP.NET teknolojileri birleştirir ve karışık ve bunların istendiği gibi eşleştirmek için seçeneği sunar. Canlı Web Forms, MVC ve Web API'in farklı bileşenleri sonra yan yana ve uygulamanızdaki tanır.

1. Açık **için Visual Studio Express 2013 Web** seçip **dosya | Yeni proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **ASP.NET Web uygulaması** altında **Visual C# | Web** sekmesinde ve emin olun **.NET Framework 4.5** seçilir. Proje adı *MyHybridSite*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni ASP.NET Web uygulaması projesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Yeni bir ASP.NET Web uygulaması projesi oluşturma*
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu ve select **MVC** ve **Web API** seçenekleri. Ayrıca, olduğundan emin olun **kimlik doğrulaması** seçeneği **tek tek kullanıcı hesaplarını**. Devam etmek için **Tamam** 'a tıklayın.

    ![Web API ve MVC bileşenleri de dahil olmak üzere Web Forms şablonla yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Web API ve MVC bileşenleri de dahil olmak üzere Web Forms şablonla yeni proje oluşturma*
4. Şimdi oluşturulan çözüm yapısını da gözden geçirebilirsiniz.

    ![Oluşturulan çözüm keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Oluşturulan çözüm keşfetme*

    1. **Hesap:** bu klasör, kaydetme, oturum açın ve uygulamanın kullanıcı hesaplarını yönetmek için Web formu sayfalar içerir. Bu klasör eklenen **tek tek kullanıcı hesaplarını** kimlik doğrulaması seçeneği, Web Forms proje şablonu yapılandırması sırasında belirlenir.
    2. **Modelleri:** bu klasör, uygulama verilerini temsil eden sınıfları içerir.
    3. **Denetleyicileri** ve **görünümleri**: Bu klasörler için gerekli olan **ASP.NET MVC** ve **ASP.NET Web API** bileşenleri. Sonraki alıştırmalarda MVC ve Web API teknolojileri inceleyeceksiniz.
    4. **Default.aspx**, **Contact.aspx** ve **About.aspx** dosyalar için belirli sayfaları oluşturmak için başlangıç noktaları olarak kullanabileceğiniz önceden tanımlanmış Web Form sayfaları, uygulama. Bu dosyaların programlama mantığı olarak adlandırılan ayrı bir dosyada bulunan &quot;arka plan kodu&quot; sahip dosya bir &quot;. aspx.vb&quot; veya &quot;. aspx.cs&quot; uzantısı (bağlı olarak dili) kullanılır. Arka plan kodu mantığı sunucuda çalışır ve dinamik olarak sayfanızı için HTML çıkışında üretir.
    5. **Site.Master** ve **Site.Mobile.Master** sayfa, uygulamada Görünüm ve tüm sayfaları standart davranışını tanımlayın.
5. Çift **Default.aspx** sayfasının içeriği keşfetmek için dosya.

    ![Default.aspx sayfasında keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Default.aspx sayfasında keşfetme*

    > [!NOTE]
    > **Sayfa** komut dosyasının üst Web Forms sayfası öznitelikleri tanımlar. Örneğin, **MasterPageFile** öznitelik asıl yolunu belirtir sayfasında - bu durumda, *Site.Master* sayfa - ve **Inherits** öznitelik tanımlar. arka plandaki kod sınıfı sayfanın devralmak. Bu sınıf tarafından belirlenen dosya bulunan **arkasındaki koda** özniteliği.
    > 
    > **Asp: içerik** denetimi (metin, biçimlendirme ve denetimleri) sayfasının gerçek içeriği tutar ve eşlenmiş bir **asp: ContentPlaceHolder** ana sayfasında denetimi. Bu durumda, sayfa içeriği içinde işlenir *MainContent* tanımlanan Denetim *Site.Master* sayfası.
6. Genişletme **uygulama\_Başlat** klasörü ve bildirim **WebApiConfig.cs** dosya. Web API projenizi bir ASP.NET şablonla yapılandırırken içerdiğinden visual Studio bu dosyayı oluşturulan çözümdeki.
7. Açık **WebApiConfig.cs** dosya. İçinde *WebApiConfig* Web API, ile ilişkili yapılandırma HTTP eşlemeleri bulacaksınız sınıfı yolları için **Web API denetleyicilerinin**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Açık **RouteConfig.cs** dosya. İçinde *RegisterRoutes* HTTP rotaları eşler MVC ile ilişkili yapılandırma bulacaksınız yöntemi **MVC denetleyicileri**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümünü çalıştırma

Bu görevde oluşturulan çözümü çalıştırın, uygulama ve URL yeniden yazma işlemi ve yerleşik kimlik doğrulama gibi özelliklerinden bazılarını keşfedin.

1. Çözümü çalıştırmak için basın **F5** veya **Başlat** düğmesinin araç çubuğunda bulunan. Tarayıcıda uygulama giriş sayfası açmanız gerekir.

    ![Çözüm çalıştırma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Web formları sayfaları çağrılan doğrulayın. Bunu yapmak için sona **/contact.aspx** basın ve adres çubuğuna URL'yi için **Enter**.

    ![Kolay URL'ler](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Kolay URL'ler*

    > [!NOTE]
    > Gördüğünüz gibi URL değişikliklerini **/başvurun**. Başlayarak **ASP.NET 4**, URL yönlendirme özellikleri, Web formları için eklenmiştir, yazabileceğiniz şekilde URL'leri ister *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* yerine  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Daha fazla bilgi için bkz [URL yönlendirme](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Şimdi uygulamaya tümleşik kimlik doğrulaması akışı inceleyeceksiniz. Bunu yapmak için tıklatın **kaydetmek** sayfanın sağ üst köşesindeki.

    ![Yeni bir kullanıcı kaydetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Yeni bir kullanıcı kaydetme*
4. İçinde **kaydetmek** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetmek**.

    ![Kayıt sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Kayıt sayfası*
5. Yeni hesap uygulama kaydettirir ve bir kullanıcının kimliği doğrulanır.

    ![Kimliği doğrulanmış kullanıcı](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Kimliği doğrulanmış kullanıcı*
6. Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Alıştırma 2: İskele kullanarak bir MVC denetleyicisi oluşturma

Bu alıştırmada, Eylemler ve Razor görünümleri tek satırlık bir kod yazmak zorunda kalmadan CRUD işlemleri gerçekleştirmek için bir ASP.NET MVC 5 denetleyici oluşturmak için Visual Studio tarafından sağlanan ASP.NET yapı İskelesi çerçevesinden sürer. Yapı iskelesi süreci Entity Framework Code First veri bağlamı ve veritabanı şeması SQL veritabanında oluşturmak için kullanır.

**Entity Framework'ü ilk kod**

Entity Framework (EF) kavramsal uygulama modeli yerine doğrudan ilişkisel depolama Şeması'nı kullanarak programlama ile programlama tarafından veri erişimi uygulamaları oluşturmanızı sağlayan bir nesne ilişkisel Eşleyici (ORM) olur.

Entity Framework Code First modelleme iş akışı, sorgulama, gerçekleştirirken EF kullanır modeli temsil etmek için kendi etki alanı sınıflarını kullanmak değişiklik izleme ve güncelleştirme işlevleri sağlar. Code First geliştirme iş akışını kullanarak, uygulamanız bir veritabanı oluşturmayı veya bir şema belirterek başlamak gerekmez. Bunun yerine, uygulamanız için en uygun etki alanı model nesneleri tanımlamak standart .NET sınıfları yazabilir ve Entity Framework veritabanı sizin için oluşturur.

> [!NOTE]
> Entity Framework hakkında daha fazla bilgiyi [burada](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Görev 1 – yeni bir Model oluşturma

Şimdi tanımlayacak bir **kişi** MVC denetleyicisi ve görünümleri oluşturmak için yapı iskelesi işlem tarafından kullanılan model olacaktır sınıfı. Oluşturarak başlar bir **kişi** model sınıfı ve denetleyicideki CRUD işlemleri otomatik olarak oluşturulacak yapı iskelesi özelliklerini kullanma.

1. Açık **için Visual Studio Express 2013 Web** ve **MyHybridSite.sln** çözüm bulunan **kaynak/Ex2-MvcScaffolding/başlangıç** klasör. Alternatif olarak, önceki alıştırmada elde çözümüyle devam edebilirsiniz.
2. İçinde **Çözüm Gezgini**, sağ **modelleri** klasöründe **MyHybridSite** proje ve seçin **Ekle | Sınıf...** .

    ![Kişi model sınıfı ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Kişi model sınıfı ekleme*
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda, dosya adı *Person.cs* tıklatıp **Ekle**.

    ![Kişi model sınıfı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Kişi model sınıfı oluşturma*
4. İçeriğinin yerine geçecek **Person.cs** aşağıdaki kod ile dosya. Tuşuna **CTRL + S** değişiklikleri kaydedin.

    (Kod parçacığını - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. İçinde **Çözüm Gezgini**, sağ **MyHybridSite** proje ve seçin **yapı**, veya basın **CTRL + SHIFT + B** Projeyi derlemek için.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Görev 2 – bir MVC denetleyicisi oluşturma

Şimdi **kişi** modeli oluşturulur, CRUD denetleyici eylemleri ve görünümleri oluşturmak için Entity Framework ile ASP.NET MVC yapı iskelesi kullanacağı **kişi**.

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasöründe **MyHybridSite** proje ve seçin **Ekle | Yeni iskele kurulmuş öğe...** .

    ![Yeni iskele kurulmuş denetleyicisi oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Yeni iskele kurulmuş denetleyicisi oluşturma*
2. İçinde **İskele Ekle** iletişim kutusunda **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan** ve ardından **Ekle.**

    ![MVC 5 denetleyici görünümleri ve Entity Framework ile seçme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *MVC 5 denetleyici görünümleri ve Entity Framework ile seçme*
3. Ayarlama *MvcPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini ve **kişi (MyHybridSite.Models)**  olarak **Model sınıfı**.

    ![Yapı iskelesi ile MVC denetleyicisi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Yapı iskelesi ile MVC denetleyicisi ekleme*
4. Altında **veri bağlamı sınıfı**, tıklatın **yeni veri bağlamı...** .

    ![Yeni bir veri bağlamı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Yeni bir veri bağlamı oluşturma*
5. İçinde **yeni veri bağlamı** iletişim kutusunda, yeni veri bağlamı adı *PersonContext* tıklatıp **Ekle**.

    ![Yeni PersonContext oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Yeni PersonContext türü oluşturma*
6. Tıklatın **Ekle** için yeni denetleyicisi oluşturmak için **kişi** yapı iskelesi ile. Visual Studio, ardından denetleyici eylemleri, kişi veri bağlamı ve Razor görünümleri oluşturur.

    ![Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra*
7. Açık **MvcPersonController.cs** dosyasını **denetleyicileri** klasör. CRUD eylem yöntemlerine otomatik olarak oluşturulmuş olan dikkat edin.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Seçerek **zaman uyumsuz denetleyici eylemlerini kullanmak** yapı iskelesi onay kutusundan seçenekleri önceki adımlarda, Visual Studio'nun oluşturduğu kişi veri bağlamı erişimi ile ilgili tüm eylemleri için zaman uyumsuz eylem yöntemleri. Zaman uyumsuz eylem yöntemleri için uzun süre çalışan kullanın, istek gerçekleştirilirken iş gerçekleştirmeyi Web sunucusu engellemekten kaçınacak şekilde istekleri CPU olmayan bağlı önerilir.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3 – çözümünü çalıştırma

Bu görevde, görünümleri yeniden doğrulamak için çözümü çalıştıracak **kişi** beklendiği gibi çalıştığını. Veritabanına başarılı bir şekilde kaydedildiğini doğrulamak için yeni bir kişiye ekleyeceksiniz.

1. Tuşuna **F5** çözümü çalıştırın.
2. Gidin **/MvcPerson**. Kişilerin listesini gösteren kurulmuş görünümü görüntülenmesi gerekir.
3. Tıklatın **Yeni Oluştur** yeni bir kişiye eklemek için.

    ![Kurulmuş MVC görünümlerine gezinme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Kurulmuş MVC görünümlerine gezinme*
4. İçinde **oluşturma** görüntülemek, sağlayın bir **adı** ve bir **yaş** kişi ve tıklatın **oluşturma**.

    ![Yeni bir kişiye ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Yeni bir kişiye ekleme*
5. Yeni bir kişiye listesine eklenir. Öğe listesinde tıklayın **ayrıntıları** kişinin ayrıntıları görüntülemek için. Ardından **ayrıntıları** görüntülemek için tıklatın **listesine dön** liste görünümüne dönmek için.

    ![Kişinin Ayrıntılar görünümü](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Kişinin Ayrıntılar görünümü*
6. Tıklatın **silmek** kişiyi silmek için bağlantı. İçinde **silmek** görüntülemek için tıklatın **silmek** işlemini onaylamak için.

    ![Bir kişi silme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Bir kişi silme*
7. Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Alıştırma 3: İskele kullanan bir Web API denetleyicisi oluşturma

Web API çerçevesi ASP.NET yığını bir parçasıdır ve uygulama HTTP Hizmetleri, genel veri gönderme ve JSON veya XML biçimli bir RESTful API'si aracılığıyla alma kolaylaştırmak için tasarlanmıştır.

Bu alıştırmada, ASP.NET yapı İskelesi yeniden Web API denetleyicisi oluşturmak için kullanın. Aynı kullanacağınız **kişi** ve **PersonContext** JSON biçiminde aynı kişi verilerini sağlamak için önceki alıştırmada sınıflardan. Aynı ASP.NET uygulama içinde farklı şekillerde aynı kaynakları nasıl getirebilir görürsünüz.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Görev 1 – bir Web API denetleyicisi oluşturma

Bu görevde yeni oluşturacağınız **Web API denetleyicisi** JSON makine tüketilebilir formatta kişi verilerini kullanıma.

1. Henüz açık değilse açın **için Visual Studio Express 2013 Web** ve açın **MyHybridSite.sln** çözüm bulunan **kaynak/Ex3-Webapı/başlangıç** klasör. Alternatif olarak, önceki alıştırmada elde çözümüyle devam edebilirsiniz.

    > [!NOTE]
    > Alıştırma 3 başlangıç çözümü ile başlatırsanız, basın **CTRL + SHIFT + B** çözümü oluşturmak için.
2. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasöründe **MyHybridSite** proje ve seçin **Ekle | Yeni iskele kurulmuş öğe...** .

    ![Yeni iskele kurulmuş denetleyicisi oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Yeni iskele kurulmuş denetleyicisi oluşturma*
3. İçinde **İskele Ekle** iletişim kutusunda **Web API** sol bölmede, ardından **Web API 2 denetleyici Entity Framework kullanarak Eylemler ile** Orta bölmede ve 'ıtıklatın **Ekleyin.**

    ![Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "eylemleri ve Entity Framework ile Web API 2 denetleyicisi seçme")

    *Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme*
4. Ayarlama *ApiPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini ve **kişi (MyHybridSite.Models)**  ve **PersonContext (MyHybridSite.Models)** olarak **modeli** ve **veri bağlamı** sırasıyla sınıfları. Ardından **Ekle**.

    ![Yapı iskelesi ile bir Web API denetleyicisi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "yapı iskelesi ile Web API denetleyicisi ekleme")

    *Yapı iskelesi ile Web API denetleyicisi ekleme*
5. Visual Studio sonra oluşturacağını **ApiPersonController** verilerinizle çalışmaya dört CRUD eylemleri ile sınıfı.

    ![Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra")

    *Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra*
6. Açık **ApiPersonController.cs** dosya ve incelemek *GetPeople* eylem yöntemi. Bu yöntem db alanı sorgular **PersonContext** kişilerin veri alabilmek için türü.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Şimdi yöntem tanımı yukarıda yorum dikkat edin. İşlemin sonraki görev kullanacağı Bu eylem gösteren URI sağlar.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Varsayılan olarak, Web API sorguları yakalamak için yapılandırılmış */api* MVC denetleyicileri çakışmalarla önlemenin yolu. Bu ayarı değiştirmek gerekiyorsa, başvurmak [ASP.NET Web API'de yönlendirme](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümünü çalıştırma

Bu görevde, Internet Explorer kullanacağınız **F12 Geliştirici Araçları** Web API denetleyicisi tam yanıtı incelemek için. Uygulama verileriniz hakkında daha fazla bilgi almak için ağ trafiğini yakalamak için nasıl görürsünüz.

> [!NOTE]
> Olduğundan emin olun **Internet Explorer** seçildiyse **Başlat** düğmesi Visual Studio araç çubuğunda yer alan.
> 
> ![Internet Explorer seçeneği](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 Geliştirici Araçları** bu hands-on-laboratuvarda kapsanmayan işlevselliği geniş kümesine sahiptir. Hakkında daha fazla bilgi edinmek istiyorsanız, başvurmak [F12 geliştirme araçlarını kullanarak](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Tuşuna **F5** çözümü çalıştırın.

    > [!NOTE]
    > Bu görev doğru takip etmek için uygulamanızın veri olmalıdır. Veritabanınızı boşsa, alıştırma 2'deki görev 3 için geri dönün ve MVC görünümlerini kullanarak yeni bir kişiye oluşturma konusunda adımları izleyin.
2. Tarayıcıda basın **F12** açmak için **Geliştirici Araçları** paneli. Tuşuna **CTRL** + **4** veya **ağ** simgesine ve ardından ağ trafiğini yakalama başlamak için yeşil ok düğmesine.

    ![Web API ağ yakalama başlatma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "başlatma Web API ağ yakalama")

    *Web API ağ yakalama başlatılıyor*
3. Append **API/ApiPerson** tarayıcının adres çubuğundaki URL'ye. Yanıttan ayrıntılarını şimdi inceler **ApiPersonController**.

    ![Web API'si aracılığıyla kişi verilerini alma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API'si aracılığıyla kişi verilerini alma")

    *Web API'si aracılığıyla kişi verilerini alma*

    > [!NOTE]
    > Yükleme tamamlandıktan sonra indirilen dosya ile bir eylem yapmanız istenir. İletişim kutusu, geliştiricilerin araç penceresi aracılığıyla yanıt içeriği izlemek için açık bırakın.
4. Şimdi yanıtın gövdesini inceler. Bunu yapmak için tıklatın **ayrıntıları** sekmesini ve sonra **yanıt gövdesi**. Karşıdan yüklenen veri özelliklere sahip bir nesne listesi olup olmadığını denetleyin **kimliği**, **adı** ve **yaş** , karşılık **kişi** sınıf.

    ![Görüntüleme Web API yanıt gövdesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "görüntüleme Web API yanıt gövdesi")

    *Görüntüleme Web API yanıt gövdesi*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Görev 3 – Web API Yardım sayfaları ekleme

Bir Web API oluşturduğunuzda, diğer geliştiriciler API'nizi nasıl bilmesini Yardım sayfasını oluşturmak kullanışlıdır. Oluşturma ve belge sayfalarını el ile güncelleştirme, ancak otomatik bakım işi yapmak zorunda kalmamak için bunları oluşturma daha iyidir. Bu görevde bir Nuget paketi otomatik olarak Web API yardım sayfalarına çözümü oluşturmak için kullanın.

1. Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu yürütün:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** paket gerekli derlemeleri yükler ve MVC görünümleri altında yardım sayfalarına ekler **alanları/HelpPage** klasör.

    ![HelpPage alanı](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage alanı")

    *HelpPage alanı*
3. Varsayılan olarak, Yardım sayfaları sahip belgeleri için yer tutucu dize. XML belgeleri yorumları belgeleri oluşturmak için kullanabilirsiniz. Bu özelliği etkinleştirmek için açık **HelpPageConfig.cs** bulunan dosya **HelpPage/alanları/uygulama\_Başlat** klasörü ve aşağıdaki satırı açıklamadan çıkarın:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. İçinde **Çözüm Gezgini**, projeye sağ tıklayın **MyHybridSite**seçin **özellikleri** tıklatıp **yapı** sekmesi.

    ![Sekme yapı](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "yapı bölümü")

    *Sekme oluşturma*
5. Altında **çıkış**seçin **XML belge dosyası**. Düzenleme kutusuna **uygulama\_Data/XmlDocument.xml**.

    ![Çıktı yapı sekmesini bölümünde](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "çıkış yapı sekmesini bölümünde")

    *Çıktı bölümünde yapı sekmesi*
6. Tuşuna **CTRL** + **S** değişiklikleri kaydedin.
7. Açık **ApiPersonController.cs** dosya **denetleyicileri** klasör.
8. Yeni bir satır arasında girin *GetPeople* yöntem imzası ve */ / GET API/ApiPerson* açıklama ve üç eğik yazın.

    > [!NOTE]
    > Visual Studio yöntemi belgelerine tanımlayan XML öğeleri otomatik olarak ekler.
9. Bir Özet metni ve dönüş değeri eklemek *GetPeople* yöntemi. Aşağıdaki gibi görünmelidir.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Tuşuna **F5** çözümü çalıştırın.
11. Append **/Yardım** yardım sayfasına göz atmak için adres çubuğundaki URL'ye.

    ![ASP.NET Web API Yardım sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Yardım sayfası")

    *ASP.NET Web API Yardım sayfası*

    > [!NOTE]
    > Ana sayfanın API'leri, denetleyici tarafından gruplandırılmış bir tablo içeriktir. Tablo girişleri kullanarak dinamik olarak üretilen **IApiExplorer** arabirimi. Eklemek veya bir API denetleyicisi güncelleştirmek tablo uygulamayı derlediğinizde başlatıldığında otomatik olarak güncelleştirilecektir.
    > 
    > **API** sütunu listeler göreli URI ve HTTP yöntemi. **Açıklama** sütun yöntemin belgelerinden ayıklanan bilgiler içerir.
12. Yöntem tanımını eklenen açıklamayı açıklama sütununda görüntülendiğine dikkat edin.

    ![API yöntemi açıklaması](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API yöntemi açıklaması")

    *API yöntemi açıklaması*
13. Örnek yanıt gövdesi dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfa gitmek için API yöntemlerini birini tıklatın.

    ![Ayrıntılı bilgi sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "ayrıntı bilgileri sayfası")

    *Ayrıntılı bilgileri sayfası*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı Laboratuvar tamamlayarak nasıl yapılır:

- Visual Studio 2013'te bir ASP.NET deneyimi kullanarak yeni bir Web uygulaması oluşturma
- Birden çok ASP.NET teknolojileri tek tek projeye tümleştirme
- MVC denetleyicilerinin ve görünümlerin ASP.NET yapı İskelesi kullanarak modeli sınıflardan oluştur
- Zaman uyumsuz programlama ve Entity Framework ile veri erişim gibi özellikleri kullanan Web API denetleyicilerinin oluştur
- Web API Yardım sayfaları denetleyicileriniz için otomatik olarak oluştur
