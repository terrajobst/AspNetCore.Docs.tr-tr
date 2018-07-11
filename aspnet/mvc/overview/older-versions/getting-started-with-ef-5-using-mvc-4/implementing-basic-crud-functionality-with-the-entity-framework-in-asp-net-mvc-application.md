---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: (2 / 10) ASP.NET MVC uygulamasındaki Entity Framework ile temel CRUD işlevselliği uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817089"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>(2 / 10) ASP.NET MVC uygulamasındaki Entity Framework ile temel CRUD işlevselliği uygulama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide depolar ve SQL Server LocalDB ve Entity Framework kullanarak verileri görüntüleyen bir MVC uygulaması oluşturdunuz. Bu öğreticide, gözden geçirin ve özelleştirme CRUD (oluşturma, okuma, güncelleştirme ve silme) MVC yapı iskelesi otomatik olarak sizin için denetleyicileri ve görünümleri oluşturan kodu.

> [!NOTE]
> Denetleyicinizi ve veri erişim katmanı arasında bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit tutmak için bu serinin sonraki bir eğitimde kadar bir depo uygulamak olmaz.


Bu öğreticide, aşağıdaki web sayfalarını oluşturacaksınız:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Ayrıntılar sayfası oluşturma

Öğrenciler için iskele kurulan kodu `Index` sol veya sayfa `Enrollments` özelliği, bunun nedeni, bu özellik bir koleksiyonu. İçinde `Details` sayfanın HTML tablosu halinde koleksiyonun içeriğini görüntülemek.

 İçinde *Controllers\StudentController.cs*, eylem yöntemi için `Details` görüntülemek kullandığı `Find` yönteminin tek bir almak için `Student` varlık. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Anahtar değeri yöntemine geçirilen `id` parametresi ve rota verileri geldiği **ayrıntıları** dizini sayfasında köprü. 

