---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Depo ve iş desenleri biriminin bir ASP.NET MVC uygulamasındaki (9, 10) uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875883"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Depo ve iş desenleri biriminin bir ASP.NET MVC uygulamasındaki (9, 10) uygulama
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide yedekli kodda azaltmak için devralma kullanılan `Student` ve `Instructor` sınıflar. Bu öğreticide depo ve iş desenleri biriminin CRUD işlemleri için kullanmanın bazı yollarını görürsünüz. Önceki öğretici olduğu gibi bu birinde, sayfalarıyla, önceden oluşturulan yeni sayfaların oluşturmak yerine, kodunuzun çalışma biçimini değiştireceksiniz.

## <a name="the-repository-and-unit-of-work-patterns"></a>Depo ve iş desenleri birimi

Depo ve iş desenleri ölçü veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasındaki bir Soyutlama Katmanı oluşturmak üzere tasarlanmıştır. Bu desenleri uygulama veri deposunda değişiklik uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya teste dayalı geliştirme (TDD) kolaylaştırabilir.

Bu öğreticide her varlık türü için bir depo sınıfına uygulamanız. İçin `Student` varlık türü bir depo arabirimi ve bir depo sınıfına oluşturacaksınız. Depo, denetleyici örneği, böylece denetleyicisi deposu arabirimini uygulayan herhangi bir nesneye başvuru kabul arabirimi kullanacaksınız. Denetleyici altında bir web sunucusunda çalıştığında, Entity Framework ile çalışan bir depo alır. Denetleyici birim testi sınıfı altında çalıştığında, bir bellek içi koleksiyonu gibi test etmek için kolayca işleyebileceğiniz şekilde depolanan verilerle çalışır bir depo alır.

Öğreticide daha sonra birden çok depoları ve bir birim için iş sınıfının kullanacağınız `Course` ve `Department` varlık türleri `Course` denetleyicisi. İş sınıfı birim, birden çok depolarının iş bunların tümünün tarafından paylaşılan tek veritabanı bağlamı sınıfının oluşturarak düzenler. Otomatik birim testi gerçekleştirmek istiyorsanız, oluşturma ve arabirimleri için bu sınıfları için yaptığınız gibi kullanın `Student` deposu. Ancak, öğreticiyi basit tutmak için oluşturup bu sınıfların olmadan arabirimler kullanabilirsiniz.

Aşağıdaki çizimde, denetleyici ve depo veya çalışma deseni biriminin hiç kullanılmıyor karşılaştırıldığında bağlamı sınıfları arasındaki ilişkileri canlandırmanın tek yolu gösterir.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Bu öğretici serisinde birim testleri oluşturmaz. Depo düzeni kullanan bir MVC uygulaması ile TDD giriş için bkz: [izlenecek yol: ASP.NET MVC ile kullanma TDD](https://msdn.microsoft.com/library/ff847525.aspx). Depo düzeni hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Depo düzeni](https://msdn.microsoft.com/library/ff649690.aspx) konusuna bakın.
- [Entity Framework 4.0 ile depo ve iş birimi düzenleri kullanarak](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) Entity Framework ekip blogunda.
- [Çevik Entity Framework 4 depo](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) Julie Lerman'ın blog gönderilerine dizi.
- [Bir bakışta HTML5/jQuery uygulama hesabı oluşturma](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) Dan Wahlin'ın blogunda.

> [!NOTE]
> Depo ve iş desenleri biriminin uygulamak için birçok yolu vardır. Depo sınıflarını olan veya olmayan bir birim iş sınıfının kullanabilirsiniz. Tüm Varlık türlerinin veya her tür için tek bir depo uygulayabilirsiniz. Her tür için bir tane uygularsanız, ayrı sınıfları, genel bir taban sınıf ve türetilen sınıflar veya Özet temel sınıf ve türetilen sınıflar kullanabilirsiniz. İş mantığı, depoya ekleme veya veri erişim mantığı için kısıtlayın. Kullanarak, veritabanı bağlamı sınıfının bir Soyutlama Katmanı oluşturabilirsiniz [Idbset](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) yerine var. arabirimleri [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) türler, varlık kümeleri için. Bu öğreticide gösterilen bir Soyutlama Katmanı uygulama yaklaşım, dikkate almanız gereken, değil tüm senaryolar ve ortamlar için bir öneri bir seçenektir.


## <a name="creating-the-student-repository-class"></a>Öğrenci depo sınıfı oluşturma

İçinde *DAL* klasörünü adlı bir sınıf dosyası oluşturma *IStudentRepository.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Bu kod iki okuma yöntemleri dahil olmak üzere CRUD yöntemleri, tipik bir dizi bildirir — tüm döndüren bir `Student` varlıkları ve tek bir bulur bir `Student` varlığı kimliğe göre

İçinde *DAL* klasörünü adlı bir sınıf dosyası oluşturma *StudentRepository.cs* dosya. Var olan kodu uygulayan aşağıdaki kodla değiştirin `IStudentRepository` arabirimi:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Veritabanı bağlamı sınıfının değişkeninde tanımlanır ve Oluşturucusu bağlam örneğini geçirmek için arama nesnesi bekliyor:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Depoya yeni bir bağlamda örneği oluşturulamadı, ancak bir denetleyicide birden çok depoları kullandıysanız, ardından her ayrı bağlamla yapmış olursunuz. İçinde birden çok depoları daha sonra kullanacağınız `Course` denetleyici ve göreceksiniz nasıl bir birimi iş sınıfının tüm depoları aynı bağlam kullanmasını sağlayabilirsiniz.

Depo uygulayan [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) ve veritabanı bağlamı olarak denetleyicisi daha önce gördüğünüz ve CRUD yöntemlerinden daha önce gördüğünüzle aynı yolla veritabanı bağlamı aramalarda siler.

## <a name="change-the-student-controller-to-use-the-repository"></a>Depoyu kullanacak şekilde Öğrenci denetleyicisi değiştirme

İçinde *StudentController.cs*, şu anda sınıfında kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Denetleyici şimdi uygulayan bir nesne için bir sınıf değişkeni bildirir `IStudentRepository` arabirimi bağlam sınıfını yerine:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Varsayılan (parametresiz) Oluşturucu yeni bir bağlam örneği oluşturur ve arayan bir içerik örneğinde geçirmek isteğe bağlı bir oluşturucu sağlar.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Kullanıyorsanız *bağımlılık ekleme*, veya dı, gerekli olmayacağını varsayılan oluşturucu dı yazılım doğru deposu nesne her zaman sağlanan olun çünkü.)

