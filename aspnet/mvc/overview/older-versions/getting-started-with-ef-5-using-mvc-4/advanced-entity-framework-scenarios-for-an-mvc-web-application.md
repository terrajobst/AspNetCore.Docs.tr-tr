---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Bir MVC Web uygulaması (10 / 10) için Gelişmiş Entity Framework senaryoları | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5fa6ff439790f70cd60d0985a791df26d69c7107
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753606"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Bir MVC Web uygulaması (10 / 10) için Gelişmiş Entity Framework senaryoları
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide depo ve iş birimi desenleri uygulanır. Bu öğretici, aşağıdaki konuları içerir:

- Ham SQL sorguları gerçekleştirme.
- Hayır-izleme sorguları gerçekleştirme.
- Sorguları İnceleme veritabanına gönderilir.
- Proxy sınıfları ile çalışma.
- Değişiklikleri otomatik olarak algılanmasını devre dışı bırakılıyor.
- Doğrulama kaydedilirken devre dışı bırakma değiştirir.
- [Hatalar ve çözüm, geçici çözüm](#errors)

Bu konular çoğu için önceden oluşturulmuş sayfaları ile çalışırsınız. Ham SQL güncelleştirmeleri toplu olarak kullanmak için güncelleştirme veritabanındaki tüm kursları kredisi sayısı yeni bir sayfa oluşturacaksınız:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Ve Hayır izleme sorgusu kullanılacak bölüm düzenleme sayfasına yeni Doğrulama mantığı ekleyeceksiniz.:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Ham SQL sorguları gerçekleştirme

Entity Framework kod ilk API SQL komutları doğrudan veritabanına geçirilecek sağlayan yöntemler içerir. Şu seçenekleriniz vardır:

- Kullanım `DbSet.SqlQuery` varlık türleri döndüren sorgular için yöntemi. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenir tarafından veritabanı bağlamı izlemeyi devre dışı sürece. (Aşağıdaki bölüme bakın `AsNoTracking` yöntemi.)
- Kullanım `Database.SqlQuery` varlıkları olmayan türleri döndüren sorgular için yöntemi. Varlık türleri almak için bu yöntem kullansanız bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.
- Kullanım [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) sorgu dışı komutları için.

Entity Framework kullanmanın avantajlarını önler, veri depolamanın çok yakından belirli bir yöntem, kodunuzu bağlamadan biridir. Bunu SQL sorgulara ve komutlara sizin için Ayrıca bunları kendiniz yazmak zorunda kalmanızı oluşturarak yapar. Ancak, el ile oluşturduğunuz belirli SQL sorguları çalıştırmak ihtiyacınız olduğunda olağanüstü senaryolar vardır ve bu yöntemler tür özel durumları işlemek için mümkün kılar.

Bir web uygulamasında SQL komutları yürütme her zaman true olduğu gibi sitenizi SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutları yorumlanamıyor emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide, kullanıcı girişi bir sorguya tümleştirdiğinizde parametreli sorgular kullanacaksınız.

### <a name="calling-a-query-that-returns-entities"></a>Çağıran bir sorgu varlıklar döndürüyor

İstediğiniz varsayalım `GenericRepository` ek filtreleme ve ek yöntemlere sahip türetilmiş bir sınıf oluşturma gerek kalmadan esneklik sıralama sağlamak için sınıf. Bir SQL sorgusu kabul eden bir yöntem ekleyin, elde etmenin bir yolu olabilir. Ardından istediğiniz denetleyicisi gibi her tür filtreleme veya sıralama belirtebilirsiniz bir `Where` yan tümcesi bir birleşim veya alt sorgu bağlıdır. Bu bölümde, bu tür bir yönteminizi nasıl uygulayacağınızı görürsünüz.

Oluşturma `GetWithRawSql` yöntemine aşağıdaki kodu ekleyerek *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

İçinde *CourseController.cs*, yeni yöntemi çağırın `Details` aşağıdaki örnekte gösterildiği gibi yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Bu durumda, kullanabilirdiniz `GetByID` yöntemi, ancak kullanmakta olduğunuz `GetWithRawSql` doğrulamak için yöntem `GetWithRawSQL` yöntemi çalışır.

Seç'ın çalıştığı sorgu doğrulamak için Ayrıntılar sayfasını çalıştırın (seçin **kurs** sekmesini ve ardından **ayrıntıları** bir kurs için).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Çağıran bir sorgu diğer nesne türlerini döndürür

Daha önce bir öğrenci istatistikleri kılavuz Öğrenci sayısı için her bir kayıt tarihi gösterdi hakkında sayfası için oluşturuldu. Bunu yapar kodu *HomeController.cs* LINQ kullanır:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Bu verileri doğrudan LINQ kullanmak yerine SQL alır kod yazmak istediğiniz varsayalım. Varlık nesnesi dışında bir şey döndüren bir sorgu çalıştırmak ihtiyacınız olan şeyi anlamına gelir, gerek kullanılacak `Database.SqlQuery` yöntemi.

İçinde *HomeController.cs*, LINQ deyiminde değiştirin `About` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Hakkında sayfayı çalıştırın. Bu, daha önceki işlevlerini sürdürmektedir aynı verileri görüntüler.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Update sorgusu çağırma

Her kurs sonunda verilen kredi sayısı değiştirme gibi bu veritabanındaki toplu değişiklikleri gerçekleştirmek contoso University Yöneticiler istediğinizi varsayalım. University dersleri çok sayıda varsa, bunları tüm varlıklar olarak almak ve bunları tek tek değiştirmek için verimsiz olabilir. Bu bölümde, tüm kursları için kredi sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulayacaksınız ve bir SQL yürüterek değişiklik yapacaksınız `UPDATE` deyimi. Web sayfasını aşağıdaki gibi görünür:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Önceki öğreticide okumak ve güncellemek için genel deponun kullanılan `Course` varlıklarda `Course` denetleyicisi. Bu toplu güncelleştirme işlemi için genel depoda olmayan yeni bir depo yöntemi oluşturmanız gerekir. Bunu yapmak için ayrılmış bir oluşturursunuz `CourseRepository` türetilen sınıf `GenericRepository` sınıfı.

İçinde *DAL* klasör oluşturma *CourseRepository.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

İçinde *UnitOfWork.cs*, değiştirme `Course` depo türünden `GenericRepository<Course>` için `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

İçinde *CourseContoller.cs*, ekleme bir `UpdateCourseCredits` yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Bu yöntem her ikisi için kullanılacak `HttpGet` ve `HttpPost`. Zaman `HttpGet` `UpdateCourseCredits` yöntemi çalışır `multiplier` değişkeni null olacaktır ve boş bir metin kutusu ve bir Gönder düğmesi önceki resimde gösterildiği gibi görünümünü gösterir.

Zaman **güncelleştirme** düğmesine tıklandığında ve `HttpPost` yöntemi çalışır `multiplier` metin kutusuna girilen değer olacaktır. Kodu daha sonra depo çağırır `UpdateCourseCredits` etkilenen satır sayısını döndüren bir yöntemi ve bu değeri depolanan `ViewBag` nesne. Görünüm, etkilenen satır sayısı aldığında `ViewBag` nesnesi, metin kutusuna yerine bu sayı olarak görüntüler ve düğme, aşağıdaki çizimde gösterildiği gibi gönderin:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Bir görünümde oluşturma *Views\Course* güncelleştirme kurs KREDİLERİ sayfası için klasör:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

İçinde *Views\Course\UpdateCourseCredits.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Çalıştırma `UpdateCourseCredits` yöntemi seçerek **kursları** sekmesini, ardından ekleme "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğuna URL'yi (örneğin: `http://localhost:50205/Course/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Tıklayın **güncelleştirme**. Etkilenen satır sayısını görürsünüz:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Tıklayın **listesine geri** kredi düzeltilmiş sayısı kurslarıyla listesini görmek için.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Ham SQL sorguları hakkında daha fazla bilgi için bkz. [ham SQL sorguları](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) Entity Framework ekip blogunda.

## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Veritabanı bağlamında veritabanı satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak varlıkları bellekte veritabanında nedir ile eşitlenmiş durumda olup olmadığını izler. Bellekteki verileri bir önbellek olarak görev yapar ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve elden her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlık, varlığın yeniden kullanılmadan önce genellikle atıldı.

Bağlamı kullanarak bir sorgu için varlık nesnesi izler olup olmadığını belirtebilirsiniz `AsNoTracking` yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

- Sorgu, izlemeyi açma performansı fark edilir derecede geliştiren veri büyük birim alır.
- Bir varlığı güncelleştirmek için eklemek istediğiniz, ancak farklı bir amaç için aynı varlık daha önce aldığınız. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlığın eklenemiyor. Bunun gerçekleşmesini önlemek için tek bir yolu `AsNoTracking` önceki sorgu seçeneği.

Bu bölümde bu senaryolar ikinci gösteren iş mantığı uygulamanız. Özellikle, bir eğitmen birden fazla bölüm Yöneticisi olamaz bildiren bir iş kuralı zorlamak.

İçinde *DepartmentController.cs*, çağırmak yeni bir yöntem ekleyin `Edit` ve `Create` yöntemleri hiçbir iki bölüm aynı yönetici olduğundan emin olun:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Kodda `try` bloğu `HttpPost` `Edit` doğrulama hatası varsa, bu yeni yöntem çağrılacak yöntem. `try` Bloğu şimdi aşağıdaki örnekteki gibi görünür:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Departman düzenlemek çalıştırırsanız ve bir departmanın yönetici zaten farklı bir bölüm yöneticisi olan bir eğitmen değiştirmeyi deneyin. Beklenen hata iletisiyle karşılaşırsınız:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Departman Düzenle sayfasında yeniden ve bu saat değişikliği çalıştırmam **bütçe** tutar. Tıkladığınızda **Kaydet**, bir hata sayfası görürsünüz:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Özel durum hata iletisi "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Bu, aşağıdaki olaylar dizisi nedeniyle oluştu:

- `Edit` Yöntem çağrılarını `ValidateOneAdministratorAssignmentPerInstructor` yönetici olarak Kim Abercrombie sahip tüm Departmanlar alan yöntemi. Bu, okunacak İngilizce departman neden olur. Düzenlenmekte olan bölümü olduğundan, herhangi bir hata bildirilir. Bu okuma işlemi sonucunda, ancak veritabanından okundu İngilizce departmanı varlık artık veritabanı bağlamı tarafından izleniyor.
- `Edit` Yöntemi ayarlamak dener `Modified` İngilizce bayrağı departmanı varlık MVC model bağlayıcı tarafından oluşturulan, ancak bağlamı zaten İngilizce departman için bir varlık izleme olduğundan başarısız.

Bu sorunun bir çözümü bağlam doğrulama sorgu tarafından alınan bellek içi departmanı varlıkları izlemelerinin tutmaktır. Bu varlık güncelleştirilmesi gerekmez çünkü bu, yapılması veya yeniden bellekte önbelleğe alınmasını verilerden avantaj elde edecektir şekilde okuma hiçbir olumsuz yoktur.

İçinde *DepartmentController.cs*, `ValidateOneAdministratorAssignmentPerInstructor` yöntemi, izleme yok, aşağıda gösterildiği gibi belirtin:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Denemeniz düzenlemek için yineleyin **bütçe** departmanı miktarı. Bu süre işleminin başarılı olması ve bölümler dizin sayfasına yeniden düzenlenen Bütçe değeri gösteren, beklendiği gibi site döndürür.

## <a name="examining-queries-sent-to-the-database"></a>Veritabanına gönderilen sorgular İnceleme

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmeniz yararlıdır. Bunu yapmak için hata ayıklayıcı bir sorgu değişkeni incelemek veya sorgunun çağrı `ToString` yöntemi. Bunu denemek için basit bir sorgu arayın ve seçenekleri yükleniyor, filtreleme ve sıralama gibi eager ekledikçe ne olur ardından aramak.

İçinde *denetleyicileri/CourseController*, değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Bir kesme noktası kümesini şimdi *GenericRepository.cs* üzerinde `return query.ToList();` ve `return orderBy(query).ToList();` bilgilerinin `Get` yöntemi. Projeyi hata ayıklama modunda çalıştırın ve kursu dizin sayfasını seçin. Kodu kesme noktasına ulaşıldığında, inceleyin `query` değişkeni. SQL sunucusuna gönderilen sorgu görürsünüz. Basit bir `Select` deyimi:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Sorgular, Visual Studio'da hata ayıklama pencerelerinde görüntülemek için çok uzun olabilir. Sorgunun tamamını görmek için değişken değerini kopyalayın ve bir metin düzenleyicisine yapıştırın:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Artık kullanıcılar için belirli bir departmandaki filtre uygulayabilirsiniz böylece kurs dizin sayfasına açılır listede ekleyeceksiniz. Kursları başlığa göre sıralamak ve istekli yükleme için belirtirsiniz `Department` gezinme özelliği. İçinde *CourseController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Yöntem açılan listede seçili değerini alır `SelectedDepartment` parametresi. Hiçbir şey seçili değilse bu parametre null olacaktır.

A `SelectList` tüm bölümleri içeren bir koleksiyon için aşağı açılan liste görünümüne geçirilir. Geçirilen parametreleri `SelectList` oluşturucusu, seçili öğe değeri alan adı ve metin alan adı belirtin.

İçin `Get` yöntemi `Course` depo kodu belirtir bir filtre ifadesi bir sıralama düzeni ve için yükleme eager `Department` gezinme özelliği. Filtre ifadesi her zaman döndürür `true` hiçbir şey aşağı açılan listede seçili olup olmadığını (diğer bir deyişle, `SelectedDepartment` null).

İçinde *Views\Course\Index.cshtml*, açmadan önce hemen `table` etiketinde, aşağı açılan liste ve bir Gönder düğmesi oluşturmak için aşağıdaki kodu ekleyin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Hala ayarlanan kesme noktaları ile `GenericRepository` sınıfı, kurs dizin sayfası çalıştırın. Sayfanın tarayıcıda görüntülenir, böylece kod bir kesme noktası isabet ilk iki kez devam edin. Aşağı açılan listeden bir bölüm seçin ve tıklayın **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Bu süre, aşağı açılan liste için bölümler sorgu için ilk kesme noktasına olacaktır. Atlama ve görüntüleme `query` değişken sonraki kodu ulaştığında kesme noktası gördükleri için `Course` gibi görünüyor artık sorgu. Aşağıdaki gibi bir şey görürsünüz:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Sorgu şimdi olduğunu görebilirsiniz bir `JOIN` yükler sorgu `Department` ile birlikte veri `Course` veri ve onu içeren bir `WHERE` yan tümcesi.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Proxy sınıfları ile çalışma

Entity Framework varlık örnekleri (örneğin, bir sorgu yürütme), genellikle olarak varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilmiş türün örneklerini oluşturur. Bu proxy özelliği erişildiğinde eylemlerini otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar. Örneğin, bu mekanizma, ilişkilerin yavaş yükleniyor desteklemek için kullanılır.

Çoğu zaman bu proxy'ler kullanımına dikkat etmeniz gerekmez, ancak özel durum vardır:

- Bazı senaryolarda Entity Framework proxy örnekleri oluşturmasını isteyebilirsiniz. Örneğin, olmayan proxy'si örneklerini serileştirmek proxy örneklerini serileştirmek değerinden daha verimli olabilir.
- Ne zaman örneği kullanarak bir varlık sınıfı `new` işleci, bir proxy örneği Al yok. Başka bir deyişle, yavaş yükleniyor ve otomatik değişiklik izleme gibi işlevselliği elde etmezsiniz. Bu genellikle Tamam, veritabanında olmayan yeni bir varlık oluşturduğumuzdan, genellikle yavaş yükleniyor, ihtiyacınız olmayan ve genellikle açıkça bir varlık olarak işaretleme durumunda değişiklik izleme gereksiniminiz yoksa `Added`. Ancak, yavaş yüklenmesi gerekir ve değişiklik izleme ihtiyacınız varsa, yeni varlık örneklerini kullanarak proxy'leriyle oluşturabileceğiniz `Create` yöntemi `DbSet` sınıfı.
- Proxy türünden bir gerçek varlık türünü almak isteyebilirsiniz. Kullanabileceğiniz `GetObjectType` yöntemi `ObjectContext` bir proxy tür örneği gerçek varlık türünü almak için sınıf.

Daha fazla bilgi için [proxy ile çalışmayı](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) Entity Framework ekip blogunda.

## <a name="disabling-automatic-detection-of-changes"></a>Değişiklikleri otomatik olarak algılanmasını devre dışı bırakma

Entity Framework, bir varlığın geçerli değerleri özgün değerlerle karşılaştırarak, varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) belirler. Varlık sorgulanan ya da bağlı orijinal değerleri depolanır. Otomatik değiştirme algılama neden yöntemlerden bazıları aşağıda verilmiştir:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Aşağıdaki yöntemlerden birini bir döngüde birçok kez çağırmak ve çok sayıda varlık izliyorsunuz, önemli performans iyileştirmeleri otomatik değişiklik algılama kullanarak geçici olarak kapatarak alabilirsiniz [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) özelliği. Daha fazla bilgi için [değişiklikleri otomatik olarak algılama](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Değişiklikler kaydedilirken doğrulama devre dışı bırakma

Çağırdığınızda `SaveChanges` yöntemi, varsayılan olarak Entity Framework doğrular değiştirilen tüm varlıkların tüm özelliklerini verileri veritabanını güncelleştirmeden önce. Çok sayıda varlık güncelleştirdik ve bu gereksiz çalışmadır zaten veri doğruladınız işlemini yapabilir değişiklikleri geçici olarak devre dışı doğrulama kapatarak daha az zaman alır. Bu kullanarak yapabileceğiniz [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) özelliği. Daha fazla bilgi için [doğrulama](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Özet

Bu, Bu öğretici serisinde, Entity Framework kullanarak bir ASP.NET MVC uygulamasındaki tamamlar. Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

Bunu oluşturduktan sonra web uygulamanızı dağıtma hakkında daha fazla bilgi için bkz. [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx) MSDN Kitaplığı'nda.

MVC için kimlik doğrulaması ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz. [MVC önerilen kaynakları](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>İlgili kaynaklar

- Tom Dykstra Bu öğreticinin özgün sürümle yazdı ve Microsoft Web Platformu ve araçları içerik ekibi üst düzey bir programlama yazıcısı.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) Bu öğretici yazarlarından ve EF 5 ve MVC 4 için güncelleştirme çoğunu vermedi. Rick bir Microsoft Azure ve MVC odaklanmak için üst düzey programlama yazardır.
- [Rowan Miller](http://www.romiller.com) ve diğer Entity Framework takım üyeleri kod incelemeleriyle Yardımlı ve biz EF 5 için öğreticiyi güncelleştirmekte olduğunuz, çıkan geçişleri ile birçok sorunlarında hata ayıklama yardımcı olmuştur.

## <a name="vb"></a>VB

Öğreticinin ilk olarak üretilen, biz tamamlanan indirme proje hem C# ve VB sürümleri sağlanır. Bu güncelleştirme ile C# indirilebilir projesine serisi, ancak zaman sınırlaması ve biz, vb için yapmadınız çalışanlarınızın diğer önceliklere nedeniyle herhangi bir yere başlama daha kolay hale getirmek her bölüm için sağlıyoruz Varsa bu öğreticileri kullanarak bir VB projesi oluşturun ve başkalarıyla paylaşmak için lütfen bize bildirin iradeye sahip olması.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Hatalar ve çözümleri

### <a name="cannot-createshadow-copy"></a>Oluşturma/kopyalama gölge olamaz

Hata iletisi:

*Oluşturma/kopyalama 'DotNetOpenAuth.OpenId' zaten mevcut olduğunda gölge olamaz.*

Çözüm:

Birkaç saniye bekleyin ve sayfayı yenileyin.

### <a name="update-database-not-recognized"></a>Veritabanını Güncelleştir tanınmıyor

Hata iletisi:

*' % S'terim 'Veritabanını Güncelleştir' cmdlet'i, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol varsa, yolun doğru olduğundan emin olun ve yeniden deneyin.* (Gelen *`Update-Database`* PMC'yi komutunu.)

Çözüm:

Visual Studio'dan çıkın. Projeyi yeniden açıp yeniden deneyin.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Hata iletisi:

*Bir veya daha fazla varlık için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.* (Gelen *`Update-Database`* PMC'yi komutunu.)

Çözüm:

Bu sorunun bir nedeni olduğundan doğrulama hataları olduğunda `Seed` yöntemi çalışır. Bkz: [Seeding ve hata ayıklama Entity Framework (EF) Db'ler](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) hata ayıklama ipuçları için `Seed` yöntemi.

### <a name="http-50019-error"></a>500.19 HTTP hatası

Hata iletisi:

*HTTP Hatası 500.19 - iç sunucu hatası istenen sayfa sayfa için ilgili yapılandırma verileri geçersiz olduğundan erişilemez.*

Çözüm:

Bu hatayı alabileceğiniz bir çözüm, her biri aynı bağlantı noktası numarası kullanarak bunları birden çok kopyasını kalmamasını yoludur. Ayrıca, Visual Studio'nun tüm örneklerini çıkıldıktan sonra üzerinde çalıştığınız projeyi yeniden genellikle bu sorunu çözebilirsiniz. Bu işe yaramazsa, bağlantı noktası numarasını değiştirmeyi deneyin. Proje dosyası üzerinde sağ tıklayın ve ardından Özellikler seçeneğine tıklayın. Seçin **Web** sekmesini ve sonra bağlantı noktası numarasını değiştirmelisiniz **proje URL'si** metin kutusu.

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

*Bir SQL Server bağlantısı kurulurken ağla ilgili veya örneğe özel bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığını doğrulayın. (sağlayıcı: SQL ağ arabirimleri, hata: 26 - Server/örneği belirtilen hata bulma)*

Çözüm:

Bağlantı dizesini kontrol edin. El ile veritabanını sildiyseniz, yapım dizesinde veritabanının adını değiştirin.

> [!div class="step-by-step"]
> [Önceki](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [İleri](building-the-ef5-mvc4-chapter-downloads.md)
