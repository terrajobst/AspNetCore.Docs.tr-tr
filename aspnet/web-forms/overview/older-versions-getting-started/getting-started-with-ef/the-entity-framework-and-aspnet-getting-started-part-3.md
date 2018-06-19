---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 3 | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889260"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 3
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtreleme, sıralama ve verileri gruplandırma

Önceki öğreticide kullandığınız `EntityDataSource` verileri görüntülemek ve düzenlemek için denetim. Bu öğreticide filtre, sipariş ve veri grubu. Bunu yaptığınızda özelliklerini ayarlayarak `EntityDataSource` denetim, söz dizimi diğer veri kaynağı denetimlerden farklı. Bununla birlikte, göreceğiniz gibi kullanabilirsiniz `QueryExtender` Bu farklılıklar en aza indirmek için denetim.

Değiştireceğiz *Students.aspx* Öğrenciler için filtre uygulamak için sayfa adı ve arama adına göre sırala. Ayrıca değiştireceğiz *Courses.aspx* kurslar arayın ve seçilen departman kursları adına göre görüntülemek için sayfa. Son olarak, Öğrenci istatistikleri ekleyeceksiniz *About.aspx* sayfası.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Verileri filtrelemek için EntityDataSource "Burada" özelliğini kullanarak

Açık *Students.aspx* önceki öğreticide oluşturduğunuz sayfası. Şu anda yapılandırılmış `GridView` sayfasında denetimindeki tüm adlarından görüntüler `People` varlık kümesi. Ancak, yalnızca Öğrenciler göstermek istediğiniz seçerek bulabileceğiniz `Person` null olmayan kayıt tarihi olan varlık.

Geçiş **tasarım** görüntülemek ve seçim `EntityDataSource` denetim. İçinde **özellikleri** penceresindeki ayarlayın `Where` özelliğine `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

De kullandığınız sözdizimi `Where` özelliği `EntityDataSource` varlık SQL denetimdir. Varlık SQL Transact-SQL benzer, ancak veritabanı nesneleri yerine varlıkları ile kullanılmak üzere özelleştirilebilir. İfadedeki `it.EnrollmentDate is not null`, word `it` sorgu tarafından döndürülen varlığı için referans temsil eder. Bu nedenle, `it.EnrollmentDate` başvurduğu `EnrollmentDate` özelliği `Person` varlık, `EntityDataSource` denetim döndürür.

Sayfayı çalıştırın. Öğrenciler listesi şimdi yalnızca Öğrenciler içerir. (Görüntülendiği satır yok hiçbir kayıt tarihi yoktur.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Sipariş verilerini EntityDataSource "OrderBy" özelliğini kullanma

İlk görüntülendiğinde adı sırada olması için bu listeyi da isteyebilirsiniz. İle *Students.aspx* sayfa içinde hala açık **tasarım** görünümü ile `EntityDataSource` denetimi yine de, seçili **özellikleri** penceresi kümesi  **OrderBy** özelliğine `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Sayfayı çalıştırın. Öğrenciler listesi sipariş son ada göre sunulmuştur.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>"Nerede" özelliğini ayarlamak için bir denetim parametresini kullanma

Diğer veri kaynağı denetimleri için parametre değerlerinin geçmesini sağlayabilirsiniz gibi `Where` özelliği. Üzerinde *Courses.aspx* sayfasında öğreticinin 2 bölümünde oluşturduğunuz, aşağı açılan listeden bir kullanıcı seçer departmanı ile ilişkili kurslar görüntülemek için bu yöntemi kullanabilirsiniz.

Açık *Courses.aspx* ve geçiş **tasarım** görünümü. İkinci bir ekleme `EntityDataSource` kontrol sayfasına ve adlandırın `CoursesEntityDataSource`. Buna bağlanmak `SchoolEntities` model ve seçin `Courses` olarak **EntitySetName** değeri.

