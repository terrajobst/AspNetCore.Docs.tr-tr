---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: "Bir ASP.NET 4 Web uygulamasında Entity Framework 4.0 eşzamanlılık işleme | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7bdcf610458631749531ed1279d27e90572f0371
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Bir ASP.NET 4 Web uygulamasında Entity Framework 4.0 eşzamanlılık işleme
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur. Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide, sıralamak ve veri filtresini kullanarak öğrendiniz `ObjectDataSource` denetimi ve Entity Framework. Bu öğretici bir ASP.NET web uygulamasında Entity Framework kullanan eşzamanlılık işleme seçenekleri gösterir. Eğitmen office atamaları güncelleştirmek için ayrılmış yeni bir web sayfası oluşturur. Bu sayfada ve daha önce oluşturduğunuz Departmanlar sayfasında eşzamanlılık sorunlarını ele alacağız.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir kullanıcı bir kayıtta düzenleme ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı kaydı düzenler bir eşzamanlılık çakışması oluşur. Bu gibi çakışmaları algılamak için Entity Framework'ü kurmak ayarlamazsanız aktaranın veritabanını güncelleştiren son diğer kullanıcının değişiklikleri geçersiz kılar. Birçok uygulama, bu kabul edilebilir riskidir ve olası eşzamanlılık çakışmalarına için uygulamayı yapılandırma gerekmez. (Birkaç kullanıcı ya da birkaç güncelleştirmeleri varsa veya bazı değişiklikleri üzerine yazılmışsa eşzamanlılık programlama maliyetini ağır avantajı gerçekten önemli değildir.) Eşzamanlılık çakışmaları hakkında endişelenmeniz gerekmez, Bu öğretici atlayabilirsiniz; serideki kalan iki öğreticileri herhangi bir şey bu yapı bağlı yok.

### <a name="pessimistic-concurrency-locking"></a>Eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybını önlemek gerekiyorsa, bunu yapmanın bir yolu veritabanı kilitleri kullanmaktır. Bu adlı *eşzamanlılık*. Örneğin, bir veritabanından bir satır okumadan önce için bir kilit isteği salt okunur veya güncelleştirme erişim. Güncelleştirme erişim için bir satır kilitlemek, başka hiçbir kullanıcı için ya da satır kilitlemek için izin verilir salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişimi güncelleştirin. Salt okunur erişim için bir satır kilitlemek, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek bazı dezavantajları vardır. Programa karmaşık olabilir. Önemli Veritabanı Yönetimi kaynaklarını gerektirir ve bu kullanıcı bir uygulamanın sayısı olarak performans sorunlarına neden olabilir artırır (diğer bir deyişle, onu iyi ölçeklendirilmediğini). Bu nedenlerle, tüm veritabanı yönetim sistemleri eşzamanlılık destekler. Entity Framework yerleşik hiçbir desteği sağlar ve bu öğreticiyi uyguladıktan nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Eşzamanlılık için alternatif *iyimser eşzamanlılık*. İyimser eşzamanlılık gerçekleşecek şekilde eşzamanlılık çakışması izin vererek ve içermiyorlarsa uygun şekilde tepki anlamına gelir. Örneğin, John çalıştıran *Department.aspx* sayfası, tıklama **Düzenle** bağlamak için Geçmiş bölümü ve azaltır **bütçe** $ $1,000,000.00 tutarı 125,000.00. (John rakip bir bölüm yönetir ve kendi departmanı para boşaltmak istiyor.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John tıklar önce **güncelleştirme**, Jane aynı sayfa çalıştırır, tıklar **Düzenle** geçmişi departmanı ve ardından değişiklikleri bağlantı **başlangıç tarihi** için 1/1/1/10/2011'dan alan 1999. (Jane geçmişi departmanı yönetir ve daha fazla Kıdem vermek istiyor.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John tıklar **güncelleştirme** ilk olarak, Jane ardından tıklattığında **güncelleştirme**. Jane'nın tarayıcı şimdi listeleri **bütçe** tutar 125,000.00 için John tarafından değiştirildiğinden tutar $1,000,000.00, ancak bu olarak yanlış.

