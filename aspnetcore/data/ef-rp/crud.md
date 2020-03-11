---
title: ASP.NET Core-CRUD-2 ' de EF Core ile Razor Pages
author: rick-anderson
description: EF Core oluşturma, okuma, güncelleştirme, silme işlemlerinin nasıl yapılacağını gösterir.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/crud
ms.openlocfilehash: 05519852fab22bd3ad5b77e3494b49191448286f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665650"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>ASP.NET Core-CRUD-2 ' de EF Core ile Razor Pages

, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide, scafkatan CRUD (oluşturma, okuma, güncelleştirme, silme) kodu incelenir ve özelleştirilir.

## <a name="no-repository"></a>Depo yok

Bazı geliştiriciler, Kullanıcı arabirimi (Razor Pages) ve veri erişim katmanı arasında bir soyutlama katmanı oluşturmak için bir hizmet katmanı veya depo deseninin kullanılmasını sağlar. Bu öğretici bunu yapmaz. Karmaşıklığı en aza indirmek ve öğreticiyi EF Core odaklanmasını sağlamak için EF Core kodu doğrudan sayfa modeli sınıflarına eklenir. 

## <a name="update-the-details-page"></a>Ayrıntılar sayfasını Güncelleştir

Öğrenciler sayfaları için yapı iskelesi kodu kayıt verilerini içermez. Bu bölümde, ayrıntılar sayfasına kayıtları eklersiniz.

### <a name="read-enrollments"></a>Kayıtları oku

Sayfada öğrenciye ait kayıt verilerini göstermek için, onu okumanız gerekir. *Pages/öğrenciler/details. cshtml. cs* içindeki scafkatlama kodu, kayıt verileri olmadan yalnızca öğrenci verilerini okur:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/Details1.cshtml.cs?name=snippet_OnGetAsync&highlight=8)]

Seçili öğrenci için kayıt verilerini okumak üzere `OnGetAsync` yöntemini aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Details.cshtml.cs?name=snippet_OnGetAsync&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) ve [thenınclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) yöntemleri, bağlamın `Student.Enrollments` gezinti özelliğini yüklemesine ve her kaydın `Enrollment.Course` gezinti özelliğine neden olur. Bu yöntemler, [okuma ilgili verileri](xref:data/ef-rp/read-related-data) öğreticisinde ayrıntılı olarak incelendi.

[Asnotracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) yöntemi, döndürülen varlıkların geçerli bağlamda güncelleştirilmediği senaryolarda performansı geliştirir. `AsNoTracking`, Bu öğreticinin ilerleyen kısımlarında ele alınmıştır.

### <a name="display-enrollments"></a>Kayıtları görüntüle

*Sayfalar/öğrenciler/details. cshtml* içindeki kodu aşağıdaki kodla değiştirin ve kayıtlar listesini görüntüleyin. Değişiklikler vurgulanır.

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Details.cshtml?highlight=32-53)]

Yukarıdaki kod, `Enrollments` Gezinti özelliğindeki varlıklar aracılığıyla döngü yapılır. Her kayıt için kurs başlığını ve sınıfı görüntüler. Kurs başlığı, kayıt varlığının `Course` gezinti özelliğinde depolanan kurs varlığından alınır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve bir öğrenci için **Ayrıntılar** bağlantısına tıklayın. Seçili öğrenci için Kurslar ve notlar listesi görüntülenir.

### <a name="ways-to-read-one-entity"></a>Bir varlığı okuma yolları

Oluşturulan kod, bir varlığı okumak için [Firstordefaultasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) kullanır. Bu yöntem, hiçbir şey bulunamazsa null değerini döndürür; Aksi takdirde, sorgu filtresi ölçütlerine uyan bulunan ilk satırı döndürür. `FirstOrDefaultAsync`, genellikle aşağıdaki alternatiflere göre daha iyi bir seçimdir:

