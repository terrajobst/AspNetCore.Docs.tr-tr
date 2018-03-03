---
title: "EF çekirdek - CRUD - 2, 10 ile ASP.NET Core MVC"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/crud
ms.openlocfilehash: a586fdde07ecf349d7523d43a623501af62257a2
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Oluşturma, okuma, güncelleştirme ve silme - EF çekirdek ASP.NET Core MVC Öğreticisi (2 / 10)

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide depolayan ve SQL Server yerel veritabanı ve Entity Framework kullanarak verileri görüntüleyen bir MVC uygulaması oluşturuldu. Bu öğreticide, gözden geçirmenizi ve CRUD özelleştirme (oluşturma, okuma, güncelleştirme, silme) MVC yapı iskelesi otomatik olarak denetleyicileri ve görünümlerde oluşturur kodu.

> [!NOTE] 
> Denetleyicinizi ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit ve Entity Framework kullanmayı öğretmek odaklanmıştır tutmak için bunlar depoları kullanmayın. Depoları EF ile ilgili daha fazla bilgi için bkz: [bu serideki son Öğreticisi](advanced.md).

Bu öğreticide, aşağıdaki web sayfalarının çalışması:

![Öğrenci Ayrıntıları sayfası](crud/_static/student-details.png)

![Öğrenci Oluştur sayfası](crud/_static/student-create.png)

![Öğrenci Düzenle sayfası](crud/_static/student-edit.png)

