---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: ASP.NET MVC uygulamasındaki temel CRUD işlevselliği Entity Framework ile uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 14f5143bb5086890d4a2f2fb3b98f1be88a549a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>ASP.NET MVC uygulamasındaki temel CRUD işlevselliği Entity Framework ile uygulama
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide depolayan ve SQL Server yerel veritabanı ve Entity Framework kullanarak verileri görüntüleyen bir MVC uygulaması oluşturuldu. Bu öğreticide gözden geçirmenizi ve CRUD özelleştirme (oluşturma, okuma, güncelleştirme, silme) MVC yapı iskelesi otomatik olarak denetleyicileri ve görünümlerde oluşturur kodu.

> [!NOTE]
> Denetleyicinizi ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit ve Entity Framework kullanmayı öğretmek odaklanmıştır tutmak için bunlar depoları kullanmayın. Uygulama havuzları hakkında daha fazla bilgi için bkz: [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).


Bu öğreticide, aşağıdaki web sayfaları oluşturursunuz:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Ayrıntılar sayfası oluşturma

Öğrenciler için kurulmuş kodu `Index` sol sayfa `Enrollments` özelliği, bunun nedeni, bu özelliği bir koleksiyon. İçinde `Details` sayfasında bir HTML tablosunda koleksiyonunun içeriğini görüntülemek.

 İçinde *Controllers\StudentController.cs*, eylem yöntemi için `Details` görüntülemek kullanır [Bul](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) tek bir alma yöntemi `Student` varlık. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Anahtar değeri yöntemine geçirilen `id` parametre ve geldiği *rota veri* içinde **ayrıntıları** dizin sayfasında köprü.

### <a name="tip-route-data"></a>İpucu: **rota verileri**

Rota verilerini, model bağlayıcı yönlendirme tablosunda belirtilen URL kesimi bulunan verilerdir. Örneğin, varsayılan yolu belirtir `controller`, `action`, ve `id` kesimleri:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Aşağıdaki URL'de varsayılan rotayı eşler `Instructor` olarak `controller`, `Index` olarak `action` ve 1 olarak `id`; bunlar rota veri değerlerdir.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID 2021 =" sorgu dizesi değeridir. Geçirdiğiniz model bağlayıcı ayrıca çalışmayacak `id` bir sorgu dizesi değeri olarak:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

URL'leri tarafından oluşturulan `ActionLink` Razor görünüm deyimlerinde. Aşağıdaki kodda, `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` rota verilerini eklenir.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Aşağıdaki kodda, `courseID` sorgu dizesi olarak eklenmiş şekilde varsayılan yol parametresinde eşleşmiyor.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Açık *Views\Student\Details.cshtml*. Her bir alan kullanılarak görüntülenen bir `DisplayFor` Yardımcısı, aşağıdaki örnekte gösterildiği gibi:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Sonra `EnrollmentDate` alan ve hemen kapatmadan önce `</dl>` etiketi, aşağıdaki örnekte gösterildiği gibi kayıtlarına listesini görüntülemek için vurgulanmış kodu ekleyin:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Kod yapıştırıldıktan sonra kod girintileme yanlışsa, düzeltmek için CTRL-K-D tuşuna basın.

    Bu kod, varlıklarda döngü `Enrollments` gezinti özelliği. Her `Enrollment` varlık özelliğinde indirmelere başlık ve sınıf gösterir. İndirmelere başlık alınır `Course` depolanan varlık `Course` Gezinti özelliğinin `Enrollments` varlık. Tüm bu verileri alınır veritabanından otomatik olarak gerektiğinde. (Diğer bir deyişle, yavaş yükleniyor burada kullanmakta olduğunuz. Belirtmemek *istekli yükleme* için `Courses` kayıtları Öğrenciler var aynı sorguda alınamadı için gezinme özelliği. Bunun yerine, ilk kez denediğinizde erişim `Enrollments` gezinti özelliği, yeni bir sorgu gönderilir veritabanına veri almak için. Daha fazla bilgiyi yavaş yükleniyor ve istekli yükleme hakkında [ilgili veri okunurken](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)
3. Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **ayrıntıları** Alexander Cihangir'in Benli için bağlantı. (Details.cshtml dosya açıkken CTRL + F5 tuşuna basarsanız, bir HTTP 400 Hata Ayrıntıları sayfası çalıştırmak Visual Studio çalışır ancak görüntülenecek Öğrenci belirten bir bağlantı üst sınırına tamamlanmadığı için elde edersiniz. İn that Case, yalnızca "Öğrenci/Ayrıntılar" URL'den kaldırmak ve yeniden deneyin veya tarayıcıyı kapatın, projeye sağ tıklayın ve **Görünüm**ve ardından **tarayıcıda görüntüle**.)

    Kurslar ve dereceleri listesi için seçilen Öğrenci bakın:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