* [Singleordefaultasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) -sorgu filtresini karşılayan birden fazla varlık varsa bir özel durum oluşturur. Sorgu tarafından birden fazla satır döndürülüp döndürülmeyeceğini anlamak için `SingleOrDefaultAsync` birden çok satır getirmeye çalışır. Sorgu yalnızca bir varlık döndürebiliyorsanız ve benzersiz bir anahtarda arama yaptığında bu ek çalışma gereksizdir.
* [Findadsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) -birincil ANAHTARLA (PK) bir varlık bulur. PK 'ye sahip bir varlık bağlam tarafından izleniyorsa, veritabanına bir istek olmadan döndürülür. Bu yöntem tek bir varlık aramak için en iyi duruma getirilmiştir, ancak `FindAsync``Include` çağıramaz.  Bu nedenle, ilgili veriler gerekliyse `FirstOrDefaultAsync` daha iyi bir seçenektir.

### <a name="route-data-vs-query-string"></a>Veri yönlendirme ve sorgu dizesi karşılaştırması

Ayrıntılar sayfasının URL 'SI `https://localhost:<port>/Students/Details?id=1`. Varlığın birincil anahtar değeri, sorgu dizesinde bulunur. Bazı geliştiriciler anahtar değerini rota verilerinde geçirmeye tercih eder: `https://localhost:<port>/Students/Details/1`. Daha fazla bilgi için bkz. [oluşturulan kodu güncelleştirme](xref:tutorials/razor-pages/da1#update-the-generated-code).

## <a name="update-the-create-page"></a>Oluştur sayfasını Güncelleştir

Oluşturma sayfasına yönelik scafkatlama `OnPostAsync` kodu [fazla deftere nakme](#overposting)karşı savunmasız olur. *Pages/öğrenciler/Create. cshtml. cs* içindeki `OnPostAsync` yöntemini aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Yukarıdaki kod bir öğrenci nesnesi oluşturur ve ardından, öğrenci nesnesinin özelliklerini güncelleştirmek için, postalanan form alanlarını kullanır. [Tryupdatemodelasync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) yöntemi:

* [Pagemodel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)Içindeki [pagecontext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinden gönderilen form değerlerini kullanır.
* Yalnızca listelenen özellikleri güncelleştirir (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).
* "Öğrenci" ön ekine sahip form alanlarını arar. Örneğin, `Student.FirstMidName`. Büyük/küçük harfe duyarlı değildir.
* , Dizelerdeki form değerlerini `Student` modelindeki türlere dönüştürmek için [model bağlama](xref:mvc/models/model-binding) sistemini kullanır. Örneğin, `EnrollmentDate` DateTime 'a dönüştürülmelidir.

Uygulamayı çalıştırın ve oluştur sayfasını test etmek için bir öğrenci varlığı oluşturun.

## <a name="overposting"></a>Fazla nakil

Deftere nakledilen değerler içeren alanları güncelleştirmek için `TryUpdateModel` kullanmak en iyi güvenlik yöntemidir çünkü fazla nakletmeyi önler. Örneğin, öğrenci varlığının bu Web sayfasının güncelleştirmesi veya eklemesi gereken bir `Secret` özelliğini içerdiğini varsayalım:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Uygulama, oluşturma veya güncelleştirme Razor sayfasında bir `Secret` alanına sahip olmasa da, bir korsan `Secret` değerini aşırı nakme göre ayarlayabilir. Bir korsan, Fiddler gibi bir araç kullanabilir veya bir `Secret` form değeri göndermek için JavaScript 'i yazabilir. Özgün kod, bir öğrenci örneği oluştururken model cildin kullandığı alanları sınırlamaz.

`Secret` form alanı için belirtilen korsanın hangi değeri veritabanında güncelleştirirler. Aşağıdaki görüntüde, `Secret` alanı ("OverPost" değeri ile), postalanan form değerlerine eklenen Fiddler aracı gösterilmektedir.

![Fiddler gizli alanı ekleniyor](../ef-mvc/crud/_static/fiddler.png)

"OverPost" değeri eklenen satırın `Secret` özelliğine başarıyla eklendi. Bu, Uygulama Tasarımcısı hiçbir şekilde `Secret` özelliğini hiçbir şekilde hiçbir şekilde hiçbir şekilde hiçbir şekilde hiçbir şekilde hedeflenmemiş olsa da oluşur.

### <a name="view-model"></a>Modeli görüntüle

Model görüntüleme, fazla nakletmeyi önlemenin alternatif bir yolunu sağlar.

Uygulama modeli genellikle etki alanı modeli olarak adlandırılır. Etki alanı modeli genellikle veritabanında ilgili varlık için gereken tüm özellikleri içerir. Görünüm modeli yalnızca için kullanılan Kullanıcı arabirimi için gereken özellikleri içerir (örneğin, oluşturma sayfası).

Görünüm modeline ek olarak, bazı uygulamalar Razor Pages sayfa modeli sınıfı ve tarayıcı arasında veri geçirmek için bir bağlama modeli veya giriş modeli kullanır. 

Aşağıdaki `Student` görüntüleme modelini göz önünde bulundurun:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentVM.cs)]

