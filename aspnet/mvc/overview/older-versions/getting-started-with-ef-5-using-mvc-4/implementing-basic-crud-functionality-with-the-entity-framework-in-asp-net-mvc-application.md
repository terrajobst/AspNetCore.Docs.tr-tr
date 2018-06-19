---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: ASP.NET MVC uygulaması (2 10) temel CRUD işlevselliği Entity Framework ile uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: acec5c9641b1de230956478c4396d1d541fcb0eb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875336"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>ASP.NET MVC uygulaması (2 10) temel CRUD işlevselliği Entity Framework ile uygulama
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide depolayan ve SQL Server yerel veritabanı ve Entity Framework kullanarak verileri görüntüleyen bir MVC uygulaması oluşturuldu. Bu öğreticide gözden geçirmenizi ve CRUD özelleştirme (oluşturma, okuma, güncelleştirme, silme) MVC yapı iskelesi otomatik olarak denetleyicileri ve görünümlerde oluşturur kodu.

> [!NOTE]
> Denetleyicinizi ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit tutmak için bu serideki sonraki öğretici kadar bir depo uygulamak olmaz.


Bu öğreticide, aşağıdaki web sayfaları oluşturursunuz:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Ayrıntılar sayfası oluşturma

Öğrenciler için kurulmuş kodu `Index` sol sayfa `Enrollments` özelliği, bunun nedeni, bu özelliği bir koleksiyon. İçinde `Details` sayfasında bir HTML tablosunda koleksiyonunun içeriğini görüntülemek.

 İçinde *Controllers\StudentController.cs*, eylem yöntemi için `Details` görüntülemek kullanır `Find` tek bir alma yöntemi `Student` varlık. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Anahtar değeri yöntemine geçirilen `id` parametre ve rota verilerini geldiği **ayrıntıları** dizin sayfasında köprü. 