Bu senaryoda gerçekleştirebileceğiniz eylemlerden bazıları şunlardır:

- Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara veritabanındaki güncelleştirin. Farklı özellikler iki kullanıcı tarafından güncelleştirildi olduğundan örnek senaryoda, hiçbir veri kaybı, olacaktır. Sonraki birisi geçmişi departmanı gözatar, 1/1/1999 görürler ve $125,000.00. 

    Bu Entity Framework'deki varsayılan davranış ve veri kaybına sebep çakışmaları sayısını önemli ölçüde azaltabilir. Ancak, bir varlığın aynı özelliğe rakip bir değişiklik yaptıysanız bu davranış veri kaybını önlemek değil. Ayrıca, bu davranış her zaman mümkün değildir; bir varlık türü için saklı yordamlar eşlediğinizde, veritabanında varlık herhangi bir değişiklik yapıldığında, bir varlığın özelliklerin tümünü güncelleştirilir.
- Jane'nın değişiklik Can'ın değişiklik üzerine izin verebilirsiniz. Jane tıkladıktan sonra **güncelleştirme**, **bütçe** tutar 1,000,000.00 için geri gider. Bu adlı bir *istemci WINS* veya *WINS'de son* senaryo. (İstemcinin değerleri veri deposunda nedir üzerinden önceliklidir.)
- Veritabanında güncelleştirilen Jane'nın değişiklik engelleyebilir. Genellikle, bir hata iletisi görüntüler her verilerin geçerli durumunu gösterir ve her aynen bunları olmak istiyorsa, kendi değişiklikleri yeniden girmek izin. Her giriş kaydetme ve her onu yeniden girmek zorunda kalmadan yeniden fırsatı veren tarafından daha fazla işlemini otomatikleştirmek. Bu adlı bir *deposu WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.)

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını algılama

Entity Framework işleyerek çakışmalarını çözebilirsiniz `OptimisticConcurrencyException` Entity Framework oluşturur özel durumları. Bu özel durumlar oluşturma zamanı bilmek için Entity Framework çakışmaları algılayabilir olması gerekir. Bu nedenle, veritabanı ve veri modelinin uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler aşağıdakileri içerir:

- Veritabanında bir satır değiştiğinde belirlemek için kullanılan bir tablo sütunu içerir. Daha sonra bu sütununu dahil etmek için Entity Framework yapılandırabilirsiniz `Where` yan tümcesi SQL `Update` veya `Delete` komutları.

    Amacı olan `Timestamp` sütununda `OfficeAssignment` tablo.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Veri türü `Timestamp` sütun olarak da adlandırılır `Timestamp`. Ancak, sütun gerçekte bir tarih veya saat değeri içermiyor. Bunun yerine, satır her güncelleştirildiğinde artar bir sıralı sayı değerdir. İçinde bir `Update` veya `Delete` komutu, `Where` yan tümcesi içeren özgün `Timestamp` değeri. Güncelleştirilen satır başka bir kullanıcı tarafından değeri değiştirildiğinde, `Timestamp` özgün değerinden farklı bu nedenle `Where` yan tümcesi güncelleştirmek için satır döndürür. Entity Framework bulduğunda geçerli tarafından hiçbir satırın güncelleştirilmediği `Update` veya `Delete` komutunu (diğer bir deyişle, etkilenen satırların sayısı sıfır olduğunda), bir eşzamanlılık çakışması yorumlar.
