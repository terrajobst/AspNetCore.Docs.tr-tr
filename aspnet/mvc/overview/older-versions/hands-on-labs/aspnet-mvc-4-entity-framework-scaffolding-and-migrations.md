---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 Entity Framework İskele ve geçişleri | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC 4 denetleyici yöntemleriyle bilginiz veya tamamladınız &quot;Yardımcıları, formlar ve doğrulama&quot; uygulamalı Laboratuvar olmanız gerekir kullanan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 396859463446d95c58271c4b00fc950bcd0d539a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework İskele ve geçişleri

Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 denetleyici yöntemleriyle bilginiz veya tamamladınız &quot;Yardımcıları, formlar ve doğrulama&quot; uygulamalı Laboratuvar size dikkat oluşturmak için mantığı çoğunu güncelleştirme, listelemek ve onu yineleniyor herhangi bir veri varlık kaldırır uygulama arasında. Modelinizi işlemek için birden fazla sınıf varsa, değil belirtmeyi, her varlık işlemi yanı sıra her görünüm için POST ve GET eylem yöntemleri yazma önemli bir süre beklemesini olası olacaktır.

Bu laboratuvarda, ASP.NET MVC 4 yapı iskelesi otomatik olarak, uygulamanızın CRUD (Oluştur, okuma, güncelleştirme ve silme) taban çizgisi oluşturmak için nasıl kullanılacağını öğreneceksiniz. Basit model sınıfından ve tek satırlık bir kod yazmayı olmadan başlayarak, tüm gerekli görünümler yanı sıra tüm CRUD işlemleri içeren bir denetleyici oluşturur. Oluşturma ve basit çözüm çalıştırma sonra MVC mantığı ve verileri işleme için görünümleri ile birlikte oluşturulan uygulama veritabanı gerekir.

Ayrıca, tüm uygulama boyunca model güncelleştirmelerini gerçekleştirmek için Entity Framework geçişler kullanın ne kadar kolay olduğunu öğreneceksiniz. Entity Framework geçişler, model ile basit adımları değiştikten sonra veritabanını değiştirme olanak tanır. Tüm bu konularda unutmayın, ASP.NET MVC 4'ın en son özelliklerini yararlanarak oluşturmak ve web uygulamalarını daha verimli bir şekilde korumak kuramaz.

