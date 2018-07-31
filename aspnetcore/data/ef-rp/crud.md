---
title: ASP.NET core'da - CRUD - 2 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Oluşturma, okuma, güncelleştirme ve EF Core ile silme işlemini gösterir
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: e3a0ec2e21ae9e9eeaae1eb7c17f1604897fb6f9
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342464"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>ASP.NET core'da - CRUD - 2 8 EF çekirdekli Razor sayfaları

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Bu öğreticide, iskele kurulmuş CRUD (oluşturma, okuma, güncelleştirme ve silme) kod inceleme ve özelleştirilmiş.

Karmaşıklığı en aza indirmek ve EF Core üzerine odaklanan bu öğreticileri tutmak için EF Core kod sayfası modellerinde kullanılır. Bazı geliştiriciler bir hizmet katmanı kullanmak veya [depo deseni](xref:fundamentals/repository-pattern) kullanıcı arabirimini (Razor sayfaları) ve veri erişim katmanı arasında bir Soyutlama Katmanı oluşturmak için.

Bu öğreticide, oluştur, Düzenle, Sil ve ayrıntıları Razor sayfaları *Öğrenci* klasör incelenir.

İskele kurulan kodu oluştur, Düzenle ve Sil sayfaları için şu biçimi kullanır:

* Alma ve HTTP GET yöntemi ile istenen veri görüntüleme `OnGetAsync`.
* HTTP POST yöntemi verilerde yapılan değişiklikleri kaydetmek `OnPostAsync`.

Dizin ve ayrıntıları sayfaları almak ve istenen verileri HTTP GET yöntemi ile görüntüleme `OnGetAsync`

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync vs FirstOrDefaultAsync

