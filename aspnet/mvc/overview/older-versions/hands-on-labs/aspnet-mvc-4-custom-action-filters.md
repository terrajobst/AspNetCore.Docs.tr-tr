---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 özel eylem filtreleri | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC filtreleme mantığı önce ya da bir eylem yöntemi çağrıldıktan sonra yürütmeye yönelik eylem filtreleri sağlar. Eylem, özel öznitelikler tha filtrelerdir...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753391"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 özel eylem filtreleri

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC filtreleme mantığı önce ya da bir eylem yöntemi çağrıldıktan sonra yürütmeye yönelik eylem filtreleri sağlar. Eylem filtreleri eylem öncesi ve sonrası eylem davranışı için denetleyicinin eylem yöntemlerini eklemek için bildirime sağlayan özel öznitelikleridir.

Bu uygulamalı laboratuvarda bir özel eylem filtresi özniteliği denetleyicisinin istekleri catch ve bir sitenin veritabanı tablosuna etkinlik günlük MvcMusicStore çözüm oluşturur. Herhangi bir denetleyici veya eylem günlüğü filtrenizle eklenmesiyle eklemek mümkün olacaktır. Son olarak, ziyaretçi listesini gösteren günlük görünümüne görürsünüz.

Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC**. Kullanmadıysanız, **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** Hands-on-Lab.