CRUD yöntemleri depo bağlamı yerine şimdi çağrılır:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Ve `Dispose` yöntemi şimdi depo bağlamı yerine siler:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Siteyi çalıştırın ve tıklayın **Öğrenciler** sekmesi.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Sayfa arar ve deposu kullanmak için kodu değiştirilmiş ve diğer Öğrenci sayfaları da aynı çalışmadan önce yaptığınız gibi aynı şekilde çalışır. Ancak, şekilde önemli bir fark yoktur `Index` denetleyicisinin yöntemi, filtreleme ve sıralama yapar. Bu yöntem özgün sürümü aşağıdaki kodu içeriyor:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Güncelleştirilmiş `Index` yöntemine aşağıdaki kodu içerir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Yalnızca vurgulanmış kodu değiştirildi.

Kod özgün sürümünde `students` olarak yazılan bir `IQueryable` nesnesi. Bir yöntemi kullanarak bir koleksiyona dönüştürülünceye kadar sorgu veritabanına gönderilen değil `ToList`, hangi oluşmaz dizin görünümünün Öğrenci modeli erişen kadar. `Where` Yukarıdaki özgün kod yönteminde hale bir `WHERE` veritabanına gönderilen SQL sorgusunda yan tümcesi. Sırayla yalnızca seçilen varlıklar veritabanı tarafından döndürülür anlamına gelir. Bununla birlikte, sonucu olarak değiştirme `context.Students` için `studentRepository.GetStudents()`, `students` bu bildirimi tamamlandıktan sonra değişkeni bir `IEnumerable` tüm Öğrenciler veritabanında içeren koleksiyonu. Uygulamanın son sonucu `Where` yöntemi aynıdır, ancak artık iş bellek web sunucusu ve veritabanı tarafından yapılır. Büyük miktarda veriyi döndüren sorgular için bu verimsiz olabilir.

> [!TIP]
> 
> **Iqueryable vs. IEnumerable**
> 
> Şeyin girmiş olsa bile, depo burada gösterildiği gibi uyguladıktan sonra **arama** kutusunu SQL Server'a gönderilen sorgu ölçütlerinizle içermediği için tüm Öğrenci satırları döndürür:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Arama ölçütleri hakkında bilmeden sorgu deposu yürütülür çünkü bu sorgu tüm Öğrenci verileri döndürür. Sıralama, arama ölçütleri uygulama ve bellek disk belleği (Bu örnekte yalnızca 3 satır gösteren) yapılır için bir veri alt kümesini seçmek işleminin sonraki olduğunda `ToPagedList` yöntemi çağrıldığında `IEnumerable` koleksiyonu.
> 
> Arama ölçütlerini uyguladıktan sonra (depo uygulanan önce) kod önceki sürümünde, sorgu kadar veritabanına gönderilmez zaman `ToPagedList` üzerinde adlı `IQueryable` nesnesi.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Ne zaman ToPagedList çağrılır üzerinde bir `IQueryable` nesnesi, SQL Server'a gönderilen sorgu, arama dizesi belirtir ve arama ölçütlerini karşılayan satırları sonuç olarak gönderilir ve hiçbir filtreleme bellekte yapılmalıdır.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Aşağıdaki öğreticide SQL Server'a gönderilen sorguların inceleyin açıklanmaktadır.)


