---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 temelleri | Microsoft Docs
author: rick-anderson
description: Bu uygulamalı laboratuvarı MVC (Model View Controller) müzik Store tanıtır ve nasıl ASP.NET MV kullanılacağını adım adım anlatan bir öğretici uygulama alan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386581"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 temelleri

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

Bu uygulamalı laboratuvarı MVC (Model View Controller) müzik Store tanıtır ve ASP.NET MVC ve Visual Studio'nun nasıl kullanılacağını adım adım anlatan bir öğretici uygulama temel alır. Laboratuvar Basitlik öğreneceksiniz. Bu teknolojiler birlikte kullanarak, henüz güç. Basit bir uygulamayla başlar ve tam işlevsel bir ASP.NET MVC 4 Web uygulaması elde edene kadar oluşturacaksınız.

Bu Laboratuvar, ASP.NET MVC 4 ile çalışır.

Öğretici uygulama ASP.NET MVC 3 sürümünü keşfedin istiyorsanız, bunu bulabilirsiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).

Bu uygulamalı Laboratuvar, geliştirici deneyimi HTML ve JavaScript gibi Web geliştirme teknolojilerine olduğu varsayılır.

> [!NOTE]
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 Temelleri](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Müzik Store uygulaması

Bu Laboratuvar içinde oluşturulacak müzik Store web uygulaması üç ana bölümden oluşur: alışveriş, kullanıma alma ve yönetim. Ziyaretçi albümleri türe göre göz atın, albümleri, Sepete Ekle, seçimi gözden geçirin ve son oturum açma ve sırasını tamamlamak için kullanıma devam etmek mümkün olacaktır. Ayrıca, depolama yöneticileri kullanılabilir albümleri, hem de ana özelliklerini yönetmek mümkün olacaktır.

![Müzik Store ekranlar](aspnet-mvc-4-fundamentals/_static/image1.png "müzik Store ekranları")

*Müzik Store ekranları*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 temelleri

Müzik Store uygulaması kullanarak oluşturulur **Model View Controller (MVC)**, uygulamanın üç ana bileşene ayıran bir mimari deseni:

- **Modelleri**: Model nesneleri, etki alanının mantığını uygulayan uygulamanın parçaları. Genellikle model nesneleri ayrıca almak ve model durumu bir veritabanında depolar.
- **Görünümler:** görünümler, uygulamanın kullanıcı arabirimini (UI) görüntüleyen bileşenleridir. Genellikle, bu UI model verilerinden oluşturulur. Örnek metin kutuları ve albüm nesnenin geçerli durumuna bağlı açılan listesini görüntüleyen albümleri düzenleme görünümü olacaktır.
- **Denetleyicileri:** denetleyicileri, kullanıcı etkileşimi işlemek, modelle ve sonuçta UI işlemek için bir görünüm seçin bileşenleridir. Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir.

MVC örüntüsü, öğeler arasında gevşek bir bağlantı sağlayarak uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini birbirlerinden ayıran uygulamalar oluşturmanıza yardımcı olur. Bu ayrım bir uygulama oluşturduğunuzda uygulamasının aynı anda tek bir yönüne odaklanma sağladığından karmaşıklığını yönetmenize yardımcı olur. Ayrıca, MVC örüntüsü, aynı zamanda teste dayalı geliştirme (TDD) uygulamaları oluşturmak için kullanımı ve uygulamaları test etmek kolaylaştırır.

**ASP.NET MVC** framework, ASP.NET MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sağlar. **ASP.NET MVC** çerçevedir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web forms tabanlı uygulamaları olduğu gibi) mevcut ASP.NET özellikleriyle gibi ana sayfalar ve üyelik tabanlı Tümleşik temel .NET framework'ün tüm gücüne sahip olursunuz, böylece kimlik doğrulaması. Kullanmakta olduğunuz tüm kitaplıkları da ASP.NET MVC 4'te kullanılabilir olmadığından, ASP.NET Web Forms ile bilginiz varsa, bu yararlıdır.

Ayrıca, bir MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ paralel gelişimi de kolaylaştırır. Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici modelinde iş mantığı üzerinde odaklanabilir.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Sıfırdan müzik Store uygulaması Öğreticisi üzerinde bir ASP.NET MVC uygulaması oluşturma
- Giriş sayfasında sitenin ve ana işlevselliğini gözatma için URL'leri işlemek için denetleyicileri ekleme
- Stilinin birlikte görüntülenen içerik özelleştirmek için bir Görünüm Ekle
- Model sınıfları içeren ve veri ve etki alanı mantığı yönetmek için ekleyin
- Denetleyici eylemlerine görünüm şablonları için bilgi geçirmek görünüm modeli düzeni kullanın.
- ASP.NET MVC 4 yeni şablon Internet uygulamaları için keşfetmek

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:

- [Web için Visual Studio 2012 Express](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

**Kod parçacıkları yükleniyor**

Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir. Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.

Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek C: kullanarak kod parçacıklarına](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:

1. [Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma](#Exercise1)
2. [Alıştırma 2: Denetleyici oluşturma](#Exercise2)
3. [Alıştırma 3: bir denetleyiciye parametreleri geçirme](#Exercise3)
4. [Alıştırma 4: bir görünüm oluşturma](#Exercise4)
5. [Alıştırma 5: bir görünüm modeli oluşturma](#Exercise5)
6. [Alıştırma 6: görünüm parametrelerini kullanma](#Exercise6)
7. [Alıştırma 7: ASP.NET MVC 4 yeni şablonu turu](#Exercise7)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma

Bu alıştırmada, bir ASP.NET MVC uygulamasını Visual Studio 2012 Express kendi ana klasörü kuruluş yanı sıra Web için nasıl oluşturulacağını öğreneceksiniz. Ayrıca, yeni bir denetleyici ekleyeceksiniz ve basit bir dize uygulamanın giriş sayfasında görüntülenen kolaylaştırmak nasıl öğreneceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Görev 1 - ASP.NET MVC Web uygulaması projesi oluşturma

1. Bu görevde, Visual Studio MVC şablonu kullanarak boş bir ASP.NET MVC uygulaması projesi oluşturur. Başlangıç **Web için VS Express**.
2. Üzerinde **dosya** menüsünü tıklatın **yeni proje**.
3. İçinde **yeni proje** iletişim kutusu seç **ASP.NET MVC 4 Web uygulaması** proje türü, altında bulunan **Visual C#** **Web** şablonu Liste.
4. Değişiklik **adı** için *MvcMusicStore*.
5. İçinde yeni bir çözüm konumunu ayarlayın **başlamak** klasöründe Bu alıştırmada'nın kaynak, örneğin **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. **Tamam**'ı tıklatın.

    ![Yeni Proje iletişim kutusu oluşturma](aspnet-mvc-4-fundamentals/_static/image2.png "yeni proje iletişim kutusu oluşturma")

    *Yeni Proje iletişim kutusu oluşturma*
6. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **temel** şablon emin olun **görünüm altyapısı** seçilmişse **Razor**. **Tamam**'ı tıklatın.

    ![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image3.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")

    *Yeni ASP.NET MVC 4 Proje iletişim kutusu*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Görev 2 - çözüm yapısını keşfetme

ASP.NET MVC çerçevesi, MVC örüntüsünü destekleyen Web uygulamaları oluşturmanıza yardımcı olan bir Visual Studio Proje şablonu içerir. Bu şablon, gerekli klasörleri, öğe şablonları ve yapılandırma dosyası girdileri ile yeni bir ASP.NET MVC Web uygulaması oluşturur.

Bu görevde, söz konusu olan öğeleri anlamak için çözüm yapısını ve ilişkilerini inceleyeceksiniz. ASP.NET MVC çerçevesi varsayılan olarak kullandığı için aşağıdaki klasörleri tüm ASP.NET MVC uygulamasında bulunan bir &quot;kuralı yapılandırmanız üzerinde&quot; yaklaşım ve bazı varsayılan varsayımlara temel klasör adlandırma üzerinde yapar kuralları.

1. Proje oluşturulduktan sonra Çözüm Gezgini'nde sağ tarafındaki oluşturulan klasör yapısını inceleyin:

    ![ASP.NET MVC klasör yapısını Çözüm Gezgini'nde](aspnet-mvc-4-fundamentals/_static/image4.png "Çözüm Gezgini'nde ASP.NET MVC klasör yapısı")

    *Çözüm Gezgini'nde ASP.NET MVC klasör yapısı*

   1. **Denetleyicileri**. Bu klasör denetleyicisi sınıfları içerir. Son kullanıcı etkileşimini işlemedeki, modeli düzenleme ve sonuçta görünümü kullanıcı arabirimini seçmek için bir temel MVC uygulamasındaki denetleyicileri sorumludur.

       > [!NOTE]
       > MVC çerçevesi ile bitmesi tüm denetleyicileri adlarını gerektiren &quot;denetleyicisi&quot;-Örneğin, HomeController, LoginController veya ProductController.
   2. **Modelleri**. Bu klasör, MVC Web uygulaması için uygulama modelini temsil eden sınıflar sağlar. Bu genellikle, nesneleri ve veri deposu ile etkileşim kurmak için mantığı tanımlayan kodu içerir. Genellikle, gerçek model nesneleri ayrı sınıf kitaplıkları olacaktır. Ancak, yeni bir uygulama oluşturduğunuzda, sınıfları ve bunları daha sonraki bir noktada ayrı sınıf kitaplıkları olarak geliştirme döngüsünde taşırsanız.
   3. **Görünümleri**. Bu klasör, görünümler, uygulamanın kullanıcı arabirimini görüntülemek için sorumlu bileşenleri için önerilen konumdur. İşleme Görünümlerle ilgili diğer tüm dosyaları ek olarak, .aspx, .ascx, .cshtml ve .master dosyaları görünümlerini kullanın. Görünüm klasörü her denetleyici için bir klasör içerir; Bu klasör, denetleyici adı ön ekine sahip olarak adlandırılır. Örneğin, adında bir denetleyici varsa **HomeController**, görünümleri klasör giriş adlı bir klasör içerir. ASP.NET MVC çerçevesi bir görünüm yüklendiğinde varsayılan olarak, Views\controllerName klasöründe istenen görünüm ada sahip .aspx dosyası arar (**görünümler [ControllerName] [Action] .aspx**) veya (**görünümler [ControllerName] [Action] .cshtml**) Razor görünümleri için.

      > [!NOTE]
      > Daha önce listelenen klasörler ek olarak, bir MVC Web uygulaması kullanan **Global.asax** genel URL yönlendirmeyi ayarlamak için dosyası varsayılan olarak ve kullanır **Web.config** uygulama yapılandırma dosyası.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Görev 3 - bir HomeController ekleme

MVC çerçevesi kullanmayan ASP.NET uygulamalarında kullanıcı etkileşimi sayfaların çevresine ve oluşturma ve bu sayfalarındaki olayları işleme düzenlenmiştir. Buna karşılık, ASP.NET MVC uygulamaları ile kullanıcı etkileşimi denetleyicileri ve kendi eylem yöntemlerini düzenlenmiştir.

Öte yandan, ASP.NET MVC çerçevesi denetleyicileri olarak başvurulan sınıfların URL'leri eşler. Denetleyicileri gelen istekleri işleyemedi, kullanıcı girişini ve etkileşimler, uygun uygulama mantığı yürütün ve istemciye geri gönderilecek yanıt belirlemek (HTML görüntülemek, dosya indirme, farklı bir URL vb. için yeniden yönlendirme). Denetleyici sınıfı HTML görüntüleme söz konusu olduğunda, isteği için HTML biçimlendirmesi oluşturmak için ayrı görünümü bileşen genellikle çağırır. Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir.

Bu görevde, giriş sayfasına müzik Store sitesinin URL'leri işleyecek bir denetleyici sınıfı ekler.

1. Sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu:

    ![Denetleyici komut ekleme](aspnet-mvc-4-fundamentals/_static/image5.png "denetleyicisi komut ekleme")

    *Denetleyici komut ekleme*
2. **Denetleyici Ekle** iletişim kutusu görüntülenir. Denetleyici adı *HomeController* basın **Ekle**.

    ![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image6.png "denetleyici Ekle iletişim kutusu")

    *Denetleyici Ekle iletişim kutusu*
3. Dosya **HomeController.cs** oluşturulur **denetleyicileri** klasör. Sahip olmak için **HomeController** değiştirin, dizin eylem üzerinde bir dize döndürecek **dizin** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex1 HomeController dizin*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında çalışır.

1. Tuşuna **F5** uygulamayı çalıştırın. Proje derlendiğinde ve yerel IIS Web sunucusu başlatır. Yerel IIS Web sunucusu otomatik Web sunucusunun URL'sine bir web tarayıcısı açılır.

    ![Bir web tarayıcısında çalışan uygulama](aspnet-mvc-4-fundamentals/_static/image7.png "bir web tarayıcısında çalışan uygulama")

    *Bir web tarayıcısında çalışan uygulama*

    > [!NOTE]
    > Yerel IIS Web sunucusunda bir rastgele ücretsiz bağlantı noktası numarası Web sitesinin çalışır. Yukarıdaki şekilde, sitesinin çalışır durumda `http://localhost:50103/`, bağlantı noktası 50103 kullanıyor. Bağlantı noktası numaranızı farklılık gösterebilir.
2. Tarayıcıyı kapatın.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Alıştırma 2: Denetleyici oluşturma

Bu alıştırmada, müzik Store uygulamasının basit işlevselliği uygulamak için denetleyici güncelleştirme öğreneceksiniz. Söz konusu denetleyici eylem yöntemleri, her biri aşağıdaki belirli isteklerini işlemek için tanımlar:

- Müzik Store içinde müzik türleri listesi sayfası
- Tüm için belirli bir türe müzik albümleri listeleyen bir göz atma sayfasından
- Belirli bir müzik Albüm hakkında bilgi gösteren Ayrıntılar sayfası

Bu alıştırmada kapsam için bu eylemleri yalnızca artık bir dize döndürür.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Görev 1 - yeni StoreController sınıf ekleme

Bu görevde, yeni bir denetleyici ekleyeceksiniz.

1. Açık değilse, başlangıç **VS Express Web 2012**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex02 CreatingAController\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Yeni denetleyici ekleyin. Bunu yapmak için sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu. Değişiklik **Denetleyici adı** için *StoreController*, tıklatıp **Ekle**.

    ![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image8.png "denetleyici Ekle iletişim kutusu")

    *Denetleyici Ekle iletişim kutusu*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Görev 2 - StoreController'ın eylemleri değiştirme

Bu görevde, çağrılan denetleyici yöntemlerinde değiştirecek **eylemleri**. Eylemler, URL isteklerini işleme ve tarayıcı veya URL çağrılan kullanıcı geri gönderilmesi gereken içeriği belirleyen sorumludur.

1. **StoreController** sınıfı zaten bir **dizin** yöntemi. Müzik deposu tüm türleri listeler sayfası uygulamak için daha sonra bu laboratuvarda bu kullanır. Şimdilik yalnızca değiştirin **dizin** yöntemini aşağıdaki kodla bir dize döndürür &quot;Hello Store.Index() gelen&quot;:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController dizin*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Ekleme **Gözat** ve **ayrıntıları** yöntemleri. Bunu yapmak için aşağıdaki kodu ekleyin. **StoreController**:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında çalışır.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlar **giriş** sayfası. Her eylemin uygulama doğrulamak için URL'yi değiştirin.

    1. **/ Store**. Göreceğiniz  **&quot;Hello Store.Index() gelen&quot;**.
    2. **/ Store/Gözat**. Göreceğiniz  **&quot;Hello Store.Browse() gelen&quot;**.
    3. **/ Store/Details**. Göreceğiniz  **&quot;Hello Store.Details() gelen&quot;**.

        ![StoreBrowse gözatma](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse gözatma")

        *Gözatma /Store/Browse*
3. Tarayıcıyı kapatın.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Alıştırma 3: bir denetleyiciye parametreleri geçirme

Şimdiye kadar sabit dizelerini denetleyicilerinden iade. Bu alıştırmada, parametreleri URL ve sorgu dizesi kullanılarak ve sonra metinle tarayıcıya yanıt yöntemi eylemleri yapma denetleyicisi nasıl geçireceğinizi öğreneceksiniz.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Görev 1 - StoreController Tarz parametresi ekleme

Bu görevde kullanacağınız **querystring** parametreleri göndermek için **Gözat** eylem yönteminde **StoreController**.

1. Açık değilse, başlangıç **Web için VS Express**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex03 PassingParametersToAController\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Açık **StoreController** sınıfı. Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.
4. Değişiklik **Gözat** için belirli bir türe istemek için bir dize parametresi ekleme yöntemi. ASP.NET MVC otomatik olarak herhangi bir sorgu dizesi geçti veya form gönderme parametreleri adlı **Tarz** çağrıldığında Bu eylem yöntemine. Bunu yapmak için değiştirin **Gözat** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Kullanmakta olduğunuz **HttpUtility.HtmlEncode** yardımcı yöntemine görünüme Javascript ekleme gibi bir bağlantı içeren gelen kullanıcıları engeller   **/Store/göz atma? Tarz =&lt;betik&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/SCRIPT&gt;**.
> 
> Daha fazla açıklama için lütfen [bu msdn makalesinde](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulamayı çalıştırma

Bu görevde, bir web tarayıcısında uygulama denemek ve kullanmak **Tarz** parametresi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlar **giriş** sayfası. URL'yi   */Store/Gözat? Tarz DISCO =* eylemi Tarz parametresi aldığını doğrulamak için.

    ![StoreBrowseGenre gözatma DISCO =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre gözatma DISCO =")

    *Gözatma /Store/Browse? Tarz DISCO =*
3. Tarayıcıyı kapatın.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Görev 3 - URL'de katıştırılmış bir kimliği parametre ekleme

Bu görevde kullanacağınız **URL** geçirilecek bir **kimliği** parametresi **ayrıntıları** eylem yöntemi **StoreController**. ASP.NET MVC'nin varsayılan yönlendirme kuralı olan bir URL kesimi sonra eylem yöntemi adının adlı bir parametre olarak değerlendirilecek **kimliği**. Eylem yönteminizin kimliği adlı parametre varsa, sonra ASP.NET MVC otomatik olarak URL kesimi, parametre olarak geçirir. URL'deki **Store/Ayrıntılar/5**, **kimliği** olarak yorumlanacaktır **5**.

1. Değişiklik **ayrıntıları** yöntemi **StoreController**, ekleyerek bir **int** adlı parametreyi **kimliği**. Bunu yapmak için değiştirin **ayrıntıları** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, bir web tarayıcısında uygulama denemek ve kullanmak **kimliği** parametresi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlar **giriş** sayfası. URL'yi */Store/Details/5* eylem kimliği parametresi aldığını doğrulamak için.

    ![StoreDetails5 gözatma](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 gözatma")

    *Gözatma /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Alıştırma 4: bir görünüm oluşturma

Şu ana kadar dizeleri denetleyici eylemlerine iade. Denetleyicileri nasıl çalıştığını anlamak için kullanışlı bir yol olan rağmen değil gerçek Web uygulamalarınızı nasıl oluşturulur açıktır. Görünümleri kullanarak şablon dosyaları tarayıcıya HTML oluşturmak için daha iyi bir yaklaşım sunan bileşenlerdir.

Bu alıştırmada, ortak HTML içeriğini, görünümünü site ve son olarak HTML döndürülecek HomeController etkinleştirmek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurmak için bir düzen ana sayfasını nasıl ekleneceğini öğreneceksiniz.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Görev 1 - dosyasını değiştirerek \_layout.cshtml

Dosya **~/Views/Shared/\_layout.cshtml** tüm Web sitesi kullanmak üzere ortak HTML için bir şablon kurulum olanak tanır. Bu görevde giriş sayfası ve Store alanına bağlantılarla birlikte ortak bir üst bilgisi düzeni ana sayfayla ekleyeceksiniz.

1. Açık değilse, başlangıç **Web için VS Express**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex04 CreatingAView\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Dosya  <strong>\_layout.cshtml</strong> sitedeki tüm sayfalar için HTML kapsayıcı düzeni içerir. İçerdiği <strong>&lt;html&gt;</strong> öğe için HTML yanıtını yanı sıra <strong>&lt;baş&gt;</strong> ve <strong>&lt;gövdesi&gt;</strong> öğeleri. <strong>@RenderBody()</strong> HTML'yi gövdesi tanımlamak bölgeleri şablonları oluşturabileceksiniz oturum dinamik içerik doldurmak bu görünümü.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Giriş sayfası ve Store alanına sitesindeki tüm sayfalara bağlantılar içeren ortak bir üst bilgisi ekleyin. Bunu yapmak için aşağıdaki kodu ekleyin. &lt;gövdesi&gt; deyimi.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Her sayfanın gövde bölümü oluşturmak için bir sayı içerir. Değiştirin  <strong>@RenderBody()</strong> aşağıdaki higlighted kodla: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Biliyor muydunuz? Visual Studio 2012 HTML, kod dosyaları ve daha sık kullanılan kodu eklemek kolaylaştıran parçacıkları var! Out yazarak deneme **&lt;div&gt;** tuşuna basarak **sekmesini** iki kez tam eklemek için **div** etiketi.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Görev 2 - CSS stil ekleme

Boş proje şablonu, yalnızca temel formlar ve doğrulama iletileri görüntülemek için kullanılan stilleri içeren çok kolaylaştırılmış bir CSS dosyası içerir. Site Azure görünümü ve deneyimini geliştirmek için ek CSS ve görüntüleri (büyük olasılıkla bir tasarımcı tarafından sağlanan) kullanır.

Bu görevde, site stillerini tanımlamak için bir CSS stil sayfası ekler.

1. CSS dosyası ve görüntüler dahil edilen **Source\Assets\Content** , bu Laboratuvarın klasör. Uygulamanıza bunları eklemek için bunların içeriğini sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** aşağıda gösterildiği gibi Web için Visual Studio Express'te:

    ![Stil içeriğini sürükleyerek](aspnet-mvc-4-fundamentals/_static/image12.png "stil içeriğini sürükleme")

    *Stil içeriğini sürükleme*
2. Bir uyarı iletişim kutusu, onay değiştirilecek istenecektir **Site.css** dosyası ve bazı mevcut görüntüler. Denetleme **tüm öğelere uygula** tıklatıp **Evet**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Görev 3 - bir görünüm şablonu ekleme

Bu görevde, şablonu Düzen ana sayfası kullanacağınız HTML yanıtı oluşturmak için görüntüleme ekleyecek ve bu alıştırmada, CSS eklendi.

1. Sitenin ana sayfasını göz atarken görünüm şablonu kullanmak için önce bir dizeyi döndürmek yerine belirtmek gerekir **HomeController dizin** yöntemi döndürür bir **görünümü**. Açık **HomeController** sınıfı ve değiştirmek, **dizin** döndürülecek yöntemi bir **actionresult öğesini**, ve dönüş **View()**.

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex4 HomeController dizin*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Şimdi, uygun bir şablonu görüntüleme eklemeniz gerekir. Bunu yapmak için **sağ** içinde **dizin** eylem yöntemini seçip alt **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

    ![Index yöntemi içinden bir görünümle ekleme](aspnet-mvc-4-fundamentals/_static/image13.png "dizin metodundaki bir görünümden ekleme")

    *Index yöntemi içinden bir görünümle ekleme*
3. **Görünüm Ekle** görünümü şablon dosyası oluşturmak için iletişim kutusu görüntülenir. Varsayılan olarak, bu iletişim kutusunu görüntüle şablonunun adını kullanacak olan eylem yönteminin eşleşmesi önceden doldurur. Nedeniyle **Görünüm Ekle** bağlam menüsü içinde **dizin** HomeController içinde eylem yönteminin **Görünüm Ekle** iletişim varsayılan görünüm adını dizinine sahip. **Ekle**'yi tıklatın.

    ![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image14.png "Görünüm Ekle iletişim kutusu")

    *Görünüm Ekle iletişim kutusu*
4. Visual Studio'nun oluşturduğu bir **Index.cshtml** görünümü şablon içinde **görünümler/giriş** klasörü ve onu açar.

    ![Oluşturulan dizini görünüm ev](aspnet-mvc-4-fundamentals/_static/image15.png "oluşturulan giriş dizini görünümü")

    *Oluşturulan giriş dizini görünümü*

    > [!NOTE]
    > ad ve konum **Index.cshtml** dosya geçerlidir ve varsayılan ASP.NET MVC adlandırma kurallarını izler.
    > 
    > Klasör \Views\**giriş** denetleyici adıyla eşleşen (**giriş** denetleyicisi). Görünüm şablonu adı (**dizin**), görünüm görüntüleme denetleyici eylem yöntemine eşleşir.
    > 
    > Bu şekilde, bir görünüme dönmek için bu adlandırma kuralını kullanırken adına veya konumuna bir görünüm şablonunun açıkça belirtmek zorunda ASP.NET MVC önler.
5. Oluşturulan görünüm şablon temel  **\_layout.cshtml** önceden tanımlı bir şablon. ViewBag.Title özelliğini güncelleştirme **giriş**, ana içeriğe değiştirip **bu giriş sayfasıdır**, aşağıdaki kodda gösterildiği gibi:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Seçin **MvcMusicStore** tuşuna basın ve Çözüm Gezgini projedeki **F5** uygulamayı çalıştırın.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Görev 4: doğrulama

Önceki alıştırmada tüm adımları doğru olarak yaptığınızdan doğrulamak için aşağıda gösterildiği gibi ilerleyin:

Bir tarayıcıda açıldığından uygulama ile birlikte unutmamalısınız:

1. HomeController'ın dizin eylem yöntemi bulunamadı ve görüntülenen **\Views\Home\Index.cshtml** kodu adlı olsa bile görüntülemek şablon **View() dönüş**, ardından şablonu görüntüleme standart adlandırma kuralı.
2. Giriş sayfası içinde tanımlanan Hoş Geldiniz iletisi görüntüler **\Views\Home\Index.cshtml** şablonu görüntüle.
3. Giriş sayfasını kullanarak  **\_layout.cshtml** şablonu ve HTML standart site düzenine Hoş Geldiniz iletisi bulunur.

    ![Giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü](aspnet-mvc-4-fundamentals/_static/image16.png "giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü")

    *Giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Alıştırma 5: bir görünüm modeli oluşturma

Şu ana kadar sabit kodlanmış HTML görüntüleme Görünümlerinizde yapılan ancak dinamik web uygulamaları oluşturmak için şablonu görüntüle denetleyicisinden bilgi almanız gerekir. Bu amaçla kullanılmak üzere bir yaygın bir tekniktir **ViewModel** deseni, denetleyicinin uygun HTML yanıtı oluşturmak için gereken tüm bilgiler paketini sağlar.

Bu alıştırmada ViewModel sınıfı oluşturun ve gerekli özellikleri ekleme hakkında bilgi edineceksiniz: depolama ve bu tür listesi türleri sayısı. Ayrıca oluşturulan ViewModel kullanılacak StoreController güncelleştirir ve son olarak, belirtilen özellikleri sayfasında görüntülenecek yeni bir görünüm şablonu oluşturur.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Görev 1 - ViewModel sınıfı oluşturma

Bu görevde, Store Tarz listeleme senaryosu uygulayan bir ViewModel sınıfı oluşturur.

1. Açık değilse, başlangıç **Web için VS Express**.
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex05 CreatingAViewModel\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Oluşturma bir **Viewmodel'lar** ViewModel tutmak için klasör. Bunu yapmak için sağ üst düzey **MvcMusicStore** proje, select **Ekle** ve ardından **yeni klasör**.

    ![Yeni bir klasör ekleme](aspnet-mvc-4-fundamentals/_static/image17.png "yeni bir klasör ekleme")

    *Yeni bir klasör ekleme*
4. Klasör adı *Viewmodel'lar*.

    ![Çözüm Gezgini'nde Viewmodel'lar klasör](aspnet-mvc-4-fundamentals/_static/image18.png "Viewmodel'lar Çözüm Gezgininde klasör")

    *Çözüm Gezgini'nde Viewmodel'lar klasörü*
5. Oluşturma bir **ViewModel** sınıfı. Bunu yapmak için sağ **Viewmodel'lar** kısa süre önce oluşturduğunuz klasör seç **Ekle** ardından **yeni öğe**. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreIndexViewModel.cs*, ardından **Ekle**.

    ![Yeni sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image19.png "yeni sınıf ekleme")

    *Yeni sınıf ekleme*

    ![StoreIndexViewModel sınıfı oluşturma](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel oluşturma sınıfı")

    *StoreIndexViewModel sınıfı oluşturma*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Görev 2 - ViewModel sınıfı özellik ekleme

StoreController görünümü şablona beklenen HTML yanıtını oluşturmak için geçirilecek iki parametre vardır: depolama ve bu tür listesi türleri sayısı.

Bu görevde, 2 özelliklere ekleyeceksiniz **StoreIndexViewModel** sınıfı: **NumberOfGenres** (tamsayı) ve **türleri** (dizelerinin listesini).

1. Ekleme **NumberOfGenres** ve **türleri** özelliklerine **StoreIndexViewModel** sınıfı. Bunu yapmak için aşağıdaki 2 satırları sınıf tanımına ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreIndexViewModel özellikleri*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Alın; ayarlayın;}**  gösterimi yapar kullanmak C# otomatik uygulanan özellikler özelliği. Bir özelliğin avantajlarından, bize destek alanı bildirmek gerek kalmadan sağlar.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Görev 3 - güncelleştirme StoreController StoreIndexViewModel kullanmak için

**StoreIndexViewModel** sınıfı geçirmek için gereken bilgileri yalıtan **StoreController**'s **dizin** yöntemi için bir yanıt oluşturmak için bir şablonu görüntüleme .

Bu görevde, güncelleştirecektir **StoreController** kullanılacak **StoreIndexViewModel**.

1. Açık **StoreController** sınıfı.

    ![StoreController sınıfı açma](aspnet-mvc-4-fundamentals/_static/image21.png "açılış StoreController sınıfı")

    *Açılış StoreController sınıfı*
2. Kullanmak için **StoreIndexViewModel** gelen sınıfı **StoreController**, aşağıdaki ad üstüne ekleyin **StoreController** kod:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 Viewmodel'lar kullanarak StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Değişiklik **StoreController**'s **dizin** BT'nin oluşturur ve doldurur eylem yöntemi bir **StoreIndexViewModel** nesnesi ve ardından bunu bir şablonu görüntüle geçirir devre dışı ile bir HTML yanıtı oluşturur.

    > [!NOTE]
    > Laboratuvardaki &quot;ASP.NET MVC modeller ve veri erişimi&quot; depolama türleri listesini veritabanından alır, kod yazacaksınız. Aşağıdaki kodda, oluşturacağınız bir **listesi** doldurur amaçlı olarak sahte veri türleri, **StoreIndexViewModel**.
    > 
    > Oluşturma ve ayarlama sonra **StoreIndexViewModel** nesnesi, bu geçirilir bağımsız değişkeni olarak **görünümü** yöntemi. Bu görünüm şablonu ile bir HTML yanıtı oluşturmak için söz konusu nesne kullanacağını belirtir.
4. Değiştirin **dizin** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreController Index yöntemi*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> C# ile ilk kez kullanıyorsanız kullanmanın varsayabilir **var** anlamına **viewModel** geç bağlanan bir değişkendir. Doğru - C# derleyicisi kullanarak değişkene atayın temel tür çıkarımı, belirlemek için değil **viewModel** türünde **StoreIndexViewModel**. Ayrıca, yerel derleme tarafından **viewModel** değişken olarak bir **StoreIndexViewModel** get derleme zamanı denetimi ve Visual Studio Kod Düzenleyicisi desteği yazın.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Görev 4 - StoreIndexViewModel kullanan bir görünüm şablonu oluşturma

Bu görevde denetleyicisinden geçirilen bir StoreIndexViewModel nesne türleri listesini görüntülemek için kullanacağı bir görünüm şablonu oluşturacaksınız.

1. Yeni Görünüm şablonu oluşturmadan önce şimdi Projeyi derlemek için **Ekle iletişim kutusunu görüntüle** bildiği **StoreIndexViewModel** sınıfı. Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.

    ![Proje derleme](aspnet-mvc-4-fundamentals/_static/image22.png "projesi oluşturma")

    *Proje oluşturma*
2. Yeni bir görünüm şablonu oluşturun. İçinde Bunu yapmak için sağ **dizin** yöntemini seçip alt **Görünüm Ekle**.

    ![Görünüm ekleme](aspnet-mvc-4-fundamentals/_static/image23.png "görünüm ekleme")

    *Görünüm Ekleme*
3. Çünkü **Ekle iletişim kutusunu görüntüle** gelen çağrıldı **StoreController**, varsayılan olarak görünüm şablonu ekleyeceksiniz bir **\Views\Store\Index.cshtml** dosya. Denetleme **bir türü kesin belirlenmiş-görünüm oluşturma** onay kutusunu seçip **StoreIndexViewModel** olarak **Model sınıfı**. Ayrıca, seçili görünüm altyapısı olduğundan emin olun **Razor**. **Ekle**'yi tıklatın.

    ![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image24.png "Görünüm Ekle iletişim kutusu")

    *Görünüm Ekle iletişim kutusu*

    **\Views\Store\Index.cshtml** görünümü şablon dosyası oluşturulup açılır. Sağlanan bilgilere dayanarak **Görünüm Ekle** iletişim son adımda, şablonu beklediğiniz görünümü bir **StoreIndexViewModel** örnek olarak bir HTML yanıtı oluşturmak için kullanılacak veri. Şablon devralması fark edeceksiniz bir `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Görev 5 - görünüm şablonu güncelleştiriliyor

Bu görevde, türleri ve sayfa içindeki adları sayısını almak için son görevde oluşturduğunuz görünüm şablonu güncelleştirir.

> [!NOTE]
> Sözdizimi @ kullanır (genellikle olarak adlandırılan &quot;kod nuggets&quot;) görünüm şablonu içindeki kodu çalıştırmak için.

1. İçinde **Index.cshtml** içinde dosya **Store** klasöründe kodunu şununla değiştirin:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Tarz listesinde üzerinde döngü **StoreIndexViewModel** ve bir HTML oluşturmak **&lt;ul&gt;** kullanarak listesinde bir **foreach** döngü.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Tuşuna **F5** uygulamayı çalıştırın ve göz atmayı **/Store**. Geçirilen türleri listesini görürsünüz **StoreIndexViewModel** nesnesinden **StoreController** görünümü şablon.

    ![Görünüm türleri listesini görüntüleyen](aspnet-mvc-4-fundamentals/_static/image26.png "görünüm türleri listesini görüntüleme")

    *Görünüm türleri listesini görüntüleme*
4. Tarayıcıyı kapatın.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Alıştırma 6: Görünüm parametrelerini kullanma

Alıştırma 3'te denetleyiciye parametreler öğrendiniz. Bu alıştırmada, görünümü şablonunda bu parametreleri kullanmayı öğreneceksiniz. Bu amaç için önce verileri ve etki alanı mantığınızı yönetmenize yardımcı olacak Model sınıflarına sunulacaktır. Buna ek olarak, nesnelerin interneti kodlama URL yolları gibi endişelenmenize gerek kalmadan bir ASP.NET MVC uygulaması içindeki sayfalara bağlantılar oluşturmak nasıl öğreneceksiniz.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Görev 1 - Model sınıfları ekleme

Yalnızca bilgi denetleyicisinden görünüme iletmek için oluşturulan Viewmodel'lar, Model sınıfları içeren ve veri ve etki alanı mantığı yönetmek için oluşturulur. Bu görevde, bu kavramları göstermek için iki model sınıfları ekleyeceksiniz: **Tarz** ve **albüm**.

1. Açık değilse, başlangıç **Web için VS Express**
2. İçinde **dosya** menüsünde seçin **Proje Aç**. Proje Aç iletişim kutusunda, Gözat **Source\Ex06 UsingParametersInView\Begin**seçin **Begin.sln** tıklatıp **açık**. Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
3. Ekleme bir **Tarz** Model sınıfı. Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Genre.cs*, ardından **Ekle**.

    ![Sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image27.png "sınıf ekleme")

    *Yeni bir öğe ekleme*

    ![Tarz Model sınıfı ekleme](aspnet-mvc-4-fundamentals/_static/image28.png "Tarz Model sınıfı ekleme")

    *Tarz Model sınıfı ekleme*
4. Ekleme bir **adı** Tarz sınıf özelliği. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 Tarz*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Aşağıdaki yordamın aynısını önce bir **albüm** sınıfı. Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Album.cs*, ardından **Ekle**.
6. İki özellik albüm sınıfa ekleyin: **Tarz** ve **başlık**. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 albüm*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Görev 2 - bir StoreBrowseViewModel ekleme

A **StoreBrowseViewModel** bu görevde, seçilen türe eşleşen albümleri göstermek için kullanılacak. Bu görevde, bu sınıf oluşturacak ve ardından işlemek için iki özellik ekleyin **Tarz** ve kendi **albüm**kullanıcının listesi.

1. Ekleme bir **StoreBrowseViewModel** sınıfı. Bunu yapmak için sağ **Viewmodel'lar** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği. Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreBrowseViewModel.cs*, ardından **Ekle**.
2. Modeller başvuruyu eklemek **StoreBrowseViewModel** sınıfı. Bunu yapmak için aşağıdakileri ekleyin.'using namespace:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. İki özellikleri **StoreBrowseViewModel** sınıfı: **Tarz** ve **albümleri**. Bunu yapmak için aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Nedir **listesi&lt;albüm&gt;**  ?: Bu tanımı kullanan **listesi&lt;T&gt;**  yazın, burada **T** kısıtlar hangi öğelere bu tür **listesi** için bu durumda ait **albüm** (veya alt öğelerinden birini).
> 
> Sınıflar ve sınıf ya da yöntem bildirilir ve istemci kodu tarafından oluşturulan C# dil özelliğidir kadar bir veya daha fazla tür belirtimi erteleme yöntemler tasarlamak için bu özelliği adlı **genel türler**.
> 
> **Liste&lt;T&gt;**  genel eşdeğerdir **ArrayList** yazın ve kullanılabilir **System.Collections.Generic** ad alanı. Kullanmanın faydaları birini **genel türler** türü belirtilmiş olduğundan, tür denetimini öğeyi atama gibi işlemleri ölçeklendirilmesini gerek olmayan **albüm** ile bir yaptığınızşekilde**ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Görev 3 - yeni ViewModel StoreController içinde kullanma

Bu görevde, değiştireceğiniz **StoreController**'s **Gözat** ve **ayrıntıları** yeni eylem yöntemleri **StoreBrowseViewModel** .

1. Modeller klasörü içinde bir başvuru ekleyin **StoreController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasöründe **Çözüm Gezgini** açın **StoreController** sınıfı. Ardından aşağıdaki kodu ekleyin:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Değiştirin **Gözat** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı. Sahte veri bir türe ve iki yeni albümleri nesneleri oluşturur (sonraki uygulamalı laboratuvarda bir veritabanındaki gerçek veriler tükettiğiniz). Bunu yapmak için değiştirin **Gözat** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Değiştirin **ayrıntıları** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı. Yeni oluşturduğunuz **albüm** için döndürülecek nesne **görünümü**. Bunu yapmak için değiştirin **ayrıntıları** yöntemini aşağıdaki kod ile:

    (Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Görev 4 - Gözat görünüm şablonu ekleme

Bu görevde, ekleyeceksiniz bir **Gözat** görünümü için belirli bir türe bulunan albümleri gösterecek şekilde.

1. Yeni Görünüm şablonu oluşturmadan önce projeyi oluşturmalısınız böylece **Görünüm Ekle** iletişim bilir hakkında **ViewModel** kullanılacak sınıfı. Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.
2. Ekleme bir **Gözat** görünümü. Bunu yapmak için sağ **Gözat** eylem yöntemi **StoreController** tıklatıp **Görünüm Ekle**.
3. İçinde **Görünüm Ekle** iletişim kutusunda, Görünüm adı olduğunu doğrulayın **Gözat**. Denetleme **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **StoreBrowseViewModel** gelen **Model sınıfı** açılır. Diğer alanları varsayılan değerlerine bırakın. Ardından **Ekle**.

    ![Göz atma görünüm ekleme](aspnet-mvc-4-fundamentals/_static/image29.png "Gözat görünüm ekleme")

    *Göz atma görünüm ekleme*
4. Değiştirme **Browse.cshtml** tarzı'nın bilgilerini görüntülemek için erişim **StoreBrowseViewModel** görünümü şablona geçirilen nesne. Bunu yapmak için içeriği aşağıdakiyle değiştirin: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, test **Gözat** yöntemi albümleri öğesinden alır **Gözat** yöntemi eylem.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi   **/Store/Gözat? Tarz DISCO =** eylemi iki albümleri döndürdüğünü doğrulayın.

    ![Store DISCO albümleri gözatma](aspnet-mvc-4-fundamentals/_static/image30.png "Store DISCO albümleri gözatma")

    *Store DISCO albümleri gözatma*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Görev 6 - bilgi hakkında belirli bir albüm görüntüleme

Bu görevde, gerçekleştireceksiniz **Store/Details** belirli bir albümü hakkındaki bilgileri görüntülemek için görünümü. Bu uygulamalı laboratuvarda, görüntüler albümü hakkında her şeyi zaten bulunan **görünümü** şablonu. Bunu oluşturmak yerine bir **StoreDetailsViewModel** sınıfı, geçerli kullanacağınız **StoreBrowseViewModel** albümü iletmeden şablonu.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Yeni bir **ayrıntıları** görüntüleme **StoreController**'s **ayrıntıları** eylem yöntemi. Bunu yapmak için sağ **ayrıntıları** yönteminde **StoreController** sınıfı ve tıklayın **Görünüm Ekle**.
2. İçinde **Görünüm Ekle** iletişim kutusunda doğrulayın **Görünüm adı** olduğu **ayrıntıları**. Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm** gelen **Model sınıfı** açılır. Diğer alanları varsayılan değerlerine bırakın. Ardından **Ekle**. Bu oluşturma ve açık bir **\Views\Store\Details.cshtml** dosya.

    ![Ayrıntılar görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image31.png "Ayrıntılar görünümü ekleme")

    *Ayrıntılar görünümü ekleme*
3. Değiştirme **Details.cshtml** albüm bilgilerini görüntülemek için dosya erişim **albüm** görünümü şablona geçirilen nesne. Bunu yapmak için içeriği aşağıdakiyle değiştirin: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Görev 7 - Uygulamayı çalıştırma

Bu görevde, test **ayrıntıları** görünümü alır albüm bilgilerinden **eylem ayrıntıları** yöntemi.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlar **giriş** sayfası. URL'yi **/Store/Details/5** albüm bilgileri doğrulayabilirsiniz.

    ![Albümleri ayrıntılı tarama](aspnet-mvc-4-fundamentals/_static/image32.png "albümleri ayrıntılı tarama")

    *Albüm ayrıntılı tarama*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Görev 8 - sayfaları arasında bağlantılar ekleme

Bu görevde, Store görünümünde her Tarz ad uygun bir bağlantı sağlamak için bir bağlantı ekleyeceksiniz **/Store/göz atma** URL'si. Bu şekilde bir türe üzerinde örneği için tıkladığınızda **DISCO**, gider **/Store/göz atma? Tarz DISCO =** URL'si.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Güncelleştirme **dizin** bağlantısı eklemek için sayfa **Gözat** sayfası. Bunu yapmak için **Çözüm Gezgini** genişletin **görünümleri** klasörü, ardından **Store** klasörü ve çift **Index.cshtml** sayfası.
2. Bağlantı, seçili Tarz gösteren göz atma görünümüne ekleyin. Bunu yapmak için aşağıdaki vurgulanmış kodu içinde değiştirin **&lt;li&gt;** etiketler: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > başka bir yaklaşım ile aşağıdaki gibi bir kod sayfası için doğrudan bağlama:
   > 
   > &lt;bir href =&quot;/Store/göz atma? Tarz =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Bu yaklaşım çalışır, ancak bir sabit kodlanmış dizesine bağlıdır. Daha sonra kumanda yeniden adlandırma, bu yönerge el ile değiştirmeniz gerekecektir. Daha iyi bir alternatif kullanmaktır bir **HTML Yardımcısı** yöntemi. ASP.NET MVC gibi görevler için kullanılabilir olan bir HTML yardımcı yöntemini içerir. **Html.ActionLink()** yardımcı yöntem HTML oluşturmak kolaylaştırır **&lt;bir&gt;** bağlantılar, URL yolları URL kodlanmış düzgün olduğundan emin olun.
   > 
   > Htlm.ActionLink birçok aşırı yüklemeye sahip. Bu alıştırmada, üç parametre almayan kullanır:
   > 
   > 1. Bağlantı metnini, tarzı adı görüntülenir
   > 2. Denetleyici eylem adı (**Gözat**)
   > 3. Rota parametresi değerleri, her iki adı belirtme (**Tarz**) değeri (**Tarz adı**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Görev 9 - uygulamayı çalıştırma

Bu görevde her bir tür için uygun bir bağlantı olarak görüntülendiğini sınayacak **/Store/göz atma** URL'si.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/Store** her türe uygun bağlantılar doğrulamak için **/Store/göz atma** URL'si.

    ![Göz atma sayfasından bağlantı türleri gözatma](aspnet-mvc-4-fundamentals/_static/image33.png "göz atma sayfasından bağlantılarla gözatma türleri")

    *Göz atma sayfasından bağlantı türleri gözatma*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Görev 10 - dinamik ViewModel koleksiyonu kullanarak değerleri geçirmek için

Bu görevde, modeldeki herhangi bir değişiklik yapmadan değerleri denetleyici ve görünüm arasında geçirmek için basit ve güçlü bir yöntemdir öğreneceksiniz. ASP.NET MVC 4 sağlar koleksiyonu &quot;ViewModel&quot;, hangi herhangi bir dinamik değer atanabilir ve denetleyicileri ve görünümleri de içinde erişilebilir.

Artık ViewBag dinamik koleksiyon listesini geçirmek için kullanacak &quot; **yıldızlı türleri** &quot; görünümüne denetleyicisinden. Store dizini görünümü için erişecek **ViewModel** ve bilgileri görüntüler.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Açık **StoreController.cs** ve değiştirme **dizin** listesini oluşturmak için yöntem türleri ViewModel koleksiyona yıldızlı:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Söz dizimi de kullanabilirsiniz **ViewBag [&quot;Starred&quot;]** özelliklerine erişmek için.
2. Yıldız simgesini **&quot;starred.png&quot;** dahil **Source\Assets\Images** , bu Laboratuvarın klasör. Uygulama eklemek için bunların içeriğini sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** Express'te Visual Web Developer aşağıda gösterildiği gibi:

    ![Çözüm ekleme yıldız görüntüsüne](aspnet-mvc-4-fundamentals/_static/image34.png "çözüme yıldız görüntü ekleme")

    *Çözüme yıldız görüntü ekleme*
3. Görünümü açma **Store/Index.cshtml** ve içeriği değiştirebilirsiniz. Okuyacaksa &quot;yıldızlı&quot; özelliğinde **ViewBag** koleksiyonu geçerli Tarz ad listesinde olup olmadığını isteyin. Bu durumda, bir yıldız simgesini sağ Tarz bağlantıyı gösterir.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Görev 11 - uygulamayı çalıştırma

Bu görevde yıldızlı türleri bir yıldız simgesini görüntüleme test eder.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje başlar **giriş** sayfası. URL'yi **/Store** öne çıkan her Tarz saygı etiket olduğunu doğrulamak için:

    ![Yıldızlı öğelerle türleri gözatma](aspnet-mvc-4-fundamentals/_static/image35.png "yıldızlı öğelerle gözatma türleri")

    *Yıldızlı öğelerle gözatma türleri*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Alıştırma 7: ASP.NET MVC 4 yeni şablonu turu

Bu alıştırmada, bir görünüm en ilgili özellikleri yeni şablon alma ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>1. Görev: ASP.NET MVC 4 Internet uygulaması şablonu keşfetme

1. Açık değilse, başlangıç **Web için VS Express**
2. Seçin **dosya | Yeni | Proje** menü komutu. İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması**. **Adı** proje *MusicStore* ve güncelleştirme **çözüm adı** için *başlamak*, ardından bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam** .

    ![Yeni bir ASP.NET MVC 4 projesi oluşturma](aspnet-mvc-4-fundamentals/_static/image36.png "yeni bir ASP.NET MVC 4 projesi oluşturma")

    *Yeni bir ASP.NET MVC 4 projesi oluşturma*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**. Görünüm altyapısı Razor ya da ASPX seçebilirsiniz dikkat edin.

    ![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](aspnet-mvc-4-fundamentals/_static/image37.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")

    *Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*

    > [!NOTE]
    > Razor sözdizimi, ASP.NET MVC 3'te sunulmuştur. Karakterler ve tuş vuruşları gerekli bir dosya içinde hızlı ve iş akışı kodlama sıvı etkinleştirme sayısını en aza indirmek için hedefi sağlamaktır. Mevcut C# /VB (veya diğer) Razor yararlanır dil becerilerine ve harika bir HTML oluşturma iş akışı sağlayan bir şablon işaretleme söz dizimi sağlar.
4. Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonu görmek için. Aşağıdaki özellikleri kontrol edebilirsiniz:

    1. **Modern stili şablonları**

        Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.

        ![ASP.NET MVC 4 restyled şablonları](aspnet-mvc-4-fundamentals/_static/image38.png "şablonları restyled ASP.NET MVC 4")

        *ASP.NET MVC 4 restyled şablonları*
    2. **Uyarlamalı işleme**

        Tarayıcı penceresini yeniden boyutlandırmadan kullanıma denetleyin ve sayfa düzeni için yeni pencere boyutunu dinamik olarak Uyarlanır nasıl dikkat edin. Bu şablonlar, hem Masaüstü hem de mobil platformlarda özelleştirme olmadan düzgün bir şekilde işlemek için Uyarlamalı işleme tekniği kullanın.

        ![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](aspnet-mvc-4-fundamentals/_static/image39.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")

        *ASP.NET MVC 4 Proje şablonunda farklı tarayıcı boyutları*
5. Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.
6. Artık çözümü keşfedin ve bazı proje şablonunda ASP.NET MVC 4'tarafından sunulan yeni özellikleri kullanıma sunuldu.

    ![ASP.NET MVC4-internet-uygulaması-proje-şablonu](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")

    *ASP.NET MVC 4 Internet uygulaması proje şablonu*

   1. **HTML5 biçimlendirme**

       Şablon görünümleri yeni tema biçimlendirme açık örnek bulmak için Gözat **About.cshtml** görünümü **giriş** klasör.

       ![Razor ve HTML5 biçimlendirme kullanarak yeni şablon](aspnet-mvc-4-fundamentals/_static/image41.png "Razor ve HTML5 biçimlendirme kullanarak yeni şablon")

       *Razor ve HTML5 biçimlendirme kullanarak yeni şablon*
   2. **JavaScript kitaplıkları dahil**

      1. **jQuery**: HTML belge geçiş yapma, olay işleme, animasyon ve Ajax etkileşimleri jQuery basitleştirir.
      2. **jQuery kullanıcı Arabirimi**: Bu kitaplık, alt düzey etkileşim, animasyon efektleri Gelişmiş ve jQuery JavaScript kitaplığı üzerine kurulu bölümlerinin tema eklenebilir pencere öğeleri için soyutlamalar sağlar.

         > [!NOTE]
         > JQuery ve jQuery kullanıcı Arabirimi hakkında bilgi edinin, [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: ASP.NET MVC 4 varsayılan şablonu artık içerir **KnockoutJS**, JavaScript ve HTML kullanarak zengin ve yüksek derecede yanıt veren web uygulamaları oluşturmanızı sağlayan bir JavaScript MVVM çerçeve. Gibi ASP.NET MVC 3'te, jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.

          > [!NOTE]
          > Bu bağlantıda KnockOutJS Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: Bu kitaplık HTML5 ve CSS3 teknolojileri kullanırken, sitenizi eski tarayıcıları ile uyumlu hale otomatik olarak çalıştırır.

          > [!NOTE]
          > Bu bağlantıda Modernizr Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **Çözümde yer alan SimpleMembership**

       SimpleMembership önceki ASP.NET rol ve üyelik sağlayıcısını sistemin bir ardılı olarak tasarlanmıştır. Güvenli web sayfaları geliştiricinin daha esnek bir biçimde kolaylaştıran birçok yeni özellik var.

       Internet şablonu zaten SimpleMembership tümleştirmek için birkaç şeyi ayarlamanız ayarladı, AccountController OAuthWebSecurity (için OAuth hesap kaydı, oturum açma, yönetim, vb.) ve Web güvenliği gibi hazırlanır.

       ![Çözüm SimpleMembership dahil](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dahil çözümü")

       *Çözüm SimpleMembership dahil*

       > [!NOTE]
       > Hakkında daha fazla bilgi bulmak [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN'de.

> [!NOTE]
> Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarı tamamlayarak ASP.NET MVC temelleri öğrendiniz:

- Bir MVC uygulaması ve nasıl etkileşim kurduklarını temel öğeleri
- Bir ASP.NET MVC uygulaması oluşturma
- URL ve sorgu dizesi ekleyin ve parametreleri işlemek için denetleyicileri yapılandırma başarılı
- Ortak HTML içeriğini, görünümünü ve HTML içeriğini görüntülemek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurmak için bir düzen ana sayfası ekleme
- Dinamik bilgileri görüntülemek için Özellikler Görünümü şablona geçirme ViewModel desenini kullanma
- Görünüm şablonda denetleyicilerine geçirilen parametreleri kullanma
- ASP.NET MVC uygulaması içindeki sayfalara bağlantılar ekleme
- Ekleme ve bir görünümle dinamik özelliklerini kullanma
- ASP.NET MVC 4 proje şablonları iyileştirmeler

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Windows Azure Portal'da oturum açın](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image49.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image50.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-fundamentals/_static/image51.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](aspnet-mvc-4-fundamentals/_static/image52.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-fundamentals/_static/image53.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.

    ![Yayımlama profili web sitesi indiriliyor](aspnet-mvc-4-fundamentals/_static/image54.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-fundamentals/_static/image55.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](aspnet-mvc-4-fundamentals/_static/image56.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-fundamentals/_static/image58.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Değişiklikleri onaylayın*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-fundamentals/_static/image60.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-fundamentals/_static/image61.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-fundamentals/_static/image62.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-fundamentals/_static/image63.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.

     ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-fundamentals/_static/image64.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-fundamentals/_static/image65.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](aspnet-mvc-4-fundamentals/_static/image66.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-fundamentals/_static/image67.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

    ![Uygulama Windows Azure'da yayımlanan](aspnet-mvc-4-fundamentals/_static/image68.png "uygulama yayımlanan Windows Azure'a")

    *Windows Azure'da yayımlanan uygulama*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-fundamentals/_static/image69.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-fundamentals/_static/image70.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-fundamentals/_static/image71.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-fundamentals/_static/image72.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-fundamentals/_static/image73.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-fundamentals/_static/image74.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
