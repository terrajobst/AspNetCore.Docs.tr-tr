---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Bir ASP.NET 4 Web uygulamasındaki Entity Framework 4.0 ile eşzamanlılığı işleme | Microsoft Docs
author: tdykstra
description: Bu öğretici serisinde, kullanmaya başlama Entity Framework 4.0 öğretici serisinin tarafından oluşturulan Contoso University web uygulaması oluşturur. BEN...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6b10aca944a322cae252666218aee4d5a2d6ed35
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754436"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Bir ASP.NET 4 Web uygulamasındaki Entity Framework 4.0 ile eşzamanlılığı işleme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur. Öğreticileri hakkında sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide nasıl sıralanacağını ve veri filtresini kullanarak öğrenilen `ObjectDataSource` denetimi ve Entity Framework. Bu öğreticide bir ASP.NET web uygulamasında Entity Framework kullanan eşzamanlılık işleme seçenekleri gösterilmektedir. Eğitmen office atamaları güncelleştiriliyor için adanmış yeni bir web sayfası oluşturur. Bu sayfada ve daha önce oluşturduğunuz Departmanlar sayfasında eşzamanlılık sorunları ele alacağız.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir kullanıcı bir kayıt düzenler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce aynı kaydı başka bir kullanıcı düzenlemeleri bir eşzamanlılık çakışması gerçekleşir. Bu gibi çakışmaları algılamak için Entity Framework ' ayarlamazsanız, kişi veritabanını güncelleştiren son diğer kullanıcının yaptığı değişikliklerin üzerine yazar. Birçok uygulama, bu kabul edilebilir riskidir ve olası eşzamanlılık çakışmalarını işleme için uygulamayı yapılandırmanız gerekmez. (Bazı kullanıcılara veya birkaç güncelleştirmeleri varsa veya bazı değişikliklerin üzerine yazılır, programlama için eşzamanlılık maliyeti gereksinimlerine ağır bastığı avantajı gerçekten önemli değildir.) Eşzamanlılık çakışmalarını hakkında endişelenmeniz gerekmez, bu öğreticide atlayabilirsiniz; kalan iki öğreticileri serisinin bu yapı herhangi bir şey bağlı yok.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybı önleme ihtiyacınız varsa, bunu yapmanın bir yolu veritabanı kilitlerini kullanmaktır. Bu adlandırılır *kötümser eşzamanlılık*. Örneğin, bir veritabanından satır okumadan önce kilit isteği salt okunur veya güncelleştirme erişim için. Güncelleştirme erişimi için bir satır kilitlerseniz, başka hiçbir kullanıcı için ya da satırın kilitlemek için izin salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişim güncelleştirin. Salt okunur erişim için bir satır kilitlerseniz, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek için bazı dezavantajları vardır. Programa karmaşık olabilir. Önemli bir veritabanı yönetim kaynakları gerektirir ve bu uygulamanın kullanıcı sayısı performans sorunlarına neden olabilir artar (diğer bir deyişle, iyi ölçeklendirme değil). Bu nedenle, tüm veritabanı yönetim sistemleri kötümser eşzamanlılık destekler. Entity Framework yerleşik desteği yok sağlar ve Bu öğreticide nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Kötümser eşzamanlılık alternatifi *iyimser eşzamanlılık*. İyimser eşzamanlılık, eşzamanlılık çakışmalarını olmasını sağlar ve eğer uygun şekilde tepki anlamına gelir. Örneğin, John çalışır *Department.aspx* sayfası, tıklama **Düzenle** bağlamak için Geçmiş bölümü ve azaltır **bütçe** $ $1,000,000.00 tutarı 125,000.00. (John rakip bir departman yönetir ve kendi bölümü için para kazanmanın istiyor.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John tıkladığında önce **güncelleştirme**, Jane aynı sayfa çalıştırılırsa tıkladığında **Düzenle** bağlantı geçmişi departmanı ve ardından değişiklikleri **başlangıç tarihi** için 1/1/1/10/2011 alanını 1999. (Jane geçmişi departmanı yönetir ve daha fazla Kıdem vermek istemektedir.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John tıkladığında **güncelleştirme** Jane önce ardından tıkladığında **güncelleştirme**. Gamze'nin tarayıcı şimdi listeleri **bütçe** tutarı 125,000.00 için John tarafından değiştirildiğinden tutarı $1,000,000.00, ancak bu, yanlış.

