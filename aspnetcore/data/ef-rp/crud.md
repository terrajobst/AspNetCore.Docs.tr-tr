---
title: Razor sayfalarının ASP.NET Core - CRUD - 2 8'in EF çekirdek ile
author: rick-anderson
description: Oluşturma, okuma, güncelleştirme, EF çekirdek ile silmek nasıl gösterir
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 17d48cae50745508a64a9fb8a153b7b891e64a23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278693"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Razor sayfalarının ASP.NET Core - CRUD - 2 8'in EF çekirdek ile

Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, kurulmuş CRUD (Oluştur, oku, Güncelleştir, Sil) kodu gözden ve özelleştirilebilir.

Not: karmaşıklık en aza indirmek ve EF çekirdek üzerine odaklanan bu öğreticileri korumak için EF çekirdek kodu Razor sayfalarının sayfa modellerinde kullanılır. Bazı geliştiriciler, kullanıcı Arabirimi (Razor sayfaları) ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için bir hizmet katmanı veya depo desende kullanın.

Bu öğretici, oluşturma, düzenleme, silme ve ayrıntıları Razor sayfalarında *Öğrenci* klasörünü değiştirdi.

İskele kurulmuş kod oluşturma, düzenleme ve silme sayfalar için şu biçimi kullanır:

* Alma ve HTTP GET yöntemiyle istenen veri görüntüleme `OnGetAsync`.
* HTTP POST yöntemi ile verilerde yapılan değişiklikleri kaydetmek `OnPostAsync`.

Dizin ve ayrıntıları sayfaları alma ve HTTP GET yöntemiyle istenen verileri görüntüleme `OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>SingleOrDefaultAsync FirstOrDefaultAsync ile değiştir

Oluşturulan kodun kullanan [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) istenen varlığın getirilemedi. [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) tek bir varlık getirme sırasında daha verimli olur:

* Kod doğrulamak gerekmedikçe sorgudan döndürülen birden fazla varlık yok. 
* `SingleOrDefaultAsync` Daha fazla veri getirir ve gereksiz çalışır.
* `SingleOrDefaultAsync` Filtre bölümü uyan birden fazla varlık ise özel durum oluşturur.
*  `FirstOrDefaultAsync` Filtre bölümü uyan birden fazla varlık ise throw değil.

Genel olarak değiştirmek `SingleOrDefaultAsync` ile `FirstOrDefaultAsync`. `SingleOrDefaultAsync` 5 yerde kullanılır:

* `OnGetAsync` Ayrıntılar sayfası.
* `OnGetAsync` ve `OnPostAsync` düzenleme ve silme sayfalarında.

<a name="FindAsync"></a>
### <a name="findasync"></a>Zaman uyumsuz olarak bulur

Çok kurulmuş kod [zaman uyumsuz olarak bulur](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) yerine kullanılabilir `FirstOrDefaultAsync` veya `SingleOrDefaultAsync`. 

`FindAsync`:

* Bir varlığın birincil anahtarının (PK) bulur. BA sahip bir varlık bağlamı tarafından izleniyorsa, Veritabanına bir isteği bu olmadan döndürülür.
* Basit ve kısa olur.
* Tek bir varlık aramak için optimize edilmiştir.
* Bazı durumlarda perf avantajlara sahiptir, ancak nadiren normal web senaryoları için oyuna geldikleri.
* Örtük olarak kullanan [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) yerine [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Ancak, diğer varlıklar dahil etmek istediğiniz sonra bulma artık değil uygun. Başka bir deyişle, Bul bırakın ve bir sorgu uygulama ilerledikçe taşımak gerekebilir.

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

Gözat `Pages/Students` sayfası. **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/Öğrenciler / Index.cshtml* dosya.

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Ayrıntıları bağlantısını seçin. URL biçimidir `http://localhost:5000/Students/Details?id=2`. Bir sorgu dizesi kullanılarak geçirilmiş Öğrenci Kimliği (`?id=2`).

Düzenleme, Ayrıntılar ve Razor Sayfaları Sil kullanmak için güncelleştirme `"{id:int}"` rota şablonu. Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`.

İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** bir tamsayı rota değeri döndürür bir HTTP 404 (bulunamadı) hata içerir. Örneğin, `http://localhost:5000/Students/Details` 404 hatası döndürür. Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

Uygulamayı çalıştırın, bir ayrıntıları bağlantısına tıklayın ve URL rota verilerini kimliği geçirme doğrulayın (`http://localhost:5000/Students/Details/2`).

Genel değişiklik yapmayın `@page` için `@page "{id:int}"`, bunu sonları giriş bağlantılara yapılması ve sayfaları oluşturun.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>İlgili veri ekleme

Öğrenciler dizin sayfası için kurulmuş kod içermeyen `Enrollments` özelliği. Bu bölümde, içeriğini `Enrollments` koleksiyonu Ayrıntılar sayfasında görüntülenir.

