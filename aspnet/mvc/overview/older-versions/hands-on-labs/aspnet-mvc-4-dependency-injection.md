---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 bağımlılık ekleme | Microsoft Docs"
author: rick-anderson
description: "Not: Bu uygulamalı Laboratuvar ASP.NET MVC ve ASP.NET MVC 4 filtreleri temel bilgiye sahip olduğunu varsayar. ASP.NET MVC 4 filtrelerden önce biz rec kullanmadıysanız..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 bağımlılık ekleme
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> [!NOTE]
> Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC** ve **ASP.NET MVC 4 filtreler**. Değil kullandıysanız **ASP.NET MVC 4 filtreler** gitmenizi öneririz önce **ASP.NET MVC özel eylem filtrelerini** uygulamalı Laboratuvar.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


İçinde **nesne odaklı programlama** standardı, nesneleri çalışma birlikte işbirliği modelinde Katkıda Bulunanlar ve tüketicilerin olduğu. Doğal olarak, bu iletişim modelini nesneleri ve karmaşıklığını artırır yönetmek zor hale bileşenler arasındaki bağımlılıkları oluşturur.

![Sınıf bağımlılıkları ve model karmaşıklık](aspnet-mvc-4-dependency-injection/_static/image1.png "sınıf bağımlılıkları ve model karmaşıklık")

*Sınıf bağımlılıklar ve model karmaşıklık*

Büyük olasılıkla hakkında duymuş **Fabrika düzeni** ve arabirim ve Hizmetleri, istemci nesneleri genellikle hizmet konumu için sorumlu olduğu kullanarak uygulama arasında ayrım.

Bağımlılık ekleme düzeni ters çevirmeyi denetiminin belirli bir uygulamasıdır. **Tersine çevirme (IOC) denetiminin** nesneler üzerinde bunlar kullanan işlerini yapmak için diğer nesneleri oluşturmayın anlamına gelir. Bunun yerine, bir dış kaynaktan (örneğin, bir xml yapılandırma dosyası) ihtiyaç duydukları nesneleri alın.

**Bağımlılık ekleme (dı)** bu nesne müdahalesi olmadan genellikle Oluşturucusu parametrelerini geçirir framework bileşeni tarafından yapılır ve özelliklerini ayarlama anlamına gelir.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Bağımlılık ekleme (dı) tasarım deseni

Yüksek bir düzeyde bağımlılık ekleme amacı olan istemci sınıfı (örneğin *golfçü*) bir arabirim karşılayan bir şey gerekir (örneğin *IClub*). Somut türde nedir önemli değildir (örneğin *WoodClub, IronClub, WedgeClub* veya *PutterClub*), işleyen başka birinin istediği (iyi bir örneğin *caddy*). ASP.NET mvc'de bağımlılık çözümleyiciyi bağımlılık mantığınızı başka bir yere kaydetmenizi izin verebilir (örn. bir kapsayıcı veya bir *Sinek paketi*).

![Bağımlılık ekleme işlemi diyagramı](aspnet-mvc-4-dependency-injection/_static/image2.png "bağımlılık ekleme çizim")

*Bağımlılık ekleme - Golf benzerleme*

Bağımlılık ekleme düzeni ve denetim ters çevirmeyi kullanmanın avantajları şunlardır:

- Sınıf azaltır
- Kodu yeniden kullanma artırır
- Kod bakımı artırır
- Uygulamayı test artırır

> [!NOTE]
> Bağımlılık ekleme bazen soyut Fabrika tasarım deseni ile karşılaştırıldığında, ancak her iki yaklaşımın arasında küçük bir fark yoktur. DI oluşturucular ve kaydedilen Hizmetleri çağırarak bağımlılıkları çözmek için arkasında çalışma bir çerçeve vardır.


Bağımlılık ekleme düzeni anladığınıza göre ASP.NET MVC 4'te uygulama bu Laboratuvar öğreneceksiniz. Bağımlılık ekleme kullanılarak başlayacak **denetleyicileri** Veritabanı Erişim hizmeti eklenecek. Ardından, bağımlılık ekleme için geçerli olacaktır **görünümleri** bir hizmeti kullanmak ve bilgileri gösterir. Son olarak, çözümü özel eylem filtresinde injecting ASP.NET MVC 4 filtreleri için dı uzatır.

Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:

