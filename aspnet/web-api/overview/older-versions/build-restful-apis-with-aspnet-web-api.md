---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: ASP.NET Web API ile RESTful API'leri oluşturmak | Microsoft Docs
author: rick-anderson
description: Son yıllarda, HTML sayfaları yalnızca hizmet vermek için HTTP değil olduğu açıkça görülmektedir oldu. Ayrıca, bir talebin o kullanarak Web API'leri oluşturmaya yönelik güçlü bir platform olan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363526"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>ASP.NET Web API ile RESTful API'leri oluşturma
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

> Son yıllarda, HTML sayfaları yalnızca hizmet vermek için HTTP değil olduğu açıkça görülmektedir oldu. Ayrıca birkaç fiiller (GET, POST vb.) gibi birkaç basit kavramları kullanarak Web API'leri oluşturmaya yönelik güçlü bir platform olan *URI'ler* ve *üstbilgileri*. ASP.NET Web API HTTP programlamayı bileşenleri kümesidir. ASP.NET MVC çalışma zamanı üzerine inşa edildiğinden, Web API HTTP alt düzey aktarım ayrıntılarını otomatik olarak işler. Aynı zamanda, Web API'si, doğal olarak HTTP programlama modeli sunar. Aslında, bir Web API'sinin olmaktır *değil* hemen HTTP gerçekte soyut. Sonuç olarak, Web API'si genişletmek kolay ve esnek. Bu uygulamalı laboratuvarda bir kişi manager uygulaması için basit bir REST API oluşturmak için Web API'sini kullanır. Ayrıca, API'yi kullanmak için bir istemci oluşturacaksınız. Bunu kesinlikle HTTP yalnızca geçerli bir yaklaşım olmasa da HTTP - yararlanmak için verimli bir yöntem olabilir REST mimari stili kanıtlanmıştır. Kişi Yöneticisi, listeleme, ekleme ve kaldırma kişiler, diğerlerinin yanı sıra RESTful açığa çıkarır. Bu Laboratuvar, HTTP, REST, temel bir anlayış gerektirir ve HTML, JavaScript ve jQuery temel bilgiye sahip olduğunu varsayar.
> 
> > [!NOTE]
> > ASP.NET Web sitesinde ASP.NET Web API çerçevesi ayrılmış bir alan yok [ https://asp.net/web-api ](https://asp.net/web-api). Bu site, en son haberler, örnekler ve Web API'si için ilgili Haberler şekilde kontrol edin, sık sağlamaya devam edecek içine neredeyse tüm cihaz veya geliştirme framework kullanılabilir özel Web API'leri oluşturmanın son teknoloji ürünü derin delve istiyorsanız.
> > 
> > ASP.NET Web API, ASP.NET MVC 4'e benzer birkaç kullanılabilir bağımlılık ekleme çerçevesini oldukça kolay kullanmanıza olanak sağlayan denetleyicilerinden Hizmet katmanını ayırarak açısından harika esnekliğine sahiptir. Olduğu iyi bir örnek için bağımlılık ekleme buradan indirebileceğiniz bir ASP.NET Web API projesinde Ninject kullanmayı gösterir MSDN [burada](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- RESTful Web API'si uygulama
- Bir HTML istemcisinde API çağırma

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleniyor**

Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir. Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek A: kullanarak kod parçacıklarına](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmada içerir:

1. [Alıştırma 1: salt okunur bir Web API'si oluşturma](#Exercise1)
2. [Alıştırma 2: okuma/yazma Web API'si oluşturma](#Exercise2)
3. [Alıştırma 3: bir HTML istemci Web API kullanma](#Exercise3)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Alıştırma 1: salt okunur bir Web API'si oluşturma

Bu alıştırmada kişi yöneticisi için salt okunur GET yöntemleri uygular.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Görev 1 - API projesi oluşturma

Bu görevde, bir Web API'si web uygulaması oluşturmak için yeni bir ASP.NET web projesi şablonları kullanır.

1. Çalıştırma **Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **Web için VS Express** tuşuna **Enter**.
2. Gelen **dosya** menüsünde **yeni proje**. Seçin **Visual C# | Web** proje türü proje türü ağaç görünümünden ve ardından **ASP.NET MVC 4 Web uygulaması** proje türü. Projenin ayarlamak **adı** için *ContactManager* ve **çözüm adı** için *başlamak*, ardından **Tamam**.

    ![Yeni bir ASP.NET MVC 4.0 Web uygulaması projesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image1.png "yeni bir ASP.NET MVC 4.0 Web uygulaması projesi oluşturma")

    *Yeni bir ASP.NET MVC 4.0 Web uygulaması projesi oluşturma*
3. ASP.NET MVC 4 Proje Türü Seç iletişim kutusu içinde **Web API** proje türü. **Tamam**'ı tıklatın.

    ![Web API projesi türünü belirterek](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API projesi türünü belirtme")

    *Web API projesi türünü belirtme*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Görev 2 - Kişi Yöneticisi APİ'si denetleyicilerinin oluşturma

Bu görevde, API yöntemleri ikamet denetleyicisi sınıflar oluşturur.

1. Adlı dosyayı silin **ValuesController.cs** içinde **denetleyicileri** proje klasöründen.
2. Sağ **denetleyicileri** seçin ve proje klasöründe **Ekle | Denetleyici** bağlam menüsünden.

    ![Yeni bir denetleyici projeye eklenirken](build-restful-apis-with-aspnet-web-api/_static/image3.png "projeye yeni bir denetleyici ekleme")

    *Projeye yeni bir denetleyici ekleme*
3. İçinde **denetleyici Ekle** görüntülenirse, seçin iletişim **boş API denetleyicisi** şablon menüsünde. Denetleyici sınıfı adı **ContactController**. ' A tıklayarak **Ekle.**

    ![Yeni bir Web API denetleyicisi oluşturmak için denetleyici Ekle iletişim kutusunu kullanarak](build-restful-apis-with-aspnet-web-api/_static/image4.png "yeni bir Web API denetleyicisi oluşturmak için denetleyici Ekle iletişim kutusunu kullanma")

    *Yeni bir Web API denetleyicisi oluşturmak için denetleyici Ekle iletişim kutusunu kullanma*
4. Aşağıdaki kodu ekleyin **ContactController**.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - API yöntem elde*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Tuşuna **F5** uygulamada hata ayıklamak için. Bir Web API projesi için varsayılan giriş sayfası görüntülenmelidir.

    ![Bir ASP.NET Web API uygulamasının varsayılan giriş sayfasını](build-restful-apis-with-aspnet-web-api/_static/image5.png "bir ASP.NET Web API uygulamasının varsayılan giriş sayfası")

    *Bir ASP.NET Web API uygulamasının varsayılan giriş sayfası*
6. Internet Explorer penceresinde tuşuna **F12** açmak için anahtar **Geliştirici Araçları** penceresi. Tıklayın **ağ** sekmesine ve ardından **Yakalamayı Başlat** penceresine ağ trafiğini yakalamaktan başlamak için düğme.

    ![Başlatma ve ağ sekmesini açıp Ağ Yakalama](build-restful-apis-with-aspnet-web-api/_static/image6.png "başlatan ve ağ sekmesini açıp ağ yakalama")

    *Ağ sekmesini açıp ve ağ yakalama başlatılıyor*
7. İle tarayıcının adres çubuğuna URL'yi ekleme **/API/contact** ve enter tuşuna basın. İletim ayrıntıları, Ağ Yakalama penceresinde görünür. Yanıtın MIME türü olduğunu unutmayın **application/json**. Bu, varsayılan çıkış biçimini JSON nasıl olduğunu gösterir.

    ![Ağ Görünümü'nde Web API isteği çıkışını görüntüleme](build-restful-apis-with-aspnet-web-api/_static/image7.png "ağ görünümü'nde Web API isteği çıkışını görüntüleme")

    *Ağ Görünümü'nde Web API isteği çıkışını görüntüleme*

    > [!NOTE]
    > Internet Explorer 10'ın varsayılan davranışı, bu noktada kullanıcı kaydetmek veya Web API çağrısından kaynaklanan stream açmak isteyip istemediğini sormak olacaktır. Çıkış JSON Web API URL'si çağrı sonucunu içeren bir metin dosyasının olacaktır. Yanıtın içerik geliştiriciler araç penceresi aracılığıyla izlemek için iletişim kutusunu iptal etme.
8. Tıklayın **ayrıntılı görünümüne gidin** bu API çağrısının yanıtı hakkında daha fazla ayrıntı görmek için düğme.

    ![Geçiş için Ayrıntılı Görünüm](build-restful-apis-with-aspnet-web-api/_static/image8.png "ayrıntıları görünümüne geçin")

    *Ayrıntılı görünümüne geç*
9. Tıklayın **yanıt gövdesi** gerçek JSON yanıt metni görüntülemek için sekmesinde.

    ![Ağ İzleyicisi'nde metin çıktısını JSON görüntüleme](build-restful-apis-with-aspnet-web-api/_static/image9.png "çıkış metnini Ağ İzleyicisi'nde JSON görüntüleme")

    *Ağ İzleyicisi'nde JSON çıkış metin görüntüleme*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Görev 3 - kişi modelleri oluşturma ve ilgili kişi denetleyicisi büyütmek

Bu görevde, API yöntemleri ikamet denetleyicisi sınıflar oluşturur.

1. Sağ **modelleri** klasörü ve select **Ekle | Sınıfı...**  bağlam menüsünden.

    ![Web uygulaması için yeni model ekleme](build-restful-apis-with-aspnet-web-api/_static/image10.png "yeni model web uygulamasına ekleme")

    *Yeni model web uygulamasına ekleme*
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda, yeni dosya adı **Contact.cs** tıklatıp **Ekle.**

    ![Yeni bir kişi sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image11.png "yeni kişi sınıf dosyası oluşturma")

    *Yeni bir kişi sınıf dosyası oluşturma*
3. Aşağıdaki vurgulanmış kodu ekleyin **kişi** sınıfı.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi sınıfı*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. İçinde **ContactController** sözcüğünü seçin, sınıf **dize** yöntemi tanımındaki **alma** yöntemi ve sözcük türü *kişi*. Word yazılmış sonra bir göstergesi sözcüğün başında görünür **kişi**. Ya da basılı **Ctrl** anahtar ve nokta (.) tuşuna basın veya otomatik olarak doldurmak için Kod Düzenleyicisi'nde Yardım iletişim kutusunu açmak için farenizi kullanarak simgesini **kullanarak** modellerine yönelik yönerge ad alanı.

    ![Ad alanı bildirimi için IntelliSense Yardım'ı kullanarak](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Ad alanı bildirimi için IntelliSense Yardım'ı kullanarak*
5. Kodu değiştirmek **alma** olan kişi modeli örnekleri bir dizi döndürecek şekilde yöntemi.

    (Kod parçacığını - *Web API kişi listesi döndüren Laboratuvar - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Tuşuna **F5** tarayıcı web uygulamasında hata ayıklamak için. API yanıt çıkışına yapılan değişiklikleri görmek için aşağıdaki adımları gerçekleştirin.

   1. Tarayıcı açılır sonra basın **F12** geliştirici araçları henüz açık değilse.
   2. Tıklayın **ağ** sekmesi.
   3. Tuşuna **Yakalamayı Başlat** düğmesi.
   4. URL soneki eklemek **/API/contact** tuşuna basın ve adres çubuğuna URL'ye **Enter** anahtarı.
   5. Tuşuna **ayrıntılı görünümüne gidin** düğmesi.
   6. Seçin **yanıt gövdesi** sekmesi. İlgili kişi örnekleri bir dizi serileştirilmiş biçiminde temsil eden bir JSON dizesi görmeniz gerekir.

      ![JSON serileştirilmiş bir karmaşık Web API yöntem çağrısının çıkış](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON seri hale getirilmiş bir karmaşık Web API yöntem çağrısının çıkış")

      *Karmaşık bir Web API yöntem çağrısının çıkış JSON seri hale getirilmiş*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Görev 4 - bir hizmet katmanına ayıklanan işlevi

Bu görevi nasıl ayıklanacağını denetleyicisi katmandan, böylece çalışmalarında gerçekten kitaplıklarımızı hizmetleri sağlayan hizmet işlevleri ayırmak, geliştiriciler için kolaylaştıran bir hizmet katmanı işlevsellik gösterilecektir.

1. Çözüm kök dizininde yeni bir klasör oluşturun ve adlandırın **Hizmetleri**. Bunu yapmak için sağ **ContactManager** proje, select **Ekle** | **yeni klasör**, adlandırın *Hizmetleri*.

    ![Oluşturma Hizmetleri klasörü](build-restful-apis-with-aspnet-web-api/_static/image14.png "oluşturma hizmetleri klasörü")

    *Hizmetleri klasörü oluşturma*
2. Sağ **Hizmetleri** klasörü ve select **Ekle | Sınıfı...**  bağlam menüsünden.

    ![Yeni sınıf Hizmetleri klasörü ekleme](build-restful-apis-with-aspnet-web-api/_static/image15.png "Hizmetleri klasöre yeni sınıf ekleme")

    *Yeni sınıf Hizmetleri klasörü ekleme*
3. Zaman **Yeni Öğe Ekle** iletişim kutusu açılır, yeni sınıfın adı **ContactRepository** tıklatıp **Ekle**.

    ![Kişi havuz hizmet katmanı için kod içeren bir sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image16.png "kişi havuz hizmet katmanı için kod içeren bir sınıf dosyası oluşturma")

    *Kişi havuz hizmet katmanı için kod içeren bir sınıf dosyası oluşturma*
4. Kullanarak bir ekleme yönergesini **ContactRepository.cs** modelleri ad alanı içerecek şekilde dosya.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Aşağıdaki vurgulanmış kodu ekleyin **ContactRepository.cs** GetAllContacts yöntemi uygulamak için dosya.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi depo*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Açık **ContactController.cs** zaten açık değilse, dosya.
7. Aşağıdaki dosya ad alanı bildirimi bölümünü using deyimi.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Aşağıdaki vurgulanmış kodu ekleyin **ContactController.cs** sınıfı üyeleri yapabilir sınıfın rest hizmeti uygulamasını kullanmak deponun örneği temsil eden bir özel alan eklemek için.

    (Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi denetleyicisi*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Değişiklik **alma** BT'nin yapsak yöntemi kişi depo hizmetini kullanın.

    (Kod parçacığını - *Web API depo kişilerin listesini döndüren Laboratuvar - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Bir kesme noktasına yerleştirip **ContactController**'s **alma** yöntem tanımı.

   ![Kesme noktası için kişi denetleyicisi ekleme](build-restful-apis-with-aspnet-web-api/_static/image17.png "kişi denetleyiciye kesme noktası ekleme")

   *Kesme noktası için kişi denetleyicisi ekleme*
11. Tuşuna **F5** uygulamayı çalıştırın.
12. Tarayıcı açıldığında basın **F12** Geliştirici Araçları'nı açın.
13. Tıklayın **ağ** sekmesi.
14. Tıklayın **Yakalamayı Başlat** düğmesi.
15. Sonekine sahip adres çubuğundaki URL Ekle **/API/contact** basın **Enter** API denetleyicisi yüklemek için.
16. Visual Studio 2012 sonu kez **alma** yöntemi yürütülmesine başlar.

   ![Get yöntemi içinde bozucu](build-restful-apis-with-aspnet-web-api/_static/image18.png "bozucu içinde Get yöntemi")

   *Get yöntemi içinde kesme*
17. Tuşuna **F5** devam etmek için.
18. Odak yoksa Internet Explorer için geri dönün. Ağ Yakalama aralığınız unutmayın.

    ![Internet Explorer Web API çağrısının sonucunu gösteren görünümünde ağ](build-restful-apis-with-aspnet-web-api/_static/image19.png "ağ Internet Explorer'da Web API çağrısının sonucunu gösteren görünüm")

    *Internet Explorer'da Web API çağrısının sonucunu gösteren ağ görünümü*
19. Tıklayın **ayrıntılı görünümüne gidin** düğmesi.
20. Tıklayın **yanıt gövdesi** sekmesi. JSON çıkışında API çağrısı ve hizmet katmanı tarafından alınan iki kişi nasıl temsil ettiğini unutmayın.

    ![Web API'si JSON çıktısını Geliştirici Araçları penceresinde görüntüleme](build-restful-apis-with-aspnet-web-api/_static/image20.png "JSON çıkışını Web API'si Geliştirici Araçları penceresinde görüntüleme")

    *Web API'si JSON çıktısını Geliştirici Araçları penceresinde görüntüleme*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Alıştırma 2: okuma/yazma Web API'si oluşturma

Bu alıştırmada, POST uygular ve veri düzenleme özellikleri ile etkinleştirmek kişi yöneticisi için PUT yöntemleri.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Görev 1 - Web API projesi açma

Bu görevde, böylece kullanıcı girişi kabul edebilir alıştırma 1'de oluşturduğunuz Web API projesi geliştirmek hazırlık yapacaksınız.

1. Çalıştırma **Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **Web için VS Express** tuşuna **Enter**.
2. Açık **başlamak** çözüm bulunan **kaynak/Ex02-ReadWriteWebAPI/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **Services/ContactRepository.cs** dosya.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Görev 2 - veri kalıcılığı özellik kişi depo uygulamaya ekleme

Bu görevde, böylece kalıcı hale getirmek ve kullanıcı girişi ve yeni kişi örnekleri kabul alıştırma 1'de oluşturduğunuz Web API projesi ContactRepository sınıfının genişletecektir.

1. Eklemek için aşağıdaki sabiti **ContactRepository** web sunucusu önbellek öğesi anahtar adı Bu alıştırmada daha sonra adını temsil eden sınıf.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Bir oluşturucuyu ekleyin **ContactRepository** aşağıdaki kodu içeren.

    (Kod parçacığını - *Web API Laboratuvar - Ex02 - kişi depo Oluşturucusu*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Kodu değiştirmek **GetAllContacts** aşağıda gösterildiği gibi yöntemi.

    (Kod parçacığını - *Web API Laboratuvar - Ex02 - tüm kişileri Al*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Bu örnek, gösterim amaçlıdır ve değerleri aynı anda birden çok istemciler için kullanılabilir yerine böylece oturumu depolama mekanizmasını veya bir istek depolama ömrü web sunucusunun önbellek depolama ortamı olarak kullanır. Bir Entity Framework, XML depolama veya tüm diğer birçok web sunucusu önbelleği yerine kullanabilirsiniz.
4. Adlı yeni bir yöntem **SaveContact** için **ContactRepository** kişi kaydetme işini yapması için sınıf. **SaveContact** yöntemi, tek bir sürecektir **kişi** belirten başarı veya başarısızlık parametre ve dönüş bir Boole değeri.

    (Kod parçacığını - *API SaveContact yöntemi uygulama Laboratuvar - Ex02 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Alıştırma 3: bir HTML istemci Web API kullanma

Bu alıştırmada, Web API'sini çağırmak için bir HTML istemci oluşturacaksınız. Bu istemci, JavaScript kullanarak Web API'si ile veri değişimi kolaylaştırmak ve sonuçları HTML biçimlendirmeyi kullanarak bir web tarayıcısında görüntülenir.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Görev 1 - kişiler görüntülemek için bir GUI sağlamak için dizini görünümünü değiştirme

Bu görevde, bir HTML tarayıcı içinde var olan bir kişi listesini görüntülemenin desteklemek için web uygulamasının varsayılan dizini görünümünü değiştirir.

1. Açık **Visual Studio 2012 Express Web** zaten açık değilse.
2. Açık **başlamak** çözüm bulunan **kaynak/Ex03-ConsumingWebAPI/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **Index.cshtml** konumundaki dosya **görünümler/giriş** klasör.
4. Div öğesinin içine HTML kodu kimliğiyle değiştirin **gövdesi** böylece şu kod gibi görünüyor.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Web API'sine HTTP isteği gerçekleştirmek için dosyanın sonuna aşağıdaki Javascript kodunu ekleyin.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Açık **ContactController.cs** zaten açık değilse, dosya.
7. Bir kesme noktası yerleştirmek **alma** yöntemi **ContactController** sınıfı.

    ![API denetleyicisi üzerinde Get yöntemi bir kesme noktası yerleştirerek](build-restful-apis-with-aspnet-web-api/_static/image21.png "bir kesme noktası yerleştirerek API denetleyicisi üzerinde Get yöntemi")

    *Bir kesme noktası yerleştirerek API denetleyicisi üzerinde Get yöntemi*
8. Tuşuna **F5** projeyi çalıştırın. Tarayıcı HTML belgesi yükler.

    > [!NOTE]
    > Uygulama kök URL'sini tarama emin olun.
9. Sayfa yükler ve yürütür JavaScript sonra kesme noktasına isabet ve kod yürütme denetleyicide duraklatılır.

    ![Web için VS Express kullanarak Web API çağrısı ile hata ayıklama](build-restful-apis-with-aspnet-web-api/_static/image22.png "Web için VS Express kullanarak Web API çağrısı ile hata ayıklama")

    *Web için Visual Studio Express 2012 kullanarak Web API çağrısı ile hata ayıklama*
10. Kesme noktası kaldırıp tuşuna **F5** veya hata ayıklama araç çubuğunun **devam** tarayıcıda görüntüle yüklemeye devam etmek için düğmeyi. Web API çağrısı tamamlandığında tarayıcıda listesi öğeleri olarak görüntülenen arama Web API'den döndürülen kişiler görmeniz gerekir.

    ![Liste öğeleri tarayıcıda görüntülenen API çağrısının sonucunu](build-restful-apis-with-aspnet-web-api/_static/image23.png "listesi öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçları")

    *Liste öğeleri tarayıcıda görüntülenen API çağrısının sonuçları*
11. Hata ayıklamayı durdurun.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Görev 2 - kişiler oluşturmak için bir GUI sağlamak için dizini görünümünü değiştirme

Bu görevde, MVC uygulama dizini görünümünü değiştirmek devam eder. Bir forma kullanıcı girişi yakalamak ve bunları yeni kişi oluşturmak için Web API'sine gönderir HTML sayfasına eklenen ve GUI tarihi toplamak için yeni bir Web API denetleyicisi yöntem oluşturulur.

1. Açık **ContactController.cs** dosya.
2. Denetleyici sınıfına adlı yeni bir yöntem ekleyin **Post** aşağıdaki kodda gösterildiği gibi.

    (Kod parçacığını - *Web API Laboratuvar - Ex03 - Post yöntemini*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Açık **Index.cshtml** zaten açık değilse Visual Studio'da dosya.
4. Aşağıdaki HTML kodu yalnızca önceki görevde eklediğiniz sırasız liste dosyaya ekleyin.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Belge alt kısmındaki betik öğesi içinde bir HTTP POST çağrısı kullanarak Web API'si için verileri post gerçekleştireceği düğme tıklama olayları işlemek için aşağıdaki vurgulanmış kodu ekleyin.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. İçinde **ContactController.cs**, bir kesme noktası yerleştirmek **Post** yöntemi.
7. Tuşuna **F5** uygulamayı tarayıcıda çalıştırın.
8. Sayfanın tarayıcıda yüklendikten sonra yeni bir kişi adı ve kimliği ve tıklatın yazın **Kaydet** düğmesi.

    ![İstemci HTML belge yüklenirken tarayıcıda](build-restful-apis-with-aspnet-web-api/_static/image24.png "istemci HTML belgesi tarayıcıda yüklendi")

    *Tarayıcıda yüklenen istemci HTML belgesi*
9. Hata ayıklayıcısı penceresinin zaman sonları **Post** yöntemi, özelliklerini atın **başvurun** parametresi. Değerleri ve forma girdiniz verilerin eşleşmesi gerekir.

    ![Web API için istemci tarafından gönderilen kişi nesnesi](build-restful-apis-with-aspnet-web-api/_static/image25.png "istemcisinden Web API'ye gönderilen kişi nesnesi")

    *Web API için istemci tarafından gönderilen kişi nesnesi*
10. Hata ayıklayıcı kadar yöntemi adımlayın **yanıt** değişken oluşturuldu. İncelemesinin bağlı **Yereller** hata ayıklayıcı penceresinde tüm özellikler ayarlandıktan göreceksiniz.

   ![Hata ayıklayıcı oluşturulduktan yanıt](build-restful-apis-with-aspnet-web-api/_static/image26.png "hata ayıklayıcısı oluşturulduktan yanıt")

   *Hata ayıklayıcı oluşturulduktan yanıt*
11. Basarsanız **F5** veya **devam** hata ayıklayıcıda istek tamamlanır. Tarayıcıya geçtikten sonra yeni kişi tarafından depolanan kişiler listesine eklendi **ContactRepository** uygulaması.

   ![Tarayıcı oluşturma başarılı yeni kişi örneğinin yansıtır](build-restful-apis-with-aspnet-web-api/_static/image27.png "tarayıcı yansıtan yeni kişi örneği başarıyla oluşturuldu")

   *Tarayıcı yansıtan yeni kişi örneği başarıyla oluşturuldu*

> [!NOTE]
> Ayrıca, bu uygulama aşağıdaki Azure dağıtabilirsiniz [ek C: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu Laboratuvar, yeni ASP.NET Web API çerçevesi ve uygulaması çerçevesini kullanarak RESTful Web API'lerini kullanıma sundu. Buradan, herhangi bir sayıda mekanizmalarını kullanarak veri kalıcılığı kolaylaştıran yeni bir havuz oluşturabilir ve yerine bu Laboratuvar içinde bir örnek olarak basit bir yedekleme, hizmet bağlayabilirsiniz. Web API'si, HTTP ve JSON veya XML destekleyen herhangi bir dilde yazılmış HTML olmayan istemcilerden gelen iletişimi etkinleştirme gibi ek özellikleri destekler. Tipik bir web uygulaması dışında bir Web API'sini barındıran olanağı da mümkündür, aynı zamanda kendi serileştirme biçimleri oluşturma yeteneği.

ASP.NET Web sitesinde ASP.NET Web API çerçevesi ayrılmış bir alan yok [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Bu site, en son haberler, örnekler ve Web API'si için ilgili Haberler şekilde kontrol edin, sık sağlamaya devam edecek içine neredeyse tüm cihaz veya geliştirme framework kullanılabilir özel Web API'leri oluşturmanın son teknoloji ürünü derin delve istiyorsanız.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Ek A: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](build-restful-apis-with-aspnet-web-api/_static/image28.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

    ![Kod parçacığı adını yazmaya başlayın](build-restful-apis-with-aspnet-web-api/_static/image29.png "kod parçacığı adını yazmaya başlayın")

    *Kod parçacığı adını yazmaya başlayın*

    ![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](build-restful-apis-with-aspnet-web-api/_static/image30.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

    *Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

    ![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](build-restful-apis-with-aspnet-web-api/_static/image31.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

    *Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için

1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.
2. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
3. Tıklayarak ilgili kod parçacığı listeden seçin.

    ![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](build-restful-apis-with-aspnet-web-api/_static/image32.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

    *Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

    ![Tıklayarak ilgili kod parçacığını listesinden çekme](build-restful-apis-with-aspnet-web-api/_static/image33.png "tıklayarak ilgili kod parçacığı listeden seçin")

    *Tıklayarak ilgili kod parçacığı listeden seçin*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Ek B: yükleme Visual Studio 2012 Web için Express

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Web kutucuğu için VS Express*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek C: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

Bu ekte, Azure portalında yeni bir web sitesi oluşturma ve Laboratuvar izleyerek Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Görev 1 - Azure portalında yeni bir Web sitesi oluşturma

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure Portal'da oturum açın](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure Portal'da oturum açın")

    *Portal'da oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image40.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Azure, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için bir ana bilgisayardır. Hızlı oluşturma seçeneği azure'a portalın dışında bir tamamlanmış web uygulaması dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image41.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](build-restful-apis-with-aspnet-web-api/_static/image42.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](build-restful-apis-with-aspnet-web-api/_static/image43.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](build-restful-apis-with-aspnet-web-api/_static/image44.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* bir her etkin yayımlama yöntemi için bir Azure web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Azure Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi indiriliyor](build-restful-apis-with-aspnet-web-api/_static/image45.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bir Azure web uygulamasına Visual Studio'dan yayımlamak için bu dosyayı kullanmak nasıl görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](build-restful-apis-with-aspnet-web-api/_static/image46.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. Aboneliğinizde Azure Yönetim Portalı'nda SQL veritabanı sunucuları görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucuPanosu**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) düğmesi.

    ![İstemci IP adresi ekleme](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Değişiklikleri onaylayın*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](build-restful-apis-with-aspnet-web-api/_static/image51.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](build-restful-apis-with-aspnet-web-api/_static/image52.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](build-restful-apis-with-aspnet-web-api/_static/image53.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image55.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](build-restful-apis-with-aspnet-web-api/_static/image56.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](build-restful-apis-with-aspnet-web-api/_static/image58.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

    ![Uygulama Windows Azure'da yayımlanan](build-restful-apis-with-aspnet-web-api/_static/image59.png "uygulama yayımlanan Windows Azure'a")

    *Azure'da yayımlanan uygulama*
