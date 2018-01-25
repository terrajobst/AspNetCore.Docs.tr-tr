---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 2: bir iş mantığı katmanı ve birim testleri ekleme | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: df37acd8901b457f7887afe767d42d53e45e4815
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 2: bir iş mantığı katmanı ve birim testleri ekleme
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur. Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide oluşturduğunuz Entity Framework kullanarak bir n katmanlı web uygulaması ve `ObjectDataSource` denetim. Bu öğretici iş mantığı katmanı (BLL) ve veri erişim katmanı (DAL) ayrı tutarken iş mantığı eklemeyi gösterir ve BLL için otomatik birim testleri oluşturmak nasıl gösterir.

Bu öğreticide aşağıdaki görevleri tamamlamanız:

- Gereksinim duyduğunuz veri erişim yöntemleri bildiren bir depo arabirimi oluşturur.
- Depo Arabirimi deposu sınıfında uygulayın.
- Veri erişim işlevleri gerçekleştirmek için depo sınıfını çağıran bir iş mantığı sınıf oluşturun.
- Connect `ObjectDataSource` iş mantığı sınıfı depo sınıfına denetimine.
- Bir birim testi projesi ve bellek içi koleksiyonlar için kendi veri deposu kullanan bir depo sınıfına oluşturun.
- Birim testi iş mantığı sınıfına testi çalıştırmak ve başarısız görmek eklemek istediğiniz iş mantığı için oluşturun.
- İş mantığı iş mantığı sınıfında uygulamanız, sonra birim test etmek ve geçirmek görmek yeniden çalıştırın.

İle çalışma *Departments.aspx* ve *DepartmentsAdd.aspx* önceki öğreticide oluşturduğunuz sayfaları.

## <a name="creating-a-repository-interface"></a>Bir depo arabirimi oluşturma

Depo Arabirimi oluşturarak başlarsınız.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

İçinde *DAL* klasörü, yeni bir sınıf dosyası oluşturun, adlandırın *ISchoolRepository.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Bir yöntem her CRUD için arabirimi tanımlar (oluşturma, okuma, güncelleştirme, silme) deposu sınıfında oluşturulan yöntemleri.

İçinde `SchoolRepository` sınıfını *SchoolRepository.cs*, bu sınıfın uyguladığını gösteren `ISchoolRepository` arabirimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Bir iş mantığı sınıf oluşturma

Ardından, iş mantığı sınıfı oluşturacaksınız. Tarafından çalıştırılan bir iş mantığı ekleyeceği bunu `ObjectDataSource` , henüz yapmayacak olsa da, denetim. Şimdilik, yeni iş mantığı sınıfı yalnızca depo mu aynı CRUD işlemleri gerçekleştirir.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Yeni bir klasör oluşturun ve adlandırın *BLL*. (Bir gerçek uygulamasında iş mantığı katmanı genellikle bir sınıf kitaplığı uygulanması — ayrı bir proje — ancak bu öğreticide basit tutmak için bir proje klasöründe BLL sınıfları tutulacak.)

İçinde *BLL* klasörü, yeni bir sınıf dosyası oluşturun, adlandırın *SchoolBL.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Bu kod, önceki depo sınıfında gördüğünüz aynı CRUD yöntemlerini oluşturur, ancak Entity Framework yöntemleri doğrudan erişim yerine depo sınıfı yöntemleri çağırır.

Depo sınıfını başvuru tutan sınıfı değişkeni bir arabirim türü tanımlanır ve depo sınıfını başlatır kod iki oluşturucular içinde yer alır. Parametresiz bir kurucusu tarafından kullanılan `ObjectDataSource` denetim. Bir örneğini oluşturur `SchoolRepository` daha önce oluşturduğunuz sınıfı. Diğer oluşturucuyu deposu arabirimini uygulayan bir nesneyi içeri aktarmanız iş mantığı sınıfı başlatır ne olursa olsun kodu sağlar.