İçinde **özellikleri** penceresinde, üç noktaya tıklayın **burada** özellik kutusu. (Emin olun `CoursesEntityDataSource` denetimin halen seçili kullanmadan önce **özellikleri** penceresi.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**İfade Düzenleyicisi** iletişim kutusu görüntülenir. Bu iletişim kutusunda seçin **nerede otomatik olarak üret ifade tabanlı sağlanan parametreleri**ve ardından **parametresi Ekle**. Parametre adı `DepartmentID`seçin **denetim** olarak **parametre kaynağı** değer ve seçin **DepartmentsDropDownList** olarak **ControlId**  değeri.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Tıklatın **gelişmiş özelliklerini göster**hem de **özellikleri** pencerenin **ifade Düzenleyicisi** iletişim kutusu, değişiklik `Type` özelliğine `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

İşiniz bittiğinde tıklatın **Tamam**.

Aşağıdaki açılan listeye eklemek bir `GridView` kontrol sayfasına ve adlandırın `CoursesGridView`. Bağlanmak `CoursesEntityDataSource` veri kaynağı denetimi,'ı tıklatın **yenileme şema**, tıklatın **Edit Columns**, kaldırıp `DepartmentID` sütun. `GridView` Denetim biçimlendirme, aşağıdaki örnekte benzer.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Kullanıcı aşağı açılan listesinde seçili Departman değiştiğinde otomatik olarak değiştirmek için ilişkili kurslar listesi istiyor. Bu durum, aşağı açılan listeden seçin yapmak için ve **özellikleri** penceresi kümesi `AutoPostBack` özelliğine `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Tasarımcı kullanarak tamamlanmış, geçiş **kaynak** görüntülemek ve değiştirmek `ConnectionString` ve `DefaultContainer` ad özelliklerini `CoursesEntityDataSource` ile kontrol `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. İşiniz bittiğinde, denetim için biçimlendirme aşağıdaki gibi görünecektir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Sayfayı çalıştırın ve farklı departmanlara seçmek için açılır listeyi kullanın. Seçili bölüm tarafından sunulan kurslar görüntülenir `GridView` denetim.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Grup verileri EntityDataSource "GroupBy" özelliğini kullanma

Contoso University bazı Öğrenci gövde istatistik hakkında sayfasında put istediğini varsayalım. Özellikle, bunlar kayıtlı tarihe göre Öğrenciler sayıda dökümünü göstermek istemektedir.

Açık *About.aspx*hem de **kaynak** görüntülemek için varolan Değiştir `BodyContent` arasında "İle Öğrenci gövde İstatistikleri" Denetim `h2` etiketler:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Başlığı sonra eklemek bir `EntityDataSource` denetlemek ve adlandırın `StudentStatisticsEntityDataSource`. Buna bağlanmak `SchoolEntities`seçin `People` varlık ayarlamak ve bırakın **seçin** değiştirmeden sihirbazda kutusu. Aşağıdaki özellikler kümesinde **özellikleri** penceresi:

- Öğrenciler için filtrelemek için `Where` özelliğine `it.EnrollmentDate is not null`.
- Sonuçları kayıt tarihe göre gruplandırmak için ayarlanmış `GroupBy` özelliğine `it.EnrollmentDate`.
- Kayıt tarihi ve öğrenciler sayısını seçmek üzere ayarlamak `Select` özelliğine `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Sonuçları kayıt tarihe göre sıralamak için ayarlanmış `OrderBy` özelliğine `it.EnrollmentDate`.

İçinde **kaynak** görüntüleyin, değiştirin `ConnectionString` ve `DefaultContainer` ad özelliklere sahip bir `ContextTypeName` özelliği. `EntityDataSource` Denetim biçimlendirme, aşağıdaki örnekte şimdi benzer.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Söz dizimi `Select`, `GroupBy`, ve `Where` özellikleri benzer Transact-SQL dışında `it` geçerli varlık belirtir anahtar sözcüğü.

Oluşturmak için aşağıdaki biçimlendirmeleri eklemek bir `GridView` verileri görüntülemek için denetim.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Kayıt tarihe göre Öğrenciler sayısını gösteren bir listesini görmek için page çalıştırın.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Filtreleme ve sıralama için QueryExtender denetimi kullanma

`QueryExtender` Denetimi biçimlendirmede sıralama ve filtreleme belirtmek için bir yol sağlar. Söz dizimi, kullandığınız veritabanı yönetim sistemi (DBMS) bağımsızdır. Bu ayrıca genellikle, gezinti özellikleri için kullandığınız sözdizimi için Entity Framework benzersiz olduğunu özel Entity Framework'ün bağımsızdır.

Öğreticinin bu bölümünde kullanacağınız bir `QueryExtender` denetimi filtrelerin ve order verilere ve bir sipariş tarafından alanlarının bir gezinti özelliği olacaktır.

(Otomatik olarak oluşturulan sorguları genişletmek için yerine biçimlendirme kodu kullanmayı tercih ederseniz, `EntityDataSource` denetimi yapabilirsiniz, işleme `QueryCreated` olay. Bunun nasıl `QueryExtender` denetimi `EntityDataSource` kontrol sorguları da.)

Açık *Courses.aspx* sayfasında ve daha önce eklediğiniz biçimlendirme altında bir başlığı, arama dizeleri, arama düğmesini girileceği metin kutusu oluşturmak için aşağıdaki biçimlendirme eklemek ve bir `EntityDataSource` bağlıdenetimi`Courses` varlık kümesi.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Dikkat `EntityDataSource` denetimin `Include` özelliği ayarlanmış `Department`. Veritabanında `Course` tablo bölüm adını içermiyor; içerdiği bir `DepartmentID` yabancı anahtar sütunu. İndirmelere verilerin bölüm adını almak için veritabanını doğrudan sorgulama, katılmak olurdu `Course` ve `Department` tabloları. Ayarlayarak `Include` özelliğine `Department`, Entity Framework ilgili alma işini yapması belirtin `Department` alır, varlık bir `Course` varlık. `Department` Varlık depolanan sonra `Department` Gezinti özelliğinin `Course` varlık. (Varsayılan olarak, `SchoolEntities` veri modeli tasarımcısı tarafından oluşturulan sınıf gerektiğinde ve böylece ayarı bu sınıf için veri kaynağı denetimi bağladınız ilgili verileri alır `Include` özellik gerekli değildir. Aksi takdirde Entity Framework veritabanı için veri alma çağrıları ayrı olarak yapmayacağı ancak ayarlamak sayfa performansını artırır `Course` varlıkları ve ilgili için `Department` varlıklar.)

Sonra `EntityDataSource` yeni denetimi oluşturmak için aşağıdaki biçimlendirme Ekle oluşturulmuş bir `QueryExtender` olarak bağlı denetim `EntityDataSource` denetim.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Başlıkları eşleşen metin kutusuna girilen değer kurslar seçmek istediğiniz öğenin belirtir. Metin kutusuna girilen karakterlerinin kaçının, çünkü Karşılaştırılacak yalnızca `SearchType` özellik belirtir `StartsWith`.

`OrderByExpression` Öğesi belirttiğinden sonuç kümesi bölüm adı içinde indirmelere başlığa göre sıralanır. Bölüm adı nasıl belirtilen dikkat edin: `Department.Name`. Çünkü arasındaki ilişkiyi `Course` varlık ve `Department` varlıktır birebir, `Department` gezinti özelliği içeren bir `Department` varlık. (Bu bir-çok ilişkisi varsa, özelliği bir koleksiyon içerecektir.) Bölüm adını almak için belirtmelisiniz `Name` özelliği `Department` varlık.

Son olarak, ekleme bir `GridView` denetiminin kurslar listesini görüntülemek için:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

İlk sütun bölüm adını görüntüleyen bir şablon alanıdır. Veri bağlama deyimi belirtir `Department.Name`, içinde gördüğünüz gibi `QueryExtender` denetim.

Sayfayı çalıştırın. İlk görünen tüm kursları listesini departmanı ve sonra da indirmelere başlığa göre sırayla gösterir.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Bir "m" girin ve tıklayın **arama** başlıkları "(arama olduğu değil büyük-küçük harf duyarlı) m" ile başlayan tüm kursları görmek için.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Verileri filtrelemek için "Gibi" işleci kullanma

Benzer şekilde bir etkisi elde edebilirsiniz `QueryExtender` denetimin `StartsWith`, `Contains`, ve `EndsWith` arama türlerini kullanarak bir `Like` işlecinde `EntityDataSource` denetimin `Where` özelliği. Öğreticinin bu bölümünde, nasıl kullanılacağını görürsünüz `Like` için öğrencinin adına göre aramak için işleci.

Açık *Students.aspx* içinde **kaynak** görünümü. Sonra `GridView` denetlemek, aşağıdaki biçimlendirmeyi ekleyin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Bu biçimlendirme ne önceki dışında gördüğünüz için benzer `Where` özellik değeri. İkinci bölümü `Where` ifade substring arama tanımlar (`LIKE %FirstMidName% or LIKE %LastName%`) ne olursa olsun metin kutusuna girilen için hem ilk ve son adlar arar.

Sayfayı çalıştırın. Varsayılan değer olduğundan Başlangıçta tüm Öğrenciler gördüğünüz `StudentName` parametresi "%" şeklindedir.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Harf "g" metin kutusuna girin ve tıklayın **arama**. Bir "g" ya da ilk veya son ada sahip Öğrenciler listesini görürsünüz.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Şimdi görüntülenen, güncelleştirilmiş, filtre, sıralı ve tek tek tablolardaki verileri gruplandırılır. Sonraki öğreticide (ana-ayrıntı senaryolarında) ilgili verilerle çalışmak başlarsınız.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [sonraki](the-entity-framework-and-aspnet-getting-started-part-4.md)
