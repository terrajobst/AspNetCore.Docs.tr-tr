---
title: "ASP.NET Core MVC EF temel - Gelişmiş - 10, 10"
author: tdykstra
description: "Bu öğretici Entity Framework Çekirdek kullanan ASP.NET web uygulamaları geliştirme temellerini gittiğiniz zaman uyumlu olması için kullanışlı olan çeşitli konular tanıtır."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: 4ee12cae0220825c81bd8b178dea3ac777f97bb6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>Gelişmiş konular - EF çekirdek ASP.NET Core MVC Öğreticisi (10 / 10)

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide tablo başına hiyerarşisi devralma uygulanmadı. Bu öğretici Entity Framework Çekirdek kullanan ASP.NET Core web uygulamaları geliştirme temellerini gittiğiniz zaman uyumlu olması için kullanışlı olan çeşitli konular tanıtır.

## <a name="raw-sql-queries"></a>Ham SQL sorguları

Entity Framework kullanmanın yararları, veri depolamanın çok yakından belirli bir yöntem kodunuzu bağlamadan önler biridir. SQL sorguları ve komutlar, ayrıca bunları kendiniz yazmak zorunda kalmaktan boşaltır oluşturarak bunu yapar. Ancak el ile oluşturduğunuz belirli SQL sorguları çalıştırmak gerektiğinde olağanüstü senaryolar vardır. Bu senaryolar için Entity Framework kod ilk API, SQL komutlarını veritabanına doğrudan geçirmenizi sağlayan yöntemler içerir. EF çekirdek 1.0 aşağıdaki seçenekleriniz vardır:

* Kullanım `DbSet.FromSql` varlık türleri döndüren sorgular için yöntem. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenen tarafından veritabanı bağlamı sürece, [izleme kapatmak](crud.md#no-tracking-queries).

* Kullanım `Database.ExecuteSqlCommand` sorgu dışı komutları için.

Varlık olmayan türleri döndüren bir sorgu çalıştırmanız gerekiyorsa, EF tarafından sağlanan veritabanı bağlantısı ile ADO.NET kullanabilirsiniz. Varlık türlerini almak için bu yöntemi kullanmak olsa bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.

Bir web uygulamasında SQL komutlarını yürüttüğünüzde her zaman true olarak, siteniz SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutlarını yorumlanan emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide kullanıcı girişi bir sorgu tümleştirdiğinizde parametreli sorgular kullanacaksınız.

## <a name="call-a-query-that-returns-entities"></a>Varlık döndüren bir sorgu arayın

`DbSet<TEntity>` Sınıfı türünde bir varlık döndüren bir sorgu çalıştırmak için kullanabileceğiniz bir yöntem sağlar `TEntity`. Bu, nasıl çalıştığını görmek için kodda değiştireceğiz `Details` departmanı denetleyicisinin yöntemi.

İçinde *DepartmentsController.cs*, `Details` yöntemi, bir bölüm ile alan kodu değiştirin bir `FromSql` vurgulanan aşağıdaki kodda gösterildiği gibi yöntem çağrısı:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Yeni kod düzgün çalıştığını doğrulamak için seçin **Departmanlar** sekmesini ve ardından **ayrıntıları** Departmanlar biri için.

![Departman ayrıntıları](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Diğer türleri döndüren bir sorgu arayın

Daha önce bir öğrenci istatistikleri kılavuz Öğrenciler sayısı için her kayıt tarihi gösterdi hakkında sayfası için oluşturduğunuz. Veri Öğrenciler varlık kümesinde var (`_context.Students`) ve sonuçları bir liste halinde projeye LINQ kullanılan `EnrollmentDateGroup` model nesneleri görüntüleyin. SQL kendisi yerine LINQ kullanarak yazma istediğinizi varsayalım. Bir SQL sorgusu çalıştırması gerekecek yapmak için varlık nesnesi dışında bir şey döndürür. EF çekirdek 1. 0'bunu yapmak için bir ADO.NET kod yazma ve veritabanı bağlantısı EF elde yoludur.

İçinde *HomeController.cs*, yerine `About` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Kullanarak bir ekleme deyimi:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Uygulama önceden olduğu aynı verileri görüntüler.

![Sayfa hakkında](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Güncelleştirme sorgusu çağırın

Contoso University yöneticiler her indirmelere için iadeleri sayısını değiştirme veritabanındaki genel değişiklikler gerçekleştirmek istediğinizi varsayalım. Çok sayıda kurslar university varsa, tüm varlıklar almak ve bunları tek tek değiştirmek için verimsiz olacaktır. Bu bölümde, tüm kurslara krediler sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulamak ve SQL UPDATE deyimi yürüterek değişiklik. Web sayfasını aşağıdaki gibi görünmelidir:

![Güncelleştirme indirmelere krediler sayfası](advanced/_static/update-credits.png)

İçinde *CoursesContoller.cs*, HttpGet ve HttpPost için UpdateCourseCredits yöntemleri ekleyin:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Denetleyici HttpGet isteği işlediğinde, hiçbir şey döndürülür `ViewData["RowsAffected"]`, yukarıdaki çizimde gösterildiği gibi görünümün boş bir metin kutusu ve bir gönderme düğmesi görüntüler.

Zaman **güncelleştirme** düğmesine tıklandığında, HttpPost yöntemi çağrılır ve çarpanı metin kutusuna girilen değere sahip. Kodu sonra kurslar güncelleştirir ve görünümüne etkilenen satır sayısını döndürür SQL yürütür `ViewData`. Görünüm zaman alır bir `RowsAffected` değeri, güncelleştirilmiş satır sayısını görüntüler.

İçinde **Çözüm Gezgini**, sağ *görünümler/kurslar* klasörünü ve ardından **Ekle > Yeni öğe**.

İçinde **Yeni Öğe Ekle** iletişim kutusunda, tıklatın **ASP.NET** altında **yüklü** sol bölmede **MVC görünüm sayfası**ve YeniGörünümadı *UpdateCourseCredits.cshtml*.

İçinde *Views/Courses/UpdateCourseCredits.cshtml*, şablon kodu aşağıdaki kodla değiştirin:

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Çalıştırma `UpdateCourseCredits` seçerek yöntemi **kurslar** sonra ekleyerek sekmesinde, "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğundaki URL'yi (örneğin: `http://localhost:5813/Courses/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Güncelleştirme indirmelere krediler sayfası](advanced/_static/update-credits.png)

Tıklatın **güncelleştirme**. Etkilenen satırların sayısını bakın:

![Etkilenen Satırlar güncelleştirme indirmelere krediler sayfası](advanced/_static/update-credits-rows-affected.png)

Tıklatın **listesine dön** krediler düzenlenen sayısıyla kurslar listesini görmek için.

Üretim kodu her zaman geçerli bir veri kümesinde güncelleştirmelerinin sağlar unutmayın. Burada gösterilen Basitleştirilmiş kodu 5'ten büyük sayılar elde etmek yeterince krediler sayısı çarpabilirsiniz. ( `Credits` Özelliğine sahip bir `[Range(0, 5)]` özniteliği.) Güncelleştirme sorgusu çalışır ancak geçersiz veriler, krediler sayısı 5 veya daha az olduğunu varsayalım diğer bölümlerinde sistem beklenmeyen sonuçlara neden olabilir.

Ham SQL sorguları hakkında daha fazla bilgi için bkz: [ham SQL sorguları](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>SQL veritabanına gönderilen inceleyin

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmek için yararlıdır. Yerleşik günlük işlevselliği için ASP.NET Core içeren SQL sorguları ve güncelleştirmeleri için günlükler yazmak için EF çekirdek tarafından otomatik olarak kullanılır. Bu bölümde bazı örnekler SQL günlüğü görürsünüz.

Açık *StudentsController.cs* ve `Details` yöntemi açık bir kesme noktası ayarlayın `if (student == null)` deyimi.

Uygulamayı hata ayıklama modunda çalıştırın ve bir öğrenci için ayrıntıları sayfasına gidin.

Git **çıkış** hata ayıklama gösteren penceresi çıkış ve sorgu bakın:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Burada herhangi bir şey, beklenmedik fark edeceksiniz: SQL en çok 2 satırları seçer (`TOP(2)`) Kişi tablosundan. `SingleOrDefaultAsync` Yöntemi, sunucu üzerindeki 1 satır için çözümlenmiyor. Neden şöyledir:

* Sorgu birden çok satır döndürürse, bu yöntem null değeri döndürür.
* Sorgu birden çok satır döndürecekti olup olmadığını belirlemek için en az 2 döndürür denetlemek EF sahiptir.

Hata ayıklama modunu kullanın ve günlük çıktısı almak için bir kesme noktasında durdurmak yok Not **çıkış** penceresi. Bu günlük çıktıyı görünmesini istediğiniz noktada durdurmak için yalnızca bir yoludur. Bunu yok ise, günlük kaydı devam eder ve geri ilgilendiğiniz bölümleri bulmak için kaydırma gerekir.

## <a name="repository-and-unit-of-work-patterns"></a>Depo ve iş desenleri birimi

Çoğu geliştiricinin depo ve iş desenleri biriminin Entity Framework ile çalışan kod çevresinde bir sarmalayıcı olarak uygulamak için kod yazma. Bu düzenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasındaki bir Soyutlama Katmanı oluşturmak üzere tasarlanmıştır. Bu desenleri uygulama veri deposunda değişiklik uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya teste dayalı geliştirme (TDD) kolaylaştırabilir. Ancak, bu desenleri uygulamak için ek kod yazma her zaman EF, çeşitli nedenlerle kullanan uygulamalar için en iyi seçenek değildir:

* EF bağlamı sınıfının kendisi veri deposu özel kod kodunuzdan korunmasını sağlar.

* EF bağlam sınıfını EF kullanarak bunu veritabanı için bir iş birimi sınıf güncelleştirmeleri olarak davranamaz.

* EF deposu kod yazmadan TDD uygulamaya yönelik özellikler içerir.

Depo ve iş desenleri ölçü uygulama hakkında daha fazla bilgi için bkz: [Bu öğretici seri Entity Framework 5 sürümünü](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Çekirdek test etmek için kullanılan bir bellek içi veritabanı sağlayıcısı uygular. Daha fazla bilgi için bkz: [Inmemory ile test](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Otomatik değiştirme algılama

Entity Framework bir varlık nasıl değiştiğini (ve bu nedenle hangi veritabanına gönderilmesi gereken güncelleştirmeler) geçerli bir varlık değerleri özgün değerlerle karşılaştırarak belirler. Varlık sorgulanan ya da bağlı olduğunda özgün değerler depolanır. Otomatik değiştirme algılama neden yöntemlerin bazıları şunlardır:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Çok sayıda varlık takip ettiğiniz ve aşağıdaki yöntemlerden birini birçok kez bir döngüde çağırmanız, algılama otomatik değişiklik kullanarak geçici olarak kapatarak önemli performans geliştirmeleri alabilirsiniz `ChangeTracker.AutoDetectChangesEnabled` özelliği. Örneğin:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Çekirdek kaynak kodu ve Geliştirme planları

Entity Framework Çekirdek kaynağı altındadır [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore). EF çekirdek depo içerir gecelik derlemeleri, sorun izleme, özellik belirtimlerin, toplantı notları, tasarım ve [ileride geliştirme için yol haritası](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Dosya veya hataları bulma ve katkıda.

Kaynak kodu açık olsa da, Entity Framework Çekirdek tam bir Microsoft ürünü desteklenir. Microsoft Entity Framework takım üzerinde Katkıları kabul edilen denetim tutar ve her sürümle kalitesini emin olmak için tüm kod değişiklikleri sınar.

## <a name="reverse-engineer-from-existing-database"></a>Varolan veritabanından ters mühendislik

Varolan bir veritabanını varlık sınıflardan dahil olmak üzere bir veri modeli ters mühendislik için kullanmak [iskele dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) komutu. Bkz: [başlangıç Öğreticisi](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Sıralama seçimi kodu basitleştirmek için dinamik LINQ kullanma

[Bu serideki üçüncü öğretici](sort-filter-page.md) LINQ kodunun kodlama sabit sütun adları tarafından nasıl yazılacağını gösterir bir `switch` deyimi. Aralarından seçim yapabileceğiniz iki sütunlarla bu düzgün çalışır, ancak çok sayıda sütun varsa, kodu ayrıntılı alabilir. Bu sorunu çözmek için kullanabileceğiniz `EF.Property` yöntemi bir dize olarak özellik adını belirtin. Bu yaklaşım denemek için değiştirme `Index` yönteminde `StudentsController` aşağıdaki kod ile.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Sonraki adımlar

Bu, bir ASP.NET MVC uygulamasındaki Entity Framework Çekirdek kullanma öğreticileri bu dizi tamamlar.

EF çekirdek hakkında daha fazla bilgi için bkz: [Entity Framework Core belgeleri](https://docs.microsoft.com/ef/core). Kitap da kullanılabilir: [Entity Framework Çekirdek eylem](https://www.manning.com/books/entity-framework-core-in-action).

Bir web uygulaması dağıtma hakkında daha fazla bilgi için bkz: [konak dağıtıp](xref:host-and-deploy/index).

ASP.NET Core MVC için kimlik doğrulama ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz: [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/).

## <a name="acknowledgments"></a>İlgili kaynaklar

Zel Dykstra ve Rick Anderson (twitter @RickAndMSFT) Bu öğretici yazıldı. Rowan Mert, Diego Vega ve diğer Entity Framework ekibi üyelerinin kod incelemeleri destekli ve biz öğreticileri için kod yazma sırada, çıkan hatalarını ayıklamanıza yardımcı oldu.

## <a name="common-errors"></a>Sık karşılaşılan hataları  

### <a name="contosouniversitydll-used-by-another-process"></a>Başka bir işlem tarafından kullanılan ContosoUniversity.dll

Hata iletisi:

> Açılamıyor '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' yazma--' işlem dosya erişemiyor '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' başka bir işlem tarafından kullanıldığından.

Çözüm:

IIS Express sitede durdurun. Windows sistem tepsisi için IIS Express bulmak ve simgesini sağ tıklatın, Contoso University siteyi seçin ve ardından Git **durdurmak Site**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Hiçbir kod bulunan yöntemleri yukarı ve aşağı iskele kurulmuş geçiş

Olası neden:

EF CLI komutları otomatik olarak kapatmak ve kod dosyaları kaydetmek yok. Programını çalıştırdığınızda, kaydedilmemiş değişiklikleriniz varsa `migrations add` komutunu EF değişikliklerinizi Bul olmaz.

Çözüm:

Çalıştırma `migrations remove` komutu, kod değişikliklerinizi kaydedin ve yeniden `migrations add` komutu.

### <a name="errors-while-running-database-update"></a>Çalışan veritabanı güncelleştirme sırasında hatalar

Şema değişiklikleri var olan verileri içeren bir veritabanına yaparken diğer hatalarıyla mümkündür. Çözümlenemiyor Geçiş hataları alırsanız, bağlantı dizesindeki veritabanı adını değiştirin veya veritabanını silin. Yeni bir veritabanı ile geçirmek için veri yok ve update-database komutunu hatasız tamamlamak çok daha yüksektir.

Veritabanında yeniden adlandırmak için basit yaklaşımdır *appsettings.json*. Sonraki çalıştırmanızda `database update`, yeni bir veritabanı oluşturulacak.

SSOX bir veritabanını silmek için veritabanına sağ tıklayın, **silmek**ve ardından **Sil veritabanı** iletişim kutusu seç **var olan bağlantıları kapatın** ve 'ıtıklatın **Tamam**.

CLI kullanarak bir veritabanını silmek için Çalıştır `database drop` CLI komutu:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

> SQL Server bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örnek belirtilen hata bulma)

Çözüm:

Bağlantı dizesini kontrol edin. Veritabanı dosyasını el ile sildiyseniz, üzerinde yeni bir veritabanı ile başlatmak için yapım dizesinde veritabanı adını değiştirin.

>[!div class="step-by-step"]
[Önceki](inheritance.md)