Bu senaryoda gerçekleştirebileceğiniz eylemlerden bazıları şunlardır:

- Bir kullanıcı değiştirmiş hangi özelliğinin izlemek ve yalnızca ilgili sütunları veritabanında güncelleştirin. Farklı özellikleri iki kullanıcı tarafından güncelleştirildiği için yer alan örnek senaryoda, veri kaybı, olacaktır. Sonraki birisi geçmişi departmanı gözatar, 1/1/1999 görürler ve $125,000.00. 

    Bu varlık çerçevesi varsayılan davranıştır ve veri kaybına neden olabilecek çakışmaları sayısını önemli ölçüde azaltabilir. Ancak, bir varlığın aynı özelliğe çelişen değişiklikler yaptıysanız bu davranış veri kaybını önlemek değil. Ayrıca, bu davranışı her zaman mümkün değildir; bir varlık türü için saklı yordamlar eşlediğinizde, veritabanında varlık herhangi bir değişiklik yapıldığında, bir varlığın özelliklerin tümünü güncelleştirilir.
- Gamze'nin değişiklik Can'ın değişiklik üzerine izin verebilirsiniz. Jane tıkladıktan sonra **güncelleştirme**, **bütçe** miktarı için 1,000,000.00 kapsar. Bu adlı bir *istemci WINS* veya *WINS'te son* senaryo. (İstemcinin değerleri veri deposunda nedir üzerinde önceliklidir.)
- Veritabanında güncelleştirilen Gamze'nin değişiklik engelleyebilirsiniz. Genellikle, bir hata iletisi görüntüler, her verilerin geçerli durumunu gösterir ve her yine de bunları yapmak istediği, değişiklikleri yeniden girmek izin. Her giriş kaydetme ve her onu yeniden girmek zorunda kalmadan yeniden fırsatı veren tarafından daha fazla sürecini otomatikleştirin. Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.)

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmaları algılama

Varlık Çerçevesi'nde işleyerek çakışmalarını çözebilirsiniz `OptimisticConcurrencyException` Entity Framework oluşturduğu özel durumları. Bu özel durumlar ne zaman öğrenmek için Entity Framework algılayamayabilir çakışmalar olması gerekir. Bu nedenle, veritabanı ve veri modeline uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler şunlardır:

- Veritabanına bir satır değiştirildiğinde belirlemek için kullanılan bir tablo sütunu içerir. Ardından bu sütunu eklemek için Entity Framework yapılandırabilirsiniz `Where` SQL yan tümcesi `Update` veya `Delete` komutları.

    Amacı olan `Timestamp` sütununda `OfficeAssignment` tablo.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Veri türü `Timestamp` sütun olarak da adlandırılır `Timestamp`. Ancak, sütun aslında bir tarih veya saat değeri içermiyor. Bunun yerine, her bir satır güncelleştirildiğinde artan sıralı bir numara değerdir. İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi içeren özgün `Timestamp` değeri. Güncelleştirilen satır değeri başka bir kullanıcı tarafından değiştirildiyse `Timestamp` özgün değerinden farklı bu nedenle `Where` yan tümcesi güncelleştirmek için hiçbir satır döndürür. Entity Framework bulduğunda tarafından geçerli hiçbir satırın güncelleştirilmediği `Update` veya `Delete` komutunu (diğer bir deyişle, etkilenen satır sayısı sıfır olduğunda), bir eşzamanlılık çakışması yorumlar.