`OnGetAsync` Yöntemi *Pages/Students/Details.cshtml.cs* kullanan `FirstOrDefaultAsync` tek bir alma yöntemi `Student` varlık. Aşağıdaki vurgulanmış kodu ekleyin:

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

`Include` Ve `ThenInclude` yöntemleri neden yüklemek bağlam `Student.Enrollments` gezinti özelliği ve her kayıt içinde `Enrollment.Course` gezinti özelliği. Bu yöntemleri ayrıntılı olarak okuma ilgili verileri öğreticide incelenir.

`AsNoTracking` Yöntemi varlıkları güncelleştirilmiyor geçerli bağlamda döndürüldüğünde senaryolarda performansı artırır. `AsNoTracking` Bu öğreticinin ilerleyen bölümlerinde ele alınmıştır.

### <a name="display-related-enrollments-on-the-details-page"></a>Ayrıntıları sayfasında ilgili kayıtları görüntüleme

Açık *Pages/Students/Details.cshtml*. Kayıtları listesini görüntülemek için aşağıdaki vurgulanmış kodu ekleyin:

 <!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Kod yapıştırılan sonra kod girintileme yanlışsa, düzeltmek için CTRL-K-D tuşuna basın.

Önceki kod varlıklarda döngü `Enrollments` gezinti özelliği. Her kayıt için indirmelere başlık ve sınıf görüntüler. İndirmelere başlık depolanan indirmelere varlık alınır `Course` kayıtları varlık gezinti özelliği.

Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** Öğrenci için bağlantı. Kurslar ve seçili Öğrenci dereceleri listesi görüntülenir.

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

Güncelleştirme `OnPostAsync` yönteminde *Pages/Students/Create.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

