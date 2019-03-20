---
title: 'Öğretici: CRUD işlevselliği - EF çekirdekli ASP.NET MVC uygulama'
description: Bu öğreticide, gözden geçirmenizi ve özelleştirme CRUD (oluşturma, okuma, güncelleştirme ve silme) MVC yapı iskelesi otomatik olarak sizin için denetleyicileri ve görünümleri oluşturan kodu.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/crud
ms.openlocfilehash: 83f5d4bdc3d5872109649818b61a6dbb656fd8be
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264831"
---
# <a name="tutorial-implement-crud-functionality---aspnet-mvc-with-ef-core"></a>Öğretici: CRUD işlevselliği - EF çekirdekli ASP.NET MVC uygulama

Önceki öğreticide, depolar ve SQL Server LocalDB ve Entity Framework kullanarak verileri görüntüleyen bir MVC uygulaması oluşturdunuz. Bu öğreticide, gözden geçirmenizi ve özelleştirme CRUD (oluşturma, okuma, güncelleştirme ve silme) MVC yapı iskelesi otomatik olarak sizin için denetleyicileri ve görünümleri oluşturan kodu.

> [!NOTE]
> Denetleyicinizi ve veri erişim katmanı arasında bir Soyutlama Katmanı oluşturmak için havuz deseni uygulamak için yaygın bir uygulamadır. Bu öğreticiler basit ve Entity Framework kullanma eğitiminde odaklanmıştır tutmak için bunlar depoları kullanmayın. Depoları EF ile ilgili daha fazla bilgi için bkz: [bu serinin son öğreticide](advanced.md).

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ayrıntılar sayfasını özelleştirme
> * Güncelleştirme Oluştur sayfası
> * Güncelleştirme düzenleme sayfası
> * Silme sayfası
> * Kapat veritabanı bağlantıları

## <a name="prerequisites"></a>Önkoşullar

* [Bir ASP.NET Core MVC web uygulamasında EF Core ile çalışmaya başlama](intro.md)

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

İskele kurulan kodu Öğrenciler dizin sayfasının sol `Enrollments` özelliği, bunun nedeni, bu özellik bir koleksiyonu. İçinde **ayrıntıları** sayfanın HTML tablosu halinde koleksiyonun içeriğini görüntüleyeceksiniz.

