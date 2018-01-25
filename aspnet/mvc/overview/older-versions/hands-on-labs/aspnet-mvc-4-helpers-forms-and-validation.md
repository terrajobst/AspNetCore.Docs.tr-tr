---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC 4 modelleri ve veri erişim uygulamalı Laboratuvar, alınan yükleniyor ve veritabanındaki verileri görüntüleme. Bu uygulamalı Laboratuvar için ekleyeceksiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

> İçinde **ASP.NET MVC 4 modelleri ve veri erişimi** uygulamalı Laboratuvar yükleniyor ve veritabanındaki verileri görüntüleme olmuştur. Bu uygulamalı laboratuar ortamında, ekleyecek **müzik deposu** uygulama bu verileri düzenleme özelliği.
> 
> Bu hedefi dikkate alarak, albümleri oluşturma, okuma, güncelleştirme ve silme (CRUD) eylemleri destekleyecek denetleyicisi ilk oluşturur. Bir HTML tablosunda albümleri özelliklerini görüntülemek için ASP.NET MVC'ın iskele özelliği yararlanarak bir dizin görünümünün şablonu oluşturur. Bu görünüm geliştirmek için uzun açıklamaları keser özel bir HTML Yardımcısı ekleyeceksiniz.
> 
> Daha sonra düzenleme ve oluşturma sağlayan görünümleri bırakmalar gibi form öğelerin yardımıyla veritabanında albümleri alter ekleyeceksiniz.
> 
> Son olarak, albümü silme kullanıcılar sağlar ve ayrıca, bunları kendi girişi doğrulama tarafından yanlış veri geçmesini engeller.
> 
> > [!NOTE]
> > Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**. Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC Temelleri** uygulamalı Laboratuvar.
> 
> 
> Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:

- CRUD işlemleri desteklemek için bir denetleyici oluşturma
- Bir HTML tablosunda varlık özellikleri görüntülemek için bir dizin Görünüm Oluştur
- Özel bir HTML Yardımcısı ekleme
- Oluşturma ve düzenleme görünümü özelleştirme
- HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri birbirinden
- Ekleme ve Görünüm Oluştur özelleştirme
- Tanıtıcı bir varlığı silme
- Kullanıcı girişini doğrulama

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

