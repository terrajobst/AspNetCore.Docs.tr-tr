---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modelleri ve veri erişimi | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı laboratuvarı ASP.NET MVC temel bilgiye sahip olduğunu varsayar. ASP.NET MVC 4 gitmenizi öneririz önce ASP.NET MVC kullanmadıysanız...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371135"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 modelleri ve veri erişimi

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC**. Kullanmadıysanız, **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** Hands-on-Lab.

Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.

> [!NOTE]
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 modelleri ve verilere erişim](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

İçinde **ASP.NET MVC ile ilgili temel bilgiler** uygulamalı Laboratuvar, sabit kodlanmış veri geçirerek denetleyicilerinden için görüntüleme şablonları. Ancak gerçek bir Web uygulaması oluşturmak için gerçek bir veritabanı kullanmak isteyebilirsiniz.

Bu uygulamalı laboratuvarı müzik Store uygulaması için gereken verileri almak ve depolamak için bir veritabanı altyapısının nasıl kullanıldığını gösterir. Bunu yapmaya yönelik var olan bir veritabanına başlamak ve varlık veri Modeli'ni oluşturmak. Bu Laboratuvar karşılayacak mı **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım.

Ancak, ayrıca kullanabileceğiniz **modeli ilk** yaklaşımı, aynı araçları kullanarak model oluşturma ve ondan sonra veritabanını oluşturmak.

![Veritabanı ilk vs. Model ilk](aspnet-mvc-4-models-and-data-access/_static/image1.png "veritabanı ilk vs. İlk model")

*Veritabanı ilk vs. İlk model*

Model oluşturduktan sonra StoreController Store görünümleri sabit kodlanmış verilerin kullanmak yerine veritabanından alınan veri sağlamak için de uygun ayarlamalar yapar. Görünüm şablonları için aynı Viewmodel'lar StoreController döndüren çünkü bu kez, veritabanından veri gelir ancak herhangi bir değişiklik görünümü şablonlarda yapmak gerekmez.

**İlk kod yaklaşımı**

İlk kod yaklaşımı genellikle framework ile bağlı sınıfları oluşturmadan kod modeli tanımlamak sağlıyor.

Kodda, ilk olarak, model nesneleri POCOs ile tanımlanan &quot;düz eski CLR nesnelerini&quot;. POCOs hiçbir devralma ve arabirimler uygulayamaz basit düz sınıflardır. Biz otomatik olarak veritabanı bunları oluşturabilir veya biz varolan veritabanını kullan ve koddan sınıf eşleme oluşturur.

Bu yaklaşımı kullanmanın avantajları olduğundan POCOs sınıfları eşleme framework ile bağlı değil olarak Model (Bu örnekte, Entity Framework), Kalıcılık çerçeveden bağımsız olarak kalır.

> [!NOTE]
> ASP.NET MVC 4'te bu Laboratuvar temel alır ve müzik Store örnek uygulamanın bir sürümü yalnızca bu uygulamalı laboratuvarda gösterilen özellikler sığdırmak için simge durumuna küçültülmüş ve özelleştirilebilir.
> 
> Tüm keşfetmek istiyorsanız **müzik Store** öğretici uygulama içinde bulabilirsiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).


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

