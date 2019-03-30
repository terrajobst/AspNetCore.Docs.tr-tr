---
title: 'Öğretici: Gelişmiş senaryoları hakkında - EF çekirdekli ASP.NET MVC ile bilgi edinin.'
description: Bu öğretici, Entity Framework Core kullanan ASP.NET Core web uygulamaları geliştirmenin temellerini ötesine geçmesini yararlı konuları tanıtır.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/advanced
ms.openlocfilehash: c6255e2b4fc67c6174bab4458ec82035b1886002
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750629"
---
# <a name="tutorial-learn-about-advanced-scenarios---aspnet-mvc-with-ef-core"></a>Öğretici: Gelişmiş senaryoları hakkında - EF çekirdekli ASP.NET MVC ile bilgi edinin.

Önceki öğreticide tablo başına hiyerarşi devralma uygulanır. Bu öğretici, ne zaman, Entity Framework Core kullanan ASP.NET Core web uygulamaları geliştirmenin temellerini gidin dikkat etmeniz yararlı olan çeşitli konuları tanıtır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Ham SQL sorguları gerçekleştirme
> * Bir sorguyu varlıkları arama
> * Bir sorguyu diğer türleri çağırın
> * Update sorgusu çağırın
> * SQL sorguları inceleyin
> * Bir soyutlama katmanı oluşturma
> * Otomatik değiştirme algılama hakkında bilgi edinin
> * EF Core kaynak kodu ve geliştirme planlar hakkında bilgi edinin
> * Dinamik LINQ kodu basitleştirmek için kullanmayı öğrenin

## <a name="prerequisites"></a>Önkoşullar

* [Devralma uygulama](inheritance.md)

## <a name="perform-raw-sql-queries"></a>Ham SQL sorguları gerçekleştirme

Entity Framework kullanmanın avantajlarını önler, veri depolamanın çok yakından belirli bir yöntem, kodunuzu bağlamadan biridir. Bunu SQL sorgulara ve komutlara sizin için Ayrıca bunları kendiniz yazmak zorunda kalmanızı oluşturarak yapar. Ancak, el ile oluşturduğunuz belirli SQL sorguları çalıştırmak ihtiyacınız olduğunda olağanüstü senaryolar vardır. Bu senaryolar için Entity Framework kod ilk API SQL komutları doğrudan veritabanına geçirilecek sağlayan yöntemler içerir. EF Core 1.0 aşağıdaki seçenekleriniz vardır:

* Kullanım `DbSet.FromSql` varlık türleri döndüren sorgular için yöntemi. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenen tarafından veritabanı bağlamı sürece, [izleme devre dışı](crud.md#no-tracking-queries).

* Kullanım `Database.ExecuteSqlCommand` sorgu dışı komutları için.

Varlık olmayan türler döndüren bir sorgu çalıştırmak ihtiyacınız varsa EF tarafından sağlanan veritabanı bağlantı ile ADO.NET kullanabilirsiniz. Varlık türleri almak için bu yöntem kullansanız bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.

Bir web uygulamasında SQL komutları yürütme her zaman true olduğu gibi sitenizi SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutları yorumlanamıyor emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide, kullanıcı girişi bir sorguya tümleştirdiğinizde parametreli sorgular kullanacaksınız.

## <a name="call-a-query-to-return-entities"></a>Bir sorguyu varlıkları arama

`DbSet<TEntity>` Sınıf türünde bir varlık döndüren bir sorgu yürütmek için kullanabileceğiniz bir yöntem sağlar `TEntity`. Bu, nasıl çalıştığını görmek için kodda değiştireceksiniz `Details` departmanı denetleyicinin yöntemi.

İçinde *DepartmentsController.cs*, `Details` yöntemi, bir bölümle alan kodu değiştirin bir `FromSql` yöntemi çağrısı, vurgulanan aşağıdaki kodda gösterildiği gibi:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10)]

Yeni kod düzgün çalıştığını doğrulamak için **Departmanlar** sekmesini ve ardından **ayrıntıları** Departmanlar biri için.

![Departman ayrıntıları](advanced/_static/department-details.png)

## <a name="call-a-query-to-return-other-types"></a>Bir sorguyu diğer türleri çağırın

