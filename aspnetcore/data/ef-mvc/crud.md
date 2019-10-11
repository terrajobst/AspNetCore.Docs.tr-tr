---
title: 'Öğretici: EF Core ile CRUD Işlevselliği uygulama-ASP.NET MVC'
description: Bu öğreticide, bu CRUD (oluşturma, okuma, güncelleştirme, silme) kodunu inceleyerek, MVC yapı iskelesi denetleyiciler ve görünümlerde sizin için otomatik olarak oluşturulur.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/crud
ms.openlocfilehash: f0c5bcff4c4b0808f9b4703e1429c3a6d1a7a2d7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259727"
---
# <a name="tutorial-implement-crud-functionality---aspnet-mvc-with-ef-core"></a>Öğretici: EF Core ile CRUD Işlevselliği uygulama-ASP.NET MVC

Önceki öğreticide, Entity Framework ve LocalDB SQL Server kullanarak verileri depolayan ve görüntüleyen bir MVC uygulaması oluşturdunuz. Bu öğreticide, bu CRUD (oluşturma, okuma, güncelleştirme, silme) kodunu inceleyerek, MVC yapı iskelesi denetleyiciler ve görünümlerde sizin için otomatik olarak oluşturulur.

> [!NOTE]
> Denetleyiciniz ve veri erişim katmanı arasında bir soyutlama katmanı oluşturmak için depo deseninin uygulanması yaygın bir uygulamadır. Bu öğreticileri basit tutmak ve Entity Framework kendisini nasıl kullanacağınızı öğretmeniz için, depoları kullanmaz. EF olan depolar hakkında daha fazla bilgi için [Bu serideki son öğreticiye](advanced.md)bakın.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ayrıntılar sayfasını özelleştirme
> * Oluştur sayfasını Güncelleştir
> * Düzenleme sayfasını Güncelleştir
> * Silme sayfasını Güncelleştir
> * Veritabanı bağlantılarını kapat

## <a name="prerequisites"></a>Önkoşullar

* [EF Core ve ASP.NET Core MVC ile çalışmaya başlama](intro.md)

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

Bu özellik bir koleksiyon içerdiğinden, öğrenciler dizin sayfasına yönelik scafkatlanmış kod `Enrollments` özelliğini bıraktı. **Ayrıntılar** sayfasında, koleksiyonun IÇERIĞINI bir HTML tablosunda görüntüleyeceksiniz.

*Controllers/StudentsController. cs*dosyasında, Ayrıntılar görünümü için eylem yöntemi tek bir `Student` varlığı almak için `SingleOrDefaultAsync` yöntemini kullanır. @No__t-0 çağıran kodu ekleyin. Aşağıdaki Vurgulanan kodda gösterildiği gibi, `ThenInclude` ve `AsNoTracking` yöntemleri.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

@No__t-0 ve `ThenInclude` yöntemleri, bağlamın `Student.Enrollments` gezinti özelliğini yüklemesine ve her kayıt için `Enrollment.Course` gezinti özelliğine neden olur.  [İlgili verileri oku](read-related-data.md) öğreticisinde bu yöntemler hakkında daha fazla bilgi edineceksiniz.

@No__t-0 yöntemi, döndürülen varlıkların geçerli bağlamın ömrü içinde güncellenmeyeceği senaryolarda performansı geliştirir. Bu öğreticinin sonunda `AsNoTracking` hakkında daha fazla bilgi edineceksiniz.

### <a name="route-data"></a>Veri yönlendirme

@No__t-0 yöntemine geçirilen anahtar değeri *Rota verilerinden*gelir. Rota verileri, model cildin URL 'nin bir kesiminde bulduğu veriler olur. Örneğin, varsayılan yol denetleyiciyi, eylemi ve kimlik segmentlerini belirtir:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Aşağıdaki URL 'de, varsayılan yol eğitmeni denetleyici olarak eşler, eylem olarak dizin ve kimlik olarak 1; Bunlar rota veri değerleridir.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL 'nin son bölümü ("? CourseID = 2021") bir sorgu dizesi değeridir. Model Ciltçi Ayrıca KIMLIK değerini bir sorgu dizesi değeri olarak geçirdiğinizde `Index` yöntemine `id` parametresine geçicektir:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Dizin sayfasında, bağ URL 'Leri Razor görünümündeki Tag Helper deyimleri tarafından oluşturulur. Aşağıdaki Razor kodunda, `id` parametresi varsayılan yol ile eşleşir, bu nedenle yol verilerine `id` eklenir.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Bu, `item.ID` 6 olduğunda aşağıdaki HTML 'yi oluşturur:

```html
<a href="/Students/Edit/6">Edit</a>
```

Aşağıdaki Razor kodunda, `studentID` varsayılan rotadaki bir parametreyle eşleşmez, bu nedenle bir sorgu dizesi olarak eklenir.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Bu, `item.ID` 6 olduğunda aşağıdaki HTML 'yi oluşturur:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Etiket Yardımcıları hakkında daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

### <a name="add-enrollments-to-the-details-view"></a>Ayrıntılar görünümüne kayıtları ekleyin

*Görünümleri/öğrencileri/details. cshtml*dosyasını açın. Her alan, aşağıdaki örnekte gösterildiği gibi `DisplayNameFor` ve `DisplayFor` yardımcıları kullanılarak görüntülenir:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Son alandan sonra ve kapanış `</dl>` etiketinden hemen önce, kayıt listesini göstermek için aşağıdaki kodu ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Kodu yapıştırdıktan sonra kod girintileme yanlış ise, düzeltmek için CTRL-K-D tuşlarına basın.

Bu kod `Enrollments` Gezinti özelliğindeki varlıklar aracılığıyla döngü başlatır. Her kayıt için kurs başlığını ve sınıfı görüntüler. Kurs başlığı, kayıt varlığının `Course` gezinti özelliğinde depolanan kurs varlığından alınır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve bir öğrenci için **Ayrıntılar** bağlantısına tıklayın. Seçili öğrenci için Kurslar ve notlar listesini görürsünüz:

![Öğrenci Ayrıntıları sayfası](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Oluştur sayfasını Güncelleştir

*StudentsController.cs*' de, bir try-catch bloğu ekleyerek ve `Bind` özniteliğinden kimliği kaldırarak httppost `Create` yöntemini değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Bu kod, ASP.NET Core MVC model Ciltçi tarafından oluşturulan öğrenci varlığını öğrenciler varlık kümesine ekler ve değişiklikleri veritabanına kaydeder. (Model Ciltçi, bir form tarafından gönderilen verilerle çalışmanıza daha kolay hale getiren ASP.NET Core MVC işlevselliğine başvurur; bir model Bağlayıcısı, postalanan form değerlerini CLR türlerine dönüştürür ve parametreleri parametreler ' de eylem yöntemine geçirir. Bu durumda model Ciltçi, form koleksiyonundaki özellik değerlerini kullanarak sizin için bir öğrenci varlığı oluşturur.)

"Satır eklendiğinde SQL Server otomatik olarak ayarlanacak birincil anahtar değeri olduğundan, `Bind` özniteliğinden `ID` ' yı kaldırmış olursunuz. Kullanıcı girişi, KIMLIK değerini ayarladı.

@No__t-0 özniteliği dışında, try-catch bloğu, scafkatlanmış kodda yapmış olduğunuz tek değişikdir. Değişiklikler kaydedilirken `DbUpdateException` ' dan türetilen bir özel durum yakalanmışsa, genel bir hata iletisi görüntülenir. `DbUpdateException` özel durumlar bazen bir programlama hatası yerine uygulamanın harici bir şeydir. bu nedenle, kullanıcının yeniden denemek tavsiye edilir. Bu örnekte uygulanmamış olsa da, bir üretim kalitesi uygulaması özel durumu günlüğe kaydeder. Daha fazla bilgi için bkz. Izleme ve telemetri bölümünde **Öngörüler Için günlük** [(Azure Ile gerçek bulut uygulamaları oluşturma)](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

