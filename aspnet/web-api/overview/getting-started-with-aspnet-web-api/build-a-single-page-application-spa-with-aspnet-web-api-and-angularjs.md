---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Uygulamalı Laboratuvar: ASP.NET Web API ve Angular.js ile tek sayfalı uygulama (SPA) oluşturun. | Microsoft Docs'
author: rick-anderson
description: Geleneksel web uygulamaları, bir sayfa isteyerek istemci (tarayıcı) sunucusu ile iletişim başlatır. Sunucu, ardından isteğini işliyor...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912260"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Uygulamalı Laboratuvar: ASP.NET Web API ve Angular.js ile tek sayfalı uygulama (SPA) oluşturun
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](http://aka.ms/webcamps-training-kit)

> Geleneksel web uygulamaları, bir sayfa isteyerek istemci (tarayıcı) sunucusu ile iletişim başlatır. Sunucu isteği işler ve sayfanın HTML istemciye gönderir. Sonraki sayfanın – örneğin kullanıcı bağlantısı gezinir veya verileri içeren formu gönderdiğinde – etkileşim içinde yeni bir istek sunucuya gönderilir ve akışı yeniden başlatır: sunucu isteği işler ve yeni bir sayfa tarayıcıya yeni eylem isteğine yanıt olarak gönderir Ed istemci tarafından.
> 
> Tek sayfa uygulamaları (Spa'lar) sayfanın tamamını ilk istek tarayıcıda yüklendi ancak etkileşiminde Ajax istekleri aracılığıyla gerçekleşir. Bu tarayıcı yalnızca değişen sayfa bölümü güncelleştirilecek olduğu anlamına gelir; Tüm sayfayı yeniden yüklemek için gerek yoktur. SPA yaklaşım daha akıcı bir deneyim, kullanıcı eylemlerini yanıt vermek için uygulama tarafından kullanılan süreyi azaltır.
> 
> Bir SPA mimarisi, geleneksel web uygulamaları mevcut olmayan bazı zorluklar içerir. Ancak, ASP.NET Web API gibi teknolojileri Gelişmekte olan, AngularJS JavaScript çerçevelerinin ister ve CSS3 tarafından sağlanan yeni stil özellikleri gerçekten Spa'lar tasarlayıp kolaylaştırır.
> 
> Bu uygulamalı laboratuvarda Geek test, SPA kavramını temel Meraklısına Notlar Web sitesi uygulamak için bu teknolojilerden sürer. Hizmet katmanı almak test sorularını ve yanıtlarını depolamak için gerekli uç noktaları kullanıma sunmak için ASP.NET Web API ile ilk uygular. Ardından, AngularJS ve CSS3 dönüştürme efektleri kullanarak zengin ve esnek bir kullanıcı Arabirimi oluşturacaksınız.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- JSON veri göndermek ve almak için bir ASP.NET Web API'si hizmetini oluşturma
- AngularJS kullanarak duyarlı bir kullanıcı Arabirimi oluşturma
- CSS3 dönüştürmeleri kullanıcı Arabirimi deneyimini geliştirin

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya üzeri

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

1. [Web API'si oluşturma](#Exercise1)
2. [SPA arabirimi oluşturma](#Exercise2)

Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**

> [!NOTE]
> Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir. Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Alıştırma 1: bir Web API'si oluşturma

Anahtar bölümlerini bir SPA Hizmet katmanını biridir. Kullanıcı Arabirimi ve veri döndürmeyi yanıt olarak bu çağrı tarafından gönderilen Ajax çağrıları işlemekten sorumludur. Alınan verileri ayrıştırılması ve istemci tarafından tüketilen için makine tarafından okunabilir bir biçimde sunulmalıdır.

Web API çerçevesi ASP.NET yığınının bir parçasıdır ve genel veri gönderme ve JSON veya XML biçimli bir RESTful API'si aracılığıyla alma HTTP hizmetlerini uygulamak amacıyla kolaylaştırmak için tasarlanmıştır. Bu alıştırmada, Geek test uygulamasını barındırmak ve ardından göstermek ve ASP.NET Web API kullanarak test verileri kalıcı hale getirmek için arka uç hizmeti uygulamak için bu Web sitesi oluşturacaksınız.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Görev 1 – Geek sınavı için ilk proje oluşturma

Bu görevde, ASP.NET Web API tabanlı yeni bir ASP.NET MVC projesi desteğiyle oluşturmaya başlayacağını **tek ASP.NET** proje Visual Studio ile birlikte gelen türü. **Bir ASP.NET** tüm ASP.NET teknolojileri birleştirir ve karıştırın ve bunları istediğiniz gibi eşleşen seçeneği sunar. Ardından, Entity Framework'ün model sınıfları ve test sorularını eklemek için veritabanı initializator ekleyeceksiniz.

1. Açık **Visual Studio Express 2013 Web** seçip **dosya | Yeni proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **ASP.NET Web uygulaması** altında **Visual C# | Web** sekmesi. Emin olun **.NET Framework 4.5** olduğu belirlenirse, ad *GeekQuiz*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni bir ASP.NET Web uygulaması projesi oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "yeni bir ASP.NET Web uygulaması projesi oluşturma")

    *Yeni bir ASP.NET Web uygulaması projesi oluşturma*
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC** şablonu seçip alt **Web API** seçeneği. Ayrıca, emin **kimlik doğrulaması** seçeneği **bireysel kullanıcı hesapları**. Devam etmek için **Tamam** 'a tıklayın.

    ![Web API bileşenler dahil olmak üzere MVC şablonuyla yeni proje oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Web API bileşenler dahil olmak üzere MVC şablonuyla yeni proje oluşturma*
4. İçinde **Çözüm Gezgini**, sağ **modelleri** klasörü **GeekQuiz** seçin ve proje **Ekle | Mevcut öğe...** .

    ![Var olan bir öğe eklemeye](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "var olan öğe ekleme")

    *Var olan öğe ekleme*
5. İçinde **varolan öğeyi Ekle** iletişim kutusunda, gitmek **varlıklar/kaynak/modelleri** klasörünü ve tüm dosyaları seçin. **Ekle**'yi tıklatın.

    ![Model varlıklar ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "modeli varlıklar ekleme")

    *Model varlıklar ekleme*

    > [!NOTE]
    > Bu dosyaları ekleyerek, veri modeli, Entity Framework'ün veritabanı bağlamı ve Geek test uygulaması için veritabanı Başlatıcı ekliyoruz.
    > 
    > **Entity Framework (EF)** olan programlama ilişkisel depolama şeması kullanarak doğrudan programlama yerine kavramsal bir uygulama modeli tarafından veri erişimi uygulamaları oluşturmanızı sağlayan bir nesne ilişkisel eşleyicidir (ORM). Entity Framework hakkında daha fazla bilgi [burada](../../../entity-framework.md).
    > 
    > Yeni eklediğiniz sınıfları açıklaması verilmiştir:
    > 
    > - **TriviaOption:** test soruyla ilişkili tek bir seçenek temsil eder
    > - **TriviaQuestion:** test soru temsil eder ve ilişkili çalışma seçeneklerde ortaya çıkaran **seçenekleri** özelliği
    > - **TriviaAnswer:** test sorusuna yanıt kullanıcı tarafından seçilen seçeneğini temsil eder
    > - **TriviaContext:** Geek test uygulama Entity Framework'ün veritabanı bağlamını temsil eder. Bu sınıfın türetildiği **DContext** ve ortaya çıkaran **olan DB** yukarıda açıklanan varlık koleksiyonları temsil eden özellikler.
    > - **TriviaDatabaseInitializer:** uygulaması için Entity Framework Başlatıcı **TriviaContext** öğesinden devralan sınıf **Createdatabaseıfnotexists**. Bu sınıfın varsayılan davranış, yoksa yalnızca, veritabanı oluşturma, varlıklar ekleme belirtilen **çekirdek** yöntemi.
6. Açık **Global.asax.cs** dosya ve aşağıdaki deyimi kullanarak.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Başında aşağıdaki kodu ekleyin **uygulama\_Başlat** ayarlanacak yöntemi **TriviaDatabaseInitializer** veritabanı başlatıcı olarak.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Değiştirme **giriş** kimliği doğrulanmış kullanıcılara erişimi kısıtlamak için denetleyici. Bunu yapmak için açık **HomeController.cs** içinde dosya **denetleyicileri** klasör ve eklemek **Authorize** özniteliğini **HomeController**sınıf tanımını.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize** filtre kullanıcının kimliği doğrulanır, denetler. Kullanıcının kimliği doğrulanmazsa, HTTP durum kodunu 401 (yetkisiz) eylemi çağırma olmadan döndürür. Genel olarak, denetleyici düzeyinde veya bireysel eylemleri düzeyini filtre uygulayabilirsiniz.
9. Artık web sayfaları ve markalama düzenini özelleştireceksiniz. Bunu yapmak için açık  **\_Layout.cshtml** içinde dosya **görünümleri | Paylaşılan** klasörü ve içeriğini güncelleştirme **&lt;başlık&gt;** değiştirerek öğesi *My ASP.NET Application* ile *Geek test* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Aynı dosyada, gezinti çubuğunda kaldırarak güncelleştirme *hakkında* ve *kişi* bağlantılar ve yeniden adlandırma *giriş* bağlantı *Play*. Ayrıca, yeniden adlandırma *uygulama adı* bağlantı *Geek test*. HTML gezinti çubuğu için şu kod gibi görünmelidir.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Düzen sayfasının alt değiştirerek güncelleştirin *My ASP.NET Application* ile *Geek test*. Bunu yapmak için içeriğinin yerine geçecek **&lt;altbilgi&gt;** aşağıdaki vurgulanmış kodu olan öğe.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Görev 2 – TriviaController Web API'si oluşturma

Önceki görevde başlangıç yapısını Geek test web uygulaması oluşturdunuz. Şimdi, test veri modeli ile etkileşime geçer ve aşağıdaki eylemleri gösteren basit bir Web API'si hizmeti oluşturacaksınız:

- **GET/api/trivia**: Sonraki soruya kimliği doğrulanmış kullanıcı tarafından yanıtlanması için test listesini alır.
- **POST/api/trivia**: kimliği doğrulanmış kullanıcı tarafından belirtilen test yanıt depolar.

Web API denetleyici sınıfı için taban çizgisi oluşturmak için Visual Studio tarafından sağlanan ASP.NET yapı İskelesi araçları kullanır.

1. Açık **WebApiConfig.cs** içinde dosya **uygulama\_Başlat** klasör. Bu dosya yolları Web API denetleyici eylemleri için nasıl eşleştirildiğini gibi Web API hizmetinin yapılandırmasını tanımlar.
2. Aşağıdaki using deyimini dosyanın başında.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Aşağıdaki vurgulanmış kodu ekleyin **kaydetme** Genel Web API eylem yöntemleri tarafından alınan JSON verileri için biçimlendiriciyi yapılandırmak için yöntem.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** özellik adlarını otomatik olarak dönüştürür *camel* JavaScript özellik adları için genel kuralı durumda.
4. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasörü **GeekQuiz** seçin ve proje **Ekle | Yeni İskeleli öğe...** .

    ![Yeni iskele kurulmuş öğe oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "yeni iskele kurulmuş öğe oluşturma")

    *Yeni iskele kurulmuş öğe oluşturma*
5. İçinde **İskele Ekle** iletişim kutusunda, emin **ortak** düğümü sol bölmede seçili. Ardından, **Web API 2 denetleyici - boş** tıklatın ve Orta bölmede şablonunda **Ekle**.

    ![Web API 2 denetleyicisi boş şablonu seçip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Web API 2 denetleyicisi boş şablonu seçme")

    *Web API 2 denetleyicisi boş şablonu seçme*

    > [!NOTE]
    > **ASP.NET iskeleti oluşturma** bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Visual Studio 2013, MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir. Standart veri işlemlerini geliştirmek için süre miktarını azaltmak için veri modelleri ile etkileşime giren kodu gerekli hızlı bir şekilde eklemek istediğiniz zaman, projenize yapı iskelesi kullanmanız gerekir.
    > 
    > İskele kurma işlemi ayrıca, tüm gerekli bağımlılıkları projede yüklendiğini sağlar. Boş ASP.NET projesi ile başlayın ve ardından bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli Web API NuGet paketleri ve başvuruları projenize otomatik olarak eklenir.
6. İçinde **denetleyici Ekle** iletişim kutusuna *TriviaController* içinde **Denetleyici adı** metin kutusu ve tıklatın **Ekle**.

    ![Meraklısına Notlar denetleyicisi ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Trivia denetleyici ekleme")

    *Meraklısına Notlar denetleyici ekleme*
7. **TriviaController.cs** dosya eklenen ardından **denetleyicileri** klasörü **GeekQuiz** içeren boş bir proje, **TriviaController** sınıfı. Aşağıdaki using deyimlerini dosyanın başında.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Başında aşağıdaki kodu ekleyin **TriviaController** tanımlayın, başlatmak ve silmek için sınıf **TriviaContext** denetleyici örneği.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** yöntemi **TriviaController** çağırır **Dispose** yöntemi **TriviaContext** sağlar örneği, tüm bağlam nesnesi tarafından kullanılan kaynakları serbest olduğunda **TriviaContext** örneği atıldı veya atık olarak toplanmış. Bu, Entity Framework tarafından açılmış olan tüm veritabanı bağlantıları kapatarak içerir.
9. Aşağıdaki yardımcı yöntemini sonuna ekleyin **TriviaController** sınıfı. Bu yöntem aşağıdaki test soru belirtilen kullanıcı tarafından yanıtlanması gereken veritabanından alır.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Aşağıdaki **alma** eylem yöntemine **TriviaController** sınıfı. Bu eylem yöntemini çağırır **NextQuestionAsync** kimliği doğrulanmış kullanıcı için bir sonraki soruya almak için önceki adımda tanımlanan yardımcı yöntemi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Aşağıdaki yardımcı yöntemini sonuna ekleyin **TriviaController** sınıfı. Bu yöntem, belirtilen yanıt veritabanında depolar ve yanıt doğru olup olmadığını belirten bir Boole değeri döndürür.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Aşağıdaki **Post** eylem yöntemine **TriviaController** sınıfı. Bu eylem yöntemini çağırır ve kimliği doğrulanmış kullanıcı yanıtı ilişkilendirir **StoreAsync** yardımcı yöntemi. Ardından, yardımcı yöntemi tarafından döndürülen bir Boole değeri ile bir yanıt gönderir.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Web API denetleyicisi ekleyerek, kimliği doğrulanmış kullanıcılara erişimi kısıtlamak için değiştirme **Authorize** özniteliğini **TriviaController** sınıf tanımını.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3: çözümü çalıştırma

Bu görevde, önceki görevde oluşturduğunuz Web API'si hizmeti beklendiği gibi çalıştığını doğrulayın. Internet Explorer'ı kullanacağı **F12 Geliştirici araçlarıyla** ağ trafiğini yakalamak ve Web API'si hizmeti gelen tam yanıtı inceleyin.

> [!NOTE]
> Emin olun **Internet Explorer** seçili **Başlat** düğmesi Visual Studio araç çubuğunda yer alan.
> 
> ![Internet Explorer seçeneği](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Tuşuna **F5** çözümü çalıştırın. **Oturum** sayfası, tarayıcıda görüntülenmelidir.

    > [!NOTE]
    > Uygulama başladığında, varsayılan MVC yönlendirme, varsayılan olarak eşleştirilir tetiklenir **dizin** eylemi **HomeController** sınıfı. Bu yana **HomeController** kimliği doğrulanmış kullanıcılara kısıtlıdır (Bu sınıf ile donatılmış unutmayın **Authorize** alıştırma 1'özniteliği) ve kimliği doğrulanmış bir kullanıcı henüz uygulama yok özgün istek oturum açma sayfasında yönlendirir.

    ![Çözüm çalıştırılmadan](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "çözümü çalıştırma")

    *Çözüm çalıştırma*
2. Tıklayın **kaydetme** yeni bir kullanıcı oluşturun.

    ![Yeni bir kullanıcı kaydetme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "yeni bir kullanıcı kaydetme")

    *Yeni bir kullanıcı kaydetme*
3. İçinde **kaydetme** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetme**.

    ![Kayıt sayfası](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "kayıt sayfası")

    *Kayıt sayfası*
4. Uygulama, yeni hesabı kaydeder ve kullanıcı kimlik doğrulaması ve giriş sayfasına yönlendirilirsiniz.

    ![Kullanıcının kimliği doğrulanır](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "kimliği doğrulanmış kullanıcı")

    *Kullanıcının kimliği doğrulanır*
5. Tarayıcıda basın **F12** açmak için **Geliştirici Araçları** paneli. Tuşuna **CTRL + 4** veya **ağ** simgesine ve ardından ağ trafiğini yakalamaktan başlamak için yeşil ok düğmesi.

    ![Web API ağ yakalama başlatma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "başlatma Web API ağ yakalama")

    *Web API ağ yakalama başlatılıyor*
6. Append **API/trivia** tarayıcının adres çubuğundaki URL. Şimdi gelen yanıt ayrıntılarını inceleyeceksiniz **alma** eylem yönteminde **TriviaController**.

    ![Web API aracılığıyla bir sonraki soruya veri alma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Web API aracılığıyla bir sonraki soruya veri alma")

    *Web API aracılığıyla bir sonraki soruya veri alma*

    > [!NOTE]
    > İndirme tamamlandıktan sonra indirilen dosya ile bir eylem yapmanız istenir. İletişim kutusu, yanıt içeriği geliştiriciler araç penceresi aracılığıyla izlemek için açık bırakın.
7. Artık yanıt gövdesinin inceleyeceksiniz. Bunu yapmak için tıklatın **ayrıntıları** sekmesine ve ardından **yanıt gövdesi**. İndirilen verilerin özelliklerini içeren bir nesne olup olmadığını kontrol edebilirsiniz **seçenekleri** (listesi olduğu **TriviaOption** nesneleri), **kimliği** ve **başlığı** karşılık gelen için **TriviaQuestion** sınıfı.

    ![Web API yanıt gövdesi görüntüleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Web API yanıt gövdesi görüntüleme")

    *Web API yanıt gövdesi görüntüleme*
8. Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Alıştırma 2: SPA arabirimi oluşturma

Bu alıştırmada, ilk web ön uç kısmı Geek test, kullanarak tek sayfalı uygulama etkileşimi odaklanarak oluşturacağınız **AngularJS**. Ardından, bir soru diğerine geçiş yaparken geçiş içeriğinin bir görsel efekt zengin animasyonlar gerçekleştirmek ve CSS3 ile kullanıcı deneyimi de ekleyeceksiniz.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Görev 1 – kullanılarak AngularJS SPA arabirimi oluşturma

Bu görevde kullanacağınız **AngularJS** Geek test uygulama istemci tarafında uygulamak için. **AngularJS** tarayıcı tabanlı uygulamalar ile çoğaltan bir açık kaynak JavaScript çerçevesi *Model-View-Controller* etkinleştirme hem geliştirme ve test etme (MVC) özelliği.

Visual Studio Paket Yöneticisi konsolundan AngularJS yükleyerek başlar. Ardından, Geek test uygulama sınav sorularını ve yanıtlarını AngularJS şablon altyapısı kullanılarak işlenecek görünümü ve davranışı sağlamak için denetleyici oluşturur.

> [!NOTE]
> AngularJS hakkında daha fazla bilgi için [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Açık **Visual Studio Express 2013 Web** açın **GeekQuiz.sln** çözüm bulunan **kaynak/Ex2-CreatingASPAInterface/başlangıcı** klasör. Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.
2. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** > **NuGet Paket Yöneticisi**. Yüklemek için aşağıdaki komutu yazın **AngularJS.Core** NuGet paketi.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. İçinde **Çözüm Gezgini**, sağ **betikleri** klasörü **GeekQuiz** seçin ve proje **Ekle | Yeni klasör**. Klasör adı **uygulama** basın **Enter**.
4. Sağ **uygulama** az önce oluşturduğunuz ve seçin klasör **Ekle | JavaScript dosyası**.

    ![Yeni bir JavaScript dosyası oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Yeni bir JavaScript dosyası oluşturma*
5. İçinde **öğe için ad belirtmek** iletişim kutusuna *test denetleyicisi* içinde **öğe adı** metin kutusu ve tıklatın **Tamam**.

    ![Yeni bir JavaScript dosyası adlandırma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Yeni bir JavaScript dosyası adlandırma*
6. İçinde **test controller.js** bildirmek ve AngularJS başlatmak için aşağıdaki kodu ekleyin **QuizCtrl** denetleyicisi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Oluşturucu işlevi **QuizCtrl** denetleyicisi adlı injectable bir parametre bekliyor **$scope**. Kapsamı yapının başlangıç durumunun Oluşturucusu işlev özellikleri ekleyerek ayarlanması gereken **$scope** nesne. Özellikleri içeren **görünüm modeli**ve denetleyiciye kaydedildiğinde şablona erişilebilir olacaktır.
    > 
    > **QuizCtrl** denetleyicisi adlı bir modül içinde tanımlanan **QuizApp**. Olanak tanıyan iş birimleri, uygulamanızın ayrı bileşenlere ayırmak modüllerdir. Modüller kullanmanın başlıca avantajlarından kod anlamak daha kolay olur ve birim testi, kullanılabilirlik ve bakımı kolaylaştıran olur.
7. Görünümden tetiklenen olaylara tepki vermek için kapsama davranış artık ekleyeceksiniz. Sonuna aşağıdaki kodu ekleyin **QuizCtrl** tanımlamak için denetleyici **nextQuestion** işlevi **$scope** nesne.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Bu işlev öğesinden sonraki soruya alır **Meraklısına Notlar** Web API'si önceki alıştırmada oluşturulan ve soru veri ekler **$scope** nesne.
8. Sonuna aşağıdaki kodu ekleyin **QuizCtrl** tanımlamak için denetleyici **sendAnswer** işlevi **$scope** nesne.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Bu işlev için kullanıcı tarafından seçilen bir yanıt gönderir **Meraklısına Notlar** Web API ve sonucu – örneğin yanıt doğru değilse veya – depolar **$scope** nesne.
    > 
    > **NextQuestion** ve **sendAnswer** yukarıdaki işlevlerini AngularJS kullanan **$http** XMLHttpRequest aracılığıyla Web API'si ile iletişimi soyutlamak için nesne Tarayıcıda JavaScript nesnesi. AngularJS soyutlama RESTful API'ler aracılığıyla bir kaynağa karşı bir CRUD işlemleri gerçekleştirmek için yüksek seviyede getiren başka bir hizmete destekler. AngularJS **$resource** nesne ile etkileşime gerek kalmadan üst düzey davranışları sağlayan eylem yöntemlerine sahip **$http** nesne. Kullanmayı göz önünde bulundurun **$resource** CRUD modelin gerektirdiği nesne senaryolarda (fore bilgi bkz [$resource belgeleri](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Sonraki adım, sınavı görünümünü tanımlayan AngularJS şablonu oluşturmaktır. Bunu yapmak için açık **Index.cshtml** içinde dosya **görünümleri | Giriş** klasörü ve içeriğini aşağıdaki kodla değiştirin.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS, model ve denetleyici bilgilerinden tarayıcıda kullanıcının gördüğü dinamik görünüm static işaretleme dönüştürmek için kullandığı bir bildirim temelli belirtimi şablonudur. AngularJS öğeleri ve şablonda kullanılabilir öğesi özniteliklerinin örnekleri aşağıda verilmiştir:
    > 
    > - **Ng-app** yönergesi, uygulama kök öğesini temsil eden DOM öğesi AngularJS söyler.
    > - **Ng-controller** yönergesi bir denetleyici burada yönergesi bildirilmiş bir noktada DOM ekler.
    > - Küme ayracı gösterimi **{{}}** denetleyicide tanımlanan kapsam özellikleri için bağlamaları belirtir.
    > - **Ng tıklamayla** yönergesi kullanıcı tıklamalara yanıt kapsamında tanımlanan işlevleri çağırmak için kullanılır.
10. Açık **Site.css** içinde dosya **içerik** klasörü ve aşağıdaki vurgulanan stilleri test görünümü için bir görünüm sağlamak için dosyanın sonuna ekleyin.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümü çalıştırma

Bu görevde, yürütecek çözümü kullanarak yeni kullanıcı arabirimi test sorulara yanıt AngularJS derlediğiniz.

1. Tuşuna **F5** çözümü çalıştırın.
2. Yeni bir kullanıcı hesabına kaydolun. Bunu yapmak için görev 3 alıştırma 1'de açıklanan kayıt adımları izleyin.

    > [!NOTE]
    > Önceki alıştırmada çözümü kullanıyorsanız, daha önce oluşturduğunuz kullanıcı hesabıyla oturum açabilir.
3. **Giriş** sayfası görüntülenmelidir, test ilk soru gösteriliyor. Seçeneklerden birini tıklatarak soruyu yanıtlayın. Bu tetikleyecek **sendAnswer** gönderen seçili seçeneği, daha önce tanımlanan işlevi **Meraklısına Notlar** Web API'si.

    ![Bir soruyu yanıtlarken](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "soru yanıtlama")

    *Bir soru yanıtlama*
4. Düğmelerden birine tıklandıktan sonra yanıt görüntülenmesi gerekir. Tıklayın **sonraki soruya** aşağıdaki soru gösterilecek. Bu tetikleyecek **nextQuestion** denetleyicide tanımlanan işlevi.

    ![Sonraki soruya isteyen](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "sonraki soruya isteme")

    *Sonraki soruya isteme*
5. Sonraki soruya görüntülenmesi gerekir. İstediğiniz gibi birçok kez soru yanıtlama devam edin. Tüm soruları tamamladıktan sonra ilk yanıtını döndürmesi gerekir.

    ![Başka bir soru](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "başka bir soru")

    *Sonraki Soru*
6. Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Görev 3 – CSS3 kullanarak çevir bir animasyon oluşturma

Bu görevde CSS3 özellikleri zengin animasyonlar Çevir etkili bir soru yanıtlandığında ve sonraki soruya alındığında ekleyerek gerçekleştirmek için kullanın.

1. İçinde **Çözüm Gezgini**, sağ **içerik** klasörü **GeekQuiz** seçin ve proje **Ekle | Mevcut öğe...** .

    ![Var olan bir öğe için içerik klasörünü ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "içerik klasörüne var olan öğe ekleme")

    *İçerik klasörüne var olan öğe ekleme*
2. İçinde **Varolan Öğe Ekle** iletişim kutusunda, gitmek **kaynak/varlıklar** klasörü ve select **Flip.css**. **Ekle**'yi tıklatın.

    ![Varlıklarından Flip.css dosya ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "varlıklarından Flip.css dosya ekleme")

    *Varlıklarından Flip.css dosya ekleme*
3. Açık **Flip.css** eklemiş dosya ve içeriğini inceleyin.
4. Bulun **ters çevirmek, dönüşümü** açıklaması. CSS stilleri açıklamanın aşağıda kullanan **perspektif** ve **rotateY** dönüşümler oluşturmak için bir &quot;kartı çevir&quot; efekt.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Bulun **Çevir sırasında arkasına Bölmesini Gizle** açıklaması. Ayarlayarak uzağa Görüntüleyicisi karşılaşıyorsanız, açıklamanın aşağıda stili dikdörtgenlerini arka tarafı gizler **arkayüz görünürlük** CSS özelliği *gizli*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Açık **BundleConfig.cs** içinde dosya **uygulama\_Başlat** klasörü ve referansı eklemek **Flip.css** dosyası **&quot;~/Content/css&quot;** stil paket

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Tuşuna **F5** çözüm ve kimlik bilgilerinizle oturum çalıştırılacak.
8. Seçeneklerden birine tıklayarak soru yanıtlayın. Çevirme etkisi görünümler arasında geçiş yaparken dikkat edin.

    ![Çevirme etkisi olan bir soruyu yanıtlarken](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Çevir etkisi olan soru yanıtlama")

    *Çevirme etkisi olan soru yanıtlama*
9. Tıklayın **sonraki soruya** aşağıdaki soru alınacak. Çevirme etkisi yeniden görüntülenmesi gerekir.

    ![Aşağıdaki soru Çevir etkisi olan alınırken](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Çevir etkisi olan aşağıdaki soru alınıyor")

    *Aşağıdaki soru Çevir etkisi olan alınıyor*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı laboratuvarı tamamlayarak nasıl yapılır:

- ASP.NET iskeleti oluşturma kullanarak bir ASP.NET Web API denetleyicisi oluşturma
- Sonraki test soruya almak için bir Web API'sini kullanmaya başlamak eylemi gerçekleştir
- Sınav yanıtlarını depolamak için bir Web API sonrası eylemi gerçekleştir
- AngularJS Visual Studio Paket Yöneticisi konsolundan yükleme
- AngularJS şablonları uygulamak ve denetleyiciler
- CSS3 geçişleri animasyon efektleri gerçekleştirmek için kullanın
