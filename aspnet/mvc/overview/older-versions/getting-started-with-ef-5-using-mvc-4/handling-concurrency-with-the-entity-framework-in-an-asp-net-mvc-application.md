---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Entity Framework bir ASP.NET MVC uygulamasındaki (10 7) ile eşzamanlılığı işleme | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eadadd1ac98191d0f802dc742294938eb48d86b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809011"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Entity Framework bir ASP.NET MVC uygulamasındaki (10 7) ile eşzamanlılığı işleme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki iki öğreticilerde ile ilgili verileri çalışmıştır. Bu öğreticide, eşzamanlılık nasıl ele alınacağını gösterir. Web sayfaları ile çalışma oluşturacaksınız `Department` varlık ve düzenleme ve silme sayfaları `Department` varlıkları eşzamanlılık hataları işleyecek. Aşağıdaki çizimler bir eşzamanlılık çakışması ortaya çıkarsa, gösterilen bazı iletileri de dahil olmak üzere dizin ve silmeyi sayfalar gösterir.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Düzenlemek için bir kullanıcı bir varlığın verilerini görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri bir eşzamanlılık çakışması gerçekleşir. Böyle bir çakışma algılanması etkinleştirmezseniz, kişi veritabanını güncelleştiren son diğer kullanıcının yaptığı değişikliklerin üzerine yazar. Birçok uygulamada, bu riski kabul edilebilir: Bazı kullanıcılara veya birkaç güncelleştirmeleri varsa veya bazı değişikliklerin üzerine yazılır, programlama için eşzamanlılık maliyeti gereksinimlerine ağır bastığı avantajı gerçekten önemli değildir. Bu durumda, eşzamanlılık çakışmalarını işleme için uygulamayı yapılandırmanız gerekmez.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybı önleme ihtiyacınız varsa, bunu yapmanın bir yolu veritabanı kilitlerini kullanmaktır. Bu adlandırılır *kötümser eşzamanlılık*. Örneğin, bir veritabanından satır okumadan önce kilit isteği salt okunur veya güncelleştirme erişim için. Güncelleştirme erişimi için bir satır kilitlerseniz, başka hiçbir kullanıcı için ya da satırın kilitlemek için izin salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişim güncelleştirin. Salt okunur erişim için bir satır kilitlerseniz, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek dezavantajları vardır. Programa karmaşık olabilir. Önemli bir veritabanı yönetim kaynakları gerektirir ve bu uygulamanın kullanıcı sayısı performans sorunlarına neden olabilir artar (diğer bir deyişle, iyi ölçeklendirme değil). Bu nedenle, tüm veritabanı yönetim sistemleri kötümser eşzamanlılık destekler. Entity Framework yerleşik desteği yok sağlar ve Bu öğreticide nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Kötümser eşzamanlılık alternatifi *iyimser eşzamanlılık*. İyimser eşzamanlılık, eşzamanlılık çakışmalarını olmasını sağlar ve eğer uygun şekilde tepki anlamına gelir. Örneğin, John Departmanlar Düzenle sayfasında, değişiklikleri çalışır **bütçe** 350,000.00 $ 0,00 ABD Doları İngilizce departmanına tutar.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John tıkladığında önce **Kaydet**, Jane çalışan aynı sayfa ve değişiklikleri **başlangıç tarihi** alanı 1/9/2007'den 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John tıkladığında **Kaydet** ilk ve tarayıcı dizin sayfasına, sonra Jane döndürdüğünde değişikliğinin tıkladığında görür **Kaydet**. Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir. Bazı seçenekleri şunlardır:

- Bir kullanıcı değiştirmiş hangi özelliğinin izlemek ve yalnızca ilgili sütunları veritabanında güncelleştirin. Farklı özellikleri iki kullanıcı tarafından güncelleştirildiği için yer alan örnek senaryoda, veri kaybı, olacaktır. Sonraki biri İngilizce departmanı gözatar, Can'ın hem Gamze'nin değişiklikleri görürler: 8/8/2013'ün Başlangıç tarihi ve bütçe sıfır dolarlık bir.

    Bu güncelleştirme yöntemini veri kaybına neden olabilecek çakışmaları sayısını azaltabilir, ancak bir varlığın aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz. Varlık Çerçevesi'Bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır. Bir varlığın özgün tüm özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir içinde bir web uygulaması pratik değildir, çünkü genellikle. Sunucu kaynaklarını gerektirir ya da web sayfasında kendisi (örneğin, gizli alanlar) dahil edilmesi için büyük miktarda durum koruma uygulama performansını etkileyebilir.
- Gamze'nin değişiklik Can'ın değişiklik üzerine izin verebilirsiniz. Sonraki biri İngilizce departmanı gözatar, 8/8/2013 görürler ve geri yüklenen $350,000.00 değeri. Bu adlı bir *istemci WINS* veya *WINS'te son* senaryo. (İstemcinin değerleri veri deposunda nedir üzerinde önceliklidir.) Bu bölüm için giriş eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.
- Veritabanında güncelleştirilen Gamze'nin değişiklik engelleyebilirsiniz. Genellikle, bir hata iletisi görüntüler, her verilerin geçerli durumunu gösterir ve her yine de bunları yapmak istediği değişiklikleri yeniden uygulamak izin. Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulayacaksınız. Bu yöntem, neler olduğunu için uyarı bir kullanıcı olmaksızın herhangi bir değişiklik kılınmamasını sağlar.

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmaları algılama

