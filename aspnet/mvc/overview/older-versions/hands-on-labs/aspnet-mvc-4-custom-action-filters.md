---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 özel eylem filtreleri | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC önce ya da bir eylem yöntemi çağrıldıktan sonra filtreleme mantığını yürütme için eylem filtrelerini sağlar. Eylem filtreleri özel öznitelikler tha olan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 özel eylem filtreleri

Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC önce ya da bir eylem yöntemi çağrıldıktan sonra filtreleme mantığını yürütme için eylem filtrelerini sağlar. Eylem filtreleri eylem öncesi ve sonrası eylem davranışı denetleyicinin eylem yöntemlerini eklemek için bildirime sağlayan özel öznitelikleridir.

Bu uygulamalı laboratuar ortamında MvcMusicStore çözüm denetleyicinin istekleri yakalamak ve bir site etkinlik bir veritabanı tablosunun içine oturum için bir özel eylem filtresi öznitelik oluşturur. Günlüğe yazma filtresini yerleştirme tarafından herhangi bir denetleyici veya eylem eklemeniz mümkün olacaktır. Son olarak, ziyaretçi listesini gösterir günlük görünümü görürsünüz.

Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**. Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** uygulamalı Laboratuvar.

> [!NOTE]
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresindeki kullanılabilir içinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 özel eylem filtrelerini](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:

- Filtreleme yeteneklerini artırmak için bir özel eylem filtresi öznitelik oluşturun
- Belirli bir düzeye yerleştirme tarafından özel bir filtre özniteliğini uygulayın
- Genel bir özel eylem Filtreleri Kaydet

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

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:

1. [Alıştırma 1: Eylemler günlüğe kaydetme](#Exercise1)
2. [Alıştırma 2: Birden çok eylem filtrelerini yönetme](#Exercise2)

Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**.

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Alıştırma 1: Eylemler günlüğe kaydetme

Bu alıştırmada, ASP.NET MVC 4 filtre sağlayıcıları kullanarak bir özel eylem günlüğü filtresi oluşturma öğreneceksiniz. Bu amaç için günlüğe yazma filtresini tüm etkinlikleri seçili denetleyicileri kaydedilecek MusicStore site uygulanır.

Filtre uzatır **ActionFilterAttributeClass** ve geçersiz kılma **OnActionExecuting** her istek catch ve günlüğe kaydetme eylemleri gerçekleştirmek için yöntem. HTTP istekleri hakkında bağlam bilgisi, yöntemleri, sonuçları ve parametreleri yürütme ASP.NET MVC tarafından sağlanacak **ActionExecutingContext** sınıfı **.**

> [!NOTE]
> ASP.NET MVC 4, özel bir filtre oluşturmadan kullanabileceğiniz varsayılan filtreleri sağlayıcıları da sahiptir. ASP.NET MVC 4 Aşağıdaki filtre türleri sağlar:
> 
> - **Yetkilendirme** filtre, hangi kimlik doğrulaması gerçekleştirmeye veya istek özelliklerini doğrulama gibi bir eylem yönteminin yürütülecek olup olmadığına dair güvenlik kararlarını verir.
> - **Eylem** eylem yöntemi yürütme sarmalar filtre. Bu filtre eylem yöntemine ek verileri sağlayan, dönüş değerini inceleyerek veya eylem yönteminin yürütülmesi iptal etme gibi ek işleme gerçekleştirebilirsiniz
> - **Sonuç** ActionResult nesnesinin yürütme sarmalar filtre. Bu filtre sonucunun HTTP yanıtı değiştirme gibi ek işleme gerçekleştirebilirsiniz.
> - **Özel durum** yere eylem yöntemindeki yetkilendirme filtreleri ile başlayıp sonucu yürütülmesiyle işlenmemiş özel durum ise, yürütür filtre. Özel durum filtreleri, günlük veya bir hata sayfası görüntüleme gibi görevleri için kullanılabilir.
> 
> Filtre sağlayıcıları hakkında daha fazla bilgi için bu MSDN bağlantıyı ziyaret edin: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>MVC müzik deposu uygulama günlüğü özelliği hakkında

Bu müzik deposu çözüm site günlüğü için yeni bir veri modeli tablosu sahip **ActionLog**, aşağıdaki alanlarla: bir istek, aranan eylemini, istemci IP'si ve zaman damgası alınan denetleyicinin adı.

![Veri modeli. ActionLog tablo. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Veri modeli. ActionLog tablo.")

*Veri modeli - ActionLog tablosu*

Konumunda bulunabilir eylem günlüğü için bir ASP.NET MVC görünümü bir çözüm sunar **görünümler/MvcMusicStores/ActionLog**:

![Eylem günlüğü görünümü](aspnet-mvc-4-custom-action-filters/_static/image2.png "eylem günlüğü görüntüle")

*Eylem Günlüğü Görüntüle*

Denetleyicinin isteği kesintiye ve özel filtreleme kullanarak günlüğe kaydetme gerçekleştirme bu yapısı verilen, tüm iş odaklanmış.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Görev 1 - bir denetleyicinin isteği yakalamak için özel bir filtresi oluşturma

Bu görevde günlük kaydı mantığı içeren bir özel filtre öznitelik sınıfı oluşturur. Bu amaç için ASP.NET MVC uzatır **ActionFilterAttribute** sınıfı ve uygulama arabirimi **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** tüm öznitelik filtreleri için temel sınıftır. Belirli bir mantığı sonra ve denetleyici eylemin yürütme önce yürütmek için aşağıdaki yöntemleri sağlar:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.
> - **OnActionExecuted**(ActionExecutedContext filterContext): eylem yöntemi çağrıldıktan sonra ve sonucu (Görünüm oluşturmadan önce) yürütülmeden önce.
> - **OnResultExecuting**(ResultExecutingContext filterContext): (Görünüm oluşturmadan önce) sonucu yürütülmeden önce yalnızca.
> - **OnResultExecuted**(ResultExecutedContext filterContext): (Görünüm işlenen sonra) sonucu yürütüldükten sonra.
> 
> Bu yöntemlerin herhangi biriyle türetilmiş bir sınıf geçersiz kılarak, filtreleme kodunuzu yürütebilir.


1. Açık **başlamak** çözüm bulunan **\Source\Ex01-LoggingActions\Begin** klasör.

   1. Devam etmeden önce bazı eksik NuGet paketlerini karşıdan yüklemeniz gerekir. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
      > 
      > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Yeni bir C# sınıfına ekleme **filtreleri** klasörü ve adlandırın *CustomActionFilter.cs*. Bu klasör, tüm özel filtreler depolar.
3. Açık **CustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanları:

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Devralma **CustomActionFilter** sınıfıyla **ActionFilterAttribute** yapın **CustomActionFilter** sınıfı uygulama **IActionFilter** arabirimi.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Olun **CustomActionFilter** sınıfı yöntemi geçersiz kılma **OnActionExecuting** ve filtrenin yürütme oturum için gerekli mantığı ekleyin. Bunu yapmak için aşağıdaki vurgulanmış kodu içinde ekleyin **CustomActionFilter** sınıfı.

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** yöntemi kullanarak **Entity Framework** yeni bir ActionLog kayıt eklemek için. Oluşturur ve yeni bir varlık örneğinin bağlamı bilgilerle doldurur **filterContext**.
    > 
    > Daha fazla bilgi edinebilirsiniz **ControllerContext** adresindeki sınıf [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Görev 2 - bir kod dinleyiciyi depolama denetleyicisi sınıfına Injecting

Bu görevde tüm denetleyicisi sınıfları ve kaydedilir denetleyici eylemleri ekleyerek özel filtre ekleyeceksiniz. Bu alıştırmada amacıyla depolama denetleyicisi sınıfı bir günlük sahip olur.

Yöntem **OnActionExecuting** gelen **ActionLogFilterAttribute** özel filtre eklenen öğenin çağrıldığında çalışır.

Belirli denetleyici yöntemi müdahale mümkündür.

1. Açık **StoreController** adresindeki **MvcMusicStore\Controllers** ve bir başvuru ekleyin **filtreleri** ad alanı:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Özel filtre ekleme **CustomActionFilter** içine **StoreController** ekleyerek sınıfı **[CustomActionFilter]** sınıf bildiriminin önce öznitelik.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Bir filtre denetleyici sınıfına eklenen, tüm eylemleri de eklenmiş. Yalnızca bir dizi eylemi için filtre uygulamak istiyorsanız, eklemesine olurdu **[CustomActionFilter]** her biri için:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde, günlüğe yazma filtresini çalışıp çalışmadığını test edeceğiz. Uygulamayı başlatın ve mağazası ziyaret edebilmesi ve günlüğe kaydedilmiş etkinliklerin kontrol eder.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için:

    ![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image3.png "oturum sayfa etkinliği önce İzleyici durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*

   > [!NOTE]
   > Varsayılan olarak, var olan türler menüsü alınırken oluşturulan bir öğe her zaman gösterir.
   > 
   > Kolaylık olması amacıyla şu Temizleme **ActionLog** tablo uygulamanın yalnızca belirli her görevin doğrulama günlükleri gösterir şekilde her çalıştığında.
   > 
   > Aşağıdaki kod kaldırmanız gerekebilir **oturum\_Başlat** yöntemi (içinde **Global.asax** sınıfı) deposu içinde yürütülen tüm eylemler için geçmişe dönük bir günlüğünü kaydetmek için Denetleyici.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
4. Gözat **/ActionLog** ve günlük boş tuşuna ise **F5** sayfayı yenilemek için. Ziyaretleriniz izlenmekte olan denetleyin:

    ![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image4.png "oturum aktivitesiyle eylem günlüğü")

    *Eylem günlüğü ile kaydedilen etkinliğin*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Alıştırma 2: Birden çok eylem filtrelerini yönetme

Bu alıştırmada, ikinci bir özel eylem filtre StoreController sınıfına ekleyin ve hem filtreleri yürütülecek belirli bir sıraya tanımlayın. Sonra genel filtre kaydetmek için kodu güncelleştirir.

Filtreler yürütme sırası tanımlarken dikkate farklı seçenekler vardır. Örneğin, sipariş özelliği ve filtreleri kapsamı:

Tanımlayabileceğiniz bir **kapsam** filtrelerin her biri için örneğin, içinde çalıştırmak için tüm eylem filtrelerini kapsamını **denetleyicisi kapsamı**ve çalıştırmak için tüm yetkilendirme filtrelerini **genel kapsamlı** . Tanımlı yürütme sırası kapsamlar.

Ayrıca, her eylem filtresi filtre kapsamı yürütme sırasında belirlemek için kullanılan bir sipariş özelliğine sahiptir.

Özel eylem filtrelerini yürütme sırası hakkında daha fazla bilgi için lütfen bu MSDN makalesine bakın: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Görev 1: yeni bir özel eylem filtresi oluşturma

Bu görevde, filtrelerin uygulanma sırası yönetmek öğrenme StoreController sınıfına eklemesine yeni bir özel eylem filtresi oluşturur.

1. Açık **başlamak** çözüm bulunan **\Source\Ex02-ManagingMultipleActionFilters\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

        > [!NOTE]
        > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
        > 
        > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Yeni bir C# sınıfına ekleme **filtreleri** klasörü ve adlandırın *MyNewCustomActionFilter.cs*
3. Açık **MyNewCustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanı:

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Varsayılan sınıf bildirimi aşağıdaki kodla değiştirin.

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Bu özel eylem filtresi neredeyse daha önceki alıştırmada oluşturulan aynıdır. Ana farktır olduğunu *&quot;oturum tarafından&quot;* öznitelik wich filtresi tanımlamak için bu yeni sınıf adıyla güncelleştirildiğinde günlük kayıtlı.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Görev 2: yeni bir kod dinleyiciyi StoreController sınıfına Injecting.

Bu görevde, yeni bir özel filtre StoreController sınıfına ekleyin ve nasıl hem filtreleri birlikte çalışmasını doğrulamak için çözümü çalıştırın.

1. Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** ve yeni özel filtre ekleme **MyNewCustomActionFilter** içine  **StoreController** gibi sınıfına aşağıdaki kodda gösterilir.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Şimdi, bu iki özel eylem filtrelerini nasıl çalıştığını görmek için uygulamayı çalıştırın. Bunu yapmak için basın **F5** ve uygulama başlatana kadar bekleyin.
3. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.

    ![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image5.png "oturum sayfa etkinliği önce İzleyici durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*
4. Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
5. Bu süre denetleyin; ziyaretleriniz iki kez izlenmekte olan: her özel eylem filtreleri için de eklendikten sonra **StorageController** sınıfı.

    ![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image6.png "oturum aktivitesiyle eylem günlüğü")

    *Eylem günlüğü ile kaydedilen etkinliğin*
6. Tarayıcıyı kapatın.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Görev 3: Filtresi sıralama yönetme

Bu görevde, sipariş verilir kullanarak filtreleri yürütme sırası yönetmeyi öğreneceksiniz.

1. Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** ve belirtin **sipariş** hem filtreleri özelliğinde ister aşağıda gösterilen.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Şimdi, filtreleri, sipariş özellik değerine bağlı olarak nasıl yürütüldüğünden emin olun. En küçük sıra değeri filtresiyle bulabilirsiniz (**CustomActionFilter**) yürütülen ilk sağlayıcıdır. Tuşuna **F5** ve uygulama başlatana kadar bekleyin.
3. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.

    ![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image7.png "oturum sayfa etkinliği önce İzleyici durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*
4. Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
5. Bu süre, ziyaretleriniz izlenmekte olan onay filtreleri sipariş değere göre sıralı: **CustomActionFilter** günlükleri ilk.

    ![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image8.png "oturum aktivitesiyle eylem günlüğü")

    *Eylem günlüğü ile kaydedilen etkinliğin*
6. Şimdi, filtreleri sipariş değeri güncelleştirin ve günlük sırası nasıl değiştiğini doğrulayın. İçinde **StoreController** sınıfı, aşağıda gösterildiği gibi filtreler sıra değeri güncelleştirin.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Tuşlarına basarak uygulamayı yeniden çalıştırın **F5**.
8. Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
9. Bu süre, günlükleri tarafından oluşturulan denetleyin **MyNewCustomActionFilter** filtre ilk görünür.

    ![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image9.png "oturum aktivitesiyle eylem günlüğü")

    *Eylem günlüğü ile kaydedilen etkinliğin*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Görev 4: Kaydetme genel filtreler

Bu görevde, yeni filtre kaydetmek için çözüm güncelleştirmek (**MyNewCustomActionFilter**) genel filtre olarak. Bunu yaparak, bu uygulamada ve yalnızca önceki görev olduğu gibi olanları StoreController ilgili tüm eylemleri perfomed tarafından tetiklenir.

1. İçinde **StoreController** sınıfı, Kaldır **[MyNewCustomActionFilter]** özniteliği ve sipariş özelliğinden **[CustomActionFilter]**. Aşağıdaki gibi görünmelidir:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Açık **Global.asax** dosya ve bulun **uygulama\_Başlat** yöntemi. Her uygulama başlar, bildirim kaydeden genel filtreleri çağırarak **RegisterGlobalFilters** yöntemi içinde **FilterConfig** sınıfı.

    ![Genel filtrelerin Global.asax dosyasında kaydetme](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax dosyasında genel filtreleri kaydetme")

    *Genel filtrelerin Global.asax dosyasında kaydetme*
3. Açık **FilterConfig.cs** içinde dosya **uygulama\_Başlat** klasör.
4. System.Web.Mvc kullanarak bir başvuru ekleyin; MvcMusicStore.Filters kullanarak; ad alanı.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Güncelleştirme **RegisterGlobalFilters** özel filtre ekleme yöntemi. Bunu yapmak için vurgulanmış kodu ekleyin:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Tuşlarına basarak uygulamayı çalıştırın **F5**.
7. Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
8. Onay artık **[MyNewCustomActionFilter]** HomeController ve ActionLogController çok eklenmiş.

    ![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image11.png "oturum aktivitesiyle eylem günlüğü")

    *Genel etkinlik oturum eylem günlüğü*

> [!NOTE]
> Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı Laboratuvar tamamlayarak özel eylemleri yürütmek için bir eylem filtresi genişletmek nasıl öğrendiniz. Ayrıca, sayfa denetleyicilerine herhangi bir filtre eklemesine öğrendiniz. Aşağıdaki kavramlar kullanılıyordu:

- Özel eylem filtreleri ile ASP.NET MVC ActionFilterAttribute sınıfındaki oluşturma
- ASP.NET MVC denetleyicileri filtreleri eklemesine nasıl
- Filtre sırası özelliğini kullanarak sıralama yönetme
- Filtreleri genel olarak kaydetme

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-custom-action-filters/_static/image12.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express Web döşemeye*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure portalında oturum açtığı](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açtığı*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image18.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image19.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-custom-action-filters/_static/image20.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](aspnet-mvc-4-custom-action-filters/_static/image21.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-custom-action-filters/_static/image22.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-custom-action-filters/_static/image23.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-custom-action-filters/_static/image24.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Değişiklikleri onaylamak*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-custom-action-filters/_static/image29.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-custom-action-filters/_static/image30.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-custom-action-filters/_static/image31.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
   - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
   - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
   - Yeni bir veritabanı adı yazın.

     ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image33.png "hedef bağlantı dizesi yapılandırma")

     *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-custom-action-filters/_static/image34.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-custom-action-filters/_static/image36.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-custom-action-filters/_static/image37.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-custom-action-filters/_static/image38.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-custom-action-filters/_static/image39.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-custom-action-filters/_static/image40.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-custom-action-filters/_static/image41.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-custom-action-filters/_static/image42.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
