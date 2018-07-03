---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 bağımlılık ekleme | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı Laboratuvar, ASP.NET MVC ve ASP.NET MVC 4 filtreleri temel bilgiye sahip varsayar. ASP.NET MVC 4 filtrelerden önce biz rec kullanmadıysanız...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 715444a6fbf491d7b99918294cfd2d0d0216cd09
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388050"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 bağımlılık ekleme

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC** ve **ASP.NET MVC 4 filtreler**. Kullanmadıysanız, **ASP.NET MVC 4 filtreler** gitmenizi öneririz önce **ASP.NET MVC özel eylem filtreleri** Hands-on-Lab.

> [!NOTE]
> Web Kampları eğitim Seti'nde bulunan, tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 bağımlılık ekleme](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

İçinde **nesne yönelimli programlama** paradigm, nesneleri birlikte çalışır bir işbirliği modelinde katkıda bulunanların ve tüketiciler olduğu. Doğal olarak, bu iletişim modelini nesneleri ve karmaşıklık artar yönetmek zor hale bileşenleri arasındaki bağımlılıkları oluşturur.

![Sınıf bağımlılıkları ve model karmaşıklığı](aspnet-mvc-4-dependency-injection/_static/image1.png "sınıfı bağımlılıkları ve model karmaşıklığı")

*Sınıf bağımlılıkları ve model karmaşıklığı*

Büyük olasılıkla hakkında duymuş **Fabrika deseni** ve arabirim ve istemci nesneler genellikle hizmet konumu için sorumlu olduğu hizmetleri kullanılarak uygulama arasındaki ayrımı.

Bağımlılık ekleme, ters çevirmeyi denetimin belirli bir uygulama modelidir. **Tersine çevirme (IOC) denetiminin** nesneleri bağlı oldukları işlerini yapmak için diğer nesnelerin oluşturmayın anlamına gelir. Bunun yerine bir dış kaynaktan (örneğin, bir xml yapılandırma dosyası) ihtiyaç duydukları nesneleri alın.

**Bağımlılık ekleme (dı)** bu nesne müdahalesi olmadan genellikle Oluşturucusu parametrelerini geçirir framework bileşeni tarafından yapılır ve özelliklerini ayarlama, anlamına gelir.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Bağımlılık ekleme (dı) tasarım deseni

Yüksek bir düzeyde bağımlılık ekleme amacı olan bir istemci sınıfı (örn *golfçü*) arabirim karşılayan bir şey gerekir (örn *IClub*). Somut türünü nedir önemli değildir (örn *WoodClub, IronClub, WedgeClub* veya *PutterClub*), işleyen başka birisi istediği (örneğin iyi *caddy*). ASP.NET mvc'de bağımlılık çözümleyiciyi bağımlılık mantığınızı başka bir yerde kaydetmenizi izin verebilirsiniz (örneğin bir kapsayıcı veya bir *kulüpleri paketi*).

![Bağımlılık ekleme diyagramı](aspnet-mvc-4-dependency-injection/_static/image2.png "bağımlılık ekleme çizim")

*Bağımlılık ekleme - Golf benzerleme*

Bağımlılık ekleme desenini ve tersine çevirme denetim kullanmanın avantajları şunlardır:

- Sınıf bağlantısı azaltır
- Kodu yeniden kullanma artırır
- Kod bakımı artırır
- Uygulamayı test artırır

> [!NOTE]
> Bağımlılık ekleme bazen soyut Fabrika tasarım deseni ile karşılaştırıldığında, ancak her iki yaklaşım arasında küçük bir fark yoktur. DI fabrikaları ve kaydedilen Hizmetleri çağırarak bağımlılıkları çözmeye arkasında çalışma bir çerçeve vardır.


Bağımlılık ekleme desenini anladığınıza göre ASP.NET MVC 4'te uygulama bu Laboratuvar öğreneceksiniz. Bağımlılık ekleme kullanılarak başlar **denetleyicileri** Veritabanı Erişim hizmeti eklemek için. Ardından, bağımlılık ekleme için uygulayacağınız **görünümleri** bir hizmeti kullanmak ve bilgi göstermek için. Son olarak, bir özel eylem filtresi çözümdeki ekleme DI ASP.NET MVC 4 filtreleri için genişletilir.

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- ASP.NET MVC 4 bağımlılık ekleme için NuGet paketlerini kullanarak Unity ile tümleştirin
- ASP.NET MVC denetleyicisi içinde kullanım bağımlılık ekleme
- Bir ASP.NET MVC görünümü içinde kullanım bağımlılık ekleme
- Bir ASP.NET MVC eylem filtresi içinde kullanım bağımlılık ekleme

