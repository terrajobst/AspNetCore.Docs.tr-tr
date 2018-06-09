---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratuvar durum: ASP.NET Web API ve Angular.js tek sayfalı uygulama (SPA) yapı | Microsoft Docs'
author: rick-anderson
description: Geleneksel web uygulamalarında bir sayfa isteyerek istemci (tarayıcı) sunucusu ile iletişim başlatır. Sunucu sonra isteğini işler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566484"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratuvar durum: ASP.NET Web API ve Angular.js tek sayfalı uygulama (SPA) derleme
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](http://aka.ms/webcamps-training-kit)

> Geleneksel web uygulamalarında bir sayfa isteyerek istemci (tarayıcı) sunucusu ile iletişim başlatır. Sunucu isteği işler ve sayfanın HTML istemciye gönderir. Sonraki etkileşimlere – örneğin kullanıcı bağlantısını gider veya verileri içeren bir form gönderen – sayfa yeni bir istek sunucuya gönderilir ve akış yeniden başlatır: sunucu isteği işler ve yeni sayfa, tarayıcı yeni eylem isteğine yanıt olarak gönderir. İstemci tarafından Ed.
> 
> Tek sayfa uygulamaları (SPAs) sayfanın tamamını sonra ilk istek tarayıcıda yüklendi ancak sonraki etkileşimler Ajax istekleri üzerinden gerçekleşir. Bu tarayıcı yalnızca değişti sayfa bölümünü güncelleştirmek sahip olduğu anlamına gelir; Tüm sayfayı yeniden yüklemek için gerek yoktur. SPA yaklaşım kullanıcı Eylemler, daha esnektir bir deneyimi kaynaklanan yanıtlamak için uygulama tarafından harcanan süre azaltır.
> 
> Bir SPA mimarisini geleneksel web uygulamaları mevcut olmayan bazı zorluklar içerir. Ancak, ASP.NET Web API gibi teknolojileri Gelişmekte olan, AngularJS JavaScript çerçeveleri ister ve CSS3 tarafından sağlanan yeni stil özellikleri gerçekten tasarlama ve oluşturma SPAs kolaylaştırır.
> 
> Bu elle üzerinde laboratuar ortamında günlük test, SPA kavramı dayalı bir trivia Web sitesi uygulamak için bu teknolojilerden sürer. İlk test sorularını almak ve yanıtları depolamak için gerekli uç noktalarını kullanıma sunmak için ASP.NET Web API ile hizmet katmanı gerçekleştireceksiniz. Ardından, AngularJS ve CSS3 dönüştürme efektleri kullanarak zengin ve esnek bir kullanıcı Arabirimi oluşturacaksınız.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- JSON veri göndermek ve almak için bir ASP.NET Web API hizmeti oluşturma
- AngularJS kullanarak esnek bir kullanıcı Arabirimi oluşturma
- CSS3 dönüşümleri UI deneyiminizi geliştirmek

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük

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

1. [Web API'si oluşturma](#Exercise1)
2. [SPA arabirimi oluşturma](#Exercise2)

Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**

> [!NOTE]
> Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir. Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Alıştırma 1: Web API'si oluşturma

Bir SPA anahtar bölümleri hizmet katmanı biridir. Kullanıcı Arabirimi ve bu çağrısının yanıtı veri döndürmeyi tarafından gönderilen Ajax çağrıları işlemekten sorumludur. Alınan veri ayrıştırılması ve istemci tarafından tüketilen için makine tarafından okunabilir bir biçimde sunulacaktır.

Web API çerçevesi ASP.NET yığını bir parçasıdır ve genellikle veri gönderme ve JSON veya XML biçimli bir RESTful API'si aracılığıyla alma HTTP Hizmetleri, uygulama kolaylaştırmak için tasarlanmıştır. Bu alıştırmada günlük test uygulamayı barındıran ve kullanıma sunmak ve ASP.NET Web API kullanarak test verilerini kalıcı hale getirmek için arka uç hizmetine uygulamak için bu Web sitesi oluşturur.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Görev 1 – günlük test için başlangıç projesi oluşturma

Bu görevde ASP.NET Web API dayalı için yeni bir ASP.NET MVC proje desteğiyle oluşturmaya başlayacağını **bir ASP.NET** proje Visual Studio ile birlikte gelen türü. **Bir ASP.NET** tüm ASP.NET teknolojileri birleştirir ve karışık ve bunların istendiği gibi eşleştirmek için seçeneği sunar. Ardından, Entity Framework'ün modeli sınıfları ve test sorularını eklemek için veritabanı initializator ekleyeceksiniz.

1. Açık **için Visual Studio Express 2013 Web** seçip **dosya | Yeni proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **ASP.NET Web uygulaması** altında **Visual C# | Web** sekmesi. Emin olun **.NET Framework 4.5** olduğunu belirlenirse, ad *GeekQuiz*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni bir ASP.NET Web uygulaması projesi oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "yeni bir ASP.NET Web uygulaması projesi oluşturma")

    *Yeni bir ASP.NET Web uygulaması projesi oluşturma*
3. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC** şablonu ve select **Web API** seçeneği. Ayrıca, olduğundan emin olun **kimlik doğrulaması** seçeneği **tek tek kullanıcı hesaplarını**. Devam etmek için **Tamam** 'a tıklayın.

    ![Web API bileşenleri de dahil olmak üzere, MVC şablonu ile yeni bir proje oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Web API bileşenleri de dahil olmak üzere, MVC şablonu ile yeni bir proje oluşturma*
4. İçinde **Çözüm Gezgini**, sağ **modelleri** klasöründe **GeekQuiz** proje ve seçin **Ekle | Varolan öğeyi...** .

    ![Var olan bir öğe ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "var olan bir öğe ekleme")

    *Var olan bir öğe ekleme*
5. İçinde **varolan öğeyi Ekle** iletişim kutusunda, gitmek **varlıklar/kaynak/modelleri** klasörünü ve tüm dosyaları seçin. **Ekle**'yi tıklatın.

    ![Model varlıklar ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "modeli varlıklar ekleme")

    *Model varlıklar ekleme*

    > [!NOTE]
    > Bu dosyaları ekleyerek, veri modeli, Entity Framework'ün veritabanı bağlamı ve günlük test uygulaması için veritabanı Başlatıcısı ekliyorsunuz.
    > 
    > **Entity Framework (EF)** kavramsal uygulama modeli yerine doğrudan ilişkisel depolama Şeması'nı kullanarak programlama ile programlama tarafından veri erişimi uygulamaları oluşturmanızı sağlayan bir nesne ilişkisel Eşleyici (ORM) değil. Entity Framework hakkında daha fazla bilgiyi [burada](../../../entity-framework.md).
    > 
    > Yeni eklediğiniz sınıfları açıklaması verilmiştir:
    > 
    > - **TriviaOption:** test soruyla ilişkili tek bir seçeneği temsil eder
    > - **TriviaQuestion:** test soru temsil eder ve ilişkili seçeneklerde gösterir **seçenekleri** özelliği
    > - **TriviaAnswer:** test soruya yanıt kullanıcı tarafından seçilen seçeneğini temsil eder
    > - **TriviaContext:** günlük test uygulama Entity Framework'ün veritabanı bağlamını temsil eder. Bu sınıfın türetildiği **DContext** ve ortaya çıkaran **DbSet** yukarıda açıklanan varlıklar koleksiyonu temsil eden özelliklerinin.
    > - **TriviaDatabaseInitializer:** için Entity Framework Başlatıcı uyarlamasını **TriviaContext** devraldığı sınıfı **Createdatabaseıfnotexists**. Bu sınıf varsayılan davranışını yoksa yalnızca, veritabanı oluşturmak için varlıkları ekleme içinde belirtilen **çekirdek** yöntemi.
6. Açık **Global.asax.cs** dosya ve aşağıdaki ekleme deyimini kullanarak.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Başında aşağıdaki kodu ekleyin **uygulama\_Başlat** ayarlamak için yöntemin **TriviaDatabaseInitializer** veritabanı Başlatıcısı olarak.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Değiştirme **giriş** erişimini kısıtlamak için denetleyici kimliği doğrulanmış kullanıcılara. Bunu yapmak için açın **HomeController.cs** içinde dosya **denetleyicileri** klasör ekleyin **Authorize** özniteliğini **HomeController**sınıf tanımının.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize** filtre kullanıcının kimliği doğrulanır durumunda olup olmadığını kontrol eder. Kullanıcının kimliği doğrulanmazsa, HTTP durum kodunu 401 (yetkisiz) eylemi çağırma olmadan döndürür. Genel olarak, denetleyici düzeyinde veya bireysel eylemlerin düzeyinde filtre uygulayabilirsiniz.
9. Şimdi web sayfalarını ve markalama düzenini özelleştirin. Bunu yapmak için açın  **\_Layout.cshtml** içinde dosya **görünümleri | Paylaşılan** klasörü ve içeriği güncelleştirme **&lt;başlık&gt;** değiştirerek öğesi *My ASP.NET Application* ile *günlük test* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Aynı dosyada kaldırarak gezinti çubuğunda güncelleştirme *hakkında* ve *kişi* bağlantılar ve yeniden adlandırma *giriş* bağlantı *Yürüt*. Ayrıca, yeniden adlandırma *uygulama adı* bağlantı *günlük test*. HTML gezinti çubuğu için aşağıdaki kod gibi görünmelidir.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Düzen sayfasının altbilgi değiştirerek güncelleştirme *My ASP.NET Application* ile *günlük test*. Bunu yapmak için içeriğinin yerine geçecek **&lt;altbilgi&gt;** aşağıdaki vurgulanmış kodu sahip öğe.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Görev 2 – TriviaController Web API'si oluşturma

Önceki görevde günlük test web uygulaması başlangıç yapısını oluşturuldu. Test veri modeli ile etkileşim kurar ve aşağıdaki eylemleri kullanıma sunan basit bir Web API hizmeti şimdi oluşturacak:

- **GET/api/trivia**: kimliği doğrulanmış kullanıcı tarafından yanıtlanması gereken test listesinden sonraki soruya alır.
- **POST/api/trivia**: kimliği doğrulanmış kullanıcı tarafından belirtilen test yanıt depolar.

Web API denetleyicisi sınıfı için taban çizgisi oluşturmak için Visual Studio tarafından sağlanan ASP.NET yapı İskelesi araçları kullanır.

1. Açık **WebApiConfig.cs** içinde dosya **uygulama\_Başlat** klasör. Bu dosya yolları Web API denetleyici eylemleri için nasıl eşlendiğini gibi Web API hizmet yapılandırmasını tanımlar.
2. Aşağıdaki using deyimi dosyasının başında.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Aşağıdaki vurgulanmış kodu ekleyin **kaydetmek** Genel Web API eylem yöntemleri tarafından alınan JSON veri biçimlendirici yapılandırmak için yöntem.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** özellik adlarını otomatik olarak dönüştürür *ortası büyük* JavaScript özellik adları için genel kuraldır durumda.
4. İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasöründe **GeekQuiz** proje ve seçin **Ekle | Yeni iskele kurulmuş öğe...** .

    ![Yeni iskele kurulmuş öğe oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "yeni iskele kurulmuş öğe oluşturma")

    *Yeni iskele kurulmuş öğe oluşturma*
5. İçinde **İskele Ekle** iletişim kutusunda, olduğundan emin olun **ortak** düğümü sol bölmede seçili. Ardından, seçin **Web API 2 denetleyici - boş** tıklatın ve Orta bölmede şablonunda **Ekle**.

    ![Web API 2 denetleyicisi boş şablonu seçilmesi](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Web API 2 denetleyicisi boş şablonu seçme")

    *Web API 2 denetleyicisi boş şablonu seçme*

    > [!NOTE]
    > **ASP.NET İskele** bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Visual Studio 2013 MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir. Standart veri işlemleri geliştirmek için süre miktarını azaltmak için veri modelleri ile etkileşime giren kodu gereken hızla eklemek istediğinizde projenizde yapı iskelesi kullanmanız gerekir.
    > 
    > Yapı iskelesi işlem ayrıca gerekli bağımlılıkları projede yüklü olan sağlar. Boş bir ASP.NET projeyi başlatın ve sonra Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gereken Web API NuGet paket ve başvuruları projenize otomatik olarak eklenir.
6. İçinde **denetleyici Ekle** iletişim kutusuna *TriviaController* içinde **Denetleyici adı** metin kutusu ve tıklatın **Ekle**.

    ![Trivia denetleyicisi ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Trivia denetleyicisi ekleme")

    *Trivia denetleyicisi ekleme*
7. **TriviaController.cs** dosya eklenen sonra **denetleyicileri** klasöründe **GeekQuiz** içeren boş bir proje **TriviaController** sınıfı. Aşağıdaki using deyimlerini dosyanın başında.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Başında aşağıdaki kodu ekleyin **TriviaController** tanımlamak, başlatmak ve silmek için sınıf **TriviaContext** denetleyici örneği.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** yöntemi **TriviaController** çağırır **Dispose** yöntemi **TriviaContext** sağlar örneği tüm bağlam nesnesi tarafından kullanılan kaynakları serbest zaman **TriviaContext** örneği atıldı veya Çöp toplanır. Bu Entity Framework tarafından açılan tüm veritabanı bağlantıları kapatarak içerir.
9. Sonunda aşağıdaki yardımcı yöntemi ekleyin **TriviaController** sınıfı. Bu yöntem, belirtilen kullanıcı tarafından yanıtlanması gereken veritabanından aşağıdaki test soru alır.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Aşağıdakileri ekleyin **almak** eylem yöntemine **TriviaController** sınıfı. Bu eylem yöntemini çağırır **NextQuestionAsync** kimliği doğrulanmış kullanıcı için sonraki soruya almak için önceki adımda tanımlanan yardımcı yöntemi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Sonunda aşağıdaki yardımcı yöntemi ekleyin **TriviaController** sınıfı. Bu yöntem, belirtilen yanıt veritabanında depolar ve yanıt doğru olup olmadığını gösteren bir Boole değeri döndürür.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Aşağıdakileri ekleyin **Post** eylem yöntemine **TriviaController** sınıfı. Bu eylem yöntemine çağrı ve kimliği doğrulanmış kullanıcı yanıtı ilişkilendirir **StoreAsync** yardımcı yöntemi. Ardından, yardımcı yöntemi tarafından döndürülen Boole değeri ile bir yanıt gönderir.

    (Kod parçacığını - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Ekleyerek kimliği doğrulanmış kullanıcılara erişimi kısıtlamak için Web API denetleyicisi değiştirme **Authorize** özniteliğini **TriviaController** sınıf tanımının.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3 – çözümünü çalıştırma

Bu görevde önceki görevde yerleşik Web API hizmeti beklendiği gibi çalıştığını doğrularsınız. Internet Explorer'ı kullanacağı **F12 Geliştirici Araçları** ağ trafiğini yakalamak ve Web API hizmetinden tam yanıtı inceleyin.

> [!NOTE]
> Olduğundan emin olun **Internet Explorer** seçildiyse **Başlat** düğmesi Visual Studio araç çubuğunda yer alan.
> 
> ![Internet Explorer seçeneği](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Tuşuna **F5** çözümü çalıştırın. **Oturum** sayfasını tarayıcıda görünmelidir.

    > [!NOTE]
    > Uygulama başladığında, varsayılan MVC yol, varsayılan olarak eşlenmiş tetiklenir **dizin** eylemi **HomeController** sınıfı. Bu yana **HomeController** kimliği doğrulanmış kullanıcılar için kısıtlı (Bu sınıfı ile donatılmış unutmayın **Authorize** alıştırma 1 özniteliğinde) ve kimliği doğrulanmış bir kullanıcı henüz, uygulama yok ilk istek oturum açma sayfasında yönlendirir.

    ![Çözümü çalıştırırken](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "çözümünü çalıştırma")

    *Çözüm çalıştırma*
2. Tıklatın **kaydetmek** yeni bir kullanıcı oluşturmak için.

    ![Yeni bir kullanıcı kaydetme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "yeni bir kullanıcı kaydetme")

    *Yeni bir kullanıcı kaydetme*
3. İçinde **kaydetmek** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetmek**.

    ![Sayfayla kaydetmek](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "kayıt sayfası")

    *Kayıt sayfası*
4. Yeni hesap uygulama kaydeder ve kullanıcı kimlik doğrulaması ve giriş sayfasına yeniden yönlendirildi.

    ![Kullanıcının kimliği doğrulanır](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "kimliği doğrulanmış kullanıcı")

    *Kullanıcının kimliği doğrulanır*
5. Tarayıcıda basın **F12** açmak için **Geliştirici Araçları** paneli. Tuşuna **CTRL + 4** veya **ağ** simgesine ve ardından ağ trafiğini yakalama başlamak için yeşil ok düğmesine.

    ![Web API ağ yakalama başlatma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "başlatma Web API ağ yakalama")

    *Web API ağ yakalama başlatılıyor*
6. Append **API/trivia** tarayıcının adres çubuğundaki URL'ye. Yanıttan ayrıntılarını şimdi inceler **almak** eylem yönteminde **TriviaController**.

    ![Web API'si aracılığıyla sonraki soru verilerini alma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Web API'si aracılığıyla sonraki soru verilerini alma")

    *Web API'si aracılığıyla sonraki soru verilerini alma*

    > [!NOTE]
    > Yükleme tamamlandıktan sonra indirilen dosya ile bir eylem yapmanız istenir. İletişim kutusu, geliştiricilerin araç penceresi aracılığıyla yanıt içeriği izlemek için açık bırakın.
7. Şimdi yanıtın gövdesini inceler. Bunu yapmak için tıklatın **ayrıntıları** sekmesini ve sonra **yanıt gövdesi**. Karşıdan yüklenen veri özelliklere sahip bir nesne olup olmadığını denetleyin **seçenekleri** (listesi olduğu **TriviaOption** nesneler), **kimliği** ve **başlık** , karşılık **TriviaQuestion** sınıfı.

    ![Web API yanıt gövdesi görüntüleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Web API yanıt gövdesi görüntüleme")

    *Görüntüleme Web API yanıt gövdesi*
8. Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Alıştırma 2: SPA arabirimi oluşturma

Bu alıştırmada, ilk web ön uç kısmını günlük test, tek sayfalı uygulama kullanarak etkileşim odaklanan oluşturacaksınız **AngularJS**. Ardından zengin animasyonları gerçekleştirmek ve bir soru diğerine geçiş sırasında geçiş bağlamının görsel bir efekt sağlamak için CSS3 ile kullanıcı deneyimi ekleyeceksiniz.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Görev 1 – kullanarak AngularJS SPA arabirimi oluşturma

Bu görevde kullanacağınız **AngularJS** günlük test uygulama istemci tarafında uygulamak için. **AngularJS** tarayıcı tabanlı uygulamalar ile güçlendirir bir açık kaynak JavaScript çerçevedir *Model-View-Controller* kolaylaştırmanın hem geliştirme ve test etme (MVC) yeteneği.

Visual Studio'nun Paket Yöneticisi Konsolu'ndan AngularJS yükleyerek başlar. Ardından, günlük test uygulamayı test sorularını ve yanıtlarını AngularJS şablonu altyapısını kullanarak işlenecek görünümü ve davranışı sağlamak için denetleyici oluşturur.

> [!NOTE]
> AngularJS hakkında daha fazla bilgi için bkz [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Açık **için Visual Studio Express 2013 Web** ve açın **GeekQuiz.sln** çözüm bulunan **kaynak/Ex2-CreatingASPAInterface/başlangıç** klasör. Alternatif olarak, önceki alıştırmada elde çözümüyle devam edebilirsiniz.
2. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** | **kitaplık Paket Yöneticisi**. Yüklemek için aşağıdaki komutu yazın **AngularJS.Core** NuGet paketi.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. İçinde **Çözüm Gezgini**, sağ **betikleri** klasöründe **GeekQuiz** proje ve seçin **Ekle | Yeni klasör**. Klasör adı **uygulama** ve basın **Enter**.
4. Sağ **uygulama** az önce oluşturduğunuz ve seçin klasörü **Ekle | JavaScript dosyası**.

    ![Yeni bir JavaScript dosyası oluşturma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Yeni bir JavaScript dosyası oluşturma*
5. İçinde **öğesi için ad belirtmek** iletişim kutusuna *test denetleyicisi* içinde **öğe adı** metin kutusu ve tıklatın **Tamam**.

    ![Yeni JavaScript dosyası adlandırma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Yeni JavaScript dosyası adlandırma*
6. İçinde **test controller.js** dosya, bildirme ve AngularJS başlatmak için aşağıdaki kodu ekleyin **QuizCtrl** denetleyicisi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Oluşturucu işlevi **QuizCtrl** denetleyicisi adlı injectable bir parametre bekler **$scope**. Kapsam ilk durumunda Oluşturucusu işlevinde özelliklerine ekleyerek ayarlanması gereken **$scope** nesnesi. Özellikleri içeren **görünüm modeli**ve denetleyici kaydedildiğinde şablona erişilebilir olacaktır.
    > 
    > **QuizCtrl** denetleyicisi adlı bir modül içinde tanımlanan **QuizApp**. Olanak tanıyan iş birimleri ayrı bileşenlerine, uygulamanızın çalışmamasına modüllerdir. Ana modülleri kullanmanın yararları, kod anlamak daha kolay olur ve birim testi, yeniden kullanılırlığı ve bakımı kolaylaştırır.
7. Görünümden tetiklenen olaylarına tepki vermek için kapsama davranışı şimdi ekleyeceksiniz. Sonuna aşağıdaki kodu ekleyin **QuizCtrl** tanımlamak için denetleyici **nextQuestion** işlevi **$scope** nesnesi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Bu işlev gelen sonraki soruya alır **Trivia** Web API önceki alıştırmada oluşturulan ve soru verileri iliştirir **$scope** nesne.
8. Sonuna aşağıdaki kodu ekleyin **QuizCtrl** tanımlamak için denetleyici **sendAnswer** işlevi **$scope** nesnesi.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Bu işlev için kullanıcı tarafından seçilen yanıt gönderir **Trivia** Web API ve sonuç – yani yanıt doğru olup olmadığını – depolar **$scope** nesnesi.
    > 
    > **NextQuestion** ve **sendAnswer** üstten işlevlerini kullanan AngularJS **$http** Web API XMLHttpRequest üzerinden iletişimi soyut nesnesi Tarayıcıdan JavaScript nesne. AngularJS RESTful API'leri aracılığıyla bir kaynak karşı CRUD işlemleri gerçekleştirmek için Özet daha yüksek düzeyde getirir başka bir hizmete destekler. AngularJS **$resource** nesnesi ile etkileşim kurmak zorunda kalmadan üst düzey davranışları sağlayan eylem yöntemlerine sahiptir **$http** nesnesi. Kullanmayı **$resource** CRUD modeli gerektiriyor senaryoları nesnesinde (ön bilgi bkz [$resource belgelerine](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Sonraki adım testi görünümünü tanımlayan AngularJS şablonunu oluşturmaktır. Bunu yapmak için açın **Index.cshtml** içinde dosya **görünümleri | Giriş** klasör ve içeriğini aşağıdaki kodla değiştirin.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS şablonu, kullanıcı tarayıcıda görür dinamik görünüm static işaretleme dönüştürmek için model ve denetleyici dosyasındaki bilgileri kullanır bildirim temelli bir özelliğidir. AngularJS öğeleri ve bir şablonda kullanılan öğesi özniteliklerini örnekleri aşağıda verilmiştir:
    > 
    > - **Ng uygulama** yönergesi uygulama kök öğesinin temsil eden DOM öğesi AngularJS söyler.
    > - **Ng denetleyicisi** yönergesi yönergesi bildirilen burada noktada DOM için bir denetleyici ekler.
    > - Süslü ayraç gösterimi **{{}}** denetleyicide tanımlanan kapsam özellikleri bağlamalar gösterir.
    > - **Ng tıklatma** yönergesi kullanıcı tıklama yanıtı kapsamda tanımlanan işlevleri çağırmak için kullanılır.
10. Açık **Site.css** içinde dosya **içerik** klasörü ve aşağıdaki vurgulanan stiller test görünümü için bir görünüm sağlamak için dosyanın sonuna ekleyin.

    (Kod parçacığını - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Görev 2 – çözümünü çalıştırma

Yürütülmez bu görevde yeni kullanıcı kullanarak çözüm arabirim, bazı test sorularını yanıtlamak için AngularJS ile oluşturulmuş.

1. Tuşuna **F5** çözümü çalıştırın.
2. Yeni bir kullanıcı hesabı kaydedin. Bunu yapmak için görev 3 alıştırma 1'de açıklanan kayıt adımları izleyin.

    > [!NOTE]
    > Önceki alıştırmada çözümden kullanıyorsanız, önce oluşturulan kullanıcı hesabıyla oturum açabilir.
3. **Giriş** sayfa görünmelidir, test ilk soru gösterme. Seçeneklerden birine tıklayarak soruyu yanıtlayın. Bu tetikleyecek **sendAnswer** seçili seçeneği gönderen daha önce tanımlanan işlevi **Trivia** Web API.

    ![Bir soruyu yanıtlarken](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "bir soru yanıtlama")

    *Bir soru yanıtlama*
4. Düğmeleri birini tıkladıktan sonra yanıt görüntülenmesi gerekir. Tıklatın **sonraki soruya** aşağıdaki soruyu göstermek için. Bu tetikleyecek **nextQuestion** denetleyicide tanımlanan işlevi.

    ![Sonraki soruya isteyen](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "sonraki soruya isteme")

    *Sonraki soruya isteme*
5. Sonraki soruya görüntülenmesi gerekir. İstediğiniz sayıda tekrar soru yanıtlama devam edin. Tüm soruları tamamladıktan sonra ilk soru dönmelidir.

    ![Başka bir soru](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "başka bir soru")

    *Sonraki Soru*
6. Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Görev 3 – CSS3 kullanarak çevir bir animasyon oluşturma

Bu görevde bir soru yanıtlandığında ve sonraki soruya alındığında Çevir etkisi ekleyerek zengin animasyonları gerçekleştirmek için CSS3 özellikleri kullanır.

1. İçinde **Çözüm Gezgini**, sağ **içerik** klasöründe **GeekQuiz** proje ve seçin **Ekle | Varolan öğeyi...** .

    ![Varolan öğeyi içerik klasörüne ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "içerik klasörüne var olan bir öğe ekleme")

    *Varolan öğeyi içerik klasörüne ekleniyor*
2. İçinde **varolan öğeyi Ekle** iletişim kutusunda, gitmek **kaynak/varlıklar** klasörü ve select **Flip.css**. **Ekle**'yi tıklatın.

    ![Varlıklarından Flip.css dosya ekleme](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "varlıklarından Flip.css dosya ekleme")

    *Varlıklarından Flip.css dosya ekleme*
3. Açık **Flip.css** , yeni eklenen dosya ve içeriğini inceleyin.
4. Bulun **Çevir dönüştürme** açıklama. CSS stilleri açıklamanın aşağıda kullanan **perspektif** ve **rotateY** dönüşümleri oluşturmak için bir &quot;kartı çevir&quot; etkisi.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Bulun **sırasında Çevir arkasına Bölmesini Gizle** açıklama. Ayarlayarak çıktığınızda Görüntüleyicisi bakacak olmadığında açıklamanın altındaki stil yüz arka tarafında gizler **backface görünürlük** CSS özelliğine *gizli*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Açık **BundleConfig.cs** içinde dosya **uygulama\_Başlat** klasörü ve başvuru ekleyin **Flip.css** dosyasını **&quot;~/Content/css&quot;** stili paket

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Tuşuna **F5** çözümü ve kimlik bilgilerinizi oturum çalıştırmak için.
8. Seçeneklerden birine tıklayarak bir soruyu yanıtlayın. Görünümleri arasında geçiş Çevir etkisi fark.

    ![Çevir etkisi olmadan bir soru yanıtlama](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Çevir etkisi olmadan bir soru yanıtlama")

    *Çevir etkisi olmadan bir soru yanıtlama*
9. Tıklatın **sonraki soruya** aşağıdaki soruyu alınamadı. Çevir etkisi yeniden görüntülenmesi gerekir.

    ![Çevir etkisi aşağıdaki soruyla alma](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Çevir etkisi aşağıdaki soruyla alınıyor")

    *Çevir etkisi aşağıdaki soruyla alınıyor*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Öğrendiğiniz Bu uygulamalı Laboratuvar tamamlayarak nasıl yapılır:

- ASP.NET İskele kullanarak bir ASP.NET Web API denetleyicisi oluşturun.
- Sonraki test soru almak için bir Web API alma eylemi gerçekleştir
- Test yanıtları depolamak için bir Web API sonrası eylemi gerçekleştir
- AngularJS Visual Studio Paket Yöneticisi konsolundan yükleme
- Uygulama AngularJS şablonları ve denetleyiciler
- Animasyon efektleri gerçekleştirmek için CSS3 geçişler kullanın