@No__t-0 özniteliği, siteler arası istek sahteciliği (CSRF) saldırılarını önlemeye yardımcı olur. Belirteç, [Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) tarafından otomatik olarak görünüme eklenir ve form kullanıcı tarafından gönderildiğinde dahil edilir. Belirteç `ValidateAntiForgeryToken` özniteliği tarafından onaylanır. CSRF hakkında daha fazla bilgi için bkz. [Istek önleyici](../../security/anti-request-forgery.md)güvenlik.

<a id="overpost"></a>

### <a name="security-note-about-overposting"></a>Fazla nakil hakkında güvenlik notunda

İskele eklenen kodun `Create` yöntemine dahil olduğu `Bind` özniteliği, oluşturma senaryolarında çok fazla gönderim için bir yoldur. Örneğin, öğrenci varlığının bu Web sayfasının ayarlamak istemediğiniz bir `Secret` özelliği içerdiğini varsayalım.

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

Web sayfasında `Secret` alanı olmasa bile, bir korsan Fiddler gibi bir araç kullanabilir veya bir `Secret` form değeri göndermek için bazı JavaScript 'ler yazabilir. @No__t-0 özniteliği olmadan model cildin bir öğrenci örneği oluşturduğunda kullandığı alanları sınırlayarak model Ciltçi, bu `Secret` form değerini seçer ve öğrenci varlık örneğini oluşturmak için onu kullanır. Daha sonra, `Secret` form alanı için belirtilen korsanın hangi değeri veritabanınızda güncelleştirilemeyebilir. Aşağıdaki görüntüde, `Secret` alanı ("OverPost" değeri ile), postalanan form değerlerine eklenirken Fiddler aracı gösterilmektedir.

![Fiddler gizli alanı ekleniyor](crud/_static/fiddler.png)

Daha sonra "OverPost" değeri, eklenen satırın `Secret` özelliğine başarıyla eklenir, ancak hiçbir şekilde Web sayfasının bu özelliği ayarlayabilmesini amaçlayamazsınız.

Önce veritabanından varlığı okuyup daha sonra açıkça izin verilen bir Özellikler listesine geçirerek `TryUpdateModel` ' ı çağırarak düzenleme senaryolarında fazla nakletmeyi engelleyebilirsiniz. Bu, bu öğreticilerde kullanılan yöntemidir.

Birçok geliştirici tarafından tercih edilen aşırı nakletmeyi önlemenin alternatif bir yolu, model bağlamasıyla varlık sınıfları yerine görüntüleme modellerini kullanmaktır. Yalnızca görünüm modelinde güncelleştirmek istediğiniz özellikleri ekleyin. MVC model Bağlayıcısı tamamlandıktan sonra, görünüm modeli özelliklerini, isteğe bağlı olarak, Automaber gibi bir araç kullanarak varlık örneğine kopyalayın. Varlık örneğindeki `_context.Entry` ' ı kullanarak durumunu `Unchanged` olarak ayarlayın ve ardından Görünüm modelinde bulunan her bir varlık özelliğinde `Property("PropertyName").IsModified` ' yi true olarak ayarlayın. Bu yöntem hem düzenleme hem de oluşturma senaryolarında çalışmaktadır.

### <a name="test-the-create-page"></a>Oluştur sayfasını test etme

*Views/öğrenciler/Create. cshtml* içindeki kod, her alan için `label`, `input` ve `span` (doğrulama iletileri için) etiket yardımcıları kullanır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve **Yeni oluştur**' a tıklayın.

Ad ve tarih girin. Tarayıcınız bunu yapmanızı sağlar, geçersiz bir tarih girmeyi deneyin. (Bazı tarayıcılar bir tarih seçici kullanmanıza zorlar.) Hata iletisini görmek için **Oluştur** ' a tıklayın.

![Tarih doğrulama hatası](crud/_static/date-error.png)

Bu, varsayılan olarak aldığınız sunucu tarafı doğrulamadır; sonraki bir öğreticide, istemci tarafı doğrulama için kod oluşturacak özniteliklerin nasıl ekleneceğini görürsünüz. Aşağıdaki Vurgulanan kodda, `Create` yönteminde model doğrulama denetimi gösterilmektedir.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Tarihi geçerli bir değer olarak değiştirin ve yeni öğrencinin **Dizin** sayfasında göründüğünü görmek için **Oluştur** ' a tıklayın.