İşleyerek çakışmalarını çözebilirsiniz [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework oluşturduğu özel durumları. Bu özel durumlar ne zaman öğrenmek için Entity Framework algılayamayabilir çakışmalar olması gerekir. Bu nedenle, veritabanı ve veri modeline uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler şunlardır:

- Veritabanı tablosunda bir satıra değiştirildiğinde belirlemek için kullanılan bir izleme sütunu içerir. Ardından bu sütunu eklemek için Entity Framework yapılandırabilirsiniz `Where` SQL yan tümcesi `Update` veya `Delete` komutları.

    İzleme sütunun veri türü genellikle [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) satır güncelleştirilir her zaman artan sıralı bir sayı değeridir. İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir. Güncelleştirilen satır değeri başka bir kullanıcı tarafından değiştirildiyse `rowversion` sütunudur özgün değerinden farklı şekilde `Update` veya `Delete` bildirimi nedeniyle güncelleştirilecek satırın bulamıyor `Where` yan tümcesi. Entity Framework bulduğunda hiçbir satır tarafından güncelleştirildiğini `Update` veya `Delete` komutunu (diğer bir deyişle, etkilenen satır sayısı sıfır olduğunda), bir eşzamanlılık çakışması yorumlar.
- Tablodaki her sütun öğesinin özgün değerleri eklemek için Entity Framework yapılandırma `Where` yan tümcesi `Update` ve `Delete` komutları.

    İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi `Where` yan tümcesi bir satırı güncelleştirmek için iade kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Birçok sütunları olan veritabanı tabloları için bu yaklaşım, çok büyük sonuçlanabilir `Where` yan tümcesi ve büyük miktarlarda durumu bakımını gerektirebilir. Sunucu kaynaklarını gerektirir ya da web sayfasının kendisine eklenmesi gerekir çünkü daha önce belirtildiği gibi büyük miktarlarda durum koruma uygulama performansını etkileyebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.

    Eşzamanlılık için bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretlemek sahip [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) öznitelik. Değişiklik SQL tüm sütunları eklemek Entity Framework sağlar `WHERE` yan tümcesi `UPDATE` deyimleri.

Bu öğreticinin geri kalanında, ekleyeceksiniz bir [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) özelliğini izleme `Department` varlık, bir denetleyici ve Görünüm ve her şeyin düzgün çalıştığını doğrulamak için test edin.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Departman varlık için bir iyimser eşzamanlılık özelliği Ekle

İçinde *Models\Department.cs*, adlı bir izleme özelliği ekleme `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Zaman damgası](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) özniteliği belirtir, bu sütun eklenecektir `Where` yan tümcesi `Update` ve `Delete` veritabanına gönderilen komutları. Adlandırılan öznitelik [zaman damgası](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü [zaman damgası](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) veri türü SQL önce [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) değiştirildi. .Net türü `rowversion` bir bayt dizisidir. Fluent API'sini kullanmayı tercih ederseniz kullanabilirsiniz [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) yöntemi izleme özelliği, aşağıdaki örnekte gösterildiği gibi belirtin:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özellik ekleyerek veritabanı modeli değişti. Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutları girin:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Bir bölüm denetleyicisi oluşturun

Oluşturma bir `Department` denetleyici ve görünüm aşağıdaki ayarları kullanarak diğer denetleyicilerin yaptığınız gibi:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

İçinde *Controllers\DepartmentController.cs*, ekleme bir `using` deyimi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Bölüm Yöneticisi açılan listeler, her yerde bu dosyadaki (dört yineleme) "Tam adı" için "LastName" değişiklik Eğitmen tam adı yerine yalnızca son adı içerir.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Mevcut kodu değiştirin `HttpPost` `Edit` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Görünümün özgün depolayacak `RowVersion` gizli bir alan değeri. Model bağlayıcı oluşturduğunda `department` örneği, o nesnenin orijinal olacaktır `RowVersion` özellik değeri ve Düzenle sayfasında kullanıcı tarafından girildiği gibi diğer özellikleri için yeni değerler. Entity Framework bir SQL zaman oluşturur `UPDATE` komutu komut içerecek bir `WHERE` özgün sahip bir satır için görünen yan tümcesi `RowVersion` değeri.

Hiçbir satır etkileniyorsanız `UPDATE` komut (hiçbir satır özgün sahip `RowVersion` değer), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durum ve kodda `catch` bloğunu alır etkilenen `Department` özel bir varlıktan nesne. Bu varlığın hem veritabanından okunan değerleri hem de kullanıcı tarafından girilen yeni değerleri vardır:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ardından, kod veritabanı değerlerini hangi kullanıcının düzenleme sayfada girilen öğesinden farklı olan her sütun için bir özel hata iletisi ekler:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Uzun bir hata iletisi, ne olduğunu ve bunu yapmanız gerekenler açıklanmaktadır:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Son olarak, kod ayarlar `RowVersion` değerini `Department` yeni değere nesne veritabanından alınır. Bu yeni `RowVersion` değeri depolanan gizli alanı sayfasını yeniden düzenleme ve sonraki kullanıcı çalıştırdığında **Kaydet**, yeniden düzenleme sayfası, yakalanan bu yana gerçekleşen eşzamanlılık hataları.

İçinde *Views\Department\Edit.cshtml*, kaydetmek için gizli bir alan ekleme `RowVersion` özellik değeri, gizli bir alan için takip `DepartmentID` özelliği:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

İçinde *Views\Department\Index.cshtml*, mevcut kodu satır bağlantıları sola taşı ve görüntülemek için sayfa başlığı ve sütun başlıkları değiştirmek için aşağıdaki kodla değiştirin `FullName` yerine `LastName` içinde**Yönetici** sütun:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>İyimser eşzamanlılık işleme test etme

Siteyi çalıştırın ve tıklayın **Departmanlar**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Sağ tıklayın **Düzenle** Kim Abercrombie ve seçin için köprü **yeni sekmede aç** ardından **Düzenle** Kim Abercrombie için köprü. İki windows aynı bilgileri görüntüler.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

İlk tarayıcı penceresinde bir alanı değiştirmek ve tıklayın **Kaydet**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Tarayıcı değişmiş değer ile dizin sayfası gösterilir.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

İkinci bir tarayıcı penceresi içindeki herhangi bir alan değiştirip'ı **Kaydet**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Tıklayın **Kaydet** ikinci bir tarayıcı penceresi içinde. Bir hata iletisi görürsünüz:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Tıklayın **Kaydet** yeniden. İkinci tarayıcıda girdiğiniz değer, ilk tarayıcıda değiştirme verileri özgün değeri ile birlikte kaydedilir. Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete sayfa güncelleştiriliyor

Silme sayfası için Entity Framework, birisi başka benzer bir şekilde bölüm düzenleme nedeni eşzamanlılık çakışmalarını algılar. Zaman `HttpGet` `Delete` yöntemi görüntüler onay görünümü, görünümün özgün içerir `RowVersion` gizli bir alan değeri. Değer daha sonra kullanılabilir olduğunu `HttpPost` `Delete` kullanıcının silmeyi onaylaması çağrılan yöntem. Entity Framework, SQL oluşturduğunda `DELETE` komutunu içerdiği bir `WHERE` yan tümcesinin orijinal `RowVersion` değeri. Sıfır satır komutu sonuçları (satır silme onayı sayfasında görüntülenen sonra değiştirildiği anlamına gelir) etkileniyorsa, bir eşzamanlılık özel durum oluşturulur ve `HttpGet Delete` ayarlamak bir hata bayrağıyla yöntemi çağrıldığında `true` görüntülemek için bir hata iletisiyle onay sayfası. Bu durumda farklı bir hata iletisi görüntülenir, böylece satırın başka bir kullanıcı tarafından silindiğinden sıfır satır etkilenmiştir mümkündür.

İçinde *DepartmentController.cs*, değiştirin `HttpGet` `Delete` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Yöntemi, bir eşzamanlılık hatası sonra sayfayı yeniden olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu bayrak ise `true`, görünümünü kullanarak bir hata iletisi gönderilen bir `ViewBag` özelliği.

Değiştirin `HttpPost` `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Yalnızca değiştirilen iskele kurulmuş kod içinde bu yöntem yalnızca bir kayıt kimliği kabul:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Bu parametre için değiştirdiyseniz bir `Department` model bağlayıcı tarafından oluşturulan varlık örneği. Bu aşağıdakilere erişmenizi sağlar `RowVersion` kayıt anahtarı yanı sıra özellik değeri.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Eylem yöntemi adından de değiştirdiğiniz `DeleteConfirmed` için `Delete`. İskele kurulan kodu adlı `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek `HttpPost` yöntemi benzersiz bir imza. (CLR'nin farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile devam edin ve aynı adı kullanın `HttpPost` ve `HttpGet` yöntemlerini silin.

Bir eşzamanlılık hatası yakalanmışsa kod silme onayı sayfası görüntüler ve bunu belirten bir bayrak eşzamanlılık hata iletisi görüntülenmelidir sağlar.

İçinde *Views\Department\Delete.cshtml*, bazı biçimlendirme sağlayan aşağıdaki kod ile iskele kurulan kodu değiştirir ve bir hata iletisi alan ekler değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Bu kod arasında bir hata iletisi ekler `h2` ve `h3` başlıkları:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Değiştirir `LastName` ile `FullName` içinde `Administrator` alan:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Son olarak, onu gizli alanlar için ekler `DepartmentID` ve `RowVersion` sonra özellikleri `Html.BeginForm` deyimi:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Departmanlar dizin sayfası çalıştırın. Sağ tıklayın **Sil** seçin ve İngilizce departmanı için köprü **yeni pencerede aç** ilk penceresinde ardından **Düzenle** İngilizce için köprü bölüm.

İlk penceresinde değerlerden birini değiştirin ve tıklayın **Kaydet** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Dizin Sayfası değişikliği doğrular.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

İkinci pencerede **Sil**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Eşzamanlılık hata iletisini görüntüleyin ve departman değerlerini şu anda veritabanında nedir ile yenilenir.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Tıklarsanız **Sil** yeniden departman silindiğini gösterir dizin sayfasına yönlendirilirsiniz.

## <a name="summary"></a>Özet

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. Çeşitli eşzamanlılık senaryoları işlemek için diğer yollar hakkında daha fazla bilgi için bkz: [iyimser eşzamanlılık desenlerinin](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) ve [özellik değerleri ile çalışma](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) Entity Framework ekip blogunda. Sonraki öğreticide için tablo başına hiyerarşi devralma uygulamak gösterilmektedir `Instructor` ve `Student` varlıklar.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