Depo sınıfını ve iki oluşturucular çağıran CRUD yöntemlerini seçtiğiniz arka uç veri deposuyla iş mantığı sınıfını kullanmak mümkün kılar. İş mantığı sınıfı çağırma sınıf verileri nasıl devam haberdar olmanız gerekmez. (Bu adlandırılırlar *Kalıcılık kullanmayan*.) Bir şey basit kullanan bir depo uygulaması için iş mantığı sınıfı bağlanabildiğinden bu kolaylaştıran birim testi, bellek içi olarak `List` verileri depolamak için koleksiyonları.

> [!NOTE]
> Teknik olarak, Entity Framework'ın devral sınıflardan örneği için varlık nesnesi hala değil Kalıcılık-kullanmayan, `EntityObject` sınıfı. Tam Kalıcılık kullanmayan için kullandığınız *düz eski CLR nesnelerini*, veya *POCOs*, devralınan nesneleri yerine `EntityObject` sınıfı. POCOs kullanarak Bu öğretici kapsamında değildir. Daha fazla bilgi için bkz: [Sınanabilirlik ve Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN Web sitesinde.)


Bağlayabilirsiniz artık `ObjectDataSource` iş mantığı denetimlere depoya sınıfı yerine ve önce yaptığınız gibi her şeyi çalıştığını doğrulayın.

İçinde *Departments.aspx* ve *DepartmentsAdd.aspx*, her oluşumu değiştirmek `TypeName="ContosoUniversity.DAL.SchoolRepository"` için `TypeName="ContosoUniversity.BLL.SchoolBL`". (Dört örneği vardır tümünde.)

Çalıştırma *Departments.aspx* ve *DepartmentsAdd.aspx* önceden olduğu gibi yine çalıştıklarını doğrulamak için sayfaları.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Bir birim sınama projesini ve depo uygulaması oluşturma

Çözümü kullanarak yeni bir proje eklemek **Test projesi** şablonu ve adlandırın `ContosoUniversity.Tests`.

Oluşturduğunuz test projesinin bir başvuru ekleyin `System.Data.Entity` ve proje başvurusu ekleyin `ContosoUniversity` projesi.

Artık ile birim testleri kullanacaksınız deposu sınıfı oluşturabilirsiniz. Bu depo için veri deposu içinde sınıfı olacaktır.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Yeni bir sınıf dosyası test projesi oluşturun, adlandırın *MockSchoolRepository.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Bu depo sınıfına Entity Framework doğrudan erişir hesapla aynı CRUD yöntemlerine sahiptir, ancak bunlar çalışmak `List` yerine bellekte bir veritabanı ile koleksiyonları. Bu ayarlama ve iş mantığı sınıfı için birim testleri doğrulamak için bir test sınıf kolaylaştırır.

## <a name="creating-unit-tests"></a>Birim testleri oluşturma

**Test** proje şablonu sizin için bir saplama birim testi sınıfı oluşturulan ve sonraki göreviniz birim test yöntemleri iş mantığı sınıfına eklemek istediğiniz iş mantığı için ekleyerek bu sınıfı değiştirin.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso University'deki herhangi tek tek Eğitmen yalnızca tek bir bölüm yönetici olabilir ve bu kural zorlamak için iş mantığı eklemeniz gerekir. Testleri ekleme ve bunları başarısız görmek için testleri çalıştırmadan başlayacaktır. Sonra kodu ekleyin ve bunları geçirmek görmeyi testleri yeniden çalıştırın.

Açık *UnitTest1.cs* dosya ve ekleme `using` deyimleri ContosoUniversity projesinde oluşturulan iş mantığı ve veri erişim katmanı için:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Değiştir `TestMethod1` aşağıdaki yöntemlerden yöntemiyle:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Yöntemi, daha sonra bir iş mantığı sınıfının yeni bir örneğini geçirir proje birim testi için oluşturduğunuz havuzu sınıfının bir örneğini oluşturur. Yöntemi, iş mantığı sınıfı sonra kullanabileceğiniz üç Departmanlar test yöntemleri eklemek için kullanır.

