---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Model bağlama ve web forms ile verileri alma ve görüntüleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396291"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Model bağlama ve web forms ile verileri alma ve görüntüleme
====================

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
>  Model bağlama deseni, tüm veri erişim teknolojisi ile çalışır. Bu öğreticide, Entity Framework kullanır, ancak en tanıdık veri erişim teknolojisi kullanabilirsiniz. GridView, ListView, DetailsView veya FormView denetimi gibi bir verilere bağlı sunucu denetimden seçme, güncelleştirme, silme ve veri oluşturmak için kullanabileceğiniz yöntemler adlarını belirtin. Bu öğreticide, SelectMethod için bir değer belirtmeniz. 
> 
> Bu yöntem içinde veri almak için mantığı sağlar. Sonraki öğreticide UpdateMethod, DeleteMethod ve InsertMethod değerleri ayarlanır.
>
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projede C# veya Visual Basic. İndirilebilir kod, Visual Studio 2012 ve sonraki sürümlerinde çalışır. Bu öğreticide gösterilen Visual Studio 2017 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.
> 
> Öğreticide uygulamayı Visual Studio'da çalıştırın. Ayrıca, bir barındırma sağlayıcısı uygulamayı dağıtmak ve internet üzerinden hale getirebilirsiniz. Microsoft'un sunduğu en fazla 10 web siteleri için ücretsiz bir web barındırma bir  
>  [Ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Visual Studio web projesini Azure App Service Web Apps'e dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak ASP.NET Web dağıtımı](../../deployment/visual-studio-web-deployment/introduction.md) serisi. Bu öğretici ayrıca SQL Server veritabanınızı Azure SQL veritabanı'na dağıtmak için Entity Framework Code First Migrations kullanmayı gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> - Microsoft Visual Studio 2017 veya Microsoft Visual Studio Community 2017
>   
> Bu öğreticide Visual Studio 2012 ve Visual Studio 2013 ile de çalışır, ancak kullanıcı arabirimi ve proje şablonu bazı farklılıklar vardır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

* Üniversite öğrencileri eğitim kayıtlı yansıtan veri nesneleri oluşturma
* Derleme nesnelerden veritabanı tabloları
* Veritabanı test verileri ile doldurma
* Bir web formunda veri görüntüleme

## <a name="create-the-project"></a>Projeyi oluşturma

1. Visual Studio 2017'de oluşturma bir **ASP.NET Web uygulaması (.NET Framework)** adlı proje **ContosoUniversityModelBinding**.

   ![Proje oluşturma](retrieving-data/_static/image19.png)

2. **Tamam**’ı seçin. Bir şablon seçmek için iletişim kutusu görüntülenir.

   ![Web formları seçin](retrieving-data/_static/image3.png)

3. Seçin **Web Forms** şablonu. 

4. Gerekirse, kimlik doğrulaması değiştirin **bireysel kullanıcı hesapları**. 

5. Seçin **Tamam** projeyi oluşturmak için.

## <a name="modify-site-appearance"></a>Site görünümünü değiştirme

   Site görünümünü özelleştirmek için bazı değişiklikler yapın. 
   
   1. Site.Master dosyasını açın.
   
   2. Görüntülenecek başlığını değiştirme **Contoso University** değil **My ASP.NET Application**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Üst bilgi metni değiştirme **uygulama adı** için **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Uygun olanları site için Gezinti üst bilgi bağlantıları değiştirin. 
   
      Kaldırmak için bağlantıları **hakkında** ve **kişi** ve bunun yerine, bağlantı bir **Öğrenciler** oluşturacağınız sayfası.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Site.Master kaydedin.

## <a name="add-a-web-form-to-display-student-data"></a>Öğrenci verileri görüntülemek için bir web formu ekleyin

   1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın, **Ekle** ardından **yeni öğe**. 
   
   2. İçinde **Yeni Öğe Ekle** iletişim kutusunda **ana sayfa ile Web formu** şablon ve adlandırın **Students.aspx**.

      ![sayfası oluşturma](retrieving-data/_static/image5.png)

   3. **Add (Ekle)** seçeneğini belirleyin.
   
   4. Web formun ana sayfa seçin **Site.Master**.
   
   5. **Tamam**’ı seçin.
   

## <a name="add-the-data-model"></a>Veri modeli ekleme

İçinde **modelleri** klasör adında bir sınıf ekleyin **UniversityModels.cs**.

   1. Sağ **modelleri**seçin **Ekle**, ardından **yeni öğe**. **Yeni Öğe Ekle** iletişim kutusu görünür.

   2. Soldaki menüden **kod**, ardından **sınıfı**.

      ![Model sınıfı oluşturma](retrieving-data/_static/image20.png)

   3. Sınıf adı **UniversityModels.cs** seçip **Ekle**.

      Bu dosyadaki tanımlamak `SchoolContext`, `Student`, `Enrollment`, ve `Course` gibi sınıfları:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      `SchoolContext` Sınıf türetilir `DbContext`, veritabanı bağlantısını yönetir ve verileri değiştirir.

      İçinde `Student` sınıfı, uygulanan öznitelikleri duyuru `FirstName`, `LastName`, ve `Year` özellikleri. Bu öğreticide, veri doğrulama için bu öznitelikler kullanılır. Kodu basitleştirmek için yalnızca şu özellikler veri doğrulama öznitelikleri ile işaretlenir. Gerçek bir projede doğrulama öznitelikleri doğrulama gerektiren tüm özellikler için geçerlidir.

   4. UniversityModels.cs kaydedin.

## <a name="set-up-the-database-based-on-classes"></a>Sınıflara göre veritabanı ayarlama

Bu öğreticide [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) nesneleri oluşturmak ve tabloları veritabanı. Bu tablolar, Öğrenciler ve bunların dersler hakkında bilgi depolar.

   1. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

   2. İçinde **Paket Yöneticisi Konsolu**, şu komutu çalıştırın:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Komut başarıyla tamamlandıktan geçişleri etkin olmadığını bildiren bir ileti görüntülenir.

      ![migrations'ı etkinleştirme](retrieving-data/_static/image8.png)

      Adlı bir dosya bildirim *Configuration.cs* oluşturuldu. `Configuration` Sınıfında bir `Seed` yöntemi test verileri ile veritabanı tablolarını önceden doldurabilirsiniz.

## <a name="pre-populate-the-database"></a>Veritabanı önceden doldurun

   1. Configuration.cs açın.
   
   2. Aşağıdaki kodu ekleyin `Seed` yöntemi. Ayrıca, bir `using` bildirimi `ContosoUniversityModelBinding. Models` ad alanı.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Configuration.cs kaydedin.

   4. Paket Yöneticisi Konsolu'nda komutu Çalıştır **ekleme geçiş ilk**.

   5. Komutunu çalıştırın **veritabanını Güncelleştir**.

      Bu komutu çalıştırırken özel durum alırsanız `StudentID` ve `CourseID` değerleri farklı olabilir `Seed` yöntemi değerleri. Bu veritabanı tabloları açın ve varolan değerleri Bul `StudentID` ve `CourseID`. Bu değerleri dengeli dağıtım için kod ekleyin `Enrollments` tablo.

## <a name="add-a-gridview-control"></a>Bir GridView denetimi ekleme

Doldurulmuş veritabanı verileri, artık bu verileri almak ve görüntülemek hazırsınız. 

1. Open Students.aspx.

2. Bulun `MainContent` yer tutucu. Bu yer tutucu içinde ekleyin bir **GridView** bu kodu içeren bir denetim.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Dikkat edilecek noktalar:
   * Değeri ayarlamak için bildirim `SelectMethod` GridView öğesinde bulunan özellik. Bu değer sonraki adımda oluşturacağınız GridView verileri almak için kullanılan yöntemi belirtir. 
   
   * `ItemType` Özelliği `Student` daha önce oluşturduğunuz sınıfı. Bu ayar, sınıf özelliklerini biçimlendirmede başvuru sağlar. Örneğin, `Student` sınıfında adlı bir koleksiyon `Enrollments`. Kullanabileceğiniz `Item.Enrollments` Bu koleksiyonu alma ve ardından [LINQ söz dizimi](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) kredisi toplam kayıtlı her öğrencinin almak için.
   
3. Students.aspx kaydedin.

## <a name="add-code-to-retrieve-data"></a>Verileri almak için kod ekleyin

   Belirtilen yöntem Students.aspx arka plan kod dosyasında, ekleme `SelectMethod` değeri. 
   
   1. Open Students.aspx.cs.
   
   2. Ekleme `using` deyimleri için `ContosoUniversityModelBinding. Models` ve `System.Data.Entity` ad alanları.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Belirtilen için yöntem ekleme `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      `Include` Yan tümce sorgu performansını artırır, ancak gerekli değildir. Olmadan `Include` yan tümcesi, verileri kullanarak alınır [ *yavaş Yükleniyor*](https://en.wikipedia.org/wiki/Lazy_loading), ayrı bir sorgu her zaman veritabanına göndermeden kapsamaktadır ilgili veriler alınır. İle `Include` yan tümcesi, veri kullanarak alınır *istekli yükleme*, tek veritabanı sorgusu başka bir deyişle, tüm ilgili verileri alır. İlgili verileri kullanılmaz, daha fazla veri alındığından istekli yükleme verimli değildir. Her kayıt için ilgili verileri görüntülendiğinden ancak bu durumda, istekli yükleme, en iyi performansı sağlar.

      İlgili verileri yüklenirken performans değerlendirmeleri hakkında daha fazla bilgi için bkz: **Lazy Eager ve açık, ilgili veri yükleme** konusundaki [bir ASP.NET Entity Framework ile ilgili verileri okuma MVC uygulama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) makalesi.

      Varsayılan olarak, veri anahtarı olarak işaretlenmiş özelliğinin değerlere göre sıralanır. Ekleyebileceğiniz bir `OrderBy` yan tümcesini farklı bir sıralama değeri belirtin. Bu örnekte, varsayılan `StudentID` özelliği, sıralama için kullanılır. İçinde [sıralama, sayfalama ve filtreleme veri](sorting-paging-and-filtering-data.md) makalesi, kullanıcı sıralamak için sütun seçmek için etkinleştirilir.
 
   4. Students.aspx.cs kaydedin.

## <a name="run-your-application"></a>Uygulamanızı çalıştırın 

Web uygulamanızı çalıştırın (**F5**) gidin **Öğrenciler** sayfasında, şunları görüntüler:

   ![verileri göster](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Model bağlama yöntemleri otomatik olarak oluşturulmasını

Bu öğretici serisinin ile çalışırken, projenize öğreticinin kodu yalnızca kopyalayabilirsiniz. Ancak, bu yaklaşımın bir dezavantajı, model bağlama yöntemleri için kod otomatik olarak oluşturmak için Visual Studio tarafından sağlanan özellik, uyumlu olmayan bir duruma gelebilir emin olur. Otomatik kod oluşturma kendi projeleri üzerinde çalışıyorsanız, zaman ve nasıl bir işlem uygulanacağı bir fikir elde Yardım kaydedebilirsiniz. Otomatik kod oluşturma özelliği bu bölümde açıklanmaktadır. Bu bölümde, yalnızca bilgi amaçlıdır ve projenizde uygulamak için gereken herhangi bir kod içermiyor. 

İçin bir değer ayarlarken `SelectMethod`, `UpdateMethod`, `InsertMethod`, veya `DeleteMethod` seçebileceğiniz özellikleri biçimlendirme kodda **yeni yöntem oluşturma** seçeneği.

![bir yöntem oluşturma](retrieving-data/_static/image18.png)

Visual Studio yalnızca arka plan kod uygun imzaya sahip bir yöntem oluşturur, ancak aynı zamanda işlemi gerçekleştirmek için uygulama kodu oluşturur. İlk ayarlarsanız `ItemType` otomatik kod oluşturma kullanmadan önce özelliğini sunan, işlemleri için türü oluşturulan kod kullanır. Örneğin, ayarlarken `UpdateMethod` özelliği, aşağıdaki kodu otomatik olarak oluşturulur:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Yeniden, bu kod projenize eklenmesi gerekmez. Sonraki öğreticide, güncelleştirme, silme ve yeni veri eklemek için yöntemleri uygulayacaksınız.

## <a name="summary"></a>Özet

Bu öğreticide, oluşturulan veri modeli sınıfları ve bu sınıflardan bir veritabanı oluşturulur. Veritabanı tablolarını test verileri ile doldurulur. Model bağlama veritabanından veri almak için kullanılır ve ardından verileri GridView içinde görüntülenir.

Sonraki [öğretici](updating-deleting-and-creating-data.md) bu dizide, güncelleştirme, silme ve verileri oluşturma etkinleştirirsiniz.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
