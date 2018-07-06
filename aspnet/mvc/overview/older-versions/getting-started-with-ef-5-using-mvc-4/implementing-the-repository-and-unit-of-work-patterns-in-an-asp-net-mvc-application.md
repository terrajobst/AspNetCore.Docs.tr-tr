---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (9, 10) depo ve iş birimi desenleri uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9de8b33e4c533b90b7653544a6814d1ee756cf50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814575"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Bir ASP.NET MVC uygulamasındaki (9, 10) depo ve iş birimi desenleri uygulama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide gereksiz kod içinde azaltmak için devralma kullanılan `Student` ve `Instructor` varlık sınıfları. Bu öğreticide, CRUD işlemleri için depo ve iş birimi desenleri kullanmak için bazı yollar görürsünüz. Önceki olduğu gibi bu birinde, kodunuzu, zaten oluşturulan yeni sayfaları oluşturmak yerine sayfalarıyla çalışma biçimini değiştireceğiz.

## <a name="the-repository-and-unit-of-work-patterns"></a>Depo ve iş birimi desenleri

Depo ve iş birimi desenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasında bir Soyutlama Katmanı oluşturmak için tasarlanmıştır. Bu desenleri uygulama veri deposundaki değişiklikleri uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya test odaklı geliştirme (TDD) kolaylaştırabilir.

Bu öğreticide her varlık türü için bir depo sınıfına uygulayacaksınız. İçin `Student` varlık türü bir depo arabirimi ve bir depo sınıfına oluşturacaksınız. Depo içinde denetleyicinizin başlattığınızda, böylece denetleyici depo arabirimi uygulayan herhangi bir nesneye bir başvuru kabul arabirimi kullanacaksınız. Denetleyici altında bir web sunucusunda çalıştığında, Entity Framework ile birlikte çalışan bir depoyu alır. Denetleyici bir birim test sınıfı altında çalıştığında, bir bellek içi koleksiyonu gibi test etmek için kolayca işleyebileceğiniz şekilde depolanan verilerle çalışır bir depoyu alır.

Öğreticide daha sonra birden çok depo ve iş sınıfı için bir birim kullanacaksınız `Course` ve `Department` varlık türleri içinde `Course` denetleyicisi. İş sınıfı birimi tümünün tarafından paylaşılan bir tek veritabanı bağlamı sınıfının oluşturarak birden çok deposu çalışması düzenler. Otomatik birim testi gerçekleştirmek istiyorsanız, oluşturun ve arabirimler için bu sınıflar için yaptığınız gibi kullanın `Student` depo. Ancak, öğreticiyi basit tutmak için oluşturma ve bu sınıfların arabirimlerini kullanın.