- Tablodaki her sütun öğesinin özgün değerleri eklemek için Entity Framework yapılandırma `Where` yan tümcesi `Update` ve `Delete` komutları.

    İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi `Where` yan tümcesi bir satırı güncelleştirmek için iade kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Bu yöntemi kullanarak olarak etkin olduğundan bir `Timestamp` alan, ancak verimsiz olabilir. Birçok sütunları olan veritabanı tabloları için çok büyük sonuçlanabilir `Where` yan tümceleri, ve bir web uygulamasında, büyük miktarlarda durumu bakımını gerektirebilir. Sunucu kaynaklarını (örneğin, oturum durumu) gerektirir ya da web sayfasının kendisine (örneğin, Görünüm durumu) eklenmesi gerekir çünkü büyük miktarlarda durum koruma uygulama performansını etkileyebilir.

Bu öğreticide, hata izleme özelliği olmayan bir varlık için iyimser eşzamanlılık çakışmaları için işleme ekleyeceksiniz ( `Department` varlık) ve izleme özelliğine sahip bir varlık için ( `OfficeAssignment` varlık).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>İzleme özelliği olmadan iyimser eşzamanlılığı işleme

İyimser eşzamanlılık uygulamak için `Department` varlık, bir izleme yoktur (`Timestamp`) özelliği, aşağıdaki görevleri tamamlamayacaksınız:

- Eşzamanlılık izleme etkinleştirmek için veri modeli değiştirmek `Department` varlıklar.
- İçinde `SchoolRepository` sınıfı, tanıtıcı eşzamanlılık özel durumları `SaveChanges` yöntemi.
- İçinde *Departments.aspx* sayfası, denenen değişiklikleri başarısız uyarı kullanıcıya bir ileti görüntüleyerek tanıtıcı eşzamanlılık özel durumları. Kullanıcı geçerli değerleri görmek ve hala gerekliyse değişiklikleri yeniden deneyin.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Eşzamanlılık veri modelinde izlemeyi etkinleştirme

Bu seri önceki öğreticide, üzerinde çalıştığınız Contoso University web uygulamasını Visual Studio'da açın.

Açık *SchoolModel.edmx*, veri modeli Tasarımcısı'nda sağ tıklayın ve `Name` özelliğinde `Department` varlık ve ardından **özellikleri**. İçinde **özellikleri** penceresinde değişiklik `ConcurrencyMode` özelliğini `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Diğer birincil anahtar skaler özellikler için de aynısını yapın (`Budget`, `StartDate`, ve `Administrator`.) (Gezinti özellikleri için bunu yapamazsınız.) Bu zaman Entity Framework ürettiğini belirtir bir `Update` veya `Delete` güncelleştirmek için SQL komutu `Department` varlık veritabanında (olan orijinal değerleri) bu sütunlar dahil edilmelidir `Where` yan tümcesi. Ne zaman hiçbir satır bulunamazsa `Update` veya `Delete` komutu yürütür, Entity Framework bir iyimser eşzamanlılık özel durum oluşturur.

Kaydet ve veri modelini kapatın.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>DAL eşzamanlılık özel durumları işleme

Açık *SchoolRepository.cs* ve aşağıdakileri ekleyin `using` bildirimi `System.Data` ad alanı:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Aşağıdaki yeni `SaveChanges` iyimser eşzamanlılık özel durumları işler yöntemi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Bu yöntem çağrıldığında bir eşzamanlılık hatası meydana gelirse, bellek varlıkta özellik değerlerini şu anda veritabanında değerleri ile değiştirilir. Web sayfası işleyebilmeniz eşzamanlılık özel durum yeniden oluşur.

İçinde `DeleteDepartment` ve `UpdateDepartment` yöntemleri, varolan çağrısını değiştirin `context.SaveChanges()` çağrısıyla `SaveChanges()` yeni yöntemi çağırmak için.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Sunu katmanı eşzamanlılık özel durumları işleme

Açık *Departments.aspx* ve ekleme bir `OnDeleted="DepartmentsObjectDataSource_Deleted"` özniteliğini `DepartmentsObjectDataSource` denetimi. Denetim için açılış etiketi, artık aşağıdaki örneğe benzer.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