Aşağıdaki kod, yeni bir öğrenci oluşturmak için `StudentVM` görünüm modelini kullanır:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi, başka bir [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesnesinden değerleri okuyarak bu nesnenin değerlerini ayarlar. `SetValues`, özellik adı eşleştirme kullanır. Görünüm modeli türünün model türüyle ilgili olması gerekmez, yalnızca eşleşen özellikleri olması gerekir.

`StudentVM` kullanmak [Create. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml) 'in `Student`yerine `StudentVM` kullanacak şekilde güncelleştirilmesini gerektirir.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

*Pages/öğrenciler/Edit. cshtml. cs*dosyasında `OnGetAsync` ve `OnPostAsync` yöntemlerini aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Edit.cshtml.cs?name=snippet_OnGetPost)]

Kod değişiklikleri, birkaç özel durum dışında oluşturma sayfasına benzerdir:

* `FirstOrDefaultAsync` [Findadsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync)ile değiştirilmiştir. İlgili verileri dahil etmeniz gerekmiyorsa `FindAsync` daha etkilidir.
* `OnPostAsync` bir `id` parametresine sahiptir.
* Geçerli öğrenci boş bir öğrenci oluşturmak yerine veritabanından getirilir.

Uygulamayı çalıştırın ve bir öğrenci oluşturup düzenleyerek test edin.

## <a name="entity-states"></a>Varlık durumları

Veritabanı bağlamı, bellekteki varlıkların veritabanında karşılık gelen satırlarıyla eşitlenmiş olup olmadığını izler. Bu izleme bilgileri, [Savechangesasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrıldığında ne olacağını belirler. Örneğin, yeni bir varlık [Addadsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) yöntemine geçirildiğinde, bu varlığın durumu [eklendi](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added)olarak ayarlanır. `SaveChangesAsync` çağrıldığında, veritabanı bağlamı bir SQL INSERT komutu yayınlar.

Bir varlık [aşağıdaki durumlardan](/dotnet/api/microsoft.entityframeworkcore.entitystate)birinde olabilir:

* `Added`: varlık veritabanında henüz yok. `SaveChanges` yöntemi bir INSERT ifadesini yayınlar.

* `Unchanged`: bu varlıkla birlikte hiçbir değişiklik kaydedilmesi gerekmiyor. Bir varlık veritabanından okurken bu durumu içerir.

* `Modified`: varlığın özellik değerlerinin bazıları veya tümü değiştirildi. `SaveChanges` yöntemi bir UPDATE ifadesini yayınlar.

* `Deleted`: varlık silinmek üzere işaretlendi. `SaveChanges` yöntemi bir DELETE ifadesini yayınlar.

* `Detached`: varlık veritabanı bağlamı tarafından izlenmiyor.

Bir masaüstü uygulamasında durum değişiklikleri genellikle otomatik olarak ayarlanır. Bir varlık okundu, değişiklikler yapılır ve varlık durumu otomatik olarak `Modified`olarak değiştirilir. `SaveChanges` çağırmak yalnızca değiştirilen özellikleri güncelleştiren bir SQL UPDATE bildirisi oluşturur.

Bir Web uygulamasında, bir varlığı okuyan ve verileri görüntüleyen `DbContext` bir sayfa işlendikten sonra atılmış olur. Bir sayfanın `OnPostAsync` yöntemi çağrıldığında yeni bir Web isteği yapılır ve `DbContext`yeni bir örneği ile yapılır. Okuyarak bu yeni bağlamdaki varlığı, masaüstü işlemesini benzetir.

## <a name="update-the-delete-page"></a>Silme sayfası

Bu bölümde, `SaveChanges` çağrısı başarısız olduğunda özel bir hata iletisi uygulayacağınızı görürsünüz.

*Pages/öğrenciler/delete. cshtml. cs* dosyasındaki kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır (`using` deyimlerini temizleme dışında).

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Delete.cshtml.cs?name=snippet_All&highlight=20,22,30,38-41,53-71)]