> [!NOTE]
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresindeki kullanılabilir içinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 Entity Framework İskele ve geçişleri](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:

- ASP.NET iskele denetleyicileriyle CRUD işlemleri için kullanın.
- Entity Framework geçişleri kullanmaya veritabanı modelini değiştirin.

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

Aşağıdaki alıştırmada bu uygulamalı laboratuarı olun:

1. [ASP.NET MVC 4 yapı İskelesi Entity Framework geçişler ile kullanma](#Exercise1)

> [!NOTE]
> Bu alıştırmada tarafından eşlik bir **son** elde alıştırma tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırma ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Alıştırma 1: ASP.NET MVC 4 yapı İskelesi Entity Framework geçişler ile kullanma

ASP.NET MVC yapı iskelesi CRUD işlemleri uygulamanızı veritabanı katmanı ile etkileşime olanak sağlayan gerekli mantığı oluşturma standartlaştırılmış bir şekilde oluşturmak için hızlı bir yol sağlar.

Bu alıştırmada, ASP.NET MVC 4 yapı iskelesi koduyla ilk CRUD yöntemleri oluşturmak için nasıl kullanılacağını öğreneceksiniz. Ardından, Entity Framework geçişler kullanarak veritabanındaki değişiklikleri uygulamadan modelinizi güncelleştirme konusunda bilgi edineceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Yapı İskelesi kullanarak görev 1-oluşturma yeni bir ASP.NET MVC 4 Proje

1. Zaten açık değilse, başlangıç **Visual Studio 2012**.
2. Seçin **dosya | Yeni proje**. Yeni Proje iletişim kutusunda, altında **Visual C# | Web** bölümünde, select **ASP.NET MVC 4 Web uygulaması**. Proje adı **MVC4andEFMigrations** ve konumunu **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** bu laboratuvarı klasör. Ayarlama **çözüm adı** için **başlamak** ve olun **çözüm için dizin oluştur** denetlenir. **Tamam**'ı tıklatın.

    ![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")

    *Yeni ASP.NET MVC 4 Proje iletişim kutusu*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **Internet uygulama** şablon emin olun **Razor** seçili olan **görünüm altyapısı**. Tıklatın **Tamam** projesi oluşturmak için.

    ![Yeni ASP.NET MVC 4 Internet uygulaması](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması")

    *Yeni ASP.NET MVC 4 Internet uygulaması*
4. Çözüm Gezgini'nde sağ **modelleri** seçip **Ekle | Sınıf** basit sınıf kişi (POCO) oluşturmak için. Bu ad **kişi** tıklatıp **Tamam**.
5. Kişi sınıfı açın ve aşağıdaki özellikleri ekleyin.

    (Kod parçacığını - *ASP.NET MVC 4 ve Entity Framework geçişleri - Ex1 kişi özellikleri*)


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Tıklatın **yapı | Çözümü derlemek** değişiklikleri kaydetmek ve projeyi oluşturmak için.

    ![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "uygulama oluşturma")

    *Uygulama oluşturma*
7. Çözüm Gezgini'nde denetleyicileri klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici**.
8. Denetleyici adı *PersonController* ve tamamlamak **yapı İskelesi seçenekleri** aşağıdaki değerlere sahip.

    1. İçinde **şablonu** aşağı açılan listesinden, **okuma/yazma eylemleri ve Entity Framework kullanarak görünümleri ile MVC denetleyicisi** seçeneği.
    2. İçinde **Model sınıfı** aşağı açılan listesinden, **kişi** sınıfı.
    3. İçinde **veri bağlamı sınıfı** listesinde  **&lt;yeni veri bağlamı... &gt;**. Herhangi bir ad seçin ve tıklatın **Tamam**.
    4. İçinde **görünümleri** aşağı açılan listesinde, olduğundan emin olun **Razor** seçilir.

    ![Yapı iskelesi ile kişi denetleyicisi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "yapı iskelesi ile kişi denetleyicisi ekleme")

    *Yapı iskelesi ile kişi denetleyicisi ekleme*
9. Tıklatın **Ekle** yeni denetleyicisi kişi için yapı iskelesi ile oluşturmak için. Denetleyici eylemleri ve bunun yanı sıra görünümleri şimdi oluşturdunuz.

    ![Yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra")

    *Yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra*
10. Açık **PersonController** sınıfı. Tam CRUD eylem yöntemlerine otomatik olarak oluşturulmuş olan dikkat edin.

    ![Kişi denetleyicisi içinde](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "içinde kişi denetleyicisi")

    *İçinde kişi denetleyicisi*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Görev 2-çalışan uygulama

Bu noktada, veritabanı henüz oluşturulmadı. Bu görevde, uygulamayı ilk kez çalıştırma ve CRUD işlemleri test etme. Veritabanı Code First ile kolay bir şekilde oluşturulur.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Tarayıcıda eklemek **/Person** kişi sayfasını açmak için URL.

    ![Uygulaması ilk çalıştırma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "uygulaması ilk çalıştırma")

    *Uygulama: ilk çalıştırma*
3. Şimdi kişi sayfaları keşfedin ve CRUD işlemleri test.

    1. Tıklatın **Yeni Oluştur** yeni bir kişiye eklemek için. Bir ad ve soyadı girin ve tıklayın **oluşturma**.

        ![Yeni bir kişiye ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "yeni bir kişiye ekleme")

        *Yeni bir kişiye ekleme*
    2. Kişinin listesinde, silme, düzenleme veya öğeleri ekleyin.

        ![kişi listesi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "kişi listesi")

        *Kişi listesi*
    3. Tıklatın **ayrıntıları** kişinin ayrıntıları açın.

        ![Kişinin ayrıntıları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "kişinin ayrıntıları")

        *Kişinin ayrıntıları*
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün. Kişi varlığı için tüm CRUD uygulamanızdan - görünümleri modeline - boyunca tek satırlık bir kod yazmak zorunda kalmadan oluşturduğunuz dikkat edin!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Görev 3-güncelleştirme Entity Framework geçişler kullanarak veritabanı

Bu görevde Entity Framework geçişler kullanarak veritabanını güncelleştirir. Model değiştirmek ve Entity Framework geçişler özelliğini kullanarak veritabanlarınızı değişiklikleri yansıtmak için ne kadar kolay olduğunu keşfeder.

1. Paket Yöneticisi Konsolu'nu açın. Seçin **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Geçişler etkinleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "geçişler etkinleştirme")

    *Geçişler etkinleştirme*

    Enable-Migration komutunu oluşturur **geçişler** veritabanını başlatmak için komut dosyası içeren klasör.

    ![Migrations klasörünü](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "geçişler klasörü")

    *Geçişler klasörü*
3. Açık **Configuration.cs** Migrations klasörünü dosyasında. Sınıf oluşturucu bulun ve değiştirin **AutomaticMigrationsEnabled** değeri *doğru*.


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Kişi sınıfı açın ve bir öznitelik için kişinin ikinci adını ekleyin. Bu yeni öznitelik model değiştiriyorsunuz.


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seçin **yapı | Çözümü derlemek** uygulamayı yapılandırmak için menüsünde.

    ![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "uygulama oluşturma")

    *Uygulama oluşturma*
6. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Bu komut veri nesnelerde değişiklikler arar ve daha sonra veritabanını uygun şekilde değiştirmek için gerekli komutları ekleyecektir.

    ![İkinci adı ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ikinci adı ekleme")

    *İkinci adı ekleme*
7. (İsteğe bağlı) Fark güncelleştirmesi ile bir SQL komut dosyası oluşturmak için aşağıdaki komutu çalıştırabilirsiniz. Bu, veritabanını el ile güncelleştirmeniz olanak tanır (Bu durumda ise gerekli değildir), veya diğer veritabanlarındaki değişiklikleri uygulayın:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![SQL komut dosyası oluşturuluyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL komut dosyası oluşturma")

    *SQL komut dosyası oluşturma*

    ![SQL komut dosyasını güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL komut dosyasını güncelleştirme")

    *SQL komut dosyasını güncelleştirme*
8. Paket Yöneticisi konsolunda veritabanını güncelleştirmek için aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Veritabanını güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "veritabanını güncelleştirme")

    *Veritabanını güncelleştirme*

    Bu ekler **MiddleName** sütununda **kişiler** geçerli tanımı eşleştirmek için tablo **kişi** sınıfı.
9. Veritabanı güncelleştirildikten sonra denetleyici klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici** kişi denetleyicisi yeniden (aynı değerlere sahip eksiksiz) eklemek için. Bu görünümler yeni öznitelik eklemek ve mevcut yöntemler güncelleştirir.

    ![Bir denetleyici güncelleştirmesini ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "denetleyici güncelleştirmesini ekleme")

    *Denetleyici güncelleştiriliyor*
10. **Ekle**'yi tıklatın. Ardından, değerleri seçin **üzerine PersonController.cs** ve **üzerine yaz ilişkili görünümleri** tıklatıp **Tamam**.

    ![Bir denetleyici üzerine yaz ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    *Denetleyici güncelleştiriliyor*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - uygulama çalışıyor

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Açık **/Person**. İkinci Ad sütunu eklendi ancak verileri, korundu olduğunu dikkat edin.

    ![İkinci eklenen adı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ikinci eklenen adı")

    *İkinci eklenen adı*
3. Tıklatırsanız **Düzenle**, geçerli kişinin ikinci adı eklemeniz mümkün olacaktır.

    ![İkinci adı edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ikinci adı edition")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Uygulamalı bu laboratuvarda, ASP.NET MVC 4 herhangi bir model sınıfını kullanarak İskele ile CRUD işlemleri oluşturmak için basit adımları öğrendiniz. Ardından, Entity Framework geçişler kullanarak bir uçtan uca güncelleştirme uygulamanızdan - görünümleri veritabanına - gerçekleştirmek nasıl öğrendiniz.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/? LinkID 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    Lisans koşulları kabul ediliyor
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    Yükleme ilerleme durumu
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    Yükleme tamamlandı
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    VS Express Web döşemeye

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