Aşağıdaki bölümde, bu iş veritabanı tarafından yapılmalıdır belirtmenizi sağlayan deposu yöntemleri gösterilmektedir.

Denetleyici ve Entity Framework veritabanı bağlamı arasındaki bir Soyutlama Katmanı şimdi oluşturduğunuzu düşünün. Otomatik birim bu uygulamayla testi gerçekleştirmek için giderek, bir alternatif depo sınıfını uygulayan birim testi projesi içinde oluşturabilirsiniz `IStudentRepository` *.* Veri okumak veya yazmak için bağlamı çağırmak yerine, bu sahte depo sınıfına bellek içi koleksiyonları denetleyicisi işlevlerini test etmek için yönlendirme.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Genel bir depo ve iş sınıfının bir birim uygular

Her bir varlık türü için bir depo sınıfına oluşturma sürü yedekli kod neden olabilir ve kısmi güncelleştirmelerinde neden olabilir. Örneğin, iki farklı varlık türleri aynı işlemin bir parçası olarak güncelleştirmek olduğunu varsayalım. Her bir ayrı veritabanı bağlam örneğini kullanıyorsa, biri başarılı olabilir ve diğer başarısız olabilir. Yedek kodu en aza indirmek için bir yoldur genel bir depo ve sağlamanın bir yolu kullanmak için tüm depoları aynı veritabanı bağlamı kullanın (ve dolayısıyla tüm güncelleştirmeleri koordine olduğunu) bir birim iş sınıfının kullanılmasıdır.

Öğreticinin bu bölümünde oluşturacağınız bir `GenericRepository` sınıfı ve bir `UnitOfWork` sınıfı ve bunları kullanmak `Course` hem erişim denetleyicisi `Department` ve `Course` varlık kümeleri. Öğreticinin bu bölümü basit tutmak için daha önce açıklandığı gibi bu sınıfları için arabirimler oluşturma değil. Ancak bunları TDD kolaylaştırmak için kullanacağınız, genellikle arabirimleriyle aynı şeklinden uygulamadan `Student` deposu.

### <a name="create-a-generic-repository"></a>Genel bir havuz oluşturma

İçinde *DAL* klasörü oluşturmak *GenericRepository.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Sınıf değişkenleri için depo örneği varlık kümesini ve veritabanı bağlamı için bildirilen:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Oluşturucu bir veritabanı bağlamını örneği kabul eder ve varlık kümesi değişkeni başlatır:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Yöntemini çağıran kodu bir filtre koşulu ve sonuçlarına göre sıralamak için sütun belirtmek izin vermek için lambda ifadeleri kullanır ve istekli yükleme için Gezinti özellikleri virgülle ayrılmış bir listesini sağlayın çağıran bir dize parametresi sağlar:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kod `Expression<Func<TEntity, bool>> filter` arayan dayalı bir lambda ifadesi sağlayacaktır anlamına gelir `TEntity` türü ve bu ifade bir Boole değeri döndürür. Örneğin, havuz için örneği `Student` arama yöntemi kodda varlık türünü belirtin `student => student.LastName == "Smith` &quot; için `filter` parametresi.

Kod `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` çağıran bir lambda ifadesi sağlayacaktır anlamına da gelir. Ancak bu durumda, deyimi girişine bir `IQueryable` için nesne `TEntity` türü. İfade, sıralı bir sürümünü döndürür `IQueryable` nesnesi. Örneğin, havuz için örneği `Student` arama yöntemi kodda varlık türünü belirtin `q => q.OrderBy(s => s.LastName)` için `orderBy` parametresi.