Yukarıdaki kod, `OnGetAsync` yöntemi imzasına `saveChangesError` isteğe bağlı parametresini ekler. `saveChangesError`, öğrenci nesnesini silme hatasından sonra yöntemin çağrılıp çağrılmadığını gösterir. Geçici ağ sorunları nedeniyle silme işlemi başarısız olabilir. Geçici ağ hataları, veritabanı bulutta olduğunda daha olasıdır. Kullanıcı arabiriminden silme sayfası `OnGetAsync` çağrıldığında `saveChangesError` parametresi false 'tur. `OnGetAsync` `OnPostAsync` tarafından çağrıldığında (silme işlemi başarısız olduğundan), `saveChangesError` parametresi true olur.

`OnPostAsync` yöntemi seçili varlığı alır, ardından varlığın durumunu `Deleted`olarak ayarlamak için [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) yöntemini çağırır. `SaveChanges` çağrıldığında, bir SQL DELETE komutu oluşturulur. `Remove` başarısız olursa:

* Veritabanı özel durumu yakalandı.
* Sayfaları sil `OnGetAsync` yöntemi `saveChangesError=true`ile çağırılır.

Razor silme sayfasına bir hata iletisi ekleyin (*Sayfalar/öğrenciler/delete. cshtml*):

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Delete.cshtml?highlight=10)]

Uygulamayı çalıştırın ve Sil sayfasını test etmek için bir öğrenci silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/intro)
> [sonraki öğretici](xref:data/ef-rp/sort-filter-page)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide, scafkatan CRUD (oluşturma, okuma, güncelleştirme, silme) kodu incelenir ve özelleştirilir.

Karmaşıklığı en aza indirmek ve bu öğreticilerin EF Core odaklanmasını sağlamak için, EF Core kod sayfa modellerinde kullanılır. Bazı geliştiriciler, Kullanıcı arabirimi (Razor Pages) ve veri erişim katmanı arasında bir soyutlama katmanı oluşturmak için bir hizmet katmanını veya depo modelini kullanır.

Bu öğreticide, *öğrenciler* klasöründeki oluşturma, düzenleme, silme ve Ayrıntılar Razor Pages incelenir.

Scafkatlanmış kod, sayfa oluşturma, düzenleme ve silme için aşağıdaki kalıbı kullanır:

* HTTP GET yöntemi `OnGetAsync`istenen verileri alın ve görüntüleyin.
* HTTP POST yöntemiyle verilerde yapılan değişiklikleri kaydedin `OnPostAsync`.

Dizin ve ayrıntı sayfaları, HTTP GET yöntemiyle istenen verileri alır ve görüntüler `OnGetAsync`

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync ile FirstOrDefaultAsync

Oluşturulan kod, genellikle [Singleordefaultasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)üzerinden tercih edilen [firstordefaultasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)kullanır.

 `FirstOrDefaultAsync` bir varlık getirilirken `SingleOrDefaultAsync` daha verimlidir:

* Kodun sorgudan döndürülen birden fazla varlık olmadığını doğrulaması gerekmiyorsa.
* `SingleOrDefaultAsync` daha fazla veri getirir ve gereksiz çalışma yapmaz.
* `SingleOrDefaultAsync`, filtre bölümüne uyan birden fazla varlık varsa bir özel durum oluşturur.
* Filtre bölümüne uyan birden fazla varlık varsa `FirstOrDefaultAsync` oluşturmaz.

<a name="FindAsync"></a>

### <a name="findasync"></a>Findadsync

Yapı iskelesi kodunun büyük bir kısmında, `FirstOrDefaultAsync`yerine [Findadsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) kullanılabilir.

`FindAsync`:

* Birincil anahtarla (PK) bir varlık bulur. PK 'ye sahip bir varlık bağlam tarafından izleniyorsa, VERITABANıNA bir istek olmadan döndürülür.
* Basittir ve kısadır.
* Tek bir varlığı aramak için iyileştirilmiştir.
* Bazı durumlarda performans avantajlarına sahip olabilir, ancak tipik Web uygulamaları için nadiren meydana gelir.
* , [Maleasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)yerine dolaylı olarak [firstasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) kullanır.

