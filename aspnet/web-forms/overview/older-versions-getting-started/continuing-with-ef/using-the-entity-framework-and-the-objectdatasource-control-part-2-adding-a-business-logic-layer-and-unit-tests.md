---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 2: iş mantığı katmanı ve birim testleri ekleme | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, kullanmaya başlama Entity Framework 4.0 öğretici serisinin tarafından oluşturulan Contoso University web uygulaması oluşturur. BEN...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6517f037a03bb520ee4f3b3185a255b0eaa9de9a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753162"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, bölüm 2: iş mantığı katmanı ve birim testleri ekleme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur. Öğreticileri hakkında sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Entity Framework kullanarak n katmanlı web uygulaması, önceki öğreticide oluşturduğunuz ve `ObjectDataSource` denetimi. Bu öğreticide iş mantığı katmanı (BLL) ve veri erişim katmanı (DAL) ayrı tutarken iş mantığı eklemek gösterilir ve BLL için otomatik birim testleri oluşturma işlemini gösterir.

Bu öğreticide aşağıdaki görevleri tamamlamanız:

- Gereksinim duyduğunuz veri erişim yöntemlerine bildiren bir depo arabirimi oluşturun.
- Depo sınıfında depo arabirim uygular.
- Veri erişimi işlevleri gerçekleştirmek için depo sınıfını çağıran bir iş mantığı sınıf oluşturun.
- Connect `ObjectDataSource` depo sınıfına yerine iş mantığı sınıfına denetimi.
- Bir birim testi projesi ve kendi veri deposu için bellek içi koleksiyonları kullanır depo sınıf oluşturun.
- Birim testi için test çalıştırması ve başarısız görmek iş mantığı sınıfa eklemek istediğiniz iş mantığını oluşturun.
- İş mantığı sınıfında iş mantığını uygular ve birim testi ve geçer bkz yeniden çalıştırın.

Birlikte çalışma *Departments.aspx* ve *DepartmentsAdd.aspx* önceki öğreticide oluşturulan sayfaları.

## <a name="creating-a-repository-interface"></a>Bir depo arabirimi oluşturma

Depo Arabirimi oluşturarak başlarsınız.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

İçinde *DAL* klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *ISchoolRepository.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Bir yöntem her CRUD için arabirimi tanımlar (oluşturma, okuma, güncelleştirme ve silme) depo sınıfında oluşturduğunuz yöntemleri.

İçinde `SchoolRepository` sınıfını *SchoolRepository.cs*, bu sınıfın uyguladığı belirtmek `ISchoolRepository` arabirimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Bir iş mantığı sınıfı oluşturma

Ardından, iş mantığı sınıfı oluşturmayı öğreneceksiniz. Tarafından yürütülecek iş mantığı ekleyebilirsiniz, böylece bunu `ObjectDataSource` henüz bunu değil olsa da, denetim. Şimdilik, yeni bir iş mantığı sınıf yalnızca depo yapan aynı CRUD işlemleri gerçekleştirir.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Yeni bir klasör oluşturun ve adlandırın *BLL*. (Gerçek bir uygulamada, iş mantığı katmanı genellikle bir sınıf kitaplığı uygulanması — ayrı proje; ancak bu öğreticide basit tutmak için bir proje klasöründe BLL sınıfları tutulacak.)

İçinde *BLL* klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *SchoolBL.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Bu kod depo sınıfında daha önce gördüğünüz aynı CRUD yöntemleri oluşturur, ancak Entity Framework yöntemlerini doğrudan erişmek yerine depo sınıfın yöntemlerini çağırır.

Depo sınıfına bir başvuru tutan sınıfı değişkeni bir arabirim türü tanımlanır ve depo sınıfını başlatan kodu iki oluşturucu içinde yer alır. Parametresiz oluşturucusu tarafından kullanılan `ObjectDataSource` denetimi. Örneği oluşturur `SchoolRepository` daha önce oluşturduğunuz sınıfı. Diğer Oluşturucu depo arabirimi uygulayan herhangi bir nesne geçirilecek iş mantığı sınıfı örneğini oluşturan hangi kod sağlar.