Daha önce bir öğrenci istatistikleri kılavuz Öğrenci sayısı için her bir kayıt tarihi gösterdi hakkında sayfası için oluşturuldu. Veri Öğrenciler varlık kümesinde var (`_context.Students`) ve sonuçları bir liste halinde projeye LINQ kullanılan `EnrollmentDateGroup` model nesneleri görüntüleyin. SQL kendisi yerine LINQ kullanarak yazmak istediğinizi varsayalım. Bir SQL sorgusunu çalıştırmak ihtiyacınız olan şeyi için varlık nesnesi dışında bir şey döndürür. EF Core 1.0, bunu yapmak için bir ADO.NET kod yazma ve veritabanı bağlantısını almak EF yoludur.

İçinde *HomeController.cs*, değiştirin `About` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Kullanarak bir ekleme deyimi:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Bu, daha önceki işlevlerini sürdürmektedir aynı verileri görüntüler.

![Sayfa hakkında](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Update sorgusu çağırın

Contoso University yöneticiler her kurs sonunda verilen kredi sayısı değiştirme gibi bu veritabanındaki genel değişiklikler yapmak istediğinizi varsayalım. University dersleri çok sayıda varsa, bunları tüm varlıklar olarak almak ve bunları tek tek değiştirmek için verimsiz olabilir. Bu bölümde, tüm kursları için kredi sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulayacaksınız ve SQL UPDATE deyimi yürüterek değişiklik yapacaksınız. Web sayfasını aşağıdaki gibi görünür:

![Güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits.png)

İçinde *CoursesController.cs*, HttpGet ve HttpPost UpdateCourseCredits yöntemleri ekleyin:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Denetleyici HttpGet isteği işlerken bir şey iade `ViewData["RowsAffected"]`, ve görünüm önceki resimde gösterildiği gibi boş bir metin kutusu ve bir Gönder düğmesi görüntüler.

Zaman **güncelleştirme** çarpanı olan metin kutusuna girilen değer düğmesine tıklandığında ve HttpPost yöntemi çağrılır. Kodu sonra dersleri güncelleştirir ve görünümüne etkilenen satır sayısını döndüren SQL yürütür `ViewData`. Zaman görünümünü alır bir `RowsAffected` değeri, güncelleştirilen satır sayısını görüntüler.

İçinde **Çözüm Gezgini**, sağ *görünümler/kursları* klasörünü ve ardından **Ekle > Yeni öğe**.

İçinde **Yeni Öğe Ekle** iletişim kutusunda, tıklayın **ASP.NET Core** altında **yüklü** sol bölmesinden **Razor Görünüm**ve Yeni Görünüm adlandırın *UpdateCourseCredits.cshtml*.

İçinde *Views/Courses/UpdateCourseCredits.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Çalıştırma `UpdateCourseCredits` yöntemi seçerek **kursları** sekmesini, ardından ekleme "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğuna URL'yi (örneğin: `http://localhost:5813/Courses/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits.png)

Tıklayın **güncelleştirme**. Etkilenen satır sayısını görürsünüz:

![Satırlardan etkilenen güncelleştirme kurs KREDİLERİ sayfası](advanced/_static/update-credits-rows-affected.png)

Tıklayın **listesine geri** kredi düzeltilmiş sayısı kurslarıyla listesini görmek için.

Üretim kodu her zaman geçerli veri sonucunda güncelleştirmelerinin sağlar unutmayın. Burada gösterilen Basitleştirilmiş kod 5'ten büyük bir sayı elde etmek yeterli kredi sayısı çarpabilirsiniz. ( `Credits` Özelliğine sahip bir `[Range(0, 5)]` özniteliği.) Güncelleştirme sorgu işe yarar, ancak geçersiz veri, kredi sayısı 5 veya daha az olduğu varsayılır diğer bölümlerinde sistemin beklenmeyen sonuçlara neden.

Ham SQL sorguları hakkında daha fazla bilgi için bkz. [ham SQL sorguları](/ef/core/querying/raw-sql).

## <a name="examine-sql-queries"></a>SQL sorguları inceleyin

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmeniz yararlıdır. ASP.NET Core için yerleşik günlüğü işlevini içeren SQL sorguları ve güncelleştirmeleri günlüklerini yazma izni EF Core tarafından otomatik olarak kullanılır. Bu bölümde bazı örnekler bir SQL günlük göreceksiniz.

Açık *StudentsController.cs* ve `Details` yöntemi, üzerinde bir kesme noktası Ayarla `if (student == null)` deyimi.

Uygulamayı hata ayıklama modunda çalıştırın ve bir öğrenci için ayrıntıları sayfasına gidin.

Git **çıkış** çıkış penceresinin hata ayıklama gösteren ve sorgu görürsünüz:

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

Burada bir şey, sizi şaşırtabilir fark edeceksiniz: SQL 2 satırları seçer (`TOP(2)`) Kişi tablosundan. `SingleOrDefaultAsync` Yöntemi, sunucu üzerindeki 1 satır için çözümlenmiyor. Bunu istememizin nedeni:

* Sorgu birden çok satır döndürür, yöntem null değeri döndürür.
* Sorgu birden çok satır döndürecekti olup olmadığını belirlemek için en az 2 döndürür denetlenecek EF sahiptir.

Hata ayıklama modunu kullanmak ve günlük çıktısı almak için bir kesme noktasında durdurmak yok Not **çıkış** penceresi. Bu günlük çıktısına görünmesini istediğiniz noktada durdurmak için yalnızca bir yoludur. Bunu yok ise, günlüğe kaydetmeye devam eder ve geri ilgilendiğiniz bölümleri bulmak için kaydırmak zorunda.

## <a name="create-an-abstraction-layer"></a>Bir soyutlama katmanı oluşturma

Birçok geliştiricinin depo ve iş birimi desenleri, Entity Framework ile çalışan kod çevresinde sarmalayıcı olarak uygulamak için kod yazın. Bu desenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasında bir Soyutlama Katmanı oluşturmak için tasarlanmıştır. Bu desenleri uygulama veri deposundaki değişiklikleri uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya test odaklı geliştirme (TDD) kolaylaştırabilir. Ancak, bu desenleri uygulamak için ek kod yazmaya her zaman EF, çeşitli nedenlerle kullanan uygulamalar için en iyi seçenek değildir:

* EF bağlam sınıfını kendi veri deposu özel kod kodunuzdan korunmasını sağlar.

* EF bağlam sınıfını kullanarak EF bunu veritabanı için bir iş birimi sınıfı güncelleştirmeleri olarak hareket eder.

* EF depo kod yazmaya gerek kalmadan TDD uygulamak için özellikler içerir.

Depo ve iş birimi desenleri uygulama hakkında daha fazla bilgi için bkz: [Entity Framework 5 sürümü Bu öğretici serisinin](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core test etmek için kullanılabilecek bir bellek içi veritabanı sağlayıcısını uygular. Daha fazla bilgi için [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Otomatik değiştirme algılama

Entity Framework, bir varlığın geçerli değerleri özgün değerlerle karşılaştırarak, varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) belirler. Varlık sorgulanan ya da bağlı orijinal değerleri depolanır. Otomatik değiştirme algılama neden yöntemlerden bazıları aşağıda verilmiştir:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Aşağıdaki yöntemlerden birini bir döngüde birçok kez çağırmak ve çok sayıda varlık izliyorsunuz, önemli performans iyileştirmeleri otomatik değişiklik algılama kullanarak geçici olarak kapatarak alabilirsiniz `ChangeTracker.AutoDetectChangesEnabled` özelliği. Örneğin:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="ef-core-source-code-and-development-plans"></a>EF Core kaynak kodu ve Geliştirme planları

Entity Framework Core kaynak altındadır [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore). EF Core deposu içeren gecelik derlemeler, sorun izleme, özellik özellikleri, toplantı notları, tasarım ve [gelecekteki geliştirme için yol haritası](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Dosya veya hataları bulmak ve katkıda bulunun.

Kaynak kodu açık olsa da, Entity Framework Core tam olarak bir Microsoft ürünü olarak desteklenir. Microsoft Entity Framework takım denetim üzerinde katkılarını kabul tutar ve her bir yayının kalitesini sağlamak için tüm kod değişikliklerinin test eder.

## <a name="reverse-engineer-from-existing-database"></a>Mevcut veritabanından ters mühendislik

Varolan bir veritabanından varlık sınıfları da dahil olmak üzere bir veri modeli tersine mühendislik için kullanın [iskele dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) komutu. Bkz: [başlangıç Öğreticisi](/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>

## <a name="use-dynamic-linq-to-simplify-code"></a>Kodu basitleştirmek için dinamik LINQ kullanma

[Bu serinin üçüncü öğreticide](sort-filter-page.md) sabit kodlama sütun adları tarafından LINQ kodunu yazma işlemi gösterilmektedir bir `switch` deyimi. Aralarından seçim yapabileceğiniz iki sütunlu, bu düzgün çalışır, ancak çok sütun varsa kod ayrıntılı alabilir. Bu sorunu çözmek için kullanabileceğiniz `EF.Property` özellik adını bir dize olarak belirtmek için yöntemi. Bu yaklaşım denemek için değiştirin `Index` yönteminde `StudentsController` aşağıdaki kod ile.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="acknowledgments"></a>İlgili kaynaklar

Tom Dykstra ve Rick Anderson (twitter @RickAndMSFT) bu Öğreticisi yazdı. Rowan Miller, Diego Vega ve diğer Entity Framework takım üyeleri kod incelemeleriyle Yardımlı ve kod öğreticileri için yazar, çıkan sorunlarında hata ayıklama yardımcı olmuştur. John Parente ve Paul Goldman öğretici ASP.NET Core 2.2 için güncelleştirme üzerinde çalışmıştır.

<a id="common-errors"></a>

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan sorunları giderme

### <a name="contosouniversitydll-used-by-another-process"></a>Başka bir işlem tarafından kullanılan ContosoUniversity.dll

Hata iletisi:

> Açılamıyor '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll'--yazmak için ' işlem dosyaya erişemiyor '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' başka bir işlem tarafından kullanıldığı için.

Çözüm:

IIS Express bir sitedeki durdurun. Windows sistem tepsisine, IIS Express bulmak ve simgesini sağ tıklatın, Contoso University siteyi seçin ve ardından Git **Durdur Site**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Hiçbir kodla yöntemleri yukarı ve aşağı iskele kurulmuş geçiş

Olası neden:

EF CLI komutları yoksa otomatik olarak kapatmak ve kod dosyaları kaydedin. Programını çalıştırdığınızda Kaydedilmemiş değişiklikleriniz var ve varsa `migrations add` komutunu EF değişikliklerinizi bulamaz.

Çözüm:

Çalıştırma `migrations remove` komutu, kod değişikliklerinizi kaydedin ve yeniden `migrations add` komutu.

### <a name="errors-while-running-database-update"></a>Çalışan veritabanı güncelleştirmesi sırasında hatalar

Diğer hatalar mevcut veriler varsa bir veritabanında şema değişiklik yaparken almak mümkündür. Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin. Yeni bir veritabanı geçirmek için veri yok ve veritabanını güncelleştir komut hatasız tamamlanması çok daha yüksektir.

Veritabanında yeniden adlandırmak için en basit yaklaşımdır *appsettings.json*. Bir sonraki çalıştırmanızda `database update`, yeni bir veritabanı oluşturulur.

SSOX veritabanında silmek için veritabanına sağ tıklayın, **Sil**ve ardından **veritabanını Sil** iletişim kutusu seç **var olan bağlantıları kapatın** tıklayın **Tamam**.

CLI kullanarak bir veritabanını silmek için çalıştırma `database drop` CLI komutunu:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

> Bir SQL Server bağlantısı kurulurken ağla ilgili veya örneğe özel bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığını doğrulayın. (sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örneği belirtilen bulma hatası)

Çözüm:

Bağlantı dizesini kontrol edin. Veritabanı dosyasını el ile sildiyseniz, yeni bir veritabanı ile baştan başlamak yapım dizesinde veritabanının adını değiştirin.

## <a name="get-the-code"></a>Kodu alma

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>Ek kaynaklar

EF Core hakkında daha fazla bilgi için bkz. [Entity Framework Core belgeleri](/ef/core). Bir kitap de kullanılabilir: [Entity Framework Core uygulamada](https://www.manning.com/books/entity-framework-core-in-action).

Bir web uygulamasının nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/index>.

ASP.NET Core MVC için kimlik doğrulaması ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz. <xref:index>.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Gerçekleştirilen ham SQL sorguları
> * Bir sorguyu varlıkları çağırılır
> * Adlı bir sorguyu diğer türleri
> * Update sorgusu çağırılır
> * Denetlenen SQL sorguları
> * Oluşturulan bir Soyutlama Katmanı
> * Otomatik değiştirme algılama hakkında bilgi edindiniz
> * EF Core kaynak kodu ve geliştirme planlar hakkında bilgi edindiniz
> * Kodu basitleştirmek için dinamik LINQ kullanmayı öğrendiniz

Bu, Bu öğretici serisinde, Entity Framework Core kullanarak bir ASP.NET Core MVC uygulamasındaki tamamlar. Yeni bir veritabanı ile birlikte çalışarak bu seri; Varolan bir veritabanından tersine mühendislik bir model için bir alternatiftir.

> [!div class="nextstepaction"]
> [Öğretici: EF Core MVC, var olan veritabanı ile](/ef/core/get-started/aspnetcore/new-db?toc=/aspnet/core/toc.json&bc=/aspnet/core/breadcrumb/toc.json)