1. [Alıştırma 1: bir veritabanı ekleme](#Exercise1)
2. [Alıştırma 2: Code First kullanarak veritabanı oluşturma](#Exercise2)
3. [Alıştırma 3: parametrelerle veritabanını sorgulama](#Exercise3)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **35 dakika**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Alıştırma 1: bir veritabanı ekleme

Bu alıştırmada, bir veritabanı tabloları verilerini kullanmak MusicStore uygulamanın çözüme ekleyin öğreneceksiniz. Veritabanı modeli ile oluşturulan ve çözüme sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController sınıfı değiştirir.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Görev 1 - veritabanı ekleme

Bu görevde, önceden oluşturulmuş bir veritabanı çözümü MusicStore uygulamanın ana tablolarla ekleyeceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-AddingADatabaseDBFirst/başlangıç/** klasör.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Ekleme **MvcMusicStore** veritabanı dosyası. Bu uygulamalı laboratuvarda adlı önceden oluşturulmuş bir veritabanı kullanacağınız **MvcMusicStore.mdf**. Bunu yapmak için sağ **uygulama\_veri** klasörünü **Ekle** ve ardından **var olan öğe**. Gözat **\Source\Assets** seçip **MvcMusicStore.mdf** dosya.

    ![Var olan bir öğe eklemeye](aspnet-mvc-4-models-and-data-access/_static/image2.png "var olan öğe ekleme")

    *Var olan öğe ekleme*

    ![Veritabanı dosyası MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf veritabanı dosyası")

    *MvcMusicStore.mdf veritabanı dosyası*

    Veritabanı projeye eklendi. Veritabanı içinde çözüm bile bulunur, sorgu ve farklı bir veritabanı sunucusu barındırılan olarak güncelleştirin.

    ![Çözüm Gezgini'nde MvcMusicStore veritabanı](aspnet-mvc-4-models-and-data-access/_static/image4.png "Çözüm Gezgini'nde MvcMusicStore veritabanı")

    *Çözüm Gezgini'nde MvcMusicStore veritabanı*
3. Veritabanı bağlantısını doğrulayın. Bunu yapmak için çift **MvcMusicStore.mdf** ile bağlantı kurar.

    ![Bağlanmak için MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "için MvcMusicStore.mdf bağlanma")

    *İçin MvcMusicStore.mdf bağlanma*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Görev 2 - bir veri modeli oluşturma

Bu görevde, önceki görevde eklenen veritabanıyla etkileşim kurmak için bir veri modeli oluşturur.

1. Veritabanı temsil edecek bir veri modeli oluşturun. Çözüm Gezgini sağ tıklatın Bunu yapmak için **modelleri** klasörünü **Ekle** ve ardından **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** şablonu ve ardından **ADO.NET varlık veri modeli** öğesi. Veri modeli adı için değiştirme **StoreDB.edmx** tıklatıp **Ekle**.

    ![StoreDB ADO.NET varlık veri modeli ekleme](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET varlık veri modeli ekleme")

    *StoreDB ADO.NET varlık veri modeli ekleme*
2. **Varlık veri modeli Sihirbazı** görünür. Bu sihirbaz modeli katmanı oluşturulmasını adım yönlendirecektir. Model eklenen var olan veritabanı recentyl tabanlı oluşturulmalıdır. bu yana seçin **veritabanından Oluştur** tıklatıp **sonraki**.

    ![Model içeriğinin seçme](aspnet-mvc-4-models-and-data-access/_static/image7.png "model içeriğinin seçme")

    *Model içeriğinin seçme*
3. Bir veritabanından model oluşturma olduğundan, kullanılacak bağlantı belirtmek gerekir. Tıklayın **yeni bağlantı**.
4. Seçin **Microsoft SQL Server veritabanı dosyası** tıklatıp **devam**.

    ![Veri Kaynağı Seç](aspnet-mvc-4-models-and-data-access/_static/image8.png "veri kaynağı Seç")

    *Veri kaynağı iletişim seçin*
5. Tıklayın **Gözat** veritabanını seçin **MvcMusicStore.mdf** bulunuyorsunuz **uygulama\_veri** klasörü ve tıklatın **Tamam**.

    ![Bağlantı özellikleri](aspnet-mvc-4-models-and-data-access/_static/image9.png "bağlantı özellikleri")

    *Bağlantı özellikleri*
6. Oluşturulan sınıfın varlık bağlantı dizesini aynı ada sahip, bu nedenle adını değiştirmek **MusicStoreEntities** tıklatıp **sonraki**.

    ![Veri bağlantısı seçerek](aspnet-mvc-4-models-and-data-access/_static/image10.png "veri bağlantısını seçme")

    *Veri bağlantısını seçme*
7. Veritabanı nesneleri seçin. Varlık modeli yalnızca veritabanı tablolarını kullanacak şekilde seçin **tabloları** seçenek ve emin **modelde yabancı anahtar sütunlarını içermesi** ve **Pluralize veya singularize oluşturulan Nesne adları** seçenekleri da seçilir. Değiştirmek için Model Namespace **MvcMusicStore.Model** tıklatıp **son**.

    ![Veritabanı nesneleri seçme](aspnet-mvc-4-models-and-data-access/_static/image11.png "veritabanı nesneleri seçme")

    *Veritabanı nesneleri seçme*

    > [!NOTE]
    > Bir güvenlik uyarısı iletişim kutusu gösteriliyorsa, tıklayın **Tamam** şablonu çalıştırın ve model varlıklar için sınıflar oluşturun.
8. Veritabanı için bir varlık diyagramda görünür, bu sırada her tablo veritabanına eşlenen ayrı bir sınıf oluşturulacak. Örneğin, **albümleri** tabloda belirtildiği bir **albüm** sınıfı burada tablodaki her bir sütunun eşlemek için bir sınıf özelliği. Bu, sorgulama ve veritabanındaki satırları temsil eden nesneleri ile çalışmanıza olanak sağlar.

    ![Varlık diyagramda](aspnet-mvc-4-models-and-data-access/_static/image12.png "varlık diyagramı")

    *Varlık diyagramı*

    > [!NOTE]
    > T4 şablonları (.tt), varlık sınıfları üretmek için kod çalıştırın ve mevcut sınıfları aynı ada sahip üzerine yazar. Bu örnekte, sınıfları &quot;albüm&quot;, &quot;Tarz&quot; ve &quot;sanatçının&quot; ile oluşturulan kodun üzerine yazıldı.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Görev 3 - uygulama oluşturma

Model oluşturma kaldırıldı ancak bu görevde, kontrol **albüm**, **Tarz** ve **sanatçının** model sınıfları, projeye başarıyla kullanarak oluşturur Yeni veri modeli sınıfları.

1. Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.

    ![Proje derleme](aspnet-mvc-4-models-and-data-access/_static/image13.png "projesi oluşturma")

    *Proje oluşturma*
2. Projeyi başarıyla derler. Neden hala çalışır? Veritabanı tablolarını kaldırılan sınıflarda, kullandığınız özellikleri içeren alanlar olduğundan çalıştığını **albüm** ve **Tarz**.

    ![Derlemeler başarılı](aspnet-mvc-4-models-and-data-access/_static/image14.png "derlemeler başarılı oldu")

    *Derlemeler başarılı oldu*
3. Tasarımcı varlıklar diyagramı biçiminde görüntülerken, bunlar gerçekten C# sınıfları. Genişletin **StoreDB.edmx** Çözüm Gezgini'nde düğümünü ve ardından **StoreDB.tt**, yeni oluşturulan varlıkları görürsünüz.

    ![Oluşturulan dosyaları](aspnet-mvc-4-models-and-data-access/_static/image15.png "oluşturulan dosyaları")

    *Oluşturulan dosyalar*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Görev 4 - veritabanı sorgulama

Bu görevde, sabit kodlanmış veri kullanmak yerine, bu bilgileri almak için bu veritabanını sorgulamanızı böylece StoreController sınıfı güncelleştirir.

1. Açık **Controllers\StoreController.cs** ve şu alan örneği için Sınıf Ekle **MusicStoreEntities** adlı bir sınıf **storeDB**:

    (Kod parçacığını - *modeller ve veri erişimi - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar. Güncelleştirme **Gözat** tüm bir türe almak için eylem yöntemini **albümleri**.

    (Kod parçacığını - *modeller ve veri erişimi - Ex1 Store Gözat*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Bir özellik adında .NET, kullanmakta olduğunuz **LINQ** (kod veritabanında yürütün ve dönüş bu koleksiyonlara karşı-kesin türü belirtilmiş sorgu ifadeleri yazmak için dil ile tümleşik sorgu) nesneleri, programlama yapabilirsiniz karşı.
    > 
    > LINQ hakkında daha fazla bilgi için lütfen [msdn sitesine](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Güncelleştirme **dizin** tüm türleri almak için eylem yöntemi.

    (Kod parçacığını - *modeller ve veri erişimi - Ex1 Store dizini*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Güncelleştirme **dizin** tüm türleri almak ve liste koleksiyon dönüştürmek için eylem yöntemi.

    (Kod parçacığını - *modeller ve veri erişimi - Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, Store dizin sayfası yerine ' % s'belirtebilseydik olanları veritabanında depolanan türleri artık göstereceğini kontrol eder. Gerekmez, çünkü görünümü şablonu değiştirmek için **StoreController** şu veritabanından veri gelir ancak aynı varlıkları önceki gibi döndürüyor.

1. Tuşuna basın ve çözümü yeniden **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Doğrulayın menüsünü **türleri** artık sabit kodlanmış bir listedir ve verileri doğrudan veritabanından alınır.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Veritabanından gözatma türleri*
3. Artık tüm Tarz için göz atın ve albümleri veritabanından doldurulur doğrulayın.

    ![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image17.png "gözatma albümleri veritabanından")

    *Veritabanından albümleri gözatma*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Alıştırma 2: Kod kullanarak veritabanı oluşturma

Bu alıştırmada MusicStore uygulama tablolarla bir veritabanı oluşturmak için ilk kod yaklaşımı kullanmayı ve kendi verilerine erişim öğreneceksiniz.

Model oluşturulduktan sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController değiştirir.

> [!NOTE]
> Alıştırma 1 tamamladıktan ve zaten veritabanı ilk yaklaşımı hakkında deneyimli olduğunuzu, artık başka bir işlem ile aynı sonuçları elde etmek nasıl öğreneceksiniz. Alıştırma 1 ortak olan görevleri, okumayı kolaylaştırmak için işaretlenmiş. Alıştırma 1 tamamlanmamış ancak ilk kod yaklaşımı öğrenmek istiyorsanız, bu çalışma başlayın ve konunun tam bir kapsamı edinin.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Görev 1 - doldurulurken örnek veriler

Code-First kullanarak başlangışta oluşturulduğunda bu görevde, örnek verilerle veritabanı doldurur.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-CreatingADatabaseCodeFirst/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Ekleme **SampleData.cs** dosyasını **modelleri** klasör. Bunu yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **var olan öğe**. Gözat **\Source\Assets** seçip **SampleData.cs** dosya.

    ![Örnek veri kodu doldurmak](aspnet-mvc-4-models-and-data-access/_static/image18.png "örnek veri kodu Doldur")

    *Örnek veri kodu Doldur*
3. Açık **Global.asax.cs** dosyasını açıp aşağıdaki *kullanarak* deyimleri.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 genel Asax kullanımları*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. İçinde **uygulama\_Start()** yöntemi veritabanı Başlatıcı ayarlamak için aşağıdaki satırı ekleyin.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 genel Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Görev 2 - veritabanı bağlantısı yapılandırma

Projemizin için bir veritabanı zaten eklenmiş, yazacağınız **Web.config** bağlantı dizesini dosya.

1. Konumunda bir bağlantı dizesi Ekle **Web.config**. Bunu yapmak için açık **Web.config** proje kök ve bağlantı dizesi ile bu satırda DefaultConnection adlı Değiştir **&lt;connectionStrings&gt;** bölümü:

    ![Web.config dosyası konumu](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config dosyası konumu")

    *Web.config dosyası konumu*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Görev 3 - modeli ile çalışma

Veritabanı bağlantısı zaten yapılandırdıysanız, veritabanı tablolarını modeliyle bağlayacaksınız. Bu görevde, Code First ile veritabanına bağlı bir sınıf oluşturur. Değiştirilmesi gereken gereği, mevcut bir POCO model sınıfı olduğunu unutmayın.

> [!NOTE]
> Alıştırma 1 tamamlandı, bu adım, bir sihirbaz tarafından gerçekleştirildiğini fark edeceksiniz. Code First yaparak, el ile veri varlıkları için bağlantılı sınıfları oluşturur.

1. POCO model sınıfı açın **Tarz** gelen **modelleri** proje klasörü ve bir kimlik içerir Adıyla bir int özelliğini kullanın **GenreId**.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk Tarz*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Kod öncelikli kurallar ile çalışmak için ' % s'sınıfı Tarz otomatik olarak algılanacak bir birincil anahtar özelliği olmalıdır.
    > 
    > Daha fazla bilgi bu kod öncelikli kurallar hakkında [msdn makalesi](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Şimdi, POCO model sınıfı açmak **albüm** gelen **modelleri** proje klasörü ve yabancı anahtarlar eklemek, adlara sahip özellikler oluşturun **GenreId** ve  **ArtistId**. Bu sınıf zaten **GenreId** birincil anahtar.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk albüm*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. POCO model sınıfı açın **sanatçının** ve **ArtistId** özelliği.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk sanatçının*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Sağ **modelleri** proje klasörü ve select **Ekle | Sınıf**. Dosya adı **MusicStoreEntities.cs**. ' A tıklayarak **Ekle.**

    ![Sınıf ekleme](aspnet-mvc-4-models-and-data-access/_static/image20.png "sınıf ekleme")

    *Yeni bir öğe ekleme*

    ![Bir class2 ekleme](aspnet-mvc-4-models-and-data-access/_static/image21.png "bir class2 ekleme")

    *Sınıf ekleme*
5. Az önce oluşturduğunuz, sınıfın açık **MusicStoreEntities.cs**ve ad alanlarını dahil **System.Data.Entity** ve **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Genişletmek için sınıf bildirimini değiştirin **DbContext** sınıfı: Genel bildirmek **olan DB** ve geçersiz kılma **OnModelCreating** yöntemi. Bu adımdan sonra modelinizi Entity Framework ile bağlayacak bir etki alanı sınıfı alırsınız. Bunu yapabilmek için sınıf kodunu aşağıdakiyle değiştirin:

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Entity Framework ile **DbContext** ve **olan DB** POCO sınıfı Tarz sorgu mümkün olacaktır. Genişleterek **OnModelCreating** yöntemi belirttiğinizden içinde **kod** tarzı bir veritabanı tablosuna nasıl eşlenir. Bu msdn makalesinde DBContext olan DB hakkında daha fazla bilgi bulabilirsiniz: [bağlantı](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Görev 4 - veritabanı sorgulama

Bu görevde, sabit kodlanmış veri kullanmak yerine, bu veritabanından alır, böylece StoreController sınıfı güncelleştirir.

> [!NOTE]
> Alıştırma 1 ortak görevdir.
> 
> Alıştırma 1 tamamladıysanız Bu adımlar, her iki yaklaşım aynı fark edeceksiniz (ilk veritabanı veya ilk kod). Bunlar verileri nasıl modeliyle bağlı olarak farklıdır, ancak veri varlıklarına erişim henüz denetleyicisinden saydamdır.


1. Açık **Controllers\StoreController.cs** ve şu alan örneği için Sınıf Ekle **MusicStoreEntities** adlı bir sınıf **storeDB**:

    (Kod parçacığını - *modeller ve veri erişimi - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar. Güncelleştirme **Gözat** tüm bir türe almak için eylem yöntemini **albümleri**.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 Store Gözat*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Bir özellik adında .NET, kullanmakta olduğunuz **LINQ** (kod veritabanında yürütün ve dönüş bu koleksiyonlara karşı-kesin türü belirtilmiş sorgu ifadeleri yazmak için dil ile tümleşik sorgu) nesneleri, programlama yapabilirsiniz karşı.
    > 
    > LINQ hakkında daha fazla bilgi için lütfen [msdn sitesine](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Güncelleştirme **dizin** tüm türleri almak için eylem yöntemi.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 Store dizini*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Güncelleştirme **dizin** tüm türleri almak ve liste koleksiyon dönüştürmek için eylem yöntemi.

    (Kod parçacığını - *modeller ve veri erişimi - Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, Store dizin sayfası yerine ' % s'belirtebilseydik olanları veritabanında depolanan türleri artık göstereceğini kontrol eder. Gerekmez, çünkü görünümü şablonu değiştirmek için **StoreController** aynı döndürüyor **StoreIndexViewModel** önceki gibi ancak bu kez verileri veritabanından gelen.

1. Tuşuna basın ve çözümü yeniden **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Doğrulayın menüsünü **türleri** artık sabit kodlanmış bir listedir ve verileri doğrudan veritabanından alınır.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Veritabanından gözatma türleri*
3. Artık tüm Tarz için göz atın ve albümleri veritabanından doldurulur doğrulayın.

    ![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image23.png "gözatma albümleri veritabanından")

    *Veritabanından albümleri gözatma*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Alıştırma 3: parametrelerle veritabanını sorgulama

Bu alıştırmada, parametreleri kullanarak veritabanını sorgulama yapmayı ve sorgu sonucu şekillendirme kullanmayı öğreneceksiniz, numara veritabanı azaltan bir özellik verileri daha verimli bir şekilde erişir.

> [!NOTE]
> Aşağıdaki sorgu sonucu şekillendirme ile ilgili daha fazla bilgi için ziyaret [msdn makalesi](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Görev 1 - değiştirme StoreController albümleri veritabanından alınamıyor

Bu görevde, değişiklik yapacağınız **StoreController** albümleri belirli bir türe almak için veritabanına erişmek için sınıf.

1. Açık **başlamak** çözüm bulunan **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** ilk kod yaklaşımı kullanmak istiyorsanız bir klasör veya **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** Database-First yaklaşımı kullanmak istiyorsanız bir klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreController** değiştirmek için sınıf **Gözat** eylem yöntemi. Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.
3. Değişiklik **Gözat** Albümler belirli bir türe'almak için eylem yöntemi. Bunu yapmak için aşağıdaki kodu değiştirin:

    (Kod parçacığını - *modeller ve veri erişimi - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Varlık koleksiyonu doldurmak için kullanmanız gerekir **INCLUDE** albümleri çok almak istediğiniz belirtmek için yöntemi. Kullanabilirsiniz. **Single()** LINQ uzantısı bu durumda yalnızca bir türe için albüm beklendiğinden. **Single()** yöntem adı ile eşleşen tanımlanan değeri olacak şekilde, bu durumda tek bir türe nesne belirten bir parametre olarak bir Lambda ifadesi alır.
> 
> Tarz nesne alındığında de yüklenen istediğiniz diğer ilgili varlıkları belirtmenize olanak tanıyan bir özelliğin avantajlarından yararlanmak. Bu özelliğin adı **sorgu sonucu şekillendirme**ve bilgi almak için veritabanına erişmek için gereken sayısını azaltmanızı sağlar. Bu senaryoda, Albümler almanızı Tarz için önceden getirme isteyeceksiniz.
> 
> Sorgu içeren **Genres.Include (&quot;albümleri&quot;)** ilgili Albümler istediğinizi belirtmek için. Tek veritabanı isteği Tarz hem albüm verileri alacak olduğundan bu daha verimli bir uygulamada neden olur.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulamayı çalıştırma

Bu görevde, uygulamayı çalıştırmak ve belirli bir türe albümleri veritabanından alınamıyor.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/Store/göz atma? Tarz Pop =** sonuçları veritabanından alınan olduğunu doğrulayın.

    ![Türe göre gözatma](aspnet-mvc-4-models-and-data-access/_static/image24.png "türe göre göz atma")

    *Tarama/Store/göz atma? Tarz Pop =*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Görev 3 - kimliğine göre albümleri erişme

Bu görevde, Albümler kimliklerine göre almak için önceki yordamı yinelenir.

1. Gerekirse Visual Studio'ya dönmek için tarayıcıyı kapatın. Açık **StoreController** değiştirmek için sınıf **ayrıntıları** eylem yöntemi. Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.
2. Değişiklik **ayrıntıları** albümleri ayrıntılarını almak için eylem yöntemine bağlı olarak kendi **kimliği**. Bunu yapmak için aşağıdaki kodu değiştirin:

    (Kod parçacığını - *modeller ve veri erişimi - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, uygulama bir web tarayıcısında çalışacak ve kimliklerine göre albümü ayrıntılarını alma

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/Store/Details/51** türleri göz atın veya veritabanından sonuçları alınıyor olduğunu doğrulamak için bir albümü seçin.

    ![Ayrıntılar gözatma](aspnet-mvc-4-models-and-data-access/_static/image25.png "ayrıntıları gözatma")

    *Gözatma /Store/Details/51*

> [!NOTE]
> Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

ASP.NET MVC modeller ve veri erişimi temelleri öğrenilen Bu uygulamalı laboratuvarı tamamlamak, kullanarak **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım:

- Bir veritabanı verilerini kullanmak için çözüm ekleme
- Görünüm şablonları yerine sabit kodlanmış bir veritabanından alınan veri sağlamak için denetleyicileri güncelleştirme
- Parametreleri kullanarak veritabanını sorgulama
- Sorgu sonucu daha verimli bir şekilde veri alma şekillendirme, veritabanı erişimlerine sayısını azaltan bir özelliğini kullanma
- Veritabanı ilk ve Code First yaklaşımları modeli ile veritabanına bağlanmak için Microsoft Entity Framework kullanma

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Windows Azure Portal'da oturum açın](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure Portal'da oturum açın")

    *Windows Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** komut çubuğunda.

    ![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image32.png "yeni bir Web sitesi oluşturma")

    *Yeni bir Web sitesi oluşturma*
3. Tıklayın **işlem** | **Web sitesi**. Ardından **hızlı Oluştur** seçeneği. Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.

    > [!NOTE]
    > Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır. Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar. Bir veritabanını ayarlamak için adımları içermez.

    ![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image33.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")

    *Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*
4. Yeni kadar bekleyin **Web sitesi** oluşturulur.
5. Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun. Yeni Web sitesi çalışıp çalışmadığını denetleyin.

    ![Yeni web sitesi için gözatma](aspnet-mvc-4-models-and-data-access/_static/image34.png "yeni web sitesine göz atma")

    *Yeni web sitesine göz atma*

    ![Web sitesi çalışan](aspnet-mvc-4-models-and-data-access/_static/image35.png "çalışan Web sitesi")

    *Çalışan Web sitesi*
6. Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-models-and-data-access/_static/image36.png "web sitesi Yönetim sayfalarının açma")

    *Web Sitesi Yönetim sayfalarının açma*
7. İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.

    > [!NOTE]
    > *Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir. Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.

    ![Yayımlama profili web sitesi indiriliyor](aspnet-mvc-4-models-and-data-access/_static/image37.png "yayımlama profili web sitesi indiriliyor")

    *Yayımlama profili Web sitesi indiriliyor*
8. Yayımlama profili dosyasını bilinen bir konuma indirin. Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.

    ![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-models-and-data-access/_static/image38.png "yayımlama profili kaydediliyor")

    *Yayımlama profili dosyasını kaydetme*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Görev 2 - veritabanı sunucusunu yapılandırma

Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir. SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.

1. SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir. SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**. Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme. Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız. Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.

    ![SQL veritabanı sunucu Panosu](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL veritabanı sunucu Panosu")

    *SQL veritabanı sunucu Panosu*
2. İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**. Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) düğmesi.

    ![İstemci IP adresi ekleme](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *İstemci IP adresi ekleme*
3. Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.

    ![Değişiklikleri onaylayın](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Değişiklikleri onaylayın*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama

1. ASP.NET MVC 4 çözüme geri dönün. İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.

    ![Uygulama yayımlama](aspnet-mvc-4-models-and-data-access/_static/image43.png "uygulama yayımlama")

    *Web sitesi yayımlama*
2. İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.

    ![Yayımlama profilini içeri aktarma](aspnet-mvc-4-models-and-data-access/_static/image44.png "yayımlama profilini içeri aktarma")

    *Yayımlama profilini içeri aktarma*
3. Tıklayın **bağlantısını doğrulama**. Doğrulama tamamlandıktan sonra tıklayın **sonraki**.

    > [!NOTE]
    > Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.

    ![Bağlantı doğrulama](aspnet-mvc-4-models-and-data-access/_static/image45.png "bağlantısı doğrulanıyor")

    *Bağlantı doğrulama*
4. İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).

    ![Web dağıtımı yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web dağıtımı yapılandırma")

    *Web dağıtımı yapılandırma*
5. Veritabanı bağlantısı aşağıdaki gibi yapılandırın:

   - İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.
   - İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.
   - İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.
   - Yeni bir veritabanı adı yazın.

     ![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image47.png "hedef bağlantı dizesini yapılandırma")

     *Hedef bağlantı dizesini yapılandırma*
6. Sonra **Tamam**'a tıklayın. Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.

    ![Veritabanı oluşturma](aspnet-mvc-4-models-and-data-access/_static/image48.png "veritabanı dizesi oluşturma")

    *Veritabanı oluşturma*
7. Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir. Sonra **İleri**'ye tıklayın.

    ![SQL veritabanı'na işaret eden bağlantı dizesi](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL veritabanına işaret eden bağlantı dizesi")

    *SQL veritabanı'na işaret eden bağlantı dizesi*
8. İçinde **Önizleme** sayfasında **Yayımla**.

    ![Web uygulaması yayımlama](aspnet-mvc-4-models-and-data-access/_static/image50.png "web uygulaması yayımlama")

    *Web uygulaması yayımlama*
9. Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Ek C: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-models-and-data-access/_static/image51.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-models-and-data-access/_static/image52.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-models-and-data-access/_static/image53.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-models-and-data-access/_static/image54.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-models-and-data-access/_static/image55.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-models-and-data-access/_static/image56.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
