---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 2. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: c43e7b9d090b0e25fe1db1ce6a944afea4b081d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802643"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 2. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource denetimi

Önceki öğreticide bir veri modeli bir web sitesi ve bir veritabanı oluşturdunuz. Bu öğreticide çalıştığınız `EntityDataSource` bir Entity Framework veri modeli ile çalışmak üzere kolaylaştırmak için ASP.NET sağlayan denetimi. Oluşturacağınız bir `GridView` görüntülemek ve Öğrenci verileri düzenleme denetimi bir `DetailsView` yeni Öğrenci eklemek için Denetim ve `DropDownList` (Bu, ilgili kurslar görüntülemek için daha sonra kullanacağınız) bir bölümü seçmek için denetim.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Bu uygulamada, giriş doğrulaması veritabanını güncelleştiren sayfalara ekleme gerekmez ve bazı hata işleme bir üretim uygulamasında gerekli olacak kadar güçlü olmayacaktır unutmayın. Entity Framework'ü odaklanan bir öğretici tutar ve uzun alma tutar. Bu özellikler, uygulamanıza ekleme hakkında daha fazla bilgi için bkz. [kullanıcı girişini doğrulama ASP.NET Web Pages'de](https://msdn.microsoft.com/library/7kh55542.aspx) ve [hata işleme ASP.NET sayfaları ve uygulamalarında](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Ekleme ve EntityDataSource denetimi yapılandırma

Yapılandırarak başlarsınız bir `EntityDataSource` okunacak denetim `Person` varlıklardan `People` varlık kümesi.

Visual Studio'nun sahip olduğundan emin olun ve proje ile çalışıyorsanız, 1. bölümünde oluşturduğunuz. Son değişiklik yaptığınız veya veri modelleri oluşturduğunuz proje oluşturulan yapmadıysanız, projeyi şimdi derleyin. Proje oluşturulana kadar değişiklikleri veri modeline tasarımcıya kullanılabilir duruma getirilmez.

Kullanarak yeni bir web sayfası oluşturma **ana sayfa kullanan Web formu** şablonunu ve adlandırın *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Belirtin *Site.Master* ana sayfa olarak. Bu öğreticiler için oluşturduğunuz tüm sayfalar, bu ana sayfanın kullanır.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

İçinde **kaynak** görüntüleme, ekleme bir `h2` için başlık `Content` adlı Denetim `Content2`aşağıdaki örnekte gösterildiği gibi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Gelen **veri** sekmesinde **araç kutusu**, sürükleyin bir `EntityDataSource` denetlemek için sayfanın altındaki başlığı bırakma ve Kimliğine değiştirme `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Geçiş **tasarım** görüntülemek, veri kaynağı denetimin akıllı etiket tıklayın ve ardından **veri kaynağı yapılandırma** başlatmak için **veri kaynağı yapılandırma** Sihirbazı.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

İçinde **yapılandırma ObjectContext** seçme Sihirbazı adımı **SchoolEntities** değeri olarak **adlı bağlantı**seçip **SchoolEntities**olarak **DefaultContainerName** değeri. Sonra **İleri**'ye tıklayın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Not: Bu aşamada aşağıdaki iletişim kutusu alırsanız, devam etmeden önce projeyi derlemek sahip.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

İçinde **yapılandırma veri seçimi** adım, select **kişiler** değeri olarak **EntitySetName**. Altında **seçin**, emin **seçin bir** ll onay kutusu seçilidir. Ardından Update hizmetini etkinleştirmek ve silmek için seçenekleri seçin. İşiniz bittiğinde tıklayın **son**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Silinmesine izin vermek için veritabanı kuralları yapılandırma

Kullanıcıların öğrencilerden Sil olanak sağlayan bir sayfa oluşturma `Person` diğer tablolarla üç ilişki olan tablo (`Course`, `StudentGrade`, ve `OfficeAssignment`). Varsayılan olarak, veritabanı, bir satırda silmesini engeller `Person` başka tablolardan birinde ilişkili satırları varsa. İlişkili satırları önce el ile silin veya veritabanı sildiğinizde otomatik olarak silmek üzere yapılandırabileceğiniz bir `Person` satır. Bu öğreticide Öğrenci kayıtları için ilgili verileri otomatik olarak silmek için veritabanı yapılandıracaksınız. İlişkili satırları Öğrenciler çünkü yalnızca `StudentGrade` tablo, üç ilişki yalnızca biri yapılandırmanız gerekir.

Kullanıyorsanız *School.mdf* bu öğreticiyle giden projesinden indirilen dosya, bu yapılandırma değişikliklerini yaptıktan zaten olduğundan, bu bölümü atlayabilirsiniz. Bir betik çalıştırarak veritabanı oluşturduysanız, aşağıdaki yordamlar gerçekleştirilerek veritabanını yapılandırın.

İçinde **Sunucu Gezgini**, 1. bölümünde oluşturduğunuz veritabanı diyagramı açın. Arasındaki ilişkiyi sağ `Person` ve `StudentGrade` (tablolar arasındaki çizgi) ve ardından **özellikleri**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

İçinde **özellikleri** penceresini genişletin **INSERT ve UPDATE tarifi** ve **DeleteRule** özelliğini **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Kaydet ve diyagramı kapatın. Veritabanını güncelleştirmek isteyip istemediğinizi tıklayın istenirse **Evet**.

Modelin bellek veritabanına yaparsanız ile eşitlenmiş olan varlıklar tutar emin olmak için karşılık gelen kuralları veri modelindeki ayarlamanız gerekir. Açık *SchoolModel.edmx*, arasındaki ilişkilendirme çizgisi sağ `Person` ve `StudentGrade`ve ardından **özellikleri**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

İçinde **özellikleri** penceresinde **End1 OnDelete** için **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Kaydet ve Kapat *SchoolModel.edmx* dosyasını bulun ve ardından projeyi yeniden derleyin.

Genel olarak, veritabanı değiştiğinde modelini eşitleme için birkaç seçeneğiniz vardır:

- Belirli türde değişiklikler (örneğin, "ekleyerek veya yenileme tablolar, görünümler ve saklı yordamlar), sağ tıklayın tasarımcı ve Seç ' **veritabanından bir güncelleştirme modeli** Tasarımcı marka değişiklikleri otomatik olarak sağlamak için.
- Veri modeli yeniden oluşturun.
- Bunun gibi el ile güncelleştirmeleri yapın.

Bu durumda, modeli yeniden ya da ilişki değişiklikten etkilenen tablolar yenilenir ancak ardından tekrar alan adı değişikliği yapmanız gerekir (gelen `FirstName` için `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Bir GridView denetimi kullanarak okuyun ve varlıklarını güncelleştirme

Bu bölümde kullanacaksınız bir `GridView` görüntülemek, güncelleştirmek veya silmek Öğrenciler için denetimi.

Açın veya geçin *Students.aspx* geçin **tasarım** görünümü. Gelen **veri** sekmesinde **araç kutusu**, sürükleyin bir `GridView` denetim sağındaki `EntityDataSource` denetlemek, adlandırın `StudentsGridView`akıllı etiket tıklayın ve ardından  **StudentsEntityDataSource** veri kaynağı olarak.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Tıklayın **şemasını Yenile** (tıklayın **Evet** onaylamanız istenirse), ardından **etkinleştirme disk belleği**, **etkinleştirme sıralama**, **Düzenlemeyi etkinleştir**, ve **silmeyi etkinleştir**.

Tıklayın **sütunları Düzenle**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

İçinde **seçili alanlar** kutusunda, silmek **Personıd**, **LastName**, ve **İşeAlmaTarihi**. Genellikle bir kayıt anahtarı kullanıcılara gösterme, işe alım tarihi Öğrenciler için uygun değil ve böylece, yalnızca ad alanı adı kısımlarını bir alana giriyorum.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Seçin **FirstMidName** alan ve ardından **bu alanı bir TemplateField dönüştürün**.

İçin de aynısını yapın **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Tıklayın **Tamam** dönersiniz **kaynak** görünümü. Kalan değişiklikleri doğrudan işaretlemede yapmak daha kolay olacaktır. `GridView` Denetimi aşağıdaki örnekte olduğu gibi biçimlendirme artık görünür.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Komut alan bir şablon şu anda alanıdır sonra ilk sütun adı görüntüler. Aşağıdaki örnekteki gibi aramak bu şablonu alan için biçimlendirmeyi Değiştir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

Görüntüleme modunda iki `Label` denetimler adı ve Soyadı görüntüler. Adı ve Soyadı değiştirebilmeniz için düzenleme modunda iki metin kutuları sağlanır. Olduğu gibi `Label` denetimlerde görüntüleme modu, kullandığınız `Bind` ve `Eval` ifadeleri tam olarak istediğiniz veritabanına doğrudan bağlanmak veri kaynağı denetimleri ASP.NET ile. Tek fark, varlık özelliklerini veritabanı sütunlarını yerine belirlediniz.

Son sütun kayıt tarihi gösteren bir şablonu alandır. Aşağıdaki örnekteki gibi aramak Bu alan için biçimlendirmeyi Değiştir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Hem de görüntüleme ve düzenleme modu, Biçim dizesinde "{0, d}" "kısa tarih" biçiminde görüntülenecek tarihi neden olur. (Bilgisayarınızda bu biçimden farklı bir şekilde Bu öğreticide gösterilen ekran görüntülerini görüntülemek için yapılandırılabilir.)

Bu alanların her biri şablon içinde Tasarımcı kullanılan fark bir `Bind` varsayılan, ancak ifadesi olarak değiştirdiniz bir `Eval` ifadesinde `ItemTemplate` öğeleri. `Bind` İfade verileri kullanılabilir kılar `GridView` kod verilere erişmek gerektiği durumlarda denetim özellikleri. Bu sayfada, kullanabilmeniz için kodda, bu verileri erişmeye ihtiyacınız yoksa `Eval`, daha verimli olan. Daha fazla bilgi için [veri denetimleri verilerinizden alma](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Performansı artırmak için düzeltilmesi EntityDataSource denetimini biçimlendirme

Biçimlendirme için `EntityDataSource` denetlemek, Kaldır `ConnectionString` ve `DefaultContainerName` öznitelikleri ve bunlarla değiştirin bir `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. Bu bir değişiklik yapmanız gerekir her oluşturduğunuzda, bir `EntityDataSource` denetimi sürece nesne bağlamı sınıfında sabit kodlanmış olan farklı bir bağlantı kullanmanız gerekir. Kullanarak `ContextTypeName` özniteliği aşağıdaki faydaları sağlar:

- Daha iyi performans. Zaman `EntityDataSource` denetimi başlatır veri modelini kullanarak `ConnectionString` ve `DefaultContainerName` öznitelikleri her istekte meta verileri yüklemek için ek çalışma gerçekleştirir. Bu belirtirseniz gerekli değildir `ContextTypeName` özniteliği.
- Yavaş yükleniyor açıksa varsayılan olarak oluşturulan nesne bağlamı sınıfları (gibi `SchoolEntities` bu öğreticideki) Entity Framework 4. 0'ı. Bu, ihtiyacınız sağ Gezinti özellikleri ile ilgili verileri otomatik olarak yüklenmiş olduğundan anlamına gelir. Yavaş yükleniyor, bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak açıklanmıştır.
- Nesne bağlamı sınıfı için uyguladığınız özelleştirmeler (Bu durumda, `SchoolEntities` sınıfı) kullanan denetimler için kullanılabilecek `EntityDataSource` denetimi. Nesne bağlamı sınıfı özelleştirme, Bu öğretici serisinde kapsamında olmayan Gelişmiş bir konudur. Daha fazla bilgi için [Entity Framework oluşturulan türleri genişletme](https://msdn.microsoft.com/library/dd456844.aspx).

Biçimlendirme, artık (özelliklerin sırasını farklı olabilir) aşağıdaki örneğe benzer:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Olduğundan yabancı anahtar sütunu varlık özellikleri gösterilmeyen Entity Framework'ün önceki sürümlerinde gerekli bir özellik özniteliğini gösterir. Geçerli sürümü kullanmayı mümkün kılar *yabancı anahtar ilişkilerini*, yabancı anahtar özelliklerini başka bir deyişle, tüm çok-çok ilişkileri için sunulur. Varlıklarınızı yabancı anahtar özelliklerini ve Hayır varsa [karmaşık türler](https://msdn.microsoft.com/library/bb738472.aspx), bu öznitelik ayarlanırsa bırakabilirsiniz `False`. Varsayılan değer olduğundan, öznitelik biçimlendirmeden kaldırmayın `True`. Daha fazla bilgi için [düzleştirme nesneleri (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Sayfayı çalıştırın ve öğrenciler ve Çalışanlar (yalnızca Öğrenciler için sonraki öğreticide filtre) listesini görürsünüz. Ad ve Soyadı birlikte görüntülenir.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Görüntü sıralamak için sütun adı'nı tıklatın.

Tıklayın **Düzenle** herhangi bir satırdaki. Metin kutuları, adı ve Soyadı değiştirebileceğiniz görüntülenir.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Sil** düğmesini de çalışır. Öğesini silmek için bir kayıt tarihi olan bir satır ve satır kaldırılır. (Kayıt tarihi olmayan sıralar Eğitmenler temsil eder ve bir başvuru bütünlüğü hatası alabilirsiniz. Sonraki öğreticide, bu liste yalnızca öğrencileri de içerecek şekilde filtre.)

## <a name="displaying-data-from-a-navigation-property"></a>Bir gezinme özelliği verileri görüntüleme

Kaç tane Derslere bilmek istediğiniz varsayalım her Öğrenci, artık kaydedilmiştir. Entity Framework sağlar, bu bilgileri `StudentGrades` gezinti özelliği `Person` varlık. Veritabanı tasarımı atanmış bir sınıf zorunda kalmadan bir kursa kaydolmak bir öğrenci izin vermediğinden bu öğretici için bu satır olması kabul edilebilir `StudentGrade` kursun Kaydedilmekte aynı olup bir kurs ile ilişkili tablo satırı. ( `Courses` Gezinti özelliği için yalnızca eğitmenler.)

Kullanırken `ContextTypeName` özniteliği `EntityDataSource` denetimi, Entity Framework otomatik olarak bilgileri alır bir gezinme özelliği için bu özelliğe eriştiğinde. Bu adlandırılır *yavaş Yükleniyor*. Her zaman ek bilgi gerekmiyor veritabanı ayrı bir çağrıda sonuçlandığından ancak bu verimsiz olabilir. Tarafından döndürülen her varlık için bir gezinti özelliği verilerden ihtiyacınız varsa `EntityDataSource` denetimi, bu veritabanına bir çağrı içinde varlığın kendisinin yanı sıra ilgili verileri almak üzere daha verimli. Bu adlandırılır *istekli yükleme*, ve ayarlayarak istekli yükleme bir gezinme özelliği için belirttiğiniz `Include` özelliği `EntityDataSource` denetimi.

İçinde *Students.aspx*kursları sayısı için her Öğrenci göstermek istiyorsunuz, bu nedenle istekli yükleme en iyi seçimdir. Tüm Öğrenciler görüntüleme ancak kursları sayısını gösteren yalnızca birkaç tanesi (hangi biçimlendirmeye ek olarak bazı kodları yazmaya gerektirir), için yavaş yükleniyor daha iyi bir seçenek olabilir.

Açın veya geçin *Students.aspx*, geçiş **tasarım** görüntülenecek `StudentsEntityDataSource`ve **özellikleri** penceresi kümesi **INCLUDE**özelliğini **StudentGrades**. (Birden çok gezinti özellikleri almak istediyseniz, adlarını virgülle ayırarak belirtebilirsiniz; Örneğin, **StudentGrades, kursları**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Geçiş **kaynak** görünümü. İçinde `StudentsGridView` en son denetim `asp:TemplateField` öğesi, aşağıdaki yeni şablon alanı ekleyin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

İçinde `Eval` ifade gezinme özelliğini başvurabilirsiniz `StudentGrades`. Bu özellik bir koleksiyonu içerdiğinden, sahip bir `Count` Öğrenci kaydolduğunu kursları sayısını görüntülemek için kullanabileceğiniz özellik. Bir sonraki öğreticide, verileri içeren koleksiyonlar yerine tek bir varlık Gezinti özellikleri görüntülemek nasıl görürsünüz. (Kullanamazsınız Not `BoundField` Gezinti özellikleri verileri görüntülemek için öğeleri.)

Sayfayı çalıştırın ve Öğrenci kaç kursları kaydedilmiş göreceksiniz.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Varlıkları eklemek için bir DetailsView denetimi kullanma

Bir sayfa oluşturmak için sonraki adımdır bir `DetailsView` yeni öğrencileri ekleme olanak tanıyan bir denetim. Tarayıcıyı kapatın ve ardından kullanarak yeni bir web sayfası oluşturma *Site.Master* ana sayfa. Sayfayı adlandırın *StudentsAdd.aspx*, dönersiniz **kaynak** görünümü.

İçin mevcut biçimlendirme değiştirmek için aşağıdaki işaretlemeyi ekleyin `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` , oluşturduğunuz bir benzer denetimi *Students.aspx*dışında ekleme sağlar. Olduğu gibi `GridView` denetim, ilişkili alanlarını `DetailsView` denetimi kodlanmış varlık özellikleri oldukları dışında tam olarak, bir veritabanına doğrudan bağlanan veri denetimi için olduğu gibi. Bu durumda, `DetailsView` denetimi için varsayılan modu ayarladığınız yalnızca satır eklemek için kullanılan `Insert`.

Sayfayı çalıştırın ve yeni bir öğrenci eklemek.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Hiçbir şey yeni bir öğrenci ekledikten sonra ancak şimdi çalıştırırsanız olacağını *Students.aspx*, yeni bir öğrenci bilgi görürsünüz.

## <a name="displaying-data-in-a-drop-down-list"></a>Aşağı açılan listede verileri görüntüleme

Aşağıdaki adımlarda databind gerekir bir `DropDownList` denetimi kullanılarak ayarlanan bir varlığa yönelik bir `EntityDataSource` denetimi. Öğreticinin bu bölümünde, bu liste ile olmaz. Sonraki bölümlerinde, bölümle ilişkilendirilmiş kursları görüntülemek için bir bölüm seçin kullanıcıların listesi kullanacaksınız.

Adlı yeni bir web sayfası oluşturma *Courses.aspx*. İçinde **kaynak** görüntülemek için bir başlığı ekleme `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

İçinde **tasarım** görüntüleme, ekleme bir `EntityDataSource` denetimi sayfası daha önce yaptığınız gibi dışında bu zaman adlandırın, `DepartmentsEntityDataSource`. Seçin **Departmanlar** olarak **EntitySetName** değeri ve yalnızca belirli **DepartmentID** ve **adı** özellikleri.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Gelen **standart** sekmesinde **araç kutusu**, sürükleyin bir `DropDownList` denetlemek sayfaya, adlandırın `DepartmentsDropDownList`, akıllı etiket tıklatın ve seçin **veri kaynağı Seç** için Başlangıç **veri kaynağı Yapılandırma Sihirbazı'nı**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

İçinde **veri kaynağı seçin** adım seçin **DepartmentsEntityDataSource** veri kaynağı olarak tıklayın **Yenile şema**ve ardından **adı** görüntülenecek veri alanı olarak ve **DepartmentID** değeri veri alanı olarak. **Tamam**'ı tıklatın.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Diğer ASP.NET veri kaynağı denetimleri dışında varlıkları ve varlık özellikleri belirlediniz Entity Framework kullanarak denetimi databind için kullandığınız yöntem aynı olur.

Geçiş **kaynak** görüntüleme ve ekleme "bir bölüm seçin:" hemen önce `DropDownList` denetimi.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Bir anımsatıcı değiştirmek için biçimlendirme `EntityDataSource` değiştirerek bu noktada denetim `ConnectionString` ve `DefaultContainerName` ile öznitelikleri bir `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. Genellikle, değiştirmeden önce veri kaynak denetimine bağlı veriye bağlı denetim oluşturduktan sonra bekle en iyisidir `EntityDataSource` değişikliği yaptıktan sonra Tasarımcı sizinle sağlamadığı için biçimlendirmeyi denetleyen bir **Yenile Şema** veriye bağlı denetim seçeneği.

Sayfayı çalıştırın ve bir departman aşağı açılan listeden seçim yapabilirsiniz.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Bu kullanmaya giriş tamamlar `EntityDataSource` denetimi. Varlıklar ve tablolar ve sütunlar yerine özellikleri başvuru dışında bu denetimi ile çalışma kaynağı denetimleri diğer ASP.NET verilerle çalışmasını genellikle farklı değildir. Tek özel durum, gezinti özellikleri erişmek istediğiniz durumdur. Sonraki öğreticide söz dizimi ile kullanmanızı görürsünüz `EntityDataSource` denetim ayrıca farklı diğer veri kaynağı denetimlerini filtrelemek, Grup ve sipariş verileri.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-3.md)
