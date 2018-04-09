---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modelleri ve veri erişimi | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı Laboratuvar ASP.NET MVC temel bilgiye sahip olduğunu varsayar. ASP.NET MVC önce kullanmadıysanız, ASP.NET MVC 4 gitmenizi öneririz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 modelleri ve veri erişimi

Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](https://aka.ms/webcamps-training-kit)

Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**. Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** uygulamalı Laboratuvar.

Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.

> [!NOTE]
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 modelleri ve veri erişimi](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

İçinde **ASP.NET MVC Temelleri** uygulamalı Laboratuvar, sabit kodlanmış veri geçirerek denetleyicilerinden görünüm şablonları. Ancak, gerçek bir Web uygulaması oluşturmak için gerçek bir veritabanını kullanmak isteyebilirsiniz.

Bu uygulamalı Laboratuvar depolamak ve müzik deposu uygulaması için gereken verileri almak için bir veritabanı motoru kullanmak nasıl yapacağınızı gösterir. Bunu yapmaya yönelik var olan bir veritabanı başlatın ve varlık veri modeli oluşturmak. Bu Laboratuvar, karşılayacağını **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım.

Ancak, aynı zamanda kullanabilirsiniz **Model First** yaklaşımını, araçları kullanarak aynı modelin oluşturun ve ondan veritabanı oluşturun.

![Veritabanı ilk vs. Model ilk](aspnet-mvc-4-models-and-data-access/_static/image1.png "veritabanı ilk vs. Model First")

*Veritabanı ilk vs. Model First*

Model oluşturma sonra StoreController sabit kodlanmış verileri kullanmak yerine veritabanından alınan veri deposu görünümlerini sağlamak için uygun düzeltmeleri yapar. Görünüm şablonları için aynı ViewModels StoreController döndürme çünkü bu kez veritabanından veri gelir ancak herhangi bir değişiklik görünüm şablonlarda yapmak gerekmez.

**İlk kod yaklaşımı**

Code First yaklaşım bize genellikle framework ile birlikte sınıfları oluşturmadan kodu modelden tanımlamanızı sağlar.

Kodda, ilk olarak, model nesneleri POCOs ile tanımlanan &quot;düz eski CLR nesnelerini&quot;. POCOs hiçbir devralma ve arabirimleri kullanılmaz basit düz sınıflarıdır. Biz otomatik olarak veritabanı onlardan oluşturabilir veya biz varolan bir veritabanını kullanın ve sınıf eşleme kodundan oluşturduğunuz.

Bu yaklaşımı kullanmanın avantajları olan POCOs sınıfları eşleme framework ile bağlı değil olarak Model (Bu durumda, Entity Framework), Kalıcılık çerçeveden bağımsız olarak kalır.

> [!NOTE]
> Bu Laboratuvar ASP.NET MVC 4 temel alır ve müzik deposu örnek uygulama sürümü özelleştirilmiş ve yalnızca bu uygulamalı laboratuar ortamında gösterilen özellikleri uyacak şekilde simge durumuna küçültülmüş.
> 
> Bütün araştırmak istiyorsanız **müzik deposu** içinde bulabilirsiniz öğretici uygulama [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).


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

1. [Alıştırma 1: bir veritabanına ekleme](#Exercise1)
2. [Alıştırma 2: Code First kullanarak veritabanı oluşturma](#Exercise2)
3. [Alıştırma 3: parametrelerle veritabanını sorgulama](#Exercise3)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **35 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Alıştırma 1: bir veritabanına ekleme

Bu alıştırmada, kendi veri tüketmek için çözüm MusicStore uygulamaya tablolar ile bir veritabanı eklemek öğreneceksiniz. Veritabanı modeli ile oluşturulan ve çözüme eklendi sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController sınıfı değiştirecektir.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Görev 1 - bir veritabanı ekleme

Bu görevde, çözüme MusicStore uygulamasının ana tablolarla önceden oluşturulmuş bir veritabanına ekler.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-AddingADatabaseDBFirst/başlangıç/** klasörü.

   1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Ekleme **MvcMusicStore** veritabanı dosyası. Bu uygulamalı laboratuar ortamında adlı bir veritabanı zaten oluşturuldu kullanacağı **MvcMusicStore.mdf**. Bunu yapmak için sağ **uygulama\_veri** klasörünü **Ekle** ve ardından **varolan öğeyi**. Gözat **\Source\Assets** seçip **MvcMusicStore.mdf** dosya.

    ![Var olan bir öğe ekleme](aspnet-mvc-4-models-and-data-access/_static/image2.png "var olan bir öğe ekleme")

    *Var olan bir öğe ekleme*

    ![MvcMusicStore.mdf veritabanı dosyası](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf veritabanı dosyası")

    *MvcMusicStore.mdf veritabanı dosyası*

    Veritabanı projeye eklendi. Veritabanı içinde çözüm bile bulunur, sorgu ve onu farklı bir veritabanı sunucusu barındırılan gibi güncelleştirin.

    ![Çözüm Gezgini'nde MvcMusicStore veritabanı](aspnet-mvc-4-models-and-data-access/_static/image4.png "Çözüm Gezgini'nde MvcMusicStore veritabanı")

    *Çözüm Gezgini'nde MvcMusicStore veritabanı*
3. Veritabanı bağlantısı doğrulayın. Bunu yapmak için çift **MvcMusicStore.mdf** bağlantı kuramıyor.

    ![MvcMusicStore.mdf için bağlanma](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf için bağlanma")

    *MvcMusicStore.mdf için bağlanma*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Görev 2 - bir veri modeli oluşturma

Bu görevde, önceki görevde eklenen veritabanıyla etkileşim kurmak için bir veri modeli oluşturur.

1. Veritabanı temsil edecek bir veri modeli oluşturun. Bu, Çözüm Gezgini içinde yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** şablonu ve ardından **ADO.NET varlık veri modeli** öğesi. Veri modeli adı değiştirmek **StoreDB.edmx** tıklatıp **Ekle**.

    ![StoreDB ADO.NET varlık veri modeli ekleme](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET varlık veri modeli ekleme")

    *StoreDB ADO.NET varlık veri modeli ekleme*
2. **Varlık veri modeli Sihirbazı** görünür. Bu sihirbaz, modeli katmanı oluşturulmasını yardımcı olur. Model eklenen varolan veritabanı recentyl tabanlı oluşturulmalıdır beri seçin **veritabanından Oluştur** tıklatıp **sonraki**.

    ![Model içeriğinin seçme](aspnet-mvc-4-models-and-data-access/_static/image7.png "model içeriğinin seçme")

    *Model içeriğinin seçme*
3. Model bir veritabanından oluşturma olduğundan, bağlantıyı kullanacak şekilde belirtmeniz gerekir. Tıklatın **yeni bağlantı**.
4. Seçin **Microsoft SQL Server veritabanı dosyası** tıklatıp **devam**.

    ![Veri Kaynağı Seç](aspnet-mvc-4-models-and-data-access/_static/image8.png "Seç veri kaynağı")

    *Veri kaynağı iletişim seçin*
5. Tıklatın **Gözat** ve veritabanını seçin **MvcMusicStore.mdf** içinde bulunan **uygulama\_veri** klasörü ve tıklatın **Tamam**.

    ![Bağlantı özellikleri](aspnet-mvc-4-models-and-data-access/_static/image9.png "bağlantı özellikleri")

    *Bağlantı özellikleri*
6. Oluşturulan sınıf varlık bağlantı dizesi ile aynı ada sahip, bu nedenle adının değiştirmek **MusicStoreEntities** tıklatıp **sonraki**.

    ![Veri bağlantısı seçme](aspnet-mvc-4-models-and-data-access/_static/image10.png "veri bağlantısı seçme")

    *Veri bağlantısı seçme*
7. Kullanmak için veritabanı nesneleri seçin. Varlık modeli yalnızca veritabanı tabloları kullanacak şekilde seçin **tabloları** seçeneği ve olduğundan emin olun **yabancı anahtar sütunlarını modele dahil** ve **Pluralize veya tekil hale getirin oluşturulan Nesne adları** seçenekleri da seçilir. Model Namespace değiştirme **MvcMusicStore.Model** tıklatıp **son**.

    ![Veritabanı nesneleri seçme](aspnet-mvc-4-models-and-data-access/_static/image11.png "veritabanı nesneleri seçme")

    *Veritabanı nesneleri seçme*

    > [!NOTE]
    > Bir güvenlik uyarısı iletişim kutusu gösterilirse, tıklatın **Tamam** şablonu çalıştırın ve model varlıkların sınıfları oluşturmak için.
8. Veritabanı için bir varlığın diyagram, her tablo veritabanına eşlemeleri ayrı bir sınıf oluşturulacak sırada görünür. Örneğin, **albümleri** tabloda belirtildiği bir **albüm** sınıfı, burada tablodaki her sütun eşlemek için bir sınıf özelliği. Bu, sorgu ve veritabanında satır temsil eden nesneler üzerinde çalışmak olanak tanır.

    ![Varlık diyagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "varlık diyagramı")

    *Varlık diyagramı*

    > [!NOTE]
    > T4 şablonları (.tt) varlıklar sınıfları oluşturmak için kodu çalıştırın ve aynı ada sahip mevcut sınıflarını üzerine yazar. Bu örnekte, sınıfları &quot;albüm&quot;, &quot;Tarz&quot; ve &quot;sanatçı&quot; ile oluşturulan kodun verilerinin üzerine yazıldı.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Görev 3 - uygulama oluşturma

Model oluşturma kaldırıldı. ancak bu görevde, kontrol eder **albüm**, **Tarz** ve **sanatçı** modeli sınıfları, projenin başarıyla kullanarak oluşturur Yeni veri modeli sınıfları.

1. Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.

    ![Proje derleme](aspnet-mvc-4-models-and-data-access/_static/image13.png "projesi oluşturma")

    *Proje derleme*
2. Proje başarıyla oluşturulur. Neden hala çalışır? Veritabanı tablolarını kaldırılan sınıflarda kullanmakta olduğunuz özellikleri içeren alanlar olduğundan çalışır **albüm** ve **Tarz**.

    ![Başarılı bir şekilde derlemeleri](aspnet-mvc-4-models-and-data-access/_static/image14.png "başarılı derlemeleri")

    *Derlemeleri başarılı oldu*
3. Tasarımcı varlıklar diyagramı biçiminde görüntüler olsa da, bunlar gerçekten C# sınıflarıdır. Genişletme **StoreDB.edmx** Çözüm Gezgini'nde düğümünü ve ardından **StoreDB.tt**, yeni oluşturulan varlıkları görürsünüz.

    ![Oluşturulan dosyaları](aspnet-mvc-4-models-and-data-access/_static/image15.png "oluşturulan dosyaları")

    *Oluşturulan dosyalar*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Görev 4 - veritabanını sorgulama

Bu görevde, sabit kodlanmış verileri kullanmak yerine, bu bilgileri almak için veritabanı sorgulayacak böylece StoreController sınıfı güncelleştirir.

1. Açık **Controllers\StoreController.cs** ve aşağıdaki alan örneği tutacak sınıfına ekleyin **MusicStoreEntities** adlı sınıf **storeDB**:

    (Kod parçacığını - *modelleri ve veri erişimi - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. **MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar. Güncelleştirme **Gözat** bir tarzını tüm almak için eylem yöntemini **albümleri**.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu Gözat*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. Güncelleştirme **dizin** tüm türler almak için eylem yöntemi.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu dizini*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. Güncelleştirme **dizin** tüm türler almak ve bir liste koleksiyona dönüştürmek için eylem yöntemi.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde deposu dizin sayfası kodlanmış olanları yerine veritabanında depolanan türler artık göstereceğini kontrol eder. Şablonu görüntüleme çünkü değiştirmenize gerek yoktur **StoreController** bu kez veritabanından veri gelir ancak aynı varlık önceki gibi döndüren.

1. Tuşuna basın ve çözüm yeniden **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Doğrulayın menüsünü **türler** artık sabit kodlanmış bir listesidir ve verileri doğrudan veritabanından alınır.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Veritabanından gözatma türler*
3. Şimdi bir tarzını gözatın ve albümleri veritabanından doldurulur doğrulayın.

    ![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image17.png "gözatma albümleri veritabanından")

    *Veritabanından albümleri gözatma*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Alıştırma 2: İlk kod kullanarak bir veritabanı oluşturma

Bu alıştırmada, kod ilk yaklaşım MusicStore uygulama tablolarla bir veritabanı oluşturmak için nasıl kullanılacağı ve onun verilere nasıl erişileceğini öğreneceksiniz.

Model oluşturulduktan sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController değiştirecektir.

> [!NOTE]
> Alıştırma 1 tamamladıktan ve veritabanı ilk yaklaşımda zaten çalıştıysa farklı bir işlem ile aynı sonuçları almak nasıl şimdi öğreneceksiniz. Alıştırma 1 ortak olan görevleri okuma kolaylaştırmak için işaretlendi. Alıştırma 1 tamamlamadıysanız, ancak ilk kod yaklaşımı öğrenmek ister misiniz Bu alıştırmada başlatın ve tam kapsamı konunun alın.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Görev 1 - doldurma örnek veriler

Kod ilk kullanılarak başlangışta oluşturulduğunda bu görevde, veritabanı örnek verileri ile doldurur.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-CreatingADatabaseCodeFirst/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Ekleme **SampleData.cs** dosya **modelleri** klasör. Bunu yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **varolan öğeyi**. Gözat **\Source\Assets** seçip **SampleData.cs** dosya.

    ![Örnek verileri doldurmak kod](aspnet-mvc-4-models-and-data-access/_static/image18.png "örnek verileri doldurmak kodu")

    *Örnek verileri kod doldurma*
3. Açık **Global.asax.cs** dosya ve aşağıdakileri ekleyin *kullanarak* deyimleri.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 genel Asax kullanımları*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. İçinde **uygulama\_Start()** yöntemi veritabanı Başlatıcısı ayarlamak için aşağıdaki satırı ekleyin.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 genel Asax SetInitializer*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Görev 2 - veritabanı bağlantısı yapılandırma

Projemizin için bir veritabanı zaten eklenmiş, yazacağınız **Web.config** bağlantı dizesi dosya.

1. Konumunda bir bağlantı dizesi eklemek **Web.config**. Bunu yapmak için açık **Web.config** proje kök ve bağlantı dizesi bu satırda ile DefaultConnection adlı Değiştir **&lt;connectionStrings&gt;** bölümü:

    ![Web.config dosyası konumu](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config dosyası konumu")

    *Web.config dosyası konumu*


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Görev 3 - modeli ile çalışma

Veritabanı bağlantısı zaten yapılandırdığınıza göre veritabanı tablolarını modeliyle bağlayacaksınız. Bu görevde, Code First olan veritabanına bağlı bir sınıf oluşturur. Değiştirilmesi gereken mevcut bir POCO model sınıfı olduğunu unutmayın.

   > [!NOTE]
> Alıştırma 1 tamamladıysa, bu adım sihirbaz tarafından gerçekleştirildi Not. Code First yaparak, veri varlıklarına bağlı sınıfları el ile oluşturur.


1. POCO model sınıfı açmak **Tarz** gelen **modelleri** proje klasörünü ve bir kimliğini de ekleyin. İnt özelliği addaki **GenreId**.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk Tarz*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. Şimdi, POCO model sınıfı açmak **albüm** gelen **modelleri** proje klasörünü ve yabancı anahtarlar dahil, adlarıyla özellikleri oluşturma **GenreId** ve  **ArtistId**. Bu sınıf zaten **GenreId** birincil anahtar.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk albüm*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. POCO model sınıfı açmak **sanatçı** ve dahil **ArtistId** özelliği.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk sanatçı*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. Sağ **modelleri** proje klasörünü ve select **Ekle | Sınıf**. Dosya adı **MusicStoreEntities.cs**. Ardından **Ekle.**

    ![Sınıf ekleme](aspnet-mvc-4-models-and-data-access/_static/image20.png "sınıf ekleme")

    *Yeni bir öğe ekleme*

    ![Bir Ders2 ekleme](aspnet-mvc-4-models-and-data-access/_static/image21.png "bir Ders2 ekleme")

    *Sınıf ekleme*
5. Az önce oluşturduğunuz, sınıfın açık **MusicStoreEntities.cs**ve ad alanlarını dahil **System.Data.Entity** ve **System.Data.Entity.Infrastructure**.


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. Genişletmek için sınıf bildirimi Değiştir **DbContext** sınıfı: Genel bildirme **DBSet** ve geçersiz kılma **OnModelCreating** yöntemi. Bu adımdan sonra modelinizi Entity Framework bağlayacak bir etki alanı sınıf alırsınız. Bunu yapmak için sınıf kodu aşağıdakilerle değiştirin:

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk MusicStoreEntities*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Görev 4 - veritabanını sorgulama

Bu görevde, sabit kodlanmış verileri kullanmak yerine, onu veritabanından alır, böylece StoreController sınıfı güncelleştirir.

> [!NOTE]
> Bu görev ortak alıştırma 1 ' dir.
> 
> Alıştırma 1 tamamladıysanız şu adımları her iki yaklaşımın aynı olduğunu unutmayın (ilk veritabanı veya ilk kod). Verileri nasıl modelle bağlı olarak farklı, ancak veri varlıklara erişim henüz denetleyicisinden saydamdır.


1. Açık **Controllers\StoreController.cs** ve aşağıdaki alan örneği tutacak sınıfına ekleyin **MusicStoreEntities** adlı sınıf **storeDB**:

    (Kod parçacığını - *modelleri ve veri erişimi - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. **MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar. Güncelleştirme **Gözat** bir tarzını tüm almak için eylem yöntemini **albümleri**.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu Gözat*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. Güncelleştirme **dizin** tüm türler almak için eylem yöntemi.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu dizini*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. Güncelleştirme **dizin** tüm türler almak ve bir liste koleksiyona dönüştürmek için eylem yöntemi.

    (Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde deposu dizin sayfası kodlanmış olanları yerine veritabanında depolanan türler artık göstereceğini kontrol eder. Şablonu görüntüleme çünkü değiştirmenize gerek yoktur **StoreController** aynı döndürmektir **StoreIndexViewModel** eskisi ancak bu kez verileri veritabanından gelen.

1. Tuşuna basın ve çözüm yeniden **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Doğrulayın menüsünü **türler** artık sabit kodlanmış bir listesidir ve verileri doğrudan veritabanından alınır.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Veritabanından gözatma türler*
3. Şimdi bir tarzını gözatın ve albümleri veritabanından doldurulur doğrulayın.

    ![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image23.png "gözatma albümleri veritabanından")

    *Veritabanından albümleri gözatma*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Alıştırma 3: parametrelerle veritabanını sorgulama

Bu alıştırmada parametreleri kullanarak veritabanını sorgulama ve sorgu sonucu şekillendirme kullanmayı öğreneceksiniz, numara veritabanı azaltan bir özellik alma verileri daha verimli bir şekilde erişir.

> [!NOTE]
> Sorgu sonucu şekillendirme hakkında daha fazla bilgi için aşağıdaki ziyaret [msdn makalesine](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Görev 1 - değiştirme StoreController albümleri veritabanından almak için

Bu görevde, değişiklik yapacağınız **StoreController** belirli bir tarzını albümleri almak için veritabanına erişmek için sınıf.

1. Açık **başlamak** çözüm bulunan **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** ilk kod yaklaşımı kullanmak istiyorsanız, klasör veya **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** veritabanı ilk yaklaşımı kullanmak istiyorsanız, klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
   3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

      > [!NOTE]
      > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreController** değiştirmek için sınıf **Gözat** eylem yöntemi. Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.
3. Değişiklik **Gözat** belirli bir tarzını albümleri almak için eylem yöntemi. Bunu yapmak için aşağıdaki kodu değiştirin:

    (Kod parçacığını - *modelleri ve veri erişimi - Ex3 StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulama çalışıyor

Bu görevde, uygulamayı çalıştırın ve belirli bir tarzını albümleri veritabanından.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/deposu/Gözat? Tarz Pop =** sonuçları veritabanından alınmakta olduğunu doğrulayın.

    ![Türe göre gözatma](aspnet-mvc-4-models-and-data-access/_static/image24.png "türe göre gözatma")

    *Tarama/deposu/Gözat? Tarz Pop =*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Görev 3 - kimliği albümlerini erişme

Bu görevde, Albümler kimliklerine göre almak için önceki yordamı tekrar edilir

1. Gerekirse Visual Studio'ya dönmek için tarayıcıyı kapatın. Açık **StoreController** değiştirmek için sınıf **ayrıntıları** eylem yöntemi. Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.
2. Değişiklik **ayrıntıları** albümleri ayrıntıları almak için eylem yöntemine bağlı olarak kendi **kimliği**. Bunu yapmak için aşağıdaki kodu değiştirin:

    (Kod parçacığını - *modelleri ve veri erişimi - Ex3 StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında çalışacak ve kimliklerine göre Albüm ayrıntılarını alma

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/Store/Details/51** veya türler göz atın ve sonuçları veritabanından alınmakta olduğunu doğrulamak için bir albümü seçin.

    ![Ayrıntılar gözatma](aspnet-mvc-4-models-and-data-access/_static/image25.png "ayrıntıları gözatma")

    */Store/details/51 gözatma*

> [!NOTE]
> Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

ASP.NET MVC modelleri ve veri erişimi temelleri öğrenilen Bu uygulamalı Laboratuvar Tamamlanıyor, kullanarak **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım:

- Bir veritabanı çözümü kendi veri tüketmek için ekleme
- Görünüm şablonları sabit kodlanmış bir yerine veritabanından alınan veri sağlamak için denetleyicileri güncelleştirme
- Parametreleri kullanarak veritabanını sorgulama
- Sorgu sonuç verileri daha verimli bir şekilde alma şekillendirme, veritabanı erişimlerine sayısını azaltan bir özellik kullanma
- Veritabanı ilk ve Code First yaklaşımlar Microsoft Entity Framework modelini veritabanıyla bağlantı için nasıl kullanılacağını

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-models-and-data-access/_static/image26.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Windows Azure portalında oturum açtığı](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açtığı*
2. Tıklatın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image32.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklatın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.

    > [!NOTE]
    > Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image33.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun. Yeni Web sitesi çalıştığından emin olun.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-models-and-data-access/_static/image34.png "yeni web sitesi için gözatma")

    *Yeni web sitesi için gözatma*

    ![Çalışan Web sitesi](aspnet-mvc-4-models-and-data-access/_static/image35.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-models-and-data-access/_static/image36.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.

    > [!NOTE]
    > *Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.

    ![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-models-and-data-access/_static/image37.png "yayımlama profili web sitesi yükleniyor")

    *Yayımlama profili Web sitesi yükleniyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-models-and-data-access/_static/image38.png "yayımlama profilini kaydetme")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.

1. Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir. Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme. Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi. Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucusu Pano](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**. Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylamak](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Değişiklikleri onaylamak*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-models-and-data-access/_static/image43.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-models-and-data-access/_static/image44.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklatın **bağlantısı doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-models-and-data-access/_static/image45.png "bağlantısı doğrulanıyor")

    *Doğrulama bağlantısı*
4. İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.
   - İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.
   - İçinde **parola** sunucu yönetici oturum açma parolasını yazın.
   - Yeni bir veritabanı adı yazın.

     ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image47.png "hedef bağlantı dizesi yapılandırma")

     *Hedef bağlantı dizesi yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-models-and-data-access/_static/image48.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturuluyor*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanına işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında, **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-models-and-data-access/_static/image50.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-models-and-data-access/_static/image51.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-models-and-data-access/_static/image52.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-models-and-data-access/_static/image53.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-models-and-data-access/_static/image54.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-models-and-data-access/_static/image55.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-models-and-data-access/_static/image56.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
