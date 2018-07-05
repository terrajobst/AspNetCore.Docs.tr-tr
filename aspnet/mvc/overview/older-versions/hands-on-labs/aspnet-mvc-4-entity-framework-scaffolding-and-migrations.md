---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 denetleyici yöntemleri ile ilgili bilgi sahibi olduğunuz veya tamamlamış olmanız durumunda &quot;yardımcılar, formlar ve doğrulama&quot; uygulamalı laboratuvarı size dikkat edin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394215"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 denetleyici yöntemleri ile ilgili bilgi sahibi olduğunuz veya tamamlamış olmanız durumunda &quot;yardımcılar, formlar ve doğrulama&quot; uygulamalı laboratuvarı olmanız gerekir birçok oluşturmak için mantıksal güncelleştirme, liste ve bunu yinelenir herhangi bir veri varlığı kaldırmak olduğunu aklınızda bulundurun uygulama arasında. Modelinizi işlemek için çeşitli sınıflar varsa, uyumlu için büyük olasılıkla her varlık işlemi yanı sıra her bir görünüm için POST ve GET eylem yöntemlerini yazma önemli bir zaman ayırın.

Bu laboratuvarda, ASP.NET MVC 4 yapı iskelesi otomatik olarak uygulamanızın CRUD (oluşturma, okuma, güncelleştirme ve silme) taban çizgisini oluşturmak için nasıl kullanılacağını öğreneceksiniz. Basit model sınıfı ve, tek satırlık bir kod yazmadan başlayarak, tüm gerekli görünümleri yanı sıra tüm CRUD işlemleri içeren bir denetleyici oluşturur. Oluşturma ve basit bir çözüm çalıştırma sonrasında, MVC mantığı ve veri işleme için görünümleri ile birlikte oluşturulan uygulama veritabanı gerekir.

Ayrıca, tüm uygulamanızda model güncelleştirmelerini gerçekleştirmek için Entity Framework geçişleri kullanmak için ne kadar kolay olduğunu öğreneceksiniz. Entity Framework geçişleri, model basit adımlarla değiştirildikten sonra veritabanını değiştirme olanak tanır. Tüm bu konularda unutmayın, ASP.NET MVC 4'ün en son özelliklerden yararlanarak oluşturun ve web uygulamalarını daha verimli bir şekilde korumak mümkün olmayacak.

> [!NOTE]
> Web Kampları eğitim Seti'nde bulunan, tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- ASP.NET iskeleti oluşturma denetleyicileri CRUD işlemleri için kullanın.
- Entity Framework geçişleri kullanarak veritabanı modeli değiştirin.

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

Aşağıdaki alıştırmada bu uygulamalı laboratuvarı ayarlama yapın:

1. [ASP.NET MVC 4 yapı İskelesi ile Entity Framework geçişleri kullanma](#Exercise1)

> [!NOTE]
> Bu alıştırmada eşlik ettiği bir **son** elde alıştırma tamamlandıktan sonra elde edilen çözümü içeren klasör. Alıştırma çalışma ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Alıştırma 1: ASP.NET MVC 4 yapı İskelesi ile Entity Framework geçişleri kullanma

ASP.NET MVC yapı iskelesi CRUD işlemleri uygulamanızı veritabanı katmanı ile etkileşime olanak sağlayan gerekli mantığı oluşturma standartlaştırılmış bir şekilde oluşturmak için hızlı bir yolunu sağlar.

Bu alıştırmada, ASP.NET MVC 4 yapı iskelesi ile kod ilk CRUD yöntemler oluşturmak için nasıl kullanılacağını öğreneceksiniz. Ardından, varlık çerçevesi geçişleriyle kullanarak veritabanındaki değişiklikleri uygulamadan modelinizi güncelleştirme öğreneceksiniz.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Görev 1-oluşturma yeni bir ASP.NET MVC 4 Proje yapı iskelesini kullanmaya

1. Açık değilse, başlangıç **Visual Studio 2012**.
2. Seçin **dosya | Yeni proje**. Yeni iletişim kutusunda, altında proje **Visual C# | Web** bölümünden **ASP.NET MVC 4 Web uygulaması**. Projeye ad **MVC4andEFMigrations** ve konumunu **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** , bu Laboratuvarın klasör. Ayarlama **çözüm adı** için **başlamak** olun **çözüm için dizin oluştur** denetlenir. **Tamam**'ı tıklatın.

    ![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")

    *Yeni ASP.NET MVC 4 Proje iletişim kutusu*
3. İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunu seçin **Internet uygulaması** şablon emin olun **Razor** seçili olduğu **Görünümaltyapısı**. Tıklayın **Tamam** projeyi oluşturmak için.

    ![Yeni bir ASP.NET MVC 4 Internet uygulaması](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması")

    *Yeni bir ASP.NET MVC 4 Internet uygulaması*
4. Çözüm Gezgini'nde sağ **modelleri** seçip **Ekle | Sınıf** basit sınıf kişi (POCO) oluşturmak için. Adlandırın **kişi** tıklatıp **Tamam**.
5. Kişi sınıfını açın ve aşağıdaki özellikleri ekleyin.

    (Kod parçacığını - *ASP.NET MVC 4 ve kurum çerçevesi geçişlerine - Ex1 kişi özellikleri*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Tıklayın **yapı | Çözüm yapı** değişiklikleri kaydedin ve projeyi derleyin.

    ![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "uygulama oluşturma")

    *Uygulama oluşturma*
7. Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayıp **Ekle | Denetleyici**.
8. Denetleyici adı *PersonController* ve tamamlayın **yapı İskelesi seçenekleri** şu değerlere sahip.

   1. İçinde **şablon** aşağı açılan listesinden **okuma/yazma eylemleri ve Entity Framework kullanarak görünümler ile MVC denetleyicisi** seçeneği.
   2. İçinde **Model sınıfı** aşağı açılan listesinden **kişi** sınıfı.
   3. İçinde **veri bağlamı sınıfı** listesinden  **&lt;yeni veri bağlamı... &gt;**. Herhangi bir ad seçin ve tıklayın **Tamam**.
   4. İçinde **görünümleri** aşağı açılan listesinde, emin **Razor** seçilir.

      ![Yapı iskelesi ile kişi denetleyicisi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "yapı iskelesi ile kişi denetleyicisi ekleme")

      *Yapı iskelesi ile kişi denetleyicisi ekleme*
9. Tıklayın **Ekle** yeni denetleyici kişi için yapı iskelesi ile oluşturmak için. Şimdi, denetleyici eylemlerini ve bunun yanı sıra görünümleri oluşturduktan.

    ![Kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra")

    *Kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra*
10. Açık **PersonController** sınıfı. Tam CRUD eylem yöntemlerine otomatik olarak oluşturulmuş dikkat edin.

   ![Kişi denetleyicisi içinde](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "içinde kişi denetleyicisi")

   *İçinde kişi denetleyicisi*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Görev 2-çalışan uygulama

Bu noktada, veritabanı henüz oluşturulmamış. Bu görevde, uygulamayı ilk kez çalıştırmak ve CRUD işlemleri test edin. Veritabanı hareket halindeyken Code First ile oluşturulur.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Tarayıcıda ekleme **/Person** kişi sayfasını açmak için URL.

    ![Uygulaması ilk çalıştırma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "uygulaması ilk çalıştırma")

    *Uygulama: ilk çalıştırma*
3. Şimdi kişi sayfaları keşfedin ve CRUD işlemleri test edin.

    1. Tıklayın **Yeni Oluştur** yeni bir kişiye eklemek için. Bir ad ve soyadı girin ve tıklayın **Oluştur**.

        ![Yeni bir kişi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "yeni bir kişi ekleme")

        *Yeni bir kişi ekleme*
    2. Kişinin listesinde, silmek, düzenlemek veya öğeleri ekleyin.

        ![kişi listesi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "kişi listesi")

        *Kişi listesi*
    3. Tıklayın **ayrıntıları** kişinin ayrıntılarını açmak için.

        ![Kişi ayrıntıları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "kişinin ayrıntıları")

        *Kişi ayrıntıları*
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün. Kişi varlığı için tüm CRUD - modelinden görünümlere - uygulamanızda tek satırlık bir kod yazmak zorunda kalmadan oluşturduğunuz dikkat edin!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Görev 3-güncelleştirme veritabanını kullanarak Entity Framework geçişleri

Bu görevde, varlık çerçevesi geçişleriyle kullanarak veritabanını güncelleştirir. Modeli değiştirmek ve kurum çerçevesi geçişlerine özelliğini kullanarak veritabanlarınızı değişiklikleri yansıtmak için ne kadar kolay olduğunu keşfeder.

1. Paket Yöneticisi konsolu açın. Seçin **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Geçişleri etkinleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "geçişler etkinleştirme")

    *Geçiş etkinleştiriliyor*

    Geçişi etkinleştirmeyi komut oluşturur **geçişler** veritabanı başlatmak üzere bir betiğin bulunduğu klasör.

    ![Geçişleri klasör](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "geçişler klasörü")

    *Geçişleri klasörü*
3. Açık **Configuration.cs** geçişler klasöründeki dosya. Sınıf oluşturucu bulun ve değiştirin **AutomaticMigrationsEnabled** değerini *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Kişi sınıfını açın ve kişinin ikinci adı için bir öznitelik ekleyin. Bu yeni öznitelikle modelini değiştiriyorsunuz.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seçin **yapı | Çözüm yapı** uygulamayı oluşturmak için menüde.

    ![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "uygulama oluşturma")

    *Uygulama oluşturma*
6. Paket Yöneticisi konsolunda aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Bu komut veri nesnelerini değişiklikleri arar ve ardından, veritabanını uygun şekilde değiştirmek için gerekli komutları ekleyeceksiniz.

    ![İkinci ad ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ikinci adı ekleme")

    *İkinci adı ekleme*
7. (İsteğe bağlı) Fark güncelleştirme ile bir SQL betiği oluşturmak için aşağıdaki komutu çalıştırabilirsiniz. Bu, veritabanını el ile güncelleştirmenizi sağlar (Bu durumda gerekli değildir), veya diğer veritabanlarındaki değişiklikleri uygulayın:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![SQL komut dosyası oluşturuluyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL komut dosyası oluşturuluyor")

    *SQL komut dosyası oluşturuluyor*

    ![SQL betiği güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL betiğini güncelleştirme")

    *SQL komut dosyasını güncelleştirme*
8. Paket Yöneticisi Konsolu'nda veritabanını güncelleştirmek için aşağıdaki komutu girin:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Veritabanını güncelleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "veritabanı güncelleştiriliyor")

    *Veritabanı güncelleştiriliyor*

    Bu ekler **MiddleName** sütununda **kişiler** geçerli tanımı eşleştirilecek tablo **kişi** sınıfı.
9. Veritabanı güncelleştirildikten sonra denetleyici klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici** kişi denetleyicisi yeniden (aynı değerlere sahip eksiksiz) eklemek için. Bu, yeni bir öznitelik ekleme görünümleri ve var olan yöntemler güncelleştirir.

    ![Denetleyici güncelleştirmesi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "denetleyicisi güncelleştirme ekleme")

    *Denetleyici güncelleştiriliyor*
10. **Ekle**'yi tıklatın. Ardından, değerleri seçin **üzerine PersonController.cs** ve **üzerine yazma görünümleri ilişkili** tıklatıp **Tamam**.

   ![Ekleme denetleyicisi üzerine yaz](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Denetleyici güncelleştiriliyor*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - uygulama çalıştırma

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Açık **/Person**. İkinci Ad sütunu eklendi ancak verileri, korundu, dikkat edin.

    ![İkinci Ad eklenen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ortasına adı eklendi")

    *İkinci Ad eklendi*
3. Tıklarsanız **Düzenle**, geçerli kişinin ikinci adı eklemek mümkün olacaktır.

    ![İkinci Ad edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ikinci adı sürümü")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı bir laboratuvarda, basit CRUD işlemleri ile ASP.NET MVC 4 herhangi bir model sınıfı kullanarak İskele oluşturma adımlarını öğrendiniz. Ardından, varlık çerçevesi geçişleriyle kullanarak uygulamanızda - veritabanından görünümler - uçtan uca bir güncelleştirme gerçekleştirmek nasıl öğrendiniz.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Web kutucuğu için VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