> [!NOTE]
> Bu Laboratuvar için bağımlılık çözümlemesi Unity.Mvc3 NuGet paketi kullanıyor, ancak ASP.NET MVC 4 ile çalışmak için bir bağımlılık ekleme Framework uyum sağlamak mümkündür.


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:

- [Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleniyor**

Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir. Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek B: kullanarak kod parçacıklarına](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:

1. [Alıştırma 1: bir denetleyici ekleme](#Exercise1)
2. [Alıştırma 2: bir görünüm ekleme](#Exercise2)
3. [Alıştırma 3: Filtre ekleme](#Exercise3)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Alıştırma 1: bir denetleyici ekleme

Bu alıştırmada, bir NuGet paketi kullanarak Unity tümleştirerek ASP.NET MVC denetleyicileri bağımlılık ekleme kullanmayı öğreneceksiniz. Bu nedenle, veri erişim mantığı ayırmak için MvcMusicStore denetleyicilere Hizmetleri dahil edilir. Hizmetleri yardımıyla bağımlılık ekleme kullanılarak çözümlenir denetleyici Oluşturucu, yeni bir bağımlılık oluşturacak **Unity**.

Bu yaklaşım, daha esnek ve bakımı ve test etmek daha kolay olan daha az bağlı uygulamalar, oluşturma adımları gösterilmektedir. Aynı zamanda ASP.NET MVC, Unity ile tümleştirmeyi öğreneceksiniz.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>StoreManager hizmeti hakkında

MVC müzik başlangıç çözümde şimdi sağlanan Store adlı Store denetleyicisi verileri yöneten bir hizmet içerir **StoreService**. Aşağıda Store hizmet uygulaması bulabilirsiniz. Tüm yöntemleri modeli varlıklarından dönüş unutmayın.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** begin işlevinden çözümü şimdi tüketir **StoreService**. Tüm veri başvurularını sıradan kaldırıldığını **StoreController**ve geçerli veri erişimi sağlayıcısı kullanan herhangi bir yöntemi değiştirmeden artık mümkün **StoreService**.

Bunun altında bulabilirsiniz **StoreController** uygulaması ile bir bağımlılık içeriyor **StoreService** sınıf oluşturucusu içinde.

> [!NOTE]
> Bu alıştırmada, sunulan bağımlılık ilgili **denetim tersine çevirme** (IOC).
> 
> **StoreController** sınıf oluşturucusunu alır bir **IStoreService** sınıf içinde hizmet çağrılarından gerçekleştirmek için gerekli olan tür parametresi. Ancak, **StoreController** herhangi bir denetleyici, ASP.NET MVC ile çalışmak için gereken varsayılan oluşturucu (parametresiz) uygulamıyor.
> 
> Bağımlılık çözmek için denetleyici soyut bir Fabrika (belirtilen türde herhangi bir nesne döndürür sınıfı) tarafından oluşturulması gerekir.


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Hizmet nesnesi göndermeden StoreController bildirilen parametresiz bir oluşturucusu olmadığından oluşturmak sınıf çalıştığında bir hata alırsınız.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Görev 1 - Uygulamayı çalıştırma

Bu görevde, veri erişimi uygulama mantığından ayıran Store denetleyici hizmeti içerir başlangıç uygulama çalışır.

Denetleyici Hizmeti varsayılan olarak bir parametre olarak geçirilir şekilde uygulama çalışırken, bir özel durum alırsınız:

1. Açık **başlamak** çözüm bulunan **Source\Ex01 ekleme Controller\Begin**.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **Ctrl + F5 tuşlarına basarak** uygulamayı hata ayıklama olmadan çalıştırın. Hata iletisi alırsınız &quot; **bu nesne için tanımlanmış parametresiz bir oluşturucusu**&quot;:

    ![ASP.NET MVC uygulaması başlamak çalıştırılırken hata](aspnet-mvc-4-dependency-injection/_static/image3.png "başlamak ASP.NET MVC uygulaması çalıştırılırken hata")

    *ASP.NET MVC uygulaması başlamak çalıştırılırken hata*
3. Tarayıcıyı kapatın.

Aşağıdaki adımlarda bu denetleyicisi bağımlılık ekleme müzik Store çözüm üzerinde çalışır.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Görev 2 - MvcMusicStore çözümünde dahil olmak üzere Unity

Bu görevde, içerecektir **Unity.Mvc3** çözüm için NuGet paketi.

> [!NOTE]
> Unity.Mvc3 paketi ASP.NET MVC 3 için tasarlanmıştır ancak ASP.NET MVC 4 ile tamamen uyumludur.
> 
> Unity örneği için bir hafif, Genişletilebilir bağımlılık ekleme kapsayıcısını isteğe bağlı destek olduğu ve durdurma yazın. Her tür .NET uygulama kullanmak için genel amaçlı bir kapsayıcıdır. Bağımlılık ekleme mekanizmalar da dahil olmak üzere tüm ortak özellikleri sağlar: nesne oluşturma, kapsayıcıya Bileşen Yapılandırması erteleyerek çalışma zamanı ve esneklik, bağımlılıkları belirterek gereksinimleri soyutlamasıdır.


1. Yükleme **Unity.Mvc3** NuGet paketini **MvcMusicStore** proje. Bunu yapmak için açık **Paket Yöneticisi Konsolu** gelen **görünümü** | **diğer Windows**.
2. Şu komutu çalıştırın.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Unity.Mvc3 NuGet paketini yükleyerek](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet paketi yükleniyor")

    *Unity.Mvc3 NuGet paketi yükleniyor*
3. Bir kez **Unity.Mvc3** paketinin yüklendiğinden, otomatik olarak ekler Unity yapılandırmasını basitleştirmek için klasör ve dosya keşfedin.

    ![Yüklü Unity.Mvc3 paket](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 paket yüklü")

    *Yüklü Unity.Mvc3 paketi*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Görev 3 - kaydetme Unity Global.asax.cs uygulamada\_Başlat

Bu görevde, güncelleştirecektir **uygulama\_Başlat** yöntemi bulunan **Global.asax.cs** Unity önyükleyici Başlatıcı arayın ve sonra önyükleyici dosya kaydetme güncelleştirmek için Denetleyici ve hizmet bağımlılık ekleme için kullanır.

1. Şimdi, Unity kapsayıcı başlatır dosyası olan önyükleyici ve bağımlılık çözümleyiciyi yedekleme kanca. Bunu yapmak için açık **Global.asax.cs** ve içinde aşağıdaki vurgulanmış kodu ekleyin **uygulama\_Başlat** yöntemi.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - Unity başlatmak*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Açık **Bootstrapper.cs** dosya.
3. Aşağıdaki ad alanlarını içerir: **MvcMusicStore.Services** ve **MusicStore.Controllers**.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex01 - Laboratuvar önyükleyici ad alanlarını ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Değiştirin **BuildUnityContainer** yöntemini aşağıdaki kodla Store denetleyici ve Store hizmet kaydeden içerik.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - kayıt Store denetleyici ve hizmet*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, bunu şimdi Unity eklendikten sonra yüklenebileceğini doğrulamak amacıyla uygulamanın çalışır.

1. Tuşuna **F5** uygulamayı çalıştırmak için uygulama artık herhangi bir hata iletisi göstermeden yüklenecektir.

    ![Uygulama bağımlılık ekleme ile çalışan](aspnet-mvc-4-dependency-injection/_static/image6.png "bağımlılık ekleme ile uygulama çalıştırma")

    *Bağımlılık ekleme ile çalışan uygulama*
2. Gözat **/Store**. Bu çağırma **StoreController**, artık oluşturulduğu kullanarak **Unity**.

    ![MVC müzik Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC müzik Store")

    *MVC müzik Store*
3. Tarayıcıyı kapatın.

Aşağıdaki alıştırmalarda ASP.NET MVC görünümleri ve eylem filtrelerini içinde kullanmak için bağımlılık ekleme kapsamını genişletmek öğreneceksiniz.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Alıştırma 2: bir görünüm ekleme

Bu alıştırmada, bağımlılık ekleme ile ASP.NET MVC 4'ün yeni özelliklerini görünümünde Unity tümleştirme için nasıl kullanılacağını öğreneceksiniz. Bunu yapabilmek için Store Gözat bir ileti ve aşağıdaki görüntü gösterilir görünümü içinde özel bir hizmeti çağırır.

Daha sonra projeyi Unity ile tümleştirin ve bağımlılıkları eklenmek üzere bir özel bağımlılık çözümleyiciyi oluşturun.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Görev 1 - bir hizmeti bir görünüm oluşturma

Bu görevde, yeni bir bağımlılık oluşturmak için bir hizmet çağrısı gerçekleştiren bir görünümü oluşturur. Bu çözümde bulunan basit bir Mesajlaşma hizmetinde hizmet oluşur.

1. Açık **başlamak** çözüm bulunan **Source\Ex02 ekleme View\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
      > 
      > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dahil **MessageService.cs** ve **IMessageService.cs** sınıfları bulunan **\Assets kaynak** klasöründe **/Hizmetleri**. Bunu yapmak için sağ **Hizmetleri** klasörü ve select **varolan öğeyi Ekle**. Dosyaların konumuna göz atın ve bunları ekleyebilirsiniz.

    ![İleti hizmeti ve hizmet arabirimi ekleme](aspnet-mvc-4-dependency-injection/_static/image8.png "ileti hizmeti ve hizmet arabirimi ekleme")

    *Ekleme ileti hizmeti ve hizmet arabirimi*

    > [!NOTE]
    > **IMessageService** arabirimi tarafından uygulanan iki özellikler tanımlar **MessageService** sınıfı. Bu özellikleri -**ileti** ve **ImageUrl**-görüntülenecek iletiyi ve görüntünün URL'sini depolayın.
3. Klasör Oluştur **/sayfaları** projenin kök klasörünü ve ardından varolan sınıf Ekle **MyBasePage.cs** gelen **Source\Assets**. Devralınacağı temel sayfa aşağıdaki yapıya sahiptir.

    ![Sayfalar klasöründe](aspnet-mvc-4-dependency-injection/_static/image9.png "sayfaları klasörü")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Açık **Browse.cshtml** görünümüne **/Views/Store** klasöründe ve devralınan hale **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. İçinde **Gözat** görüntülemek için bir çağrı ekleyin **MessageService** görüntü ve hizmet tarafından alınan bir ileti görüntülemek için.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Görev 2 - özel bağımlılık çözümleyiciyi ve özel görünüm sayfa Etkinleştiricisini dahil

Önceki görevde bir hizmet çağrısı içindeki gerçekleştirmek için bir görünüm içindeki yeni bir bağımlılık eklenmiş. ASP.NET MVC bağımlılık ekleme arabirimlerini uygulayarak, bağımlılık şimdi çözülecektir **IViewPageActivator** ve **Idependencyresolver**. Çözümde uygulaması içerecektir **Idependencyresolver** , baş hizmet alma ile Unity kullanarak. Ardından, başka bir özel uygulanışı içerecektir **IViewPageActivator** oluşturulması görünüm çözmek için arabirim.

> [!NOTE]
> ASP.NET MVC 3'ten sonraki bağımlılık ekleme için uygulama Hizmetleri'ne kaydetme arabirimler Basitleştirilmiş. **Idependencyresolver** ve **IViewPageActivator** bağımlılık ekleme için ASP.NET MVC 3 özellikleri parçasıdır.
> 
> **-Idependencyresolver** arabirimi önceki IMvcServiceLocator değiştirir. Uygulayıcılar Idependencyresolver, hizmetin veya hizmet koleksiyonu örneği döndürmelidir.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** arabirimi görünüm sayfalarının bağımlılık ekleme nasıl örneği oluşturulur üzerinde daha ayrıntılı denetim sağlar. Uygulayan sınıflar **IViewPageActivator** arabirimi örnekleri görüntüle bağlam bilgilerini kullanarak oluşturabilirsiniz.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Oluşturma /**fabrikaları** projenin kök klasöründe.
2. Dahil **CustomViewPageActivator.cs** çözümünüzden için **/kaynakları/varlıklar/** için **fabrikaları** klasör. Bunu yapmak için sağ **/Factories** klasörüne **Ekle | Var olan öğe** seçip **CustomViewPageActivator.cs**. Bu sınıfın uyguladığı **IViewPageActivator** Unity kapsayıcı tutmak için arabirim.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** Unity kapsayıcısını kullanarak bir görünüm oluşturma yönetmekten sorumludur.
3. Dahil **UnityDependencyResolver.cs** dosya **/kaynakları/varlıklar** için **/Factories** klasör. Bunu yapmak için sağ **/Factories** klasörüne **Ekle | Var olan öğe** seçip **UnityDependencyResolver.cs** dosya.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** Unity için özel bir DependencyResolver sınıftır. Bir hizmet içinde Unity kapsayıcı bulunamıyor, temel çözümleyici invocated.

Aşağıdaki görev hem de uygulamaları, hizmetleri ve görünümleri konumunu bilmeniz modeli izin vermek için kaydedilir.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Görev 3 - için bağımlılık ekleme kapsayıcısına Unity kaydediliyor

Bu görevde, birlikte çalışma bağımlılık ekleme yapmak için önceki her şeyi koyacaktır.

Şimdiye kadar çözümünüzün aşağıdaki öğelere sahiptir:

- A **Gözat** devralınan görünümü **MyBaseClass** ve **MessageService**.
- Bir ara sınıfı -**MyBaseClass**-bağımlılık ekleme için hizmet arabirimi bildirilen sahiptir.
- Hizmeti - **MessageService** - ve arabirimiyle **IMessageService**.
- Unity için-özel bağımlılık çözümleyici **UnityDependencyResolver** -hizmet alma ile ilgilidir.
- Bir görünüm sayfa etkinleştiricisini - **CustomViewPageActivator** -sayfası oluşturur.

Eklemesine **Gözat** görünümü artık kayıt özel bağımlılık çözümleyiciyi Unity kapsayıcısında.

1. Açık **Bootstrapper.cs** dosya.
2. Örneğini kaydetmek **MessageService** hizmeti başlatmak için Unity kapsayıcıya alın:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt iletisi hizmeti*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Bir başvuru ekleyin **MvcMusicStore.Factories** ad alanı.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex02 - Laboratuvar fabrikaları Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Kayıt **CustomViewPageActivator** Unity kapsayıcı içine bir görünüm sayfa etkinleştiricisini olarak:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. ASP.NET MVC 4 varsayılan bağımlılık çözümleyici örneği ile değiştirin **UnityDependencyResolver**. Bunu yapmak için değiştirin **Initialise** yöntemini aşağıdaki kodla içerik:

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - güncelleştirme bağımlılık çözümleyiciyi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC varsayılan bağımlılık çözümleyici sınıfı sağlar. Unity için oluşturduk biri olarak özel bağımlılık çözümleyiciler ile çalışmak için bu çözümleyici değiştirilmesi gerekir.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, Store Tarayıcı hizmeti ve görüntü ve alınan ileti gösterilir doğrulamak amacıyla uygulamanın çalışır:

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Tıklayın **Rock** görmek türler menü ve içinde nasıl **MessageService** Hoş Geldiniz iletisi ve görüntü yüklendi ve görünüm eklendi. Bu örnekte biz için giriyorsunuz &quot; **Rock**&quot;:

    ![MVC müzik Store - görünümü ekleme](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC müzik Store - görünümü ekleme")

    *MVC müzik Store - görünümü ekleme*
3. Tarayıcıyı kapatın.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Alıştırma 3: Eylem filtrelerini ekleme

Önceki uygulamalı laboratuvarda **özel eylem filtreleri** filtreleri özelleştirme ve ekleme çalıştı. Bu alıştırmada, Unity kapsayıcısını kullanarak bağımlılık ekleme ile filtre eklemesine öğreneceksiniz. Bunu yapmak için site etkinliklerini izler bir özel eylem filtresi müzik Store çözüme ekleyeceksiniz.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Görev 1 - izleme filtresi dahil

Bu görevde, bir özel eylem filtresi olayları izlemek müzik Store içerecektir. Özel eylem filtresi olarak kavramları zaten önceki laboratuvarda değerlendirilir &quot;özel eylem filtreleri&quot;, yalnızca bu Laboratuvar varlıklarını klasöründen filtre sınıfının içerir ve Unity için filtre sağlayıcı oluşturup:

1. Açık **başlamak** çözüm bulunan **Source\Ex03 - ekleme eylemi Filter\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
      > 
      > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dahil **TraceActionFilter.cs** dosya **/kaynakları/varlıklar** için **/filtreler** klasör.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Bu özel eylem filtresi ASP.NET izleme gerçekleştirir. Denetleyebilirsiniz &quot;ASP.NET MVC 4 yerel ve dinamik eylem filtrelerini&quot; Laboratuvar için daha fazla başvuru.
3. Boş sınıf ekleme **FilterProvider.cs** klasöründe projeye   **/filtreler.**
4. Ekleme **System.Web.Mvc** ve **Microsoft.Practices.Unity** ad alanlarında **FilterProvider.cs**.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı ad alanları ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Devralınan sınıf olun **IFilterProvider** arabirimi.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Ekleme bir **IUnityContainer** özelliğinde **FilterProvider** sınıfı ve kapsayıcı atamak için bir sınıf oluşturucusuna oluşturun.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcısı Oluşturucusu*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Filtre sağlayıcısı sınıf oluşturucusu olmayan oluşturma bir **yeni** iç nesne. Kapsayıcı, bir parametre olarak geçirilir ve bağımlılık Unity tarafından çözülür.
7. İçinde **FilterProvider** yöntemi uygulamak, sınıf **GetFilters** gelen **IFilterProvider** arabirimi.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcısı GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Görev 2 - kaydetme ve filtre etkinleştirme

Bu görevde, site izlemeyi etkinleştirir. Bunu yapmak için filtreye kaydolacak **Bootstrapper.cs BuildUnityContainer** yöntemi izlemeyi başlatmak için:

1. Açık **Web.config** etkinleştir izleme izleme System.Web grubu ve proje kök bulunur.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Açık **Bootstrapper.cs** proje kökündeki.
3. Bir başvuru ekleyin **MvcMusicStore.Filters** ad alanı.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex03 - Laboratuvar önyükleyici ad alanlarını ekleme*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Seçin **BuildUnityContainer** yöntemi ve kayıt Unity kapsayıcısında filtre. Eylem filtresi yanı sıra filtresi sağlayıcıyı kaydetmek gerekir.

    (Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - kayıt FilterProvider ve ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, uygulamayı çalıştırmak ve özel eylem filtresi Etkinlik izleme, test edin:

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Tıklayın **Rock** içindeki türleri. İsterseniz daha fazla oyun türü için göz atabilirsiniz.

    ![Müzik Store](aspnet-mvc-4-dependency-injection/_static/image11.png "müzik Store")

    *Müzik Deposu*
3. Gözat **/Trace.axd** uygulama sayfasını ve ardından izlemek için **ayrıntıları**.

    ![Uygulama izleme günlüğü](aspnet-mvc-4-dependency-injection/_static/image12.png "uygulama izleme günlüğü")

    *Uygulama izleme günlüğü*

    ![Uygulama izleme - istek ayrıntılarını](aspnet-mvc-4-dependency-injection/_static/image13.png "uygulama izleme - istek ayrıntıları")

    *Uygulama izleme - istek ayrıntıları*
4. Tarayıcıyı kapatın.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarı tamamlayarak bağımlılık ekleme ASP.NET MVC 4'te bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğrendiniz. Bunu başarmak için bağımlılık ekleme denetleyicileri, görünümler ve eylem filtrelerini içinde kullandınız.

Aşağıdaki kavramlar kapsanan:

- ASP.NET MVC 4 bağımlılık ekleme özellikleri
- Unity.Mvc3 NuGet paketi kullanarak unity tümleştirmesi
- Denetleyicileri bağımlılık ekleme
- Görünümlere bağımlılık ekleme
- Eylem filtreleri bağımlılık ekleme

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Web kutucuğu için VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-dependency-injection/_static/image19.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-dependency-injection/_static/image20.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-dependency-injection/_static/image21.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-dependency-injection/_static/image22.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-dependency-injection/_static/image23.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-dependency-injection/_static/image24.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
