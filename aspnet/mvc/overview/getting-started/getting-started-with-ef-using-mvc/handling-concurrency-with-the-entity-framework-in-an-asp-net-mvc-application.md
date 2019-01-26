---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Öğretici: Bir ASP.NET MVC 5 uygulamasında eşzamanlılık EF ile işleme'
description: Bu öğreticide, iyimser eşzamanlılık birden çok kullanıcı aynı anda aynı varlık güncelleştirme çakışmaları işlemek için nasıl kullanılacağını gösterir.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2b41b51de5fd6d87b9905ba2bb3ddf7cedb47e69
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889840"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>Öğretici: Bir ASP.NET MVC 5 uygulamasında eşzamanlılık EF ile işleme

Önceki öğreticilerde, verileri güncelleştirmek öğrendiniz. Bu öğreticide, iyimser eşzamanlılık birden çok kullanıcı aynı anda aynı varlık güncelleştirme çakışmaları işlemek için nasıl kullanılacağını gösterir. Çalışan web sayfalarını değiştirmesine `Department` varlık böylece bunlar eşzamanlılık hata işleme. Aşağıdaki çizimler bir eşzamanlılık çakışması ortaya çıkarsa, gösterilen bazı iletileri de dahil olmak üzere düzenleme ve silme sayfalar gösterir.

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eşzamanlılık çakışmalarını hakkında bilgi edinin
> * İyimser eşzamanlılık ekleme
> * Departman denetleyicisini değiştirmek
> * Test eşzamanlılığı işleme
> * Silme sayfası

## <a name="prerequisites"></a>Önkoşullar