- Tablodaki her sütunun özgün değerler eklemek için Entity Framework yapılandırma `Where` yan tümcesi `Update` ve `Delete` komutları.

    İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi `Where` yan tümcesi güncelleştirmek için bir satır dönüş kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Bu yöntemi kullanarak olarak etkin olduğu bir `Timestamp` alan ancak verimsiz olabilir. Çok sayıda sütuna sahip veritabanı tabloları için çok büyük sonuçlanabilir `Where` yan tümceleri, ve bir web uygulaması durumu büyük miktarlarda korumak görünür duruma gerektirebilir. Sunucu kaynakları (örneğin, oturum durumu) gerekir ya da web sayfasının kendisi (örneğin, Görünüm durumu) eklenmesi gerekir çünkü durumu büyük miktarlarda koruma uygulama performansını etkileyebilir.

Bu öğreticide, bir izleme özelliğine sahip olmayan bir varlık için iyimser eşzamanlılık çakışmalar için işleme hatası ekleyeceksiniz ( `Department` varlık) ve izleme özelliğine sahip bir varlık için ( `OfficeAssignment` varlık).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>İzleme özelliği olmadan iyimser eşzamanlılık işleme

İyimser eşzamanlılık uygulamak için `Department` bir izleme yok varlık (`Timestamp`) özelliği, aşağıdaki görevleri tamamlar:

- Eşzamanlılık izlemeyi etkinleştirmek için veri modelini değiştirme `Department` varlıklar.
- İçinde `SchoolRepository` sınıfı, tanıtıcı eşzamanlılık özel durumları `SaveChanges` yöntemi.
- İçinde *Departments.aspx* sayfası, denenen değişiklikleri başarısız uyarı kullanıcıya bir ileti görüntüleyerek tanıtıcı eşzamanlılık özel durumları. Kullanıcı geçerli değerleri görmek ve hala gerekli değişiklikler yeniden deneyin.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Veri modelinde izleme eşzamanlılık etkinleştirme

Visual Studio'da bu serideki önceki öğreticideki birlikte çalıştığınız Contoso University web uygulamasını açın.

Açık *SchoolModel.edmx*ve veri modeli Tasarımcısı'nda sağ `Name` özelliğinde `Department` varlık ve ardından **özellikleri**. İçinde **özellikleri** penceresinde, değişiklik `ConcurrencyMode` özelliğine `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Diğer birincil anahtar skaler özellikler için aynı yapın (`Budget`, `StartDate`, ve `Administrator`.) (Bu gezinti özellikleri için işlemi yapamazsınız.) Bu zaman Entity Framework ürettiği belirtir bir `Update` veya `Delete` güncelleştirmek için SQL komutu `Department` varlık veritabanında bu sütunlarla (orijinal değerleri) dahil edilmelidir `Where` yan tümcesi. Satır ne zaman bulunursa `Update` veya `Delete` komutu çalıştırır, Entity Framework iyimser eşzamanlılık özel durum atar.

Kaydedin ve veri modelinin kapatın.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>DAL eşzamanlılık özel durumları işleme

Açık *SchoolRepository.cs* ve aşağıdakileri ekleyin `using` bildirimi `System.Data` ad alanı:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Aşağıdaki yeni ekleyin `SaveChanges` iyimser eşzamanlılık özel durumları işler yöntemi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Bu yöntem çağrıldığında bir eşzamanlılık hatası oluşursa, bellekte varlığın özellik değerlerini şu anda veritabanındaki değerlerle değiştirilir. Böylece web sayfasını işlemek eşzamanlılık özel durumunu işlenemezse.

İçinde `DeleteDepartment` ve `UpdateDepartment` yöntemleri, varolan çağrısı Değiştir `context.SaveChanges()` çağrısıyla `SaveChanges()` yeni yöntemini çağırmak için.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Sunu katmanındaki eşzamanlılık özel durumları işleme

Açık *Departments.aspx* ve ekleme bir `OnDeleted="DepartmentsObjectDataSource_Deleted"` özniteliğini `DepartmentsObjectDataSource` denetim. Denetim için açılış etiketi şimdi aşağıdaki örnekte benzeyecektir.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