![Öğrenci Sil sayfası](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

Öğrenciler dizin sayfası için kurulmuş kod sol `Enrollments` özelliği, bunun nedeni, bu özelliği bir koleksiyon. İçinde **ayrıntıları** sayfasında, bir HTML tablosunda koleksiyonunun içeriğini görüntülersiniz.

İçinde *Controllers/StudentsController.cs*, Ayrıntılar için eylem yöntemini kullanır görüntüleyin `SingleOrDefaultAsync` tek bir alma yöntemi `Student` varlık. Çağıran kodu eklemek `Include`. `ThenInclude`, ve `AsNoTracking` vurgulanan aşağıdaki kodda gösterildiği gibi yöntemleri.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` Ve `ThenInclude` yöntemleri neden yüklemek bağlam `Student.Enrollments` gezinti özelliği ve her kayıt içinde `Enrollment.Course` gezinti özelliği.  Bu yöntemleri hakkında daha fazla bilgi edineceksiniz [ilgili verileri okuma](read-related-data.md) Öğreticisi.

`AsNoTracking` Yöntemi burada döndürülen varlıklar olmaz güncelleştirilmesi geçerli bağlamın yaşam süresi senaryolarda performansı artırır. Daha fazla hakkında bilgi edineceksiniz `AsNoTracking` Bu öğreticinin sonunda.

### <a name="route-data"></a>Rota verileri

Geçirilen anahtar değeri `Details` yöntemi gelir *rota veri*. Rota verilerini, model bağlayıcı URL kesimdeki bulunan verilerdir. Örneğin, denetleyici, eylem ve kimliği kesimleri varsayılan yolu belirtir:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Aşağıdaki URL'de denetleyici, eylem olarak dizin ve kimlik olarak 1 olarak Eğitmen varsayılan rotayı eşler; Rota veri değerleri şunlardır.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL son parçası ("? courseID 2021 =") bir sorgu dizesi değeridir. Model bağlayıcı Ayrıca kimliği değerine geçer `Details` yöntemi `id` sorgu dize değeri geçirirseniz parametre:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Dizin sayfasında köprü URL'leri Razor görünümünde etiket Yardımcısı deyimleri tarafından oluşturulur. Aşağıdaki Razor kodunda `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` rota verilerini eklenir.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Bu aşağıdaki HTML oluşturur, `item.ID` 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

Aşağıdaki Razor kodunda `studentID` sorgu dizesi olarak eklenmiş şekilde varsayılan yol parametresinde eşleşmiyor.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Bu aşağıdaki HTML oluşturur, `item.ID` 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Etiket yardımcıları hakkında daha fazla bilgi için bkz: [etiket Yardımcıları ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Ayrıntılar görünümü kayıtları ekleyin

Açık *Views/Students/Details.cshtml*. Her bir alan kullanılarak görüntülenen `DisplayNameFor` ve `DisplayFor` aşağıdaki örnekte gösterildiği gibi Yardımcıları:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Son alan sonra ve hemen kapatmadan önce `</dl>` etiketi, kayıtları listesini görüntülemek için aşağıdaki kodu ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Kod yapıştırıldıktan sonra kod girintileme yanlışsa, düzeltmek için CTRL-K-D tuşuna basın.

Bu kod, varlıklarda döngü `Enrollments` gezinti özelliği. Her kayıt için indirmelere başlık ve sınıf görüntüler. İndirmelere başlık depolanan indirmelere varlık alınır `Course` kayıtları varlık gezinti özelliği.

Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** Öğrenci için bağlantı. Kurslar ve dereceleri listesi için seçilen Öğrenci bakın:

![Öğrenci Ayrıntıları sayfası](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

İçinde *StudentsController.cs*, HttpPost değiştirmek `Create` try-catch bloğu ekleyerek ve Kimliğinden kaldırma yöntemi `Bind` özniteliği.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Bu kod, ASP.NET MVC model bağlayıcı Öğrenciler varlığa tarafından oluşturulan Öğrenci varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder ekler. (Bir form tarafından sunulan verilerle çalışmayı kolaylaştırır ASP.NET MVC işlevlerini model bağlayıcı başvurduğu; bir model bağlayıcı gönderilen form değerleri CLR türlerine dönüştürür ve eylem yönteminin parametrelerini geçirir. Bu durumda, model bağlayıcı Öğrenci varlık, Form koleksiyonu özellik değerleri kullanarak başlatır.)

Kaldırılan `ID` gelen `Bind` kimliği satır zaman eklenen SQL Server otomatik olarak ayarlayacak birincil anahtar değeri olduğundan özniteliği. Kullanıcı girişi kimliği değeri ayarlamaz.

Dışında `Bind` , try-catch bloğu bir özniteliktir kurulmuş kodu yapılan tek değişiklik. Türetilen bir özel durum, `DbUpdateException` olan değişiklikler kaydedilirken yakalandı, genel bir hata iletisi görüntülenir. `DbUpdateException` Kullanıcı yeniden denemek için tavsiye edilir şekilde özel durumlar bazen bir programlama hatası yerine uygulama için dış bir şey tarafından hatalardır. Bu örnekte uygulanmadı karşın, bir üretim kalitesi uygulama özel durum oturum açabilirsiniz. Daha fazla bilgi için bkz: **hakkında bilgi için günlük** bölümüne [izleme ve Telemetri (yapı gerçek bulut uygulamaları Azure ile)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

`ValidateAntiForgeryToken` Öznitelik, siteler arası istek sahtekarlığı (CSRF) saldırılarını önlemeye yardımcı olur. Belirteç otomatik olarak görünüm tarafından içine eklenen [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) ve kullanıcı tarafından form gönderildiğinde yer alır. Belirteç tarafından doğrulandığından `ValidateAntiForgeryToken` özniteliği. CSRF hakkında daha fazla bilgi için bkz: [karşı istek Sahteciliğine](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Güvenlik Notu overposting hakkında

`Bind` Kurulmuş kodu içerir özniteliği `Create` yöntemdir overposting karşı korumak için bir yol senaryoları oluşturun. Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` ayarlamak için bu web sayfası istemediğiniz özelliği.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Yüklü olmasa bile, bir `Secret` alan web sayfasında, bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri. Olmadan `Bind` Öğrenci örneği, model bağlayıcı oluşturduğunda, model Bağlayıcısı kullandığını alanları sınırlama özniteliği ayarladıysanız çekme `Secret` form değeri ve Öğrenci varlık örneği oluşturmak için kullanabilirsiniz. Sonra ne olursa olsun değer için belirtilen korsan `Secret` form alanı veritabanınızda güncelleştirilmesi. Fiddler aracı ekleme aşağıdaki görüntüde gösterir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

![Fiddler ekleme gizli alan](crud/_static/fiddler.png)

"OverPost" sonra başarıyla eklenmesi için değer `Secret` özelliği eklenen web sayfası bu özelliği ayarlayamaz hiçbir zaman amaçlı rağmen satır.

Varlık veritabanından ilk okuma ve ardından çağırma overposting düzenleme senaryolarda engelleyebilirsiniz `TryUpdateModel`, açıkça izin verilen özellikler listesinde geçen. Bu öğreticiler kullanılan yöntem olmasıdır.

Birçok geliştiriciler tarafından tercih edilen overposting önlemek için alternatif bir yol sınıflar yerine modelleri görüntüle model bağlamayla kullanmaktır. Yalnızca görünüm modeli güncelleştirmek istediğiniz özellikleri içerir. MVC model bağlayıcı tamamladığında, görünüm model özellikleri isteğe bağlı olarak AutoMapper gibi bir araç kullanarak varlık örneği kopyalayın. Kullanım `_context.Entry` durumunu ayarlamak için varlık örneğinde `Unchanged`ve ardından `Property("PropertyName").IsModified` görünüm modele dahil her varlık özellikte true. Bu yöntem works hem de düzenleyebilir ve senaryoları oluşturabilir.

### <a name="test-the-create-page"></a>Test oluşturma sayfası

Kodda *Views/Students/Create.cshtml* kullanan `label`, `input`, ve `span` (için doğrulama ileti) etiket Yardımcıları her bir alan için.

Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **Yeni Oluştur**.

Adları ve bir tarih girin. Tarayıcınız, yapmanıza olanak sağlayan, geçersiz bir tarih girmeyi deneyin. (Bazı tarayıcılar tarih seçiciyi kullanın zorlayabilir.) Ardından **oluşturma** hata iletisini görmek için.

![Tarih doğrulama hatası](crud/_static/date-error.png)

Varsayılan olarak aldığınız sunucu tarafında doğrulama budur; sonraki öğreticide istemci tarafı doğrulama kodunu da oluşturur özniteliklerini ekleme görürsünüz. Model doğrulama denetimi ile aşağıdaki vurgulanmış kodu gösterir `Create` yöntemi.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Tarih geçerli bir değere değiştirip'ı **oluşturma** görünür yeni Öğrenci görmek için **dizin** sayfası.

## <a name="update-the-edit-page"></a>Güncelleştirmeyi Düzenle sayfası

İçinde *StudentController.cs*, HttpGet `Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `SingleOrDefaultAsync` de anlatıldığı gibi seçilen Öğrenci varlık alma yöntemi `Details` yöntemi. Bu yöntem değiştirmeniz gerekmez.

### <a name="recommended-httppost-edit-code-read-and-update"></a>HttpPost düzenleme kod önerilir: Okuma ve güncelleştirme

HttpPost düzenleme eylem yöntemini aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Bu değişiklikler overposting önlemek için bir en iyi güvenlik uygulaması uygulayın. Oluşturulan iskele kurucu bir `Bind` özniteliği ve varlık ile kümesi için model bağlayıcı tarafından oluşturulan varlık eklenen bir `Modified` bayrağı. Kod için birçok senaryoları için önerilmez `Bind` özniteliği temizler listelenmez alanları önceden mevcut verileri dışarı `Include` parametresi.

Varolan bir varlık ve çağrıları yeni kod okur `TryUpdateModel` alınan varlık alanlarını güncelleştirmek için [gönderilen form verilerini uygulamasında kullanıcı girdisi göre](xref:mvc/models/model-binding#how-model-binding-works). Entity Framework'ün otomatik değişiklik kümelerini izleme `Modified` form girişi tarafından değiştirilen alanları bayrağı. Zaman `SaveChanges` yöntemi çağrıldığında, Entity Framework veritabanı satırı güncelleştirmek için SQL deyimleri oluşturur. Eşzamanlılık çakışması göz ardı edilir ve yalnızca kullanıcı tarafından güncelleştirildi tablo sütunları veritabanında güncelleştirilir. (Bir sonraki öğretici eşzamanlılık çakışmaları nasıl ele alınacağını gösterir.)

Overposting, tarafından güncelleştirilebilmesi için istediğiniz alanları önlemek için en iyi uygulama olarak **Düzenle** sayfa içinde Güvenilenler listesine olan `TryUpdateModel` parametreleri. (Parametre listesinde bir alanlar listesi önceki boş dize form alan adları ile kullanmak bir önek içindir.) Şu anda koruduğunuz hiçbir ek alanlar vardır, ancak veri modeline gelecekte alanlar eklerseniz, açıkça bunları burada yüklemediğiniz sürece otomatik olarak korumalı olup olmadıklarını olduğunu bağlamak için model bağlayıcı istediğiniz alanları listesi sağlar.

Bu değişiklikler, HttpPost yöntemi imzası sonucunda `Edit` yöntemi aynıdır HttpGet `Edit` yöntemi; bu nedenle yöntemi değiştirdiniz `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternatif HttpPost düzenleme kod: oluşturun ve ekleme

Önerilen HttpPost düzenleme kod yalnızca değiştirilen sütun güncelleştirilmesi ve model bağlama için dahil istemediğiniz özellikleri verilerini korur sağlar. Ancak, ek veritabanı okuma ve eşzamanlılık çakışmalarını işleme için daha karmaşık kod sonuçlanabilir okuma ilk yaklaşım gerektirir. EF bağlamı için model bağlayıcı tarafından oluşturulan bir varlık eklemek ve değiştirilmiş olarak işaretlemek için kullanılan bir alternatiftir. (Projenizin güncelleştirmemeniz şu kodla yalnızca isteğe bağlı bir yaklaşım göstermek için gösterilen.) 

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Web sayfası kullanıcı Arabirimi tüm alanlar varlıkta içerir ve bunlardan herhangi birinin güncelleştirebilirsiniz, bu yaklaşım kullanabilirsiniz.

İskele kurulmuş kod oluşturma-ve-ekleme yaklaşımı kullanır, ancak yalnızca yakalar `DbUpdateConcurrencyException` 404 hata kodları özel durumlar ve döndürür.  Gösterilen örnek veritabanı güncelleştirme özel durumları yakalar ve bir hata iletisi görüntüler.

### <a name="entity-states"></a>Varlık durumları

Veritabanı bağlamı varlıkları bellekte kendi veritabanında karşılık gelen satır ile eşitleme ve bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, geçirdiğiniz zaman yeni bir varlık `Add` varlığın durumu kümesine yöntemi, `Added`. Çağırdığınızda sonra `SaveChanges` yöntemi, veritabanı bağlamı sorunları SQL INSERT komutu.

Bir varlık şu durumlardan birinde olabilir:

* `Added`. Varlık henüz veritabanında yok. `SaveChanges` Yöntemi INSERT deyimi verir.

* `Unchanged`. Hiçbir şey bu varlık tarafından ile yapılması gerektiğini `SaveChanges` yöntemi. Veritabanından bir varlık okurken varlık bu durumu ile başlar.

* `Modified`. Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş. `SaveChanges` Yöntemi bir güncelleştirme deyimi sorunları.

* `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Yöntemi DELETE deyimi verir.

* `Detached`. Varlık veritabanı bağlamı tarafından izleniyor değil.

Bir masaüstü uygulaması durum değişiklikleri genellikle otomatik olarak ayarlanır. Bir varlık okuma ve bazı özellik değerleri değişiklikleri yapın. Bu şekilde otomatik olarak değiştirilmesi varlık durumuna neden `Modified`. Çağırdığınızda sonra `SaveChanges`, Entity Framework yalnızca değiştirdiğiniz gerçek özelliklerini güncelleştirir SQL UPDATE deyimi oluşturur.

Bir web uygulamasında `DbContext` başlangıçta okuyan bir varlık ve düzenlenmesi için verileri bir sayfa oluşturulduğunda atıldı görüntüler. Zaman HttpPost `Edit` eylem yöntemi çağrıldığında, yeni bir web istek yapıldığında ve yeni bir örneğini sahip `DbContext`. Bu yeni bağlam varlıkta yeniden okuyun, Masaüstü işleme benzetimi.

Ancak yapmak istemiyorsanız, ek okuma işlemi, model bağlayıcı tarafından oluşturulan varlığı nesne kullanmak zorunda.  Bunu yapmanın en kolay yolu varlık durumu değiştirme için daha önce gösterilen alternatif HttpPost düzenleme kodda yapıldığı şekilde ayarlamaktır. Çağırdığınızda sonra `SaveChanges`, bağlam değiştirdiğiniz hangi özelliklerin kaybedeceğinizi öğrenmenin bir yolu bulunduğundan Entity Framework veritabanı satırın tüm sütunları güncelleştirir.

Okuma ilk yaklaşım önlemek istiyorsanız, ancak aynı zamanda kullanıcının gerçekten değiştirilen alanları güncelleştirmek için SQL UPDATE deyimini istediğiniz, daha karmaşık bir kodudur. Herhangi bir yolla özgün değerler kaydetmek zorunda (gibi gizli alanları kullanarak) bunların ne zaman kullanılabilir olduğunu HttpPost `Edit` yöntemi çağrılır. Çağrı orijinal değerleri kullanarak bir öğrenci varlık oluşturup `Attach` varlık özgün sürümünün yöntemiyle varlığın değerleri için yeni değerleri güncelleştirmek ve ardından çağırın `SaveChanges`.

### <a name="test-the-edit-page"></a>Sınama Düzenle sayfası

Uygulama, belirleyin **Öğrenciler** sekmesini ve ardından bir **Düzenle** köprü.

![Öğrenciler Düzenle sayfası](crud/_static/student-edit.png)

Bazı tıklatın ve verileri değiştirme **kaydetmek**. **Dizin** sayfası açılır ve değiştirilen verileri bakın.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

İçinde *StudentController.cs*, HttpGet için şablonu kodu `Delete` yöntemi kullanan `SingleOrDefaultAsync` yöntemleri Ayrıntılar ve düzenleme anlatıldığı gibi seçilen Öğrenci varlık alma yöntemi. Ancak, uygulamak için bir özel hata iletisi ne zaman çağrısı `SaveChanges` başarısız olursa, karşılık gelen görebilir ve bu yöntem işlevselliğinin ekleyeceksiniz.

Güncelleştirmesi gördünüz ve işlemler oluşturmak gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak adlandırılan yöntemi onaylamak veya silme işlemini iptal etmek için bir fırsat kullanıcıya veren bir görünüm görüntüler. Kullanıcı onaylarsa, bir POST isteği oluşturulur. Bu durumda, HttpPost `Delete` yöntemi çağrılır ve bu yöntem gerçekte silme işlemi gerçekleştirir.

Try-catch bloğu HttpPost ekleyeceksiniz `Delete` veritabanı güncelleştirildiğinde oluşabilecek tüm hataları işlemek için yöntem. Bir hata oluşursa, bir hata oluştuğunu gösteren bir parametre geçirme HttpGet Delete yöntemini HttpPost Delete yöntemini çağırır. HttpGet Delete yöntemini ardından onay sayfasında kullanıcı iptal etme veya yeniden denemek için bir fırsat vermiş hata iletisi ile birlikte görüntüler.

HttpGet Değiştir `Delete` hata raporlama yönetir aşağıdaki kodla eylem yöntemi.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Bu kod değişiklikleri kaydetmek için bir hatadan sonra yöntemi çağrıldı olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu parametre yanlış HttpGet `Delete` yöntemi, önceki bir hata çağrılır. Ne zaman çağırıldığında tarafından HttpPost `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası parametresi true olduğunda ve bir hata iletisi görünüme iletilir.

### <a name="the-read-first-approach-to-httppost-delete"></a>Okuma ilk yaklaşım HttpPost silme

HttpPost Değiştir `Delete` eylem yöntemi (adlı `DeleteConfirmed`) gerçek silme işlemi gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Seçilen varlığın bu kodu alır sonra çağırır `Remove` varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır SQL DELETE komutu oluşturulur.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Create-ve-ekleme yaklaşım HttpPost silme

Yüksek hacimli uygulama performansını iyileştirme bir öncelik ise, yalnızca birincil kullanarak bir öğrenci varlık örneği tarafından gereksiz bir SQL sorgusu kaçının anahtar değeri ve varlık durumu ayarı `Deleted`. Entity Framework varlığı silmek için gereken tüm budur. (Yalnızca bir alternatif göstermek için bu aşağıda verilmiştir; bu kodu projenizde koymayın.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Varlık ilgili de silinip silinmeyeceği verileri, o art arda silme veritabanında yapılandırıldığından emin olun. Varlık silme işlemi için Bu yaklaşımda, silinecek ilgili varlık vardır EF farkına varmazsınız.

### <a name="update-the-delete-view"></a>Delete görünümünü güncelleştirme

İçinde *Views/Student/Delete.cshtml*, hata iletisi h2 başlığı ve h3 başlık arasında aşağıdaki örnekte gösterildiği gibi ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın bir **silmek** köprü:

![Onay sayfasında Sil](crud/_static/student-delete.png)

Tıklatın **silmek**. Dizin Sayfası silinen Öğrenci görüntülenir. (Hata işleyen kodu eşzamanlılık öğreticideki uygulamada bir örneğini göreceksiniz.)

## <a name="closing-database-connections"></a>Veritabanı bağlantıları kapatma

İşiniz bittiğinde ile bir veritabanı bağlantısı tutan kaynakları boşaltmak için bağlam örneğinin mümkün olan en kısa sürede çıkarılması gerekir. ASP.NET Core yerleşik [bağımlılık ekleme](../../fundamentals/dependency-injection.md) alır bu görev sizin için gerçekleştirir.

İçinde *haline*, çağırmanız [AddDbContext genişletme yöntemi](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) sağlamak için `DbContext` ASP.NET dı kapsayıcısında sınıfı. Yöntem hizmet ömrü ayarlar `Scoped` varsayılan olarak. `Scoped` bağlam nesne ömrü örtüşür web isteği yaşam süresiyle anlamına gelir ve `Dispose` yöntemi çağrılır otomatik olarak web isteği sonunda.

## <a name="handling-transactions"></a>İşleme işlemleri

Varsayılan olarak Entity Framework örtük olarak işlemleri gerçekleştirir. Burada birden çok satır veya tablo için değişiklik ve ardından çağıran senaryolarda `SaveChanges`, Entity Framework otomatik olarak tüm değişikliklerinizi başarılı veya başarısız hepsi emin olur. Bazı değişiklikler ilk yapılır ve bir hata olur, bu değişiklikleri otomatik olarak geri alınır. Burada, daha fazla denetim--Örneğin, bir işlem içinde--Entity Framework dışında yapılan işlemleri dahil etmek istiyorsanız senaryolar görmek için [işlemleri](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Veritabanı bağlamı tablo satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak veritabanında nedir ile varlıkları bellekte eşit olup olmadığını izler. Bellekteki veri önbelleği olarak davranır ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve atıldı her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlığın varlık yeniden kullanılmadan önce genellikle atıldı.

Çağırarak varlık nesnesi bellekte izleme devre dışı bırakabilirsiniz `AsNoTracking` yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

* Bağlam ömrü boyunca herhangi bir varlık güncelleştirmeniz gerekmez ve EF için gerekmeyen [otomatik olarak ayrı sorgular tarafından alınan varlıklarla Gezinti özellikleri yük](read-related-data.md). Bu koşullar bir denetleyicinin HttpGet eylem yöntemlerinde sık karşılıyor.

* Büyük miktarda veri alan bir sorgu çalıştırıyorsanız ve yalnızca küçük bir bölümü, döndürülen verilerin güncelleştirilir. Büyük sorgu için izlemeyi devre dışı bırakın ve daha sonra güncelleştirilmesi gereken birkaç varlıklar için bir sorgu çalıştırmak için daha etkili olabilir.

* Güncelleştirmek için bir varlık eklemek için kullanmak istediğiniz, ancak daha önce farklı bir amaç için aynı varlık alınır. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlık eklenemiyor. Bu durumu yönetmek için bir yol çağırmaktır `AsNoTracking` önceki sorgusu.

Daha fazla bilgi için bkz: [vs izleme. Hayır-izleme](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Özet

Artık Öğrenci varlıklar için basit CRUD işlemleri gerçekleştiren sayfaları eksiksiz bir kümesini var. İşlevlerini genişletmek sonraki öğreticide **dizin** sıralama, filtreleme ve disk belleği ekleyerek sayfası.

>[!div class="step-by-step"]
[Önceki](intro.md)
[sonraki](sort-filter-page.md)  