İçinde `DepartmentsGridView` denetlemek, tüm tablo sütunları belirtin `DataKeyNames` özniteliği aşağıdaki örnekte gösterildiği gibi. Bu çok büyük görünüm durumu alanları, bir neden olduğu oluşturacağını Not nedeni izleme alanını kullanarak Genel eşzamanlılık çakışmalarını izlemek için tercih edilen yoludur.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Açık *Departments.aspx.cs* ve aşağıdakileri ekleyin `using` bildirimi `System.Data` ad alanı:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Veri kaynak denetiminden 's çağıracak aşağıdaki yeni yöntemi ekleyin `Updated` ve `Deleted` eşzamanlılık özel durumları işlemek için olay işleyiciler:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Bu kod özel durum türü denetler ve bir eşzamanlılık özel durumunu ise, dinamik olarak kod oluşturur bir `CustomValidator` sırayla bir ileti görüntüler denetim `ValidationSummary` denetimi.

Yeni yöntemi çağırın `Updated` , daha önce eklediğiniz bir olay işleyicisi. Ayrıca, yeni bir oluşturma `Deleted` aynı yöntemi çağıran (ancak başka hiçbir şey yapmıyor) olay işleyicisi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>İyimser eşzamanlılık Departmanlar sayfasında test etme

Çalıştırma *Departments.aspx* sayfası.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Tıklayın **Düzenle** satırda etrafta dolanıp **bütçe** sütun. (Yalnızca Bu öğretici için oluşturduğunuz kayıt çünkü düzenleyebilirsiniz olduğunu unutmayın. mevcut `School` veritabanı kayıtlarını bazı geçersiz veri içeriyor. Ekonomik departmanı denemek için güvenli bir kayıttır.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Yeni bir tarayıcı penceresi açın ve sayfanın yeniden (kopya ilk tarayıcı penceresinin adres kutusuna URL'den ikinci bir tarayıcı penceresi için) çalıştırın.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Tıklayın **Düzenle** aynı satır, daha önce ve değiştirme **bütçe** farklı bir değer.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

İkinci tarayıcı penceresinde **güncelleştirme**. **Bütçe** miktarı bu yeni değere başarıyla değiştirildi.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

İlk tarayıcı penceresinde tıklayın **güncelleştirme**. Güncelleştirme başarısız. **Bütçe** tutar ikinci bir tarayıcı penceresinde ayarlanan değer kullanarak yeniden ve bir hata iletisi görürsünüz.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>İzleme özelliği kullanılarak iyimser eşzamanlılığı işleme

İzleme özelliği olan bir varlık için iyimser eşzamanlılık işlemek için aşağıdaki görevleri tamamlamayacaksınız:

- Saklı yordamlar yönetmek için veri modeline Ekle `OfficeAssignment` varlıklar. (İzleme özelliklerini ve saklı yordamlar birlikte kullanılan zorunda değildir; bunlar yalnızca buraya çizim için gruplandırılmış.)
- DAL ve BLL için CRUD yöntemler `OfficeAssignment` varlıklar, DAL iyimser eşzamanlılık özel durumları işlemek için kod dahil olmak üzere.
- Bir office atamaları web sayfası oluşturun.
- İyimser eşzamanlılık yeni web sayfasındaki test edin.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Saklı yordamları veri modeline OfficeAssignment ekleme

Açık *SchoolModel.edmx* dosya modeli Tasarımcısı'nda, tasarım yüzeyine sağ tıklayın ve **veritabanından bir güncelleştirme modeli**. İçinde **Ekle** sekmesinde **veritabanı nesnelerinizi seçin** iletişim kutusunda **saklı yordamlar** üç seçip `OfficeAssignment` saklı yordamlar (bkz: ekran görüntüsünü izleyerek) ve ardından **son**. (İndirdiğiniz ya da bir komut dosyası kullanarak oluşturduğunuz zaman bu saklı yordamlar zaten veritabanında bulunuyordu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Sağ `OfficeAssignment` varlık ve select **saklı yordam eşlemesi**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ayarlama **Ekle**, **güncelleştirme**, ve **Sil** işlevleri bunlara karşılık gelen kullanmak için saklı yordamlar. İçin `OrigTimestamp` parametresinin `Update` işlev olarak ayarlayın **özelliği** için `Timestamp` seçip **kullanım özgün değer** seçeneği.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Entity Framework çağırdığında `UpdateOfficeAssignment` saklı yordam, bu öğenin özgün değerinde geçecek `Timestamp` sütununda `OrigTimestamp` parametresi. Bu parametrede saklı yordam kullanır, `Where` yan tümcesi:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Saklı yordam yeni değeri de seçer `Timestamp` güncelleştirmesinden sonra sütun Entity Framework koruyabilirsiniz böylece `OfficeAssignment` bellekte karşılık gelen veritabanı satır ile eşitlenmiş olan varlık.

(Bir ofis ataması silmek için saklı yordam yok Not bir `OrigTimestamp` parametresi. Bu nedenle, Entity Framework silmeden önce bir varlık değişmeden doğrulanamıyor.)

Kaydet ve veri modelini kapatın.

### <a name="adding-officeassignment-methods-to-the-dal"></a>DAL ile OfficeAssignment yöntemler ekleme

Açık *ISchoolRepository.cs* ve eklemek için aşağıdaki CRUD yöntemleri `OfficeAssignment` varlık kümesi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Aşağıdaki yeni yöntemi ekleyin *SchoolRepository.cs*. İçinde `UpdateOfficeAssignment` yöntemi, yerel çağrı `SaveChanges` yöntemi yerine `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Test projesi içinde açın *MockSchoolRepository.cs* ve aşağıdakileri ekleyin `OfficeAssignment` toplama ve CRUD yöntemleri. (Sahte depo depo arabirimi uygulamalıdır veya çözüm derlenemeyecektir.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>BLL için OfficeAssignment yöntemler ekleme

Ana proje Aç *SchoolBL.cs* ve eklemek için aşağıdaki CRUD yöntemleri `OfficeAssignment` varlık kümesi için:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Bir OfficeAssignments Web sayfası oluşturma

Kullanan yeni bir web sayfası oluşturma *Site.Master* ana sayfa ve adlandırın *OfficeAssignments.aspx*. İçin aşağıdaki işaretlemeyi ekleyin `Content` adlı Denetim `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Dikkat `DataKeyNames` biçimlendirmeyi belirtir, öznitelik `Timestamp` kayıt anahtarı yanı sıra, özellik (`InstructorID`). Özellikleri belirterek `DataKeyNames` öznitelik neden olur (durumunu görüntülemek için benzer) denetim durumda kaydetmeyi denetim özgün değerler geri gönderme işlemi sırasında kullanılabilir olacak şekilde.

Kaydetmediyseniz `Timestamp` değeri, Entity Framework değil haritamın için `Where` SQL yan tümcesi `Update` komutu. Bu nedenle hiçbir şey güncelleştirilecek bulundu. Sonuç olarak, Entity Framework iyimser eşzamanlılık her zaman özel durum bir `OfficeAssignment` varlık güncelleştirilir.

Açık *OfficeAssignments.aspx.cs* ve aşağıdakileri ekleyin `using` veri erişim katmanı için bildirimi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Aşağıdaki `Page_Init` yöntemi dinamik veri işlevi etkinleştirir. Ayrıca aşağıdaki işleyicisi ekleyin `ObjectDataSource` denetimin `Updated` eşzamanlılık hataları denetlemek için olay:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>İyimser eşzamanlılık OfficeAssignments sayfasında test etme

Çalıştırma *OfficeAssignments.aspx* sayfası.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Tıklayın **Düzenle** satırda etrafta dolanıp **konumu** sütun.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Yeni bir tarayıcı penceresi açın ve sayfanın yeniden (kopya URL'sini ikinci bir tarayıcı penceresi ilk tarayıcı penceresinden) çalıştırın.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Tıklayın **Düzenle** aynı satır, daha önce ve değiştirme **konumu** farklı bir değer.

[![image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

İkinci tarayıcı penceresinde **güncelleştirme**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

İlk tarayıcı penceresine geçin ve tıklayın **güncelleştirme**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Bir hata iletisi görürsünüz ve **konumu** değeri, değiştirdiğiniz için ikinci bir tarayıcı penceresinde değeri gösterecek şekilde güncelleştirilmiştir.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Eşzamanlılık EntityDataSource denetimi ile işleme

`EntityDataSource` Denetimi eşzamanlılık ayarları veri modelindeki tanıdığı için yerleşik mantık içerir ve güncelleştirme ve silme işlemleri uygun şekilde işler. Ancak, tüm istisnalar gibi işlemesi `OptimisticConcurrencyException` özel durumları kendiniz kolay bir hata iletisi sağlamak için.

Ardından, yapılandıracağınız *Courses.aspx* sayfa (kullanan bir `EntityDataSource` denetimi) güncelleştirmeye izin ver ve silme işlemleri ve eşzamanlılık çakışma oluşursa bir hata iletisi görüntüler. `Course` Varlık ile yaptığınız aynı metodu kullanacak şekilde sütun izleme eşzamanlılığı yoksa `Department` varlık: tüm anahtar olmayan özelliklerin değerlerini izleme.

Açık *SchoolModel.edmx* dosya. Anahtar olmayan özelliklerinin `Course` varlık (`Title`, `Credits`, ve `DepartmentID`) ayarlayın **eşzamanlılık modu** özelliğini `Fixed`. Ardından kaydedin ve veri modelini kapatın.

Açık *Courses.aspx* sayfasında ve aşağıdaki değişiklikleri yapın:

- İçinde `CoursesEntityDataSource` ekleyin, denetimi `EnableUpdate="true"` ve `EnableDelete="true"` öznitelikleri. Bu denetim için açılış etiketi, artık aşağıdaki örneğe benzer:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- İçinde `CoursesGridView` denetlemek, değişiklik `DataKeyNames` öznitelik değerine `"CourseID,Title,Credits,DepartmentID"`. Ardından Ekle bir `CommandField` öğesine `Columns` gösteren öğesi **Düzenle** ve **Sil** düğmeler (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Denetimi artık aşağıdaki örnekte benzer:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Sayfayı çalıştırın ve bir çakışma durum Departmanlar sayfasında önce yaptığınız gibi oluşturun. İki Tarayıcı pencerelerinde çalıştırırsanız, tıklayın **Düzenle** aynı satır her penceresinde ve her birinde farklı bir değişiklik yapın. Tıklayın **güncelleştirme** bir pencere ve ardından **güncelleştirme** diğer penceresinde. Tıkladığınızda **güncelleştirme** ikinci seferde, sonuçları bir işlenmemiş bir eşzamanlılık özel hata sayfası görürsünüz.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Bu hata, bunun için işlenme için çok benzer bir şekilde işlemek `ObjectDataSource` denetimi. Açık *Courses.aspx* sayfasında ve `CoursesEntityDataSource` denetim için işleyiciler belirtin `Deleted` ve `Updated` olayları. Denetimin etiketiyle artık aşağıdaki örneğe benzer:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Önce `CoursesGridView` aşağıdakileri ekleyin, denetimi `ValidationSummary` denetimi:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

İçinde *Courses.aspx.cs*, ekleme bir `using` bildirimi `System.Data` ad alanını denetleyen bir eşzamanlılık özel durumları yöntemi ekleyin ve işleyicileri eklemek `EntityDataSource` denetimin `Updated` ve `Deleted`işleyicileri. Kod aşağıdaki gibi görünür:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Bu kod ve ne yaptığınızı arasındaki tek fark `ObjectDataSource` denetimi, bu durumda eşzamanlılık özel durumunu olduğunu `Exception` özelliği olay bağımsız değişkenleri nesnesinin yerine bu özel durumun `InnerException` özelliği.

Sayfayı çalıştırın ve bir eşzamanlılık çakışması yeniden oluşturun. Bu kez, bir hata iletisi görürsünüz:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. Sonraki öğreticiye Entity Framework kullanan bir web uygulaması performansını geliştirmeye yönelik rehberlik sağlar.

> [!div class="step-by-step"]
> [Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [İleri](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