1. [Mağaza Yöneticisi denetleyicisi ve onun dizini görünümü oluşturma](#Exercise1)
2. [Bir HTML Yardımcısı ekleme](#Exercise2)
3. [Düzenleme görünümü oluşturma](#Exercise3)
4. [Oluştur görünümünün ekleme](#Exercise4)
5. [İşleme silme](#Exercise5)
6. [Doğrulama Ekleme](#Exercise6)
7. [İstemci tarafında örtük jQuery kullanma](#Exercise7)

> [!NOTE]
> Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör. Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.


Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Alıştırma 1: mağaza yöneticisi denetleyicisi ve onun dizini görünümü oluşturma

Bu alıştırmada, CRUD işlemleri desteklemek için veritabanını ve son olarak ASP.NET MVC'nin yapı iskelesi yararlanarak bir dizin görünümünün şablonu oluşturma albümleri listesini döndürmek için dizin eylem yöntemini Özelleştir yeni bir denetleyici oluşturma öğreneceksiniz. bir HTML tablosunda albümleri özelliklerini görüntülemek için özellik.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Görev 1 - StoreManagerController oluşturma

Bu görevde adlı yeni bir denetleyici oluşturacak **StoreManagerController** CRUD işlemleri desteklemek için.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex1-CreatingTheStoreManagerController/başlangıç/** klasörü.

    1. Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Yeni denetleyici ekleyin. Bunu yapmak için sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu. Değişiklik **denetleyicisi** **adı** için **StoreManagerController** ve seçeneği emin olun **boşokuma/yazmaeylemleriileMVCdenetleyicisi**seçilir. **Ekle**'yi tıklatın.

    ![Ekle denetleyicisi iletişim](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Ekle denetleyicisi iletişim")

    *Denetleyici Ekle iletişim kutusu*

    Yeni bir denetleyici sınıfı oluşturulur. Okuma/yazma, kişiler, saplama yöntemleri için Eylemler eklemek için belirtilen beri genel CRUD eylemleri uygulama belirli mantığı eklemek isteyen doldurulur Yapılacaklar açıklamaları ile oluşturulur.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Görev 2 - StoreManager dizinini özelleştirme

Bu görevde, veritabanından albümleri listesiyle birlikte bir görünüme dönmek için StoreManager dizin eylem yönteminin özelleştirin.

1. Aşağıdaki StoreManagerController sınıfında ekleyin *kullanarak* yönergeleri.

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MvcMusicStore kullanarak*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Bir alan eklemek **StoreManagerController** örneği tutmak için **MusicStoreEntities.**

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Albümleri listesiyle birlikte bir görünüme dönmek için StoreManagerController dizin eylemi uygulayın.

    Denetleyici eylem mantığı StoreController'ın dizin eylem için daha önce yazılmış çok benzer. LINQ tarz ve sanatçı bilgilerini görüntülemek için de dahil olmak üzere tüm albümleri almak için kullanın.

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 StoreManagerController dizin*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Görev 3 - dizin görünümü oluşturma

Bu görevde tarafından döndürülen albümleri listesini görüntülemek için dizin görünümünün şablonu oluşturacak **StoreManager** denetleyicisi.

1. Yeni Görünüm şablonu oluşturmadan önce projeyi derlemek böylece **Ekle iletişim kutusunu görüntüle** bildiği **albüm** kullanılacak sınıfı. Seçin **yapı | MvcMusicStore derleme** Projeyi derlemek için.
2. İçinde sağ **dizin** eylem yöntemi ve select **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

    ![Görünüm Ekle](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "görünümü ekleme")

    *Dizin yöntemi içinden bir görünümle ekleme*
3. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **dizin**. Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır. Seçin **listesi** gelen **İskele şablonu** açılır. Bırakın **görünüm altyapısı** için **Razor** ve diğer alanları varsayılan ile değer ve ardından **Ekle**.

    ![Bir dizini görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "bir dizini görünümü ekleme")

    *Bir dizini görünümü ekleme*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Görev 4 - dizin görünümünün iskele özelleştirme

Bu görevde, istediğiniz alanları görüntülemek için ASP.NET MVC yapı iskelesi özelliğiyle oluşturulmuş basit görünüm şablonu ayarlanır.

> [!NOTE]
> **İskele** ASP.NET MVC içinde desteği albüm modelindeki tüm alanları listeleyen basit bir görünüm şablon oluşturur. **İskele** kesin türü belirtilmiş bir görünüm üzerinde çalışmaya başlamak için hızlı bir yol sunar: görünüm şablonu el ile yazmak yerine hızlı bir şekilde iskele varsayılan şablon oluşturur ve oluşturulan kod daha sonra değiştirebilirsiniz.


1. Oluşturulan kod gözden geçirin. Alan listesinin oluşturulmasını aşağıdaki parçası olacak HTML tablo **İskele** tablo veri görüntülemek için kullanma.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Değiştir  **&lt;tablo&gt;**  yalnızca görüntülemek için aşağıdaki kodu koduyla **Tarz**, **sanatçı**, **albüm başlığı**, ve **fiyat** alanları. Bu siler **AlbumId** ve **albüm resim URL'si** sütun. Ayrıca, kullanıcıların bağlı sınıf özelliklerini görüntülemek için GenreId ve ArtistId sütunları değiştirir **Artist.Name** ve **Genre.Name**ve kaldırır **ayrıntıları** bağlantı.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Aşağıdaki açıklamaları değiştirin.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme önceki adımları tasarımını göre Albümler listesini görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager** gösteren albümleri listesini görüntülendiğini doğrulamak için kendi **başlık**, **sanatçı** ve **Tarz**.

    ![Albümleri listesi gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "albümleri listesi gözatma")

    *Albümleri listesi gözatma*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Alıştırma 2: bir HTML Yardımcısı ekleme

StoreManager dizin sayfası olası bir sorunu var: Başlık ve sanatçı ad özellikleri her ikisi de olabilir tablosunu biçimlendirme kapalı throw yetecek kadar uzun. Bu alıştırmada, metnin kesmek için özel bir HTML Yardımcısı eklemek öğreneceksiniz.

Aşağıdaki şekilde, bir küçük tarayıcı boyutu kullandığınızda biçimi metnin uzunluğu nedeniyle nasıl değiştirilir görebilirsiniz.

![İle albümleri listesi gözatma değil metin kesilmiş](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "ile albümleri listesi gözatma değil metin kesildi")

*İle albümleri listesi gözatma metin kesilmiş değil*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Görev 1 - HTML Yardımcısı genişletme

Bu görevde, yeni bir yöntem ekleyecek **Truncate** için **HTML** ASP.NET MVC görünümler içinde sunulan nesne. Bunu yapmak için gerçekleştireceksiniz bir **genişletme yöntemi** yerleşik için **System.Web.Mvc.HtmlHelper** ASP.NET MVC tarafından sağlanan sınıfı.

> [!NOTE]
> Hakkında daha fazla bilgi edinmek için **genişletme yöntemleri**, lütfen bu msdn makalesine bakın. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Açık **başlamak** çözüm bulunan **kaynak/Ex2-AddingAnHTMLHelper/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. StoreManager'ın dizini görünümünü açın. Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.
3. Aşağıdaki kodu ekleyin  **@model**  tanımlamak için yönergesi **Truncate** yardımcı yöntemi.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Görev 2 - metnin sayfadaki kesiliyor

Bu görevde, kullanacağınız **Truncate** şablonu görüntüleme metni kesmek için yöntem.

1. StoreManager'ın dizini görünümünü açın. Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.
2. Göster satırları değiştirmek **sanatçı adı** ve albüm **başlık**. Bunu yapmak için aşağıdaki satırları değiştirin.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme albüm başlık ve sanatçı adı tamsayıya dönüştürür.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager** bu uzun doğrulamak için metinleri **başlık** ve **sanatçı** sütun kesilir.

    ![Kesilen başlıkları ve Sanatçılar adları](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "kesilmiş başlıkları ve Sanatçılar adları")

    *Kesilmiş başlıklarını ve sanatçı adları*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Alıştırma 3: düzenleme görünümü oluşturma

Bu alıştırmada, albüm düzenlemek Mağaza yöneticileri izin vermek için bir formun nasıl oluşturulacağını öğreneceksiniz. Gözat **/StoreManager/Edit/id** URL (**kimliği** düzenlemek için albümü benzersiz kimliğini olan), böylece sunucusuna bir HTTP GET çağrısı yapma.

Denetleyici Düzenle eylem yöntemi uygun albüm veritabanından, oluşturma bir **StoreManagerViewModel** nesne (listesini Sanatçılar ve türler ile birlikte) kapsüllemek ve daha sonra bir görünüm şablonuna geçirin için HTML sayfası kullanıcıya geri işlenemiyor. Bu sayfayı içerecek bir  **&lt;form&gt;**  metin kutuları ve albüm özelliklerini düzenlemek için bırakmalar öğe.

Kullanıcı albüm form değerleri güncelleştirir ve tıklar sonra **kaydetmek** düğmesi, değişiklikleri aracılığıyla gönderilir bir HTTP POST geri çağrı **/StoreManager/Edit/id**. URL son çağrı olduğu gibi aynı kalsa da, ASP.NET MVC, bir HTTP POST ve bu nedenle farklı bir düzen eylem yöntemi yürütülmeden bu kez tanımlar (bir donatılmış ile **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Görev 1 - HTTP GET düzenleme eylem yöntemi uygulama

Bu görevde, uygun albüm veritabanından almak için düzenleme eylem yöntemini HTTP GET sürümü yanı sıra, tüm türler ve Sanatçılar listesini gerçekleştireceksiniz. Bu veriler içine paketi **StoreManagerViewModel** ardından yanıtı işlemek için bir şablonu görüntüleme geçirilecektir son adımda tanımlanan nesne.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex3-CreatingTheEditView/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Değiştir **HTTP GET Düzenle** uygun almak için aşağıdaki kodu eylem yöntemiyle **albüm** yanı sıra **türler** ve **Sanatçılar**listeler.

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP GET Düzenle eylem*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Kullanmakta olduğunuz **System.Web.Mvc** **SelectList** Sanatçılar ve yerine türler için **System.Collections.Generic** listesi.
    > 
    > **SelectList** HTML bırakmalar doldurmak ve geçerli seçim gibi şeyleri yönetmek için temiz bir yoludur. Örnek oluşturma ve bu denetleyici eylemini ViewModel nesneleri sonraki ayarlama düzenleme form senaryo temizleyici hale getirir.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Görev 2 - düzenleme görünümü oluşturma

Bu görevde, daha sonra Albüm özellikleri görüntüleyecek bir görünümü Düzenle şablonu oluşturacaksınız.

1. Düzenleme görünümü oluşturun. İçinde Bunu yapmak için sağ **Düzenle** eylem yöntemi ve select **Görünüm Ekle**.
2. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Düzenle**. Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm (MvcMusicStore.Models)** gelen **görüntülemek veri sınıfı** açılır. Seçin **Düzenle** gelen **İskele şablonu** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Düzenleme görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "düzenleme görünümü ekleme")

    *Düzenleme görünümü ekleme*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası parametre olarak geçirilen albüm özelliklerin değerlerini görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager/Edit/1** geçirilen albüm özelliklerin değerlerini görüntülendiğini doğrulayın.

    ![Albüm düzenleme görünümü gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "albüm düzenleme görünümü gözatma")

    *Albüm düzenleme görünümü gözatma*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Görev 4 - aşağı açılan listeler albüm Düzenleyicisi şablonu uygulama

Sanatçılar ve türler listesinden seçebilmeniz için bu görevde, aşağı açılan listeler son görevde oluşturulan görünüm şablona ekleyeceksiniz.

1. Tüm değiştirmek **albüm** fieldset kodu aşağıdakilerle:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Bir **Html.DropDownList** Yardımcısı, Sanatçılar ve türler seçmek için aşağı açılan listeler işlemek için eklendi. Parametreleri geçirilen **Html.DropDownList** şunlardır:
    > 
    > 1. Form alanı adını (**&quot;ArtistId&quot;**).
    > 2. **SelectList** aşağı açılan değer.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager/Edit/1** aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntülediğini doğrulamak üzere.

    ![Albüm Görünümü Düzenle açılan listeleri ile tarama](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "gözatma albüm düzenleme görünümü açılan listeleri")

    *Albüm düzenleme görünümü, bu kez bırakmalar gözatma*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Görev 6 - HTTP POST Düzenle eylem yöntemi uygulama

Görünümü Düzenle beklendiği gibi görüntüler, albümü yapılan değişiklikleri kaydetmek için HTTP POST Düzenle eylem yöntemi uygulamak için gerekir.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** gelen **denetleyicileri** klasör.
2. Değiştir **HTTP POST Düzenle** eylem yöntemini aşağıdaki kodla (değiştirilmelidir yöntemi iki parametre alan aşırı yüklenmiş sürümü olduğunu unutmayın):

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP POST Düzenle eylem*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Bu yöntem kullanıcı tıklattığında yürütülür **kaydetmek** görünümünün düğmesi ve bir HTTP veritabanında kalıcı hale getirmek için POST form değerlerinin sunucu gerçekleştirir. Oluşturma öğesi **[HttpPost]** yöntemi bu HTTP POST senaryoları için kullanılması gerektiğini gösterir. Yöntem alır bir **albüm** nesnesi. ASP.NET MVC otomatik olarak oluşturacak albüm nesne gönderilen &lt;form&gt; değerleri.
    > 
    > Yöntemi, aşağıdaki adımları gerçekleştirir:
    > 
    > 1. Model geçerliyse:
    > 
    >     1. Değiştirilen bir nesne olarak işaretlemek için bağlamda albüm giriş güncelleştirin.
    >     2. Değişiklikleri kaydetmek ve dizin görünümüne yönlendirin.
    > 2. Model geçerli değilse, Görünüm Paketi ile doldurulacaktır **GenreId** ve **ArtistId**, izin vermek için alınan albüm nesnenin görünümüyle döndürecektir gerekli herhangi bir güncelleştirme gerçekleştirin.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>7 - uygulama çalışan görev

Bu görevde, sınayacak **StoreManager düzenleme** görünüm sayfası veritabanında gerçekten güncelleştirilmiş albüm verileri kaydeder.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager/Edit/1**. Albüm başlığını değiştirme **yük** ve tıklayın **kaydetmek**. Albüm başlık gerçekte Albümler listesinde değiştiğini doğrulayın.

    ![Albüm güncelleştirme](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "albüm güncelleştiriliyor")

    *Albüm güncelleştiriliyor*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Alıştırma 4: oluşturma görünümü ekleme

Şimdi **StoreManagerController** destekleyen **Düzenle** özelliği, depolama sağlamak için Create VIEW şablon eklemek nasıl öğrenin Bu alıştırmada yöneticileri uygulamaya yeni Albümler ekleyin.

Düzenleme işlevselliği ile yaptığınız gibi içinde iki ayrı yöntemlerini kullanarak oluşturma senaryosu gerçekleştireceksiniz **StoreManagerController** sınıfı:

1. Bir eylem yöntemi, boş bir form görüntülenir, Mağaza yöneticileri ilk ziyaret ettiğinizde **/StoreManager/oluşturma** URL.
2. İkinci bir eylem yöntemi, burada mağaza yöneticisi tıklar senaryo işleyecek **kaydetmek** düğmesini formda ve değerleri yeniden gönderir **/StoreManager/oluşturma** URL bir HTTP POST olarak.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Görev 1 - HTTP GET oluşturma eylem yöntemi uygulama

Bu görevde, tüm türler ve Sanatçılar listesini almak için içine bu veri paketini oluşturma eylem yönteminin HTTP GET sürümünü uygulayacak bir **StoreManagerViewModel** sonra şablonu görüntüleme için geçirilen nesne.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex4-AddingACreateView/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Değiştir **oluşturma** eylem yöntemini aşağıdaki kodla:

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP GET oluşturma eylem*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Görev 2 - oluşturma görünümü ekleme

Bu görevde, yeni (boş) albüm form görüntüler Create VIEW şablon ekleyeceksiniz.

1. İçinde sağ **oluşturma** eylem yöntemi ve select **Görünüm Ekle**. Bu Görünüm Ekle iletişim kutusunu getirir.
2. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **oluşturma**. Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır ve **Oluştur** gelen **İskele şablonu** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Oluştur görünümünün ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ekleme-a-create-view.png")

    *Oluştur görünümünün ekleme*
3. Güncelleştirme **GenreId** ve **ArtistId** alanları aşağıda gösterildiği gibi açılır listesini kullanın:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Görev 3 - uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **oluşturma** görünüm sayfası boş bir albüm formu görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **oluşturma/StoreManager/**. Yeni albüm özellikleri doldurmak için boş bir form görüntülendiğini doğrulayın.

    ![Boş bir formla görünümü oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "boş bir formla Görünüm Oluştur")

    *Boş bir formla görünümü oluşturma*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Görev 4 - HTTP POST uygulama oluşturma eylemini yöntemi

Bu görevde, bir kullanıcı tıkladığında çağrılacak oluşturma eylem yöntemine HTTP POST sürümü gerçekleştireceksiniz **kaydetmek** düğmesi. Yöntem yeni albümü veritabanında kaydetmeniz gerekir.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
2. Değiştir **HTTP POST oluşturmak** eylem yöntemini aşağıdaki kodla:

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP POST Oluştur eylemi*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Oluşturma eylemini önceki düzenleme eylem yöntemine oldukça benzer, ancak nesne değiştirilmiş olarak ayarlamak yerine, bu bağlamda ekleniyor.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Görev 5 - Uygulama çalışıyor

Bu görevde, sınayacak **StoreManager oluşturma** görünüm sayfası, yeni albümü oluşturmanıza olanak tanır ve StoreManager dizin görünümüne yeniden yönlendirir.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **oluşturma/StoreManager/**. Tüm form alanlarını, aşağıdaki şekilde bir gibi yeni bir albüm için verilerle doldurun:

    ![Albüm oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "albüm oluşturma")

    *Albüm oluşturma*
3. Yeni oluşturduğunuz yeni albümü içeren StoreManager dizini görünümünü yönlendirilirsiniz doğrulayın.

    ![Oluşturulan yeni albümü](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "oluşturulan yeni albümü")

    *Oluşturulan yeni albümü*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Alıştırma 5: Silme işleme

Albümleri silme becerisine henüz uygulanmadı. Bu alıştırmada hakkında olacaktır budur. İçinde iki ayrı yöntemlerini kullanarak silme senaryoyu önce gerçekleştireceksiniz gibi **StoreManagerController** sınıfı:

1. Bir eylem yöntemi bir onay form görüntülenir
2. İkinci bir eylem yöntemi form gönderme işleyecek

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Görev 1 - HTTP GET silme eylem yöntemi uygulama

Bu görevde, albüm bilgileri almak için Delete eylem yöntemini HTTP GET sürümü gerçekleştireceksiniz.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex5-HandlingDeletion/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **StoreManagerController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
3. Delete denetleyici eylemi tam olarak önceki deposu ayrıntıları denetleyici eylemi aynıdır: Bu sorgular **albüm** kullanarak veritabanını nesnesinin **kimliği** sağlanan URL ve döndürür uygun **Görünüm**. Bunu yapmak için HTTP GET yerini **silmek** eylem yöntemini aşağıdaki kodla:

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP GET silme eylem*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. İçinde sağ **silmek** eylem yöntemi ve select **Görünüm Ekle**. Bu Görünüm Ekle iletişim kutusunu getirir.
5. Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **silmek**. Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır. Seçin **silmek** gelen **İskele şablonu** açılır. Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.

    ![Delete görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Delete görünümü ekleme")

    *Delete görünümü ekleme*
6. Delete şablonu modelden tüm alanları gösterir. Yalnızca albüm başlık gösterilir. Bunu yapmak için görünümü içeriğini aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulama çalışıyor

Bu görevde, sınayacak **StoreManager** **silmek** görünüm sayfası onay silme form görüntüler.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager**. Tıklayarak silmek için bir albümü seçmek **silmek** ve yeni görünüm yüklendiğini doğrulayın.

    ![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "albümü silme")

    *Albüm silme*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Görev 3-HTTP POST silme eylem yöntemi uygulama

Bu görevde, kullanıcı tıkladığında çağrılacak Delete eylem yöntemini HTTP POST sürümü gerçekleştireceksiniz **silmek** düğmesi. Yöntemi veritabanında albüm silmeniz gerekir.

1. Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın. Açık **StoreManagerController** sınıfı. Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.
2. Değiştir **HTTP POST silme** eylem yöntemini aşağıdaki kodla:

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP POST silme eylem*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Görev 4 - Uygulamayı çalıştırma

Bu görevde, sınayacak **StoreManager Delete** görünüm sayfası albüm silmenize olanak sağlar ve StoreManager dizin görünümüne yeniden yönlendirir.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **/StoreManager**. Tıklayarak silmek için bir albümü seçmek **silin.** Silme işlemini onaylamak **silmek** düğmesi:

    ![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "albümü silme")

    *Albüm silme*
3. İçinde görünmediğinden albümü silindiğini doğrulayın **dizin** sayfası.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Alıştırma 6: Doğrulama ekleme

Şu anda kullandığınız var oluşturma ve düzenleme forms her türlü doğrulama gerçekleştirmeyin. Kullanıcı fiyat alanında gerekli alan boş veya harflerini yazın ayrılsa karşılaşırsınız ilk hata veritabanından olacaktır.

Veri ek açıklamaları model sınıfınıza ekleyerek uygulamaya doğrulama ekleyebilirsiniz. ASP.NET MVC, zorlama ve kullanıcılara uygun ileti görüntüleme ilgilenebilmek ve veri ek açıklamaları model özelliklerinizi uygulanan istediğiniz kuralları açıklayan izin verin.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Görev 1 - veri ek açıklamaları ekleme

Bu görevde, doğrulama iletisi uygun olduğunda veri ek açıklamaları oluşturma ve düzenleme sayfa albüm modeline görüntülemek ekleyeceksiniz.

Basit bir Model sınıfı için bir veri ek açıklama ekleme yalnızca ekleyerek işlenir bir **kullanarak** bildirimi **System.ComponentModel.DataAnnotation**, sonra yerleştirerek bir **[gerekli]**uygun özellikleri özniteliği. Aşağıdaki örnek yapacağı **adı** özelliği görünümünde gerekli bir alan.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Varlık veri modeli oluşturulduğu bu uygulama gibi durumlarda biraz daha karmaşık budur. Veri ek açıklamaları model sınıflarına doğrudan eklediyseniz, modeli veritabanından güncelleştir varsa bunların üzerine. Bunun yerine, yapabileceğiniz ek açıklamalar tutmak için mevcut olur ve modeli ile ilişkili meta verileri kısmi sınıflarını kullanımını sınıflarını kullanarak **[MetadataType]** özniteliği.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex6-AddingValidation/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Açık **Album.cs** gelen **modelleri** klasör.
3. Değiştir **Album.cs** aşağıdaki gibi görünüyor. böylece vurgulanmış kodu ile içerik:

    > [!NOTE]
    > Satır **[DisplayFormat(ConvertEmptyStringToNull=false)]** veri alanı veri kaynağında güncelleştirildiğinde modelden boş dizelerin bir null değerine dönüştürülüp olmaz olduğunu gösterir. Veri ek açıklamasını alanları doğrular önce Entity Framework null değerler modele atarken bu ayar bir özel durum kaçının.

    (Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex6 albüm meta verileri parçalı sınıf*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Bu **albüm** kısmi sınıfına sahip bir **MetadataType** işaret özniteliği **AlbumMetaData** veri ek açıklamaları için sınıf. Bunlar, albüm model öğesine açıklama eklemek için kullandığınız veri ek açıklamasını öznitelikleri bazıları şunlardır:
    > 
    > - Gerekli - özelliği gerekli bir alan olduğunu gösterir
    > - DisplayName - form alanlarını ve doğrulama iletileri kullanılacak metni tanımlar
    > - DisplayFormat - veri alanları nasıl görüntülenir ve biçimlendirilmiş belirtir.
    > - StringLength - tanımlayan bir dize alanı için en fazla uzunluk
    > - Aralık - sayısal bir alan için bir maksimum ve minimum değeri verir
    > - ScaffoldColumn - sağlayan alanları Düzenleyicisi formlarda gizleme

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Görev 2 - uygulama çalışıyor

Bu görevde, oluşturma ve düzenleme sayfaları alanları doğrulamak son görevde seçilen görünen adları kullanarak test.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. URL'ye değiştirin **oluşturma/StoreManager/**. Görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**)
3. Tıklatın **oluşturma**, formu doldurarak olmadan. Karşılık gelen doğrulama iletilerini alma doğrulayın.

    ![Oluştur sayfası alanlarında doğrulanmış](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Oluştur sayfası alanlarında doğrulandı")

    *Doğrulanmış alanlarında Oluştur sayfası*
4. Aynı ile oluşur doğrulayabilirsiniz **Düzenle** sayfası. URL'ye değiştirin **/StoreManager/Edit/1** ve görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**). Boş **başlık** ve **fiyat** alanları ve tıklatın **kaydetmek**. Karşılık gelen doğrulama iletilerini alma doğrulayın.

    ![Düzenleme sayfasını doğrulanmış alanları](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Düzenleme sayfasını doğrulanmış alanları*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Alıştırma 7: İstemci tarafında örtük jQuery kullanma

Bu alıştırmada, istemci tarafında MVC 4 örtük jQuery doğrulamasını etkinleştirmek öğreneceksiniz.

> [!NOTE]
> Örtük jQuery veri ajax önek JavaScript intrusively verme satır içi istemci komut dosyalarını yerine sunucunuzda eylem yöntemleri çağırmak için kullanır.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Görev 1 - etkinleştirme örtük jQuery önce uygulamayı çalıştırma

Bu görevde, hem doğrulama modeli karşılaştırmak için jQuery dahil olmak üzere önce uygulamayı çalıştırın.

1. Açık **başlamak** çözüm bulunan **kaynak/Ex7-UnobtrusivejQueryValidation/başlangıç/** klasörü. Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.

    1. Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce. Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.
    2. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.
    3. Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.

    > [!NOTE]
    > NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir. NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır. Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.
2. Tuşuna **F5** uygulamayı çalıştırın.
3. Proje giriş sayfası başlatır. Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:

    ![İstemci doğrulama devre dışı](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "istemci doğrulama devre dışı")

    *İstemci doğrulama devre dışı*
4. Tarayıcıda, HTML kaynak kodunu açın:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Görev 2 - etkinleştirme örtük istemci doğrulama

Bu görevde, jQuery etkinleştirecek **örtük istemci doğrulama** gelen **Web.config** dosya, varsayılan olarak tüm yeni ASP.NET MVC 4 projelerini false değerini alır. Ayrıca, gerekli jQuery örtük istemci doğrulaması iş yapmak için başvurular komutlar ekleyeceksiniz.

1. Açık **Web.Config** dosya proje kök dizininde ve olduğundan emin olun **ClientValidationEnabled** ve **UnobtrusiveJavaScriptEnabled** anahtarları değerler için ayarlanır**true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Aynı sonuçları almak için Global.asax.cs kodu tarafından istemci doğrulaması da etkinleştirebilirsiniz:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Ayrıca, özel davranışı sağlamak için tüm Denetleyicisi'nde ClientValidationEnabled özniteliği atayabilirsiniz.
2. Açık **Create.cshtml** adresindeki **Views\StoreManager**.
3. Aşağıdaki komut dosyalarını emin olun **jquery.validate** ve **jquery.validate.unobtrusive**, görünüm yoluyla başvurulan &quot; **~/bundles/jqueryval** &quot; paket.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Bu jQuery kitaplıklar, MVC 4 yeni projelerinde dahil edilir. Daha fazla kitaplıklarında bulabilirsiniz **/komut dosyası** , projenin klasör.
    > 
    > Bu doğrulama yapmak için kitaplıkları iş, jQuery framework kitaplığına bir başvuru eklemeniz gerekir. Bu başvuru sayfasına eklendiğinden beri  **\_Layout.cshtml** dosyası gerektirmeyen belirli bu görünümde eklemek.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Görev 3 - uygulama kullanarak örtük jQuery doğrulama çalıştırma

Bu görevde, sınayacak **StoreManager** şablonu gerçekleştiren kullanıcı yeni albümü oluşturduğunda, jQuery kitaplıkları kullanarak istemci tarafı doğrulama görünüm oluşturun.

1. Tuşuna **F5** uygulamayı çalıştırın.
2. Proje giriş sayfası başlatır. Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:

    ![İstemci doğrulama etkin jQuery ile](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "etkin jQuery ile istemci doğrulama")

    *Etkin jQuery ile istemci doğrulama*
3. Tarayıcıda oluşturma görünüm için kaynak kodunu açın:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > Her istemci doğrulama kuralı için örtük jQuery verilerle bir öznitelik ekler-val -*rulename*=&quot;*ileti*&quot;. Etiketlerin listesini bu Unobtrusive aşağıdadır jQuery istemci doğrulama gerçekleştirmek için html giriş alanında ekler:
    > 
    > - Veri val
    > - Data-val-number
    > - Veri val aralığı
    > - Veri val aralığı min / val aralığı maksimum veri
    > - Veri val gerekiyor
    > - Veri val uzunluğu
    > - Verileri-val-uzunluk-max / verileri-val-uzunluk-min
    > 
    > Tüm veri değerleri ile model doldurulur **veri ek açıklamasını**. Ardından, sunucu tarafında çalışan tüm mantığı istemci tarafında çalıştırılabilir. Örneğin, fiyat özniteliği aşağıdaki veri ek açıklamasını modele sahiptir:
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > Örtük jQuery kullandıktan sonra oluşturulan kodu verilmiştir:
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı Laboratuvar tamamlayarak kullanıcılar aşağıdakileri kullanarak veritabanında depolanan verileri değiştirmek etkinleştirme öğrendiniz:

- Dizin oluşturma, düzenleme, silme gibi denetleyici eylemleri
- ASP.NET MVC'ın yapı iskelesi özelliği için bir HTML tablosunda özelliklerini görüntüleme
- Kullanıcı artırmak için özel HTML Yardımcıları deneyimi
- HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri
- Paylaşılan Düzenleyici şablonu oluşturma ve düzenleme gibi benzer görünüm şablonları için
- Aşağı açılan listeler gibi form öğeleri
- Model doğrulama için veri ek açıklamaları
- İstemci tarafı jQuery örtük kitaplığını kullanarak doğrulama

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Ek A: Yükleme Web Visual Studio Express 2012 için

Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** . Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.

1. Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.
2. Tıklayın **Şimdi Yükle**. Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.
3. Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.

    ![Visual Studio Express yükleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "yükleme Visual Studio Express")

    *Visual Studio Express yükleme*
4. Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Lisans koşulları kabul ediliyor*
5. İndirme ve yükleme işlemi tamamlanana kadar bekleyin.

    ![Yükleme ilerleme durumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Yükleme ilerleme durumu*
6. Yükleme tamamlandığında tıklatın **son**.

    ![Yükleme tamamlandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Yükleme tamamlandı*
7. Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.
8. Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.

    ![VS Express Web döşemeye](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express Web döşemeye*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Ek B: kod parçacıkları

Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip. Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.

![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")

*Kod projenize eklemek için Visual Studio kod parçacıkları*

***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***

1. İmleci, burada kod eklemek istediğiniz yerleştirin.
2. (Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.
3. Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.
4. Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).
5. İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.

![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "parçacığı adını yazmaya başlayın")

*Kod parçacığında adını yazmaya başlayın*

![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")

*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*

![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")

*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*

***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1. Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.

1. Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.
2. Tıklayarak ilgili kod parçacığında listeden seçin.

![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")

*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*

![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "tıklayarak ilgili kod parçacığında listeden seçin")

*Tıklayarak ilgili kod parçacığında listeden seçin*