Ancak başka varlıklar `Include` istiyorsanız `FindAsync` artık uygun değildir. Bu, `FindAsync` iptal etmeniz ve uygulamanız ilerledikçe bir sorguya taşımanız gerekebileceği anlamına gelir.

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

`Pages/Students` sayfasına gidin. **Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/öğrenciler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Uygulamayı çalıştırın ve bir **Ayrıntılar** bağlantısı seçin. URL `http://localhost:5000/Students/Details?id=2`formundadır. Öğrenci KIMLIĞI bir sorgu dizesi (`?id=2`) kullanılarak geçirilir.

`"{id:int}"` Route şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin. Bu sayfaların her biri için Page yönergesini `@page "{id:int}"``@page` değiştirin.

Bir tamsayı yol **değeri içermeyen "** {id: Int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor. Örneğin, `http://localhost:5000/Students/Details` bir 404 hatası döndürür. KIMLIĞI isteğe bağlı yapmak için `?` yol kısıtlamasına ekleyin:

 ```cshtml
@page "{id:int?}"
```

Uygulamayı çalıştırın, Ayrıntılar bağlantısına tıklayın ve URL 'nin KIMLIĞI yönlendirme verileri (`http://localhost:5000/Students/Details/2`) olarak geçirdiğini doğrulayın.

`@page` `@page "{id:int}"`genel olarak değiştirmeyin, böylece giriş ve sayfa oluşturma bağlantıları kesilir.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>İlgili verileri ekleme

Öğrenciler dizin sayfasına yönelik scafkatlanmış kod `Enrollments` özelliğini içermez. Bu bölümde, `Enrollments` koleksiyonun içeriği ayrıntılar sayfasında görüntülenir.

*Pages/öğrenciler/details. cshtml. cs* `OnGetAsync` yöntemi, tek bir `Student` varlığı almak için `FirstOrDefaultAsync` metodunu kullanır. Aşağıdaki vurgulanmış kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) ve [thenınclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) yöntemleri, bağlamın `Student.Enrollments` gezinti özelliğini yüklemesine ve her kaydın `Enrollment.Course` gezinti özelliğine neden olur. Bu yöntemler, okuma ile ilgili veri öğreticisinde ayrıntılı olarak incelenmelidir.

[Asnotracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) yöntemi, döndürülen varlıkların geçerli bağlamda güncelleştirilmediği durumlarda, senaryolarda performansı geliştirir. `AsNoTracking`, Bu öğreticinin ilerleyen kısımlarında ele alınmıştır.

### <a name="display-related-enrollments-on-the-details-page"></a>Ayrıntılar sayfasında ilgili kayıtları görüntüleme

*Sayfaları/öğrencileri/ayrıntıları. cshtml*'yi açın. Kayıtlar listesini göstermek için aşağıdaki vurgulanmış kodu ekleyin:

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

Kod yapıştırıldıktan sonra kod girintisi yanlışsa, düzeltmek için CTRL-K-D ' a basın.

Yukarıdaki kod, `Enrollments` Gezinti özelliğindeki varlıklar aracılığıyla döngü yapılır. Her kayıt için kurs başlığını ve sınıfı görüntüler. Kurs başlığı, kayıt varlığının `Course` gezinti özelliğinde depolanan kurs varlığından alınır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve bir öğrenci için **Ayrıntılar** bağlantısına tıklayın. Seçili öğrenci için Kurslar ve notlar listesi görüntülenir.

## <a name="update-the-create-page"></a>Oluştur sayfasını Güncelleştir

*Sayfalardaki/öğrenciler/Create. cshtml. cs* dosyasındaki `OnPostAsync` yöntemi aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