Aşağıdaki çizim, denetleyici ve depoyu veya çalışma deseni birimi hiç kullanılmıyor karşılaştırılan bağlam sınıflar arasındaki ilişkileri kavramsallaştırmanın yollarından biri gösterilmektedir.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Bu öğretici serisinde birim testleri oluşturmaz. Depo desen kullanan bir MVC uygulaması ile TDD giriş için bkz: [izlenecek yol: ASP.NET MVC ile kullanma TDD](https://msdn.microsoft.com/library/ff847525.aspx). Depo düzeni hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Depo düzeni](https://msdn.microsoft.com/library/ff649690.aspx) MSDN'de.
- [Entity Framework 4.0 ile depo ve iş birimi düzenleri kullanarak](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) Entity Framework ekip blogunda.
- [Çevik Entity Framework 4 depo](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) Julie Lerman'ın blog gönderilerine dizi.
- [Bir bakışta HTML5/jQuery uygulama hesabı oluşturmaya](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) Dan Wahlin'ın blogunda.

> [!NOTE]
> Depo ve iş birimi desenleri uygulamak için birçok yolu vardır. Depo sınıfları ile veya olmadan iş sınıfı birimi kullanabilirsiniz. Tüm varlık türlerini veya her tür için tek bir depoda uygulayabilirsiniz. Her tür için bir tane uygularsanız, ayrı sınıfları, genel bir temel sınıf ve türetilen sınıflar veya soyut bir temel sınıf ve türetilen sınıflar kullanabilirsiniz. İş mantığı deponuza dahil edebilir veya veri erişim mantığını kısıtlarız. Kullanarak, veritabanı bağlamı sınıfının bir Soyutlama Katmanı oluşturabilirsiniz [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) var. yerine arabirimleri [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) varlık kümeleriniz için türleri. Bu öğreticide gösterilen bir Soyutlama Katmanı yaklaşım dikkate almanız için değil tüm senaryolar ve ortamlar için bir öneri bir seçenektir.


## <a name="creating-the-student-repository-class"></a>Öğrenci depo sınıfı oluşturma

İçinde *DAL* klasöründe adlı bir sınıf dosyası oluşturma *IStudentRepository.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Bu kod, iki okuma yöntemleri dahil olmak üzere CRUD yöntemleri, normal bir dizi bildirir; tüm döndüren bir `Student` varlıkları ve tek bir bulur bir `Student` varlığı kimliğe göre

İçinde *DAL* klasör adında bir sınıf dosyası oluşturma *StudentRepository.cs* dosya. Varolan kodu uygulayan aşağıdaki kodla değiştirin `IStudentRepository` arabirimi:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Veritabanı bağlamı sınıfının değişkeninde tanımlanır ve oluşturucu bağlamının bir örneğine geçirmek için çağrı nesnesi bekliyor:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Depoya yeni bir bağlamda örneği oluşturulamadı, ancak bir denetleyici birden fazla depoyla kullandıysanız, ardından her bir ayrı bağlamıyla sonunda. Birden fazla depoyla daha sonra kullanacağınız `Course` denetleyicisi ve göreceksiniz nasıl bir birim iş sınıfın tüm depolar aynı bağlam kullanmasını sağlayabilirsiniz.

Depo uygulayan [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) ve veritabanı bağlamı denetleyicisi daha önce gördüğünüz ve CRUD yöntemlerinden daha önce gördüğünüzle aynı şekilde aramalarda veritabanı bağlamını siler.

## <a name="change-the-student-controller-to-use-the-repository"></a>Depo kullanmak üzere Öğrenci denetleyicisini değiştirme

İçinde *StudentController.cs*, şu anda sınıfında kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Denetleyici artık uygulayan bir nesne için bir sınıf değişken bildirir `IStudentRepository` arabirimi yerine bağlamı sınıfı:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Varsayılan (parametresiz) Oluşturucu yeni bir bağlam örneği oluşturur ve çağıran bir bağlam örneğinde geçirmek isteğe bağlı bir oluşturucu sağlar.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Kullandıysanız *bağımlılık ekleme*, veya bir DI, mıydı varsayılan oluşturucu doğru depoya nesne her zaman sağlanan DI yazılım olun çünkü.)

CRUD yöntemlerdeki depo bağlamı yerine artık çağrılır:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Ve `Dispose` yöntemi artık bağlam yerine depoyu siler:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Siteyi çalıştırın ve tıklayın **Öğrenciler** sekmesi.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Sayfa görünümünü ve değiştirdiğiniz deposuna kullanmak için kodu ve diğer Öğrenci sayfaları da aynı çalışmadan önce yaptığınız gibi aynı şekilde çalışır. Ancak, yolunda önemli bir fark yoktur `Index` denetleyicinin yöntemi, filtreleme ve sıralama yapar. Aşağıdaki kod bu yöntemin özgün sürümle yer:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Güncelleştirilmiş `Index` yöntem aşağıdaki kodu içerir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Yalnızca vurgulanmış kodu değişti.

Kod özgün sürümünde `students` olarak yazılmış bir `IQueryable` nesne. Gibi bir yöntem kullanılarak bir koleksiyona dönüştürüldü kadar sorgu veritabanına gönderilen değil `ToList`, hangi oluşmaz kadar dizin görünümünün Öğrenci modeli erişir. `Where` Yukarıdaki özgün koda yönteminde olur bir `WHERE` veritabanına gönderilen SQL sorgu yan tümcesi. Bu sırayla yalnızca seçili varlıklar veritabanı tarafından döndürülür anlamına gelir. Ancak, değiştirilmesi sonucunda `context.Students` için `studentRepository.GetStudents()`, `students` sonra bu ifade bir değişken bir `IEnumerable` veritabanındaki tüm Öğrenciler içeren koleksiyonu. Son sonucu `Where` yöntemi aynıdır, ancak artık iş bellek web sunucusundaki ve veritabanı tarafından gerçekleştirilir. Büyük miktarda veriyi döndüren sorgular için bu verimsiz olabilir.

> [!TIP]
> 
> **Iqueryable vs. IEnumerable**
> 
> Bir şey girmiş olsa bile, depo burada gösterildiği gibi uyguladıktan sonra **arama** kutusu SQL sunucusuna gönderilen sorgu ölçütlerinizle içermediği için tüm Öğrenci satırları döndürür:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Arama ölçütleriyle bilmeden depo sorgu çalıştırılmış olduğundan bu sorgu tüm Öğrenci verileri döndürür. Sıralama, arama ölçütlerini uygulamak ve bellek disk belleği (Bu örnekte yalnızca 3 satır gösteriliyor) yapılır için verilerin bir alt kümesini seçme işleminin sonraki olduğunda `ToPagedList` yöntemi çağrıldığında `IEnumerable` koleksiyonu.
> 
> Arama ölçütleri uyguladıktan sonra kodu (depo uygulanmış önce) önceki sürümünde, sorgu kadar veritabanına gönderilmez olduğunda `ToPagedList` üzerinde çağrılır `IQueryable` nesne.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Ne zaman ToPagedList çağrıldığında bir `IQueryable` nesnesi, hiçbir filtreleme bellekte yapılması gerektiğini SQL sunucusuna gönderilen sorgu, arama dizesi belirtir ve yalnızca arama ölçütleri karşılayan satırların sonuç olarak döndürülür.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Şu öğretici SQL sunucusuna gönderilen sorguların açıklanmaktadır.)