Test yöntemleri birisi aynı yönetici mevcut bir bölümü olarak ile yeni bir bölüm eklemek çalışırsa veya birisi bir kişinin Kimliğini ayarlayarak bir departmandaki yönetici güncelleştirmeye çalışırsa, iş mantığı sınıfı bir özel durum oluşturur doğrulayın kimin zaten başka bir departman yöneticisidir.

Bu kod derlenmez için özel durum sınıfı henüz oluşturmadınız. Derlemek için almak için sağ `DuplicateAdministratorException` seçip **Generate**ve ardından **sınıfı**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Bu sınıf, silebilirsiniz test projesinde oluşturur ana projede özel durum sınıfı oluşturduktan sonra. iş mantığı uygulamıştır.

Test projesini çalıştırın. Beklendiği gibi sınama başarısız.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Bir Test geçişi yapmak için iş mantığı ekleme

Ardından, zaten başka bir bölümünün yönetici haklarına sahip birisi bir bölüm yönetici olarak ayarlamak mümkün kılan iş mantığı uygulamanız. İş mantığı katmanından bir özel durum ve bir kullanıcı bir bölüm düzenler ve tıklar sunu katmanı'catch **güncelleştirme** zaten bir yönetici haklarına sahip birisi seçtikten sonra. (Eğitmen sayfayı oluşturmak önce olan zaten Yöneticiler aşağı açılan listeden kaldırabilirsiniz, ancak burada iş mantığı katmanı ile çalışmak için amaçtır.)

Bir kullanıcı birden fazla bölüm Yöneticisi bir eğitmen yapmaya çalıştığında throw özel durum sınıfı oluşturarak başlayın. Yeni bir sınıf dosyasında ana proje oluşturma *BLL* klasörü adlandırın *DuplicateAdministratorException.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Şimdi geçici silme *DuplicateAdministratorException.cs* test projesinde önceden derlemek için oluşturduğunuz dosya.