Depo sınıfını ve iki Oluşturucu çağırmak CRUD yöntemleri seçtiğiniz arka uç veri deposuyla iş mantığı sınıfı kullanmayı mümkün kılar. İş mantığı sınıfı, bunu çağırma sınıf verileri nasıl kalıcı dikkat etmeniz gerekmez. (Buna genellikle denir *Kalıcılık ignorance*.) İş mantığı sınıfı bir şey kullanan basit bir depo uygulaması kurabildiğinden birim testi, bunu kolaylaştırır. bellek içi olarak `List` verileri depolamak için koleksiyon.

> [!NOTE]
> Entity Framework'ın devralan sınıflardan örneği olduğundan varlık nesnesi teknik olarak hala değil Kalıcılık-ignorant, `EntityObject` sınıfı. Tam Kalıcılık ignorance için kullanabileceğiniz *düz eski CLR nesnelerini*, veya *POCOs*, devralınan nesneleri yerine `EntityObject` sınıfı. Bu öğreticinin kapsamı dışındadır POCOs kullanmaktır. Daha fazla bilgi için [Sınanabilirlik ve Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN Web sitesindeki.)


Şimdi bağlanabilirsiniz `ObjectDataSource` iş mantığı denetimlere depoya sınıfı yerine ve önceden yaptığınız gibi her şeyin çalıştığını doğrulayın.

İçinde *Departments.aspx* ve *DepartmentsAdd.aspx*, her bir yinelemesini değiştirmek `TypeName="ContosoUniversity.DAL.SchoolRepository"` için `TypeName="ContosoUniversity.BLL.SchoolBL`". (Dört örnekler vardır tüm.)

Çalıştırma *Departments.aspx* ve *DepartmentsAdd.aspx* Öncekine gibi bunlar hala çalıştığını doğrulamak için sayfa.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Bir birim sınama projesini ve depo uygulaması oluşturma

Çözümü kullanarak yeni bir proje ekleyin **Test projesi** şablonunu ve adlandırın `ContosoUniversity.Tests`.

Test projesinde başvuru eklemek `System.Data.Entity` ve bir proje başvurusu Ekle `ContosoUniversity` proje.

Artık, birim testleri ile kullanacağınız depo sınıfını da oluşturabilirsiniz. Bu depo için veri deposu içindeki sınıf olacaktır.

