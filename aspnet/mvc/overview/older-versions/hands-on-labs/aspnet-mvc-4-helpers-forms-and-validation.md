---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 yardımcılar, formlar ve doğrulama | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 modelleri ve veri erişim uygulamalı laboratuvarı, size alınan yükleniyor ve veritabanından veri görüntüleme. Bu uygulamalı laboratuvarda, ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376743"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 yardımcılar, formlar ve doğrulama

Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](https://aka.ms/webcamps-training-kit)

İçinde **ASP.NET MVC 4 modelleri ve verilere erişim** Hands-on-Lab, yükleme ve veritabanından veri görüntüleme olmuştur. Bu uygulamalı laboratuvarda için ekleyeceksiniz **müzik Store** uygulama bu verileri düzenleme olanağı.

Bu hedefe aklınızda Albümler oluşturma, okuma, güncelleştirme ve silme (CRUD) eylemleri destekleyen denetleyici önce oluşturursunuz. ASP.NET MVC'nin iskele kurma özelliği ile HTML tablosu halinde albümleri özelliklerini görüntülemek için yararlanarak bir dizin görünümünün şablonu oluşturur. Bu görünümü geliştirmek için uzun açıklamaları kısaltabilir özel bir HTML Yardımcısı ekleyeceksiniz.

Daha sonra düzenleme ve oluşturma olanak tanıyan görünümleri bırakmalar gibi form öğelerin yardımıyla veritabanında albümleri alter ekleyeceksiniz.

Son olarak, kullanıcıların albümü silme sağlar ve ayrıca, bunları yanlış veri girişi doğrulayarak girmesini engeller.

Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC**. Kullanmadıysanız, **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC ile ilgili temel bilgiler** Hands-on-Lab.

Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.

> [!NOTE]
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit). Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 yardımcılar, formlar ve doğrulama](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- CRUD işlemlerini desteklemek için bir denetleyici oluşturma
- HTML tablosu halinde varlık özelliklerini görüntülemek için bir dizin görünümü oluştur
- Bir özel HTML Yardımcısı Ekle
- Oluşturma ve özelleştirme Görünümü Düzenle
- HTTP GET veya POST HTTP çağrıları için react eylem yöntemleri birbirinden ayırt
- Ekleme ve özelleştirme Görünüm Oluştur
- Tanıtıcı bir varlığı silme
- Kullanıcı girişini doğrulama

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

Bu uygulamalı laboratuvarı ayarlama aşağıdaki alıştırmalar yapın:

1. [Store Yöneticisi denetleyicisi ve onun dizini görünümünü oluşturma](#Exercise1)
2. [Bir HTML Yardımcısı ekleme](#Exercise2)
3. [Düzenleme görünümü oluşturma](#Exercise3)
4. [Bir oluşturma görünümü ekleme](#Exercise4)
5. [İşleme silme](#Exercise5)
6. [Doğrulama Ekleme](#Exercise6)
7. [İstemci tarafında örtük jQuery kullanma](#Exercise7)

> [!NOTE]
> Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör. Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Alıştırma 1: Store Yöneticisi denetleyicisi ve onun dizini görünümünü oluşturma

Bu alıştırmada, veritabanını ve son olarak ASP.NET MVC'nin yapı iskelesi yararlanarak bir dizin görünümünün şablonu oluşturulurken albümleri listesini döndürmek için dizin eylem yöntemini Özelleştir CRUD işlemlerini desteklemek için yeni bir denetleyici nasıl oluşturulacağını öğreneceksiniz. HTML tablosu halinde albümleri özelliklerini görüntülemek için özellik.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Görev 1 - StoreManagerController oluşturma

Bu görevde, adlı yeni bir denetleyici oluşturacaksınız **StoreManagerController** CRUD işlemlerini desteklemek için.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-CreatingTheStoreManagerController/başlangıç/** klasör.

   1. Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Yeni bir denetleyici ekleyeceksiniz. Bunu yapmak için sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu. Değişiklik **denetleyicisi** **adı** için **StoreManagerController** seçeneği emin **boşokuma/yazmaeylemleriileMVCdenetleyicisi**seçilir. **Ekle**'yi tıklatın.

    ![Ekleme denetleyicisi iletişim](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "ekleme denetleyicisi iletişim")

    *Denetleyici Ekle iletişim kutusu*

    Yeni bir denetleyici sınıfı oluşturulur. Okuma/yazma, kişiler, saptama yöntemleri için Eylemler eklemek için belirtilen beri yaygın CRUD eylemleri uygulama belirli mantığı eklemek isteyen doldurulur TODO yorumları ile oluşturulur.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Görev 2 - StoreManager dizini özelleştirme

Bu görevde, veritabanından albümleri listesini içeren bir görünümü döndürülecek StoreManager dizin eylem yönteminin özelleştireceksiniz.

1. StoreManagerController sınıfında, aşağıdaki ekleyin *kullanarak* yönergeleri.

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 MvcMusicStore kullanarak*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Bir alan ekleyerek **StoreManagerController** örneği tutacak **MusicStoreEntities.**

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Albümleri listesini içeren bir görünüme dönmek için StoreManagerController dizin eylemi uygulayın.

    Denetleyici eylem mantıksal StoreController'ın dizin eylemini daha önce yazılmış çok benzer olacaktır. LINQ tarz ve sanatçı bilgilerini görüntülemek için de dahil olmak üzere tüm Albümler almak için kullanın.

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 StoreManagerController dizin*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Görev 3 - dizin görünümü oluşturma

Bu görevde, dizin görünümünün şablonu tarafından döndürülen albümleri listesini görüntülemek için oluşturacağınız **StoreManager** denetleyicisi.

1. Yeni Görünüm şablonu oluşturmadan önce projeyi oluşturmalısınız böylece **Ekle iletişim kutusunu görüntüle** bildiği **albüm** kullanılacak sınıfı. Seçin **yapı | Derleme MvcMusicStore** Projeyi derlemek için.
2. İçinde sağ **dizin** eylem yöntemini seçip alt **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

    ![Görünüm Ekle](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "görünümü ekleme")

    *Index yöntemi içinden bir görünümle ekleme*
3. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **dizin**. Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini belirtin ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır. Seçin **listesi** gelen **İskele şablon** açılır. Bırakın **görünüm altyapısı** için **Razor** ve diğer alanları varsayılan değer ve ardından **Ekle**.

    ![Bir dizini görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "dizini görünümü ekleme")

    *Bir dizini görünümü ekleme*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Görev 4 - dizin görünümünün iskele özelleştirme

Bu görevde, Basit Görünüm şablonu istediğiniz alanları görüntülemek için ASP.NET MVC iskele kurma özelliği ile oluşturulan uyum sağlar.

> [!NOTE]
> **Yapı iskelesi** içinde ASP.NET MVC desteği albüm modelinde tüm alanları listeler basit bir görünüm şablonu oluşturur. **Yapı iskelesi** türü kesin belirlenmiş bir görünüm üzerinde kullanmaya başlamak için hızlı bir yol sunar: görünüm şablonu el ile yazmak zorunda kalmak yerine hızlı bir şekilde iskele kurma özelliği bir varsayılan şablon oluşturur ve oluşturulan kodun daha sonra değiştirebilirsiniz.


1. Oluşturulan kodu gözden geçirin. Oluşturulan alanlar listesi aşağıdaki bir parçası olacak HTML tablosu **yapı İskelesi** tablosal verileri görüntülemek için kullanıyor.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Değiştirin **&lt;tablo&gt;** yalnızca görüntülemek için aşağıdaki kod ile kod **Tarz**, **sanatçının**, **albümbaşlığı**, ve **fiyat** alanları. Bu siler **AlbumId** ve **albüm resim URL'si** sütunları. Ayrıca, kendi bağlantılı sınıf özelliklerini görüntülemek için GenreId ve ArtistId sütunları değiştirirse **Artist.Name** ve **Genre.Name**ve kaldırır **ayrıntıları** bağlantı.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Aşağıdaki tanımlamalar değiştirir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **dizin** görünüm şablonu albümleri önceki adımları tasarımını göre bir listesini görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager** gösteren albümleri listesini görüntülendiğini doğrulamak için kendi **başlık**, **sanatçının** ve **Tarz**.

    ![Albümleri biri listeyi gözden geçireceğiniz](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "albümleri listesini gözatma")

    *Gözatma albümleri listesi*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Alıştırma 2: bir HTML Yardımcısı ekleme

Bir olası sorun StoreManager dizin sayfası vardır: Başlık ve sanatçı adı özellikleri hem de olabilir tablo biçimlendirmeyi devre dışı throw yetecek kadar uzun. Bu alıştırmada, metnin kesin bir özel HTML Yardımcısı ekleme öğreneceksiniz.

Aşağıdaki resimde, bir küçük tarayıcı boyutu kullandığınızda biçimi nedeniyle metnin uzunluğunu nasıl değiştirilir görebilirsiniz.

![Albümleri listeyi gözden geçireceğiniz değil metin kesilmiş](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "albümleri listeyi gözden geçireceğiniz değil metin kesiliyor")

*Albümleri listeyi gözden geçireceğiniz metin kesilmiş değil*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Görev 1 - HTML Yardımcısı genişletme

Bu görevde, yeni bir yöntem ekleyeceksiniz **Truncate** için **HTML** ASP.NET MVC görünümleri içinde kullanıma sunulan nesne. Bunu yapmak için uygular bir **genişletme yöntemi** için yerleşik **System.Web.Mvc.HtmlHelper** ASP.NET MVC tarafından sağlanan sınıfı.

> [!NOTE]
> Hakkında daha fazla bilgi edinmek için **genişletme yöntemleri**, lütfen bu msdn makalesine bakın. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-AddingAnHTMLHelper/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. StoreManager'ın dizini görünümünü açın. Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, ardından **StoreManager** açın **Index.cshtml** dosya.
3. Aşağıdaki kodu ekleyin <strong>@model</strong> tanımlamak için yönergesi <strong>Truncate</strong> yardımcı yöntemi.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Görev 2 - sayfasında kesiliyor metin

Bu görevde kullanacağınız **Truncate** truncate şablonu görüntüleme metni için yöntemi.

1. StoreManager'ın dizini görünümünü açın. Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, ardından **StoreManager** açın **Index.cshtml** dosya.
2. Gösteren satırları değiştirin **sanatçı adı** ve albüm **başlık**. Bunu yapmak için aşağıdaki satırları değiştirin.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **dizin** görünüm şablonu albüm başlık ve sanatçı adı keser.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager** bu uzun doğrulamak için metinleri **başlık** ve **sanatçının** sütun kesilir.

    ![Kesilmiş başlıklar ve sanatçıların adları](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "kesilmiş başlıklar ve sanatçıların adları")

    *Kesilmiş başlıklar ve sanatçının adları*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Alıştırma 3: düzenleme görünümü oluşturma

Bu alıştırmada, albüm düzenlemek Mağaza yöneticileri izin vermek için bir formun nasıl oluşturulacağını öğreneceksiniz. Kullanıcıların göz **/StoreManager/Edit/id** URL (**kimliği** düzenlemek için albümü benzersiz kimliği olan), bu nedenle sunucunun bir HTTP GET çağrı yapma.

Denetleyici eylem yöntemi Düzenle veritabanından uygun albümü, oluşturun bir **StoreManagerViewModel** nesne (bir liste sanatçıların ve türleri ile birlikte) kapsüllemek ve ardından bunu bir şablonu görüntüle geçirmek devre dışı HTML sayfasının, kullanıcıya geri işleyin. Bu sayfa içerecek bir **&lt;form&gt;** metin kutuları ve albüm özelliklerini düzenlemek için açılan öğe.

Kullanıcı albüm form değerlerini güncelleştirir ve tıkladığında sonra **Kaydet** düğmesi, değişiklikleri aracılığıyla gönderilir bir HTTP POST geri çağırma **/StoreManager/Edit/id**. ASP.NET MVC URL son çağrının olduğu gibi aynı kalsa da, bir HTTP POST ve bu nedenle farklı bir düzen eylem yöntemini yürütür. Bu süre tanımlayan (bir düzenlenmiş **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Görev 1 - HTTP-GET düzenleme eylem yöntemi uygulama

Bu görevde, HTTP GET sürüm uygun albümü veritabanından alınacak düzenleme eylem yönteminin yanı sıra, tüm türleri ve sanatçıların listesini uygular. Bu verileri içine paketi **StoreManagerViewModel** ardından Yanıtla işlemek için bir görünüm şablonu geçirilir son adımda tanımlanan nesne.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex3-CreatingTheEditView/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Değiştirin **HTTP GET Düzenle** uygun almak için aşağıdaki kodu eylem yöntemine **albüm** yanı sıra **türleri** ve **Sanatçılar**listeler.

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - eylemini Ex3 StoreManagerController HTTP GET Düzenle*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Kullanmakta olduğunuz **System.Web.Mvc** **SelectList** sanatçıların ve türleri yerine **System.Collections.Generic** listesi.
    > 
    > **SelectList** HTML bırakmalar doldurmak ve geçerli seçimi gibi şeyleri yönetmek için temiz bir yoludur. Örnekleme ve bu denetleyici eylemini ViewModel nesnelere sonraki ayarlama düzenleme formu senaryo temizleyici hale getirir.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Görev 2 - düzenleme görünümü oluşturma

Bu görevde, daha sonra Albüm özellikleri görüntüleyecek bir görünümü düzenleme şablonu oluşturur.

1. Düzenleme görünümü oluşturun. İçinde Bunu yapmak için sağ **Düzenle** eylem yöntemini seçip alt **Görünüm Ekle**.
2. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Düzenle**. Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm (MvcMusicStore.Models)** gelen **görüntülemek veri sınıfı** açılır. Seçin **Düzenle** gelen **İskele şablon** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Düzenleme görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "düzenleme görünümü ekleme")

    *Düzenleme görünümü ekleme*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası parametre olarak geçirilen albümü özelliklerin değerlerini görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/Edit/1** geçirilen albümü özelliklerin değerlerini görüntülendiğini doğrulayın.

    ![Albüm düzenleme görünümünü gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "albüm düzenleme görünümünü gözatma")

    *Albüm düzenleme görünümünü gözatma*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Görev 4 - açılan listeler albüm Düzenleyicisi şablon uygulama

Sanatçıların ve türleri listesinden seçebilmeniz için bu görevde, açılan listeler son görevde oluşturduğunuz görünüm şablonu ekler.

1. Tümünü Değiştir **albüm** fieldset kodu şu kodla:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Bir **Html.DropDownList** Yardımcısı, sanatçıların ve tür seçmek için açılır listeleri işlemek için eklendi. Geçirilen parametreleri **Html.DropDownList** şunlardır:
    > 
    > 1. Form alanı adını (**&quot;ArtistId&quot;**).
    > 2. **SelectList** aşağı açılan değer.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **Düzenle** görünümü sayfasında görüntüler sanatçı ve Tarz kimliği metin alanları yerine açılan listeler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/Edit/1** açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntülediğini doğrulamak üzere.

    ![Albüm düzenleme görünümü ile açılan listeler gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "gözatma albüm görünümü açılan listeler ile Düzenle")

    *Albüm düzenleme görünümü açılır menüleri kullanarak bu sefer gözatma*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Görev 6 - HTTP-POST Düzenle eylem yöntemi uygulama

Düzenleme görünümü beklendiği gibi görüntüler, albümü yaptığınız değişiklikleri kaydetmek için HTTP POST eylemini Düzenle yöntemi uygulaması gerekir.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** gelen **denetleyicileri** klasör.
2. Değiştirin **HTTP POST Düzenle** eylem yöntemi kodu şu kodla (değiştirilmesi gereken yöntemini iki parametre alan Aşırı yüklenen sürümü olduğunu unutmayın):

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - eylemini Ex3 StoreManagerController HTTP POST Düzenle*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Kullanıcı tıkladığında bu yöntem yürütülecek **Kaydet** görünümün düğme ve bir HTTP veritabanında kalıcı hale getirmek için POST form değerlerinin sunucuya geri gerçekleştirir. Dekoratörün **[HttpPost]** yöntemi bu HTTP POST senaryoları için kullanılması gerektiğini belirtir. Yöntem alır bir **albüm** nesne. ASP.NET MVC otomatik olarak oluşturur albüm nesne deftere nakledilen &lt;form&gt; değerleri.
    > 
    > Yöntemi, aşağıdaki adımları gerçekleştireceksiniz:
    > 
    > 1. Model geçerliyse:
    > 
    >     1. Değiştirilen bir nesnesi olarak işaretlemek için bağlam albüm girişi güncelleştirin.
    >     2. Değişiklikleri kaydetmek ve dizin görünümüne yönlendirin.
    > 2. Model geçerli değilse ViewBag ile doldurulur **GenreId** ve **ArtistId**, izin vermek için alınan albümü nesnenin görünümüyle döndürecektir herhangi bir gerekli güncelleştirme gerçekleştirin.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Görev 7 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager düzenleme** görünüm sayfası gerçekten veritabanında güncelleştirilmiş albüm verileri kaydeder.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/Edit/1**. Albüm başlığı değiştirmek **yük** tıklayın **Kaydet**. Albüm başlık gerçekten albümleri listesinde değiştiğini doğrulayın.

    ![Albüm güncelleştirme](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "albüm güncelleştiriliyor")

    *Albüm güncelleştiriliyor*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Alıştırma 4: bir oluşturma görünümü ekleme

Şimdi **StoreManagerController** destekler **Düzenle** özelliği, bu alıştırmada depolamak için bir görünüm oluştur şablonu ekleme öğreneceksiniz yöneticilerini yeni Albümler uygulamaya ekleyin.

Düzenleme işlevsellikle yaptığınız gibi iki farklı yöntemle içinde oluşturma senaryosu uygular **StoreManagerController** sınıfı:

1. Bir eylem yöntemi, boş bir form görüntülenir, depolama yöneticilerinin ilk kez ziyaret ettiğinizde **/StoreManager/Create** URL'si.
2. İkinci bir eylem yöntemi burada mağaza yöneticisi tıkladığında senaryo işleyecek **Kaydet** düğmesi, formda ve değerlerin geri gönderir **/StoreManager/Create** olarak bir HTTP POST URL'si.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Görev 1 - HTTP GET oluşturma eylemi yöntemini uygulama

Bu görevde, tüm türleri ve sanatçıların listesini almak için bu verileri içine paketini oluşturma eylem yöntemi GET HTTP sürümü gerçekleştireceksiniz bir **StoreManagerViewModel** bir görünüm şablonu için geçirilecek nesne.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex4-AddingACreateView/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Değiştirin **Oluştur** eylem yöntemi kodu şu kodla:

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex4 StoreManagerController HTTP GET Oluştur eylemi*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Görev 2 - oluşturma görünümü ekleme

Bu görevde, yeni (boş) albüm form görüntüler Görünüm Oluştur şablona ekleyeceksiniz.

1. İçinde sağ **Oluştur** eylem yöntemini seçip alt **Görünüm Ekle**. Bu Görünüm Ekle iletişim kutusu getirir.
2. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Oluştur**. Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini işaretleyip **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır ve **Oluştur** gelen **İskele şablon** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Bir oluşturma görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ekleme-a-Oluştur-view.png")

    *Oluşturma görünümü ekleme*
3. Güncelleştirme **GenreId** ve **ArtistId** alanlar aşağıda gösterildiği gibi açılan listesini kullanın:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **Oluştur** görünüm sayfası boş bir albüm form görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/oluşturma**. Yeni albüm özelliklerini doldurmak için boş bir form görüntülendiğini doğrulayın.

    ![Boş bir formla görünümü oluşturmak](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "boş bir formla Görünüm Oluştur")

    *Boş bir formla görünümü oluşturma*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Görev 4 - HTTP-POST uygulama oluşturma eylem yöntemi

Bu görevde bir kullanıcı tıkladığında çağrılır oluşturma eylem yöntemine HTTP POST sürümünü gerçekleştireceksiniz **Kaydet** düğmesi. Yöntemi veritabanına yeni albümü kaydetmeniz gerekir.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
2. Değiştirin **HTTP POST oluşturma** eylem yöntemi kodu şu kodla:

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex4 StoreManagerController HTTP - POST Oluştur eylemi*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Oluşturma eylemini önceki düzenleme eylem yöntemine oldukça benzer, ancak nesne değiştirilmiş olarak ayarlamak yerine, bunu bağlamına ekleniyor.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager oluşturma** görünümü sayfasında, yeni albümü oluşturmanıza olanak sağlar ve ardından StoreManager dizin görünümüne yeniden yönlendirir.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/oluşturma**. Tüm form alanlarını, aşağıdaki şekilde benzeyen yeni bir albümü için verilerle doldurun:

    ![Albüm oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "albüm oluşturma")

    *Albüm oluşturma*
3. Yeni oluşturduğunuz yeni albümü içerir StoreManager dizini görünümü yeniden yönlendirilen doğrulayın.

    ![Oluşturulan yeni albümü](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "oluşturulan yeni albümü")

    *Oluşturulan yeni albümü*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Alıştırma 5: Silme işleme

Albümleri silme olanağı henüz uygulanmadı. Bu alıştırmada hakkında olacaktır budur. İçinde iki ayrı yöntemlerini kullanarak silme senaryosunu önce uygular gibi **StoreManagerController** sınıfı:

1. Bir eylem yöntemi bir doğrulama formu görüntülenir.
2. İkinci bir eylem yöntemi form gönderme işleyecek

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Görev 1 - GET HTTP Delete eylem yöntemi uygulama

Bu görevde, albüm bilgi almak için Delete eylem yöntemini HTTP GET sürümünü uygular.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex5-HandlingDeletion/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Silme denetleyici eylemi tam olarak önceki Store ayrıntıları denetleyici eylemini aynıdır: Bu sorgular **albüm** kullanarak veritabanı nesne **kimliği** döndürür ve URL içinde sağlanan uygun **görünümü**. Bunu yapmak için HTTP GET değiştirin **Sil** eylem yöntemi kodu şu kodla:

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex5 işleme silme HTTP GET silme eylemi*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. İçinde sağ **Sil** eylem yöntemini seçip alt **Görünüm Ekle**. Bu Görünüm Ekle iletişim kutusu getirir.
5. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Sil**. Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini işaretleyip **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır. Seçin **Sil** gelen **İskele şablon** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Delete görünüm ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "silme görünüm ekleme")

    *Delete görünüm ekleme*
6. Tüm alanları modelden Sil şablon gösterir. Yalnızca albüm başlık gösterilir. Bunu yapmak için görünümün içeriğini aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager** **Sil** görünüm sayfası onay silme form görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager**. Tıklayarak silmek için bir albümü seçmek **Sil** ve yeni görüntüyü karşıya yüklendiğini doğrulayın.

    ![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "albüm siliniyor")

    *Albüm siliniyor*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Görev 3-HTTP-POST silme eylem yöntemi uygulama

Bu görevde, kullanıcı tıkladığında çağrılır Delete eylem yöntemini HTTP POST sürümünü gerçekleştireceksiniz **Sil** düğmesi. Yöntemi, veritabanındaki albümü silmeniz gerekir.

1. Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** sınıfı. Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
2. Değiştirin **HTTP POST Sil** eylem yöntemi kodu şu kodla:

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex5 işleme silme HTTP POST silme eylemi*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager silme** görünüm sayfası albüm silmenize olanak sağlar ve ardından StoreManager dizin görünümüne yeniden yönlendirir.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager**. Tıklayarak silmek için bir albümü seçmek **silin.** Silme işlemini onaylamak **Sil** düğmesi:

    ![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "albüm siliniyor")

    *Albüm siliniyor*
3. İçinde görünmediğinden albümü silindiğini doğrulayın **dizin** sayfası.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Alıştırma 6: Doğrulama ekleme

Şu anda prosedürleriniz var oluşturma ve düzenleme form doğrulama herhangi bir türden gerçekleştirmeyin. Fiyat alanında harflerini yazın veya gerekli alanın boş kullanıcı ayrılsa erişmenizi sağlayacak ilk hata veritabanından olacaktır.

Veri ek açıklamaları model sınıfınızın ekleyerek uygulamaya doğrulama ekleyebilirsiniz. Veri ek açıklamaları model özelliklerinizi uygulanan istediğiniz kuralları açıklayan izin ve ASP.NET MVC zorlama ve kullanıcılara uygun iletisini gösteren ilgileniriz.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Görev 1 - veri ek açıklamaları ekleme

Bu görevde, uygun olduğunda doğrulama iletileri oluşturma ve düzenleme sayfası oluşturacak albüm modeline veri ek açıklamaları görüntülemek ekleyeceksiniz.

Basit bir Model sınıfı için veri ek açıklama ekleme yalnızca ekleyerek gerçekleştirilir bir **kullanarak** bildirimi **System.ComponentModel.DataAnnotation**, sonra yerleştirerek bir **[gerekli]** uygun özellikleri özniteliği. Aşağıdaki örnekte yapacağınız **adı** görünümünde gerekli bir alan özelliği.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Varlık veri modeli oluşturulduğu biraz daha karmaşık durumlarda bu uygulama gibi budur. Veri ek açıklamaları model sınıflarına doğrudan eklediyseniz, veritabanı modelden güncelleştirirseniz bunlar üzerine yazılır. Bunun yerine, yapabileceğiniz hangi ek açıklamalar tutacak yer alır ve modelle ilişkili meta verileri kısmi sınıflarının sınıflarını kullanarak **[MetadataType]** özniteliği.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex6-AddingValidation/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Album.cs** gelen **modelleri** klasör.
3. Değiştirin **Album.cs** içerik ile vurgulanmış kodu, böylece aşağıdaki gibi görünür:

    > [!NOTE]
    > Satır **[DisplayFormat(ConvertEmptyStringToNull=false)]** veri alanı veri kaynağında güncelleştirildiğinde modelinden boş dizeler null değerine dönüştürülüp olmaz olduğunu gösterir. Veri ek açıklama alanları doğrular önce Entity Framework modele null değerleri atarken bu ayar bir özel durum uğraşmasına gerek kalmaz.

    (Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex6 albüm meta verileri kısmi sınıf*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Bu **albüm** kısmi sınıf sahip bir **MetadataType** işaret özniteliği **AlbumMetaData** için veri ek açıklama sınıfı. Albüm model öğesine açıklama eklemek için kullanmakta olduğunuz veri ek açıklama öznitelikleri bazıları şunlardır:
    > 
    > - Gereklidir - gerekli bir alan özelliği belirtir
    > - DisplayName - form alanlarını ve doğrulama iletilerinin kullanılacak metni tanımlar
    > - -DisplayFormat veri alanlarını nasıl görüntülenir ve biçimlendirilmiş belirtir.
    > - StringLength - tanımlayan bir dize alanı için en fazla uzunluk
    > - Range - sayısal bir alan için bir maksimum ve minimum değer verir
    > - ScaffoldColumn - sağlayan alanları Düzenleyicisi formlarda gizleme

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulamayı çalıştırma

Bu görevde, oluşturma ve düzenleme sayfaları alanları doğrulamak son görevde seçilen görünen adlarını kullanarak test eder.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'yi **/StoreManager/oluşturma**. Görünen adlarını kısmi sınıf dışındaki eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**)
3. Tıklayın **Oluştur**, form doldurma olmadan. Karşılık gelen doğrulama iletilerini aldığını doğrulayın.

    ![Oluştur sayfasında alanlarını doğrulandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Oluştur sayfasında alanlarını doğrulandı")

    *Doğrulanmış alanları oluşturma sayfası*
4. Aynı ile gerçekleşir doğrulayabilirsiniz **Düzenle** sayfası. URL'yi **/StoreManager/Edit/1** ve görünen adlarını kısmi sınıf dışındaki eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**). Boş **başlık** ve **fiyat** alanları ve tıklatın **Kaydet**. Karşılık gelen doğrulama iletilerini aldığını doğrulayın.

    ![Doğrulanmış alanları düzenleme sayfası](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Doğrulanmış alanları düzenleme sayfası*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Alıştırma 7: İstemci tarafında örtük jQuery kullanma

Bu alıştırmada, istemci tarafındaki MVC 4 örtük jQuery doğrulamasını etkinleştirmek öğreneceksiniz.

> [!NOTE]
> Örtük jQuery JavaScript veri ajax önek intrusively yayan satır içi istemci betiklerini yerine sunucu eylem yöntemlerini çağırmak için kullanır.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Görev 1 - etkinleştirme örtük jQuery önce uygulamayı çalıştırma

Bu görevde, iki doğrulama modeli karşılaştırmak için jQuery dahil olmak üzere önce uygulamayı çalışır.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex7-UnobtrusivejQueryValidation/başlangıç/** klasör. Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.

   1. Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
   2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.
   3. Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.

      > [!NOTE]
      > NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir. NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak. Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **F5** uygulamayı çalıştırın.
3. Proje giriş sayfası başlatır. Gözat **/StoreManager/Create** tıklatıp **Oluştur** doğrulama iletilerinin aldığını doğrulamak için form doldurma olmadan:

    ![İstemci doğrulamasını devre dışı](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "istemci doğrulama devre dışı")

    *İstemci doğrulamasını devre dışı*
4. HTML kaynak kodu tarayıcıda açın:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Görev 2 - örtük istemci etkinleştirme doğrulama

Bu görevde, jQuery olanak sağlayacak **örtük istemci doğrulama** gelen **Web.config** dosya, varsayılan olarak tüm yeni ASP.NET MVC 4 projeleri false değerini alır. Ayrıca, gerekli komut dosyası başvuruları jQuery örtük istemci doğrulama iş yapma ekleyeceksiniz.

1. Açık **Web.Config** dosya proje kök dizininde ve emin **ClientValidationEnabled** ve **UnobtrusiveJavaScriptEnabled** anahtarları değerleri içinayarlanmış**true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > İstemci doğrulama kodu aynı sonuçları elde etmek için Global.asax.cs tarafından da etkinleştirebilirsiniz:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Ayrıca, özel bir davranış sağlamak için tüm Denetleyicisi'nde ClientValidationEnabled özniteliği atayabilirsiniz.
2. Açık **Create.cshtml** adresindeki **Views\StoreManager**.
3. Aşağıdaki komut dosyalarını emin **jquery.validate** ve **jquery.validate.unobtrusive**, görüntüleme yoluyla içinde başvurulan &quot; **~/bundles/jqueryval** &quot; paket.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Bu jQuery kitaplıklar yeni MVC 4 projeleri dahil edilir. Daha fazla kitaplıklarında bulabilirsiniz **/komut dosyası** , proje klasörü.
    > 
    > Bu doğrulama yapmak için kitaplıklar çalışma, jQuery framework kitaplığına bir başvuru eklemeniz gerekir. Bu başvuru zaten eklendiğinden beri  **\_Layout.cshtml** dosyası gerektirmeyen belirli bu görünümde eklemek.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Görev 3 - uygulamayı kullanarak örtük jQuery doğrulaması çalışan

Bu görevde, sınayacak **StoreManager** şablon kullanıcı yeni albümü oluşturduğunda, jQuery kitaplıklarını kullanarak istemci tarafı doğrulama gerçekleştirir görünüm oluşturma.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Gözat **/StoreManager/Create** tıklatıp **Oluştur** doğrulama iletilerinin aldığını doğrulamak için form doldurma olmadan:

    ![İstemci doğrulama etkin jQuery ile](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "etkin jQuery ile istemci doğrulaması")

    *Etkin jQuery ile istemci doğrulaması*
3. Oluştur görünümünün için kaynak kodu tarayıcıda açın:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Her istemci doğrulama kuralı için örtük jQuery verilerle bir öznitelik ekler-val -*rulename*=&quot;*ileti*&quot;. Etiketlerin listesini bu Unobtrusive aşağıdadır jQuery istemci doğrulama gerçekleştirmek için html giriş alanına ekler:
   > 
   > - Veri val
   > - Veri val numarası
   > - Veri val aralığı
   > - Veri val aralığı dakika / val aralığı maksimum veri
   > - Veri val gerekiyor
   > - Veri val uzunluğu
   > - Veri-val-uzunluk-max / veri val uzunluğu dakika
   > 
   > Tüm veri değerleri ile model doldurulur **veri ek açıklama**. Ardından, sunucu tarafında çalışan tüm mantığı, istemci tarafında çalıştırılabilir. Örneğin, fiyat özniteliği aşağıdaki veri ek açıklama modele sahiptir:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Örtük jQuery kullandıktan sonra oluşturulan kod şöyledir:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarı tamamlayarak aşağıdaki kullanarak veritabanında depolanan verileri değiştirme olanağı tanıma öğrendiniz:

- Dizin oluşturma, düzenleme, silme gibi denetleyici eylemleri
- ASP.NET MVC'nin iskele kurma özelliği ile HTML tablosu halinde özelliklerini görüntülemek için
- Kullanıcı geliştirmek için özel HTML Yardımcıları deneyimi
- HTTP GET veya POST HTTP çağrıları için react eylem yöntemleri
- Benzer bir görünüm şablonları oluşturma ve düzenleme gibi paylaşılan Düzenleyici şablonu
- Açılan listeler gibi form öğeleri
- Model doğrulama için veri ek açıklamaları
- JQuery örtük kitaplığını kullanarak istemci tarafı doğrulama

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Visual Studio'yu Web için Express 2012

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**. Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.
2. Tıklayarak **Şimdi Yükle**. Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.
3. Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleyin](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express'i yükle")

    *Visual Studio Express yükleyin*
4. Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Lisans koşullarını kabul etme*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında, tıklayın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Yükleme tamamlandı*
7. Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.

    ![Web kutucuğu için VS Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Web kutucuğu için VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***

1. Kod eklemesini istediğiniz imleci yerleştirin.
2. (Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).
5. İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.

![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "kod parçacığı adını yazmaya başlayın")

*Kod parçacığı adını yazmaya başlayın*

![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")

*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*

![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")

*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*

***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.

1. Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.
2. Tıklayarak ilgili kod parçacığı listeden seçin.

![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*

![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "tıklayarak ilgili kod parçacığı listeden seçin")

*Tıklayarak ilgili kod parçacığı listeden seçin*