Aşağıdaki bölümde, bu iş veritabanı tarafından yapılması gerektiğini belirtmenize olanak verir depo yöntemleri gösterilmektedir.

Denetleyici ve Entity Framework veritabanı bağlamı arasında bir Soyutlama Katmanı oluşturdunuz. Otomatik birim bu uygulamayla testi gerçekleştirmek için oluşturacağınız, bir alternatif bir depo sınıfına uygulayan bir birim testi projesine oluşturabilirsiniz `IStudentRepository` *.* Veri okuma ve yazma için bağlamı çağırmak yerine bu sahte bir depo sınıfına bellek içi koleksiyonları denetleyicisi işlevlerini test etmek için yönlendirme.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Genel depo ve iş sınıfı bir birimi uygulayın

Her varlık türü için bir depo sınıfına oluşturma gereksiz kod içinde birçok neden olabilir ve kısmi güncelleştirmeleri neden olabilir. Örneğin, iki farklı varlık türleri aynı işlemin bir parçası güncelleştirmek olduğunu varsayalım. Her bir ayrı bir veritabanı bağlamını örneği kullanıyorsa, biri başarılı olabilir ve diğer başarısız olabilir. Gereksiz kod en aza indirmek için bir yol olan genel bir depo ve emin olmanın bir yolu kullanmak için tüm depolar aynı veritabanı bağlamını kullanır (ve bu nedenle tüm güncelleştirmeleri koordine) iş sınıfı birimi kullanılacak olmasıdır.

Öğreticinin bu bölümünde, oluşturacağınız bir `GenericRepository` sınıfı ve `UnitOfWork` sınıfı ve bunları kullanmak `Course` hem erişim için denetleyici `Department` ve `Course` varlık kümeleri. Öğreticinin bu bölümünde basit tutmak için daha önce açıklandığı şekilde, bu sınıflar arabirimleri oluşturma değildir. Ancak bunları TDD kolaylaştırmak için kullanmayı düşünüyor, genellikle bunları arabirimleriyle aynı şeklinden uygulamak `Student` depo.

### <a name="create-a-generic-repository"></a>Genel bir depo oluşturun

İçinde *DAL* klasör oluşturma *GenericRepository.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Sınıf değişkenleri veritabanı bağlamının ve havuz için örneği bir varlık kümesi için belirtilir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Oluşturucu, bir veritabanı bağlamını örneği kabul eder ve varlık kümesi değişkeni başlatır:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Yöntemini çağıran kod, bir filtre koşulu ve sonuçları sıralamak için sütun belirtmek izin vermek için lambda ifadeleri kullanır ve istekli yükleme için virgülle ayrılmış bir gezinti özelliklerinin listesini sağlayan arayan bir dize parametresi sağlar:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kod `Expression<Func<TEntity, bool>> filter` çağıran bir lambda ifadesine göre sağlayacaktır anlamına gelir `TEntity` türü ve bu ifade bir Boole değeri döndürür. Örneğin, havuz için örneği varsa `Student` yöntemi çağrılırken kodda varlık türü belirtin `student => student.LastName == "Smith` &quot; için `filter` parametresi.

Kod `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` çağıran bir lambda ifadesi sağlayacağı anlamına da gelir. Ancak bu durumda, ifade giriştir bir `IQueryable` nesnesi `TEntity` türü. İfade, sıralı bir sürümünü döndürür `IQueryable` nesne. Örneğin, havuz için örneği varsa `Student` yöntemi çağrılırken kodda varlık türü belirtin `q => q.OrderBy(s => s.LastName)` için `orderBy` parametresi.