1. Açık *Views\Student\Details.cshtml*. Her bir alan kullanılarak görüntülenen bir `DisplayFor` Yardımcısı, aşağıdaki örnekte gösterildiği gibi: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Sonra `EnrollmentDate` alan ve hemen kapatmadan önce `fieldset` etiketi, aşağıdaki örnekte gösterildiği gibi kayıtlarına listesini görüntülemek için kodu ekleyin:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Bu kod, varlıklarda döngü `Enrollments` gezinti özelliği. Her `Enrollment` varlık özelliğinde indirmelere başlık ve sınıf gösterir. İndirmelere başlık alınır `Course` depolanan varlık `Course` Gezinti özelliğinin `Enrollments` varlık. Tüm bu verileri alınır veritabanından otomatik olarak gerektiğinde. (Diğer bir deyişle, yavaş yükleniyor burada kullanmakta olduğunuz. Belirtmemek *istekli yükleme* için `Courses` gezinti özelliği, bu özelliğe erişmek deneyin ilk kez şekilde, bir sorgu gönderilir veritabanına veri almak için. Daha fazla bilgiyi yavaş yükleniyor ve istekli yükleme hakkında [ilgili veri okunurken](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)
3. Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **ayrıntıları** Alexander Cihangir'in Benli için bağlantı. Kurslar ve dereceleri listesi için seçilen Öğrenci bakın:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Oluştur sayfası güncelleştiriliyor

1. İçinde *Controllers\StudentController.cs*, Değiştir `HttpPost``Create` eklemek için aşağıdaki kodu eylem yöntemiyle bir `try-catch` bloğu ve [Bind özniteliği](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) kurulmuş yöntemi için: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Bu kod ekler `Student` için ASP.NET MVC model bağlayıcı tarafından oluşturulan varlık `Students` varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder. (*Model Bağlayıcısı* yapar, bir form tarafından sunulan verilerle çalışmak daha kolaydır; gönderilen dönüştürür form değerleri CLR türlerine ve eylem yönteminin parametrelerini geçirir bir model bağlayıcı ASP.NET MVC işlevlerini başvuruyor. İn this Case, model bağlayıcı başlatır bir `Student` varlık özelliğini kullanarak için değerleri `Form` koleksiyonu.)

    `ValidateAntiForgeryToken` Özniteliği yardımcı engellemek [siteler arası istek sahteciliğine](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak **Yeni Oluştur**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Bazı veri doğrulaması varsayılan olarak çalışır. Adları ve geçersiz bir tarih girin ve tıklayın **oluşturma** hata iletisini görmek için.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Model doğrulama denetimi aşağıdaki vurgulanmış kodu gösterir.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Tarih 1/9/2005 gibi geçerli bir değere değiştirip'ı **oluşturma** görünür yeni Öğrenci görmek için **dizin** sayfası.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Düzen POST sayfa güncelleştiriliyor

İçinde *Controllers\StudentController.cs*, `HttpGet` `Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `Find` seçili alma yöntemi `Student` varlık, gördüğünüz içinde `Details` yöntemi. Bu yöntem değiştirmeniz gerekmez.

Ancak, yerine `HttpPost` `Edit` eklemek için aşağıdaki kodu eylem yöntemiyle bir `try-catch` blok ve [Bind özniteliği](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Bu kod, ne de gördüğünüz için benzer `HttpPost` `Create` yöntemi. Bununla birlikte, varlık kümesi için model bağlayıcı tarafından oluşturulan varlık eklemek yerine, bu kod değiştirildi belirten varlık üzerinde bir bayrak ayarlar. Zaman [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi çağrıldığında, [değiştirilen](https://msdn.microsoft.com/library/system.data.entitystate.aspx) bayrağı veritabanı satırı güncelleştirmek için SQL deyimlerini oluşturmak Entity Framework neden olur. Kullanıcı değişmedi, da dahil olmak üzere tüm sütunları veritabanı satırın güncelleştirilir ve eşzamanlılık çakışması göz ardı edilir. (Bu serideki sonraki öğretici eşzamanlılık işlemeye nasıl öğreneceksiniz.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Varlık durumları ve Attach ve SaveChanges yöntemleri

Veritabanı bağlamı varlıkları bellekte kendi veritabanında karşılık gelen satır ile eşitleme ve bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, geçirdiğiniz zaman yeni bir varlık [Ekle](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) varlığın durumu kümesine yöntemi, `Added`. Çağırdığınızda sonra [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi, veritabanı bağlamı sorunları bir SQL `INSERT` komutu.

Bir varlık birinde olabilir[durumları aşağıdaki](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Varlık veritabanında henüz yok. `SaveChanges` Gerekir yöntemi sorun bir `INSERT` deyimi.
- `Unchanged`. Hiçbir şey bu varlık tarafından ile yapılması gerektiğini `SaveChanges` yöntemi. Veritabanından bir varlık okurken varlık bu durumu ile başlar.
- `Modified`. Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş. `SaveChanges` Gerekir yöntemi sorun bir `UPDATE` deyimi.
- `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Gerekir yöntemi sorun bir `DELETE` deyimi.
- `Detached`. Varlık veritabanı bağlamı tarafından izleniyor değil.

Bir masaüstü uygulaması durum değişiklikleri genellikle otomatik olarak ayarlanır. Uygulama Masaüstü tür, bir varlık okuyun ve bazı özellik değerleri değişiklikleri yapın. Bu şekilde otomatik olarak değiştirilmesi varlık durumuna neden `Modified`. Çağırdığınızda sonra `SaveChanges`, bir SQL Entity Framework oluşturur `UPDATE` yalnızca değiştirdiğiniz gerçek özelliklerini güncelleştirir deyimi.

Web uygulamaları bağlantısı kesilmiş yapısını sürekli bu sıra için izin vermez. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) okuyan bir sayfa oluşturulduğunda bir varlık atıldı. Zaman `HttpPost` `Edit` eylem yöntemi çağrıldığında, yeni bir istek yapıldığında ve yeni bir örneğini sahip [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), böylece varlık durumu el ile ayarlamak zorunda `Modified.` sonra çağırdığınızda `SaveChanges`, bağlam değiştirdiğiniz hangi özelliklerin kaybedeceğinizi öğrenmenin bir yolu bulunduğundan Entity Framework veritabanı satırın tüm sütunları güncelleştirir.

SQL istiyorsanız `Update` kullanıcının gerçekten değişen alanları güncelleştirmek için deyimi bunların ne zaman kullanılabilir olmasını sağlamak, özgün değerler (örneğin, gizli alanlar) herhangi bir yolla kaydedebilirsiniz `HttpPost` `Edit` yöntemi çağrılır. Oluşturabileceğiniz sonra bir `Student` çağrısı orijinal değerleri kullanarak varlık `Attach` varlık özgün sürümünün yöntemiyle varlığın değerleri için yeni değerleri güncelleştirmek ve ardından çağırın `SaveChanges.` daha fazla bilgi için bkz: [ Varlık durumları ve SaveChanges](https://msdn.microsoft.com/data/jj592676) ve [yerel veri](https://msdn.microsoft.com/data/jj592872) MSDN veri Geliştirici Merkezi'nde.

Kodda *Views\Student\Edit.cshtml* ne de gördüğünüz için benzer *Create.cshtml*, ve değişiklik gerekmez.

Sayfayı seçerek çalıştırın **Öğrenciler** sekmesini ve ardından bir **Düzenle** köprü.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Bazı tıklatın ve verileri değiştirme **kaydetmek**. Dizin Sayfası değiştirilen verileri bakın.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete sayfa güncelleştiriliyor

İçinde *Controllers\StudentController.cs*, şablon kodunu `HttpGet` `Delete` yöntemi kullanır `Find` seçili almak için yöntemini `Student` varlığı gördüğünüz içinde `Details` ve `Edit` yöntemleri. Ancak, uygulamak için bir özel hata iletisi ne zaman çağrısı `SaveChanges` başarısız olursa, karşılık gelen görebilir ve bu yöntem işlevselliğinin ekleyeceksiniz.

Güncelleştirmesi gördünüz ve işlemler oluşturmak gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak adlandırılan yöntemi onaylamak veya silme işlemini iptal etmek için bir fırsat kullanıcıya veren bir görünüm görüntüler. Kullanıcı onaylarsa, bir POST isteği oluşturulur. Bu durum oluştuğunda `HttpPost` `Delete` yöntemi çağrılır ve bu yöntem gerçekte silme işlemi gerçekleştirir.

Ekleyeceksiniz bir `try-catch` için engelleyin `HttpPost` `Delete` veritabanı güncelleştirildiğinde oluşabilecek tüm hataları işlemek için yöntem. Bir hata oluşursa, `HttpPost` `Delete` yöntem çağrılarını `HttpGet` `Delete` yöntemi, bir hata oluştuğunu gösteren bir parametre geçirme. `HttpGet Delete` Yöntemi ardından onay sayfasında kullanıcı iptal etme veya yeniden denemek için bir fırsat vermiş hata iletisi ile birlikte görüntüler.

1. Değiştir `HttpGet` `Delete` hata raporlama yönetir aşağıdaki kodla eylem yöntemi: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Bu kodu kabul eden bir [isteğe bağlı](https://msdn.microsoft.com/library/dd264739.aspx) değişiklikleri kaydetmek için bir hatadan sonra çağrıldı olup olmadığını gösteren Boole parametresi. Bu parametre `false` zaman `HttpGet` `Delete` yöntemi, önceki bir hata çağrılır. Ne zaman çağırıldığında `HttpPost` `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası parametredir `true` ve bir hata iletisi görünümüne geçirilir.
2. Değiştir `HttpPost` `Delete` eylem yöntemi (adlı `DeleteConfirmed`) gerçek silme işlemi gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Seçilen varlığın bu kodu alır sonra çağırır [kaldırmak](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, bir SQL `DELETE` komutu oluşturulur. Ayrıca eylem yönteminin adı değiştirilmiştir `DeleteConfirmed` için `Delete`. Adlı kurulmuş kodu `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek için `HttpPost` yöntemi benzersiz bir imza. (CLR farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile takılıyor ve için aynı adı kullanmak `HttpPost` ve `HttpGet` yöntemlerini silin.

     Yüksek hacimli uygulama performansını iyileştirme bir öncelik ise, çağrı kod satırlarını değiştirerek satır almak için gerekli olmayan bir SQL sorgusu kaçının `Find` ve `Remove` sarı ile gösterildiği gibi aşağıdaki kodla yöntemleri vurgula:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Bu kod başlatır bir `Student` yalnızca birincil anahtar değerini kullanarak varlık ve varlık durumu ayarlar `Deleted`. Entity Framework varlığı silmek için gereken tüm budur.

     Belirtildiği gibi `HttpGet` `Delete` yöntemi verileri sil değil. Bir GET'e yanıt olarak bir silme işlemi isteği (veya herhangi bir düzenleme işlemi gerçekleştirme Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) bir güvenlik riski oluşturur. Daha fazla bilgi için bkz: [ASP.NET MVC ipucu #46 — güvenlik açıklarını oluşturduğundan bağlantılarını sil kullanmayan](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther'ın blogunda.
3. İçinde *Views\Student\Delete.cshtml*, hata iletisi arasında eklemek `h2` başlık ve `h3` , aşağıdaki örnekte gösterildiği gibi Başlık:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **silmek** köprü:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Tıklatın **silmek**. Dizin Sayfası silinen Öğrenci görüntülenir. (Hata işleyen kodu eyleminin içine bir örneğini göreceksiniz [eşzamanlılık işleme](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Veritabanı bağlantıları açık kalan sağlama

Veritabanı bağlantıları düzgün kapalı olduğundan ve yukarı boşaltılmış tutun kaynakları emin olmak için kendisine bağlam örneği atıldı görmeniz gerekir. Diğer bir deyişle neden kurulmuş kodu sağlar bir [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) yöntemi sonunda `StudentController` sınıfını *StudentController.cs*, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Temel `Controller` sınıf zaten uygulayan `IDisposable` Bu kod yalnızca bir geçersiz kılma ekler için arabirim `Dispose(bool)` açıkça bağlam örneğinin dispose yöntemi.

## <a name="summary"></a>Özet

Basit CRUD işlemleri için sayfaları eksiksiz bir kümesini şimdi sahip `Student` varlıklar. MVC Yardımcıları veri alanları için kullanıcı Arabirimi öğeleri oluşturmak için kullanılır. MVC yardımcıları hakkında daha fazla bilgi için bkz: [bir Form kullanarak HTML Yardımcıları işleme](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (sayfa için MVC 3 ancak MVC 4 için hala geçerlidir.).

Sonraki öğreticide sıralama ve disk belleği ekleyerek dizin sayfası işlevselliğini genişletmek.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [sonraki](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