İçinde *Controllers/StudentsController.cs*, eylem yöntemi için ayrıntıları görüntülemek kullandığı `SingleOrDefaultAsync` yönteminin tek bir almak için `Student` varlık. Çağıran kod ekleme `Include`. `ThenInclude`, ve `AsNoTracking` vurgulanan aşağıdaki kodda gösterildiği gibi yöntemleri.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` Ve `ThenInclude` yöntemlerinin neden yüklemek bağlam `Student.Enrollments` gezinti özelliği ve her kaydı `Enrollment.Course` gezinme özelliği.  Bu yöntemleri hakkında daha fazla bilgi edineceksiniz [ilgili verileri okuma](read-related-data.md) öğretici.

`AsNoTracking` Yöntemi burada döndürülen varlıklara olmaz güncelleştirilmesi geçerli bağlamın yaşam sürelerinin başlarında senaryolarda performansı artırır. Daha fazla hakkında bilgi edineceksiniz `AsNoTracking` Bu öğreticinin sonunda.

### <a name="route-data"></a>Rota verileri

İletilen anahtar değeri `Details` yöntemi geldiği *verilerini yönlendirme*. Rota verilerini, model bağlayıcı URL'sinin bir Segmentte bulunan verilerdir. Örneğin, denetleyici, eylem ve kimliği segmentleri varsayılan yolu belirtir:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Aşağıdaki URL'de varsayılan yol, denetleyici, eylem olarak dizin ve kimlik olarak 1 olarak Eğitmen eşleştirir; Rota veri değerleri şunlardır.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL son kısmını ("? courseID 2021 =") bir sorgu dizesi değeri. Model bağlayıcı kimliği değerine de geçecek `Details` yöntemi `id` sorgu dize değeri olarak geçirirseniz parametresi:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Dizin sayfasında köprü URL'ler, etiket Yardımcısı deyimlerinde Razor görünüm tarafından oluşturulur. Aşağıdaki Razor kodunda `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` için rota verilerini eklenir.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Bu aşağıdaki HTML'yi oluşturur, `item.ID` 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

Aşağıdaki Razor kodunda `studentID` sorgu dizesi olarak eklenir, böylece varsayılan yol, bir parametre ile eşleşmiyor.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Bu aşağıdaki HTML'yi oluşturur, `item.ID` 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Etiket yardımcıları hakkında daha fazla bilgi için bkz: <xref:mvc/views/tag-helpers/intro>.

### <a name="add-enrollments-to-the-details-view"></a>Ayrıntılar görünümü için kayıtlar ekleme

Açık *Views/Students/Details.cshtml*. Her bir alan kullanarak görüntülenen `DisplayNameFor` ve `DisplayFor` Yardımcıları, aşağıdaki örnekte gösterildiği gibi:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Son alan sonra ve kapatmadan önce hemen `</dl>` etiketinde, kayıtları bir listesini görüntülemek için aşağıdaki kodu ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Kod yapıştırıldıktan sonra kod girintilemesinin yanlışsa, düzeltmek için CTRL-K-D tuşlarına basın.

Bu kod, varlıklarda döngü `Enrollments` gezinme özelliği. Her kayıt için bu kurs başlığı ve sınıf görüntüler. Kurs başlığı depolanan kurs varlık alınır `Course` kayıtları varlık gezinme özelliği.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** bir öğrenci bağlantısı. Seçili Öğrenci için kursları ve notlarınızı listesi görürsünüz:

![Öğrenci Ayrıntıları sayfası](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

İçinde *StudentsController.cs*, HttpPost değiştirme `Create` yöntemi bir try-catch bloğu ekleme ve kaldırma Kimliğinden `Bind` özniteliği.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Bu kod, Öğrenciler varlığı için ASP.NET Core MVC model bağlayıcı tarafından oluşturulan Öğrenci varlık ayarlayın ve ardından değişiklikleri veritabanına kaydeder ekler. (Bir form tarafından sunulan verilerle çalışmayı kolaylaştıran ASP.NET Core MVC işlevselliği için model bağlayıcı başvuruyor; bir model bağlayıcı gönderilen form değerleri, CLR türlerine dönüştürür ve eylem yöntemi parametrelerine geçirir. Bu durumda, model bağlayıcı Öğrenci varlık, bir Form koleksiyonu özellik değerleri kullanarak örneği oluşturur.)

Kaldırılan `ID` gelen `Bind` kimliği SQL Server, otomatik olarak satır ne zaman eklendiği ayarlayacak birincil anahtar değeri olduğundan özniteliği. Kullanıcı girişi kimlik değerini ayarlamaz.

Dışındaki `Bind` try-catch bloğu özniteliktir iskele kurulan kodu için yaptığınız tek değişiklik. Türetilen bir özel durum, `DbUpdateException` olan değişiklikleri kaydedilirken yakalandı, genel bir hata iletisi görüntülenir. `DbUpdateException` Kullanıcı yeniden denemeniz önerilir özel durum bazen bir programlama hatası yerine bir uygulama için dış bir şey tarafından kaynaklanır. Bu örnekte uygulanmadı olsa da, üretim kalitesinde uygulaması özel durumu günlüğe kaydedersiniz. Daha fazla bilgi için **ilgili ayrıntılı bilgi için günlük** konusundaki [izleme ve Telemetri (gerçek hayatta kullanılan bulut uygulamaları Azure ile oluşturma)](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

`ValidateAntiForgeryToken` Öznitelik, siteler arası istek sahteciliği (CSRF) saldırılarını önlemeye yardımcı olur. Belirteç görünümde tarafından otomatik olarak eklenen [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) ve kullanıcı tarafından form gönderildiğinde dahildir. Belirteç tarafından doğrulanır `ValidateAntiForgeryToken` özniteliği. CSRF hakkında daha fazla bilgi için bkz: [istek sahteciliğinden koruma](../../security/anti-request-forgery.md).

<a id="overpost"></a>

### <a name="security-note-about-overposting"></a>Güvenlik Notu overposting hakkında

`Bind` Üzerinde iskele kurulan kodu içeren bir öznitelik `Create` yöntemdir overposting karşı korumak için bir yol senaryoları oluşturun. Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` ayarlamak için bu web sayfası istemediğiniz özelliği.

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

Olmasa bile bir `Secret` alan web sayfasında, bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri. Olmadan `Bind` model bağlayıcısının bir öğrenci örneği oluşturduğunda, model Bağlayıcısı kullandığını alanları sınırlama özniteliği ayarladıysanız çekme `Secret` form değeri ve Öğrenci varlık örneği oluşturmak için bunu kullanın. Sonra ne olursa olsun değer için belirtilen korsanın `Secret` form alanı veritabanınızda güncelleştirilmesi. Fiddler aracı ekleme, aşağıdaki resimde gösterilmektedir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