İçinde `DepartmentsGridView` denetlemek, tüm tablo sütunları `DataKeyNames` , aşağıdaki örnekte gösterildiği gibi özniteliği. Bu çok büyük görünüm durumu alanları nedenlerinden biri olduğu oluşturacağını Not izleme alanını kullanarak neden genellikle eşzamanlılık çakışması izlemek için önerilen yöntemdir.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Açık *Departments.aspx.cs* ve aşağıdakileri ekleyin `using` bildirimi `System.Data` ad alanı:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Verileri kaynak denetiminden 's çağıracak aşağıdaki yeni yöntemi ekleyin `Updated` ve `Deleted` eşzamanlılık özel durumları işlemek için olay işleyicileri:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Özel durum türü bu kodu denetler ve bir eşzamanlılık özel durumu ise, kod dinamik olarak oluşturur bir `CustomValidator` sırayla bir ileti görüntüler denetim `ValidationSummary` denetim.

Yeni yöntemi çağırın `Updated` daha önce eklediğiniz olay işleyicisi. Ayrıca, yeni bir oluşturma `Deleted` aynı yöntemi çağırır (ancak başka bir şey yapmaz) olay işleyicisi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>İyimser eşzamanlılık Departmanlar sayfasında test etme

Çalıştırma *Departments.aspx* sayfası.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Tıklatın **Düzenle** bir satırda ve değeri değiştirin **bütçe** sütun. (Yalnızca Bu öğretici için oluşturduğunuz kayıtları çünkü düzenleyebilirsiniz olduğunu unutmayın varolan `School` veritabanı kayıtlarını bazı geçersiz veri içeriyor. Öğesidir departmanı denemeniz için güvenli bir kayıttır.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Yeni bir tarayıcı penceresi açın ve sayfayı yeniden (kopya ikinci bir tarayıcı penceresi ilk tarayıcı penceresinin adresi kutusundan URL'sine) çalıştırın.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Tıklatın **Düzenle** aynı satır, önceki düzenlenen ve değiştirme **bütçe** farklı bir değer.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

İkinci tarayıcı penceresinde **güncelleştirme**. **Bütçe** tutar bu yeni değer başarıyla değiştirildi.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

İlk tarayıcı penceresinde **güncelleştirme**. Güncelleştirme başarısız. **Bütçe** tutar ikinci bir tarayıcı penceresinde ayarlanan değer kullanarak yeniden ve hata iletisine bakın.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>İzleme özelliği kullanılarak iyimser eşzamanlılık işleme

İzleme özelliğine sahip bir varlık için iyimser eşzamanlılık işlemek için aşağıdaki görevleri tamamlar:

- Saklı yordamlar yönetmek için veri modeline Ekle `OfficeAssignment` varlıklar. (İzleme özellikleri ve saklı yordamlar birlikte kullanılması gerekmez; bunlar yalnızca burada çizim için gruplanan.)
- DAL ve BLL için CRUD yöntemleri eklemek `OfficeAssignment` DAL iyimser eşzamanlılık özel durumları işlemek için kod dahil olmak üzere varlıklar.
- Bir office atamaları web sayfası oluşturun.
- İyimser eşzamanlılık yeni web sayfasındaki sınayın.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Saklı yordamlar veri modeline OfficeAssignment ekleme

Açık *SchoolModel.edmx* dosya modeli Tasarımcısı'nda, tasarım yüzeyine sağ tıklayın ve **güncelleştirme modeli veritabanından**. İçinde **Ekle** sekmesinde **veritabanı nesnelerinizi** iletişim kutusunda, genişletin **saklı yordamlar** ve üç seçin `OfficeAssignment` saklı yordamlar (bkz. Ekran aşağıdakileri) ve ardından **son**. (Karşıdan ya da bir komut dosyası kullanılarak oluşturulan bu saklı yordamları zaten veritabanında bulunuyordu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Sağ `OfficeAssignment` varlık ve select **depolanan yordamı eşleme**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ayarlama **Ekle**, **güncelleştirme**, ve **silmek** işlevleri karşılık gelen kullanmak için saklı yordamlar. İçin `OrigTimestamp` parametresinin `Update` işlev, ayarlamak **özelliği** için `Timestamp` seçip **kullanım özgün değeri** seçeneği.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Entity Framework çağırdığında `UpdateOfficeAssignment` saklı yordam, özgün değeri geçecek `Timestamp` sütununda `OrigTimestamp` parametresi. Bu parametrede saklı yordamı kullanan kendi `Where` yan tümcesi:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Saklı yordam ayrıca yeni değeri seçer `Timestamp` güncelleştirmeden sonra sütun Entity Framework tutabilirsiniz böylece `OfficeAssignment` bellekte karşılık gelen veritabanı satır ile eşitlenmiş olan varlık.

(Office atama silmek için saklı yordam yok Not bir `OrigTimestamp` parametresi. Bu nedenle, Entity Framework silmeden önce bir varlık değişmeden doğrulayamıyor.)

Kaydedin ve veri modelinin kapatın.

### <a name="adding-officeassignment-methods-to-the-dal"></a>DAL ile OfficeAssignment yöntemler ekleme

Açık *ISchoolRepository.cs* ve eklemek için aşağıdaki CRUD yöntemleri `OfficeAssignment` varlık kümesi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Aşağıdaki yeni yöntemleri ekleyin *SchoolRepository.cs*. İçinde `UpdateOfficeAssignment` yöntemi, yerel arama `SaveChanges` yöntemi yerine `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Test projesinde açmak *MockSchoolRepository.cs* ve aşağıdakileri ekleyin `OfficeAssignment` toplama ve CRUD yöntemleri. (Sahte deposu depo arabirimi uygulamalıdır veya çözümü derleme çalışmaz.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>BLL OfficeAssignment yöntemler ekleme

Ana projeyi açın *SchoolBL.cs* ve eklemek için aşağıdaki CRUD yöntemleri `OfficeAssignment` varlık kümesi için:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>OfficeAssignments Web sayfası oluşturma

Kullanan yeni bir web sayfası oluşturmak *Site.Master* ana sayfa ve adlandırın *OfficeAssignments.aspx*. Aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

İçinde dikkat `DataKeyNames` özniteliği, biçimlendirmeyi belirtir `Timestamp` kayıt anahtarı yanı sıra özelliği (`InstructorID`). Özelliklerinde belirtme `DataKeyNames` özniteliği neden olur (durumunu görüntülemek için benzer) denetim durumda kaydetmek denetimi özgün değerler geri gönderme işlemesi sırasında kullanılabilir olmasını sağlamak.

Kaydetmediyseniz `Timestamp` değeri, Entity Framework sahip için `Where` yan tümcesi SQL `Update` komutu. Sonuç olarak hiçbir şey güncelleştirmek için bulunması. Sonuç olarak, Entity Framework iyimser eşzamanlılık özel durum her zaman throw bir `OfficeAssignment` varlık güncelleştirilir.

Açık *OfficeAssignments.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi veri erişim katmanı için:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Aşağıdakileri ekleyin `Page_Init` dinamik veri işlevini etkinleştirir yöntemi. Ayrıca aşağıdaki işleyicisi ekleme `ObjectDataSource` denetimin `Updated` eşzamanlılık hataları denetlemek için olay:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>İyimser eşzamanlılık OfficeAssignments sayfasında test etme

Çalıştırma *OfficeAssignments.aspx* sayfası.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Tıklatın **Düzenle** bir satırda ve değeri değiştirin **konumu** sütun.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Yeni bir tarayıcı penceresi açın ve sayfayı yeniden (kopya ikinci bir tarayıcı penceresi ilk tarayıcı penceresinde URL'sine) çalıştırın.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Tıklatın **Düzenle** aynı satır, önceki düzenlenen ve değiştirme **konumu** farklı bir değer.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

İkinci tarayıcı penceresinde **güncelleştirme**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

İlk tarayıcı penceresine geçip tıklatın **güncelleştirme**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Bir hata iletisi görürsünüz ve **konumu** değeri, değerin değiştirdiğiniz için ikinci bir tarayıcı penceresi göstermek için güncelleştirilmiştir.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Eşzamanlılık EntityDataSource denetimi ile işleme

`EntityDataSource` Ve tanıtıcıları güncelleştirme ve silme işlemleri uygun şekilde denetim veri modelindeki eşzamanlılık ayarları tanıdığı yerleşik mantığı içerir. Ancak, tüm istisnalar gibi işlemelidir `OptimisticConcurrencyException` özel durumlar kendiniz kolay hata iletisi sağlamak için.

Ardından, yapılandıracağınız *Courses.aspx* sayfa (kullanan bir `EntityDataSource` denetimi) güncelleştirmeye izin ver ve silme işlemleri ve bir eşzamanlılık çakışması olursa bir hata iletisi görüntülenir. `Course` Varlık ile yaptığınız aynı yöntemi kullanacak sütun, izleme bir eşzamanlılık yoksa `Department` varlık: tüm anahtar olmayan özellik değerlerini izleyin.

Açık *SchoolModel.edmx* dosya. Anahtar olmayan özelliklerinin `Course` varlık (`Title`, `Credits`, ve `DepartmentID`) ayarlayın **eşzamanlılık modu** özelliğine `Fixed`. Ardından kaydedin ve veri modelinin kapatın.

Açık *Courses.aspx* sayfasında ve aşağıdaki değişiklikleri yapın:

- İçinde `CoursesEntityDataSource` denetlemek, ekleme `EnableUpdate="true"` ve `EnableDelete="true"` öznitelikleri. Bu denetim için açılış etiketi şimdi aşağıdaki örneğe benzer:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- İçinde `CoursesGridView` denetlemek, değişiklik `DataKeyNames` öznitelik değeri için `"CourseID,Title,Credits,DepartmentID"`. Ardından ekleyin bir `CommandField` öğesine `Columns` gösterir öğesi **Düzenle** ve **silmek** düğmeleri (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Denetim şimdi aşağıdaki örnekte benzer:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Sayfayı çalıştırın ve Departmanlar sayfasında önce yaptığınız gibi bir çakışma durum oluşturun. Sayfayı iki tarayıcı pencerelerini çalıştırın, tıklatın **Düzenle** aynı satır her penceresinde ve her biri farklı bir değişiklik yapın. Tıklatın **güncelleştirme** bir pencere ve ardından **güncelleştirme** diğer penceresinde. Tıkladığınızda **güncelleştirme** ikincisinde ise, bir işlenmemiş eşzamanlılık özel durumundan sonuçları hata sayfasına bakın.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Bu hata, bunun için işlenme için çok benzer bir şekilde işlemek `ObjectDataSource` denetim. Açık *Courses.aspx* sayfasında hem de `CoursesEntityDataSource` denetim için işleyiciler belirleyin `Deleted` ve `Updated` olaylar. Denetimi açılış etiketinde şimdi aşağıdaki örneğe benzer:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Önce `CoursesGridView` denetlemek, aşağıdakileri ekleyin `ValidationSummary` denetimi:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

İçinde *Courses.aspx.cs*, ekleme bir `using` bildirimi `System.Data` ad alanı, denetleyen bir eşzamanlılık yöntemi ekleyin ve işleyicileri ekleyin `EntityDataSource` denetimin `Updated` ve `Deleted`işleyicileri. Kod aşağıdaki gibi görünür:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Bu kod ve ne yaptığınızı arasındaki tek fark `ObjectDataSource` denetimidir bu durumda eşzamanlılık özel durumunu olduğunu `Exception` özelliği olay bağımsız değişkenleri nesnesinin yerine bu özel durumun `InnerException` özelliği.

Sayfayı çalıştırın ve bir eşzamanlılık çakışması yeniden oluşturun. Bu süre, bir hata mesajı görürsünüz:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. Sonraki öğretici bir Entity Framework kullanan bir web uygulaması performansını artırmak nasıl hakkında kılavuzluk sağlar.

>[!div class="step-by-step"]
[Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
[sonraki](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
