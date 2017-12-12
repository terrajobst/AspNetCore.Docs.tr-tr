---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 2 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 4e2a3176aaedccd40ef6b619efa3c4052dd8470b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 2
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource denetimi

Önceki öğreticide bir web sitesi, bir veritabanı ve veri modeli oluşturdu. Bu öğreticide çalıştığınız `EntityDataSource` kolaylaştıran bir Entity Framework veri modeli ile çalışmak için ASP.NET sağlayan denetimi. Oluşturacağınız bir `GridView` görüntülemek ve Öğrenci verileri düzenleme denetimi bir `DetailsView` yeni Öğrenciler eklemek için Denetim ve `DropDownList` (hangi ilişkili kurslar görüntülemek için daha sonra kullanacağız) departman seçmek için denetim.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Bu uygulamada, giriş doğrulaması veritabanını güncelleştiren sayfalara ekleme olmaz ve hata işleme bazıları bir üretim uygulamasında gerekli olacak şekilde sağlam olmaz unutmayın. Entity Framework üzerine odaklanan öğretici tutar ve uzun bulaşmasından tutar. Bu özellikler, uygulamanıza ekleme hakkında daha fazla bilgi için bkz [kullanıcı girişini doğrulama ASP.NET Web Pages'de](https://msdn.microsoft.com/en-us/library/7kh55542.aspx) ve [hata işleme ASP.NET sayfaları ve uygulamaları](https://msdn.microsoft.com/en-us/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Ekleme ve EntityDataSource denetimi yapılandırma

Yapılandırarak başlarsınız bir `EntityDataSource` okumak için Denetim `Person` varlıklardan `People` varlık kümesi.

Visual Studio açın sahip olduğundan emin olun ve proje ile çalışıyorsanız, bölüm 1 oluşturduğunuz. Veri modeli oluşturulan ya da son değişiklik yaptığınız bu yana projenin oluşturduysanız henüz projeyi şimdi oluşturun. Proje oluşturulana kadar değişiklikleri veri modeline Designer'a kullanılabilir duruma getirilmez.

Kullanarak yeni bir web sayfası oluşturmak **Web ana sayfa kullanarak Form** şablonu ve adlandırın *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Belirtin *Site.Master* ana sayfa olarak. Bu öğretici için oluşturduğunuz tüm sayfalar bu ana sayfa kullanır.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

İçinde **kaynak** görüntülemek için add bir `h2` için başlık `Content` adlı Denetim `Content2`, aşağıdaki örnekte gösterildiği gibi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Gelen **veri** sekmesinde **araç**, sürükleyin bir `EntityDataSource` denetlemek için sayfası, başlığı altındaki bırakın ve Kimliğine değiştirmek `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Geçiş **tasarım** görüntülemek, veri kaynağı kontrolünün akıllı etiket'ı tıklatın ve ardından **Configure Data Source** başlatmak için **Configure Data Source** Sihirbazı.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

İçinde **yapılandırma ObjectContext** Sihirbaz adımı, select **SchoolEntities** değeri olarak **adlı bağlantı**seçip **SchoolEntities**olarak **DefaultContainerName** değeri. Sonra **İleri**'ye tıklayın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Not: aşağıdaki iletişim kutusunu bu noktada alırsanız, devam etmeden önce projeyi derlemek sahipsiniz.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

İçinde **veri seçimini Yapılandır** adım, select **kişiler** değeri olarak **EntitySetName**. Altında **seçin**, emin olun **seçin A** üm onay kutusu seçilidir. Daha sonra güncelleştirmeyi etkinleştirmek ve silmek için seçenekleri seçin. İşiniz bittiğinde tıklatın **son**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Silme işlemi izin vermek için veritabanı kurallarını yapılandırma

Öğrenciler gelen silme kullanıcıların olanak sağlayan bir sayfa oluşturma `Person` üç ilişkilerine sahip diğer tablolarla tablosu (`Course`, `StudentGrade`, ve `OfficeAssignment`). Varsayılan olarak, veritabanı, bir satırda silmesini engeller `Person` diğer tablolarla biriyle ilgili satır yoksa. İlişkili satırları önce el ile silin veya veritabanı sildiğinizde otomatik olarak silmek üzere yapılandırabilirsiniz bir `Person` satır. Bu öğreticide Öğrenci kayıtları için ilgili verileri otomatik olarak silmesini veritabanına yapılandıracaksınız. İlişkili satırları Öğrenciler çünkü yalnızca `StudentGrade` tablo, gereken üç ilişkiler yalnızca birini yapılandırın.

Kullanıyorsanız, *School.mdf* Bu öğretici ile gider projeden indirilen dosya, bu yapılandırma değişikliklerini yaptıktan zaten olduğundan, bu bölümü atlayabilirsiniz. Bir komut dosyası çalıştırarak veritabanını oluşturduysanız, aşağıdaki yordamları izleyerek veritabanını yapılandırın.

İçinde **Sunucu Gezgini**, bölüm 1 oluşturulan veritabanı diyagramı açın. Arasındaki ilişkiyi sağ `Person` ve `StudentGrade` (tablolar arasında satır) ve ardından **özellikleri**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

İçinde **özellikleri** penceresinde genişletin **ekleme ve güncelleştirme belirtimi** ve **DeleteRule** özelliğine **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Kaydet ve Kapat diyagramı. Veritabanını güncelleştirmek isteyip tıklatın sorulursa **Evet**.

Model veritabanı yapmakta olduğu ile eşitlenmiş bellekte olan varlıkları tutar emin olmak için veri modelinde karşılık gelen kuralları ayarlamanız gerekir. Açık *SchoolModel.edmx*, arasındaki ilişkiyi satıra sağ `Person` ve `StudentGrade`ve ardından **özellikleri**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

İçinde **özellikleri** penceresindeki ayarlayın **End1 OnDelete** için **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Kaydet ve Kapat *SchoolModel.edmx* dosya ve projeyi yeniden derleyin.

Genel olarak, veritabanı değiştiğinde nasıl modelini eşitlemek birkaç seçeneğiniz vardır:

- Tür (örneğin, "ekleme veya yenileme tabloları, görünümleri ve saklı yordamlar), değişiklikleri belirli sağ Tasarımcısı ve select **güncelleştirme modeli veritabanından** Tasarımcı marka değişiklikleri otomatik olarak sağlamak için.
- Veri modeli yeniden oluşturun.
- Bunun gibi el ile güncelleştirmeleri yapın.

Bu durumda, model yeniden veya ilişki değişiklikten etkilenen tabloları yenilenir, ancak ardından alan adı değişikliği yeniden yapmanız gerekir (gelen `FirstName` için `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Okuma ve varlıkları güncelleştirmek için bir GridView denetimi kullanma

Bu bölümde kullanacağınız bir `GridView` görüntülemek, güncelleştirmek veya Öğrenciler silmek için denetim.

Açın veya geçin *Students.aspx* ve geçiş **tasarım** görünümü. Gelen **veri** sekmesinde **araç**, sürükleyin bir `GridView` sağındaki denetim `EntityDataSource` denetlemek, adlandırın `StudentsGridView`akıllı etiket'ı tıklatın ve ardından  **StudentsEntityDataSource** veri kaynağı olarak.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

' I tıklatın **yenileme şema** ('ı tıklatın **Evet** onaylamanız istenirse), ardından **etkinleştirmek disk belleği**, **etkinleştirmek sıralama**, **Düzenlemeyi etkinleştir**, ve **silmeyi etkinleştir**.

