---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 temelleri | Microsoft Docs
author: rick-anderson
description: "Bu uygulamalı Laboratuvar MVC (Model View Controller) müzik deposu tanıtır ve ASP.NET MV kullanmak adım adım açıklanmaktadır öğretici bir uygulama tabanlı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 086084b63cceca1c2d4e0bd4e5b654aaad6637a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 temelleri
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> Bu uygulamalı Laboratuvar MVC (Model View Controller) müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio nasıl kullanılacağını hakkında adım adım anlatan öğretici bir uygulama temel alır. Laboratuvar Basitlik öğreneceksiniz henüz güç bu teknolojiler birlikte kullanma. Basit bir uygulama ile başlar ve tam olarak işlevsel bir ASP.NET MVC 4 Web uygulaması elde edene kadar oluşturacaksınız.
> 
> Bu Laboratuvar, ASP.NET MVC 4 ile çalışır.
> 
> Eğitmen uygulama ASP.NET MVC 3 sürümünü keşfetmek isterseniz içinde bulabilirsiniz [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).
> 
> > [!NOTE]
> > Bu uygulamalı Laboratuvar Geliştirici HTML ve JavaScript gibi Web geliştirme teknolojilerinde deneyim sahibi olduğunu varsayar.
> 
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Müzik deposu uygulama

Bu Laboratuvar oluşturulacak müzik deposu web uygulaması üç ana bölümden oluşur: alışveriş, kullanıma alma ve yönetim. Ziyaretçilerin albümleri türe göre Gözat, Albümler kendi Sepete Ekle, bunların seçimini inceleyin ve son olarak oturum açma ve sırasını tamamlamak için kullanıma alma devam mümkün olacaktır. Ayrıca, depolama yöneticileri ana özelliklerinin yanı sıra kullanılabilir albümleri yönetebilmek için olacaktır.

![Müzik deposu ekranlar](aspnet-mvc-4-fundamentals/_static/image1.png "müzik deposu ekranları")

*Müzik deposu ekranlar*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 temelleri

Müzik deposu uygulaması kullanılarak oluşturulur **Model View Controller (MVC)**, uygulamanın üç ana bileşene ayırır mimari bir desen:

- **Modelleri**: Model nesneleri olan etki alanı mantığı uygulamak uygulama bölümlerinin. Genellikle, model nesneleri de alabilir ve model durumu bir veritabanında depolar.
- **Görünümleri:** görünümler, uygulamanın kullanıcı arabirimini (UI) görüntüleme bileşenleridir. Genellikle, bu UI model verilerinden oluşturulur. Bir örnek metin kutuları ve açılan listesini albüm nesnenin geçerli durumu görüntüler, Albümler düzenleme görünümünü olabilir.
- **Denetleyicileri:** denetleyicileri, kullanıcı etkileşimini işleyen, modelle ve sonuçta kullanıcı arabirimini oluşturmak için bir görünüm seçin bileşenleridir. Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.

MVC örüntüsü, öğeler arasında gevşek bağlantı sağlarken uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini ayrı uygulamaları oluşturmak için yardımcı olur. Bu ayrım bir uygulama oluşturduğunuzda bir seferde bir yönüne odaklanmanızı sağlar gibi karmaşıklığını yönetmenize yardımcı olur. Buna ek olarak, MVC örüntüsü, uygulamalar, aynı zamanda teste dayalı geliştirme (TDD) kullanımını uygulamaları oluşturmak ve sınamak kolaylaştırır.

**ASP.NET MVC** framework ASP.NET MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sunar. **ASP.NET MVC** çerçevedir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web forms tabanlı uygulamaları olduğu gibi) mevcut ASP.NET özellikleriyle gibi ana sayfalar ve üyelik tabanlı Tümleşik Çekirdek .NET framework'ün tüm güç almak için kimlik doğrulaması. Bu zaten kullandığınız tüm kitaplıkları da ASP.NET MVC 4'te kullanılabilir olmadığından, ASP.NET Web Forms bilginiz varsa yararlı olur.

Ayrıca, bir MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ paralel gelişimi de kolaylaştırır. Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici üzerinde modeldeki iş mantığına odaklanabilir.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:

- Müzik deposu uygulama öğretici temel sıfırdan bir ASP.NET MVC uygulaması oluşturma
- Giriş sayfasına sitenin ve ana işlevselliğini gözatmak için URL'leri işlemek için denetleyicileri ekleme
- Kendi stil birlikte görüntülenen içeriği özelleştirmek için bir görünüm ekleyin
- İçerir ve veri ve etki alanı mantığı yönetmek için modeli sınıfları ekleme
- Görünüm şablonları denetleyicisi eylemlerden bilgi geçirmek görünüm modeli deseni kullan
- Internet uygulamaları için ASP.NET MVC 4 yeni şablon keşfedin

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:

- [Web için Visual Studio 2012 Express](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleme**

Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir. Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:

1. [Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma](#Exercise1)
2. [Alıştırma 2: bir denetleyici oluşturma](#Exercise2)
3. [Alıştırma 3: bir denetleyiciye parametreleri geçirme](#Exercise3)
4. [Alıştırma 4: bir görünüm oluşturma](#Exercise4)
5. [Alıştırma 5: görünüm Model oluşturma](#Exercise5)
6. [Alıştırma 6: görünümünde parametrelerini kullanma](#Exercise6)
7. [Alıştırma 7: ASP.NET MVC 4 yeni şablonu çevresinde bir diz](#Exercise7)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma

Bu alıştırmada, bir ASP.NET MVC uygulaması Visual Studio Express 2012 ana klasör kuruluş yanı sıra Web için nasıl oluşturulacağını öğreneceksiniz. Ayrıca, yeni denetleyici ekleyin ve basit bir dize uygulamanın giriş sayfası görüntüleme sağlamak nasıl öğreneceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Görev 1 - ASP.NET MVC Web uygulaması projesi oluşturma

1. Bu görevde, MVC Visual Studio şablonu kullanarak boş bir ASP.NET MVC uygulaması projesi oluşturacaksınız. Başlat **VS için Web Express**.
2. Üzerinde **dosya** menüsünde tıklatın **yeni proje**.
3. İçinde **yeni proje** iletişim kutusu seç **ASP.NET MVC 4 Web uygulaması** proje türü, altında bulunan **Visual C#** **Web** şablonu Liste.
4. Değişiklik **adı** için *MvcMusicStore*.
5. İçinde yeni bir çözüm konumunu ayarlama **başlamak** klasöründe Bu alıştırmada'nın kaynak, örneğin **[YOUR HOL yol] \Source\Ex01-CreatingMusicStoreProject\Begin**. **Tamam**'ı tıklatın.

    ![Yeni Proje iletişim kutusu oluşturma](aspnet-mvc-4-fundamentals/_static/image2.png "yeni proje iletişim kutusu oluşturma")

    *Yeni Proje iletişim kutusu oluşturma*
6. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **temel** şablonu emin olun **görünüm altyapısı** seçili **Razor**. **Tamam**'ı tıklatın.

    ![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image3.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")

    *Yeni ASP.NET MVC 4 Proje iletişim kutusu*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Görev 2 - çözüm yapısını keşfetme

ASP.NET MVC çerçevesi, MVC örüntüsünü destekleyen Web uygulamaları oluşturmanıza yardımcı olacak bir Visual Studio Proje şablonu içerir. Bu şablon, gerekli klasörleri, öğe şablonları ve yapılandırma dosyası girdileri yeni bir ASP.NET MVC Web uygulaması oluşturur.

Bu görevde, ilgili olan öğeler anlamak için çözüm yapısını ve ilişkilerini inceleyeceksiniz. ASP.NET MVC çerçevesi varsayılan olarak kullandığı için aşağıdaki klasörlerin tüm ASP.NET MVC uygulamasında bulunan bir &quot;kuralı yapılandırması üzerinden&quot; yaklaşım ve bazı varsayılan varsayımları temel klasörü adlandırma yapar kuralları.

1. Proje oluşturulduktan sonra sağ taraftaki Solution Explorer'da oluşturulan klasör yapısı gözden geçirin:

    ![Çözüm Gezgini'nde ASP.NET MVC klasör yapısını](aspnet-mvc-4-fundamentals/_static/image4.png "Çözüm Gezgini'nde ASP.NET MVC klasör yapısı")

    *Çözüm Gezgini'nde ASP.NET MVC klasör yapısı*

    1. **Denetleyicileri**. Bu klasör denetleyicisi sınıfları içerir. Bir temel MVC uygulamasındaki denetleyicileri son kullanıcı etkileşimi işleme, model düzenleme ve sonuçta UI görünümü seçerek sorumludur.

        > [!NOTE]
        > MVC çerçevesi ile sona erdirmek için tüm denetleyicilerinin adlarını gerektirir &quot;denetleyicisi&quot;-Örneğin, HomeController, LoginController veya ProductController.
    2. **Modelleri**. Bu klasör, MVC Web uygulaması için uygulama modeli temsil eden sınıflar için sağlanır. Bu genellikle, nesneleri ve veri deposu ile etkileşim için mantığı tanımlayan kodu içerir. Tipik olarak gerçek model nesneleri ayrı sınıf kitaplıkları olacaktır. Ancak, yeni bir uygulama oluşturduğunuzda, sınıfları içerir ve ardından bunları ayrı sınıf kitaplıkları sonraki bir zamanda geliştirme döngüsü taşınıp.
    3. **Görünümleri**. Görünümler, uygulamanın kullanıcı arabirimini görüntülemeden sorumlu bileşenler için önerilen konum klasörüdür. Görünümleri .aspx, .ascx, .cshtml ve .master dosyaları işleme görünümlerine ilgili diğer dosyaların yanı sıra kullanın. Görünümler klasöründe her denetleyici için bir klasör içerir; Bu klasör Denetleyici adı önekiyle adlandırılır. Örneğin, adlandırılmış bir denetleyiciniz varsa **HomeController**, görünümler klasör giriş adlı bir klasör içerir. ASP.NET MVC çerçevesi bir görünüm yüklendiğinde varsayılan olarak, .aspx dosyası Views\controllerName klasöründe istenen görünüm adıyla arar (**görünümler [controllername öğesi] [eylem] .aspx**) veya (**görünümler [controllername öğesi] [Action] .cshtml**) Razor görünümleri için.

    > [!NOTE]
    > Daha önce listelenen klasörler ek olarak, bir MVC Web uygulamasını kullanan **Global.asax** Genel Yönlendirme URL'sini ayarlamak için dosya Varsayılanları ve kullandığı **Web.config** uygulamayı yapılandırmak için bir dosya.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Görev 3 - bir HomeController ekleme

MVC çerçevesi kullanmayın ASP.NET uygulamalarında kullanıcı etkileşimi sayfaların çevresine ve oluşturma ve bu sayfaların etkinlikleri işleme etrafında düzenlenmiştir. Buna karşılık, ASP.NET MVC uygulamaları ile kullanıcı etkileşimi denetleyicileri ve kendi eylem yöntemlerini düzenlenmiştir.

Diğer taraftan, ASP.NET MVC çerçevesi denetleyicileri olarak adlandırılır sınıfları URL'leri eşler. Denetleyicileri gelen istekleri işleyen, kullanıcı girişi ve etkileşimleri işlemek, uygun uygulama mantığını yürütme ve istemciye geri gönderilecek yanıt belirlemek (HTML görüntülemek, dosya indirme, farklı bir URL vb. için yeniden yönlendirme). HTML görüntüleme söz konusu olduğunda, denetleyici sınıfını genellikle bir istek için HTML biçimlendirmesi oluşturmak için ayrı görünümü bileşen çağırır. Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.

Bu görevde URL'leri müzik deposu site giriş sayfasına işleyecek denetleyici sınıfını ekleyeceksiniz.

1. Sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu:

    ![Bir denetleyici komut ekleme](aspnet-mvc-4-fundamentals/_static/image5.png "denetleyicisi komut ekleme")

    *Denetleyici komut ekleme*
2. **Denetleyici Ekle** iletişim kutusu görüntülenir. Denetleyici adı *HomeController* ve basın **Ekle**.

    ![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image6.png "denetleyici Ekle iletişim kutusu")

    *Denetleyici Ekle iletişim kutusu*
3. Dosya **HomeController.cs** oluşturulan **denetleyicileri** klasör. Olması için **HomeController** dizin eylemi üzerinde bir dize döndürecek, yerine **dizin** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex1 HomeController dizin*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında çalışacaktır.

1. Tuşuna **F5** uygulamayı çalıştırın. Proje derlenir ve yerel IIS Web sunucusu başlatır. Yerel IIS Web sunucusu otomatik olarak Web sunucusu URL'sine yönlendiren bir web tarayıcısı açılır.

    ![Bir web tarayıcısında çalışan uygulama](aspnet-mvc-4-fundamentals/_static/image7.png "bir web tarayıcısında çalışan uygulama")

    *Bir web tarayıcısında çalışan uygulama*

    > [!NOTE]
    > Yerel IIS Web sunucusu Web sitesi bir rastgele serbest bağlantı noktası numarası üzerinde çalışır. Yukarıdaki şekilde, site çalışıyor `http://localhost:50103/`, bağlantı noktası 50103 kullanıyor. Bağlantı noktası numarası farklılık gösterebilir.
2. Tarayıcıyı kapatın.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Alıştırma 2: bir denetleyici oluşturma

Bu alıştırmada, basit müzik deposu uygulamanın işlevselliğini uygulamak için denetleyici güncelleştirmek öğreneceksiniz. Bu denetleyici her aşağıdaki belirli isteklerini işlemek için eylem yöntemleri tanımlar:

- Müzik deposundaki müzik türler listeleme sayfası
- Belirli bir tarzını müzik albümlerini tümünün listeleyen bir Gözat sayfası
- Belirli Müzik albüm hakkındaki bilgileri gösterir Ayrıntılar sayfası

Bu alıştırmada kapsam için bu eylemleri yalnızca artık bir dize döndürür.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Görev 1 - yeni StoreController sınıf ekleme

Bu görevde, yeni bir denetleyicisi ekleyeceksiniz.

1. Zaten açık değilse, başlangıç **VS Express Web 2012**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex02 CreatingAController\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Yeni denetleyici ekleyin. Bunu yapmak için sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu. Değişiklik **Denetleyici adı** için *StoreController*, tıklatıp **Ekle**.

    ![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image8.png "denetleyici Ekle iletişim kutusu")

    *Denetleyici Ekle iletişim kutusu*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Görev 2 - StoreController'ın Eylemler değiştirme

Bu görevde denir denetleyici yöntemlerine değiştirecek **Eylemler**. Eylemler URL istekleri işleme ve tarayıcı veya URL çağrılan kullanıcıya geri gönderilmesi gereken içerik belirleme sorumludur.

1. **StoreController** sınıf zaten bir **dizin** yöntemi. Müzik deposu tüm türler listeler sayfası uygulamak için bu laboratuvarda daha sonra onu kullanır. Şu an için yalnızca Değiştir **dizin** bir dize döndürür aşağıdaki kod ile yöntemi &quot;Store.Index() gelen Hello&quot;:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController dizin*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Ekleme **Gözat** ve **ayrıntıları** yöntemleri. Bunu yapmak için aşağıdaki kodu ekleyin **StoreController**:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde, uygulama bir web tarayıcısında çalışacaktır.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlayacağını **giriş** sayfası. Her eylemin uygulama doğrulamak için URL'yi değiştirin.

    1. **/ Depolama**. Göreceğiniz  **&quot;Store.Index() gelen Hello&quot;**.
    2. **/ Deposu/Gözat**. Göreceğiniz  **&quot;Store.Browse() gelen Hello&quot;**.
    3. **/ Deposu/ayrıntıları**. Göreceğiniz  **&quot;Store.Details() gelen Hello&quot;**.

        ![StoreBrowse gözatma](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse gözatma")

        */Store/Browse gözatma*
3. Tarayıcıyı kapatın.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Alıştırma 3: bir denetleyiciye parametreleri geçirme

Şimdiye kadar sabit dizeler denetleyicilerinden iade. Bu alıştırmada URL ve sorgu dizesi kullanılarak ve tarayıcıya metinle yanıt yöntemi eylemler yapma denetleyicisi parametreleri geçirmek öğreneceksiniz.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Görev 1 - StoreController ekleme Tarz parametresi

Bu görevde, kullanacağınız **querystring** parametreleri göndermek için **Gözat** eylem yönteminde **StoreController**.

1. Zaten açık değilse, başlangıç **VS Express Web**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex03 PassingParametersToAController\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **StoreController** sınıfı. Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.
4. Değişiklik **Gözat** için belirli bir tarzını istemek için bir dize parametresi ekleme yöntemi. ASP.NET MVC otomatik olarak herhangi bir sorgu dizesi geçti veya adlandırılmış parametreleri form gönderme **Tarz** çağrıldığında Bu eylem yöntemine. Bunu yapmak için yerini **Gözat** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > Kullanmakta olduğunuz **HttpUtility.HtmlEncode** yardımcı yöntemi, Javascript gibi bir bağlantıyla görünüme injecting gelen kullanıcılar önler   **/deposu/Gözat? Tarz =&lt;betik&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
    > 
    > Daha fazla açıklama için lütfen şu adresi ziyaret [bu msdn makalesine](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulama çalışıyor

Bu görevde, uygulama bir web tarayıcısında denemek ve kullanmanızı **Tarz** parametresi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlayacağını **giriş** sayfası. URL'ye değiştirin   */depolamak/Gözat? Tarz DISCO =* eylemi Tarz parametre aldığını doğrulamak için.

    ![StoreBrowseGenre gözatma DISCO =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre gözatma DISCO =")

    *Gözatma /Store/Browse? Tarz DISCO =*
3. Tarayıcıyı kapatın.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Görev 3 - URL'de katıştırılmış bir ID parametresi ekleme

Bu görevde, kullanacağınız **URL** geçirmek için bir **kimliği** parametresi **ayrıntıları** eylem yöntemi **StoreController**. Yönlendirme kuralı bir URL kesimi eylem yöntemi adından sonra adlı bir parametre işlemek için ASP.NET MVC'ın varsayılan **kimliği**. Eylem yöntemi kimliği adlı parametre varsa, daha sonra ASP.NET MVC otomatik olarak URL kesimi için parametre olarak geçer. URL'deki **deposu/Ayrıntılar/5**, **kimliği** olarak yorumlanacak **5**.

1. Değişiklik **ayrıntıları** yöntemi **StoreController**, ekleyerek bir **int** adlı bir parametreye **kimliği**. Bunu yapmak için yerini **ayrıntıları** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında denemek ve kullanmanızı **kimliği** parametresi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlayacağını **giriş** sayfası. URL'ye değiştirin */Store/Details/5* eylemi ID parametresi aldığını doğrulamak için.

    ![StoreDetails5 gözatma](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 gözatma")

    */Store/details/5 gözatma*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Alıştırma 4: bir görünüm oluşturma

Şu ana kadar dizeleri denetleyicisi eylemlerden iade. Denetleyicileri nasıl çalıştığını anlamak yararlı bir yolu olsa da, değil gerçek Web uygulamalarınızın nasıl oluşturulur düşürür. Şablon dosyalarını kullanarak tarayıcıya HTML oluşturmak için daha iyi bir yaklaşım sağlayan bileşenleri görünümlerdir.

Bu alıştırmada, ortak HTML içerik, site ve son olarak HTML döndürülecek HomeController etkinleştirmek için bir şablonu görüntüleme Görünüm ve yapısını geliştirmek için bir stil sayfası için bir şablon kurulumu için bir düzen ana sayfa eklemek öğreneceksiniz.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Görev 1 - dosyasını değiştirerek \_layout.cshtml

Dosya **~/Views/Shared/\_layout.cshtml** tüm Web sitesi kullanmak üzere ortak HTML için bir şablon kurulum olanak tanır. Bu görevde giriş sayfası ve depolama alanına bir düzen ana sayfa bağlantılar ile birlikte ortak bir üstbilgiyle ekleyeceksiniz.

1. Zaten açık değilse, başlangıç **VS Express Web**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex04 CreatingAView\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Dosya  **\_layout.cshtml** sitesindeki tüm sayfalara HTML kapsayıcı düzeni içeriyor. İçerdiği  **&lt;html&gt;**  öğe için HTML yanıtını yanı sıra  **&lt;head&gt;**  ve  **&lt;gövde&gt;**  öğeleri. **@RenderBody()** HTML içindeki gövde tanımlamak bölgeler şablonları kurulamayacak oturum dinamik içerik doldurmak bu görünümü.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Giriş sayfası ve depolama alanı sitesindeki tüm sayfalara bağlantılar sahip ortak bir üstbilgi ekleyin. Bunu yapmak için aşağıdaki aşağıdaki kodu ekleyin &lt;gövde&gt; deyimi.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Her sayfanın gövde bölümü oluşturmak için bir sayı içerir. Değiştir  **@RenderBody()** aşağıdaki higlighted kodla: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Biliyor muydunuz? Visual Studio 2012 yaygın olarak kullanılan kod HTML, kod dosyaları ve daha fazlasını eklemek kolaylaştıran parçacıkları var! Out yazarak deneyin  **&lt;div&gt;**  tuşuna basarak **sekmesini** iki kez bir tam eklemek için **div** etiketi.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Görev 2 - ekleme CSS stil sayfası

Boş proje şablonu yalnızca temel formlar ve doğrulama iletileri görüntülemek için kullanılan stiller içeren çok kolaylaştırılmış bir CSS dosyası içerir. Site Görünüm ve yapısını geliştirmek için ek CSS ve görüntüleri (büyük olasılıkla bir tasarımcı tarafından sağlanan) kullanır.

Bu görevde, sitenin stillerini tanımlamak için bir CSS stil ekleyeceksiniz.

1. CSS dosyası ve görüntüler dahil edilen **Source\Assets\Content** bu laboratuvarı klasör. Uygulama eklemek için bunların içerikten sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** aşağıda gösterildiği gibi Web için Visual Studio Express içinde:

    ![Stil içeriği sürükleyerek](aspnet-mvc-4-fundamentals/_static/image12.png "stili içeriği sürükleme")

    *Stil içeriği sürükleme*
2. Bir uyarı iletişim kutusu, değiştirdiğinizden onayı soran görünür **Site.css** dosyası ve var olan bazı görüntüler. Denetleme **tüm öğeleri Uygula** tıklatıp **Evet**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Görev 3 - bir görünüm şablonu ekleme

Bu görevde, Düzen ana sayfa kullanacağınız HTML yanıtı oluşturmak için bir şablonu görüntüleme ekleyecek ve bu alıştırmada CSS eklenir.

1. Sitenin giriş sayfasına göz atarken görünüm şablonu kullanmak için önce bir dize döndüren yerine belirtmek gerekir **HomeController dizin** yöntemi döndürür bir **Görünüm**. Açık **HomeController** sınıfı ve değiştirmek kendi **dizin** döndürülecek yöntemi bir **ActionResult**, ve dönüş **View()**.

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex4 HomeController dizin*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Şimdi, uygun bir görünüm şablon eklemeniz gerekir. Bunu yapmak için **sağ** içinde **dizin** eylem yöntemi ve select **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

    ![Dizin yöntemi içinden bir görünümle ekleme](aspnet-mvc-4-fundamentals/_static/image13.png "dizin yöntemi içinden bir görünümle ekleme")

    *Dizin yöntemi içinden bir görünümle ekleme*
3. **Görünüm Ekle** bir görünüm şablon dosyası oluşturmak için iletişim kutusu görünecektir. Varsayılan olarak, bu iletişim kutusu görünümü şablonunun adını böylece kullanacağı eylem yönteminin eşleşen önceden doldurur. Kullandığınız olduğundan **Görünüm Ekle** bağlam menüsü içinde **dizin** HomeController içinde eylem yönteminin **Görünüm Ekle** iletişim dizin varsayılan görünüm adı olarak sahip. **Ekle**'yi tıklatın.

    ![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image14.png "Görünüm Ekle iletişim kutusu")

    *Görünüm Ekle iletişim kutusu*
4. Visual Studio'nun oluşturduğu bir **Index.cshtml** görünüm şablonu içinde **görünümler** klasörünü ve ardından açar.

    ![Oluşturulan dizin görünüm ev](aspnet-mvc-4-fundamentals/_static/image15.png "oluşturulan giriş dizini görünüm")

    *Oluşturulan giriş dizini görünüm*

    > [!NOTE]
    > adını ve konumunu **Index.cshtml** dosya geçerlidir ve varsayılan ASP.NET MVC adlandırma kuralları izler.
    > 
    > Klasör \Views\**giriş** denetleyici adıyla eşleşen (**giriş** denetleyicisi). Görünüm şablonu adı (**dizin**), görünüm görüntüleme denetleyici eylem yöntemine eşleşir.
    > 
    > Bu şekilde, ASP.NET MVC açıkça bir görünüme dönmek için bu adlandırma kuralını kullanırken, bir görünüm şablonu konumu veya adı belirtin gereğini ortadan kaldırır.
5. Oluşturulan görünüm şablonu dayanır  **\_layout.cshtml** daha önce tanımlanan şablon. ViewBag.Title özelliğine güncelleştirme **giriş**, ana içeriği değiştirip **bu giriş sayfasıdır**, aşağıdaki kodda gösterildiği gibi:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Seçin **MvcMusicStore** basın ve Çözüm Gezgini proje **F5** uygulamayı çalıştırın.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Görev 4: doğrulama

Önceki alıştırmada tüm adımları doğru gerçekleştirmiş doğrulamak için aşağıdaki gibi ilerleyin:

Bir tarayıcıda açıldığından uygulamayla unutmamalısınız:

1. Bulundu ve görüntülenen HomeController'ın dizin eylem yöntemi **\Views\Home\Index.cshtml** kodu denilen olsa bile şablonu görüntülemek **View() dönmek**, ardından şablonu görüntüleme standart adlandırma kuralı.
2. Giriş sayfası içinde tanımlanan Hoş Geldiniz iletisi görüntüler **\Views\Home\Index.cshtml** şablonu görüntüle.
3. Giriş sayfasını kullanarak  **\_layout.cshtml** şablonu ve Hoş Geldiniz iletisi standart site HTML düzenini içinde yer alır.

    ![Giriş dizini tanımlı LayoutPage ve stil kullanarak görünümü](aspnet-mvc-4-fundamentals/_static/image16.png "tanımlı LayoutPage ve stil kullanarak giriş dizini görünümü")

    *Giriş dizini tanımlı LayoutPage ve stil kullanarak görünümü*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Alıştırma 5: görünüm Model oluşturma

Şu ana kadar sabit kodlanmış HTML görüntülemek görünümlerinizi yapılan ancak dinamik web uygulamaları oluşturmak için şablonu görüntüleme denetleyicisinden bilgi almanız gerekir. Bu amaç için kullanılacak bir ortak tekniktir **ViewModel** uygun HTML yanıtı oluşturmak için gereken tüm bilgileri paketini denetleyicinin sağlar düzeni.

Bu alıştırmada, ViewModel sınıfı oluşturmak ve gerekli özellikleri eklemek öğreneceksiniz: türler deposu ve bu tür listesi sayısı. Oluşturulan ViewModel kullanmak için StoreController güncelleştirmek ve son olarak, belirtilen özellikleri sayfasında görüntüler yeni bir görünüm şablonu oluşturur.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Görev 1 - ViewModel sınıfı oluşturma

Bu görevde deposu Tarz listeleme senaryo gerçekleştireceksiniz ViewModel sınıfı oluşturur.

1. Zaten açık değilse, başlangıç **VS Express Web**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex05 CreatingAViewModel\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Oluşturma bir **ViewModels** ViewModel tutmak için klasör. Bunu yapmak için üst düzey sağ **MvcMusicStore** proje, select **Ekle** ve ardından **yeni klasör**.

    ![Yeni bir klasör ekleme](aspnet-mvc-4-fundamentals/_static/image17.png "yeni bir klasör ekleme")

    *Yeni bir klasör ekleme*
4. Klasör adı *ViewModels*.

    ![Çözüm Gezgini'nde ViewModels klasörü](aspnet-mvc-4-fundamentals/_static/image18.png "Çözüm Gezgini'nde ViewModels klasörü")

    *Çözüm Gezgini'nde ViewModels klasörü*
5. Oluşturma bir **ViewModel** sınıfı. Bunu yapmak için sağ **ViewModels** kısa süre önce oluşturuldu, klasörü seçin **Ekle** ve ardından **yeni öğe**. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreIndexViewModel.cs*, ardından **Ekle**.

    ![Yeni bir sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image19.png "yeni sınıf ekleme")

    *Yeni bir sınıf ekleme*

    ![StoreIndexViewModel sınıfı oluşturma](aspnet-mvc-4-fundamentals/_static/image20.png "oluşturma StoreIndexViewModel sınıfı")

    *StoreIndexViewModel sınıfı oluşturma*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Görev 2 - ViewModel sınıfı özellik ekleme

StoreController beklenen HTML yanıtı oluşturmak için şablonu görüntüleme iletilmek üzere iki parametre vardır: türler deposu ve bu tür listesi sayısı.

Bu görevde, 2 Bu özellikler ekleyeceksiniz **StoreIndexViewModel** sınıfı: **NumberOfGenres** (tamsayı) ve **türler** (dizeler listesi).

1. Ekleme **NumberOfGenres** ve **türler** özelliklerine **StoreIndexViewModel** sınıfı. Bunu yapmak için sınıf tanımına 2 aşağıdaki satırları ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreIndexViewModel özellikleri*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > **{Alın; ayarlayın;}**  gösterimi yapar C#, kullanıcının kullanan otomatik uygulanan özellikler özelliği. Bir özelliğin avantajlarından bize yedekleme alanı bildirmek gerek kalmadan sağlar.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Görev 3 - güncelleştirme StoreController StoreIndexViewModel kullanmak için

**StoreIndexViewModel** sınıfı geçirmek için gereken bilgileri yalıtır **StoreController**'s **dizin** yanıt oluşturmak için bir şablonu görüntüleme yöntemi .

Bu görevde, güncelleştirecektir **StoreController** kullanmak için **StoreIndexViewModel**.

1. Açık **StoreController** sınıfı.

    ![StoreController sınıfı açma](aspnet-mvc-4-fundamentals/_static/image21.png "açılış StoreController sınıfı")

    *Açılış StoreController sınıfı*
2. Kullanmak için **StoreIndexViewModel** sınıfıyla **StoreController**, şu ad alanının üstüne eklemeniz **StoreController** kod:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 ViewModels kullanarak StoreIndexViewModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Değişiklik **StoreController**'s **dizin** şekilde oluşturur ve doldurur eylem yöntemi bir **StoreIndexViewModel** nesnesi ve ardından bunu bir görünüm şablonu geçirir devre dışı bir HTML yanıtını onunla oluşturur.

    > [!NOTE]
    > Laboratuarda &quot;ASP.NET MVC modelleri ve veri erişimi&quot; deposu türler listesini veritabanından alır kod yazacaksınız. Aşağıdaki kodda oluşturacağınız bir **listesi** dolduracaktır kukla veri müzik **StoreIndexViewModel**.
    > 
    > Oluşturma ve ayarlama sonra **StoreIndexViewModel** nesnesi, onu geçirilecektir bağımsız değişken olarak **Görünüm** yöntemi. Bu görünüm şablon söz konusu nesne sahip bir HTML yanıtı oluşturmak için kullanacağı gösterir.
4. Değiştir **dizin** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreController dizin yöntemi*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > C# ile tanınmayan, kullanmanın varsayabilir **var** anlamına **viewModel** değişkenidir geç bağlama. C# Derleyici kullanarak tür çıkarımı değişkene atayın göre belirlemek için doğru değil - **viewModel** türü **StoreIndexViewModel**. Ayrıca, yerel derleme tarafından **viewModel** değişken olarak bir **StoreIndexViewModel** derleme zamanı get denetimi ve Visual Studio kod düzenleyicisini desteği yazın.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Görev 4 - StoreIndexViewModel kullanan bir görünüm şablonu oluşturma

Bu görevde denetleyicisinden geçirilen StoreIndexViewModel nesnesi türler listesini görüntülemek için kullanacağı bir görünüm şablonu oluşturacaksınız.

1. Yeni Görünüm şablonu oluşturmadan önce şimdi Projeyi derlemek için **Ekle iletişim kutusunu görüntüle** bildiği **StoreIndexViewModel** sınıfı. Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.

    ![Proje derleme](aspnet-mvc-4-fundamentals/_static/image22.png "projesi oluşturma")

    *Proje derleme*
2. Yeni bir görünüm şablonu oluşturun. Bunu yapmak için sağ tıklatın içinde **dizin** yöntemi ve seçin **Görünüm Ekle**.

    ![Bir görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image23.png "bir görünümü ekleme")

    *Bir görünümü ekleme*
3. Çünkü **Ekle iletişim kutusunu görüntüle** gelen çağrıldı **StoreController**, varsayılan olarak şablonu görüntüleme ekleyecek bir **\Views\Store\Index.cshtml** dosya. Denetleme **bir kesin türü belirtilmiş-görünüm oluşturma** onay kutusunu ve ardından **StoreIndexViewModel** olarak **Model sınıfı**. Ayrıca, seçili görünüm altyapısı olduğundan emin olun **Razor**. **Ekle**'yi tıklatın.

    ![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image24.png "Görünüm Ekle iletişim kutusu")

    *Görünüm Ekle iletişim kutusu*

    **\Views\Store\Index.cshtml** görünüm şablon dosyası oluşturulur ve açılır. Sağlanan bilgilere dayanarak **Görünüm Ekle** iletişim şablon beklediğiniz görünümü son adımda bir **StoreIndexViewModel** örneği bir HTML yanıtı oluşturmak için kullanılacak veri olarak. Şablon devralması fark edeceksiniz bir `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Görev 5 - görünüm şablonunu güncelleştirme

Bu görevde, türler ve sayfa içinde adları sayısını almak üzere son görevde oluşturduğunuz görünüm şablonu güncelleştirir.

> [!NOTE]
> @ Sözdizimini kullanır (genellikle olarak adlandırılan &quot;kod nuggets&quot;) görünüm şablonu içindeki kod yürütmek için.


1. İçinde **Index.cshtml** içinde dosya **deposu** klasör, kendisine kodu şununla değiştirin:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > Word süre yazarak biter bitmez **modeli**, Visual Studio'nun IntelliSense, olası özellikleri ve yöntemleri arasından seçim listesi gösterilir.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Model özellikleri ve yöntemleri Visual Studio'nun IntelliSense ile Başlarken*
    > 
    > **Modeli** özelliği başvuruları **StoreIndexViewModel** denetleyici görünüm şablona geçirilen nesne. Bu görünüm şablon denetleyicisinden geçirilen verilerin tümünü erişemeyeceği anlamına gelir. **modeli** özelliği ve görünüm şablonu içindeki uygun bir HTML yanıt içine biçimlendirin.
    > 
    > Yalnızca seçebilirsiniz **NumberOfGenres** IntelliSense özelliğinden liste içinde ve ardından yazarak otomatik tamamlama olacak yerine onu basarak **SEKME tuşuna**.
2. Döngü Tarz listesinde üzerinden **StoreIndexViewModel** ve bir HTML oluşturmak  **&lt;ul&gt;**  kullanarak listesinde bir **foreach** döngü.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Tuşuna **F5** uygulamayı çalıştırın ve gözatmak için **/deposu**. Geçirilen türler listesini görürsünüz **StoreIndexViewModel** nesnesinin **StoreController** görünüm şablon.

    ![Türler listesini görüntüleyen görünümü](aspnet-mvc-4-fundamentals/_static/image26.png "türler listesini görüntüleyen görünümü")

    *Türler listesini görüntüleyen görünümü*
4. Tarayıcıyı kapatın.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Alıştırma 6: Görünümünde parametrelerini kullanma

Alıştırma 3'te denetleyiciye parametreler öğrendiniz. Bu alıştırmada, görünüm şablonunda bu parametreleri kullanmayı öğreneceksiniz. Bu amaç için önce veri ve etki alanı mantığı yönetmenize yardımcı olacak Model sınıflarına görülecektir. Ayrıca, kodlama URL yolu gibi şeyler endişelenmeden ASP.NET MVC uygulaması içindeki sayfalarına bağlantılar oluşturma öğreneceksiniz.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Görev 1 - modeli sınıfları ekleme

Yalnızca bilgi denetleyicisinden görünüme iletmek için oluşturulan ViewModels modeli sınıfları içerir ve veri ve etki alanı mantığı yönetmek için oluşturulur. Bu görevde bu kavramları göstermek için iki modeli sınıfları ekleyeceksiniz: **Tarz** ve **albüm**.

1. Zaten açık değilse, başlangıç **VS Express Web**
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex06 UsingParametersInView\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Ekleme bir **Tarz** Model sınıfı. Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Genre.cs*, ardından **Ekle**.

    ![Sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image27.png "sınıf ekleme")

    *Yeni bir öğe ekleme*

    ![Tarz Model sınıfı ekleme](aspnet-mvc-4-fundamentals/_static/image28.png "Tarz Model sınıfı ekleme")

    *Tarz Model sınıfı ekleme*
4. Ekleme bir **adı** Tarz sınıf özelliğine. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 Tarz*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Yordamın aynısını önce aşağıdaki eklemek bir **albüm** sınıfı. Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Album.cs*, ardından **Ekle**.
6. İki özellik albüm sınıfına ekleyin: **Tarz** ve **başlık**. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 albüm*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Görev 2 - bir StoreBrowseViewModel ekleme

A **StoreBrowseViewModel** bu görevin seçilen bir tarzını eşleşen albümleri göstermek için kullanılır. Bu görevde, bu sınıf oluşturacak ve işlemek için iki özellik eklemek **Tarz** ve kendi **albüm**kullanıcının listeleyin.

1. Ekleme bir **StoreBrowseViewModel** sınıfı. Bunu yapmak için sağ **ViewModels** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreBrowseViewModel.cs*, ardından **Ekle**.
2. Modeller için bir başvuru ekleyin **StoreBrowseViewModel** sınıfı. Bunu yapmak için aşağıdakileri ekleyin. ad alanını kullanarak:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. İki özellikleri **StoreBrowseViewModel** sınıfı: **Tarz** ve **albümleri**. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > Nedir **listesi&lt;albüm&gt;**  ?: Bu tanımı kullanarak **listesi&lt;T&gt;**  yazın, burada **T** kısıtlar hangi öğelerin bu türe **listesi** , bu durumda ait **albüm** (veya alt öğelerinden birini).
    > 
    > Sınıflar ve sınıf veya yöntemin bildirilir ve istemci kodu tarafından örneği C# dili özelliğidir kadar bir veya daha fazla türü belirtimini erteleneceği yöntemler tasarlamak için bu özelliği adlı **genel türler**.
    > 
    > **Liste&lt;T&gt;**  genel eşdeğerdir **ArrayList** yazın ve kullanılabilir **System.Collections.Generic** ad alanı. Kullanmanın avantajlarından biri **genel türler** türü belirtilmiş olduğundan, ilgilenebilmek denetimi elemanlara atama gibi işlemleri türü gerek olmayan **albüm** bir ileyaptığınızgibi**ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Görev 3 - yeni ViewModel StoreController kullanma

Bu görevde değiştirecek **StoreController**'s **Gözat** ve **ayrıntıları** yeni kullanmak için eylem yöntemleri **StoreBrowseViewModel** .

1. Modeller klasörü için bir başvuru ekleyin **StoreController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasöründe **Çözüm Gezgini** açarak **StoreController** sınıfı. Daha sonra aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Değiştir **Gözat** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı. Sahte verilerle bir tarzını ve iki yeni Albümler nesneler oluşturur (sonraki uygulamalı laboratuar ortamında, bir veritabanından gerçek veri kullanır). Bunu yapmak için yerini **Gözat** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Değiştir **ayrıntıları** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı. Yeni oluşturduğunuz **albüm** için döndürülecek nesne **Görünüm**. Bunu yapmak için yerini **ayrıntıları** aşağıdaki kod ile yöntemi:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Görev 4 - Gözat görünüm şablonu ekleme

Bu görevde, ekleyeceksiniz bir **Gözat** için belirli bir tarzını bulunan albümleri göstermek için Görünüm.

1. Yeni Görünüm şablonu oluşturmadan önce projeyi derlemek böylece **Görünüm Ekle** iletişim bilir hakkında **ViewModel** kullanılacak sınıfı. Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.
2. Ekleme bir **Gözat** görünümü. Bunu yapmak için sağ **Gözat** eylem yöntemi **StoreController** tıklatıp **Görünüm Ekle**.
3. İçinde **Görünüm Ekle** iletişim kutusunda, Görünüm adı olduğundan emin olun **Gözat**. Denetleme **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **StoreBrowseViewModel** gelen **Model sınıfı** açılır. Diğer alanları varsayılan değerlerine bırakın. Ardından **Ekle**.

    ![Gözat görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image29.png "Gözat görünümü ekleme")

    *Gözat görünümü ekleme*
4. Değiştirme **Browse.cshtml** Tarz'ın bilgilerini görüntülemek için erişim **StoreBrowseViewModel** şablonu görüntüleme geçirilen nesne. Bunu yapmak için içerik şununla değiştirin: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde, test **Gözat** yöntemi alır albümleri gelen **Gözat** yöntemi eylem.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin   **/depolamak/Gözat? Tarz DISCO =** eylemi iki albümleri döndürür doğrulanamadı.

    ![Mağaza DISCO albümleri gözatma](aspnet-mvc-4-fundamentals/_static/image30.png "deposu DISCO albümleri gözatma")

    *Mağaza DISCO albümleri gözatma*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Görev 6 - bilgi hakkında belirli bir albümü görüntüleme

Bu görevde, gerçekleştireceksiniz **deposu/ayrıntıları** belirli bir albümü hakkında bilgi görüntülemek için görünümü. Bu uygulamalı laboratuar ortamında her şeyi hakkında albüm görüntülenir zaten bulunan **Görünüm** şablonu. Bunu oluşturmak yerine bir **StoreDetailsViewModel** sınıfı, geçerli kullanacağınız **StoreBrowseViewModel** albümü kendisine geçirme şablonu.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Yeni bir ekleme **ayrıntıları** görüntüleme **StoreController**'s **ayrıntıları** eylem yöntemi. Bunu yapmak için sağ tıklatın **ayrıntıları** yönteminde **StoreController** sınıfı ve tıklayın **Görünüm Ekle**.
2. İçinde **Görünüm Ekle** iletişim kutusunda, doğrulayın **Görünüm adı** olan **ayrıntıları**. Denetleyin **kesin türü belirtilmiş görünüm oluşturma** onay kutusunu seçip **albüm** gelen **Model sınıfı** açılır. Diğer alanları varsayılan değerlerine bırakın. Ardından **Ekle**. Bu oluşturun ve açık bir **\Views\Store\Details.cshtml** dosya.

    ![Ayrıntılar görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image31.png "Ayrıntılar görünümü ekleme")

    *Ayrıntılar görünümü ekleme*
3. Değiştirme **Details.cshtml** albüm bilgileri görüntülemek için dosya erişme **albüm** şablonu görüntüleme geçirilen nesne. Bunu yapmak için içerik şununla değiştirin: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>7 - uygulama çalışan görev

Bu görevde, test **ayrıntıları** görünümü alır albüm bilgilerinden **ayrıntıları eylem** yöntemi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlayacağını **giriş** sayfası. URL'ye değiştirin **/Store/Details/5** albüm bilgilerini doğrulamak için.

    ![Albümleri ayrıntı gözatma](aspnet-mvc-4-fundamentals/_static/image32.png "albümleri ayrıntı gözatma")

    *Albüm ayrıntı gözatma*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Görev 8 - sayfaları arasında bağlantılar ekleme

Bu görevde her Tarz adı uygun bir bağlantı sağlamak için depolama görünümünde bir bağlantı ekleyeceksiniz **/deposu/Gözat** URL. Bu şekilde, bir tarzını üzerinde örneği için tıklattığınızda **DISCO**, gider **/deposu/Gözat? Tarz DISCO =** URL.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Güncelleştirme **dizin** bağlantısı eklemek için sayfa **Gözat** sayfası. Bunu yapmak için **Çözüm Gezgini** genişletin **görünümleri** klasörü, sonra **deposu** klasörü ve çift **Index.cshtml** sayfası.
2. Bir bağlantı seçili Tarz belirten Gözat görünümüne ekleyin. Bunu yapmak için aşağıdaki vurgulanmış kodu içinde yerini  **&lt;li&gt;**  etiketler: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > başka bir yaklaşım, aşağıdaki gibi bir kodla, doğrudan sayfasına bağlantılandırma:
    > 
    > &lt;bir href =&quot;/deposu/Gözat? Tarz =@genreName&quot;&gt;@genreName&lt;/a&gt;
    > 
    > Bu yaklaşım kullanılabilse de, bir sabit kodlanmış dize bağlıdır. Daha sonra denetleyicisi yeniden adlandırırsanız, bu yönerge el ile değiştirmeniz gerekecektir. Daha iyi bir alternatif kullanmaktır bir **HTML Yardımcısı** yöntemi. ASP.NET MVC gibi görevler için kullanılabilir olan bir HTML yardımcı yöntemi içerir. **Html.ActionLink()** yardımcı yöntem HTML oluşturmanızı kolaylaştırır  **&lt;bir&gt;**  bağlantılar, URL yollarını URL kodlanmış düzgünce emin olun.
    > 
    > Htlm.ActionLink birçok aşırı yüklemeye sahip. Bu alıştırmada üç parametre alır birini kullanır:
    > 
    > 1. Bağlantı metnini, tarz adını görüntüler
    > 2. Denetleyici eylem adı (**Gözat**)
    > 3. Rota parametre değerleri, hem adı belirtme (**Tarz**) ve değeri (**Tarz adı**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>9 - uygulama çalışan görev

Bu görevde her bir tarzını uygun bir bağlantı görüntülenir sınayacak **/deposu/Gözat** URL.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/deposu** her bir tarzını uygun bağlantıları doğrulamak için **/deposu/Gözat** URL.

    ![Türler Gözat sayfasına bağlantı gözatma](aspnet-mvc-4-fundamentals/_static/image33.png "Gözat sayfasına bağlantılarla gözatma türler")

    *Türler Gözat sayfasına bağlantı gözatma*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>10 - değerleri geçirmek için dinamik ViewModel koleksiyonu kullanarak görev

Bu görevde, modelde değişiklik yapmadan değerleri denetleyici ve görünüm arasında geçirmek için basit ve güçlü bir yöntemdir öğreneceksiniz. ASP.NET MVC 4 sağlar koleksiyon &quot;ViewModel&quot;, hangi herhangi bir dinamik değer atanabilir ve denetleyicileri ve görünümleri de içinde erişilebilir.

Şimdi ViewBag dinamik koleksiyon listesi geçirmek için kullanacağınız &quot; **Starred türler** &quot; görünümüne denetleyicisinden. Depolama dizini görünümü için erişecek **ViewModel** ve bilgileri görüntüler.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Açık **StoreController.cs** ve değiştirme **dizin** ViewModel koleksiyona listesini oluşturmak için yöntem starred türler:


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Sözdizimi da kullanabilirsiniz **ViewBag [&quot;Starred&quot;]** özelliklerine erişmek için.
2. Yıldız simgesine  **&quot;starred.png&quot;**  dahil **Source\Assets\Images** bu laboratuvarı klasör. Uygulama eklemek için bunların içerikten sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** Express'te Visual Web Developer aşağıda gösterildiği gibi:

    ![Çözüme yıldız görüntü ekleme](aspnet-mvc-4-fundamentals/_static/image34.png "çözüme yıldız görüntü ekleme")

    *Çözüme yıldız görüntü ekleme*
3. Görünümü açma **Store/Index.cshtml** ve içeriği değiştirebilirsiniz. Okuma yapacak &quot;starred&quot; özelliğinde **ViewBag** koleksiyonu ve geçerli bir tarzını adı listede olup olmadığını isteyin. Bu durumda Tarz bağlantısına sağ yıldız simgesiyle gösterilir.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>11 - uygulama çalışan görev

Bu görevde yıldızlanmıştır türler yıldız simgesiyle görüntülemek test edeceğiz.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlayacağını **giriş** sayfası. URL'ye değiştirin **/deposu** her öne çıkan bir tarzını saygı etiket olduğunu doğrulayın:

    ![Türler yıldızlanmıştır öğeleriyle gözatma](aspnet-mvc-4-fundamentals/_static/image35.png "yıldızlanmıştır öğelerle gözatma türler")

    *Yıldızlanmıştır öğelerle gözatma türler*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Alıştırma 7: ASP.NET MVC 4 yeni şablonu çevresinde bir diz

Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri göz en ilgili özellikleri yeni şablon alma inceleyeceksiniz.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Görev 1: ASP.NET MVC 4 Internet uygulama şablonu keşfetme

1. Zaten açık değilse, başlangıç **VS Express Web**
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması**. **Ad** proje *MusicStore* ve güncelleştirme **çözüm adı** için *başlamak*, ardından bir konum seçin (veya varsayılan adı bırakın) tıklatıp**Tamam**.

    ![Yeni bir ASP.NET MVC 4 proje oluşturma](aspnet-mvc-4-fundamentals/_static/image36.png "yeni bir ASP.NET MVC 4 proje oluşturma")

    *Yeni bir ASP.NET MVC 4 proje oluşturma*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**. Görünüm altyapısı olarak Razor veya ASPX seçebilirsiniz dikkat edin.

    ![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](aspnet-mvc-4-fundamentals/_static/image37.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")

    *Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*

    > [!NOTE]
    > Razor sözdizimi ASP.NET MVC 3'te sunulmuştur. Karakterler ve gerekli bir dosya bir hızlı ve iş akışı kodlama sıvı etkinleştirme tuş vuruşları sayısını en aza indirmek için kendi hedeftir. Razor yararlanır varolan C# /VB (veya diğer) dil becerileri ve harika bir HTML oluşturma iş akışı sağlayan bir şablon biçimlendirme sözdizimi sunar.
4. Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonu görmek için. Aşağıdaki özellikleri denetleyebilirsiniz:

    1. **Modern stili şablonları**

        Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.

        ![ASP.NET MVC 4 stili şablonları](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC şablonları Stili 4")

        *ASP.NET MVC 4 stili şablonları*
    2. **Uyarlamalı işleme**

        Tarayıcı penceresini yeniden boyutlandırma çıkışı denetleyin ve nasıl sayfa düzeni yeni pencere boyutunu dinamik olarak uyum dikkat edin. Bu şablonlar, hem Masaüstü hem de mobil platformları hiçbir özelleştirme gerektirmeden düzgün işlenecek Uyarlamalı işleme tekniği kullanın.

        ![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](aspnet-mvc-4-fundamentals/_static/image39.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")

        *ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda*
5. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
6. Şimdi çözümü keşfedin ve ASP.NET MVC 4 proje şablonu içinde sunulan yeni özelliklerden bazıları göz atın.

    ![ASP.NET MVC4-Internet-uygulama-proje-şablonu](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")

    *ASP.NET MVC 4 Internet uygulaması proje şablonu*

    1. **HTML5 biçimlendirme**

        Şablon görünümleri yeni temayı işaretleme, örneğin açık bulmak için Gözat **About.cshtml** görünümü **giriş** klasör.

        ![Razor ve HTML5 biçimlendirme kullanarak yeni şablonu](aspnet-mvc-4-fundamentals/_static/image41.png "Razor ve HTML5 biçimlendirme kullanarak yeni şablonu")

        *Razor ve HTML5 biçimlendirme kullanarak yeni şablonu*
    2. **JavaScript kitaplıklarını dahil**

        1. **jQuery**: jQuery HTML belge çapraz geçiş yapma, olay işleme, animasyon ve Ajax etkileşimleri basitleştirir.
        2. **jQuery UI**: Bu kitaplık için alt düzey etkileşim ve etkileri Gelişmiş animasyon ve bölümlerinin tema eklenebilir widgets, jQuery JavaScript kitaplığı üzerine inşa soyutlamalar sağlar.

            > [!NOTE]
            > JQuery ve jQuery UI hakkında bilgi edinin içinde [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
        3. **Çakıştırmaları**: ASP.NET MVC 4 varsayılan şablonu artık içerir **Çakıştırmaları**, JavaScript ve HTML kullanarak zengin ve hızlı yanıt veren web uygulamaları oluşturmanıza olanak sağlayan bir JavaScript MVVM çerçevesi. Gibi ASP.NET MVC 3'te, jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te bulunur.

            > [!NOTE]
            > Bu bağlantıyı Çakıştırmaları kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
        4. **Modernizr**: sitenizi HTML5 ve CSS3 teknolojiler kullanılırken eski tarayıcılarla uyumlu hale getirme bu kitaplığı otomatik olarak çalıştırır.

            > [!NOTE]
            > Bu bağlantıyı Modernizr kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [http://www.modernizr.com/](http://www.modernizr.com/).
    3. **Çözümdeki SimpleMembership**

        SimpleMembership, önceki ASP.NET rol ve üyelik sağlayıcısı sistem için bir yedek olarak tasarlanmıştır. Güvenli web sayfalarına geliştirici için daha esnek bir şekilde kolaylaştıran birçok yeni özellik vardır.

        Internet şablonu SimpleMembership tümleştirmek için birkaç şey zaten ayarlanmış, örneğin, AccountController OAuthWebSecurity (için OAuth hesap kaydı, oturum açma, yönetim, vb.) ve Web güvenlik kullanmak için hazırlanır.

        ![Çözüm SimpleMembership dahil](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dahil çözümü")

        *Çözüm SimpleMembership dahil*

        > [!NOTE]
        > Hakkında daha fazla bilgi bulmak [OAuthWebSecurity](https://msdn.microsoft.com/en-us/library/jj158393(v=vs.111).aspx) MSDN'de.

> [!NOTE]
> Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı Laboratuvar tamamlayarak ASP.NET MVC ile ilgili temel bilgileri öğrendiniz:

- Bir MVC uygulaması ve nasıl etkileşim kurduklarını çekirdek öğeleri
- Bir ASP.NET MVC uygulaması oluşturma
- URL ve sorgu dizesi eklemek ve parametreleri işlemek için denetleyicileri yapılandırmak nasıl geçirildi
- Ortak HTML içerik, Görünüm ve HTML içeriğini görüntülemek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurulumu için bir düzen ana sayfası ekleme
- Dinamik bilgileri görüntülemek için Görünüm şablonu özellikleri geçirmesi ViewModel desen kullanma
- Görünüm şablonunda denetleyicilerine geçirilen parametrelerini kullanma
- ASP.NET MVC uygulaması içindeki sayfalar için bağlantılar ekleme
- Ekleme ve bir görünümde dinamik özelliklerini kullanın
- ASP.NET MVC 4 proje şablonları'deki geliştirmeler

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-fundamentals/_static/image43.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Windows Azure portalında oturum açtığı](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açtığı*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image49.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image50.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-fundamentals/_static/image51.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](aspnet-mvc-4-fundamentals/_static/image52.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-fundamentals/_static/image53.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-fundamentals/_static/image54.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-fundamentals/_static/image55.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](aspnet-mvc-4-fundamentals/_static/image56.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-fundamentals/_static/image58.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Değişiklikleri onaylamak*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-fundamentals/_static/image60.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-fundamentals/_static/image61.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-fundamentals/_static/image62.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-fundamentals/_static/image63.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

    - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
    - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
    - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
    - Yeni bir veritabanı adı girin: *MVC4SampleDB*.

    ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-fundamentals/_static/image64.png "hedef bağlantı dizesi yapılandırma")

    *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-fundamentals/_static/image65.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-fundamentals/_static/image66.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-fundamentals/_static/image67.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

    ![Uygulama için Windows Azure yayımlanan](aspnet-mvc-4-fundamentals/_static/image68.png "uygulama için Windows Azure yayımlanır")

    *Windows Azure için yayımlanan uygulama*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-fundamentals/_static/image69.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-fundamentals/_static/image70.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-fundamentals/_static/image71.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-fundamentals/_static/image72.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-fundamentals/_static/image73.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-fundamentals/_static/image74.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