1. Açık *Views\Student\Details.cshtml*. Her bir alan kullanarak görüntülenen bir `DisplayFor` Yardımcısı, aşağıdaki örnekte gösterildiği gibi: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Sonra `EnrollmentDate` alan ve kapatmadan önce hemen `fieldset` etiketinde, aşağıdaki örnekte gösterildiği gibi kayıtları bir listesini görüntülemek için kod ekleyin:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Bu kod, varlıklarda döngü `Enrollments` gezinme özelliği. Her `Enrollment` varlık özelliğine, kurs başlığı ve sınıf gösterir. Kurs başlığı alınır `Course` depolanan varlık `Course` gezinti özelliği `Enrollments` varlık. Tüm bu verileri alınır veritabanından otomatik olarak gerektiğinde. (Diğer bir deyişle, yavaş yükleniyor burada kullanırsınız. Sizin belirtmediğiniz *istekli yükleme* için `Courses` gezinti özelliği, ilk kez bu özelliğe erişim deneyin. Bu nedenle, bir sorgu gönderilir veritabanına verileri almak üzere. Daha fazla bilgi edinebilirsiniz yavaş yükleniyor ve istekli yükleme hakkında [ilgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)
3. Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **ayrıntıları** Alexander Carson bağlantısı. Seçili Öğrenci için kursları ve notlarınızı listesi görürsünüz:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Oluştur sayfasında güncelleştiriliyor

1. İçinde *Controllers\StudentController.cs*, değiştirin `HttpPost``Create` eylem yöntemi eklemek için aşağıdaki kodu ile bir `try-catch` blok ve [Bind özniteliği](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) iskele kurulmuş yöntemi: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Bu kod ekler `Student` için ASP.NET MVC model bağlayıcı tarafından oluşturulan varlık `Students` varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder. (*Model bağlayıcı* yapar, bir form tarafından sunulan verilerle çalışmak daha kolaydır; dönüştürür gönderilen form değerleri CLR türlerine ve eylem yöntemi parametrelerine geçirir bir model bağlayıcı ASP.NET MVC işlevlerini gösterir. İn this Case, model bağlayıcı örnekleyen bir `Student` varlık özelliği kullanılarak sizin için değerleri `Form` koleksiyonu.)

    `ValidateAntiForgeryToken` Özniteliği önlemeye yardımcı olur; [siteler arası istek sahteciliğini](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları.

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

    Bazı veri doğrulama, varsayılan olarak çalışır. Adları ve geçersiz bir tarih girin ve tıklayın **Oluştur** hata iletisini görmek için.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Model doğrulama denetimi aşağıdaki vurgulanmış kodu gösterir.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Tarih 1/9/2005 gibi geçerli bir değere değiştirip'ı **Oluştur** görünen yeni Öğrenci görmek için **dizin** sayfası.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Düzenleme sonrası sayfası güncelleştiriliyor

İçinde *Controllers\StudentController.cs*, `HttpGet` `Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `Find` seçilen alınacak yöntemi `Student` varlığı gördüğünüz içinde `Details` yöntemi. Bu yöntem değiştirmeniz gerekmez.

Ancak, değiştirin `HttpPost` `Edit` eylem yöntemi eklemek için aşağıdaki kodu ile bir `try-catch` blok ve [Bind özniteliği](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Bu kod, ne gördüğünüz üzere benzer `HttpPost` `Create` yöntemi. Ancak, varlık kümesi için model bağlayıcı tarafından oluşturulan varlığı eklemek yerine, bu kod varlık üzerinde değiştirilip değiştirilmediğini gösteren bir bayrak ayarlar. Zaman [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi çağrıldığında [değiştirilen](https://msdn.microsoft.com/library/system.data.entitystate.aspx) bayrağı veritabanı satırı güncelleştirmek için SQL deyimlerini oluşturmak Entity Framework neden olur. Kullanıcı değişmedi, o da dahil olmak üzere veritabanı satırdaki tüm sütunlar güncelleştirilir ve eşzamanlılık çakışmalarını göz ardı edilir. (Bir sonraki Öğreticide bu serideki eşzamanlılık nasıl ele alınacağını öğreneceksiniz.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Varlık durumları ve ekleme ve SaveChanges yöntemleri

Veritabanı bağlamı varlıkları bellekte veritabanında ilgili satırlarında ile eşitlenmiş, bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, yeni bir varlık gönderdiğinizde [Ekle](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) varlığın durumu ayarlanmış yöntemi `Added`. Ardından çağırdığınızda [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi, veritabanı bağlamı veren bir SQL `INSERT` komutu.

Bir varlık birinde olabilir[durumları izleyen](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Varlık henüz veritabanında mevcut değil. `SaveChanges` Gereken yöntemini sorun bir `INSERT` deyimi.
- `Unchanged`. Hiçbir şey bu varlıkla tarafından yapılması gereken `SaveChanges` yöntemi. Bir varlık veritabanından okurken, varlık bu durumu ile başlar.
- `Modified`. Bazıları veya tümü varlığın özellik değerleri değiştirilmiş. `SaveChanges` Gereken yöntemini sorun bir `UPDATE` deyimi.
- `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Gereken yöntemini sorun bir `DELETE` deyimi.
- `Detached`. Varlık veritabanı bağlamı izleniyor değil.

Bir masaüstü uygulamasında, durum değişikliklerini genellikle otomatik olarak ayarlanır. Bir masaüstü uygulaması türünde, bir varlık okuyun ve değişiklik bazı özellik değerleri. Bu varlık durumunu otomatik olarak değiştirilecek şekilde neden `Modified`. Ardından çağırdığınızda `SaveChanges`, Entity Framework SQL oluşturur `UPDATE` deyimi, değiştirdiğiniz gerçek özelliklerini güncelleştirir.

Bağlantısı kesilmiş yapısı web Apps sürekli bu sıra için izin vermez. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) okuyan bir varlık, bir sayfa oluşturulduğunda bırakıldı. Zaman `HttpPost` `Edit` eylem yöntemi çağrıldığında, yeni bir istekte bulunulduğunda ve yeni bir örneğine sahip [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), varlık durumu el ile ayarlamak zorunda `Modified.` sonra çağırdığınızda `SaveChanges`, bağlamı değiştirmiş hangi özellikleri bilmek bir yolu yoktur çünkü Entity Framework, veritabanı satırdaki tüm sütunlar güncelleştirir.

SQL istiyorsanız `Update` kullanıcının gerçekten değişen alanları güncelleştirmek için deyimi bunların ne zaman kullanılabilir olacak şekilde bir şekilde (örneğin, gizli alanlar) özgün değerlerine kaydedebilirsiniz `HttpPost` `Edit` yöntemi çağrılır. Oluşturabileceğiniz daha sonra bir `Student` orijinal değerleri, çağrı kullanarak varlık `Attach` yöntemi, özgün varlık sürümü ile yeni değerlere varlığın değerlerini güncelleştirin ve ardından çağırmak `SaveChanges.` daha fazla bilgi için [ Varlık durumları ve SaveChanges](https://msdn.microsoft.com/data/jj592676) ve [yerel veri](https://msdn.microsoft.com/data/jj592872) MSDN veri Geliştirici Merkezi'nde.

Kodda *Views\Student\Edit.cshtml* ne gördüğünüz üzere benzer *Create.cshtml*, ve değişiklik gerekmez.

Sayfayı seçerek çalıştırın **Öğrenciler** sekmesini ve ardından bir **Düzenle** köprü.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Bazı tıklayın ve veri değiştirme **Kaydet**. Dizin sayfasında değiştirilen verileri görürsünüz.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete sayfa güncelleştiriliyor

İçinde *Controllers\StudentController.cs*, şablon kodunu `HttpGet` `Delete` yöntemi kullanan `Find` seçilen alınacak yöntemi `Student` varlığı gördüğünüz içinde `Details` ve `Edit` yöntemleri. Ancak, uygulamak için bir özel hata iletisi zaman çağrısı `SaveChanges` başarısız olursa, bazı işlevler, karşılık gelen görebilir ve bu yöntem ekleyeceksiniz.

Güncelleştirme için gördüğünüz ve oluşturma işlemleri gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak çağrılan yöntem kullanıcı onaylamak veya silme işlemi iptal etmek için bir şans verir bir görünüm görüntüler. Kullanıcıyı onaylarsa, bir POST isteği oluşturulur. Bu durum oluştuğunda `HttpPost` `Delete` yöntemi çağrılır ve bu yöntem gerçekten silme işlemini gerçekleştirir.

Ekleyeceğiniz bir `try-catch` bloğunu `HttpPost` `Delete` veritabanı güncelleştirildiğinde oluşabilecek hataları işlemek için yöntemi. Bir hata oluşursa `HttpPost` `Delete` yöntem çağrılarını `HttpGet` `Delete` yöntemi, bir hata oluştuğunu belirten bir parametre geçirerek. `HttpGet Delete` Yöntemi sonra onay sayfasında kullanıcı yeniden deneyin veya iptal etme fırsatı veren hata iletisi ile birlikte görüntüler.

1. Değiştirin `HttpGet` `Delete` eylem yöntemini aşağıdaki kodla hata raporlama'yı yöneten: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Bu kodu kabul eden bir [isteğe bağlı](https://msdn.microsoft.com/library/dd264739.aspx) değişiklikleri kaydetmek için bir hatadan sonra çağrıldı olup olmadığını gösteren bir Boole parametresi. Bu parametre `false` olduğunda `HttpGet` `Delete` önceki bir hata yöntemi çağrılır. Ne zaman çağırıldığında `HttpPost` `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası parametredir `true` ve bir hata iletisi görünüme iletilir.
2. Değiştirin `HttpPost` `Delete` eylem yöntemine (adlı `DeleteConfirmed`) gerçek silme işlemini gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Bu kod, seçili varlığı alır. ardından çağırır [Kaldır](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, bir SQL `DELETE` komut oluşturulur. Eylem yöntemi adından de değiştirdiğiniz `DeleteConfirmed` için `Delete`. İskele kurulan kodu adlı `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek `HttpPost` yöntemi benzersiz bir imza. (CLR'nin farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile devam edin ve aynı adı kullanın `HttpPost` ve `HttpGet` yöntemlerini silin.

     Yüksek hacimli uygulama performansını iyileştirme bir önceliktir çağıran kod satırı sayısını değiştirerek bir satır almak için gerekli olmayan bir SQL sorgusu kaçının `Find` ve `Remove` sarı renkli gösterildiği gibi aşağıdaki kod ile yöntemi Vurgulama:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Bu kod oluşturur bir `Student` varlık yalnızca birincil anahtar değerini kullanarak ve ardından varlık durumu ayarlar `Deleted`. Entity Framework varlığı silmek için gereken tüm budur.

     Belirtildiği gibi `HttpGet` `Delete` yöntemi verileri silme değil. Bir GET'e yanıt olarak bir silme işlemi gerçekleştirme isteği (veya herhangi bir düzenleme işlemini gerçekleştirirken bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturun) bir güvenlik riski oluşturur. Daha fazla bilgi için bkz. [ASP.NET MVC ipucu #46; bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther'ın blogunda.
3. İçinde *Views\Student\Delete.cshtml*, bir hata iletisi arasında ekleme `h2` başlık ve `h3` aşağıdaki örnekte gösterildiği gibi başlığı:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **Sil** köprü:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Tıklayın **Sil**. Dizin Sayfası silinen Öğrenci görüntülenir. (Bir örneğini bir hata işleme kodunu nasıl gerçekleştirildiğini göreceksiniz [eşzamanlılık işleme](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Veritabanı bağlantıları açık bırakılan değil, sağlama

Veritabanı bağlantıları düzgün bir şekilde kapatılır ve yukarı boşaltılmış tutun kaynakları emin olmak için bağlam örneğinin elden görmeniz gerekir. Diğer bir deyişle iskele kurulan kodu neden sağlar bir [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) sonunda yöntemi `StudentController` sınıfını *StudentController.cs*aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Temel `Controller` sınıf zaten uygular `IDisposable` Bu kod yalnızca bir geçersiz kılma ekler için arabirim `Dispose(bool)` bağlam örneğinin açıkça atmayı yöntemi.

## <a name="summary"></a>Özet

Artık sahip olduğunuz için basit CRUD işlemleri gerçekleştiren sayfalar eksiksiz bir kümesini `Student` varlıklar. MVC Yardımcıları veri alanları için kullanıcı Arabirimi öğeleri oluşturmak için kullanılır. MVC yardımcıları hakkında daha fazla bilgi için bkz: [formu kullanarak bir HTML Yardımcıları oluşturma](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (sayfa için MVC 3 ancak MVC 4 için hala geçerlidir.).

Sonraki öğreticide sayfalama ve sıralama ekleyerek dizin sayfası işlevselliğini genişletin.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [İleri](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
