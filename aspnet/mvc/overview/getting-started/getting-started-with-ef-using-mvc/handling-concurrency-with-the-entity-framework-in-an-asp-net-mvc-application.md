---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Bir ASP.NET MVC 5 uygulamada (10 / 12) Entity Framework 6 eşzamanlılık işleme | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7b79503a1d297d40c6824f8b2b7bbc1f42b9fca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Bir ASP.NET MVC 5 uygulamada (10 / 12) Entity Framework 6 eşzamanlılık işleme
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki eğitimlerine verileri güncelleştirmek öğrendiniz. Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.

Çalışmak web sayfaları değiştireceğiz `Department` varlık eşzamanlılık hataları işlemek böylece. Aşağıdaki çizimler bir eşzamanlılık çakışması olursa, görüntülenen bazı iletiler de dahil olmak üzere dizin ve Delete sayfalarında gösterilir.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir eşzamanlılık çakışması düzenlemek için bir kullanıcı bir varlığın verileri görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri oluşur. Bu gibi çakışmaları algılama etkinleştirmezseniz aktaranın veritabanını güncelleştiren son diğer kullanıcının değişiklikleri geçersiz kılar. Birçok uygulamada, bu riski kabul edilebilir: birkaç kullanıcı ya da birkaç güncelleştirmeleri varsa veya bazı değişiklikleri üzerine yazılmışsa eşzamanlılık programlama maliyetini ağır avantajı gerçekten önemli değildir. Bu durumda, eşzamanlılık çakışmalarına için uygulamayı yapılandırma gerekmez.

### <a name="pessimistic-concurrency-locking"></a>Eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybını önlemek gerekiyorsa, bunu yapmanın bir yolu veritabanı kilitleri kullanmaktır. Bu adlı *eşzamanlılık*. Örneğin, bir veritabanından bir satır okumadan önce için bir kilit isteği salt okunur veya güncelleştirme erişim. Güncelleştirme erişim için bir satır kilitlemek, başka hiçbir kullanıcı için ya da satır kilitlemek için izin verilir salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişimi güncelleştirin. Salt okunur erişim için bir satır kilitlemek, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek dezavantajları vardır. Programa karmaşık olabilir. Önemli Veritabanı Yönetimi kaynaklarını gerektirir ve bu kullanıcı bir uygulamanın sayısı olarak performans sorunlarına neden olabilir artırır. Bu nedenlerle, tüm veritabanı yönetim sistemleri eşzamanlılık destekler. Entity Framework yerleşik hiçbir desteği sağlar ve bu öğreticiyi uyguladıktan nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Eşzamanlılık için alternatif *iyimser eşzamanlılık*. İyimser eşzamanlılık gerçekleşecek şekilde eşzamanlılık çakışması izin vererek ve içermiyorlarsa uygun şekilde tepki anlamına gelir. Örneğin, John Departmanlar düzenleme sayfasında, değişiklikleri çalıştıran **bütçe** $350,000.00 $0,00 İngilizce departmanına tutar.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John tıklar önce **kaydetmek**, Jane çalıştıran aynı sayfa ve değişiklikleri **başlangıç tarihi** alanı 1/9/2007'den 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John tıklar **kaydetmek** ilk ve tarayıcı sonra Jane dizin sayfasına geri döndüğünde, değişiklik tıklar görür **kaydetmek**. Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir. Bazı seçenekleri şunlardır:

- Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara veritabanındaki güncelleştirin. Farklı özellikler iki kullanıcı tarafından güncelleştirildi olduğundan örnek senaryoda, hiçbir veri kaybı, olacaktır. Sonraki birisi İngilizce departmanı gözatar, Can'ın ve Jane'nın değişiklikleri görürsünüz — bir 8/8/2013 başlangıç tarihini ve bütçe sıfır Doları bir.

    Güncelleştirme bu yöntem veri kaybına sebep çakışmaları sayısını azaltır, ancak bir varlık aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz. Entity Framework bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır. Bir varlık için tüm özgün özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir bir web uygulamasında pratik değildir, çünkü genellikle. Büyük miktarlarda durumu koruma uygulama performansı etkileyebilir sunucu kaynakları gerekir ya da web sayfasındaki kendisini (örneğin, gizli alanlar) dahil gerekir çünkü ya da bir tanımlama bilgisinde.