Kodda `Get` yöntemi oluşturur bir `IQueryable` nesne ve filtre ifadesi varsa uygular:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Sonraki virgülle ayrılmış Liste Ayrıştırma sonra eager yükleme ifadeleri geçerlidir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Son olarak, geçerli `orderBy` varsa ifade ve sonuçları; döndürür aksi sırasız sorgunun sonuçlarını döndürür:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Çağırdığınızda `Get` yöntemi yaptığınız filtreleme ve sıralama `IEnumerable` bu işlevler için parametreleri sağlamak yerine yöntemi tarafından döndürülen koleksiyon. Ancak, sıralama ve filtreleme iş ardından web sunucusu üzerindeki bellekte uygulanır. Bu parametreleri kullanarak, web sunucusu yerine veritabanı iş yapıldığından emin olun. Belirli bir varlık türleri için türetilen sınıflar oluşturabilir ve özel bir alternatifidir `Get` yöntemleri gibi `GetStudentsInNameOrder` veya `GetStudentsByName`. Ancak, karmaşık bir uygulamada bu çok sayıda gibi türetilmiş sınıflar ve korumak için daha fazla iş olabilecek özel yöntemler sonuçlanabilir.

Kodda `GetByID`, `Insert`, ve `Update` yöntem genel olmayan depoda gördüğünüz üzerine benzerdir. (Bir istekli yükleme parametresi sağlayarak olmayan `GetByID` imza ile istekli yükleme yapamazsınız çünkü `Find` yöntemi.)

İki aşırı yükleme için sağlanan `Delete` yöntemi:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Bu sağlar biri silinecek yalnızca kimliği varlık içinde geçirirsiniz ve bir varlık örneğini alır. Gördüğünüz gibi [eşzamanlılık işleme](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) işleme, eşzamanlılık gereksinim için öğretici bir `Delete` varlık örneğini alan yöntemi izleme özelliği özgün değeri içerir.

Bu genel deponun tipik CRUD gereksinimleri işler. Bir özel varlık türü gibi daha karmaşık filtreleme veya sıralama, özel gereksinimleri varsa, ek yöntemleri türüne sahip türetilmiş bir sınıf oluşturabilirsiniz.

## <a name="creating-the-unit-of-work-class"></a>İş sınıfı birimini oluşturma

İş sınıfı birimi bir amaca hizmet eder: birden çok deposu kullandığınızda emin olmak için bunlar bir tek veritabanı bağlamını paylaşabilir. Böylece, iş birimi tamamlandığında çağırabilirsiniz `SaveChanges` yöntemi örneğine ilişkin bağlam ve tüm ilgili değişiklikler Eşgüdümlü olacaktır garanti. Tüm bu sınıfı gereksinimleri olan bir `Save` yöntemi ve her bir depo bir özellik. Her bir depo özellik bir depo örnekleri olarak aynı veritabanı bağlamı örneği kullanılarak oluşturulmuş bir depo örneği döndürür.

İçinde *DAL* klasöründe adlı bir sınıf dosyası oluşturma *UnitOfWork.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Bu kod, veritabanı bağlamı ve her deponun sınıfı değişkenlerini oluşturur. İçin `context` yeni bir bağlam değişkeni oluşturulur:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Her bir depo özellik deposu zaten var olup olmadığını denetler. Aksi durumda, bağlam örneğinde geçirerek havuzu oluşturur. Sonuç olarak, tüm depolar, aynı bağlam örneğinin paylaşın.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Yöntem çağrılarını `SaveChanges` üzerinde veritabanı bağlamı.

Gibi bir sınıf değişkeni veritabanı bağlamında başlatan herhangi bir sınıf `UnitOfWork` sınıfının Implements `IDisposable` ve bağlam siler.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>UnitOfWork sınıfı ve depoları kullanılacak kurs denetleyicisini değiştirme

Şu anda sahip kodu değiştirin *CourseController.cs* aşağıdaki kod ile:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Bu kod için bir sınıf değişken ekler `UnitOfWork` sınıfı. (Arabirimler burada kullandıysanız, burada değişkeni başlatmak mıydı; bunun yerine, iki Oluşturucu desenini uygulamak yaptığınız gibi `Student` depo.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Sınıf kalanında veritabanı bağlamı tüm başvurularını uygun depoyu başvuruları değiştirilir kullanarak `UnitOfWork` depoya erişmek için özelliklere. `Dispose` Yöntemi siler `UnitOfWork` örneği.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Siteyi çalıştırın ve tıklayın **kursları** sekmesi.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Sayfası görünür ve aynı önce yaptığınız değişiklikleri yaptık ve diğer kurs sayfaları da aynı şekilde işler gibi çalışır.

## <a name="summary"></a>Özet

Şimdi, hem depoya hem de iş birimi desenleri uyguladınız. Lambda ifadeleri, genel deponun yöntemi parametreler olarak kullandınız. Bu ifadeler ile kullanma hakkında daha fazla bilgi için bir `IQueryable` nesne, bkz: [IQueryable(T) arabirimi (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN Kitaplığı'nda. Sonraki Gelişmiş senaryoları Öğreticisi bazı nasıl ele alınacağını öğreneceksiniz.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