- ASP.NET MVC 4 ile Unity NuGet paketlerini kullanarak bağımlılık ekleme için tümleştirme
- ASP.NET MVC denetleyicisinin içinde bağımlılık ekleme kullanın
- ASP.NET MVC görünümü içinde bağımlılık ekleme kullanın
- Bir ASP.NET MVC eylem filtresi içinde bağımlılık ekleme kullanın

> [!NOTE]
> Bu Laboratuvar Unity.Mvc3 NuGet paketi için bağımlılık çözümlemesi kullanıyor, ancak ASP.NET MVC 4 ile çalışmak için hiçbir bağımlılık ekleme Framework uyum mümkündür.


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleme**

Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir. Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek B: kullanarak kod parçacıkları](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:

1. [Alıştırma 1: bir denetleyici Injecting](#Exercise1)
2. [Alıştırma 2: bir görünüm Injecting](#Exercise2)
3. [Alıştırma 3: Filtreleri Injecting](#Exercise3)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Alıştırma 1: bir denetleyici Injecting

Bu alıştırmada, bağımlılık ekleme ASP.NET MVC denetleyicileri bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğreneceksiniz. Bu nedenle, veri erişim mantığı ayırmak için MvcMusicStore denetleyicilerinizi içine Hizmetleri dahil edilir. Hizmetleri yardımıyla bağımlılık ekleme kullanılarak çözümlenir denetleyici Oluşturucu, yeni bir bağımlılık oluşturacak **Unity**.

Bu yaklaşım daha esnek ve daha kolay korumak ve test etmek daha az bağlı uygulamaları oluşturmak nasıl yapacağınızı gösterir. Ayrıca ASP.NET MVC, Unity ile tümleştirmek öğreneceksiniz.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>StoreManager hizmeti hakkında

Adlı depolama denetleyicisi verileri yöneten bir hizmet başlangıç çözümde şimdi sağlanan MVC müzik deposu içerir **StoreService**. Aşağıda depolama hizmeti uygulaması bulabilirsiniz. Tüm yöntemleri modeli varlıkları döndürme unutmayın.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** begin işleminden çözüm şimdi tüketir **StoreService**. Tüm veri başvuruları sıradan kaldırıldığını **StoreController**ve geçerli veri erişimi sağlayıcı tüketir herhangi bir yöntemini değiştirmeden artık mümkün **StoreService**.

Bunun altında bulabilirsiniz **StoreController** uygulama ile bir bağımlılığa sahip **StoreService** sınıfı oluşturucusu içinde.

> [!NOTE]
> Bu alıştırmada sunulan bağımlılık ilgili **ters çevirmeyi denetim** (IOC).
> 
> **StoreController** sınıfı oluşturucusu alan bir **IStoreService** sınıfı içinde hizmet çağrılarından gerçekleştirmek için gerekli olan tür parametresi. Ancak, **StoreController** herhangi bir denetleyicisi ASP.NET MVC ile çalışmak zorunda varsayılan Oluşturucusu (parametresiz) uygulamıyor.
> 
> Bağımlılık çözmek için denetleyici soyut fabrikası (belirtilen türdeki herhangi bir nesne döndürür sınıfı) tarafından oluşturulması gerekir.


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Sınıf bildirilen parametresiz bir kurucusu olduğundan StoreController hizmet nesnesi göndermeden oluşturmaya çalıştığında bir hata alırsınız.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Görev 1 - uygulama çalışıyor

Bu görevde, veri erişim uygulama mantığından ayıran depolama denetleyicisi içine hizmet içeren başlangıç uygulama çalışır.

Denetleyici Hizmeti varsayılan olarak bir parametre olarak geçirilen değil gibi uygulama çalışırken bir özel durum alırsınız:

1. Açık **başlamak** çözüm bulunan **Source\Ex01 Injecting Controller\Begin**.

    1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **Ctrl + F5** hata ayıklama olmadan uygulamayı çalıştırın. Hata iletisi alırsınız &quot; **bu nesne için tanımlanan parametresiz bir kurucusu**&quot;:

    ![ASP.NET MVC başlamak uygulaması çalıştırılırken hata](aspnet-mvc-4-dependency-injection/_static/image3.png "başlamak ASP.NET MVC uygulaması çalıştırılırken hata")

    *ASP.NET MVC başlamak uygulaması çalıştırılırken hata*
3. Tarayıcıyı kapatın.

Aşağıdaki adımlarda bu denetleyicisinin gereksinim duyduğu bağımlılık eklemesine müzik deposu çözüm üzerinde çalışır.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Görev 2 - dahil olmak üzere Unity MvcMusicStore çözüm

Bu görevde, içerecektir **Unity.Mvc3** çözüm için NuGet paketi.

> [!NOTE]
> Unity.Mvc3 paketi ASP.NET MVC 3 için tasarlanmıştır ancak ASP.NET MVC 4 ile tam uyumludur.
> 
> Unity bir hafif, Genişletilebilir bağımlılık ekleme kapsayıcısını isteğe bağlı destek ile örneği için olan ve kişiler tarafından ele yazın. Her tür .NET uygulama kullanmak için genel amaçlı bir kapsayıcıdır. İçinde bağımlılık ekleme mekanizmaları dahil olmak üzere bulunan tüm genel özellikleri sağlar: nesne oluşturma, Bileşen Yapılandırması kapsayıcısı ertelemeyi tarafından çalışma zamanı ve esneklik, bağımlılıkları belirterek gereksinimlerinin Özet.


1. Yükleme **Unity.Mvc3** NuGet paketi **MvcMusicStore** projesi. Bunu yapmak için açın **Paket Yöneticisi Konsolu** gelen **Görünüm** | **diğer pencereler**.
2. Şu komutu çalıştırın.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Unity.Mvc3 NuGet paketi Yükleniyor](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet paketi yükleniyor")

    *Unity.Mvc3 NuGet paketi yükleniyor*
3. Bir kez **Unity.Mvc3** paketi yüklendiğinde, dosya ve klasörleri otomatik olarak ekler Unity yapılandırmasını basitleştirmek için keşfedin.

    ![Yüklü Unity.Mvc3 paket](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 paketi yüklü")

    *Yüklü Unity.Mvc3 paketi*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Görev 3 - kaydetme Unity Global.asax.cs uygulamada\_Başlat

Bu görevde, güncelleştirecektir **uygulama\_Başlat** yöntemi bulunan **Global.asax.cs** Unity önyükleyici Başlatıcı arayın ve ardından, önyükleyici dosyası kaydediliyor güncelleştirmek için Denetleyici ve hizmet bağımlılık ekleme işlemi için kullanır.

1. Şimdi, Unity kapsayıcı başlatır dosyası olan önyükleyici ve bağımlılık çözümleyiciyi kanca. Bunu yapmak için açın **Global.asax.cs** ve içinde aşağıdaki vurgulanmış kodu ekleyin **uygulama\_Başlat** yöntemi.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - başlatmak Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Açık **Bootstrapper.cs** dosya.
3. Şu ad alanlarından içerir: **MvcMusicStore.Services** ve **MusicStore.Controllers**.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - önyükleyici ad alanlarını ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Değiştir **BuildUnityContainer** yöntemi depolama denetleyicisi ve depolama hizmeti kaydeder aşağıdaki kod ile içerik.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - kayıt depolama denetleyicisi ve hizmet*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, bunu şimdi Unity dahil olmak üzere sonra yüklenebilir olduğunu doğrulamak için uygulamanın çalıştırılacağı.

1. Tuşuna **F5** uygulamayı çalıştırmak için uygulama artık herhangi bir hata iletisi göstermeden yüklenecektir.

    ![Bağımlılık ekleme ile uygulama çalıştıran](aspnet-mvc-4-dependency-injection/_static/image6.png "uygulama bağımlılık ekleme ile çalıştırma")

    *Bağımlılık ekleme ile çalışan uygulama*
2. Gözat **/deposu**. Bu çağıracağı **StoreController**, şimdi oluşturulduğu kullanarak **Unity**.

    ![MVC müzik deposu](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC müzik deposu")

    *MVC müzik deposu*
3. Tarayıcıyı kapatın.

Aşağıdaki alıştırmalarda ASP.NET MVC görünümleri ve eylem filtrelerini içinde kullanılacak bağımlılık ekleme kapsamı genişletmek öğreneceksiniz.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Alıştırma 2: bir görünüm Injecting

Bu alıştırmada, bağımlılık ekleme ASP.NET MVC 4'ün yeni özelliklerle görünümünde Unity tümleştirme için nasıl kullanılacağını öğreneceksiniz. Bunu yapmak için özel bir hizmet deposu Gözat bir ileti ve bir görüntü gösterecektir görünümü içinde çağırır.

Ardından, projeyi Unity ile tümleştirmek ve bağımlılıkları eklemesine özel bağımlılık çözümleyici oluşturun.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Görev 1 - bir hizmeti kullanan bir görünüm oluşturma

Bu görevde, yeni bir bağımlılık oluşturmak için bir hizmet çağrı yapan bir görünüm oluşturur. Bu çözümdeki basit bir Mesajlaşma hizmetinde servis oluşur.

1. Açık **başlamak** çözüm bulunan **Source\Ex02 Injecting View\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
    > 
    > Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dahil **MessageService.cs** ve **IMessageService.cs** sınıfları bulunan **\Assets kaynak** klasöründe **/Hizmetleri**. Bunu yapmak için sağ **Hizmetleri** klasörü ve select **varolan öğeyi Ekle**. Dosya konumuna göz atın ve bunları ekleyebilirsiniz.

    ![İleti hizmeti ve hizmet arabirimi ekleme](aspnet-mvc-4-dependency-injection/_static/image8.png "ileti hizmeti ve hizmet arabirimi ekleme")

    *Ekleme ileti hizmeti ve hizmet arabirimi*

    > [!NOTE]
    > **IMessageService** arabirimi tanımlar tarafından uygulanan iki Özellikler **MessageService** sınıfı. Bu özellikleri -**ileti** ve **ImageUrl**-ileti ve görüntünün URL'si görüntülenecek depolar.
3. Bir klasör oluşturun **/sayfaları** projenin kök klasörünü ve ardından mevcut bir sınıf ekleyin **MyBasePage.cs** gelen **Source\Assets**. Öğesinden devralacak taban sayfası aşağıdaki yapısına sahip olabilir.

    ![Sayfaları klasöründeki](aspnet-mvc-4-dependency-injection/_static/image9.png "sayfaları klasörü")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Açık **Browse.cshtml** gelen görüntülemek **/görünümler/deposu** klasörünü ve devralınmalıdır hale **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. İçinde **Gözat** görüntülemek için bir çağrı ekleyin **MessageService** görüntü ve hizmet tarafından alınan ileti görüntülemek için.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Görev 2 - özel bağımlılık çözümleyici ve özel görünüm sayfa Etkinleştiricisini dahil

Önceki görevde bir hizmeti çağrısı içindeki gerçekleştirmek için bir görünüm içinde yeni bir bağımlılık eklenmiş. ASP.NET MVC bağımlılık ekleme arabirimleri uygulayarak bu bağımlılığı şimdi çözümleyecek **IViewPageActivator** ve **Idependencyresolver**. Çözümde uygulaması içerecektir **Idependencyresolver** , Dağıt hizmet alma ile Unity kullanarak. Ardından, başka bir özel uyarlamasını içerecektir **IViewPageActivator** görünümleri oluşturma çözmek için arabirim.

> [!NOTE]
> ASP.NET MVC 3 itibaren bağımlılık ekleme uygulamasını Hizmetleri kaydettirmek için arabirimler Basitleştirilmiş. **Idependencyresolver** ve **IViewPageActivator** bağımlılık ekleme işlemi için ASP.NET MVC 3 özellikleri parçasıdır.
> 
> **-Idependencyresolver** arabirimi önceki IMvcServiceLocator yerini alır. Idependencyresolver Implementers hizmet veya hizmet koleksiyonu örneğini döndürmelidir.
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** arabirimi görünüm sayfalarının bağımlılık ekleme nasıl oluşturulur üzerinde daha ayrıntılı denetim sağlar. Uygulayan sınıflar **IViewPageActivator** arabirimi bağlamını kullanarak görünüm örnekleri oluşturabilirsiniz.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Oluşturma /**oluşturucuları** projenin kök klasöründe.
2. Dahil **CustomViewPageActivator.cs** çözümünüzden için **/kaynakları/varlıklar/** için **oluşturucuları** klasör. Bunu yapmak için sağ **/Factories** klasöründe seçin **Ekle | Varolan öğeyi** ve ardından **CustomViewPageActivator.cs**. Bu sınıf uygulayan **IViewPageActivator** Unity kapsayıcı tutmak için arabirim.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** Unity kapsayıcısını kullanarak bir görünüm oluşturma yönetilmesinden sorumludur.
3. Dahil **UnityDependencyResolver.cs** dosya **/kaynakları/varlıklar** için **/Factories** klasör. Bunu yapmak için sağ **/Factories** klasöründe seçin **Ekle | Varolan öğeyi** ve ardından **UnityDependencyResolver.cs** dosya.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** Unity için özel bir DependencyResolver bir sınıftır. Bir hizmet Unity kapsayıcısı içindeki bulunamadığında temel çözümleyici invocated.

Aşağıdaki görev, hizmetleri ve görünümleri yerini modeli izin vermek için her iki uygulamaları kaydedilir.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Görev 3 - Unity kapsayıcıda bağımlılık ekleme işlemi için kaydetme

Bu görevde, birlikte çalışma bağımlılık ekleme yapmak için önceki bir şey sokar.

Şimdiye kadar çözümünüzü aşağıdaki öğeleri içerir:

- A **Gözat** devraldığı Görünüm **MyBaseClass** ve **MessageService**.
- Bir ara sınıf -**MyBaseClass**-için hizmet arabirimi bildirilen bağımlılık ekleme sahiptir.
- Hizmeti - **MessageService** - ve onun arabirim **IMessageService**.
- Unity için-özel bağımlılık çözümleyiciyi **UnityDependencyResolver** -hizmet alma ile ilgilidir.
- Bir görünüm sayfası Etkinleştirici - **CustomViewPageActivator** -sayfası oluşturur.

Eklemesine **Gözat** görünümü şimdi kaydettiğiniz özel bağımlılık çözümleyiciyi Unity kapsayıcısında.

1. Açık **Bootstrapper.cs** dosya.
2. Örneğini kaydetmek **MessageService** hizmeti başlatmak için Unity kapsayıcı içine:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt iletisi hizmeti*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Bir başvuru ekleyin **MvcMusicStore.Factories** ad alanı.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - oluşturucuları Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Kayıt **CustomViewPageActivator** bir görünüm sayfası Etkinleştirici Unity kapsayıcı içine olarak:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. ASP.NET MVC 4 varsayılan bağımlılık Çözümleyicisi örneği ile Değiştir **UnityDependencyResolver**. Bunu yapmak için yerini **Initialise** yöntemini aşağıdaki kodla içerik:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - güncelleştirme bağımlılık çözümleyiciyi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC varsayılan bağımlılık çözümleyici sınıfı sağlar. Unity için oluşturduk biri olarak özel bağımlılık çözümleyiciler ile çalışmak için bu çözümleyici değiştirilmesi gerekir.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde deposu Tarayıcı hizmeti kullanır ve görüntü ve alınan ileti gösterilir doğrulamak için uygulamanın çalıştırılacağı:

1. Tuşuna **F5** uygulamayı çalıştırın.
2. ' I tıklatın **Rock** bakın ve türler menüden içinde nasıl **MessageService** görünümüne eklendi ve Hoş Geldiniz iletisi ve görüntünün yüklendi. Bu örnekte, biz için giriyorsunuz &quot; **Rock**&quot;:

    ![MVC müzik deposu - görünüm ekleme](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC müzik deposu - görünümü ekleme")

    *MVC müzik deposu - görünümü ekleme*
3. Tarayıcıyı kapatın.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Alıştırma 3: Eylem filtrelerini Injecting

Önceki uygulamalı laboratuarda **özel eylem filtrelerini** filtreleri özelleştirme ve ekleme çalıştı. Bu alıştırmada, bağımlılık ekleme ile Unity kapsayıcısını kullanarak filtreleri eklemesine öğreneceksiniz. Bunu yapmak için site etkinliğini izleme özel eylemi filtre müzik deposu çözüme ekleyecek.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Görev 1 - çözümde izleme filtresi dahil olmak üzere

Bu görevde, müzik mağazası'nda bir özel eylem filtresi olayları izlemek dahil edilir. Özel eylem filtre olarak kavramları zaten önceki laboratuar ortamında davranılır &quot;özel eylem filtrelerini&quot;, yalnızca bu Laboratuvar varlıklar klasöründen filtre sınıfının içerir ve Unity için filtre sağlayıcı oluşturma:

1. Açık **başlamak** çözüm bulunan **Source\Ex03 - Injecting eylem Filter\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
    > 
    > Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dahil **TraceActionFilter.cs** dosya **/kaynakları/varlıklar** için **/filtreler** klasör.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Bu özel eylem filtresi ASP.NET izleme gerçekleştirir. Kontrol edebilirsiniz &quot;ASP.NET MVC 4 yerel ve dinamik eylem filtrelerini&quot; Laboratuvar için daha fazla başvuru.
3. Boş sınıfı ekleme **FilterProvider.cs** klasörü'nde projeye   **/filtreler.**
4. Ekleme **System.Web.Mvc** ve **Microsoft.Practices.Unity** ad alanları **FilterProvider.cs**.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı ad alanlarını ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Devralınan sınıfı olun **IFilterProvider** arabirimi.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Ekleme bir **IUnityContainer** özelliğinde **FilterProvider** sınıfı ve kapsayıcı atamak için bir sınıf oluşturucu oluşturun.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı Oluşturucusu*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Filtre sağlayıcı sınıfı oluşturucusu değil oluşturma bir **yeni** içinde nesne. Kapsayıcı parametre olarak geçirilir ve bağımlılık Unity tarafından çözüldü.
7. İçinde **FilterProvider** sınıfı, yöntemi uygulaması **GetFilters** gelen **IFilterProvider** arabirimi.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Görev 2 - kaydetme ve filtre etkinleştirme

Bu görevde, site izlemeyi etkinleştirir. Bunu yapmak için filtreye kaydedeceksiniz **Bootstrapper.cs BuildUnityContainer** yöntemi izlemeyi başlatmak için:

1. Açık **Web.config** proje kök ve etkinleştir izleme izleme System.Web Grup adresindeki bulunur.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Açık **Bootstrapper.cs** proje kökündeki.
3. Bir başvuru ekleyin **MvcMusicStore.Filters** ad alanı.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - önyükleyici ad alanlarını ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Seçin **BuildUnityContainer** yöntemi ve kaydetme Unity kapsayıcısında filtre. Eylem filtresi yanı sıra filtresi sağlayıcısını kaydetmek gerekir.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - kayıt FilterProvider ve ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde uygulamayı çalıştırın ve özel eylem filtresi Etkinlik izleme olduğunu sınayın:

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Tıklatın **Rock** türler menü içinde. İsterseniz daha fazla türler göz atabilirsiniz.

    ![Müzik deposu](aspnet-mvc-4-dependency-injection/_static/image11.png "müzik deposu")

    *Müzik Deposu*
3. Gözat **/Trace.axd** uygulama sayfasında ve ardından izleme görmek için **ayrıntıları görüntüle**.

    ![Uygulama izleme günlüğü](aspnet-mvc-4-dependency-injection/_static/image12.png "uygulama izleme günlüğü")

    *Uygulama izleme günlüğü*

    ![Uygulama izleme - istek ayrıntıları](aspnet-mvc-4-dependency-injection/_static/image13.png "uygulama izleme - istek ayrıntıları")

    *Uygulama izleme - istek ayrıntıları*
4. Tarayıcıyı kapatın.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı Laboratuvar tamamlayarak bağımlılık ekleme ASP.NET MVC 4'te bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğrendiniz. Elde etmek için bağımlılık ekleme denetleyicileri, görünümler ve eylem filtrelerini içinde kullandınız.

Aşağıdaki kavramlar kapsanan:

- ASP.NET MVC 4 bağımlılık ekleme özellikleri
- Unity.Mvc3 NuGet paketi kullanarak unity tümleştirme
- Denetleyicileriyle bağımlılık ekleme
- Görünümlerde bağımlılık ekleme
- Eylem filtreleri bağımlılık ekleme

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/? LinkID 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-dependency-injection/_static/image14.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express Web döşemeye*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-dependency-injection/_static/image19.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-dependency-injection/_static/image20.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-dependency-injection/_static/image21.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-dependency-injection/_static/image22.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-dependency-injection/_static/image23.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-dependency-injection/_static/image24.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