## <a name="update-the-edit-page"></a>Düzenleme sayfasını Güncelleştir

*StudentController.cs*' de, httpget `Edit` yöntemi (`HttpPost` özniteliği olmayan), `Details` yönteminde gördüğünüz gibi seçili öğrenci varlığını almak için `SingleOrDefaultAsync` yöntemini kullanır. Bu yöntemi değiştirmeniz gerekmez.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Önerilen HttpPost düzenleme kodu: okuma ve güncelleştirme

HttpPost düzenleme eylemi yöntemini aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Bu değişiklikler, aşırı nakletmeyi engellemek için en iyi güvenlik uygulamasını uygular. Desteği bir `Bind` özniteliği üretti ve model Ciltçi tarafından oluşturulan varlığı bir `Modified` bayrağıyla ekledi. Bu kod pek çok senaryo için önerilmez çünkü `Bind` özniteliği `Include` parametresinde listelenmeyen alanlarda önceden var olan verileri temizler.

Yeni kod, mevcut varlığı okur ve alınan varlıktaki alanları, [postalanan form verilerinde Kullanıcı girişine göre](xref:mvc/models/model-binding)güncelleştirmek için `TryUpdateModel` çağırır. Entity Framework otomatik değişiklik izleme, form girişi tarafından değiştirilen alanlarda `Modified` bayrağını ayarlar. @No__t-0 yöntemi çağrıldığında, Entity Framework veritabanı satırını güncelleştirmek için SQL deyimleri oluşturur. Eşzamanlılık çakışmaları yok sayılır ve yalnızca Kullanıcı tarafından güncellenen tablo sütunları veritabanında güncelleştirilir. (Sonraki bir öğretici eşzamanlılık çakışmalarının nasıl işleneceğini gösterir.)

Fazla nakletmeyi önleyen en iyi yöntem olarak, **düzenleme** sayfası tarafından güncelleştirilemek istediğiniz alanlar `TryUpdateModel` parametrelerinde beyaz listeye alınır. (Parametre listesindeki alanlar listesinden önceki boş dize, form alanları adlarıyla kullanılacak bir ön ek içindir.) Şu anda koruduğunuz ek alan yok, ancak model cildin bağlamasını istediğiniz alanları listelemek, gelecekte veri modeline alanlar eklerseniz, bunları buraya açıkça eklemeene kadar otomatik olarak korunur.

Bu değişikliklerin sonucu olarak, HttpPost `Edit` yönteminin yöntem imzası, HttpGet `Edit` yöntemiyle aynıdır; Bu nedenle, `EditPost` metodunu yeniden adlandırdınız.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternatif HttpPost düzenleme kodu: oluşturma ve iliştirme