* [Zaman Uyumsuz ve Saklı Yordamlar](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Düzenlemek için bir kullanıcı bir varlığın verilerini görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri bir eşzamanlılık çakışması gerçekleşir. Böyle bir çakışma algılanması etkinleştirmezseniz, kişi veritabanını güncelleştiren son diğer kullanıcının yaptığı değişikliklerin üzerine yazar. Birçok uygulamada, bu riski kabul edilebilir: Bazı kullanıcılara veya birkaç güncelleştirmeleri varsa veya bazı değişikliklerin üzerine yazılır, programlama için eşzamanlılık maliyeti gereksinimlerine ağır bastığı avantajı gerçekten önemli değildir. Bu durumda, eşzamanlılık çakışmalarını işleme için uygulamayı yapılandırmanız gerekmez.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybı önleme ihtiyacınız varsa, bunu yapmanın bir yolu veritabanı kilitlerini kullanmaktır. Bu adlandırılır *kötümser eşzamanlılık*. Örneğin, bir veritabanından satır okumadan önce kilit isteği salt okunur veya güncelleştirme erişim için. Güncelleştirme erişimi için bir satır kilitlerseniz, başka hiçbir kullanıcı için ya da satırın kilitlemek için izin salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişim güncelleştirin. Salt okunur erişim için bir satır kilitlerseniz, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek dezavantajları vardır. Programa karmaşık olabilir. Önemli bir veritabanı yönetim kaynakları gerektirir ve bu uygulamanın kullanıcı sayısı performans sorunlarına neden olabilir artırır. Bu nedenle, tüm veritabanı yönetim sistemleri kötümser eşzamanlılık destekler. Entity Framework yerleşik desteği yok sağlar ve Bu öğreticide nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Kötümser eşzamanlılık alternatifi *iyimser eşzamanlılık*. İyimser eşzamanlılık, eşzamanlılık çakışmalarını olmasını sağlar ve eğer uygun şekilde tepki anlamına gelir. Örneğin, John Departmanlar Düzenle sayfasında, değişiklikleri çalışır **bütçe** 350,000.00 $ 0,00 ABD Doları İngilizce departmanına tutar.

John tıkladığında önce **Kaydet**, Jane çalışan aynı sayfa ve değişiklikleri **başlangıç tarihi** alanı 1/9/2007'den 8/8/2013.

John tıkladığında **Kaydet** ilk ve tarayıcı dizin sayfasına, sonra Jane döndürdüğünde değişikliğinin tıkladığında görür **Kaydet**. Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir. Bazı seçenekleri şunlardır:

- Bir kullanıcı değiştirmiş hangi özelliğinin izlemek ve yalnızca ilgili sütunları veritabanında güncelleştirin. Farklı özellikleri iki kullanıcı tarafından güncelleştirildiği için yer alan örnek senaryoda, veri kaybı, olacaktır. Sonraki biri İngilizce departmanı gözatar, Can'ın hem Gamze'nin değişiklikleri görürler: 8/8/2013'ün Başlangıç tarihi ve bütçe sıfır dolarlık bir.

    Bu güncelleştirme yöntemini veri kaybına neden olabilecek çakışmaları sayısını azaltabilir, ancak bir varlığın aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz. Varlık Çerçevesi'Bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır. Bir varlığın özgün tüm özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir içinde bir web uygulaması pratik değildir, çünkü genellikle. Koruma durumu büyük miktarlarda uygulama performansı etkileyebilir sunucu kaynaklarını gerektirir ya da web sayfasında kendisi (örneğin, gizli alanlar) dahil edilmesi için veya bir tanımlama bilgisinde.
- Gamze'nin değişiklik Can'ın değişiklik üzerine izin verebilirsiniz. Sonraki biri İngilizce departmanı gözatar, 8/8/2013 görürler ve geri yüklenen $350,000.00 değeri. Bu adlı bir *istemci WINS* veya *WINS'te son* senaryo. (Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Bu bölüm için giriş eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.
- Veritabanında güncelleştirilen Gamze'nin değişiklik engelleyebilirsiniz. Genellikle, bir hata iletisi görüntüler, her verilerin geçerli durumunu gösterir ve her yine de bunları yapmak istediği değişiklikleri yeniden uygulamak izin. Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulayacaksınız. Bu yöntem, neler olduğunu için uyarı bir kullanıcı olmaksızın herhangi bir değişiklik kılınmamasını sağlar.

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmaları algılama

İşleyerek çakışmalarını çözebilirsiniz [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework oluşturduğu özel durumları. Bu özel durumlar ne zaman öğrenmek için Entity Framework algılayamayabilir çakışmalar olması gerekir. Bu nedenle, veritabanı ve veri modeline uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler şunlardır:

- Veritabanı tablosunda bir satıra değiştirildiğinde belirlemek için kullanılan bir izleme sütunu içerir. Ardından bu sütunu eklemek için Entity Framework yapılandırabilirsiniz `Where` SQL yan tümcesi `Update` veya `Delete` komutları.

    İzleme sütunun veri türü genellikle [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) satır güncelleştirilir her zaman artan sıralı bir sayı değeridir. İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir. Güncelleştirilen satır değeri başka bir kullanıcı tarafından değiştirildiyse `rowversion` sütunudur özgün değerinden farklı şekilde `Update` veya `Delete` bildirimi nedeniyle güncelleştirilecek satırın bulamıyor `Where` yan tümcesi. Entity Framework bulduğunda hiçbir satır tarafından güncelleştirildiğini `Update` veya `Delete` komutunu (diğer bir deyişle, etkilenen satır sayısı sıfır olduğunda), bir eşzamanlılık çakışması yorumlar.
- Tablodaki her sütun öğesinin özgün değerleri eklemek için Entity Framework yapılandırma `Where` yan tümcesi `Update` ve `Delete` komutları.

    İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi `Where` yan tümcesi bir satırı güncelleştirmek için iade kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Birçok sütunları olan veritabanı tabloları için bu yaklaşım, çok büyük sonuçlanabilir `Where` yan tümcesi ve büyük miktarlarda durumu bakımını gerektirebilir. Daha önce belirtildiği gibi büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.

    Eşzamanlılık için bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretlemek sahip [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) öznitelik. Değişiklik SQL tüm sütunları eklemek Entity Framework sağlar `WHERE` yan tümcesi `UPDATE` deyimleri.

Bu öğreticinin geri kalanında, ekleyeceksiniz bir [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) özelliğini izleme `Department` varlık, bir denetleyici ve Görünüm ve her şeyin düzgün çalıştığını doğrulamak için test edin.

## <a name="add-optimistic-concurrency"></a>İyimser eşzamanlılık ekleme

İçinde *Models\Department.cs*, adlı bir izleme özelliği ekleme `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Zaman damgası](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) özniteliği belirtir, bu sütun eklenecektir `Where` yan tümcesi `Update` ve `Delete` veritabanına gönderilen komutları. Adlandırılan öznitelik [zaman damgası](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü [zaman damgası](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) veri türü SQL önce [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) değiştirildi. .Net türü [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) bir bayt dizisidir.

Fluent API'sini kullanmayı tercih ederseniz kullanabilirsiniz [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) yöntemi izleme özelliği, aşağıdaki örnekte gösterildiği gibi belirtin:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özellik ekleyerek veritabanı modeli değişti. Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutları girin:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>Departman denetleyicisini değiştirmek

İçinde *Controllers\DepartmentController.cs*, ekleme bir `using` deyimi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

İçinde *DepartmentController.cs* dosya, departman Yöneticisi açılır listede Eğitmen tam adı yerine yalnızca son adını içerecek şekilde "tam"adı "LastName" dört tüm oluşumlarını değiştirin.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Mevcut kodu değiştirin `HttpPost` `Edit` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Varsa `FindAsync` yöntemi null değerini döndürür, departman, başka bir kullanıcı tarafından silindi. Gösterilen kod gönderilen form değerlerini içeren bir hata iletisi düzenleme sayfası yeniden, böylece bir departman varlık oluşturmak için kullanır. Alternatif olarak, departman alanları yeniden görüntüleme olmadan yalnızca bir hata iletisi görüntüler, departman varlık yeniden oluşturmak zorunda mıydı.

Görünümün özgün depolar `RowVersion` değerini gizli bir alan ve yöntem içinde alan `rowVersion` parametresi. Çağırmadan önce `SaveChanges`, özgün moduna sahip `RowVersion` özellik değeri `OriginalValues` varlık için koleksiyon. Entity Framework bir SQL zaman oluşturur `UPDATE` komutu komut içerecek bir `WHERE` özgün sahip bir satır için görünen yan tümcesi `RowVersion` değeri.

Hiçbir satır etkileniyorsanız `UPDATE` komut (hiçbir satır özgün sahip `RowVersion` değer), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durum ve kodda `catch` bloğunu alır etkilenen `Department` özel bir varlıktan nesne.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Bu nesne kullanıcı tarafından girilen yeni değerlere sahip kendi `Entity` özelliğini çağırarak veritabanından okunan değerleri alabilirsiniz `GetDatabaseValues` yöntemi.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Yöntemi birisi veritabanından satır silinip null değerini döndürür; Aksi takdirde döndürülen nesne türüne sahip `Department` erişmek için sınıf `Department` özellikleri. (Zaten silinmek üzere işaretli olduğundan `databaseEntry` departman sonra silinmişse null olacaktır `FindAsync` yürütür ve önce `SaveChanges` yürütür.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Ardından, kod veritabanı değerlerini hangi kullanıcının düzenleme sayfada girilen öğesinden farklı olan her sütun için bir özel hata iletisi ekler:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Uzun bir hata iletisi, ne olduğunu ve bunu yapmanız gerekenler açıklanmaktadır:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Son olarak, kod ayarlar `RowVersion` değerini `Department` yeni değere nesne veritabanından alınır. Bu yeni `RowVersion` değeri depolanan gizli alanı sayfasını yeniden düzenleme ve sonraki kullanıcı çalıştırdığında **Kaydet**, yeniden düzenleme sayfası, yakalanan bu yana gerçekleşen eşzamanlılık hataları.

İçinde *Views\Department\Edit.cshtml*, kaydetmek için gizli bir alan ekleme `RowVersion` özellik değeri, gizli bir alan için takip `DepartmentID` özelliği:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>Test eşzamanlılığı işleme

Siteyi çalıştırın ve tıklayın **Departmanlar**.

Sağ tıklayın **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç** ardından **Düzenle** İngilizce departmanı için köprü. İki sekme aynı bilgileri görüntüler.

İlk tarayıcı sekmesine bir alana değiştirin ve tıklatın **Kaydet**.

Tarayıcı değişmiş değer ile dizin sayfası gösterilir.

Bir alan ikinci bir tarayıcı sekmesinde değiştirip'ı **Kaydet**. Bir hata iletisi görürsünüz:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Tıklayın **Kaydet** yeniden. İkinci tarayıcı sekmesinde girdiğiniz değer, ilk tarayıcıda değiştirilen verileri özgün değeri ile birlikte kaydedilir. Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

Silme sayfası için Entity Framework, birisi başka benzer bir şekilde bölüm düzenleme nedeni eşzamanlılık çakışmalarını algılar. Zaman `HttpGet` `Delete` yöntemi görüntüler onay görünümü, görünümün özgün içerir `RowVersion` gizli bir alan değeri. Değer daha sonra kullanılabilir olduğunu `HttpPost` `Delete` kullanıcının silmeyi onaylaması çağrılan yöntem. Entity Framework, SQL oluşturduğunda `DELETE` komutunu içerdiği bir `WHERE` yan tümcesinin orijinal `RowVersion` değeri. Sıfır satır komutu sonuçları (satır silme onayı sayfasında görüntülenen sonra değiştirildiği anlamına gelir) etkileniyorsa, bir eşzamanlılık özel durum oluşturulur ve `HttpGet Delete` ayarlamak bir hata bayrağıyla yöntemi çağrıldığında `true` görüntülemek için bir hata iletisiyle onay sayfası. Bu durumda farklı bir hata iletisi görüntülenir, böylece satırın başka bir kullanıcı tarafından silindiğinden sıfır satır etkilenmiştir mümkündür.

İçinde *DepartmentController.cs*, değiştirin `HttpGet` `Delete` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Yöntemi, bir eşzamanlılık hatası sonra sayfayı yeniden olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu bayrak ise `true`, görünümünü kullanarak bir hata iletisi gönderilen bir `ViewBag` özelliği.

Değiştirin `HttpPost` `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Yalnızca değiştirilen iskele kurulmuş kod içinde bu yöntem yalnızca bir kayıt kimliği kabul:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Bu parametre için değiştirdiyseniz bir `Department` model bağlayıcı tarafından oluşturulan varlık örneği. Bu aşağıdakilere erişmenizi sağlar `RowVersion` kayıt anahtarı yanı sıra özellik değeri.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Eylem yöntemi adından de değiştirdiğiniz `DeleteConfirmed` için `Delete`. İskele kurulan kodu adlı `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek `HttpPost` yöntemi benzersiz bir imza. (CLR'nin farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile devam edin ve aynı adı kullanın `HttpPost` ve `HttpGet` yöntemlerini silin.

Bir eşzamanlılık hatası yakalanmışsa kod silme onayı sayfası görüntüler ve bunu belirten bir bayrak eşzamanlılık hata iletisi görüntülenmelidir sağlar.

İçinde *Views\Department\Delete.cshtml*, iskele kurulan kodu bir hata iletisi alan ve gizli alanları DepartmentID ve RowVersion özellikleri ekleyen aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Bu kod arasında bir hata iletisi ekler `h2` ve `h3` başlıkları:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Değiştirir `LastName` ile `FullName` içinde `Administrator` alan:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Son olarak, onu gizli alanlar için ekler `DepartmentID` ve `RowVersion` sonra özellikleri `Html.BeginForm` deyimi:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Departmanlar dizin sayfası çalıştırın. Sağ tıklayın **Sil** seçin ve İngilizce departmanı için köprü **yeni sekmede aç** birinci sekmede ardından **Düzenle** İngilizce departmanı için köprü.

İlk penceresinde değerlerden birini değiştirin ve tıklayın **Kaydet**.

Dizin Sayfası değişikliği doğrular.

İkinci sekmesini tıklatın **Sil**.

Eşzamanlılık hata iletisini görüntüleyin ve departman değerlerini şu anda veritabanında nedir ile yenilenir.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Tıklarsanız **Sil** yeniden departman silindiğini gösterir dizin sayfasına yönlendirilirsiniz.

## <a name="get-the-code"></a>Kodu alma

[Projeyi yükle](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ek kaynaklar

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

Çeşitli eşzamanlılık senaryoları işlemek için diğer yollar hakkında daha fazla bilgi için bkz: [iyimser eşzamanlılık desenlerinin](https://msdn.microsoft.com/data/jj592904) ve [özellik değerleri ile çalışma](https://msdn.microsoft.com/data/jj592677) MSDN'de. Sonraki öğreticide için tablo başına hiyerarşi devralma uygulamak gösterilmektedir `Instructor` ve `Student` varlıklar.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eşzamanlılık çakışmalarını hakkında bilgi edindiniz
> * Eklenen iyimser eşzamanlılık
> * Değiştirilmiş bölüm denetleyicisi
> * Test edilen eşzamanlılık işleme
> * Silme sayfası güncelleştirildi

Veri modelinde devralma uygulama hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Veri modelinde devralma uygulama](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)