İncelemek [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kod:

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Önceki kod `TryUpdateModelAsync<Student>` güncelleştirmeyi dener `emptyStudent` gönderilen form değerleri kullanarak nesne [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinde [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync` Yalnızca listelenen özellikler güncelleştirmeleri (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Önceki örnekte:

* İkinci bağımsız değişken (` "student", // Prefix`) öneki değerleri aramak için kullanılır. Büyük küçük harfe duyarlı değildir.
* Gönderilen form değerleri türler dönüştürülür `Student` kullanarak model [model bağlama](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

Kullanarak `TryUpdateModel` overposting engellediğinden alanları gönderilen değerlerle güncelleştirmek için bir güvenlik en iyi uygulamadır. Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` , bu web sayfası ekleme veya döndürmemelidir güncelleştirme özelliği:

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Uygulama olmasa dahi bir `Secret` oluşturma/güncelleştirme Razor sayfasını, bir bilgisayar korsanının alan kümesi `Secret` overposting tarafından değeri. Bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri. Özgün kod Öğrenci örneğini oluşturduğunda, model Bağlayıcısı kullandığını alanları sınırlamak değil.

Ne olursa olsun değer için belirtilen korsan `Secret` form alanı DB'de güncelleştirilir. Fiddler aracı ekleme aşağıdaki görüntüde gösterir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

![Fiddler ekleme gizli alan](../ef-mvc/crud/_static/fiddler.png)

"OverPost" eklenen başarıyla değeri `Secret` eklenen satırın özelliği. Hiçbir zaman amaçlı Uygulama Tasarımcısı `Secret` Oluştur sayfası ayarlanacak özelliği.

<a name="vm"></a>
### <a name="view-model"></a>Görünüm modeli

Bir görünüm modeli genellikle uygulama tarafından kullanılan modelinde bulunan özellikler kümesini içerir. Uygulama modeli genellikle etki alanı modeli adı verilir. Etki alanı modeli genellikle veritabanında karşılık gelen varlığın gerektirdiği tüm özellikleri içerir. Görünüm modeli yalnızca UI katmanında (örneğin, Oluştur sayfası) için gereken özellikleri içerir. Görünüm modeli ek olarak, bazı uygulamalar, Razor sayfalarının sayfa model sınıfı ve tarayıcı arasında veri iletmek için bir bağlama modelini veya giriş modeli kullanın. Aşağıdakileri göz önünde bulundurun `Student` görünüm modeli:

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

Görünüm modelleri overposting önlemek için alternatif bir yol sağlar. Görünüm modeli (Görüntüle) görüntülemek veya güncelleştirmek için yalnızca özellikleri içerir.

Aşağıdaki kod `StudentVM` görünüm modeli yeni Öğrenci oluşturmak için:

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi diğerinden değerleri okuyarak bu nesnenin değerlerini ayarlar [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesnesi. `SetValues` özellik adıyla eşleşen kullanır. Görünüm model türü için model türü ilgili gerekmez, bu yalnızca eşleşen özelliklere sahip olmalıdır.

Kullanarak `StudentVM` gerektirir [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) kullanacak şekilde güncelleştirilmesi `StudentVM` yerine `Student`.

Razor sayfalarında `PageModel` türetilmiş sınıf, görünüm modeli. 

## <a name="update-the-edit-page"></a>Güncelleştirmeyi Düzenle sayfası

Düzen sayfası için sayfa modeli güncelleştirin. Önemli değişiklikler vurgulanmıştır:

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Kod değişiklikleri Oluştur sayfası birkaç istisna dışında benzerdir:

* `OnPostAsync` İsteğe bağlı bir sahip `id` parametresi.
* Geçerli Öğrenci DB'den getirilen boş Öğrenci oluşturmak yerine.
* `FirstOrDefaultAsync` ile değiştirilmiştir [zaman uyumsuz olarak bulur](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync` bir varlığın birincil anahtardan seçerken olduğunda iyi bir seçimdir. Bkz: [zaman uyumsuz olarak bulur](#FindAsync) daha fazla bilgi için.

### <a name="test-the-edit-and-create-pages"></a>Düzen sınamak ve sayfa oluşturma

Oluşturun ve birkaç Öğrenci varlıklar düzenleyin.

## <a name="entity-states"></a>Varlık durumları

Veritabanı bağlamını varlıkları bellekte DB karşılık gelen satırlarında ile eşit olup olmadığını izler. DB bağlamı eşitleme bilgileri ne olacağını belirler, `SaveChanges` olarak adlandırılır. Örneğin, ne zaman yeni bir varlık iletilir `Add` varlığın durumu kümesine yöntemi, `Added`. Zaman `SaveChanges` çağrılır, DB bağlamı sorunları SQL INSERT komutu.

Bir varlık şu durumlardan birinde olabilir:

* `Added`: Varlık DB'de henüz yok. `SaveChanges` Yöntemi INSERT deyimi verir.

* `Unchanged`: Hiçbir değişiklik bu varlıkla kaydedilmesi gerekir. DB'den okurken bir varlık bu durum vardır.

* `Modified`: Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş. `SaveChanges` Yöntemi bir güncelleştirme deyimi sorunları.

* `Deleted`: Varlık silinmek üzere işaretlendi. `SaveChanges` Yöntemi DELETE deyimi verir.

* `Detached`: Varlık DB bağlamı tarafından izleniyor değil.

Bir masaüstü uygulamasının durumu değişiklikleri genellikle otomatik olarak ayarlanır. Bir varlık okuma, değişiklik yapıldıysa ve otomatik olarak değiştirilmesi varlık durumu `Modified`. Çağırma `SaveChanges` yalnızca değiştirilen özellikler güncelleştirmeleri SQL UPDATE deyimi oluşturur.

Bir web uygulamasında `DbContext` bir varlık ve verileri bir sayfa oluşturulduğunda atıldı görüntüler okur. Bir sayfa zaman 's `OnPostAsync` yöntemi çağrıldığında, yeni bir web istek yapıldığında ve yeni bir örneğini ile `DbContext`. Bu yeni bağlam varlıkta yeniden okuma Masaüstü işleme benzetimini yapar.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

Bu bölümde, bir özel hata uygulamak için kod eklenir ne zaman ileti çağrısı `SaveChanges` başarısız olur. Olası hata iletilerini içeren bir dize ekleyin:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Değiştir `OnGetAsync` aşağıdaki kod ile yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Önceki kod isteğe bağlı bir parametre içeren `saveChangesError`. `saveChangesError` yöntem Öğrenci nesnesini silmek için bir hatadan sonra çağrıldı olup olmadığını gösterir. Silme işlemi, geçici ağ sorunları nedeniyle başarısız olabilir. Geçici ağ hataları bulutta daha yüksektir. `saveChangesError`yanlış olduğunda Delete sayfa `OnGetAsync` kullanıcı Arabiriminden olarak adlandırılır. Zaman `OnGetAsync` tarafından çağrılır `OnPostAsync` (silme işlemi başarısız oldu çünkü), `saveChangesError` parametresi doğrudur.

### <a name="the-delete-pages-onpostasync-method"></a>Delete sayfaları OnPostAsync yöntemi

Değiştir `OnPostAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Seçilen varlığın önceki kod alır sonra çağırır `Remove` varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır SQL DELETE komutu oluşturulur. Varsa `Remove` başarısız olur:

* DB özel durum yakalandı.
* Delete sayfaları `OnGetAsync` yöntemi ile çağrılır `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Delete Razor sayfasını güncelleştir

Aşağıdaki vurgulanan hata iletisini silme Razor sayfasına ekleyin.

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Delete sınayın.

## <a name="common-errors"></a>Sık karşılaşılan hataları

Öğrenci/Home veya diğer bağlantılar çalışmıyor:

Razor sayfasını içeren doğru doğrulayın `@page` yönergesi. Örneğin, Öğrenci/giriş Razor sayfa gereken **değil** rota şablonu içerir:

```cshtml
@page "{id:int}"
```

Her Razor sayfasını içermelidir `@page` yönergesi.

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/intro)
> [sonraki](xref:data/ef-rp/sort-filter-page)
