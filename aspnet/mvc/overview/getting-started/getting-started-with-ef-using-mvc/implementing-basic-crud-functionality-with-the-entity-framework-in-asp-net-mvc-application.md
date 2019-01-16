---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Öğretici: ASP.NET MVC'de Entity Framework ile CRUD işlevselliği uygulama | Microsoft Docs"
description: Gözden geçirin ve oluşturma özelleştirme, okuyabilir, güncelleştirebilir, MVC yapı iskelesinde denetleyicileri ve görünümleri otomatik olarak oluşturur (CRUD) kodu silin.
author: tdykstra
ms.author: riande
ms.date: 01/11/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9c6f8f3a2ffc0a9c5e15111ae47c331dab24ff43
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341730"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Öğretici: ASP.NET MVC'de Entity Framework ile CRUD işlevselliği uygulama

İçinde [önceki öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), depolar ve Entity Framework (EF) 6 ve SQL Server LocalDB kullanarak verileri görüntüleyen bir MVC uygulaması oluşturuldu. Bu öğreticide, gözden geçirin ve oluşturma özelleştirme, okuyabilir, güncelleştirebilir, MVC yapı iskelesi otomatik olarak denetleyicileri ve görünümleri oluşturur (CRUD) kodu silin.

> [!NOTE]
> Denetleyicinizi ve veri erişim katmanı arasında bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit ve EF 6 kendisini kullanmayı eğitiminde odaklanmıştır tutmak için bunlar depoları kullanmayın. Depoları gerçekleştirme hakkında daha fazla bilgi için bkz. [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

Oluşturduğunuz web sayfaları örnekleri aşağıda verilmiştir:

![Öğrenci ayrıntıları sayfasının ekran görüntüsü.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Öğrenci görüntüsü oluşturma sayfası.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Ekran ot Öğrenci sayfayı silin.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ayrıntılar sayfası oluşturma
> * Güncelleştirme Oluştur sayfası
> * Güncelleştirme HttpPost düzenleme yöntemi
> * Silme sayfası
> * Kapat veritabanı bağlantıları
> * Tanıtıcı işlemleri

## <a name="prerequisites"></a>Önkoşullar

* [Entity Framework veri modeli oluşturma](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Ayrıntılar sayfası oluşturma

Öğrenciler için iskele kurulan kodu `Index` sol veya sayfa `Enrollments` özelliği, bunun nedeni, bu özellik bir koleksiyonu. İçinde `Details` sayfanın HTML tablosu halinde koleksiyonun içeriğini görüntüleyeceksiniz.

İçinde *Controllers\StudentController.cs*, eylem yöntemi için `Details` görüntülemek kullandığı [Bul](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) yönteminin tek bir almak için `Student` varlık.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Anahtar değeri yöntemine geçirilen `id` parametresi ve geldiği *verilerini yönlendirme* içinde **ayrıntıları** dizini sayfasında köprü.

### <a name="tip-route-data"></a>İpucu: **Rota verileri**

Rota verilerini, model Bağlayıcısı yönlendirme tablosunda belirtilen URL kesimi bulunan verilerdir. Örneğin, varsayılan yolu belirtir `controller`, `action`, ve `id` segmentleri:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Aşağıdaki URL'de varsayılan rotayı eşler `Instructor` olarak `controller`, `Index` olarak `action` ve 1 olarak `id`; bunlar rota veri değerlerdir.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` bir sorgu dizesi değeri var. Model bağlayıcıya geçirirseniz da çalışır `id` bir sorgu dizesi değeri olarak:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

URL'leri tarafından oluşturulan `ActionLink` Razor görünüm deyimlerinde. Aşağıdaki kodda, `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` için rota verilerini eklenir.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Aşağıdaki kodda, `courseID` sorgu dizesi olarak eklenir, böylece varsayılan yol, bir parametre ile eşleşmiyor.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Ayrıntılar sayfası oluşturmak için

1. Açık *Views\Student\Details.cshtml*.

   Her bir alan kullanarak görüntülenen bir `DisplayFor` Yardımcısı, aşağıdaki örnekte gösterildiği gibi:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Sonra `EnrollmentDate` alan ve kapatmadan önce hemen `</dl>` etiketinde, aşağıdaki örnekte gösterildiği gibi kayıtları bir listesini görüntülemek için vurgulanmış kodu ekleyin:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Kod girintileme yanlış ise, kod yapıştırıldıktan sonra basın **Ctrl**+**K**, **Ctrl**+**D** biçimlendirmek için .

    Bu kod, varlıklarda döngü `Enrollments` gezinme özelliği. Her `Enrollment` varlık özelliğine, kurs başlığı ve sınıf gösterir. Kurs başlığı alınır `Course` depolanan varlık `Course` gezinti özelliği `Enrollments` varlık. Tüm bu verileri alınır veritabanından otomatik olarak gerektiğinde. Diğer bir deyişle, yavaş yükleniyor burada kullanırsınız. Sizin belirtmediğiniz *istekli yükleme* için `Courses` kayıtları Öğrenciler var aynı sorguda alınamadı için gezinme özelliği. Bunun yerine, ilk kez denediğinizde erişim `Enrollments` gezinti özelliği, yeni bir sorgu gönderilir veritabanına verileri almak üzere. Daha fazla bilgi edinebilirsiniz yavaş yükleniyor ve istekli yükleme hakkında [ilgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.

3. Programını başlatarak Ayrıntılar sayfasını açın (**Ctrl**+**F5**), seçme **Öğrenciler** sekmesini ve ardından **Ayrıntıları** Alexander Carson bağlantısı. (Tuşuna basarsanız **Ctrl**+**F5** sırada *Details.cshtml* dosya açıksa, bir HTTP 400 hata alıyorsunuz. Visual Studio Ayrıntılar sayfası çalıştırmayı dener, ancak görüntülemek için Öğrenci belirten bir bağlantı üst sınırına değildi budur. Bu, "Öğrenci/Details" URL'den kaldırın ve yeniden deneyin veya tarayıcıyı kapatın, projeye sağ tıklayın ve tıklayın ortaya çıkarsa **görünümü** > **tarayıcıda görüntüle**.)

    Seçili Öğrenci için kursları ve notlarınızı listesini görürsünüz.

4. Tarayıcıyı kapatın.

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

1. İçinde *Controllers\StudentController.cs*, değiştirin <xref:System.Web.Mvc.HttpPostAttribute> `Create` eylem yöntemine aşağıdaki kodu. Bu kod ekler bir `try-catch` blok ve kaldırır `ID` gelen <xref:System.Web.Mvc.BindAttribute> özniteliği için iskele kurulmuş yöntemi:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Bu kod ekler `Student` için ASP.NET MVC model bağlayıcı tarafından oluşturulan varlık `Students` varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder. *Model bağlayıcı* yapar, bir form tarafından sunulan verilerle çalışmak daha kolaydır; dönüştürür gönderilen form değerleri CLR türlerine ve eylem yöntemi parametrelerine geçirir bir model bağlayıcı ASP.NET MVC işlevlerini gösterir. Bu durumda, model bağlayıcı örnekleyen bir `Student` varlık özelliği kullanılarak sizin için değerleri `Form` koleksiyonu.

    Kaldırılan `ID` bağlama özniteliği olduğundan `ID` satırın zaman eklenen SQL Server otomatik olarak ayarlayacak birincil anahtar değeri. Kullanıcı girişi ayarlı değil `ID` değeri.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Güvenlik Uyarısı - `ValidateAntiForgeryToken` özniteliği önlemeye yardımcı olur; [siteler arası istek sahteciliğini](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları. Karşılık gelen gerektirir `Html.AntiForgeryToken()` deyimi görünümünde, daha sonra göreceksiniz.

    `Bind` Özniteliktir karşı korumak için bir yol *aşırı yayınlayarak* senaryoları oluşturun. Örneğin, varsayalım `Student` varlığı içeren bir `Secret` ayarlamak için bu web sayfası istemediğiniz özelliği.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Olmasa bile bir `Secret` alan web sayfasında, bir bilgisayar korsanının kullanabileceğiniz bir aracı gibi [fiddler](http://fiddler2.com/home), veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri. Olmadan <xref:System.Web.Mvc.BindAttribute> oluştururken, model Bağlayıcısı kullandığını alanları sınırlama özniteliği bir `Student` örneği<em>,</em> model bağlayıcı ayarladıysanız seçiyordu `Secret` form değeri ve oluşturmak için bunu kullanın `Student` Varlık örneği. Sonra ne olursa olsun değer için belirtilen korsanın `Secret` form alanı veritabanınızda güncelleştirilmesi. Fiddler'ı aşağıdaki resimde gösterilmektedir aracı ekleme `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    "OverPost" sonra başarıyla eklenmesi için değer `Secret` özelliği eklenen hiçbir zaman web sayfasının bu özelliği ayarlayamaz yönelik olsa da, satır.

    Kullanmak en iyisidir `Include` parametresiyle `Bind` özniteliğini *beyaz liste* alanları. Kullanmak da mümkündür `Exclude` parametresi *kara liste* hariç tutmak istediğiniz alanları. Nedeni `Include` daha güvenlidir, varlık üzerinde yeni bir özellik eklediğinizde, yeni alan otomatik olarak tarafından korunmayan emin olan bir `Exclude` listesi.

    Engel olabilir overposting düzenleme senaryolarda olan varlığı ilk veritabanından okuma ve ardından arama `TryUpdateModel`, geçen bir açık izin verilen özellikler listesinde. Aşağıdaki öğreticilerde kullanılan yöntem olmasıdır.

    Çok sayıda geliştirici tarafından tercih edilen overposting önlemek için alternatif bir yolu varlık sınıfları yerine görünüm modelleri model bağlamayla kullanmaktır. Yalnızca görünüm modelinde güncelleştirmek istediğiniz özellikleri içerir. MVC model bağlayıcı tamamlandıktan sonra Görünüm modeli özellikleri isteğe bağlı olarak bir aracı gibi kullanarak varlık örneği için Kopyala [AutoMapper](http://automapper.org/). DB kullanın. Giriş olarak durumuna ayarlanır ve ardından Property("PropertyName") varlık örneği üzerinde. IsModified görünümü modele dahil edilen her bir varlık özellik üzerinde true. Bu yöntem çalışır hem de düzenleyin ve senaryolar oluşturun.

    Dışındaki `Bind` özniteliği `try-catch` iskele kurulan kodu için yaptığınız tek değişiklik bloğudur. Türetilen bir özel durum, <xref:System.Data.DataException> olan değişiklikleri kaydedilirken yakalandı, genel bir hata iletisi görüntülenir. <xref:System.Data.DataException> Kullanıcı yeniden denemeniz önerilir özel durum bazen bir programlama hatası yerine bir uygulama için dış bir şey tarafından kaynaklanır. Bu örnekte uygulanmadı olsa da, üretim kalitesinde uygulaması özel durumu günlüğe kaydedersiniz. Daha fazla bilgi için **ilgili ayrıntılı bilgi için günlük** konusundaki [izleme ve Telemetri (gerçek hayatta kullanılan bulut uygulamaları Azure ile oluşturma)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kodda *Views\Student\Create.cshtml* ne gördüğünüz üzere benzer *Details.cshtml*dışında `EditorFor` ve `ValidationMessageFor` Yardımcıları yerineherbiralaniçinkullanılan`DisplayFor`. İlgili kod aşağıdaki gibidir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* de içerir `@Html.AntiForgeryToken()`, çalıştığı ile `ValidateAntiForgeryToken` önlemeye yardımcı olmak için denetleyici özniteliğinde [siteler arası istek sahteciliğini](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) saldırıları.

    Hiçbir değişiklik gerekli *Create.cshtml*.

2. Programını başlatarak çalıştırırsanız seçerek **Öğrenciler** sekmesini ve ardından **Yeni Oluştur**.

3. Adları ve geçersiz bir tarih girin ve tıklayın **Oluştur** hata iletisini görmek için.

    Varsayılan olarak aldığınız sunucu tarafı doğrulama budur. Bir sonraki öğreticide, istemci tarafı doğrulama kodunu oluşturmak öznitelikleri ekleme görürsünüz. Model doğrulama denetimi ile aşağıdaki vurgulanmış kodu gösterir **Oluştur** yöntemi.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Tarihi geçerli bir değere değiştirip'ı **Oluştur** görünen yeni Öğrenci görmek için **dizin** sayfası.

5. Tarayıcıyı kapatın.

## <a name="update-httppost-edit-method"></a>Güncelleştirme HttpPost Düzenle yöntemi

1. Değiştirin <xref:System.Web.Mvc.HttpPostAttribute> `Edit` eylem yöntemini aşağıdaki kod ile:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > İçinde *Controllers\StudentController.cs*, `HttpGet Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `Find` seçilen alınacak yöntemi `Student` varlığı gördüğünüz içinde `Details`yöntemi. Bu yöntem değiştirmeniz gerekmez.

   Bu değişiklikleri önlemek için bir güvenlik en iyi yöntemi uygulamak [overposting](#overpost), oluşturulan iskele kurucu bir `Bind` özniteliğini ve bir değiştirildi bayrağı ile varlık için model bağlayıcı tarafından oluşturulan varlığı eklendi. Kod nedeniyle artık önerilir `Bind` özniteliği temizler içinde listelenmeyen alanlar önceden mevcut olan tüm verileri dışarı `Include` parametresi. MVC denetleyicisi iskele kurucu, böylece bu oluşturmaz gelecekte güncelleştirilecek `Bind` düzenleme metotlarını için öznitelikler.

   Yeni kod çağrıları ve var olan bir varlığa okur <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> alanları kullanıcı girişi gönderilen form verileri olarak güncelleştirilecek. Entity Framework'ün otomatik değişiklik kümeleri izleme [EntityState.Modified](<xref:System.Data.EntityState.Modified>) varlık bayrağı. Zaman [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi çağrıldığında <xref:System.Data.EntityState.Modified> bayrağı veritabanı satırı güncelleştirmek için SQL deyimlerini oluşturmak Entity Framework neden olur. [Eşzamanlılık çakışmalarını](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) göz ardı edilir ve kullanıcı değişmedi olanlar dahil olmak üzere veritabanı satırdaki tüm sütunlar güncelleştirilir. (Bir sonraki öğreticide eşzamanlılık çakışmalarını nasıl ele alınacağını gösterir ve veritabanında güncelleştirilmesi için tek tek alanları yalnızca istiyorsanız, varlık ayarlayabilirsiniz [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) ve ayrı ayrı alanlar olarak [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Overposting önlemek için düzenleme sayfası tarafından güncelleştirilebilmesi için istediğiniz alanları, güvenilir listededir `TryUpdateModel` parametreleri. Şu anda koruduğunuz hiçbir ek alanlar vardır, ancak bağlamak için model bağlayıcı istediğiniz alanları listesi alanları veri modeli gelecekte eklerseniz, siz açıkça burada ekleyinceye kadar otomatik olarak korumalı olup olmadıklarını olduğunu sağlar.

   Bu değişikliklerin bir sonucu olarak HttpPost düzenleme yöntemin yöntem imzası HttpGet düzenleme yöntemi ile aynıdır. Bu nedenle EditPost yöntemi değiştirdiniz.

   > [!TIP]
   >
   > **Varlık durumları ve ekleme ve SaveChanges yöntemleri**
   >
   > Veritabanı bağlamı varlıkları bellekte veritabanında ilgili satırlarında ile eşitlenmiş, bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, yeni bir varlık gönderdiğinizde [Ekle](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) varlığın durumu ayarlanmış yöntemi `Added`. Ardından çağırdığınızda [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi, veritabanı bağlamı veren bir SQL `INSERT` komutu.
   >
   > Bir varlık aşağıdakilerden biri olabilir [durumları](xref:System.Data.EntityState):
   >
   > - `Added`. Varlık henüz veritabanında mevcut değil. `SaveChanges` Gereken yöntemini sorun bir `INSERT` deyimi.
   > - `Unchanged`. Hiçbir şey bu varlıkla tarafından yapılması gereken `SaveChanges` yöntemi. Bir varlık veritabanından okurken, varlık bu durumu ile başlar.
   > - `Modified`. Bazıları veya tümü varlığın özellik değerleri değiştirilmiş. `SaveChanges` Gereken yöntemini sorun bir `UPDATE` deyimi.
   > - `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Gereken yöntemini sorun bir `DELETE` deyimi.
   > - `Detached`. Varlık veritabanı bağlamı izleniyor değil.
   >
   > Bir masaüstü uygulamasında, durum değişikliklerini genellikle otomatik olarak ayarlanır. Bir masaüstü uygulaması türünde, bir varlık okuyun ve değişiklik bazı özellik değerleri. Bu varlık durumunu otomatik olarak değiştirilecek şekilde neden `Modified`. Ardından çağırdığınızda `SaveChanges`, Entity Framework SQL oluşturur `UPDATE` deyimi, değiştirdiğiniz gerçek özelliklerini güncelleştirir.
   >
   > Bağlantısı kesilmiş yapısı web Apps sürekli bu sıra için izin vermez. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) okuyan bir varlık, bir sayfa oluşturulduğunda bırakıldı. Zaman `HttpPost` `Edit` eylem yöntemi çağrıldığında, yeni bir istekte bulunulduğunda ve yeni bir örneğine sahip [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), varlık durumu el ile ayarlamak zorunda `Modified.` sonra çağırdığınızda `SaveChanges`, bağlamı değiştirmiş hangi özellikleri bilmek bir yolu yoktur çünkü Entity Framework, veritabanı satırdaki tüm sütunlar güncelleştirir.
   >
   > SQL istiyorsanız `Update` kullanıcının gerçekten değişen alanları güncelleştirmek için deyimi bunların ne zaman kullanılabilir olacak şekilde bir şekilde (örneğin, gizli alanlar) özgün değerlerine kaydedebilirsiniz `HttpPost` `Edit` yöntemi çağrılır. Oluşturabileceğiniz daha sonra bir `Student` orijinal değerleri, çağrı kullanarak varlık `Attach` yöntemi, özgün varlık sürümü ile yeni değerlere varlığın değerlerini güncelleştirin ve ardından çağırmak `SaveChanges.` daha fazla bilgi için [ Varlık durumları ve SaveChanges](/ef/ef6/saving/change-tracking/entity-state) ve [yerel veri](/ef/ef6/querying/local-data).

   HTML ve Razor kodunda *Views\Student\Edit.cshtml* ne gördüğünüz üzere benzer *Create.cshtml*, ve değişiklik gerekmez.

2. Programını başlatarak çalıştırırsanız seçerek **Öğrenciler** sekmesini ve ardından bir **Düzenle** köprü.

3. Bazı tıklayın ve veri değiştirme **Kaydet**. Dizin sayfasında değiştirilen verileri görürsünüz.

4. Tarayıcıyı kapatın.

## <a name="update-the-delete-page"></a>Silme sayfası

İçinde *Controllers\StudentController.cs*, şablon kodunu <xref:System.Web.Mvc.HttpGetAttribute> `Delete` yöntemi kullanan `Find` seçilen alınacak yöntemi `Student` varlığı gördüğünüz içinde `Details` ve `Edit` yöntemleri. Ancak, uygulamak için bir özel hata iletisi zaman çağrısı `SaveChanges` başarısız olursa, bazı işlevler, karşılık gelen görebilir ve bu yöntem ekleyeceksiniz.

Güncelleştirme için gördüğünüz ve oluşturma işlemleri gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak çağrılan yöntem kullanıcı onaylamak veya silme işlemi iptal etmek için bir şans verir bir görünüm görüntüler. Kullanıcıyı onaylarsa, bir POST isteği oluşturulur. Bu durum oluştuğunda `HttpPost` `Delete` yöntemi çağrılır ve bu yöntem gerçekten silme işlemini gerçekleştirir.

Ekleyeceğiniz bir `try-catch` bloğunu <xref:System.Web.Mvc.HttpPostAttribute> `Delete` veritabanı güncelleştirildiğinde oluşabilecek hataları işlemek için yöntemi. Bir hata oluşursa <xref:System.Web.Mvc.HttpPostAttribute> `Delete` yöntem çağrılarını <xref:System.Web.Mvc.HttpGetAttribute> `Delete` yöntemi, bir hata oluştuğunu belirten bir parametre geçirerek. <xref:System.Web.Mvc.HttpGetAttribute> `Delete` Yöntemi sonra onay sayfasında kullanıcı yeniden deneyin veya iptal etme fırsatı veren hata iletisi ile birlikte görüntüler.

1. Değiştirin <xref:System.Web.Mvc.HttpGetAttribute> `Delete` eylem yöntemini aşağıdaki kodla hata raporlama'yı yöneten:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Bu kodu kabul eden bir [isteğe bağlı parametre](https://msdn.microsoft.com/library/dd264739.aspx) değişiklikleri kaydetmek için bir hatadan sonra yöntemi çağrıldı olup olmadığını gösterir. Bu parametre `false` olduğunda `HttpGet` `Delete` önceki bir hata yöntemi çağrılır. Ne zaman çağırıldığında `HttpPost` `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası parametredir `true` ve bir hata iletisi görünüme iletilir.

2. Değiştirin <xref:System.Web.Mvc.HttpPostAttribute> `Delete` eylem yöntemine (adlı `DeleteConfirmed`) gerçek silme işlemini gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Bu kod, seçili varlığı alır. ardından çağırır [Kaldır](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, bir SQL `DELETE` komut oluşturulur. Eylem yöntemi adından de değiştirdiğiniz `DeleteConfirmed` için `Delete`. İskele kurulan kodu adlı `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek `HttpPost` yöntemi benzersiz bir imza. (CLR'nin farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile devam edin ve aynı adı kullanın `HttpPost` ve `HttpGet` yöntemlerini silin.

    Yüksek hacimli uygulama performansını iyileştirme bir önceliktir çağıran kod satırı sayısını değiştirerek bir satır almak için gerekli olmayan bir SQL sorgusu kaçının `Find` ve `Remove` yöntemleri aşağıdaki kod ile:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Bu kod oluşturur bir `Student` varlık yalnızca birincil anahtar değerini kullanarak ve ardından varlık durumu ayarlar `Deleted`. Entity Framework varlığı silmek için gereken tüm budur.

    Belirtildiği gibi `HttpGet` `Delete` yöntemi verileri silme değil. Bir GET'e yanıt olarak bir silme işlemi gerçekleştirme isteği (veya herhangi bir düzenleme işlemini gerçekleştirirken bu konular için işlem veya veriler değiştiğinde başka bir işlem oluşturun) bir güvenlik riski oluşturur. Daha fazla bilgi için bkz. [ASP.NET MVC ipucu #46; bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther'ın blogunda.

3. İçinde *Views\Student\Delete.cshtml*, bir hata iletisi arasında ekleme `h2` başlık ve `h3` aşağıdaki örnekte gösterildiği gibi başlığı:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Programını başlatarak çalıştırırsanız seçerek **Öğrenciler** sekmesini ve ardından bir **Sil** köprü.

5. Seçin **Sil** bildiren sayfasında **bu silmek istediğinizden emin misiniz?**.

    Dizin Sayfası silinen Öğrenci görüntüler. (Bir örneğini bir hata işleme kodunu nasıl gerçekleştirildiğini göreceksiniz [eşzamanlılık öğretici](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Kapat veritabanı bağlantıları

Veritabanı bağlantıları kapatın ve olabildiğince çabuk oldukları kaynakları boşaltmak için bağlam örneği ile işiniz bittiğinde atın. Diğer bir deyişle iskele kurulan kodu neden sağlar bir [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) sonunda yöntemi `StudentController` sınıfını *StudentController.cs*aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Temel `Controller` sınıf zaten uygular `IDisposable` Bu kod yalnızca bir geçersiz kılma ekler için arabirim `Dispose(bool)` bağlam örneğinin açıkça atmayı yöntemi.

## <a name="handle-transactions"></a>Tanıtıcı işlemleri

Varsayılan olarak Entity Framework, örtük olarak işlemler uygular. Burada birden çok satır veya tablo için değişiklik ve sonra çağrı senaryolarda `SaveChanges`, Entity Framework otomatik olarak tüm değişikliklerinizi başarılı veya başarısız tüm emin olur. Bazı değişiklikler önce yapılır ve ardından bir hata olur, bu değişiklikleri otomatik olarak geri alınır. Daha denetlediğiniz senaryoları için&mdash;Entity Framework dışında bir işlemde yapılan işlemler dahil etmek istiyorsanız, örneğin&mdash;bkz [işlemleri çalışma](/ef/ef6/saving/transactions).

## <a name="additional-resources"></a>Ek kaynaklar

Artık sahip olduğunuz için basit CRUD işlemleri gerçekleştiren sayfalar eksiksiz bir kümesini `Student` varlıklar. MVC Yardımcıları veri alanları için kullanıcı Arabirimi öğeleri oluşturmak için kullanılır. MVC yardımcıları hakkında daha fazla bilgi için bkz. [formu kullanarak bir HTML Yardımcıları oluşturma](/previous-versions/aspnet/dd410596(v=vs.98)) (MVC 3 ancak hala MVC 5 için ilgili makaleyi belirtilir).

EF 6 diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ayrıntılar sayfası oluşturuldu
> * Oluştur sayfası güncelleştirildi
> * HttpPost düzenleme yöntemi güncelleştirildi
> * Silme sayfası güncelleştirildi
> * Kapalı veritabanı bağlantıları
> * İşlenmiş işlemleri

Sıralama, filtreleme ve sayfalama projeye ekleme hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Sıralama, Filtreleme ve Sayfalama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)