[![image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Test projesinde yeni bir sınıf dosyası oluşturun, adlandırın *MockSchoolRepository.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Entity Framework doğrudan erişir biri olarak aynı CRUD yöntemler bu depo sınıfına sahip, ancak birlikte çalıştıkları `List` bir veritabanı yerine bellekle koleksiyonları. Bu, ayarlamak ve iş mantığı sınıfı için birim testleri doğrulamak bir test sınıfı için kolaylaştırır.

## <a name="creating-unit-tests"></a>Birim testleri oluşturma

**Test** proje şablonu, bir saplama birim test sınıfı oluşturulan ve sonraki göreviniz birim test yöntemlerini iş mantığı sınıfa eklemek istediğiniz iş mantığı için ekleyerek bu sınıfı değiştirin.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso University'deki herhangi tek bir eğitmen yalnızca tek bir departman Yöneticisi olabilir ve bu kuralını uygulamak için iş mantığı eklemeniz gerekir. Testleri ekleme ve bunları başarısız görmek için testleri çalıştırmadan başlar. Ardından kod ekleyecek ve bunları geçirmek görmeyi testleri yeniden çalıştırın.

Açık *UnitTest1.cs* dosya ve ekleme `using` deyimleri ContosoUniversity projede oluşturulan iş mantığı ve veri erişim katmanları için:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Değiştirin `TestMethod1` aşağıdaki yöntemlerle yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Yöntemi daha sonra iş mantığı sınıfının yeni bir örneğine geçirir proje, birim testi için oluşturduğunuz havuzu sınıfının bir örneğini oluşturur. Yöntemi daha sonra kullanabileceğiniz üç Departmanlar test yöntemlerinde eklemek için iş mantığı sınıfı kullanır.

İş mantığı sınıfı biri ile aynı yönetici mevcut bir bölümü olarak yeni bir bölüm eklemek çalışırsa veya birisi kişinin Kimliğini ayarlayarak bir departmanın yöneticisini güncelleştirmek çalışırsa, bir özel durum oluşturur, test yöntemleri doğrulayın zaten başka bir bölüme yöneticisinin kim.

Bu kod derlenmez için özel durum sınıfı henüz oluşturmadınız. Buna derlemeye ulaşmak için sağ `DuplicateAdministratorException` seçip **Oluştur**, ardından **sınıfı**.

[![image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Bu, silmeniz ve test projesinde bir sınıf oluşturur ana proje özel durum sınıfı oluşturduktan sonra. ve iş mantığı uygulanır.

Test projesini çalıştırın. Testler, beklendiği gibi başarısız.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Test geçiş yapmak için iş mantığı ekleme

Ardından, zaten başka bir bölüme yöneticisidir birisi bir departman Yöneticisi olarak ayarlanacak mümkün kılan iş mantığı uygulamanız. Özel durum iş mantığı katmanından ve bir kullanıcı bir departman düzenler ve tıkladığında sunu katmanı catch **güncelleştirme** zaten yönetici olan birisi seçtikten sonra. (Eğitmenler sayfayı oluşturmak önce kimin zaten yöneticilerdir aşağı açılan listeden kaldırabilirsiniz, ancak burada amacı ile iş mantığı katmanı çalışmaktır.)

Bir kullanıcı birden fazla bölüm Yöneticisi bir eğitmen yapmaya çalıştığında, throw özel durum sınıfı oluşturarak başlayın. Ana proje yeni bir sınıf dosyası oluşturma *BLL* klasörünü adlandırın *DuplicateAdministratorException.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Artık geçici silme *DuplicateAdministratorException.cs* oluşturduğunuz test projesini derlemek için bir dosya.

Ana proje Aç *SchoolBL.cs* dosya ve Doğrulama mantığı içeren aşağıdaki yöntemi ekleyin. (Kodu daha sonra oluşturacağınız bir Metoda başvuruyor.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Bu yöntem ekleme veya güncelleştirme kullandığınız zaman sizi ararız `Department` başka bir bölüme aynı yönetici olup olmadığını denetlemek için varlıklar.

Kod veritabanı için aranacak bir yöntemi çağıran bir `Department` aynı olan varlık `Administrator` varlık olarak özellik değeri eklenir veya güncelleştirilir. Bulunması durumunda, kod bir özel durum oluşturur. Eklenen veya güncelleştirilen varlığı Hayır varsa hiçbir doğrulama denetimi gereklidir `Administrator` değeri ve hiçbir özel güncelleştirme sırasında yöntemi çağrılırsa oluşturulur ve `Department` varlık eşleşme bulundu `Department` güncelleştirilen varlık.

Yeni yöntemi çağırın `Insert` ve `Update` yöntemleri:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

İçinde *ISchoolRepository.cs*, yeni veri erişim yönteminde aşağıdaki bildirimi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

İçinde *SchoolRepository.cs*, aşağıdaki `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

İçinde *SchoolRepository.cs*, aşağıdaki yeni veri erişim yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Bu kod alır `Department` belirtilen yönetici sahip varlıklar. Yalnızca bir bölüm (varsa) bulunması. Hiçbir kısıtlama veritabanına inşa edildiğinden durumda birden çok bölümler bulundu ancak, dönüş türü bir koleksiyondur.

Varsayılan olarak, nesne bağlamı veritabanından varlıklar aldığında, bunları kendi nesne durum Yöneticisi'nde izler. `MergeOption.NoTracking` Parametresi, bu izleme bu sorgu için yapılan değil belirtir. Bunun gerekli olmasının nedeni sorgu güncelleştirmeye çalıştığınız tam varlık döndürebilir ve ardından söz konusu varlık eklemek mümkün olmaz. Örneğin geçmişi departmanındaki düzenlerseniz, *Departments.aspx* sayfası ve yönetici değiştirmeden bırakın, bu sorguyu döndürür geçmişi bölümü. Varsa `NoTracking` , nesne bağlamı, nesne durumu Yöneticisi'nde geçmişi departmanı varlık zaten haritamın, ayarlı değil. Görünüm durumu yeniden oluşturulduğunda geçmişi departmanı varlık eklediğinizde, nesne bağlamı bildiren bir özel durum alanlarına sonra `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Belirtmek için alternatif olarak `MergeOption.NoTracking`, yalnızca bu sorgu için yeni bir nesne bağlamına oluşturabilirsiniz. Yeni nesne bağlamı kendi nesne durum Yöneticisi yeterli olacağından olacaktır çakışma çağırdığınızda `Attach` yöntemi. Bu alternatif yaklaşım, performans cezası en düşük olacaktır. yeni nesne bağlamı özgün nesne bağlamı ile meta verileri ve veritabanı bağlantı paylaşımında yapabileceği. Bununla birlikte, burada gösterilen bir yaklaşım sunar `NoTracking` seçeneği, hangi diğer bağlamlarda yararlı bulabilirsiniz. `NoTracking` Seçeneği ele alınmıştır bir sonraki Öğreticide bu serideki diğer.)

Test projesinde yeni bir veri erişim yöntemine ekleyin *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Bu kod, aynı veri seçimi gerçekleştirmek için LINQ kullanır, `ContosoUniversity` projesinin deposuna LINQ to Entities için kullanır.

Test projesi yeniden çalıştırın. Şu testler başarılı.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource özel durumları işleme

İçinde `ContosoUniversity` projeyi Çalıştır *Departments.aspx* sayfasında ve zaten başka bir bölüme için yönetici olan bir kişiye yönetici bir departman için değiştirmeyi deneyin. (Veritabanı geçersiz veri ile önceden yüklenmiş geldiğinden, Bu öğretici sırasında eklediğiniz bölümler yalnızca düzenleyebilirsiniz unutmayın.) Aşağıdaki sunucu hata sayfası olursunuz:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Bu nedenle hata işleme kodu eklemeniz gerekir bu tür bir hata sayfası, kullanıcıların istemezsiniz. Açık *Departments.aspx* ve belirtmek için bir işleyici `OnUpdated` olayı `DepartmentsObjectDataSource`. `ObjectDataSource` Etiketiyle artık, aşağıdaki örnekte benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

İçinde *Departments.aspx.cs*, aşağıdaki `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Aşağıdaki işleyicisi eklemek `Updated` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Varsa `ObjectDataSource` denetimi, güncelleştirmeyi gerçekleştirmeye çalıştığında bir özel durum yakalarsa, özel durum olay bağımsız değişkeni geçirir (`e`) için bu işleyici. İşleyici kod özel durum yinelenen bir yönetici özel durum olup olmadığını denetler. İse, kod için hata iletisi içeren bir doğrulayıcı denetimi oluşturur `ValidationSummary` görüntülenecek denetimi.

Sayfayı çalıştırın ve birinin iki bölüm Yöneticisi yeniden çıkarmaya çalışın. Bu süre `ValidationSummary` denetimi, bir hata iletisi görüntüler.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Benzer değişiklik *DepartmentsAdd.aspx* sayfası. İçinde *DepartmentsAdd.aspx*, belirtmek için bir işleyici `OnInserted` olayı `DepartmentsObjectDataSource`. Sonuçta elde edilen biçimlendirme, aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

İçinde *DepartmentsAdd.aspx.cs*, aynı `using` deyimi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Aşağıdaki olay işleyicisini ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Artık test *DepartmentsAdd.aspx.cs* sayfasına, birden fazla bölüm Yöneticisi bir kişinin yapma girişimleri de doğru bir şekilde işleme doğrulayın.

Bu depo düzeni kullanmak için uygulama giriş tamamlar `ObjectDataSource` Entity Framework ile denetimi. Depo düzeni ve Test Edilebilirlik hakkında daha fazla bilgi için bkz MSDN teknik incelemeyi [Sınanabilirlik ve Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Aşağıdaki öğreticide, sıralama ve filtreleme uygulamanıza işlevsellik ekleme görürsünüz.

> [!div class="step-by-step"]
> [Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [İleri](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