Tıklatın **Düzenle sütunları**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

İçinde **seçili alanları** kutusunda, silmek **PersonID**, **LastName**, ve **İşeAlmaTarihi**. Kullanıcılara kayıt anahtarı genellikle gösterme, işe alma tarihi Öğrenciler için ilgili değildir ve yalnızca ad alanlarından birini gerekir böylece adı kısımlarını bir alana gireceğiniz.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Seçin **FirstMidName** alan ve ardından **TemplateField'a Bu alan Dönüştür**.

Aynı işlemi gerçekleştirmek **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Tıklatın **Tamam** ve o **kaynak** görünümü. Kalan değişikliklerini doğrudan biçimlendirme yapmak daha kolay olacaktır. `GridView` Denetim biçimlendirme şimdi aşağıdaki örnekte görülüyor.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Komut alan bir şablon şu anda alanıdır sonra ilk sütun adı görüntüler. Aşağıdaki örnekteki gibi aramak bu şablonu alan için biçimlendirme değiştirin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

Görüntü modunda, iki `Label` denetimleri görünen ilk ve son adı. Ad ve Soyadı değiştirebilmeniz için düzenleme modunda iki metin kutusu sağlanır. İle `Label` denetimlerde görüntüleme modunu, kullandığınız `Bind` ve `Eval` ifadeleri tam olarak misiniz doğrudan veritabanlarına bağlanmak ASP.NET veri kaynağı denetimleri ile. Varlık özellikleri veritabanı sütunlarını yerine belirlediniz yalnızca farktır.