![Fiddler'ı ekleme gizli alan](crud/_static/fiddler.png)

"OverPost" sonra başarıyla eklenmesi için değer `Secret` özelliği eklenen hiçbir zaman web sayfasının bu özelliği ayarlayamaz yönelik olsa da, satır.

Varlığın ilk veritabanından okumak ve ardından arama düzenleme senaryolarda overposting engelleyebilir `TryUpdateModel`, geçen bir açık izin verilen özellikler listesinde. Aşağıdaki öğreticilerde kullanılan yöntem olmasıdır.

Çok sayıda geliştirici tarafından tercih edilen overposting önlemek için alternatif bir yolu varlık sınıfları yerine görünüm modelleri model bağlamayla kullanmaktır. Yalnızca görünüm modelinde güncelleştirmek istediğiniz özellikleri içerir. MVC model bağlayıcı tamamlandıktan sonra Görünüm modeli özellikleri isteğe bağlı olarak AutoMapper gibi bir araç kullanarak varlık örneği kopyalayın. Kullanım `_context.Entry` durumunu ayarlamak için varlık örneği üzerinde `Unchanged`ve ardından `Property("PropertyName").IsModified` görünümü modele dahil edilen her bir varlık özellik üzerinde true. Bu yöntem çalışır hem de düzenleyin ve senaryolar oluşturun.

### <a name="test-the-create-page"></a>Test oluşturma sayfası

Kodda *Views/Students/Create.cshtml* kullanan `label`, `input`, ve `span` (için doğrulama iletilerinin) etiket Yardımcıları için her bir alan.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın **Yeni Oluştur**.

Adları ve bir tarih girin. Tarayıcınız kaydolmanıza izin veriyorsa, geçersiz bir tarih girmeyi deneyin. (Bazı tarayıcılar, bir tarih seçicinin kullanılması için zorla.) Ardından **Oluştur** hata iletisini görmek için.

![Tarih doğrulama hatası](crud/_static/date-error.png)

Varsayılan olarak aldığınız sunucu tarafı doğrulama budur; bir sonraki öğreticide, istemci tarafı doğrulama kodunu da oluşturacağı öznitelikleri ekleme görürsünüz. Model doğrulama denetimi ile aşağıdaki vurgulanmış kodu gösterir `Create` yöntemi.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Tarihi geçerli bir değere değiştirip'ı **Oluştur** görünen yeni Öğrenci görmek için **dizin** sayfası.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

İçinde *StudentController.cs*, HttpGet `Edit` yöntemi (olmadan bir `HttpPost` özniteliği) kullanan `SingleOrDefaultAsync` gördüğünüz gibi seçili Öğrenci varlığı almak için yöntemi `Details` yöntemi. Bu yöntem değiştirmeniz gerekmez.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Önerilen HttpPost düzenleme kodu: Okuma ve güncelleştirme

HttpPost düzenleme eylem yöntemini aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Bu değişiklikler overposting önlemek için güvenlik en iyi uygulama uygulayın. Oluşturulan iskele kurucu bir `Bind` öznitelik veya varlık kümesi için model bağlayıcı tarafından oluşturulan varlık eklenebilir bir `Modified` bayrağı. Kod için birçok senaryo için önerilmez `Bind` özniteliği temizler içinde listelenmeyen alanlar önceden mevcut olan tüm verileri dışarı `Include` parametresi.

Yeni kod çağrıları ve var olan bir varlığa okur `TryUpdateModel` alınan varlık alanları güncelleştirmek için [gönderilen form verilerini kullanıcı girişini temel](xref:mvc/models/model-binding#how-model-binding-works). Entity Framework'ün otomatik değişiklik kümeleri izleme `Modified` form girişi tarafından değiştirilen alanları bayrağı. Zaman `SaveChanges` yöntemi çağrıldığında, Entity Framework veritabanı satırı güncelleştirmek için SQL deyimleri oluşturur. Eşzamanlılık çakışmalarını göz ardı edilir ve yalnızca kullanıcı tarafından güncelleştirildiği tablo sütunları veritabanında güncelleştirilir. (Bir sonraki öğreticide eşzamanlılık çakışmalarını nasıl ele alınacağını gösterir.)

Tarafından güncelleştirilebilmesi için istediğiniz alanları overposting önlemek için en iyi uygulama olarak **Düzenle** sayfası olan içinde beyaz listeye `TryUpdateModel` parametreleri. (Form alanları adları ile kullanılacak bir ön eki parametre listesindeki alanlar listesinde önceki boş bir dize içindir.) Şu anda koruduğunuz hiçbir ek alanlar vardır, ancak bağlamak için model bağlayıcı istediğiniz alanları listesi alanları veri modeli gelecekte eklerseniz, siz açıkça burada ekleyinceye kadar otomatik olarak korumalı olup olmadıklarını olduğunu sağlar.

Bu değişiklikler, yöntem imzasını HttpPost sonucunda `Edit` yöntemi HttpGet aynı olup `Edit` yöntemi; bu nedenle, yöntem yeniden adlandırdıktan `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternatif HttpPost düzenleme kodu: Oluşturma ve ekleme

Önerilen HttpPost düzenleme kod sağlar: Yalnızca değiştirilen sütun güncelleştirilmesi ve model bağlama için dahil edilen istemediğiniz özellikleri verilerini korur. Ancak, ek bir veritabanı okuma ve eşzamanlılık çakışmalarını işleme için daha karmaşık kod sonuçlanabilir okuma öncelikli bir yaklaşım gerektirir. EF bağlamı için model bağlayıcı tarafından oluşturulan bir varlık eklemek ve değiştirilmiş olarak işaretlemek için kullanılan bir alternatiftir. (Projenizi güncelleştirme ile bu kod, yalnızca isteğe bağlı bir yaklaşım göstermek için gösterilen.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Web sayfası kullanıcı Arabirimi varlıktaki tüm alanları içerir ve bunlardan herhangi birini güncelleştirebilirsiniz bu yaklaşımı kullanabilirsiniz.

İskele kurulan kodu oluştur-ve-ekleme yaklaşımını kullanır, ancak yalnızca yakalar `DbUpdateConcurrencyException` 404 hata kodları özel durumlar ve döndürür.  Gösterilen örnekte, tüm veritabanı güncelleştirme özel durumu yakalar ve bir hata iletisi görüntüler.

### <a name="entity-states"></a>Varlık durumları

Veritabanı bağlamı varlıkları bellekte veritabanında ilgili satırlarında ile eşitlenmiş, bu bilgileri çağırdığınızda ne olacağını belirler olup olmadığını izler `SaveChanges` yöntemi. Örneğin, yeni bir varlık gönderdiğinizde `Add` varlığın durumu ayarlanmış yöntemi `Added`. Ardından çağırdığınızda `SaveChanges` yöntemi, veritabanı bağlamı veren bir SQL INSERT komutu.

Bir varlık şu durumlardan birinde olabilir:

* `Added`. Varlık henüz veritabanında mevcut değil. `SaveChanges` Yöntemi sorunları INSERT deyimi.

* `Unchanged`. Hiçbir şey bu varlıkla tarafından yapılması gereken `SaveChanges` yöntemi. Bir varlık veritabanından okurken, varlık bu durumu ile başlar.

* `Modified`. Bazıları veya tümü varlığın özellik değerleri değiştirilmiş. `SaveChanges` Yöntemi, bir güncelleştirme deyimiyle verir.

* `Deleted`. Varlık silinmek üzere işaretlendi. `SaveChanges` Yöntemi DELETE deyimi verir.

* `Detached`. Varlık veritabanı bağlamı izleniyor değil.

Bir masaüstü uygulamasında, durum değişikliklerini genellikle otomatik olarak ayarlanır. Bir varlığı okuma ve değişiklik bazı özellik değerleri. Bu varlık durumunu otomatik olarak değiştirilecek şekilde neden `Modified`. Ardından çağırdığınızda `SaveChanges`, Entity Framework değiştirdiğiniz gerçek özelliklerini güncelleştiren SQL UPDATE deyimi oluşturur.

Bir web uygulaması `DbContext` başlangıçta okuyan bir varlık ile düzenlenecek verilerini bir sayfa oluşturulduğunda elden görüntüler. Zaman HttpPost `Edit` eylem yöntemi çağrıldığında, yeni bir web isteği yapılır ve yeni bir örneğine sahip `DbContext`. Varlık bu yeni bir bağlam içinde yeniden okuma Masaüstü işleme benzetimini yapar.

Ancak yapmak istemiyorsanız, ek okuma işlemi, model bağlayıcı tarafından oluşturulan varlık nesnesi kullanmanız gerekiyor.  Bunu yapmanın en kolay yolu varlık durumu değiştirme için daha önce gösterilen alternatif HttpPost düzenleme kodunda yapılan olarak ayarlamaktır. Ardından çağırdığınızda `SaveChanges`, bağlamı değiştirmiş hangi özellikleri bilmek bir yolu yoktur çünkü Entity Framework, veritabanı satırdaki tüm sütunlar güncelleştirir.

Okuma öncelikli yaklaşım önlemek istediğiniz, ancak kullanıcının gerçekten değiştirilen alanları güncelleştirmek için güncelleştirme SQL deyimi de istiyorsanız, daha karmaşık bir kodudur. Orijinal değerleri bir şekilde kaydetmek zorunda (gibi gizli alanları kullanarak) ve böylece bunlar ne zaman kullanılabilir HttpPost `Edit` yöntemi çağrılır. Orijinal değerleri, çağrı kullanarak bir öğrenci varlık oluşturup `Attach` yöntemi, özgün varlık sürümü ile yeni değerlere varlığın değerlerini güncelleştirin ve ardından çağırmak `SaveChanges`.

### <a name="test-the-edit-page"></a>Testi düzenleme sayfası

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve ardından'a tıklayın bir **Düzenle** köprü.

![Öğrenciler düzenleme sayfası](crud/_static/student-edit.png)

Bazı tıklayın ve veri değiştirme **Kaydet**. **Dizin** sayfası açılır ve değiştirilen verileri görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

İçinde *StudentController.cs*, şablon kodunu HttpGet `Delete` yöntemi kullanan `SingleOrDefaultAsync` yöntemleri Ayrıntılar ve düzenleme gördüğünüz gibi seçili Öğrenci varlığı almak için yöntemi. Ancak, uygulamak için bir özel hata iletisi zaman çağrısı `SaveChanges` başarısız olursa, bazı işlevler, karşılık gelen görebilir ve bu yöntem ekleyeceksiniz.

Güncelleştirme için gördüğünüz ve oluşturma işlemleri gibi silme işlemleri iki eylem yöntemleri gerektirir. Bir GET isteğine yanıt olarak çağrılan yöntem kullanıcı onaylamak veya silme işlemi iptal etmek için bir şans verir bir görünüm görüntüler. Kullanıcıyı onaylarsa, bir POST isteği oluşturulur. Bu durumda, HttpPost `Delete` yöntemi çağrılır ve bu yöntem gerçekten silme işlemini gerçekleştirir.

Bir try-catch bloğu için HttpPost ekleyeceksiniz `Delete` veritabanı güncelleştirildiğinde oluşabilecek hataları işlemek için yöntemi. Bir hata oluşursa bir hata oluştuğunu belirten bir parametre geçirerek HttpGet Delete yöntemini HttpPost Delete yöntemini çağırır. HttpGet Delete yöntemini sonra onay sayfasında kullanıcı yeniden deneyin veya iptal etme fırsatı veren hata iletisi ile birlikte görüntüler.

HttpGet değiştirin `Delete` eylem yöntemini aşağıdaki kodla hata raporlama'yı yöneten.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Bu kod, değişiklikleri kaydetmek için bir hatadan sonra yöntemi çağrıldı olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu parametre false olduğunda HttpGet `Delete` önceki bir hata yöntemi çağrılır. Ne zaman çağırıldığında tarafından HttpPost `Delete` yöntemi yanıt olarak bir veritabanı güncelleştirme hatası, bir parametre true ise ve bir hata iletisi görünüme iletilir.

### <a name="the-read-first-approach-to-httppost-delete"></a>Okuma-ilk yaklaşım HttpPost silme

HttpPost değiştirin `Delete` eylem yöntemine (adlı `DeleteConfirmed`) gerçek silme işlemini gerçekleştirir ve veritabanını güncelleştirme hataları yakalar aşağıdaki kodu.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Bu kod, seçili varlığı alır. ardından çağırır `Remove` varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, SQL'i Sil komut oluşturulur.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Oluştur-ve-ekleme yaklaşım HttpPost silme

Yüksek hacimli uygulama performansını iyileştirme öncelik ise, yalnızca birincil kullanarak bir öğrenci varlık oluşturarak gereksiz bir SQL sorgusu önlemek anahtar değer ve varlık durumunu ardından ayarını `Deleted`. Entity Framework varlığı silmek için gereken tüm budur. (Bu kodu projenizde koymayın; yalnızca bir alternatif göstermek için aşağıda verilmiştir.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Varlık de silinmelidir veri ilişkili ise bu art arda silme veritabanında yapılandırıldığından emin olun. Varlık silinme Bu yaklaşımda, silinmesi için ilgili varlık EF farkına varmazsınız.

### <a name="update-the-delete-view"></a>Delete Görünümü güncelleştirme

İçinde *Views/Student/Delete.cshtml*, H2 bölüm başlığı h3 başlık arasındaki bir hata iletisi aşağıdaki örnekte gösterildiği gibi ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın bir **Sil** köprü:

![Onay sayfası Sil](crud/_static/student-delete.png)

Tıklayın **Sil**. Dizin Sayfası silinen Öğrenci görüntülenir. (Hata işleme kodunu eyleminin eşzamanlılık öğreticideki örneği görürsünüz.)

## <a name="close-database-connections"></a>Kapat veritabanı bağlantıları

İle işiniz bittiğinde veritabanı bağlantısı tutan kaynakları boşaltmak için bağlam örneğinin olabildiğince çabuk çıkarılması gerekir. ASP.NET Core yerleşik [bağımlılık ekleme](../../fundamentals/dependency-injection.md) bu görev sizin için üstlenir.

İçinde *Startup.cs*, çağırmanızı [AddDbContext genişletme yöntemi](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) sağlama `DbContext` ASP.NET Core DI kapsayıcı sınıfı. Yöntem hizmet ömrü ayarlar `Scoped` varsayılan olarak. `Scoped` bağlam nesne ömrü web isteğinin yaşam süresi ile örtüşür anlamına gelir ve `Dispose` yöntemin çağrılacağı otomatik olarak web isteği sonunda.

## <a name="handle-transactions"></a>Tanıtıcı işlemleri

Varsayılan olarak Entity Framework, örtük olarak işlemler uygular. Burada birden çok satır veya tablo için değişiklik ve sonra çağrı senaryolarda `SaveChanges`, Entity Framework otomatik olarak tüm değişikliklerinizi başarılı veya başarısız tüm emin olur. Bazı değişiklikler önce yapılır ve ardından bir hata olur, bu değişiklikleri otomatik olarak geri alınır. Daha denetlediğiniz--Örneğin, bir işlemde--Entity Framework dışında yapılan işlemler dahil etmek istiyorsanız senaryolar görmek için [işlemleri](/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Bir veritabanı bağlamını tablo satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak varlıkları bellekte veritabanında nedir ile eşitlenmiş durumda olup olmadığını izler. Bellekteki verileri bir önbellek olarak görev yapar ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve elden her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlık, varlığın yeniden kullanılmadan önce genellikle atıldı.

Çağırarak bellekte nesnelerin varlık izleme devre dışı bırakabilirsiniz `AsNoTracking` yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

* Bağlam ömrü boyunca herhangi bir varlık güncelleştirmeniz gerekmez ve EF için ihtiyacınız olmayan [ayrı sorgular tarafından alınan varlıklar ile yük Gezinti özellikleri otomatik olarak](read-related-data.md). Bu koşullar, bir denetleyicinin eylem yöntemlerinde HttpGet sık karşılanır.

* Büyük miktarda veri alan bir sorgu çalıştırıyorsanız ve döndürülen verilerin küçük bir kısmını güncelleştirilir. Büyük bir sorgu için izlemeyi devre dışı bırakın ve daha sonra güncelleştirilmesi gereken birkaç varlıklar için bir sorgu çalıştırmak için daha verimli olabilir.

* Bir varlığı güncelleştirmek için eklemek istediğiniz, ancak aynı varlığa farklı bir amaç için daha önce aldığınız. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlığın eklenemiyor. Bu durum işlemek için bir yol çağırmaktır `AsNoTracking` önceki sorguda.

Daha fazla bilgi için [izleme ile. Hayır-izleme](/ef/core/querying/tracking).

## <a name="get-the-code"></a>Kodu alma

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Özelleştirilmiş Ayrıntılar sayfası
> * Oluştur sayfası güncelleştirildi
> * Düzenleme sayfası güncelleştirildi
> * Silme sayfası güncelleştirildi
> * Kapalı veritabanı bağlantıları

İşlevlerini genişletmek hakkında bilgi edinmek için sonraki makaleye ilerleyin **dizin** sıralama, filtreleme ve sayfalama ekleyerek, sayfa.
> [!div class="nextstepaction"]
> [Sıralama, filtreleme ve sayfalama](sort-filter-page.md)