> [!NOTE]
> Web Kampları eğitim Seti'nde bulunan, tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 özel eylem filtreleri](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Filtreleme yetenekleri genişletmek için bir özel eylem filtresi özniteliği oluşturma
- Belirli bir düzeye ekleme tarafından özel bir filtre özniteliğini uygulayın.
- Genel olarak özel eylem Filtreleri Kaydet

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

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek C: kullanarak kod parçacıklarına](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:

1. [Alıştırma 1: eylem günlüğü](#Exercise1)
2. [Alıştırma 2: Birden çok eylem filtreleri yönetme](#Exercise2)

Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**.

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Alıştırma 1: Eylem günlüğü

Bu alıştırmada, ASP.NET MVC 4 filtre sağlayıcıları kullanarak bir özel eylem günlüğü filtresi oluşturulacağını öğreneceksiniz. Bu amaç için tüm etkinlikleri seçili denetleyicileri kaydedilecek MusicStore site günlük filtre uygulanır.

Filtre genişletecek **ActionFilterAttributeClass** ve geçersiz kılma **OnActionExecuting** her isteğin yakalayın ve günlüğe kaydetme işlemleri yöntemi. HTTP istekleri hakkında bağlam bilgisi, yöntemleri, sonuçları ve parametreleri yürütme ASP.NET MVC tarafından sağlanacak **ActionExecutingContext** sınıfı **.**

> [!NOTE]
> ASP.NET MVC 4, özel bir filtre oluşturmak zorunda kalmadan kullanabileceğiniz varsayılan filtreler sağlayıcıları de vardır. ASP.NET MVC 4 şu filtre türlerini sağlar:
> 
> - **Yetkilendirme** filtre, hangi kimlik doğrulaması gerçekleştirme veya istek özelliklerini doğrulama gibi bir eylem yönteminin yürütme konusunda güvenlik kararları.
> - **Eylem** filtresi, yürütülen eylem yöntemini sarmalar. Bu filtre, eylem yöntemi için ek veri sağlayan, dönüş değeri inceleyerek veya eylem yönteminin yürütmeyi iptal eden gibi ek işleme gerçekleştirebilirsiniz
> - **Sonuç** ActionResult nesnesinin yürütme sarmalar Filtresi. Bu filtre, HTTP yanıtı değiştirme gibi bu sonucun ek işlem gerçekleştirebilirsiniz.
> - **Özel durum** yere eylem yöntemindeki yetkilendirme filtreleri ile başlayıp sonucu yürütülmesiyle işlenmemiş özel durum ise, yürütür filtre. Özel durum filtreleri, oturum veya bir hata sayfası görüntüleme gibi görevleri için kullanılabilir.
> 
> Filtre sağlayıcıları hakkında daha fazla bilgi için şu MSDN bağlantısına ziyaret edin: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>MVC müzik Store uygulaması günlüğe kaydetme özelliği hakkında

Bu müzik Store çözüm site günlük kaydı için yeni bir veri modeli tablosuna sahip **ActionLog**, aşağıdaki alanlarla: bir istek, aranan eylemini, istemci IP'si ve zaman damgası alınan denetleyicinin adı.

![Veri modeli. ActionLog tablo. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Veri modeli. ActionLog tablo.")

*Veri modeli - ActionLog tablo*

Şurada bulunabilir eylem günlüğü için bir ASP.NET MVC görünümü bir çözüm sunar **MvcMusicStores/görünümler/ActionLog**:

![Eylem günlüğü görünümü](aspnet-mvc-4-custom-action-filters/_static/image2.png "eylem günlüğü görüntüle")

*Eylem Günlüğü Görüntüle*

Denetleyicinin isteği kesintiye ve özel filtreleme'ı kullanarak günlüğe kaydetme işlemi gerçekleştirmeye bu yapısı verilen, tüm iş odaklanmış.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Görev 1 - bir denetleyicinin isteği yakalamak için özel bir filtre oluşturma

Bu görevde, günlük kaydı mantığı içeren bir özel filtre öznitelik sınıfı oluşturacaksınız. Bu amaç için ASP.NET MVC genişletecek **ActionFilterAttribute** sınıf ve arabirim uygulama **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** tüm öznitelik filtreleri için temel sınıftır. Bu, belirli bir mantıksal sonra ve denetleyici eylem yürütme önce yürütmek için aşağıdaki yöntemleri sağlar:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): yöntem eylem hemen önce çağrılır.
> - **OnActionExecuted**(ActionExecutedContext filterContext): eylem yöntemi çağrıldıktan sonra ve (Görünüm oluşturmadan önce) sonuç yürütülmeden önce.
> - **OnResultExecuting**(ResultExecutingContext filterContext): (Görünüm oluşturmadan önce) sonuç yürütülmeden önce yeterlidir.
> - **OnResultExecuted**(ResultExecutedContext filterContext): (Görünüm işlenen sonra), sonuç yürütüldükten sonra.
> 
> Bu yöntemlerin herhangi biriyle türetilmiş bir sınıf içinde geçersiz kılarak, filtreleme kodunuzu çalıştırabilirsiniz.


1. Açık **başlamak** çözüm bulunan **\Source\Ex01-LoggingActions\Begin** klasör.

   1. Devam etmeden önce bazı eksik NuGet paketleri indirmeniz gerekir. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
      > 
      > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Yeni bir C# sınıfına ekleme **filtreleri** klasörünü adlandırın *CustomActionFilter.cs*. Bu klasör, tüm özel filtreler depolar.
3. Açık **CustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanları:

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Devralınan **CustomActionFilter** gelen sınıfı **ActionFilterAttribute** ve ardından **CustomActionFilter** sınıfı uygulama **IActionFilter** arabirimi.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Olun **CustomActionFilter** sınıfı yöntemi geçersiz kılma **OnActionExecuting** ve filtrenin yürütme açmak için gerekli mantığı ekleyin. Bunu yapmak için aşağıdaki vurgulanmış kodu ekleyin. **CustomActionFilter** sınıfı.

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** yöntemi kullanarak **Entity Framework** ActionLog yeni bir kayıt eklemek için. Oluşturur ve yeni bir varlık örneğinin bağlamı bilgilerle doldurur **filterContext**.
    > 
    > Daha fazla bilgi edinebilirsiniz **ControllerContext** , sınıf [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Görev 2 - kod dinleyiciyi Store denetleyici sınıfına ekleme

Bu görevde tüm denetleyici sınıflarına ve günlüğe kaydedilir denetleyici eylemleri için ekleyerek özel filtre ekleyeceksiniz. Bu alıştırma için bir günlük Store denetleyici sınıfı sahip olur.

Yöntem **OnActionExecuting** gelen **ActionLogFilterAttribute** özel filtre, eklenen öğe çağrıldığında çalıştırır.

Belirli bir denetleyici yöntemi ele alınması mümkündür.

1. Açık **StoreController** adresindeki **MvcMusicStore\Controllers** ve bir başvuru ekleyin **filtreleri** ad alanı:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Özel filtre ekleme **CustomActionFilter** içine **StoreController** ekleyerek sınıfı **[CustomActionFilter]** özniteliği sınıf bildiriminden önce.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Bir filtre denetleyici sınıfına eklenmiş durumda olduğunda, tüm eylemleri de eklenmiş. Yalnızca bir eylemler kümesi için filtre uygulamak isterseniz ekleme gerekirdi **[CustomActionFilter]** her biri için:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, günlüğe kaydetme filtre çalışıp çalışmadığını test edeceksiniz. Uygulama başlatma ve mağazasını ziyaret ve sonra günlüğe kaydedilen etkinlikler denetler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için:

    ![Oturum, İzleyici durumu sayfası etkinlik önce](aspnet-mvc-4-custom-action-filters/_static/image3.png "oturum sayfa etkinliği önce İzleyicisi durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*

   > [!NOTE]
   > Varsayılan olarak, bu her zaman varolan türleri menüsü alınırken oluşturulan bir öğeyi gösterir.
   > 
   > Kolaylık olması amacıyla şu Temizleme **ActionLog** tablo her zaman sadece belirli her görevin doğrulama günlükleri gösterir böylece uygulama çalışır.
   > 
   > Aşağıdaki kod kaldırmanız gerekebilir **oturumu\_Başlat** yöntemi (içinde **Global.asax** sınıfı), Store içinde yürütülen tüm eylemler için bir geçmiş günlüğe kaydetmek için Denetleyici.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Birini **türleri** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
4. Gözat **/ActionLog** ve günlük boş tuşuna **F5** sayfayı yenilemek için. Ziyaretleriniz izlenen denetleyin:

    ![Etkinlik günlüğe eylem günlüğü](aspnet-mvc-4-custom-action-filters/_static/image4.png "günlüğe etkinlik eylem günlüğü")

    *Etkinlik günlüğe eylem günlüğü*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Alıştırma 2: Birden çok eylem filtreleri yönetme

Bu alıştırmada, ikinci bir özel eylem filtresi StoreController sınıfa eklemek ve hem filtreleri yürütüleceği belirli düzenini tanımlamak. Ardından genel filtre kaydetmek için kodu güncelleştirir.

Filtreler yürütme sırası tanımlarken dikkate almanız için farklı seçenekler vardır. Örneğin, sipariş özelliği ve filtreler kapsamı:

Tanımlayabileceğiniz bir **kapsam** her filtre gibi içinde çalıştırmak için tüm eylem filtrelerini kapsamında çalışabilirsiniz **denetleyicisi kapsamı**ve çalıştırmak için tüm yetkilendirme filtrelerini **genel kapsam** . Kapsamlar tanımlı yürütme sırası.

Ayrıca, her bir eylem filtresi filtre kapsamında yürütme sırasını belirlemek için kullanılan bir sipariş özelliğine sahiptir.

Özel eylem filtreleri yürütme sırası hakkında daha fazla bilgi için lütfen bu MSDN makalesine bakın: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Görev 1: yeni bir özel eylem filtresi oluşturma

Bu görevde, filtrelerin uygulanma sırası yönetmek öğrenme StoreController sınıfı eklenmek üzere yeni bir özel eylem filtresi oluşturur.

1. Açık **başlamak** çözüm bulunan **\Source\Ex02-ManagingMultipleActionFilters\Begin** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
    3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

        > [!NOTE]
        > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
        > 
        > Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Yeni bir C# sınıfına ekleme **filtreleri** klasörünü adlandırın *MyNewCustomActionFilter.cs*
3. Açık **MyNewCustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanı:

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Varsayılan sınıf bildirimi, aşağıdaki kod ile değiştirin.

    (Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Bu özel eylem filtresi neredeyse daha önceki alıştırmada oluşturduğunuz aynıdır. Ana fark olduğunu *&quot;oturum tarafından&quot;* sona eren filtresi tanımlamak için bu yeni sınıf adıyla güncelleştirilmiş öznitelik günlüğe kayıtlı.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>2. Görev: yeni bir kod dinleyiciyi StoreController sınıfına ekleme

Bu görevde, yeni bir özel filtre StoreController sınıfına ekleyebilir ve nasıl hem filtreleri birlikte çalıştığını doğrulamak için çözümü çalıştırın.

1. Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** ve yeni özel filtre ekleme **MyNewCustomActionFilter** içine  **StoreController** sınıfı gibi aşağıdaki kodda gösterilmiştir.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Şimdi, bu iki özel eylem filtreleri nasıl çalıştığını görmek için uygulamayı çalıştırın. Bunu yapmak için basın **F5** ve uygulama başladığında kadar bekleyin.
3. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.

    ![Oturum, İzleyici durumu sayfası etkinlik önce](aspnet-mvc-4-custom-action-filters/_static/image5.png "oturum sayfa etkinliği önce İzleyicisi durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*
4. Birini **türleri** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
5. Bu süre denetleyin; ziyaretleriniz iki kez izlenen: her özel eylem filtreleri de eklendikten sonra **StorageController** sınıfı.

    ![Etkinlik günlüğe eylem günlüğü](aspnet-mvc-4-custom-action-filters/_static/image6.png "günlüğe etkinlik eylem günlüğü")

    *Etkinlik günlüğe eylem günlüğü*
6. Tarayıcıyı kapatın.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>3. Görev: Filtre Sıralama yönetme

Bu görevde, sipariş işaretleyemezsiniz kullanarak filtreler yürütme düzenini yönetmek öğreneceksiniz.

1. Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** belirtin **sipariş** hem filtreleri özelliğinde ister aşağıda gösterilmiştir.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Şimdi, filtreler, sıra özelliğin değerini bağlı olarak nasıl yürütülür doğrulayın. Filtre sırası en küçük değer, bulabilirsiniz (**CustomActionFilter**) yürütülen ilki. Tuşuna **F5** ve uygulama başladığında kadar bekleyin.
3. Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.

    ![Oturum, İzleyici durumu sayfası etkinlik önce](aspnet-mvc-4-custom-action-filters/_static/image7.png "oturum sayfa etkinliği önce İzleyicisi durumu")

    *Sayfa etkinliği önce günlük İzleyici durumu*
4. Birini **türleri** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
5. Bu süre, ziyaretleriniz izlenmekte olan onay filtreler siparişi değere göre sıralı: **CustomActionFilter** günlükleri ilk.

    ![Etkinlik günlüğe eylem günlüğü](aspnet-mvc-4-custom-action-filters/_static/image8.png "günlüğe etkinlik eylem günlüğü")

    *Etkinlik günlüğe eylem günlüğü*
6. Şimdi, filtreler siparişi değerini güncelleştirin ve günlüğe kaydetme sırasını nasıl değiştiğini doğrulayın. İçinde **StoreController** sınıfı, aşağıda gösterildiği gibi filtreler sıra değeri güncelleştirin.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Tuşlarına basarak uygulamayı yeniden çalıştırmak **F5**.
8. Birini **türleri** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
9. Bu kez, günlükleri tarafından oluşturulan denetleyin **MyNewCustomActionFilter** filtre ilk görünür.

    ![Etkinlik günlüğe eylem günlüğü](aspnet-mvc-4-custom-action-filters/_static/image9.png "günlüğe etkinlik eylem günlüğü")

    *Etkinlik günlüğe eylem günlüğü*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>4. Görev: Genel filtre kaydetme

Bu görevde, yeni filtre kaydetmek için çözüm güncelleştirir (**MyNewCustomActionFilter**) genel bir filtre olarak. Bunu yaparak, bu uygulamadaki ve yalnızca önceki görevde olduğu gibi StoreController olanlarla ilgili tüm eylemleri parametreye tarafından tetiklenir.

1. İçinde **StoreController** sınıfı, Kaldır **[MyNewCustomActionFilter]** özniteliği ve sipariş özelliğinden **[CustomActionFilter]**. Aşağıdaki gibi görünmelidir:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Açık **Global.asax** bulun ve dosya **uygulama\_Başlat** yöntemi. Her zaman uygulama başlar, bildirim kaydeden genel filtreleri çağırarak **RegisterGlobalFilters** yöntemi içinde **FilterConfig** sınıfı.

    ![Genel filtrelerin Global.asax'ta kaydetme](aspnet-mvc-4-custom-action-filters/_static/image10.png "genel filtrelerin Global.asax'ta kaydediliyor")

    *Genel filtrelerin Global.asax'ta kaydediliyor*
3. Açık **FilterConfig.cs** içinde dosya **uygulama\_Başlat** klasör.
4. System.Web.Mvc kullanarak bir başvuru ekleyin MvcMusicStore.Filters kullanma; ad alanı.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Güncelleştirme **RegisterGlobalFilters** özel filtrenizle ekleme yöntemi. Bunu yapmak için vurgulanmış kodu ekleyin:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Tuşlarına basarak uygulamayı çalıştırmak **F5**.
7. Birini **türleri** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.
8. Onay artık **[MyNewCustomActionFilter]** HomeController ve ActionLogController çok eklenmiş olur.

    ![Etkinlik günlüğe eylem günlüğü](aspnet-mvc-4-custom-action-filters/_static/image11.png "günlüğe etkinlik eylem günlüğü")

    *Genel etkinlik günlüğe eylem günlüğü*

> [!NOTE]
> Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarı tamamlayarak özel eylemler yürütmek için bir eyleme eylem filtresi genişletme öğrendiniz. Ayrıca, sayfa denetleyicilerine herhangi bir filtre eklemesine öğrendiniz. Aşağıdaki kavramlar kullanılıyordu:

- ASP.NET MVC ActionFilterAttribute sınıfıyla özel eylem filtreleri oluşturma
- Nasıl ASP.NET MVC denetleyicileri filtre ekleme
- Order özelliğini kullanarak filtre sıralama yönetme
- Genel filtre kaydetme

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Web kutucuğu için VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı

1. Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.

    > [!NOTE]
    > Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın. Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).

    ![Windows Azure Portal'da oturum açın](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image18.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image19.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-custom-action-filters/_static/image20.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](aspnet-mvc-4-custom-action-filters/_static/image21.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-custom-action-filters/_static/image22.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.

    ![Yayımlama profili web sitesi indiriliyor](aspnet-mvc-4-custom-action-filters/_static/image23.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-custom-action-filters/_static/image24.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Değişiklikleri onaylayın*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-custom-action-filters/_static/image29.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-custom-action-filters/_static/image30.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-custom-action-filters/_static/image31.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Yeni bir veritabanı adı yazın.

     ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image33.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-custom-action-filters/_static/image34.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-custom-action-filters/_static/image36.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-custom-action-filters/_static/image37.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-custom-action-filters/_static/image38.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-custom-action-filters/_static/image39.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-custom-action-filters/_static/image40.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-custom-action-filters/_static/image41.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-custom-action-filters/_static/image42.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
