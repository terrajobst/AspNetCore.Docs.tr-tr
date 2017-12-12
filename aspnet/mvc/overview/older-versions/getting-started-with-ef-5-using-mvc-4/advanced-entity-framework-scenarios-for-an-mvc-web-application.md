---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Gelişmiş bir MVC Web uygulaması (10 / 10) için Entity Framework senaryoları | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Gelişmiş Entity Framework senaryoları için bir MVC Web uygulaması (10 / 10)
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide depo ve iş desenleri biriminin uygulanmadı. Bu öğretici, aşağıdaki konuları içerir:

- Ham SQL sorguları gerçekleştirme.
- Hayır-izleme sorguları gerçekleştirme.
- Sorguları inceleniyor veritabanına gönderilir.
- Proxy sınıflarıyla çalışıyor.
- Değişiklikleri otomatik algılanmasını devre dışı bırakılıyor.
- Doğrulama kaydedilirken devre dışı bırakma değiştirir.
- [Hatalar ve çözüm, geçici çözüm](#errors)

Bu konular çoğu için daha önce oluşturduğunuz sayfaları ile çalışmak. Ham SQL güncelleştirmeleri toplu olarak kullanmak için güncelleştirme veritabanındaki tüm kursları krediler sayısı yeni bir sayfa oluşturursunuz:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Ve departman Düzenle sayfasına yeni doğrulama mantığını ekleyeceksiniz Hayır izleme sorgusu kullanmak için:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Gerçekleştirme ham SQL sorguları

Entity Framework kod ilk API, SQL komutlarını veritabanına doğrudan geçirmenizi sağlayan yöntemler içerir. Şu seçenekleriniz vardır:

- Kullanım `DbSet.SqlQuery` varlık türleri döndüren sorgular için yöntem. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenir tarafından veritabanı bağlamı izleme kapatmak sürece. (Aşağıdaki bölüme bakın `AsNoTracking` yöntemi.)
- Kullanım `Database.SqlQuery` varlıkları olmayan türleri döndüren sorgular için yöntem. Varlık türlerini almak için bu yöntemi kullanmak olsa bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.
- Kullanım [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) sorgu dışı komutları için.

Entity Framework kullanmanın yararları, veri depolamanın çok yakından belirli bir yöntem kodunuzu bağlamadan önler biridir. SQL sorguları ve komutlar, ayrıca bunları kendiniz yazmak zorunda kalmaktan boşaltır oluşturarak bunu yapar. Ancak el ile oluşturduğunuz belirli SQL sorguları çalıştırmak gerektiğinde olağanüstü senaryolar vardır ve bu yöntemler, bu tür özel durumları işleme mümkün kılar.

Bir web uygulamasında SQL komutlarını yürüttüğünüzde her zaman true olarak, siteniz SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutlarını yorumlanan emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide kullanıcı girişi bir sorgu tümleştirdiğinizde parametreli sorgular kullanacaksınız.

### <a name="calling-a-query-that-returns-entities"></a>Bir sorgu çağırma varlıklar döndürüyor

İstediğiniz varsayalım `GenericRepository` ek filtreleme ve ek yöntemleriyle türetilmiş bir sınıf oluşturun gerek kalmadan esneklik sıralama sağlamak için sınıf. Bir SQL sorgusu kabul eden bir yöntem eklemek için olacak elde etmenin bir yolu. Sıralama veya filtreleme herhangi bir tür denetleyicisi gibi istediğiniz sonra belirtebilirsiniz bir `Where` bir birleştirmeler veya alt sorgu bağlıdır yan tümcesi. Bu bölümde, bu tür bir yöntem uygulamak nasıl görürsünüz.

Oluşturma `GetWithRawSql` aşağıdaki kodu ekleyerek yöntemi *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

İçinde *CourseController.cs*, yeni yöntemi çağırın `Details` yöntemi, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Bu durumda, kullanabilirdik `GetByID` yöntemi, ancak kullanmakta olduğunuz `GetWithRawSql` doğrulamak için yöntem `GetWithRawSQL` yöntemi çalışır.

Select works sorgu doğrulamak için Ayrıntılar sayfasını çalıştırın (seçin **Seyrinde** sekmesini ve ardından **ayrıntıları** bir indirmelere için).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Bir sorgu çağırma diğer nesne türlerini döndürür

Daha önce bir öğrenci istatistikleri kılavuz Öğrenciler sayısı için her kayıt tarihi gösterdi hakkında sayfası için oluşturduğunuz. Bunu yapan kod *HomeController.cs* LINQ kullanır:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Veriyi doğrudan LINQ kullanmak yerine SQL alır kod yazmaya istediğinizi varsayalım. Varlık nesnesi dışında bir şey döndüren bir sorgu çalıştırması gerekecek yapmak için anlamına kullanmanızı gerektiren `Database.SqlQuery` yöntemi.

İçinde *HomeController.cs*, LINQ deyiminde Değiştir `About` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Hakkında sayfayı çalıştırın. Uygulama önceden olduğu aynı verileri görüntüler.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Güncelleştirme sorgusu çağırma

Contoso University yöneticiler her indirmelere için iadeleri sayısını değiştirme veritabanındaki toplu değişiklikler gerçekleştirebilecek istediğinizi varsayalım. Çok sayıda kurslar university varsa, tüm varlıklar almak ve bunları tek tek değiştirmek için verimsiz olacaktır. Bu bölümde, tüm kurslara krediler sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulamak ve bir SQL yürüterek değişiklik `UPDATE` deyimi. Web sayfasını aşağıdaki gibi görünmelidir:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Önceki öğreticide okumak ve güncelleştirmek için genel depo kullanılan `Course` varlıklarda `Course` denetleyicisi. Bu toplu güncelleştirme işlemi için genel havuzda değil yeni bir havuz yöntemi oluşturmanız gerekir. Bunu yapmak için ayrılmış bir oluşturacağınız `CourseRepository` öğesinden türetilen sınıf `GenericRepository` sınıfı.

İçinde *DAL* klasörü oluşturmak *CourseRepository.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

İçinde *UnitOfWork.cs*, değiştirme `Course` depo türünden `GenericRepository<Course>` için`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

İçinde *CourseContoller.cs*, ekleme bir `UpdateCourseCredits` yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Bu yöntem her ikisi için kullanılacak `HttpGet` ve `HttpPost`. Zaman `HttpGet` `UpdateCourseCredits` yöntemi çalıştığında, `multiplier` değişken null olur ve boş bir metin kutusu ve bir gönderme düğmesi Yukarıdaki çizimde gösterildiği gibi görünümünü gösterir.

Zaman **güncelleştirme** düğmesine tıklandığında ve `HttpPost` yöntemi çalıştığında, `multiplier` metin kutusuna girdiğiniz değer olacaktır. Kod sonra depo çağırır `UpdateCourseCredits` sayısını döndürür yöntemi etkilenen satırlar ve bu değeri depolanan `ViewBag` nesnesi. Ne zaman görünümü alır etkilenen satır sayısı `ViewBag` nesnesi, metin kutusunun yerine bu sayı gösterir ve düğme, aşağıdaki çizimde gösterildiği gibi gönderin:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Bir görünüm oluşturma *Views\Course* güncelleştirme indirmelere krediler sayfa için klasör:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

İçinde *Views\Course\UpdateCourseCredits.cshtml*, şablon kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Çalıştırma `UpdateCourseCredits` seçerek yöntemi **kurslar** sonra ekleyerek sekmesinde, "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğundaki URL'yi (örneğin: `http://localhost:50205/Course/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Tıklatın **güncelleştirme**. Etkilenen satırların sayısını bakın:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Tıklatın **listesine dön** krediler düzenlenen sayısıyla kurslar listesini görmek için.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Ham SQL sorguları hakkında daha fazla bilgi için bkz: [ham SQL sorguları](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) Entity Framework ekip blogunda.

## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Veritabanı bağlamı veritabanı satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak veritabanında nedir ile varlıkları bellekte eşit olup olmadığını izler. Bellekteki veri önbelleği olarak davranır ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve atıldı her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlığın varlık yeniden kullanılmadan önce genellikle atıldı.

Bağlamı kullanarak bir sorgu için varlık nesnesi izler olup olmadığını belirtebilirsiniz `AsNoTracking` yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

- Sorgu, izleme devre dışı kapatma performansı önemli ölçüde artırmak veri büyük hacimli alır.
- Güncelleştirmek için bir varlık eklemek için kullanmak istediğiniz, ancak farklı bir amaç için daha önce aynı varlık alınır. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlık eklenemiyor. Bunun olmaması için bir yol kullanmaktır `AsNoTracking` önceki sorgu seçeneği.

Bu bölümde bu senaryoları ikinci gösterilmektedir iş mantığı uygulamanız. Özellikle, bir eğitmen birden fazla bölüm yönetici olamaz belirten bir iş kuralı zorlamak.

İçinde *DepartmentController.cs*, çağırmak yeni bir yöntem ekleyin `Edit` ve `Create` yöntemleri hiçbir iki Departmanlar aynı yönetici olduğundan emin olun:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Kod ekleme `try` , engellemek `HttpPost` `Edit` hiçbir doğrulama hatası varsa, bu yeni yöntemin çağrılacak yöntem. `try` Bloğu şimdi aşağıdaki gibi görünür:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Sayfa departmanı Düzenle çalıştırın ve bir departmandaki yönetici zaten farklı bir bölüm yöneticisi olan bir eğitmen değiştirmeyi deneyin. Beklenen hata iletisini alırsınız:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Departman Düzenle sayfası yeniden ve bu zaman değişikliği Şimdi Çalıştır **bütçe** tutar. Tıkladığınızda **kaydetmek**, bir hata sayfası bakın:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Özel durum hata iletisi "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Bu aşağıdaki olaylar dizisi nedeniyle oluşmuş:

- `Edit` Yöntem çağrılarını `ValidateOneAdministratorAssignmentPerInstructor` Kim Abercrombie kendi yönetici olarak sahip tüm bölümleri alır yöntemi. Okumak İngilizce departmanı neden olur. Düzenlenen bölüm olduğu için herhangi bir hata bildirilir. Bu okuma işlemi sonucunda, ancak, veritabanından okundu İngilizce departmanı varlık şimdi veritabanı bağlamı tarafından izleniyor.
- `Edit` Yöntemi çalıştığında ayarlamak `Modified` İngilizce bayrağı departmanı varlık MVC model bağlayıcı tarafından oluşturulan ancak bağlamı zaten bir varlık İngilizce departmanı için izleme çünkü başarısız.

Bu sorun için bir çözüm, doğrulama sorgu tarafından alınan bellek içi departmanı varlıklar izleme bağlamı korumaktır. Bu varlık güncelleştirme olmaz olduğundan, bunu veya yeniden dışarı bellekte önbelleğe alınması için yararlı bir şekilde okuma hiçbir olumsuz yoktur.

İçinde *DepartmentController.cs*, `ValidateOneAdministratorAssignmentPerInstructor` yöntemi, hiçbir izleme aşağıda gösterildiği gibi belirtin:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Düzenlenecek girişiminiz yineleyin **bütçe** bir bölüm miktarı. Bu süre işleminin başarılı olması ve Departmanlar dizin sayfasına yeniden düzenlenen Bütçe değeri gösteren, beklendiği gibi site döndürür.

## <a name="examining-queries-sent-to-the-database"></a>Veritabanına gönderilen sorguların inceleniyor

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmek için yararlıdır. Bunu yapmak için bir sorgu değişkeni hata ayıklayıcısında inceleyin veya sorgunun arama `ToString` yöntemi. Bu sorunu anlamak denemek için basit bir sorgu arayın ve yüklenirken, filtreleme ve sıralama böyle eager seçenekleri ekledikçe için olanlar adresindeki Ara.

İçinde *denetleyicileri/CourseController*, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Şimdi bir kesme noktası kümesinde *GenericRepository.cs* üzerinde `return query.ToList();` ve `return orderBy(query).ToList();` bilgilerinin `Get` yöntemi. Projeyi hata ayıklama modunda çalıştırın ve indirmelere dizin sayfasını seçin. Kod kesme noktasına ulaştığında, inceleyin `query` değişkeni. SQL Server'a gönderilen sorgu bakın. Basit bir olduğu `Select` deyimi:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Sorguları Visual Studio'da hata ayıklama Windows görüntülenmesi uzun olabilir. Tüm sorgu görmek için değişken değerini kopyalayın ve bir metin düzenleyicisine yapıştırın:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Şimdi böylece kullanıcılar için belirli bir bölüm filtreleyebilirsiniz indirmelere dizin sayfasına açılan listesini ekleyeceksiniz. Kurslar başlığa göre sıralamak ve istekli yükleme için belirtirsiniz `Department` gezinti özelliği. İçinde *CourseController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Aşağı açılan listesinde seçilen değeri yöntemi alır `SelectedDepartment` parametresi. Hiçbir şey seçili ise, bu parametre null olur.

A `SelectList` tüm bölümleri içeren bir koleksiyon için aşağı açılan liste görünümüne geçirilir. Parametreleri geçirilen `SelectList` Oluşturucusu değerini alan adını, metin alanı adı ve seçilen öğeyi belirtin.

İçin `Get` yöntemi `Course` deposu, kod belirtir bir filtre ifadesi, sıralama ve için yükleme eager `Department` gezinti özelliği. Filtre ifadesi her zaman döndürür `true` hiçbir şey aşağı açılan listesinde seçili olup olmadığını (diğer bir deyişle, `SelectedDepartment` null).

İçinde *Views\Course\Index.cshtml*, açmadan önce hemen `table` etiketi, aşağı açılan liste ve bir gönderme düğmesi oluşturmak için aşağıdaki kodu ekleyin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Hala kümesinde kesme noktalarıyla `GenericRepository` sınıfı, indirmelere dizin sayfası çalıştırın. Böylece sayfasını tarayıcıda görüntülenen kodu bir kesme noktası isabet ilk iki kez devam edin. Aşağı açılan listeden bir bölüm seçin ve tıklatın **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Bu süre, aşağı açılan liste Departmanlar sorgu için ilk kesme olacaktır. Atlayın ve görüntülemek `query` değişken sonraki kod ulaştığında kesme gördükleri için `Course` gibi görünüyor artık sorgu. Aşağıdakine benzer bir şey görürsünüz:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Sorgu artık olduğunu görebilirsiniz bir `JOIN` yükler sorgu `Department` veri ile birlikte `Course` veri ve onu içeren bir `WHERE` yan tümcesi.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Proxy sınıflar ile çalışma

Entity Framework varlık örnekleri (örneğin, bir sorgu yürütülürken), bu genellikle varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilen tür örneği olarak oluşturur. Bu proxy özelliği erişildiğinde eylemleri otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar. Örneğin, bu mekanizma ilişkileri yavaş yüklenmesini desteklemek için kullanılır.

Çoğu zaman bu proxy'leri kullanımını farkında olmanız gerekmez, ancak özel durum vardır:

- Bazı senaryolarda proxy örnekleri oluşturma Entity Framework engellemek isteyebilirsiniz. Örneğin, proxy olmayan örneklerini serileştirmek proxy örneklerini serileştirmek değerinden daha etkili olabilir.
- Ne zaman örneği kullanarak bir varlık sınıfı `new` işleci, bir proxy örneği Al yok. Bu, yavaş yükleniyor ve otomatik değişiklik izleme gibi işlevselliği elde etmezsiniz anlamına gelir. Bu genellikle Tamam olur; veritabanında olmayan yeni bir varlık oluşturmakta olduğunuz çünkü genellikle yavaş yükleniyor, gerekli olmayan ve genellikle açıkça bir varlık olarak işaretleme varsa izleme değişiklik gerekmez `Added`. Ancak, yavaş yüklenmesi gereken ve değişiklik izleme gereksiniminiz varsa, yeni varlık örneklerini kullanarak proxy'leri oluşturabileceğiniz `Create` yöntemi `DbSet` sınıfı.
- Gerçek bir varlık türü bir proxy türünden almak isteyebilirsiniz. Kullanabileceğiniz `GetObjectType` yöntemi `ObjectContext` sınıfı bir proxy türü örneğinin gerçek bir varlık türü alınamadı.

Daha fazla bilgi için bkz: [proxy'leri çalışma](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) Entity Framework ekip blogunda.

## <a name="disabling-automatic-detection-of-changes"></a>Değişiklikleri otomatik algılanmasını devre dışı bırakma

Entity Framework bir varlık nasıl değiştiğini (ve bu nedenle hangi veritabanına gönderilmesi gereken güncelleştirmeler) geçerli bir varlık değerleri özgün değerlerle karşılaştırarak belirler. Varlık sorgulanan ya da bağlı olduğunda özgün değerler depolanır. Otomatik değiştirme algılama neden yöntemlerin bazıları şunlardır:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Çok sayıda varlık takip ettiğiniz ve aşağıdaki yöntemlerden birini birçok kez bir döngüde çağırmanız, algılama otomatik değişiklik kullanarak geçici olarak kapatarak önemli performans geliştirmeleri alabilirsiniz [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) özelliği. Daha fazla bilgi için bkz: [değişiklikleri otomatik olarak algılama](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Doğrulama kaydedilirken devre dışı bırakma değiştirir

Çağırdığınızda `SaveChanges` yöntemi, Entity Framework varsayılan doğrular tüm değiştirilen varlıkların tüm özellikleri verilerde veritabanını güncelleştirmeden önce. Çok sayıda varlık güncelleştirdik ve bu iş gereksiz verileri zaten doğruladıktan ve kaydetme işlemi yapabilir değişiklikler geçici olarak doğrulama devre dışı bırakarak daha az zaman olur. Bu kullanarak yapabilirsiniz [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) özelliği. Daha fazla bilgi için bkz: [doğrulama](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Özet

Bu, bir ASP.NET MVC uygulamasındaki Entity Framework kullanma öğreticileri bu dizi tamamlar. Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

Temel aldık sonra web uygulamanızı dağıtma hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/en-us/library/bb386521.aspx) MSDN Kitaplığı'nda.

MVC için kimlik doğrulama ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz: [MVC önerilen kaynakları](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>İlgili kaynaklar

- Zel Dykstra özgün sürümü bu öğreticinin yazdı ve Microsoft Web Platformu ve araçları içerik ekibi Kıdemli bir programlama yazıcısı.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) Bu öğretici birlikte yazılmış ve işlerin çoğunu EF 5 ve MVC 4 için güncelleştirme vermedi. Rick Microsoft Azure ve MVC odaklanan için üst düzey bir programlama yazıcı ' dir.
- [Rowan Mert](http://www.romiller.com) ve diğer Entity Framework ekibi üyelerinin kod incelemeleri destekli biz öğretici için EF 5 güncelleştirmekte olduğunuz sırada çıkan geçişler ile birçok hatalarını ayıklamanıza yardımcı oldu.

## <a name="vb"></a>VB

Öğreticinin ilk olarak üretilen, biz tamamlanan yükleme proje hem C# ve VB sürümleri sağlamıştır. Bu güncelleştirmeyle biz bir C# indirilebilir projesi serideki ancak zaman sınırlamaları ve vb için bunu olmayan diğer öncelikleri nedeniyle herhangi bir yere başlamak kolaylaştırmak her bölüm için sağlanmaktadır Varsa bu öğreticileri kullanarak bir VB projeyi oluşturun ve diğer kişilerle paylaşmak için lütfen bize bildirin istekli olacaktır.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Hatalar ve geçici çözümler

### <a name="cannot-createshadow-copy"></a>Oluşturma/kopyalama gölge olamaz

Hata iletisi:

*Oluşturma/Kopyala 'DotNetOpenAuth.OpenId' Bu dosya zaten varken gölge olamaz.*

Çözüm:

Birkaç saniye bekleyin ve sayfayı yenileyin.

### <a name="update-database-not-recognized"></a>Update-Database tanınmıyor

Hata iletisi:

*'Update-Database' terimi, bir cmdlet, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol dahilse, yolun doğru olduğundan emin olun ve yeniden deneyin.* (Gelen  *`Update-Database`*  PMC komutunu.)

Çözüm:

Visual Studio'dan çıkın. Projeyi yeniden açın ve yeniden deneyin.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Hata iletisi:

*Bir veya daha fazla varlıklar için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.* (Gelen  *`Update-Database`*  PMC komutunu.)

Çözüm:

Bir bu sorunun nedeni doğrulama hataları olduğunda `Seed` yöntemi çalışır. Bkz: [Seeding ve hata ayıklama Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) hata ayıklama ipuçları için `Seed` yöntemi.

### <a name="http-50019-error"></a>HTTP 500.19 hata

Hata iletisi:

*Sayfayla ilgili yapılandırma verileri geçersiz olduğundan HTTP Hatası 500.19 - iç sunucu hatası istenen sayfaya erişilemiyor.*

Çözüm:

Bu hatayı alabilirsiniz bir çözüm, bunların aynı bağlantı noktası numarası kullanan her biri birden çok kopyasını kalmaktan yoludur. Genellikle, Visual Studio tüm örneklerini çıkma ardından proje üzerinde çalışan yeniden başlatma bu sorunu çözebilirsiniz. Bu işe yaramazsa, bağlantı noktası numarasını değiştirmeyi deneyin. Proje dosyasını sağ tıklatın ve ardından Özellikler'i tıklatın. Seçin **Web** sekmesini ve sonra bağlantı noktası numarasını değiştirin **proje URL'sini** metin kutusu.

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

*SQL Server bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örnek belirtilen hata bulma)*

Çözüm:

Bağlantı dizesini kontrol edin. Veritabanını el ile sildiyseniz, yapı dizesinde veritabanı adını değiştirin.

>[!div class="step-by-step"]
[Önceki](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[sonraki](building-the-ef5-mvc4-chapter-downloads.md)