Kodda `Get` yöntemi oluşturur bir `IQueryable` nesne ve varsa filtre ifadesi geçerlidir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Sonraki virgülle ayrılmış liste ayrıştırmadan sonra eager yükleme ifadeleri geçerlidir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Son olarak, geçerli `orderBy` varsa ifade ve; sonuçları döndürür Aksi takdirde sırasız sorgusundan sonuçları döndürür:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Çağırdığınızda `Get` yöntemi, bunu filtreleme ve sıralama `IEnumerable` parametreleri için bu işlevler sağlayan yerine yöntemi tarafından döndürülen koleksiyonu. Ancak sıralama ve filtreleme iş ardından web sunucusu üzerindeki bellekte uygulanır. Bu parametreleri kullanarak, iş web sunucusu yerine veritabanı tarafından yapıldığından emin olun. Belirli bir varlık türleri için türetilen sınıflar oluşturmak ve özelleştirilmiş eklemek için bir alternatif olan `Get` yöntemleri gibi `GetStudentsInNameOrder` veya `GetStudentsByName`. Ancak, karmaşık bir uygulamada bu çok sayıda gibi türetilen sınıflar ve korumak için daha fazla iş olabilir özel yöntemler neden olabilir.

Kodda `GetByID`, `Insert`, ve `Update` yöntemleri genel olmayan depoya gördüğünüz üzerine benzerdir. (İstekli yükleme parametresinde sağlama olmayan `GetByID` imza ile istekli yükleme yapamayacağı çünkü `Find` yöntemi.)

İki aşırı yüklemeleri için sağlanan `Delete` yöntemi:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Bu olanak tanır birini silinecek yalnızca Kimliğinde varlık geçirebilir ve bir varlık örneğini alır. İçinde anlatıldığı gibi [eşzamanlılık işleme](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) öğretici eşzamanlılık, işleme gerekir bir `Delete` yönteminin bir varlık örneği alan bir izleme özelliğinin özgün değeri içerir.

Bu genel havuzda tipik CRUD gereksinimleri işleyecek. Belirli bir varlık türü daha karmaşık filtreleme veya sıralama gibi özel gereksinimleriniz olduğunda, bu tür için ek yöntemleri sahip türetilmiş bir sınıf oluşturabilirsiniz.

## <a name="creating-the-unit-of-work-class"></a>İş sınıfı birim oluşturma

İş sınıfı biriminin bir amaca hizmet eder: tek veritabanı bağlamı paylaştıkları birden çok depoları kullandığınızda emin olmak için. Böylece, bir iş birimine tamamlandığında çağırabilirsiniz `SaveChanges` bağlam örneğine ilişkin yöntemi ve tüm ilgili değişiklikleri Eşgüdümlü olacaktır gösterilmeyeceği. Tüm bu sınıf gereksinimleri olan bir `Save` yöntemi ve her havuz için bir özellik. Her bir havuz özellik diğer depo örnekleri aynı veritabanı bağlam örneğine kullanılarak örneği bir depo örneğini döndürür.

İçinde *DAL* klasörünü adlı bir sınıf dosyası oluşturma *UnitOfWork.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Sınıf değişkenleri veritabanı bağlamı ve her deposu için kod oluşturur. İçin `context` değişken, yeni bir bağlam örneği:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Her bir havuz özellik deposu zaten var olup olmadığını denetler. Aksi durumda, içerik örneğinde geçirme depo başlatır. Sonuç olarak, tüm depoları aynı bağlam örneğine paylaşır.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Yöntem çağrılarını `SaveChanges` üzerinde veritabanı bağlamı.

Gibi bir sınıf değişkeni veritabanı bağlamında başlatır herhangi bir sınıf `UnitOfWork` uygulayan sınıf `IDisposable` ve bağlam siler.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Depoları ve UnitOfWork sınıfını kullanmak üzere indirmelere denetleyicisi değiştirme

Şu anda sahip kod değiştirin *CourseController.cs* aşağıdaki kod ile:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Bu kod için bir sınıf değişkeni ekler `UnitOfWork` sınıfı. (Arabirimleri burada kullanıyorsanız, değişkeni burada başlatmak olmayacaktır; bunun yerine, yaptığınız gibi iki oluşturucular desenini uygulamak `Student` depo.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Sınıf kalan tüm başvuruları veritabanı bağlamı için uygun depo başvurular değiştirilir kullanarak `UnitOfWork` deposuna erişmek için özellikler. `Dispose` Yöntemi siler `UnitOfWork` örneği.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Siteyi çalıştırın ve tıklayın **kurslar** sekmesi.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Sayfa arar ve aynı değişikliklerinizi önce yapılmış ve diğer indirmelere sayfaları da aynı iş gibi çalışır.

## <a name="summary"></a>Özet

Şimdi depo ve iş desenleri biriminin uyguladık. Lambda ifadeleri, genel havuzda yöntem parametreleri olarak kullandınız. Bu ifadelerle kullanma hakkında daha fazla bilgi için bir `IQueryable` nesne için bkz: [IQueryable(T) arabirimi (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN Kitaplığı'nda. Sonraki Gelişmiş senaryoları Öğreticisi bazı nasıl ele alınacağını öğreneceksiniz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