Önerilen HttpPost düzenleme kodu yalnızca değiştirilen sütunların güncelleştirilmesini sağlar ve model bağlama dahil etmek istemediğiniz özelliklerde verileri korur. Ancak, ilk okuma yaklaşımı, fazladan bir veritabanı okuması gerektirir ve eşzamanlılık çakışmalarını işlemek için daha karmaşık kod oluşmasına neden olabilir. Diğer bir seçenek de model cildi tarafından oluşturulan bir varlığı EF bağlamına eklemektir ve değiştirildi olarak işaretler. (Projenizi bu kodla güncelleştirmeyin, yalnızca isteğe bağlı bir yaklaşımı göstermek için gösterilir.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Web sayfası kullanıcı arabirimi varlıktaki tüm alanları içerdiğinde ve bunlardan herhangi birini güncelleştirebilmeniz durumunda bu yaklaşımı kullanabilirsiniz.

Scafkatlanmış kod, oluşturma ve iliştirme yaklaşımını kullanır, ancak yalnızca `DbUpdateConcurrencyException` özel durumlarını yakalar ve 404 hata kodları döndürür.  Gösterilen örnek herhangi bir veritabanı güncelleştirme özel durumunu yakalar ve bir hata iletisi görüntüler.

### <a name="entity-states"></a>Varlık durumları

Veritabanı bağlamı, bellekteki varlıkların veritabanında karşılık gelen satırlarıyla eşitlenmiş olup olmadığını izler ve bu bilgiler `SaveChanges` yöntemini çağırdığınızda ne olacağını belirler. Örneğin, `Add` yöntemine yeni bir varlık geçirdiğinizde, bu varlığın durumu `Added` olarak ayarlanır. Sonra, `SaveChanges` yöntemini çağırdığınızda veritabanı bağlamı bir SQL INSERT komutu yayınlar.

Bir varlık aşağıdaki durumlardan birinde olabilir:

* `Added`. Varlık veritabanında henüz yok. @No__t-0 yöntemi bir INSERT ifadesini yayınlar.

* `Unchanged`. @No__t-0 yöntemiyle bu varlıkla hiçbir şeyin yapılması gerekmez. Veritabanından bir varlık okuduğunuzda, varlık bu durumla başlar.

* `Modified`. Varlığın özellik değerlerinin bazıları veya tümü değiştirildi. @No__t-0 yöntemi bir UPDATE ifadesini yayınlar.

* `Deleted`. Varlık silinmek üzere işaretlendi. @No__t-0 yöntemi bir DELETE ifadesini yayınlar.

* `Detached`. Varlık, veritabanı bağlamı tarafından izlenmiyor.

Bir masaüstü uygulamasında durum değişiklikleri genellikle otomatik olarak ayarlanır. Bir varlığı okur ve bazı özellik değerlerinde değişiklik yaparsınız. Bu, varlık durumunun otomatik olarak `Modified` olarak değiştirilmesine neden olur. @No__t-0 ' ı çağırdığınızda, Entity Framework yalnızca değiştirdiğiniz gerçek özellikleri güncelleştiren bir SQL UPDATE bildirisi oluşturur.

Bir Web uygulamasında, ilk olarak bir varlığı okuyan `DbContext` ve bir sayfa işlendikten sonra düzenlenecek verilerini görüntüler. HttpPost `Edit` eylem yöntemi çağrıldığında yeni bir Web isteği yapılır ve `DbContext` ' in yeni bir örneğine sahip olursunuz. Varlığı bu yeni bağlamda yeniden okuduğunuzda masaüstü işleme benzetimi yapılır.

Ancak, fazladan okuma işlemi yapmak istemiyorsanız, model Ciltçi tarafından oluşturulan varlık nesnesini kullanmanız gerekir.  Bunu yapmanın en kolay yolu, daha önce gösterilen diğer HttpPost düzenleme kodunda yapıldığı gibi varlık durumunun değiştirilme olarak ayarlanmanız olur. Sonra, `SaveChanges` ' ı çağırdığınızda, bağlam hangi özelliklerden hangilerinin değiştirildiğini bilen bir yolu olmadığından, Entity Framework veritabanı satırının tüm sütunlarını günceller.

İlk okuma yaklaşımına engel olmak istiyorsanız, ancak SQL UPDATE bildiriminin yalnızca kullanıcının gerçekten değiştirdiği alanları güncelleştirmesini istiyorsanız, kod daha karmaşıktır. HttpPost `Edit` yöntemi çağrıldığında kullanılabilir olmaları için özgün değerleri bir şekilde (örneğin, gizli alanları kullanarak) kaydetmeniz gerekir. Ardından özgün değerleri kullanarak bir öğrenci varlığı oluşturabilir, `Attach` yöntemini varlığın orijinal sürümüyle çağırabilir, varlığın değerlerini yeni değerlerle güncelleştirebilir ve ardından `SaveChanges` ' i çağırabilirsiniz.

### <a name="test-the-edit-page"></a>Düzenleme sayfasını test etme

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve köprü **Düzenle** ' ye tıklayın.

![Öğrenciler düzenleme sayfası](crud/_static/student-edit.png)

Bazı verileri değiştirin ve **Kaydet**' e tıklayın. **Dizin** sayfası açılır ve değiştirilen verileri görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfasını Güncelleştir

*StudentController.cs*' de, httpget `Delete` yöntemi için şablon kodu, Ayrıntılar ve düzenleme yöntemlerinde gördüğünüz gibi seçili öğrenci varlığını almak için `SingleOrDefaultAsync` yöntemini kullanır. Ancak, `SaveChanges` ' a yapılan çağrı başarısız olduğunda özel bir hata iletisi uygulamak için, bu yönteme ve buna karşılık gelen görünüme bazı işlevler eklersiniz.

Güncelleştirme ve oluşturma işlemleri için gördüğünüz gibi silme işlemleri için iki eylem yöntemi gerekir. GET isteğine yanıt olarak çağrılan yöntem, kullanıcıya silme işlemini onaylama veya iptal etme şansı veren bir görünüm görüntüler. Kullanıcı onu onayladığında, bir POST isteği oluşturulur. Bu durumda, HttpPost `Delete` yöntemi çağrılır ve bu yöntem aslında silme işlemini gerçekleştirir.

Veritabanı güncelleştirilirken oluşabilecek hataları işlemek için HttpPost `Delete` yöntemine bir try-catch bloğu ekleyeceksiniz. Bir hata oluşursa, HttpPost Delete yöntemi bir hata oluştuğunu gösteren bir parametre geçirerek HttpGet Delete yöntemini çağırır. HttpGet Delete yöntemi daha sonra hata iletisiyle birlikte onay sayfasını yeniden görüntüler ve kullanıcıya iptal etmek veya yeniden denemek için bir fırsat verir.

HttpGet `Delete` eylem yöntemini aşağıdaki kodla değiştirin, bu hata raporlamayı yönetir.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Bu kod, bir yöntemin değişiklikleri kaydetme hatasından sonra döndürülüp çağrılmadığını belirten isteğe bağlı bir parametresini kabul eder. HttpGet `Delete` yöntemi önceki bir hata olmadan çağrıldığında bu parametre false 'tur. Bir veritabanı güncelleştirme hatasına yanıt olarak HttpPost `Delete` yöntemi tarafından çağrıldığında, parametresi true olur ve görünüme bir hata mesajı geçirilir.

### <a name="the-read-first-approach-to-httppost-delete"></a>HttpPost silme için ilk okuma yaklaşımı

HttpPost `Delete` eylem yöntemini (`DeleteConfirmed` adlı), gerçek silme işlemini gerçekleştiren ve tüm veritabanı güncelleştirme hatalarını yakalayan aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6-9,11-12,16-21)]