[Tryupdatemodelasync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodunu inceleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Yukarıdaki kodda `TryUpdateModelAsync<Student>`, [Pagemodel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)Içindeki [pagecontext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinden gönderilen form değerlerini kullanarak `emptyStudent` nesnesini güncelleştirmeye çalışır. `TryUpdateModelAsync` yalnızca listelenen özellikleri güncelleştirir (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Yukarıdaki örnekte:

* İkinci bağımsız değişken (`"student", // Prefix`) ön ek değerleri aramak için kullanılır. Büyük/küçük harfe duyarlı değildir.
* Postalanan form değerleri, [model bağlama](xref:mvc/models/model-binding)kullanılarak `Student` modelindeki türlere dönüştürülür.

<a id="overpost"></a>

### <a name="overposting"></a>Fazla nakil

Deftere nakledilen değerler içeren alanları güncelleştirmek için `TryUpdateModel` kullanmak en iyi güvenlik yöntemidir çünkü fazla nakletmeyi önler. Örneğin, öğrenci varlığının bu Web sayfasının güncelleştirmesi veya eklemesi gereken bir `Secret` özelliğini içerdiğini varsayalım:

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Uygulama, oluşturma/güncelleştirme Razor sayfasında bir `Secret` alanına sahip olmasa da, bir korsan `Secret` değerini aşırı nakme ile ayarlayabilir. Bir korsan, Fiddler gibi bir araç kullanabilir veya bir `Secret` form değeri göndermek için JavaScript 'i yazabilir. Özgün kod, bir öğrenci örneği oluştururken model cildin kullandığı alanları sınırlamaz.

`Secret` form alanı için belirtilen korsanın değeri, VERITABANıNDA güncellenir. Aşağıdaki görüntüde, `Secret` alanı ("OverPost" değeri ile), postalanan form değerlerine eklenen Fiddler aracı gösterilmektedir.

![Fiddler gizli alanı ekleniyor](../ef-mvc/crud/_static/fiddler.png)

"OverPost" değeri eklenen satırın `Secret` özelliğine başarıyla eklendi. Uygulama Tasarımcısı `Secret` özelliğini hiçbir şekilde hiçbir şekilde hedeflenmiştir.

<a name="vm"></a>

### <a name="view-model"></a>Modeli görüntüle

Bir görünüm modeli, genellikle uygulama tarafından kullanılan modelde bulunan özelliklerin bir alt kümesini içerir. Uygulama modeli genellikle etki alanı modeli olarak adlandırılır. Etki alanı modeli genellikle VERITABANıNDAKI karşılık gelen varlık için gereken tüm özellikleri içerir. Görünüm modeli yalnızca UI katmanı için gereken özellikleri içerir (örneğin, Oluştur sayfası). Görünüm modeline ek olarak, bazı uygulamalar Razor Pages sayfa modeli sınıfı ve tarayıcı arasında veri geçirmek için bir bağlama modeli veya giriş modeli kullanır. Aşağıdaki `Student` görüntüleme modelini göz önünde bulundurun:

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Model görüntüleme, fazla nakletmeyi önlemenin alternatif bir yolunu sağlar. Görünüm modeli yalnızca görüntüleme (görüntüleme) veya güncelleştirme özelliklerini içerir.

Aşağıdaki kod, yeni bir öğrenci oluşturmak için `StudentVM` görünüm modelini kullanır:

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi, başka bir [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesnesinden değerleri okuyarak bu nesnenin değerlerini ayarlar. `SetValues`, özellik adı eşleştirme kullanır. Görünüm modeli türünün model türüyle ilgili olması gerekmez, yalnızca eşleşen özellikleri olması gerekir.

`StudentVM` kullanmak [Createvm gerektirir. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) `Student`yerine `StudentVM` kullanacak şekilde güncelleştirilir.

Razor Pages, `PageModel` türetilmiş sınıf görünüm modelidir.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

Düzenleme sayfasının sayfa modelini güncelleştirin. Büyük değişiklikler vurgulanır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Kod değişiklikleri, birkaç özel durum dışında oluşturma sayfasına benzerdir:

* `OnPostAsync` isteğe bağlı bir `id` parametresine sahiptir.
* Geçerli öğrenci boş bir öğrenci oluşturmak yerine DB 'den getirilir.
* `FirstOrDefaultAsync` [Findadsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync)ile değiştirilmiştir. birincil anahtardan bir varlık seçerken `FindAsync` iyi bir seçenektir. Daha fazla bilgi için bkz. [Findadsync](#FindAsync) .

### <a name="test-the-edit-and-create-pages"></a>Düzenleme ve oluşturma sayfalarını test etme

Birkaç öğrenci varlığı oluşturun ve düzenleyin.

## <a name="entity-states"></a>Varlık durumları

DB bağlamı, bellekteki varlıkların VERITABANıNDA karşılık gelen satırlarıyla eşitlenmiş olup olmadığını izler. VERITABANı bağlamı eşitleme bilgileri, [Savechangesasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrıldığında ne olacağını belirler. Örneğin, yeni bir varlık [Addadsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) yöntemine geçirildiğinde, bu varlığın durumu [eklendi](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added)olarak ayarlanır. `SaveChangesAsync` çağrıldığında, DB bağlamı bir SQL INSERT komutu yayınlar.

Bir varlık [aşağıdaki durumlardan](/dotnet/api/microsoft.entityframeworkcore.entitystate)birinde olabilir:

* `Added`: varlık VERITABANıNDA henüz yok. `SaveChanges` yöntemi bir INSERT ifadesini yayınlar.

* `Unchanged`: bu varlıkla birlikte hiçbir değişiklik kaydedilmesi gerekmiyor. Bir varlık, DB 'den okurken bu duruma sahip olur.

* `Modified`: varlığın özellik değerlerinin bazıları veya tümü değiştirildi. `SaveChanges` yöntemi bir UPDATE ifadesini yayınlar.

* `Deleted`: varlık silinmek üzere işaretlendi. `SaveChanges` yöntemi bir DELETE ifadesini yayınlar.

* `Detached`: varlık DB bağlamı tarafından izlenmiyor.

Bir masaüstü uygulamasında durum değişiklikleri genellikle otomatik olarak ayarlanır. Bir varlık okundu, değişiklikler yapılır ve varlık durumu otomatik olarak `Modified`olarak değiştirilecek. `SaveChanges` çağırmak yalnızca değiştirilen özellikleri güncelleştiren bir SQL UPDATE bildirisi oluşturur.

Bir Web uygulamasında, bir varlığı okuyan ve verileri görüntüleyen `DbContext` bir sayfa işlendikten sonra atılmış olur. Bir sayfanın `OnPostAsync` yöntemi çağrıldığında yeni bir Web isteği yapılır ve `DbContext`yeni bir örneği ile yapılır. Yeni bağlamdaki varlığı yeniden okumak masaüstü işlemesini benzetir.

## <a name="update-the-delete-page"></a>Silme sayfası

Bu bölümde, `SaveChanges` çağrısı başarısız olduğunda özel bir hata iletisi uygulamak için kod eklenir. Olası hata iletilerini içeren bir dize ekleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

`OnGetAsync` yöntemini aşağıdaki kod ile değiştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Yukarıdaki kod `saveChangesError`isteğe bağlı parametresini içerir. `saveChangesError`, öğrenci nesnesini silme hatasından sonra yöntemin çağrılıp çağrılmadığını gösterir. Geçici ağ sorunları nedeniyle silme işlemi başarısız olabilir. Geçici ağ hataları, bulutta daha olasıdır. Kullanıcı arabiriminden silme sayfası `OnGetAsync` çağrıldığında `saveChangesError`false 'tur. `OnGetAsync` `OnPostAsync` tarafından çağrıldığında (silme işlemi başarısız olduğundan), `saveChangesError` parametresi true olur.

### <a name="the-delete-pages-onpostasync-method"></a>Sayfaları sil OnPostAsync yöntemi

`OnPostAsync` aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Önceki kod seçili varlığı alır, ardından varlığın durumunu `Deleted`olarak ayarlamak için [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) yöntemini çağırır. `SaveChanges` çağrıldığında, bir SQL DELETE komutu oluşturulur. `Remove` başarısız olursa:

* DB özel durumu yakalandı.
* Sayfaları sil `OnGetAsync` yöntemi `saveChangesError=true`ile çağırılır.

### <a name="update-the-delete-razor-page"></a>Razor Sil sayfasını Güncelleştir

Aşağıdaki Vurgulanan hata iletisini Razor Sil sayfasına ekleyin.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Test silme.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Öğrenciler/dizin veya diğer bağlantılar çalışmaz:

Razor sayfasında doğru `@page` yönergesini içerdiğini doğrulayın. Örneğin, öğrenciler/Dizin Razor sayfası bir yol şablonu **içermemelidir:**

```cshtml
@page "{id:int}"
```

Her Razor sayfası `@page` yönergesini içermelidir.



## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=K4X1MT2jt6o)

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/intro)
> [İleri](xref:data/ef-rp/sort-filter-page)

::: moniker-end
