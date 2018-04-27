---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: ASP.NET Web API ile RESTful API'lerini yapı | Microsoft Docs
author: rick-anderson
description: Son yıllarda Temizle HTTP HTML sayfaları yalnızca hizmet vermek için olmadığından emin olur. Web API'leri, bazı o kullanarak oluşturmak için de güçlü bir platformdur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ded549109ca6e7ad806f1c3f53387766527e5a94
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>ASP.NET Web API ile RESTful API'lerini derleme
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> Son yıllarda Temizle HTTP HTML sayfaları yalnızca hizmet vermek için olmadığından emin olur. Ayrıca Web API'leri, birkaç basit kavramları gibi fiiller (GET, POST ve benzeri) sayıda kullanarak oluşturmak için güçlü bir platform olan *URI'ler* ve *üstbilgileri*. ASP.NET Web API HTTP programlama basitleştirmek bileşenleri kümesidir. ASP.NET MVC çalışma zamanı üzerine inşa edildiğinden, Web API HTTP alt düzey taşıma ayrıntılarını otomatik olarak yönetir. Aynı zamanda Web API HTTP programlama modeli doğal olarak kullanıma sunar. Aslında, bir Web API için hedefidir *değil* hemen HTTP gerçekte soyut. Sonuç olarak, Web API esnek ve kolay genişletmek ' dir. Bu uygulamalı laboratuarda bir kişi manager uygulaması için basit bir REST API oluşturmak için Web API'sini kullanır. Ayrıca, API kullanmak için bir istemci oluşturacaksınız. Bunu kesinlikle yalnızca geçerli bir yaklaşım HTTP olmasa da HTTP - yararlanmak için etkili bir yol olması REST mimari stili kanıtlamış. Kişi Yöneticisi, listeleme, ekleme ve diğerlerinin yanı sıra kişiler kaldırma için RESTful açığa çıkarır. Bu Laboratuvar, HTTP, REST, temel bir anlayış gerektirir ve HTML, JavaScript ve jQuery temel bilgiye sahip olduğunu varsayar.
> 
> > [!NOTE]
> > ASP.NET Web sitesi ASP.NET Web API çerçevesi için ayrılmış bir alana sahip [ https://asp.net/web-api ](https://asp.net/web-api). Bu site, en son haberler, örnekler ve Web API'sine ilgili haberleri nedenle denetleyin, sık sağlamaya devam edecek özel Web API'leri neredeyse tüm aygıt ya da geliştirme framework kullanılabilir oluşturma resmi içine daha derin inceleyin isteyip istemediğinizi.
> > 
> > ASP.NET Web API, ASP.NET MVC 4'e benzer birkaç kullanılabilir bağımlılık ekleme çerçeveleri oldukça kullanmanızı sağlayan denetleyicilerinden hizmet katmanı ayırarak açısından büyük esneklik vardır. ASP.NET Web API projesinde buradan indirebilirsiniz bağımlılık ekleme için Ninject kullanmayı gösterir MSDN'de iyi bir örnek yok [burada](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- RESTful Web API'si uygulama
- Bir HTML İstemcisi API çağrısından

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleme**

Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir. Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek A: kullanarak kod parçacıkları](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar alıştırma içerir:

1. [Alıştırma 1: salt okunur bir Web API oluşturma](#Exercise1)
2. [Alıştırma 2: okuma/yazma Web API'si oluşturma](#Exercise2)
3. [Alıştırma 3: bir HTML istemcisinden gelen Web API kullanma](#Exercise3)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Alıştırma 1: salt okunur bir Web API oluşturma

Bu alıştırmada, salt okunur GET yöntemleri için Kişi Yöneticisi gerçekleştireceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Görev 1 - API projesi oluşturma

Bu görevde, bir Web API web uygulaması oluşturmak için yeni ASP.NET web projesi şablonları kullanır.

1. Çalıştırma **için Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **VS Express Web** tuşuna basarak **Enter**.
2. Gelen **dosya** menüsünde, select **yeni proje**. Seçin **Visual C# | Web** proje türü proje türü ağaç görünümünden sonra seçin **ASP.NET MVC 4 Web uygulaması** proje türü. Projenin ayarlamak **adı** için *ContactManager* ve **çözüm adı** için *başlamak*, ardından **Tamam**.

    ![Yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image1.png "yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma")

    *Yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma*
3. ASP.NET MVC 4 proje türü iletişim seçin **Web API** proje türü. **Tamam**'ı tıklatın.

    ![Web API projesi türünü belirtme](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API projesi türünü belirtme")

    *Web API projesi türünü belirtme*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Görev 2 - Contact Manager API denetleyicisi oluşturma

Bu görevde, API yöntemlerini yer alacağı denetleyicisi sınıfları oluşturur.

1. Adlı dosyayı silin **ValuesController.cs** içinde **denetleyicileri** proje klasöründen.
2. Sağ **denetleyicileri** seçin ve proje klasöründe **Ekle | Denetleyici** ve bağlam menüsünden.

    ![Projeye yeni bir denetleyicisi ekleme](build-restful-apis-with-aspnet-web-api/_static/image3.png "projeye yeni bir denetleyicisi ekleme")

    *Projeye yeni bir denetleyicisi ekleme*
3. İçinde **denetleyici Ekle** görünen seçin iletişim **boş API denetleyicisi** şablonu menüsünde. Denetleyici sınıfı adı **ContactController**. Ardından **Ekle.**

    ![Yeni bir Web API denetleyicisi oluşturmak için denetleyici Ekle iletişim kutusu kullanılarak](build-restful-apis-with-aspnet-web-api/_static/image4.png "denetleyici Ekle iletişim kutusunu kullanarak yeni bir Web API denetleyicisi oluşturmak için")

    *Denetleyici Ekle iletişim kutusunu kullanarak yeni bir Web API denetleyicisi oluşturmak için*
4. Aşağıdaki kodu ekleyin **ContactController**.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - alma API yöntemi*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Tuşuna **F5** uygulamada hata ayıklama için. Bir Web API projesi için varsayılan giriş sayfası gösterilmelidir.

    ![Bir ASP.NET Web API uygulamasının varsayılan giriş sayfası](build-restful-apis-with-aspnet-web-api/_static/image5.png "bir ASP.NET Web API uygulamasının varsayılan giriş sayfası")

    *Bir ASP.NET Web API uygulamasının varsayılan giriş sayfası*
6. Internet Explorer penceresinde basın **F12** açmak için anahtar **Geliştirici Araçları** penceresi. Tıklatın **ağ** sekmesini ve ardından **Başlat yakalama** penceresine ağ trafiğini yakalama başlamak için düğmesini.

    ![Ağ yakalama ağ sekmesini açmak ve başlatma](build-restful-apis-with-aspnet-web-api/_static/image6.png "ağ yakalama ağ sekmesini açmak ve başlatma")

    *Ağ sekmesini açmak ve ağ yakalama başlatma*
7. İle tarayıcının adres çubuğundaki URL Ekle **/api/kişi** ve ENTER tuşuna basın. İletim ayrıntıları Ağ Yakalama penceresinde görünür. Yanıtın MIME türü Not **uygulama/json**. Bu, varsayılan çıkış biçimi JSON nasıl gerçekleştiğini gösterir.

    ![Web API isteği çıktısını ağ görünümünde görüntüleyerek](build-restful-apis-with-aspnet-web-api/_static/image7.png "Web API isteği çıktısını ağ görünümünde görüntüleme")

    *Web API isteği çıktısını ağ görünümünde görüntüleme*

    > [!NOTE]
    > Internet Explorer 10'ın varsayılan davranışı, kullanıcının kaydetmek veya Web API çağrısından kaynaklanan akış açmak isteyip istemediğini sormak için bu noktada olacaktır. Çıkış Web API'si URL çağrı JSON sonucunu içeren bir metin dosyası olacaktır. İletişim kutusu, geliştiricilerin araç penceresi yoluyla yanıtın içeriği izlemek için iptal eder.
8. Tıklatın **ayrıntılı görünümüne gidin** bu API çağrısının yanıtı hakkında daha fazla ayrıntı görmek için düğmesi.

    ![Geçiş için Ayrıntılı Görünüm](build-restful-apis-with-aspnet-web-api/_static/image8.png "anahtar için Ayrıntılar görünümü")

    *Ayrıntılı görünümüne geçin*
9. Tıklatın **yanıt gövdesi** gerçek JSON yanıt metni görüntülemek için.

    ![JSON görüntüleme çıktı metin Ağ İzleyicisi'nde](build-restful-apis-with-aspnet-web-api/_static/image9.png "çıktı metin Ağ İzleyicisi'nde JSON görüntüleme")

    *Ağ İzleyicisi'nde JSON çıktı metin görüntüleme*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Görev 3 - kişi modelleri oluşturma ve kişi denetleyicisi büyütmek

Bu görevde, API yöntemlerini yer alacağı denetleyicisi sınıfları oluşturur.

1. Sağ **modelleri** klasörü ve select **Ekle | Sınıf...**  ve bağlam menüsünden.

    ![Web uygulaması için yeni bir model ekleme](build-restful-apis-with-aspnet-web-api/_static/image10.png "web uygulaması için yeni bir modeli ekleme")

    *Web uygulaması için yeni bir modeli ekleme*
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda, yeni dosya adı **Contact.cs** tıklatıp **Ekle.**

    ![Yeni kişi sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image11.png "yeni kişi sınıf dosyası oluşturma")

    *Yeni kişi sınıf dosyası oluşturma*
3. Aşağıdaki vurgulanmış kodu ekleyin **kişi** sınıfı.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi sınıfı*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. İçinde **ContactController** sınıfı, word seçin **dize** yöntemi tanımındaki **almak** yöntemi ve türünü word *kişi*. Word yazılmış sonra bir gösterge sözcüğün başında görünür **kişi**. Ya da basılı **Ctrl** anahtar ve nokta (.) tuşuna basın veya Yardım iletişim kutusu otomatik olarak doldurmak için kod düzenleyicisinde açmak için farenizi kullanarak simgesini **kullanarak** modelleri için yönergesi ad alanı.

    ![Ad alanı bildirimleri için IntelliSense Yardım'ı kullanarak](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Ad alanı bildirimleri için IntelliSense Yardım'ı kullanarak*
5. Kodunu değiştirmek **almak** olan kişi model örnekleri bir dizi döndürecek şekilde yöntemi.

    (Kod parçacığını - *Web API kişilerin listesini döndürme Laboratuvar - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Tuşuna **F5** tarayıcıda web uygulamasını hata ayıklamak için. API yanıt çıkışı yapılan değişiklikleri görmek için aşağıdaki adımları gerçekleştirin.

   1. Tarayıcı açılır sonra basın **F12** geliştirici araçları henüz açık değilse.
   2. Tıklatın **ağ** sekmesi.
   3. Tuşuna **Başlat yakalama** düğmesi.
   4. URL soneki eklemek **/api/kişi** basın ve adres çubuğuna URL'yi için **Enter** anahtarı.
   5. Tuşuna **ayrıntılı görünümüne gidin** düğmesi.
   6. Seçin **yanıt gövdesi** sekmesi. Kişi örnekleri bir dizi serileştirilmiş biçiminde temsil eden bir JSON dizesinde görmeniz gerekir.

      ![JSON serileştirilmiş karmaşık Web API yöntem çağrısının çıktısını](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serileştirilmiş karmaşık Web API yöntem çağrısı çıktısı")

      *Karmaşık Web API yöntem çağrısının çıktısını JSON seri hale getirilmiş*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Görev 4 - bir hizmet katmanı ayıklanan işlevsellik

Bu görev hizmeti işlevleri böylece gerçekte yapması Hizmetleri kullanılırlığı izin vererek denetleyicisi katmandan ayırmak geliştiricilere kolaylaştırmak için bir hizmet katmanı işlevsellik çıkarmayı gösterilmektedir.

1. Çözüm kök dizininde yeni bir klasör oluşturun ve adlandırın **Hizmetleri**. Bunu yapmak için sağ **ContactManager** proje, select **Ekle** | **yeni klasör**, adlandırın *Hizmetleri*.

    ![Oluşturma Hizmetleri klasörü](build-restful-apis-with-aspnet-web-api/_static/image14.png "oluşturma hizmetleri klasörü")

    *Hizmetleri klasörü oluşturma*
2. Sağ **Hizmetleri** klasörü ve select **Ekle | Sınıf...**  ve bağlam menüsünden.

    ![Yeni bir sınıf Hizmetleri klasörü ekleme](build-restful-apis-with-aspnet-web-api/_static/image15.png "Hizmetleri klasörü için yeni bir sınıf ekleme")

    *Yeni bir sınıf Hizmetleri klasörü ekleme*
3. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenirse, yeni sınıfın adını **ContactRepository** tıklatıp **Ekle**.

    ![Kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image16.png "kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma")

    *Kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma*
4. Kullanarak bir eklemek için yönerge **ContactRepository.cs** dosyasını modelleri ad alanı içerecek şekilde.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. Aşağıdaki vurgulanmış kodu ekleyin **ContactRepository.cs** GetAllContacts yöntemi uygulamak için dosya.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi depo*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Açık **ContactController.cs** zaten açık değilse, dosya.
7. Aşağıdaki ekleme deyimini dosyanın ad alanı bildirimi bölümünü kullanarak.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. Aşağıdaki vurgulanmış kodu ekleyin **ContactController.cs** üyeleri yapabilir sınıfı rest hizmeti uygulaması kullanabileceğiniz deposu örneğini temsil etmek için özel bir alan eklemek için sınıfı.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi denetleyicisi*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Değişiklik **almak** onun yapar şekilde yöntemi kişi Deposu hizmetini kullanın.

    (Kod parçacığını - *Web API kişilerin listesini havuz döndürme Laboratuvar - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Bir kesme noktası yerleştirin **ContactController**'s **almak** yöntemi tanımı.

   ![Kesme noktaları kişi denetleyiciye ekleme](build-restful-apis-with-aspnet-web-api/_static/image17.png "kişi denetleyiciye kesme noktaları ekleme")

   *Kişi denetleyiciye kesme noktaları ekleme*
11. Tuşuna **F5** uygulamayı çalıştırın.
12. Tarayıcı açıldığında basın **F12** Geliştirici Araçları'nı açın.
13. Tıklatın **ağ** sekmesi.
14. Tıklatın **Başlat yakalama** düğmesi.
15. Soneki adres çubuğundaki URL'yi ekleme **/api/kişi** ve basın **Enter** API denetleyicisi yüklemek için.
16. Visual Studio 2012 bölün kez **almak** yöntemi yürütülmesine başlar.

   ![Get yöntemi içinde çiğnemekten](build-restful-apis-with-aspnet-web-api/_static/image18.png "içinde Get yöntemi bölme")

   *Get yöntemi içinde sonu*
17. Tuşuna **F5** devam etmek için.
18. Odak değilse geri Internet Explorer'a gidin. Ağ yakalama penceresi unutmayın.

    ![Internet Explorer'da Web API çağrısının sonuçlarını gösteren görünüm ağ](build-restful-apis-with-aspnet-web-api/_static/image19.png "ağ Internet Explorer'da Web API çağrısının sonuçlarını gösteren görünüm")

    *Internet Explorer'da Web API çağrısının sonuçlarını gösteren ağ görünümü*
19. Tıklatın **ayrıntılı görünümüne gidin** düğmesi.
20. Tıklatın **yanıt gövdesi** sekmesi. API çağrısı JSON çıktısını not alın ve hizmet katmanı tarafından alınan nasıl iki kişiler temsil eder.

    ![Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme](build-restful-apis-with-aspnet-web-api/_static/image20.png "Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme")

    *Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Alıştırma 2: okuma/yazma Web API'si oluşturma

Bu alıştırmada, POST uygulayacak ve veri düzenleme özellikleri ile etkinleştirmek için Kişi Yöneticisi PUT yöntemleri.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Görev 1 - Web API projesi açma

Bu görevde, böylece kullanıcı girişi kabul edebilir alıştırma 1'de oluşturulan Web API projesi geliştirmek hazırlarsınız.

1. Çalıştırma **için Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **VS Express Web** tuşuna basarak **Enter**.
2. Açık **başlamak** çözüm bulunan **kaynak/Ex02-ReadWriteWebAPI/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **Services/ContactRepository.cs** dosya.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Görev 2 - kişi depo uygulamasına veri kalıcılığını Özellikler ekleme

Bu görevde, böylece kalıcı hale getirmek ve kullanıcı girişi ve yeni kişi örnekleri kabul alıştırma 1'de oluşturulan Web API projesi ContactRepository sınıfının büyütmek.

1. Aşağıdaki sabit eklemek **ContactRepository** web sunucusu önbellek öğesi anahtar adı daha sonra Bu alıştırmada adını temsil eden sınıf.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Bir oluşturucu ekleyin **ContactRepository** aşağıdaki kodu içeren.

    (Kod parçacığını - *Web API Laboratuvar - Ex02 - kişi depo Oluşturucusu*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Kodunu değiştirmek **GetAllContacts** aşağıda gösterildiği gibi yöntemi.

    (Kod parçacığını - *Web API Laboratuvar - Ex02 - tüm kişileri Al*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Bu örnek tanıtım amaçlıdır ve değerleri aynı anda birden çok istemciler için kullanılabilir yerine böylece oturum depolama mekanizmasını veya bir istek depolama ömrü kullanın web sunucusunun önbellek depolama ortamı olarak kullanır. Bir Entity Framework, XML depolama ya da diğer birçok web sunucusu önbellek yerine kullanabilirsiniz.
4. Adlı yeni bir yöntem **SaveContact** için **ContactRepository** kişi kaydetme işlemlerini yapmak için sınıf. **SaveContact** yöntemi, tek bir almalıdır **kişi** belirten başarı veya başarısızlık parametre ve dönüş bir Boole değeri.

    (Kod parçacığını - *API SaveContact yöntemi uygulama Laboratuvar - Ex02 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Alıştırma 3: bir HTML istemcisinden gelen Web API kullanma

Bu alıştırmada, Web API'sini çağırmak için bir HTML istemci oluşturur. Bu istemci JavaScript kullanarak Web API'si ile veri değişimi kolaylaştırmak ve sonuçları bir HTML biçimlendirmesi kullanarak bir web tarayıcısında görüntülenir.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Görev 1 - kişiler görüntülemek için bir GUI sağlamak için dizin görünümü değiştirme

Bu görevde, bir HTML tarayıcısında varolan kişilerinizin listesini görüntüleme gereksinimi desteklemek için web uygulamasının varsayılan dizini görünümünü değiştirir.

1. Açık **için Visual Studio 2012 Express Web** zaten açık değilse.
2. Açık **başlamak** çözüm bulunan **kaynak/Ex03-ConsumingWebAPI/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **Index.cshtml** konumunda bulunan dosya **görünümler/giriş** klasör.
4. Div öğesinin içinde HTML kod kimliğiyle değiştirin **gövde** böylece aşağıdaki kod gibi görünüyor.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. Web API HTTP isteği gerçekleştirmek için dosyanın sonuna şu Javascript kodunu ekleyin.


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. Açık **ContactController.cs** zaten açık değilse, dosya.
7. Bir kesme noktası yerleştirin **almak** yöntemi **ContactController** sınıfı.

    ![API denetleyicisi Get yöntemini bir kesme noktası yerleştirme](build-restful-apis-with-aspnet-web-api/_static/image21.png "API denetleyicisi Get yöntemini bir kesme noktası yerleştirme")

    *API denetleyicisi Get yöntemini bir kesme noktası yerleştirme*
8. Tuşuna **F5** projeyi çalıştırın. Tarayıcı, HTML belgesinde yükler.

    > [!NOTE]
    > Uygulamanızın kök URL'sini tarama emin olun.
9. Sayfa yükler ve JavaScript yürütür sonra kesme noktası isabet ve kod yürütmeyi denetleyicide duraklatılır.

    ![Web API çağrıları VS Express kullanarak Web uygulamasına hata ayıklama](build-restful-apis-with-aspnet-web-api/_static/image22.png "VS Express Web kullanarak Web API çağrıları halinde hata ayıklama")

    *Web için Visual Studio Express 2012 kullanarak Web API çağrısı içine hata ayıklama*
10. Tuşuna basın ve kesme noktası kaldırma **F5** veya hata ayıklama araç çubuğunun **devam** tarayıcıda görüntüle yüklemeye devam etmek için düğmesi. Web API çağrısı tamamlandığında tarayıcıda listesi öğeleri olarak görüntülenen çağrı Web API öğesinden geri döndürülen kişiler görmeniz gerekir.

    ![Liste öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını](build-restful-apis-with-aspnet-web-api/_static/image23.png "listesi öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını")

    *Liste öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını*
11. Hata ayıklamayı durdurun.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Görev 2 - kişiler oluşturmak için bir GUI sağlamak için dizin görünümü değiştirme

Bu görevde, MVC uygulaması dizini görünümünü değiştirmeye devam eder. Bir form kullanıcı girişi yakalama ve yeni bir kişi oluşturmak için Web API Gönder HTML sayfası eklenir ve yeni bir Web API denetleyicisi yöntemi tarih GUI toplamak için oluşturulacak.

1. Açık **ContactController.cs** dosya.
2. Yeni bir yöntem adlı denetleyici sınıfına ekleyin **Post** aşağıdaki kodda gösterildiği gibi.

    (Kod parçacığını - *Web API Laboratuvar - Ex03 - Post yöntemini*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. Açık **Index.cshtml** zaten açık değilse, Visual Studio'da dosya.
4. Aşağıdaki HTML kod, yalnızca önceki görevde eklenen sırasız liste sonra dosyasına ekleyin.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. Belgenin sonuna betik öğesi içinde bir HTTP POST çağrısı kullanarak Web API'sine verileri gönderecek düğme tıklama olayları işlemek için aşağıdaki vurgulanmış kodu ekleyin.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. İçinde **ContactController.cs**, bir kesme noktası yerleştirin **Post** yöntemi.
7. Tuşuna **F5** tarayıcıda uygulamayı çalıştırın.
8. Sayfa tarayıcıda yüklendikten sonra yeni ilgili kişi adı ve kimliği ve tıklatın yazın **kaydetmek** düğmesi.

    ![İstemci HTML belgesi tarayıcıda yüklenen](build-restful-apis-with-aspnet-web-api/_static/image24.png "istemci HTML belgesi tarayıcıda yüklenen")

    *Tarayıcıda yüklenen istemci HTML belgesi*
9. Hata ayıklayıcı penceresini zaman sonları **Post** yöntemi, Al özelliklerini göz bir **başvurun** parametresi. Değerler biçiminde girilen verilerin eşleşmesi gerekir.

    ![Web API İstemci tarafından gönderilen kişi nesnesi](build-restful-apis-with-aspnet-web-api/_static/image25.png "Web API İstemci tarafından gönderilen kişi nesnesi")

    *Web API İstemci tarafından gönderilen kişi nesnesi*
10. Hata ayıklayıcı kadar yöntemiyle adım **yanıt** değişkeni oluşturuldu. İncelemesinin bağlı **Yereller** hata ayıklayıcı penceresinde, tüm özellikler ayarlandıktan göreceksiniz.

   ![Hata ayıklayıcı oluşturma aşağıdaki yanıt](build-restful-apis-with-aspnet-web-api/_static/image26.png "hata ayıklayıcısında oluşturma aşağıdaki yanıt")

   *Hata ayıklayıcı oluşturma aşağıdaki yanıt*
11. Basarsanız **F5** veya **devam** Hata Ayıklayıcısı'ndaki istek tamamlayacak. Tarayıcıya geçiş yaptıktan sonra yeni kişi tarafından depolanan kişiler listesine eklenmiş olan **ContactRepository** uygulaması.

   ![Yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır](build-restful-apis-with-aspnet-web-api/_static/image27.png "yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır")

   *Yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır*

> [!NOTE]
> Ayrıca, Azure aşağıdaki bu uygulamayı dağıtabilmek için [ek C: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu Laboratuvar yeni ASP.NET Web API çerçevesi ve framework kullanarak RESTful Web API uygulaması için sunulan. Buradan, herhangi bir sayıda mekanizmalarını kullanarak veri kalıcılığını kolaylaştıran yeni bir havuz oluşturabilir ve bu laboratuarda bir örnek olarak sağlanan Basit bir yerine yukarı hizmet wire. Web API HTTP ve JSON veya XML destekleyen herhangi bir dilde yazılmış olarak HTML olmayan istemcilerden gelen iletişimi etkinleştirme gibi ek özellikleri destekler. Tipik web uygulaması dışında bir Web API barındırmak için özelliği de mümkündür, aynı zamanda kendi serileştirme biçimleri oluşturma yeteneği.

ASP.NET Web sitesi ASP.NET Web API çerçevesi için ayrılmış bir alana sahip [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Bu site, en son haberler, örnekler ve Web API'sine ilgili haberleri nedenle denetleyin, sık sağlamaya devam edecek özel Web API'leri neredeyse tüm aygıt ya da geliştirme framework kullanılabilir oluşturma resmi içine daha derin inceleyin isteyip istemediğinizi.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Ek A: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](build-restful-apis-with-aspnet-web-api/_static/image28.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

    ![Kod parçacığında adını yazmaya başlayın](build-restful-apis-with-aspnet-web-api/_static/image29.png "parçacığı adını yazmaya başlayın")

    *Kod parçacığında adını yazmaya başlayın*

    ![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](build-restful-apis-with-aspnet-web-api/_static/image30.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

    *Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

    ![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](build-restful-apis-with-aspnet-web-api/_static/image31.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

    *Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için

1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.
2. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
3. Tıklayarak ilgili kod parçacığında listeden seçin.

    ![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](build-restful-apis-with-aspnet-web-api/_static/image32.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

    *Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

    ![Tıklayarak ilgili kod parçacığında listeden çekme](build-restful-apis-with-aspnet-web-api/_static/image33.png "tıklayarak ilgili kod parçacığında listeden seçin")

    *Tıklayarak ilgili kod parçacığında listeden seçin*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Ek B: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Azure SDK'sı Web</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](build-restful-apis-with-aspnet-web-api/_static/image34.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express Web döşemeye*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek C: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

Bu ekte Azure Portal'dan yeni bir web sitesi oluşturma ve Laboratuvar izleyerek Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Görev 1 - Azure portalından yeni bir Web sitesi oluşturma

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure portalında oturum açtığı](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure Portal'da oturum açın")

    *Portal'da oturum açın*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image40.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Azure, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için bir ana bilgisayardır. Hızlı oluştur seçeneği Azure portal dışındaki bir tamamlanmış bir web uygulamasını dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image41.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](build-restful-apis-with-aspnet-web-api/_static/image42.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](build-restful-apis-with-aspnet-web-api/_static/image43.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](build-restful-apis-with-aspnet-web-api/_static/image44.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* Azure her etkin yayımlama yöntemi için bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Azure Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](build-restful-apis-with-aspnet-web-api/_static/image45.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Azure Visual Studio'dan bir web uygulamasına yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](build-restful-apis-with-aspnet-web-api/_static/image46.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) düğmesi.

    ![İstemci IP adresi ekleme](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Değişiklikleri onaylamak*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](build-restful-apis-with-aspnet-web-api/_static/image51.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](build-restful-apis-with-aspnet-web-api/_static/image52.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](build-restful-apis-with-aspnet-web-api/_static/image53.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
   - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
   - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
   - Yeni bir veritabanı adı girin: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image55.png "hedef bağlantı dizesi yapılandırma")

     *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](build-restful-apis-with-aspnet-web-api/_static/image56.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](build-restful-apis-with-aspnet-web-api/_static/image58.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

    ![Uygulama için Windows Azure yayımlanan](build-restful-apis-with-aspnet-web-api/_static/image59.png "uygulama için Windows Azure yayımlanır")

    *Azure için yayımlanan uygulama*