Bu kod seçili varlığı alır, ardından varlığın durumunu `Deleted` olarak ayarlamak için `Remove` yöntemini çağırır. @No__t-0 çağrıldığında, bir SQL DELETE komutu oluşturulur.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>HttpPost silme için Oluştur ve Ekle yaklaşımı

Yüksek hacimli bir uygulamadaki performansı artırmak bir önceliktir, yalnızca birincil anahtar değerini kullanarak bir öğrenci varlığı oluşturarak ve ardından varlık durumunu `Deleted` olarak ayarlayarak gereksiz bir SQL sorgusundan kaçınabilirsiniz. Bu, Entity Framework varlığı silmek için ihtiyaç duymaktadır. (Bu kodu projenize yerleştirmeyin; bir alternatif göstermek de yeterlidir.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Varlığın de silinmesi gereken ilgili veriler varsa, veritabanında Cascade silmenin yapılandırıldığından emin olun. Varlık silme yaklaşımıyla EF, silinecek ilgili varlıkların olduğunu fark etmeyebilir.

### <a name="update-the-delete-view"></a>Silme görünümünü Güncelleştir

*Görünümler/öğrenci/delete. cshtml*' de, aşağıdaki örnekte gösterildiği gibi, H2 başlığı ve H3 başlığı arasına bir hata iletisi ekleyin:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve bir **Delete** köprüsüne tıklayın:

![Onay sayfasını Sil](crud/_static/student-delete.png)

**Sil**'e tıklayın. Dizin sayfası, silinen öğrenci olmadan görüntülenir. (Eşzamanlılık öğreticisinde işlem içinde kodu işleme hatası hakkında bir örnek görürsünüz.)

## <a name="close-database-connections"></a>Veritabanı bağlantılarını kapat

Bir veritabanı bağlantısının tuttuğu kaynakları boşaltmak için bağlam örneği, bununla işiniz bittiğinde en kısa sürede atılmalıdır. ASP.NET Core yerleşik [bağımlılık ekleme](../../fundamentals/dependency-injection.md) , sizin için bu görevi gerçekleştirir.

*Startup.cs*, `DbContext` SıNıFıNı ASP.NET Core dı kapsayıcısında sağlamak Için [adddbcontext genişletme yöntemini](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) çağırın. Bu yöntem, hizmet ömrünü varsayılan olarak `Scoped` olarak ayarlar. `Scoped`, Web isteği yaşam süresi ile saatle çakışan bağlam nesnesi ömrünü gösterir ve Web isteğinin sonunda `Dispose` yöntemi otomatik olarak çağrılır.

## <a name="handle-transactions"></a>İşlemleri işle

Entity Framework, varsayılan olarak işlemleri örtülü olarak uygular. Birden çok satır veya tabloda değişiklik yaptığınız ve sonra `SaveChanges` ' ı çağıran senaryolarda, Entity Framework otomatik olarak tüm değişikliklerinizin başarılı olduğundan veya tümünün başarısız olduğundan emin olur. Önce bazı değişiklikler yapıldıktan sonra bir hata oluşursa, bu değişiklikler otomatik olarak geri alınır. Daha fazla denetime ihtiyacınız olan senaryolar için--örneğin, işlem içinde Entity Framework dışında yapılan işlemleri eklemek istiyorsanız, bkz. [işlemler](/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>İzleme sorguları yok

Bir veritabanı bağlamı tablo satırları aldığında ve bunları temsil eden varlık nesneleri oluşturduğunda, varsayılan olarak, bellekteki varlıkların veritabanında bulunan verilerle eşitlenmiş olup olmadığını izler. Bellekteki veriler önbellek olarak davranır ve bir varlığı güncelleştirdiğinizde kullanılır. Bağlam örnekleri genellikle kısa süreli olduğundan (her istek için yeni bir tane oluşturulup bırakıldığı) ve bir varlığı okuyan bağlam genellikle bu varlık yeniden kullanılmadan önce atıldığından, bu önbelleğe alma işlemi bir Web uygulamasında genellikle gereksizdir.

@No__t-0 yöntemini çağırarak bellekte varlık nesnelerinin izlenmesini devre dışı bırakabilirsiniz. Bunu yapmak isteyebileceğiniz tipik senaryolar şunlardır:

* Bağlam ömrü boyunca herhangi bir varlığı güncelleştirmeniz gerekmez ve [ayrı sorgular tarafından alınan varlıklarla birlikte gezinti özelliklerini otomatik olarak yüklemek](read-related-data.md)için EF 'e ihtiyacınız yoktur. Bu koşullar genellikle denetleyicinin HttpGet eylem yöntemlerinde karşılanır.

* Büyük miktarda veri alan bir sorgu çalıştırıyorsunuz ve döndürülen verilerin yalnızca küçük bir kısmı güncelleştiriliyor. Büyük sorgu için izlemeyi devre dışı bırakmak ve daha sonra güncellenmesi gereken birkaç varlık için bir sorgu çalıştırmak daha verimli olabilir.

* Bir varlığı güncelleştirmek için eklemek istiyorsunuz, ancak daha önce farklı bir amaç için aynı varlığı elde edersiniz. Varlık veritabanı bağlamı tarafından zaten izlenmekte olduğundan, değiştirmek istediğiniz varlığı iliştiremiyoruz. Bu durumu işlemenin bir yolu, önceki sorgudaki `AsNoTracking` öğesini çağırmanız olur.

Daha fazla bilgi için bkz. izleme [vs.](/ef/core/querying/tracking)

## <a name="get-the-code"></a>Kodu edinin

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ayrıntılar sayfası özelleştirildi
> * Oluşturma sayfası güncelleştirildi
> * Düzenleme sayfası güncelleştirildi
> * Silme sayfası güncelleştirildi
> * Kapalı veritabanı bağlantıları

Sıralama, filtreleme ve sayfalama ekleyerek **Dizin** sayfasının işlevselliğini genişletmeyi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sonraki: sıralama, filtreleme ve sayfalama](sort-filter-page.md)