Oluşturulan kod [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), genellikle tercih üzerinden olduğu [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

 `FirstOrDefaultAsync` Daha fazla verimlidir `SingleOrDefaultAsync` bir varlık alma sırasında:

* Kodu doğrulayın gerek duymadığı sürece sorgusundan döndürülen birden fazla varlık yok.
* `SingleOrDefaultAsync` Daha fazla veri getirir ve gereksiz çalışır.
* `SingleOrDefaultAsync` Sorgunuzun filtre bölümü uygun birden fazla varlık ise bir özel durum oluşturur.
* `FirstOrDefaultAsync` Sorgunuzun filtre bölümü uygun birden fazla varlık ise oluşturmaz.

<a name="FindAsync"></a>

### <a name="findasync"></a>FindAsync

İskele kurulan kodu çoğunu içinde [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) yerine kullanılan `FirstOrDefaultAsync`.

`FindAsync`:

* Bir varlığın birincil anahtarla (PK) bulur. Bir varlıkla PK bağlam tarafından izleniyorsa, Veritabanına gerek olmadan isteği döndürülür.
* Basit ve kısa maliyetlidir.
* Tek bir varlığı aramak için optimize edilmiştir.
* Bazı durumlarda performans yararları olabilir, ancak bunlar nadiren gerçekleşir normal web apps için.
* Örtülü olarak kullanan [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) yerine [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

Ancak isterseniz `Include` diğer varlıklar, ardından `FindAsync` artık uygun değil. Bu iptal gerekebilir anlamına gelir `FindAsync` ve bir sorgu için uygulama ilerledikçe taşıyın.

## <a name="customize-the-details-page"></a>Ayrıntılar sayfasını özelleştirme

Gözat `Pages/Students` sayfası. **Düzenle**, **ayrıntıları**, ve **Sil** tarafından oluşturulan bağlantıları [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/Öğrenciler / Index.cshtml* dosya.

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Uygulamayı çalıştırmak ve seçmek bir **ayrıntıları** bağlantı. URL biçimindedir `http://localhost:5000/Students/Details?id=2`. Öğrenci Kimliği, sorgu dizesi kullanılarak geçirilir (`?id=2`).

Düzenle, Ayrıntılar ve Sil Razor sayfaları kullanmaya güncelleştirme `"{id:int}"` rota şablonu. Her biri bu sayfaları için sayfa yönergesi değiştirme `@page` için `@page "{id:int}"`.

"{Kimliği: int}" rota şablonuyla yapan sayfasına bir istek **değil** tamsayı rota değeri döndürür bir HTTP 404 (bulunamadı) hatası içerir. Örneğin, `http://localhost:5000/Students/Details` 404 hatası döndürür. Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

Uygulamayı çalıştırmak için bir ayrıntıları bağlantıya tıkladığınızda URL rota verilerini kimliği geçirerek doğrulayın (`http://localhost:5000/Students/Details/2`).

Genel olarak değiştirme `@page` için `@page "{id:int}"`, bunu sonu bağlantılar giriş yapmak ve sayfaları oluşturun.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>İlgili veri ekleme

Öğrenciler dizin sayfası için iskele kurulmuş kod içermeyen `Enrollments` özelliği. Bu bölümde, içeriğini `Enrollments` koleksiyon Ayrıntıları sayfasında görüntülenir.

`OnGetAsync` Yöntemi *Pages/Students/Details.cshtml.cs* kullanan `FirstOrDefaultAsync` yönteminin tek bir almak için `Student` varlık. Aşağıdaki vurgulanmış kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

[INCLUDE](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) ve [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) yöntemlerinin neden yüklemek bağlam `Student.Enrollments` gezinme özelliği ve her kaydı `Enrollment.Course` gezinme özelliği. Bu yöntemler, ilgili okuma veri öğreticide ayrıntılı olarak incelenir.

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) yöntemi varlıkları güncelleştirilmez geçerli bağlamda döndürüldüğünde senaryolarda performansı artırır. `AsNoTracking` Bu öğreticinin sonraki bölümlerinde ele alınmıştır.

### <a name="display-related-enrollments-on-the-details-page"></a>Ayrıntılar sayfasında ilgili kayıtları görüntüle

Açık *Pages/Students/Details.cshtml*. Kayıtları bir listesini görüntülemek için aşağıdaki vurgulanmış kodu ekleyin:

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

Kod girintileme kodu yapıştırılır sonra yanlışsa, düzeltmek için CTRL-K-D tuşlarına basın.

Yukarıdaki kod, varlıklarda döngü `Enrollments` gezinme özelliği. Her kayıt için bu kurs başlığı ve sınıf görüntüler. Kurs başlığı depolanan kurs varlık alınır `Course` kayıtları varlık gezinme özelliği.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** bir öğrenci bağlantısı. Kursları ve notlarınızı için seçilen Öğrenci listesi görüntülenir.

## <a name="update-the-create-page"></a>Güncelleştirme Oluştur sayfası

Güncelleştirme `OnPostAsync` yönteminde *Pages/Students/Create.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

İnceleme [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kod:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Önceki kodda, `TryUpdateModelAsync<Student>` güncelleştirmeyi dener `emptyStudent` gönderilen form değerleri kullanarak nesne [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinde [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). `TryUpdateModelAsync` Yalnızca listelenen özelliklerini güncelleştirir (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Önceki örnekte:

* İkinci bağımsız değişkeni (`"student", // Prefix`) öneki değerleri aramak için kullanılır. Büyük/küçük harfe duyarlı değildir.
* Gönderilen form değerleri türleri dönüştürülür `Student` modelini [model bağlama](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>

### <a name="overposting"></a>Overposting

Kullanarak `TryUpdateModel` overposting önlediği için gönderilen değerler alanları güncelleştirmek için bir güvenlik en iyi uygulamadır. Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` eden bu web sayfası güncelleştirin veya olmamalıdır özelliği:

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Uygulama almasa bile bir `Secret` Razor sayfası, bir bilgisayar korsanının oluştur/güncelleştir alanı ayarlayabilirsiniz `Secret` overposting tarafından değeri. Bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri. Özgün koda bir öğrenci örneği oluşturduğunda, model Bağlayıcısı kullandığını alanları kısıtlamaz.

İnovasyonunuz ne olursa olsun değer için belirtilen korsanın `Secret` form alanı DB'de güncelleştirilir. Fiddler aracı ekleme, aşağıdaki resimde gösterilmektedir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).

![Fiddler'ı ekleme gizli alan](../ef-mvc/crud/_static/fiddler.png)

Değerin "OverPost" eklenen başarıyla `Secret` eklenen satırın özelliği. Hedeflenen hiçbir Uygulama Tasarımcısı `Secret` Oluştur sayfası ayarlanacak özelliği.

<a name="vm"></a>

### <a name="view-model"></a>Görünüm modeli

Bir görünüm modeli, genellikle bir uygulama tarafından kullanılan modeline dahil özellik alt kümesi içerir. Uygulama modeli, genellikle etki alanı modeli adı verilir. Etki alanı modeli, genellikle DB'de karşılık gelen varlığın gerektirdiği tüm özellikleri içerir. Görünüm modeli yalnızca UI yerleşiminde (örneğin, oluşturma sayfası) için gereken özellikleri içerir. Görünüm modeli ek olarak, bazı uygulamalar, Razor sayfaları sayfa modeli sınıfı ile tarayıcı arasında veri iletmek için bağlama modeli veya giriş modeli kullanın. Aşağıdakileri göz önünde bulundurun `Student` görünüm modeli:

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Görünüm modelleri overposting önlemek için alternatif bir yol sağlar. Görünüm modeli (görüntüleme) görüntülemek veya güncelleştirmek için yalnızca özellikleri içerir.

Aşağıdaki kod `StudentVM` görünüm modeli yeni bir öğrenci oluşturmak için:

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi, başka değerleri okuyarak bu nesnenin değerleri ayarlar [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesne. `SetValues` özellik adıyla eşleşen kullanır. Görünüm modeli türü modeli türüyle ilişkili gerekmez, bunu yalnızca eşleşen özelliklere sahip olması.

Kullanarak `StudentVM` gerektirir [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) güncelleştirilmesi kullanılacak `StudentVM` yerine `Student`.

Razor sayfalarında `PageModel` türetilmiş sınıf, görünüm modeli.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

Sayfa modeli düzenleme sayfası için güncelleştirin. Önemli değişiklikler vurgulanmıştır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Kod değişikliklerini, birkaç özel durum Oluştur sayfasında benzerdir:

* `OnPostAsync` İsteğe bağlı olan `id` parametresi.
* Geçerli Öğrenci DB'den getirilir. yerine boş bir öğrenci oluşturma.
* `FirstOrDefaultAsync` ile değiştirilmiştir [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). `FindAsync` bir varlığın birincil anahtarı seçerken olduğunda iyi bir seçimdir. Bkz: [FindAsync](#FindAsync) daha fazla bilgi için.

### <a name="test-the-edit-and-create-pages"></a>Testi düzenleme ve sayfa oluşturma

Oluşturun ve birkaç Öğrenci varlıklarını düzenleyin.

## <a name="entity-states"></a>Varlık durumları

DB bağlamı varlıkları bellekte DB'de karşılık gelen satırlarında ile eşitlenmiş durumda olup olmadığını izler. DB bağlamı eşitleme bilgileri ne olacağını belirler, [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrılır. Örneğin, ne zaman yeni bir varlık geçirildiğinde [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) varlığın durumu ayarlanmış yöntemi [eklenen](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Zaman `SaveChangesAsync` çağrıldığında DB bağlamı veren bir SQL Ekle komutu.

Bir varlık birinde olabilir [durumları izleyen](/dotnet/api/microsoft.entityframeworkcore.entitystate):

* `Added`: Varlık DB'de henüz mevcut değil. `SaveChanges` Yöntemi sorunları INSERT deyimi.

* `Unchanged`: Herhangi bir değişiklik ile bu varlık kaydedilmesi gerekir. Veritabanından okurken bir varlık bu duruma sahip.

* `Modified`: Bazılarını veya tümünü varlığın özellik değerleri değiştirilmiş. `SaveChanges` Yöntemi, bir güncelleştirme deyimiyle verir.

* `Deleted`: Varlık silinmek üzere işaretlendi. `SaveChanges` Yöntemi DELETE deyimi verir.

* `Detached`: Varlık DB bağlamı tarafından izleniyor değil.

İçinde bir masaüstü uygulaması, durum değişikliklerini genellikle otomatik olarak ayarlanır. Bir varlık okuma, değişiklik yapıldıysa ve otomatik olarak değiştirilecek şekilde varlık durumu `Modified`. Çağırma `SaveChanges` yalnızca değiştirilen özellikler güncelleştiren SQL UPDATE deyimi oluşturur.

Bir web uygulaması `DbContext` varlık ve verileri bir sayfa oluşturulduğunda elden görüntüler okur. Bir sayfa ayarlandığında `OnPostAsync` yöntemi çağrıldığında, yeni bir web isteği yapılır ve yeni bir örneğini `DbContext`. Varlık bu yeni bir bağlam içinde yeniden okuma Masaüstü işleme benzetimini yapar.

## <a name="update-the-delete-page"></a>Silme sayfası

Bu bölümde, kod bir özel hata uygulamak için eklenen zaman ileti çağrısı `SaveChanges` başarısız olur. Olası hata iletileri içeren bir dize ekleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Değiştirin `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Yukarıdaki kod, isteğe bağlı parametre içeren `saveChangesError`. `saveChangesError` Öğrenci nesneyi silmek için bir hatadan sonra yöntemi çağrıldı olup olmadığını gösterir. Silme işlemi, geçici ağ sorunları nedeniyle başarısız olabilir. Geçici ağ hataları bulutta daha yüksektir. `saveChangesError`false olduğunda silme sayfası `OnGetAsync` Arabiriminden çağrılır. Zaman `OnGetAsync` tarafından çağrılır `OnPostAsync` (silme işlemi başarısız olduğundan), `saveChangesError` parametre değeri true.

### <a name="the-delete-pages-onpostasync-method"></a>Silmeyi sayfalar OnPostAsync yöntemi

Değiştirin `OnPostAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Yukarıdaki kod, seçili varlığı alır. ardından çağırır [Kaldır](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) varlığın durumu ayarlamak için yöntemi `Deleted`. Zaman `SaveChanges` çağrılır, SQL'i Sil komut oluşturulur. Varsa `Remove` başarısız olur:

* DB özel durum yakalandı.
* Silmeyi sayfalar `OnGetAsync` yöntemi çağrıldığında `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Delete Razor sayfası

Aşağıdaki vurgulanan hata iletisini Sil Razor sayfasına ekleyin.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Delete test edin.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Öğrenci/dizin veya diğer bağlantılar çalışmaz:

Razor sayfası doğru içerdiğini doğrulayın `@page` yönergesi. Örneğin, Öğrenci/dizin Razor sayfası gereken **değil** rota şablonu içerir:

```cshtml
@page "{id:int}"
```

Her bir Razor sayfası içermelidir `@page` yönergesi.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/intro)
> [İleri](xref:data/ef-rp/sort-filter-page)