Ana projeyi açın *SchoolBL.cs* dosya ve Doğrulama mantığı içeren aşağıdaki yöntemi ekleyin. (Kod daha sonra oluşturacaksınız bir yöntemi gösterir.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Ekleme veya güncelleştirme bu yöntemi çağırmanız `Department` başka bir bölüm zaten aynı yönetici olup olmadığını denetlemek için varlıklar.

Kod için veritabanını aramak için bir yöntem çağırır bir `Department` aynı olan varlık `Administrator` varlık olarak özellik değeri eklenir veya güncelleştirilir. Biri bulunursa, bir özel durum kodu oluşturur. Eklenen veya güncelleştirilen varlığı Hayır varsa, doğrulama denetiminin gereklidir `Administrator` değeri ve hiçbir özel durum oluşur güncelleştirme sırasında yöntemi çağrıldıysa ve `Department` eşleşme buldu varlık `Department` güncelleştirilen varlık.

Yeni yöntemi çağırın `Insert` ve `Update` yöntemleri:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

İçinde *ISchoolRepository.cs*, aşağıdaki yeni veri erişimi yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

İçinde *SchoolRepository.cs*, aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

İçinde *SchoolRepository.cs*, aşağıdaki yeni veri erişimi yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Bu kod alır `Department` belirtilen yönetici varlıklar. Yalnızca bir bölüm (varsa) bulunması. Hiçbir kısıtlama veritabanına oluşturulduğundan, durumunda birden çok Departmanlar bulundu ancak, dönüş türü bir koleksiyondur.

Varsayılan olarak, nesne bağlamına veritabanından varlıklar aldığında, bunları kendi nesne durum Yöneticisi'nde izler. `MergeOption.NoTracking` Parametresi, bu izleme bu sorgu için yapılan değil belirtir. Sorgu, güncelleştirmeye çalıştığınız tam varlık döndürebilir gerekli olmasıdır ve ardından o varlık eklemek mümkün olmayacaktır. Geçmiş departmanında düzenlerseniz, örneğin, *Departments.aspx* sayfası ve yönetici değiştirmeden bırakın, bu sorgu döndürecektir geçmişi departman. Varsa `NoTracking` , nesne bağlamı zaten sahip olabilir geçmişi departmanı varlık, nesne durumu Yöneticisi'nde, ayarlı değil. Görünüm durumundan yeniden oluşturulacak geçmiş departmanı varlık taktığınızda, nesne bağlamına bildiren bir özel durum sonra `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Belirtme alternatif olarak `MergeOption.NoTracking`, yalnızca bu sorgu için yeni bir nesne bağlamına oluşturabilirsiniz. Yeni nesne bağlamı kendi nesne durumu Yöneticisi gerekir çünkü olurdu çakışma çağırdığınızda `Attach` yöntemi. Yaşanan performans sorunları alternatif Bu yaklaşımın en az olacak şekilde yeni nesne bağlamı özgün nesne bağlamı ile meta verileri ve veritabanı bağlantısı paylaşmak. Burada gösterilen yaklaşım ancak sunmaktadır `NoTracking` seçeneği, hangi diğer bağlamlarda yararlı bulabilirsiniz. `NoTracking` Seçeneği ele alınmıştır bu serideki sonraki öğretici daha ileri.)

Test projesinde yeni veri erişim yöntemine ekleyin *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Bu kod, aynı veri seçimi gerçekleştirmek için LINQ kullanır, `ContosoUniversity` proje deposu LINQ to Entities için kullanır.

Oluşturduğunuz test projesinin yeniden çalıştırın. Bu süre testleri geçirin.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource özel durumları işleme

İçinde `ContosoUniversity` çalıştırmak proje *Departments.aspx* sayfasında ve zaten başka bir bölüm için yönetici haklarına sahip birisi için bir bölüm Yöneticisi değiştirmeyi deneyin. (Veritabanı geçersiz veri ile önceden geldiğinden sırasında Bu öğreticide, eklediğiniz Departmanlar yalnızca düzenleyebilirsiniz unutmayın.) Aşağıdaki sunucu hata sayfası alın:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Hata işleme kodu eklemeniz gerekir böylece bu tür bir hata sayfası, kullanıcıların istemezsiniz. Açık *Departments.aspx* ve belirtmek için bir işleyici `OnUpdated` olayı `DepartmentsObjectDataSource`. `ObjectDataSource` Etiketi şimdi açma, aşağıdaki örnekte benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

İçinde *Departments.aspx.cs*, aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Aşağıdaki işleyicisi ekleme `Updated` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Varsa `ObjectDataSource` denetim güncelleştirme gerçekleştirmeye çalışırken bir özel durum yakalar, olay bağımsız değişkeninde özel durum geçirir (`e`) bu işleyicisi. İşleyici kodda özel durum yinelenen yönetici özel durum olup olmadığını denetler. Varsa, kodu için bir hata iletisi içeren bir doğrulayıcı denetimi oluşturur `ValidationSummary` görüntülemek için denetimi.

Sayfayı çalıştırın ve birisi iki Departmanlar yönetici yeniden olun girişimi. Bu süre `ValidationSummary` denetimi, bir hata iletisi görüntüler.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Benzer değişiklik *DepartmentsAdd.aspx* sayfası. İçinde *DepartmentsAdd.aspx*, belirtmek için bir işleyici `OnInserted` olayı `DepartmentsObjectDataSource`. Sonuçta elde edilen biçimlendirme, aşağıdaki örnekte benzeyecektir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

İçinde *DepartmentsAdd.aspx.cs*, aynı eklemek `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Aşağıdaki olay işleyicisini ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Artık test *DepartmentsAdd.aspx.cs* , aynı zamanda doğru kişi birden fazla bölüm yönetici yapma denemelerini işleme doğrulamak için sayfa.

Bu depo düzenini kullanarak için uygulama giriş tamamlar `ObjectDataSource` denetimi Entity Framework ile. Depo düzeni ve Test Edilebilirlik hakkında daha fazla bilgi için MSDN teknik incelemesine bakın [Sınanabilirlik ve Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Aşağıdaki öğreticide sıralama ve filtreleme uygulamaya işlevselliği ekleme görürsünüz.

>[!div class="step-by-step"]
[Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[sonraki](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