- Jane'nın değişiklik Can'ın değişiklik üzerine izin verebilirsiniz. Sonraki birisi İngilizce departmanı gözatar, 8/8/2013 görürsünüz ve geri yüklenen $350,000.00 değeri. Bu adlı bir *istemci WINS* veya *WINS'de son* senaryo. (İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Bu bölüm için giriş hiçbir eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.
- Veritabanında güncelleştirilen Jane'nın değişiklik engelleyebilir. Genellikle, bir hata iletisi görüntüler her verilerin geçerli durumunu gösterir ve her aynen bunları olmak istiyorsa, kendi değişiklikleri uygulamak izin. Bu adlı bir *deposu WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulama. Bu yöntem, hiçbir değişiklik olanlar için uyarı bir kullanıcı olmaksızın üzerine yazılır sağlar.

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını algılama

İşleyerek çakışmalarını çözme [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework oluşturur özel durumları. Bu özel durumlar oluşturma zamanı bilmek için Entity Framework çakışmaları algılayabilir olması gerekir. Bu nedenle, veritabanı ve veri modelinin uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler aşağıdakileri içerir:

- Veritabanı tablosunda bir satırı değiştiğinde belirlemek için kullanılan bir izleme sütun içerir. Daha sonra bu sütununu dahil etmek için Entity Framework yapılandırabilirsiniz `Where` yan tümcesi SQL `Update` veya `Delete` komutları.

    İzleme sütunun veri türünü genellikle [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) değerdir satır güncelleştirilir her zaman artar sıralı bir sayı. İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir. Güncelleştirilen satır başka bir kullanıcı tarafından değeri değiştirildiğinde, `rowversion` sütundur özgün değerinden farklı şekilde `Update` veya `Delete` deyimi nedeniyle güncelleştirilecek satır bulamıyor `Where` yan tümcesi. Entity Framework bulduğunda tarafından hiçbir satırın güncelleştirilmediği `Update` veya `Delete` komutunu (diğer bir deyişle, etkilenen satırların sayısı sıfır olduğunda), bir eşzamanlılık çakışması yorumlar.
- Tablodaki her sütunun özgün değerler eklemek için Entity Framework yapılandırma `Where` yan tümcesi `Update` ve `Delete` komutları.

    İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi `Where` yan tümcesi güncelleştirmek için bir satır dönüş kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Çok sayıda sütuna sahip veritabanı tabloları için bu yaklaşım çok büyük sonuçlanabilir `Where` yan tümceleri ve büyük miktarlarda durumu bakımını gerektirebilir. Daha önce belirtildiği gibi büyük miktarlarda durumu koruma uygulama performansını etkileyebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.

    Eşzamanlılık bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri'nde ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretleyin zorunda [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) onlara özniteliği. Değişiklik SQL'de tüm sütunları içerecek şekilde Entity Framework sağlar `WHERE` yan tümcesi `UPDATE` deyimleri.

Bu öğreticinin geri kalanında, ekleyeceksiniz bir [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) özelliğine izleme `Department` varlık, bir denetleyici ve görünümler oluşturma ve her şeyin doğru şekilde çalıştığını doğrulamak için test edin.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Departman varlığa bir iyimser eşzamanlılık özellik ekleme

İçinde *Models\Department.cs*, adlı izleme özellik ekleme `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Zaman damgası](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) özniteliği belirtir. Bu sütun olarak eklenecek `Where` yan tümcesi `Update` ve `Delete` veritabanına gönderilen komutları. Öznitelik adı verilen [zaman damgası](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü [zaman damgası](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) önce SQL veri türü [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) yerine. .Net türü için [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) bir bayt dizisi.

Fluent API kullanmayı tercih ederseniz kullanabilirsiniz [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) yöntemi izleme özelliği aşağıdaki örnekte gösterildiği gibi belirtin:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özelliği ekleyerek veritabanı modeli değişti. Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutları girin:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Departman denetleyicisi değiştirme

İçinde *Controllers\DepartmentController.cs*, ekleme bir `using` deyimi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

İçinde *DepartmentController.cs* dosya, departman yönetici açılan listeleri Eğitmen tam adı yerine yalnızca son adını içerecek şekilde "Soyadı" dört tüm oluşumlarını "FullName" değiştirin.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Varolan kodunu değiştir `HttpPost` `Edit` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Varsa `FindAsync` yöntemi null değerini döndürür, departman başka bir kullanıcı tarafından silindi. Gösterilen kodu gönderilen form değerleri düzenleme sayfasını bir hata iletisi ile görünürler bir bölüm varlık oluşturmak üzere kullanır. Alternatif olarak, departman alanları yeniden görüntüleme olmadan yalnızca bir hata iletisi görüntülerseniz departmanı varlık yeniden oluşturmak zorunda olmayacaktır.

Özgün görünümü depolar `RowVersion` değere gizli bir alan ve yöntemi alır içinde `rowVersion` parametresi. Çağırmadan önce `SaveChanges`, özgün put zorunda `RowVersion` özellik değeri `OriginalValues` varlık koleksiyonu. Ardından Entity Framework bir SQL oluşturduğunda `UPDATE` komutunu komutu içerecek bir `WHERE` özgün sahip bir satır için arar yan tümcesi `RowVersion` değeri.

Hiçbir satır etkilenir, `UPDATE` komutu (özgün hiçbir satır sahip `RowVersion` değeri), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durumu ve kodda `catch` bloğunu alır etkilenen `Department` özel varlıktan nesne.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Bu nesne kullanıcı tarafından girilen yeni değerleri olan kendi `Entity` özelliği ve çağırarak veritabanından okunan değerleri alabilirsiniz `GetDatabaseValues` yöntemi.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Yöntemi birisi satır veritabanından silmişse null değerini döndürür; Aksi halde, döndürülen nesnesi yayınlanamıyor sahip `Department` erişmek için sınıf `Department` özellikleri. (Zaten silinmek üzere işaretli olduğundan `databaseEntry` departman sonra silindiyse null olacak `FindAsync` yürütür ve önce `SaveChanges` yürütür.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Ardından, kod ne kullanıcı düzenleme sayfasında girilen öğesinden farklı veritabanı değerlerine sahip her bir sütun için bir özel hata iletisi ekler:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Uzun bir hata iletisi ne olduğu ve bu konuda yapmanız gerekenler açıklanmaktadır:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Son olarak, kod ayarlar `RowVersion` değerini `Department` yeni değer nesnesine veritabanından alınır. Bu yeni `RowVersion` değeri sayfa yeniden düzenleme ve sonraki kullanıcı çalıştırdığında gizli alanında depolanacağı **kaydetmek**, düzenleme sayfasını yeniden yakalanan bu yana, eşzamanlılık hataları.

İçinde *Views\Department\Edit.cshtml*, kaydetmek için gizli bir alan eklemek `RowVersion` hemen gizli alanı için aşağıdaki özellik değeri `DepartmentID` özelliği:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>İyimser eşzamanlılık işleme test etme

Siteyi çalıştırın ve tıklayın **Departmanlar**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Sağ tıklayın **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç** ardından **Düzenle** İngilizce departmanı için köprü. İki sekme aynı bilgileri görüntüler.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

İlk tarayıcı sekmesinde alanında değiştirip'ı **kaydetmek**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Tarayıcı değiştirilen değerle dizin sayfası gösterilir.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

İkinci tarayıcı sekmesinde alanında değiştirip'ı **kaydetmek**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Tıklatın **kaydetmek** ikinci tarayıcı sekmesinde. Bir hata iletisi görüntülenir:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Tıklatın **kaydetmek** yeniden. İkinci bir tarayıcı sekmesinde girilen değerin ilk tarayıcıda değiştirilen veri özgün değeri birlikte kaydedilir. Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete sayfa güncelleştiriliyor

Delete sayfa için Entity Framework birisi başka departman benzer bir şekilde düzenleyerek nedeni eşzamanlılık çakışması algılar. Zaman `HttpGet` `Delete` yöntemi onay görünümü görüntüler, özgün görünümü içerir `RowVersion` gizli bir alan değeri. Değer sonra kullanılabilir olduğunu `HttpPost` `Delete` kullanıcı silme doğruladığında çağrılan yöntem. Entity Framework, SQL oluşturduğunda `DELETE` komutu, içerdiği bir `WHERE` yan tümcesiyle birlikte özgün `RowVersion` değeri. Sıfır satır komutu sonuçlarında (satır silme onayı sayfası görüntülendikten sonra değiştirildiği anlamına gelir) etkilenen, bir eşzamanlılık özel durum oluşur ve `HttpGet Delete` yöntemi, bir hata bayrağı çağrılır `true` görüntülemek için onay sayfasında bir hata iletisi ile. Bu durumda farklı bir hata iletisi görüntülenir şekilde satır başka bir kullanıcı tarafından silindiği için sıfır satır etkilendi mümkündür.

İçinde *DepartmentController.cs*, yerine `HttpGet` `Delete` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Yöntemi, bir eşzamanlılık hatası sonra sayfanın görünürler olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu bayrak ise `true`, görünümü kullanarak bir hata iletisi gönderilen bir `ViewBag` özelliği.

Kodla `HttpPost` `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Yalnızca değiştirilen kurulmuş kodda, bu yöntem yalnızca bir kayıt kimliği kabul:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Bu parametre için değiştirdiyseniz bir `Department` model bağlayıcı tarafından oluşturulan varlık örneği. Bu, erişim sağlayan `RowVersion` kayıt anahtarı ek özellik değeri.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ayrıca eylem yönteminin adı değiştirilmiştir `DeleteConfirmed` için `Delete`. Adlı kurulmuş kodu `HttpPost` `Delete` yöntemi `DeleteConfirmed` vermek için `HttpPost` yöntemi benzersiz bir imza. (CLR farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile takılıyor ve için aynı adı kullanmak `HttpPost` ve `HttpGet` yöntemlerini silin.

Bir eşzamanlılık hatası yakalanmışsa kodu silme onayı sayfası görüntüler ve bunu belirten bir bayrak bir eşzamanlılık hata iletisi görüntülenmelidir sağlar.

İçinde *Views\Department\Delete.cshtml*, kurulmuş kodu bir hata iletisini alan ve Gizli alanlar DepartmentID ve RowVersion özellikleri için ekleyen aşağıdaki kod ile değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Bu kod hata iletisine arasında ekler `h2` ve `h3` başlıkları:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Yerini `LastName` ile `FullName` içinde `Administrator` alan:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Son olarak, gizli alanlar için ekler `DepartmentID` ve `RowVersion` sonra özellikleri `Html.BeginForm` deyimi:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Departmanlar dizin sayfası çalıştırın. Sağ tıklayın **silmek** seçin ve İngilizce departmanı için köprü **yeni sekmede aç** sonra ilk sekmesini **Düzenle** İngilizce departmanı için köprü.

İlk penceresinde değerlerden birini değiştirmek ve tıklatın **kaydetmek** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Dizin Sayfası değişikliği onaylar.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

İkinci sekmesini tıklatın **silmek**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Eşzamanlılık hata iletisine bakın ve departman değerleri şu anda veritabanında nedir ile yenilenir.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Tıklatırsanız **silmek** yeniden departman silinip silinmediğini gösterir dizin sayfasına yönlendirilirsiniz.

## <a name="summary"></a>Özet

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. Çeşitli eşzamanlılık senaryoları işlemek için diğer yolları hakkında daha fazla bilgi için bkz: [iyimser eşzamanlılık desenleri](https://msdn.microsoft.com/en-us/data/jj592904) ve [özellik değerleri ile çalışma](https://msdn.microsoft.com/en-us/data/jj592677) konusuna bakın. Sonraki öğretici için tablo başına hiyerarşisi devralma uygulamak gösterilmiştir `Instructor` ve `Student` varlıklar.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[sonraki](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