Son sütun kayıt tarihi görüntüler şablonu bir alandır. Aşağıdaki örnekteki gibi aramak Bu alan için biçimlendirme değiştirin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Hem de görüntüleme ve düzenleme moduna biçim dizesi "{0, d}" "kısa tarih" biçiminde görüntülenecek tarihi neden olur. (Bilgisayarınız bu biçimden farklı Bu öğreticide gösterilen ekran görüntüleri görüntülemek için yapılandırılabilir.)

Alanların her birinde Bu şablon, Tasarımcı kullanılan fark bir `Bind` varsayılan, ancak ifadeyle olarak değiştirdiniz bir `Eval` ifadesinde `ItemTemplate` öğeleri. `Bind` İfade verileri kullanılabilir kılar `GridView` kod verilere erişim gerekebileceği özellikleri denetim. Bu sayfada kullanabilmek için bu verilere kodda gerekmez `Eval`, daha verimli olduğu. Daha fazla bilgi için bkz: [verilerinizi veri denetimleri dışında alma](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Performansı artırmak için düzeltilmesi EntityDataSource denetimini biçimlendirme

Biçimlendirme için `EntityDataSource` denetlemek, Kaldır `ConnectionString` ve `DefaultContainerName` öznitelikleri ve bunlarla Değiştir bir `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. Bu her oluşturduğunuzda olmalısınız farklıdır bir `EntityDataSource` nesne bağlamı sınıfında sabit kodlanmış olandan farklı bir bağlantı kullanmak gerekli olmadıkça denetim. Kullanarak `ContextTypeName` özniteliği, aşağıdaki avantajları sağlar:

- Daha iyi performans. Zaman `EntityDataSource` denetim başlatan veri modelini kullanarak `ConnectionString` ve `DefaultContainerName` öznitelikleri, her istekte meta veri yüklemek için başka çalışma gerçekleştirir. Bu belirtirseniz gerekli değildir `ContextTypeName` özniteliği.
- Yavaş yükleniyor açık varsayılan olarak oluşturulan nesne bağlamı sınıfları (gibi `SchoolEntities` bu öğreticideki) Entity Framework 4. 0'ı. Bu, sağ gerektiğinde gezinti özellikleri ile ilgili veriler otomatik olarak yüklenmesini anlamına gelir. Yavaş yükleniyor Bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak açıklanmıştır.
- Nesne bağlamı sınıfına uyguladığınız özelleştirmeler (Bu durumda, `SchoolEntities` sınıfı) kullanan denetimler için kullanılabilir olacak `EntityDataSource` denetim. Nesne bağlamı sınıfı özelleştirme Bu öğretici serisinde kapsanmayan Gelişmiş bir konudur. Daha fazla bilgi için bkz: [Entity Framework oluşturulan türleri genişletme](https://msdn.microsoft.com/en-us/library/dd456844.aspx).

İşaretleme şimdi (özelliklerin sırasını farklı olabilir) aşağıdaki örnek benzeyecektir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Yabancı anahtar sütunları varlık özellikleri ortaya değil çünkü Entity Framework'ün önceki sürümlerde gerekli bir özellik için öznitelik başvuruyor. Geçerli sürüm kullanmayı mümkün kılar *yabancı anahtar ilişkileri*, yabancı anahtar özellikleri başka bir deyişle, tüm ancak çok-çok ilişkileri sunulur. Varlıklarınızı yabancı anahtar özellikleri ve no varsa [karmaşık türler](https://msdn.microsoft.com/en-us/library/bb738472.aspx), bu öznitelik ayarlanırsa bırakabilirsiniz `False`. Varsayılan değer olduğundan, öznitelik biçimlendirmeden kaldırmayın `True`. Daha fazla bilgi için bkz: [düzleştirme nesneleri (EntityDataSource)](https://msdn.microsoft.com/en-us/library/ee404746.aspx).

Sayfayı çalıştırın ve öğrenciler ve Çalışanlar (yalnızca Öğrenciler için sonraki öğreticide filtrelemenize) listesini görürsünüz. Ad ve Soyadı birlikte görüntülenir.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Görüntü sıralamak için sütun adını tıklatın.

Tıklatın **Düzenle** herhangi bir satır içinde. Metin kutuları adı ve Soyadı değiştirebileceğiniz görüntülenir.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Silmek** düğmesini de çalışır. Öğesini silmek için bir kayıt tarihi olan bir satır ve satır kaldırılır. (Bir kayıt tarihi olmayan sıralar Eğitmen temsil eder ve bir başvuru bütünlüğü hatası alabilirsiniz. Sonraki öğreticide, yalnızca Öğrenciler dahil etmek için bu listeyi filtre.)

## <a name="displaying-data-from-a-navigation-property"></a>Bir gezinti özelliğinden verileri görüntüleme

Şimdi kaç kurslar bilmek ister varsayalım her Öğrenci olarak kaydedilir. Entity Framework sağlar, bu bilgileri `StudentGrades` Gezinti özelliğinin `Person` varlık. Veritabanı tasarım atanmış bir düzeyde gerek kalmadan bir indirmelere kaydedilecek öğrencinin izin vermediğinden bu öğretici için bu bir satır elde etmeyle kabul edilebilir `StudentGrade` indirmelere ile ilişkili bir tablo satırının aynıdır indirmelere kaydediliyor. ( `Courses` Gezinti özelliği olduğundan yalnızca eğitmen için.)

Kullandığınızda `ContextTypeName` özniteliği `EntityDataSource` denetim, Entity Framework otomatik olarak alır bilgi bir gezinti özelliği için bu özelliğe erişim. Bu adlı *yavaş Yükleniyor*. Her zaman ek bilgiler gerekli veritabanı için ayrı bir çağrısı sonuçlandığından ancak, Bu Verimsiz, olabilir. Tarafından döndürülen her varlık için bir gezinti özelliği verilerden gerekip gerekmediğini `EntityDataSource` denetim, bu veritabanı için tek bir çağrı varlık kendisi ile birlikte ilgili verileri daha verimli. Bu adlı *istekli yükleme*, ve ayarlayarak istekli Yükleme özelliği bir gezinti özelliği için belirttiğiniz `Include` özelliği `EntityDataSource` denetim.

İçinde *Students.aspx*kurslar sayısı için her Öğrenci göstermek istediğiniz, bu nedenle istekli yüklenirken en iyi seçimdir. Tüm Öğrenciler görüntüleme ancak kurslar sayısını gösteren yalnızca birkaç tanesi (hangi biçimlendirme yanı sıra biraz kod yazmaya gerekir), için geç yükleme daha iyi bir seçim olabilir.

Açın veya geçin *Students.aspx*, geçiş **tasarım** görünümü, select `StudentsEntityDataSource`ve **özellikleri** penceresi kümesi **INCLUDE**özelliğine **StudentGrades**. (Birden çok gezinti özellikleri almak istiyorsanız, virgülle ayrılmış adlarını belirtebilirsiniz; Örneğin, **StudentGrades, kurslar**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Geçiş **kaynak** görünümü. İçinde `StudentsGridView` son sonra Denetim `asp:TemplateField` öğesi, aşağıdaki yeni şablonu alanını ekleyin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

İçinde `Eval` ifadesi, gezinti özelliğinin başvuru `StudentGrades`. Bu özellik bir koleksiyon içerdiğinden sahip bir `Count` içinde Öğrenci kayıtlı kurslar sayısını görüntülemek için kullanabileceğiniz özelliği. Sonraki öğreticide koleksiyonları yerine tek varlıkları içeren Gezinti özellikleri verileri görüntülemek nasıl görürsünüz. (Kullanamazsınız Not `BoundField` öğeleri gezinti özellikleri verileri görüntülemek için.)

Sayfayı çalıştırın ve şimdi Öğrenci kaç kurslar kayıtlı görürsünüz.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Varlıkları eklemek için DetailsView denetimini kullanma

Bir sayfa oluşturmak için sonraki adımdır bir `DetailsView` yeni Öğrenciler eklemenize olanak sağlayacak denetim. Tarayıcıyı kapatın ve kullanarak yeni bir web sayfası oluşturmak *Site.Master* ana sayfa. Sayfa adı *StudentsAdd.aspx*ve ardından geçmek **kaynak** görünümü.

İçin varolan biçimlendirmesini değiştirmek için aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` , oluşturduğunuz bir benzer denetim *Students.aspx*, ekleme etkinleştirir dışında. Olduğu gibi `GridView` denetlemek, ilişkili alanlarını `DetailsView` denetim kodlanmış varlık özellikleri başvuru dışında tam olarak, bir veritabanına doğrudan bağlanan bir veri denetimi için olur. Bu durumda, `DetailsView` denetimi için varsayılan modu ayarladığınız şekilde yalnızca satır eklemek için kullanılan `Insert`.

Sayfayı çalıştırın ve yeni Öğrenci ekleyin.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Hiçbir şey yeni Öğrenci ekledikten sonra ancak şimdi çalıştırırsanız olacağını *Students.aspx*, yeni Öğrenci bilgileri görürsünüz.

## <a name="displaying-data-in-a-drop-down-list"></a>Aşağı açılan listesinde verileri görüntüleme

Aşağıdaki adımlarda databind gerekir bir `DropDownList` denetim kullanılarak ayarlanan bir varlığa bir `EntityDataSource` denetim. Öğreticinin bu bölümünde, bu liste ile olmaz. Sonraki bölümlerinde yine de departmanı ile ilişkili kurslar görüntülemek için bir bölüm seçin kullanıcıların izin vermek için listenin kullanacaksınız.

Adlı yeni bir web sayfası oluşturun *Courses.aspx*. İçinde **kaynak** görüntülemek için bir başlığı eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

İçinde **tasarım** görüntülemek için add bir `EntityDataSource` denetimi sayfasına daha önce yaptığınız gibi dışında bu zaman adlandırın, `DepartmentsEntityDataSource`. Seçin **Departmanlar** olarak **EntitySetName** değeri ve yalnızca seçin **DepartmentID** ve **adı** özellikleri.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Gelen **standart** sekmesinde **araç**, sürükleyin bir `DropDownList` denetiminde sayfasına, adlandırın `DepartmentsDropDownList`, akıllı etiket tıklatın ve seçin **veri kaynağı Seç** için Başlat **veri kaynağı Yapılandırma Sihirbazı'nı**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

İçinde **veri kaynağı seçin** adım, select **DepartmentsEntityDataSource** veri kaynağı olarak tıklatın **yenileme şema**ve ardından **adı** olarak görüntülenecek veri alan ve **DepartmentID** değeri veri alanı olarak. **Tamam**'ı tıklatın.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Diğer ASP.NET veri kaynağı denetimleri dışında varlıkları ve varlık özellikleri belirlediniz Entity Framework kullanarak denetim databind için kullandığınız yöntem aynıdır.

Geçiş **kaynak** görüntülemek ve eklemek "bir bölüm seçin:" hemen önce `DropDownList` denetim.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

İçin biçimlendirme bir anımsatıcı değiştirmek `EntityDataSource` değiştirerek bu noktada denetim `ConnectionString` ve `DefaultContainerName` ile öznitelikleri bir `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. Genellikle kadar değiştirmeden önce veri kaynağı denetime bağlı veri bağlama denetimi oluşturduktan sonra beklenecek en iyisidir `EntityDataSource` değişikliği yaptıktan sonra Tasarımcı sizinle sağlamadığı için biçimlendirme, denetim bir **Yenile Şema** veri bağlama denetimi seçeneği.

Sayfayı çalıştırın ve bir bölüm aşağı açılan listeden seçebilirsiniz.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Bu kullanmaya giriş tamamlar `EntityDataSource` denetim. Varlıklar ve Özellikler tablolar ve sütunlar yerine başvuru dışında bu denetimi ile çalışma kaynağı denetimleri, diğer ASP.NET verilerle çalışma genellikle farklı değildir. Gezinti özellikleri erişmek istediğinizde tek özel durumdur. Sonraki öğreticide sözdizimi ile kullanmanız görürsünüz `EntityDataSource` denetim ayrıca farklı diğer veri kaynağı denetimlerden filtre, Grup ve sipariş veri.

>[!div class="step-by-step"]
[Önceki](the-entity-framework-and-aspnet-getting-started-part-1.md)
[sonraki](the-entity-framework-and-aspnet-getting-started-part-3.md)