1. İçinde *Controllers\StudentController.cs*, Değiştir `HttpPost``Create` eklemek için aşağıdaki kodu eylem yöntemiyle bir `try-catch` engelleme ve kaldırma `ID` gelen [Bind özniteliği](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) için kurulmuş yöntemi:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Bu kod ekler `Student` için ASP.NET MVC model bağlayıcı tarafından oluşturulan varlık `Students` varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder. (*Model Bağlayıcısı* yapar, bir form tarafından sunulan verilerle çalışmak daha kolaydır; gönderilen dönüştürür form değerleri CLR türlerine ve eylem yönteminin parametrelerini geçirir bir model bağlayıcı ASP.NET MVC işlevlerini başvuruyor. İn this Case, model bağlayıcı başlatır bir `Student` varlık özelliğini kullanarak için değerleri `Form` koleksiyonu.)

    Kaldırılan `ID` bağlama özniteliği olduğundan `ID` satır zaman eklenen SQL Server otomatik olarak ayarlayacak birincil anahtar değeri. Kullanıcı girişi ayarlı değil `ID` değeri.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Güvenlik Uyarısı - `ValidateAntiForgeryToken` özniteliği yardımcı engellemek [siteler arası istek sahteciliğine](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları. Karşılık gelen gerektirir `Html.AntiForgeryToken()` deyimi görünümünde, daha sonra göreceksiniz.

    `Bind` Karşı koruma yollarından özniteliğidir *aşırı gönderim* senaryoları oluşturun. Örneğin, varsayalım `Student` varlık içeren bir `Secret` ayarlamak için bu web sayfası istemediğiniz özelliği.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Yüklü olmasa bile, bir `Secret` alan web sayfasında, bir bilgisayar korsanının kullanabilir bir aracı gibi [fiddler](http://fiddler2.com/home), veya sonrası için bazı JavaScript Yazma bir `Secret` form değeri. Olmadan [bağlamak](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) oluştururken, model Bağlayıcısı kullandığını alanları sınırlama özniteliği bir `Student` örneği<em>,</em> model bağlayıcı ayarladıysanız çekme `Secret` form değeri ve için bunu kullanın oluşturma `Student` varlık örneği. Sonra ne olursa olsun değer için belirtilen korsan `Secret` form alanı veritabanınızda güncelleştirilmesi. Aşağıdaki resimde fiddler gösterilmiştir aracı ekleme `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    "OverPost" sonra başarıyla eklenmesi için değer `Secret` özelliği eklenen web sayfası bu özelliği ayarlayamaz hiçbir zaman amaçlı rağmen satır.

    Bunu kullanmak için bir güvenlik en iyi uygulamadır `Include` parametresiyle `Bind` özniteliğini *beyaz liste* alanları. Kullanmak da mümkündür `Exclude` parametresi *kara liste* hariç tutmak istediğiniz alanları. Bunun nedeni `Include` daha güvenlidir varlığa yeni bir özellik eklediğinizde, yeni alan otomatik olarak tarafından korunmadığını olan bir `Exclude` listesi.

    Engel olabilirsiniz overposting düzenleme senaryolarında olduğu varlık veritabanından ilk okuma ve ardından çağırma `TryUpdateModel`, açıkça izin verilen özellikler listesinde geçen. Bu öğreticiler kullanılan yöntem olmasıdır.

    Birçok geliştiriciler tarafından tercih edilen overposting önlemek için alternatif bir yol sınıflar yerine modelleri görüntüle model bağlamayla kullanmaktır. Yalnızca görünüm modeli güncelleştirmek istediğiniz özellikleri içerir. MVC model bağlayıcı tamamladığında, isteğe bağlı olarak bir aracı gibi kullanarak varlık örneği için Görünüm model özellikleri kopyalamak [AutoMapper](http://automapper.org/). DB kullanın. Girişi durumuna Unchanged olarak ayarlayın ve ardından Property("PropertyName") ayarlamak için varlık örneği. IsModified görünüm modele dahil her varlık özellikte true. Bu yöntem works hem de düzenleyebilir ve senaryoları oluşturabilir.

    Dışında `Bind` özniteliği `try-catch` taşıdır kurulmuş kodu yapılan tek değişiklik. Türetilen bir özel durum, [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) olan değişiklikler kaydedilirken yakalandı, genel bir hata iletisi görüntülenir. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) özel durumlar bazen neden bir programlama hatası yerine uygulama için dış bir şey için kullanıcı yeniden denemek için önerilir. Bu örnekte uygulanmadı karşın, bir üretim kalitesi uygulama özel durum oturum açabilirsiniz. Daha fazla bilgi için bkz: **hakkında bilgi için günlük** bölümüne [izleme ve Telemetri (yapı gerçek bulut uygulamaları Azure ile)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kodda *Views\Student\Create.cshtml* ne de gördüğünüz için benzer *Details.cshtml*dışında `EditorFor` ve `ValidationMessageFor` Yardımcıları yerineherbiralaniçinkullanılan`DisplayFor`. İlgili kod aşağıdaki gibidir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* de içerir `@Html.AntiForgeryToken()`, çalıştığı ile `ValidateAntiForgeryToken` önlemeye yardımcı olmak için denetleyici özniteliğinde [siteler arası istek sahteciliğine](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları.

    ' De değişiklik gerekli *Create.cshtml*.
2. Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak **Yeni Oluştur**.
3. Adları ve geçersiz bir tarih girin ve tıklayın **oluşturma** hata iletisini görmek için.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Varsayılan olarak aldığınız sunucu tarafında doğrulama budur; sonraki öğreticide istemci tarafı doğrulama kodunu da oluşturur özniteliklerini ekleme görürsünüz. Model doğrulama denetimi ile aşağıdaki vurgulanmış kodu gösterir **oluşturma** yöntemi.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Tarih geçerli bir değere değiştirip'ı **oluşturma** görünür yeni Öğrenci görmek için **dizin** sayfası.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Güncelleştirmeyi Düzenle HttpPost yöntemi

İçinde *Controllers\StudentController.cs*, `HttpGet` `Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `Find` seçili alma yöntemi `Student` varlık, gördüğünüz içinde `Details` yöntemi. Bu yöntem değiştirmeniz gerekmez.

Ancak, yerine `HttpPost` `Edit` eylem yöntemine aşağıdaki kodu:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Bu değişiklikleri önlemek için bir en iyi güvenlik uygulaması uygulamak [overposting](#overpost), oluşturulan iskele kurucu bir `Bind` özniteliği ve varlık değiştirilen bayrağıyla kümesi için model bağlayıcı tarafından oluşturulan varlık eklenir. Kod nedeniyle artık önerilir `Bind` özniteliği temizler listelenmez alanları önceden mevcut verileri dışarı `Include` parametresi. Böylece, oluşturmaz gelecekte MVC denetleyicisi iskele kurucu güncelleştirileceği `Bind` düzenleme yöntemleri için öznitelikler.

Varolan bir varlık ve çağrıları yeni kod okur [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) gönderilen form verilerini uygulamasında kullanıcı girdisi alanlardan güncelleştirmek için. Entity Framework'ün otomatik değişiklik kümelerini izleme [değiştirilen](https://msdn.microsoft.com/library/system.data.entitystate.aspx) varlık bayrağı. Zaman [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi çağrıldığında, `Modified` bayrağı veritabanı satırı güncelleştirmek için SQL deyimlerini oluşturmak Entity Framework neden olur. [Eşzamanlılık çakışması](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) dikkate alınmaz ve kullanıcı değişmedi da dahil olmak üzere tüm sütunları veritabanı satırın güncelleştirilir. (Bir sonraki öğretici eşzamanlılık çakışmaları nasıl ele alınacağını gösterir ve veritabanında güncelleştirilmesi için tek tek alanların yalnızca istiyorsanız, varlık Unchanged olarak ayarlamak ve tek tek ayarlamak değiştirilen alanları.)

Overposting önlemek için en iyi uygulama olarak, Güvenilenler listesine Düzenle sayfası tarafından güncelleştirilebilmesi için istediğiniz alanları olan `TryUpdateModel` parametreleri. Şu anda koruduğunuz hiçbir ek alanlar vardır, ancak veri modeline gelecekte alanlar eklerseniz, açıkça bunları burada yüklemediğiniz sürece otomatik olarak korumalı olup olmadıklarını olduğunu bağlamak için model bağlayıcı istediğiniz alanları listesi sağlar.

Bu değişikliklerin sonucu olarak HttpPost düzenleme yöntemi yöntemi imzası HttpGet düzenleme yöntemi ile aynıdır; Bu nedenle EditPost yöntemi değiştirdiniz.

> [!TIP] 
> 
> **Varlık durumları ve Attach ve SaveChanges yöntemleri**
> 
> Veritabanı bağlamı varlıkları bellekte kendi veritabanında karşılık gelen satır ile eşitleme ve bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, geçirdiğiniz zaman yeni bir varlık [Ekle](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) varlığın durumu kümesine yöntemi, `Added`. Çağırdığınızda sonra [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi, veritabanı bağlamı sorunları bir SQL `INSERT` komutu.
> 
> Bir varlık birinde olabilir[durumları aşağıdaki](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`. Varlık veritabanında henüz yok. `SaveChanges` Gerekir yöntemi sorun bir `INSERT` deyimi.
> - `Unchanged`. Hiçbir şey bu varlık tarafından ile yapılması gerektiğini `SaveChanges` yöntemi. Veritabanından bir varlık okurken varlık bu durumu ile başlar.
> - `Modified`. Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş. `SaveChanges` Gerekir yöntemi sorun bir `UPDATE` deyimi.
> - `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Gerekir yöntemi sorun bir `DELETE` deyimi.
> - `Detached`. Varlık veritabanı bağlamı tarafından izleniyor değil.
> 
> Bir masaüstü uygulaması durum değişiklikleri genellikle otomatik olarak ayarlanır. Uygulama Masaüstü tür, bir varlık okuyun ve bazı özellik değerleri değişiklikleri yapın. Bu şekilde otomatik olarak değiştirilmesi varlık durumuna neden `Modified`. Çağırdığınızda sonra `SaveChanges`, bir SQL Entity Framework oluşturur `UPDATE` yalnızca değiştirdiğiniz gerçek özelliklerini güncelleştirir deyimi.
> 
> Web uygulamaları bağlantısı kesilmiş yapısını sürekli bu sıra için izin vermez. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) okuyan bir sayfa oluşturulduğunda bir varlık atıldı. Zaman `HttpPost` `Edit` eylem yöntemi çağrıldığında, yeni bir istek yapıldığında ve yeni bir örneğini sahip [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), böylece varlık durumu el ile ayarlamak zorunda `Modified.` sonra çağırdığınızda `SaveChanges`, bağlam değiştirdiğiniz hangi özelliklerin kaybedeceğinizi öğrenmenin bir yolu bulunduğundan Entity Framework veritabanı satırın tüm sütunları güncelleştirir.
> 
> SQL istiyorsanız `Update` kullanıcının gerçekten değişen alanları güncelleştirmek için deyimi bunların ne zaman kullanılabilir olmasını sağlamak, özgün değerler (örneğin, gizli alanlar) herhangi bir yolla kaydedebilirsiniz `HttpPost` `Edit` yöntemi çağrılır. Oluşturabileceğiniz sonra bir `Student` çağrısı orijinal değerleri kullanarak varlık `Attach` varlık özgün sürümünün yöntemiyle varlığın değerleri için yeni değerleri güncelleştirmek ve ardından çağırın `SaveChanges.` daha fazla bilgi için bkz: [ Varlık durumları ve SaveChanges](https://msdn.microsoft.com/data/jj592676) ve [yerel veri](https://msdn.microsoft.com/data/jj592872) MSDN veri Geliştirici Merkezi'nde.


HTML ve Razor kodunda *Views\Student\Edit.cshtml* ne de gördüğünüz için benzer *Create.cshtml*, ve değişiklik gerekmez.

Sayfayı seçerek çalıştırın **Öğrenciler** sekmesini ve ardından bir **Düzenle** köprü.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Bazı tıklatın ve verileri değiştirme **kaydetmek**. Dizin Sayfası değiştirilen verileri bakın.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Delete sayfa güncelleştiriliyor

İçinde *Controllers\StudentController.cs*, şablon kodunu `HttpGet` `Delete` yöntemi kullanır `Find` seçili almak için yöntemini `Student` varlığı gördüğünüz içinde `Details` ve `Edit` yöntemleri. Ancak, uygulamak için bir özel hata iletisi ne zaman çağrısı `SaveChanges` başarısız olursa, karşılık gelen görebilir ve bu yöntem işlevselliğinin ekleyeceksiniz.

Güncelleştirmesi gördünüz ve işlemler oluşturmak gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak adlandırılan yöntemi onaylamak veya silme işlemini iptal etmek için bir fırsat kullanıcıya veren bir görünüm görüntüler. Kullanıcı onaylarsa, bir POST isteği oluşturulur. Bu durum oluştuğunda `HttpPost` `Delete` yöntemi çağrılır ve bu yöntem gerçekte silme işlemi gerçekleştirir.

Ekleyeceksiniz bir `try-catch` için engelleyin `HttpPost` `Delete` veritabanı güncelleştirildiğinde oluşabilecek tüm hataları işlemek için yöntem. Bir hata oluşursa, `HttpPost` `Delete` yöntem çağrılarını `HttpGet` `Delete` yöntemi, bir hata oluştuğunu gösteren bir parametre geçirme. `HttpGet Delete` Yöntemi ardından onay sayfasında kullanıcı iptal etme veya yeniden denemek için bir fırsat vermiş hata iletisi ile birlikte görüntüler.

1. Değiştir `HttpGet` `Delete` hata raporlama yönetir aşağıdaki kodla eylem yöntemi: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Bu kodu kabul eden bir [isteğe bağlı bir parametre](https://msdn.microsoft.com/library/dd264739.aspx) değişiklikleri kaydetmek için bir hatadan sonra yöntemi çağrıldı olup olmadığını gösterir. Bu parametre `false` zaman `HttpGet` `Delete` yöntemi, önceki bir hata çağrılır. Ne zaman çağırıldığında `HttpPost` `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası parametredir `true` ve bir hata iletisi görünümüne geçirilir.
2. Değiştir `HttpPost` `Delete` eylem yöntemi (adlı `DeleteConfirmed`) gerçek silme işlemi gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Seçilen varlığın bu kodu alır sonra çağırır [kaldırmak](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, bir SQL `DELETE` komutu oluşturulur. Ayrıca eylem yönteminin adı değiştirilmiştir `DeleteConfirmed` için `Delete`. Adlı kurulmuş kodu `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek için `HttpPost` yöntemi benzersiz bir imza. (CLR farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile takılıyor ve için aynı adı kullanmak `HttpPost` ve `HttpGet` yöntemlerini silin.

     Yüksek hacimli uygulama performansını iyileştirme bir öncelik ise, çağrı kod satırlarını değiştirerek satır almak için gerekli olmayan bir SQL sorgusu kaçının `Find` ve `Remove` yöntemleri ile aşağıdaki kodu:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Bu kod başlatır bir `Student` yalnızca birincil anahtar değerini kullanarak varlık ve varlık durumu ayarlar `Deleted`. Entity Framework varlığı silmek için gereken tüm budur.

     Belirtildiği gibi `HttpGet` `Delete` yöntemi verileri sil değil. Bir GET'e yanıt olarak bir silme işlemi isteği (veya herhangi bir düzenleme işlemi gerçekleştirme Bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturmak) bir güvenlik riski oluşturur. Daha fazla bilgi için bkz: [ASP.NET MVC ipucu #46 — güvenlik açıklarını oluşturduğundan bağlantılarını sil kullanmayan](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther'ın blogunda.
3. İçinde *Views\Student\Delete.cshtml*, hata iletisi arasında eklemek `h2` başlık ve `h3` , aşağıdaki örnekte gösterildiği gibi Başlık:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Sayfayı seçerek çalıştırın **Öğrenciler** sekmesi ve tıklayarak bir **silmek** köprü:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Tıklatın **silmek**. Dizin Sayfası silinen Öğrenci görüntülenir. (Hata işleyen kodu eyleminin içine bir örneğini göreceksiniz [eşzamanlılık Öğreticisi](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Kapatma veritabanı bağlantıları

Veritabanı bağlantıları kapatın ve mümkün olan en kısa sürede oldukları kaynakları boşaltmak için ile bittiğinde bağlam örneği siler. Diğer bir deyişle neden kurulmuş kodu sağlar bir [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) yöntemi sonunda `StudentController` sınıfını *StudentController.cs*, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Temel `Controller` sınıf zaten uygulayan `IDisposable` Bu kod yalnızca bir geçersiz kılma ekler için arabirim `Dispose(bool)` açıkça bağlam örneğinin dispose yöntemi.

<a id="transactions"></a>
## <a name="handling-transactions"></a>İşleme işlemleri

Varsayılan olarak Entity Framework örtük olarak işlemleri gerçekleştirir. Burada birden çok satır veya tablo için değişiklik ve ardından çağıran senaryolarda `SaveChanges`, Entity Framework otomatik olarak tüm değişikliklerinizi başarılı veya başarısız olmasına neden emin olur. Bazı değişiklikler ilk yapılır ve bir hata olur, bu değişiklikleri otomatik olarak geri alınır. Burada, daha fazla denetim--Örneğin, bir işlem içinde--Entity Framework dışında yapılan işlemleri dahil etmek istiyorsanız senaryolar görmek için [hareketlerle çalışma](https://msdn.microsoft.com/data/dn456843) MSDN'de.

## <a name="summary"></a>Özet

Basit CRUD işlemleri için sayfaları eksiksiz bir kümesini şimdi sahip `Student` varlıklar. MVC Yardımcıları veri alanları için kullanıcı Arabirimi öğeleri oluşturmak için kullanılır. MVC yardımcıları hakkında daha fazla bilgi için bkz: [bir Form kullanarak HTML Yardımcıları işleme](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (sayfa için MVC 3 ancak MVC 5 için hala geçerlidir.).

Sonraki öğreticide sıralama ve disk belleği ekleyerek dizin sayfası işlevselliğini genişletmek.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [sonraki](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
