---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - bölüm 3 | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: fb06176ef195ea5523a66baa938ed5076e0ebe9d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802373"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 3. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtreleme, sıralama ve verileri gruplandırma

Önceki öğreticide kullandığınız `EntityDataSource` verileri görüntülemek ve düzenlemek için denetimi. Bu öğreticide filtre, sipariş ve veri grubu. Bunu yaptığınızda özelliklerini ayarlayarak `EntityDataSource` denetimi sözdizimidir diğer veri kaynağı denetimlerden farklı. Gördüğünüz gibi ancak kullanabileceğiniz `QueryExtender` bu farklılıkları en aza indirmek için denetimi.

Değiştireceksiniz *Students.aspx* Öğrenciler için filtre uygulamak için sayfa adı ve arama adına göre sırala. Ayrıca değiştireceksiniz *Courses.aspx* kursları kursları arayın ve seçili bölüm adına göre görüntülemek için sayfa. Öğrenci istatistikleri nihayetinde ekleyeceksiniz *About.aspx* sayfası.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Verilere filtre uygulamak için EntityDataSource "Nerede" özelliğini kullanma

Açık *Students.aspx* önceki öğreticide oluşturduğunuz sayfası. Şu anda yapılandırılmış, `GridView` sayfasında denetimindeki tüm adlarından görüntüler `People` varlık kümesi. Ancak, yalnızca Öğrenciler, göstermek istediğiniz seçerek bulabileceğiniz `Person` null olmayan kayıt tarihleri sahip varlıklar.

Geçiş **tasarım** görüntüleyebilir ve seçebilir `EntityDataSource` denetimi. İçinde **özellikleri** penceresinde `Where` özelliğini `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Söz dizimi, kullandığınız `Where` özelliği `EntityDataSource` varlık SQL denetimidir. Entity SQL Transact-SQL'e benzer, ancak veritabanı nesneleri yerine varlıklar ile kullanmak için özelleştirilmiş. İfadedeki `it.EnrollmentDate is not null`, word `it` sorgu tarafından döndürülen varlık başvuruyu temsil eder. Bu nedenle, `it.EnrollmentDate` başvurduğu `EnrollmentDate` özelliği `Person` varlık, `EntityDataSource` denetim döndürür.

Sayfayı çalıştırın. Öğrenciler listesine artık yalnızca Öğrenciler içerir. (Burada görüntülenen satır yok hiçbir kayıt tarihi yoktur.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Sipariş verilerini EntityDataSource "OrderBy" özelliğini kullanma

Ayrıca ilk görüntülendiğinde adı sırada olması için bu listeyi istediğiniz. İle *Students.aspx* sayfa içinde açık **tasarım** görünümü ile `EntityDataSource` hala seçiliyken denetim **özellikleri** penceresi kümesi  **OrderBy** özelliğini `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Sayfayı çalıştırın. Öğrenciler listesine sipariş soyadına göre sunulmuştur.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>"Nerede" özelliğini ayarlamak için bir denetim parametresini kullanma

Diğer veri kaynağı denetimleri için parametre değerlerinin geçmesini sağlayabilirsiniz gibi `Where` özelliği. Üzerinde *Courses.aspx* sayfasında öğreticinin 2 oluşturduğunuz, bir kullanıcının açılan listeden seçtiği departmanı ile ilişkili olan kurslar görüntülemek için bu yöntemi kullanabilirsiniz.

Açık *Courses.aspx* geçin **tasarım** görünümü. İkinci bir ekleme `EntityDataSource` sayfasına denetlemek ve adlandırın `CoursesEntityDataSource`. Bağlanmamız `SchoolEntities` model ve seçin `Courses` olarak **EntitySetName** değeri.

İçinde **özellikleri** penceresinde üç noktaya tıklayarak **burada** özellik kutusu. (Emin `CoursesEntityDataSource` denetimi, kullanılmadan önce yine de seçildiğinde **özellikleri** penceresi.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**İfade Düzenleyicisi** iletişim kutusu görüntülenir. Bu iletişim kutusunda **Where otomatik olarak oluşturma ifadesi sağlanan parametreleri alan**ve ardından **parametresi Ekle**. Parametre adı `DepartmentID`seçin **denetimi** olarak **parametre kaynağıyla** değeri ve seçin **DepartmentsDropDownList** olarak **ControlId**  değeri.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Tıklayın **Gelişmiş özellikleri göster**hem de **özellikleri** pencerenin **ifade Düzenleyicisi** iletişim kutusunda, değişiklik `Type` özelliğini `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

İşiniz bittiğinde tıklayın **Tamam**.

Aşağıdaki açılan listeye eklemek bir `GridView` sayfasına denetlemek ve adlandırın `CoursesGridView`. Bağlanmamız `CoursesEntityDataSource` veri kaynak denetimi, tıklayın **Yenile şema**, tıklayın **sütunları Düzenle**, kaldırıp `DepartmentID` sütun. `GridView` Denetim biçimlendirme, aşağıdaki örnekte benzer şekilde görünür.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Kullanıcı açılır listede seçilen departmanındaki değiştiğinde otomatik olarak değiştirmek için ilgili kurslar listesinde istediğiniz. Bu durum, aşağı açılan listeden seçin yapmak ve **özellikleri** penceresi kümesi `AutoPostBack` özelliğini `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

İşiniz bittiğinde Tasarımcısı'nı kullanarak, geçiş **kaynak** görüntüleyin ve değiştirin `ConnectionString` ve `DefaultContainer` ad özelliklerini `CoursesEntityDataSource` denetimini `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` özniteliği. İşiniz bittiğinde, denetim için biçimlendirme, aşağıdaki örnekteki gibi görünecektir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Sayfayı çalıştırın ve farklı departmanlara seçmek için açılan listeyi kullanın. Seçili bölüm tarafından sunulan kurslar görüntülenir `GridView` denetimi.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Grup verileri EntityDataSource "Gruplandırma ölçütü" özelliğini kullanma

Contoso Üniversitesi, ilgili sayfada bazı Öğrenci gövdesi istatistikleri put istediğini varsayalım. Özellikle, Öğrencilerin sayısını dökümünü kayıtlı oldukları tarihe göre gösterilecek istiyor.

Açık *About.aspx*hem de **kaynak** görüntülemek için mevcut içeriğini değiştirin `BodyContent` "Öğrenci gövdesi İstatistikleri" ile Denetim arasındaki `h2` etiketler:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Başlığı sonra ekleme bir `EntityDataSource` denetlemek ve adlandırın `StudentStatisticsEntityDataSource`. Bağlanmamız `SchoolEntities`seçin `People` varlık ayarlayın ve devre dışı bırakın **seçin** değiştirmeden sihirbazda kutusu. Aşağıdaki özellikleri ayarlayın **özellikleri** penceresi:

- Öğrenciler için filtrelemek için `Where` özelliğini `it.EnrollmentDate is not null`.
- Kayıt tarihe göre sonuçları gruplandırmak için ayarlanmış `GroupBy` özelliğini `it.EnrollmentDate`.
- Kayıt tarihi ve Öğrenci sayısını seçmek için ayarlanmış `Select` özelliğini `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Kayıt tarihe göre sonuçları sıralamak için ayarlamak `OrderBy` özelliğini `it.EnrollmentDate`.

İçinde **kaynak** görüntüleyin, değiştirin `ConnectionString` ve `DefaultContainer` ad özelliklere sahip bir `ContextTypeName` özelliği. `EntityDataSource` Denetim biçimlendirme, aşağıdaki örnek artık benzer.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Söz dizimi `Select`, `GroupBy`, ve `Where` özellikleri benzer Transact-SQL dışında `it` geçerli varlığı belirten bir anahtar sözcüğü.

Oluşturmak için aşağıdaki işaretlemeyi ekleyin bir `GridView` verileri görüntülemek için denetim.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Kayıt tarihi tarafından Öğrenci sayısını gösteren bir listesini görmek için sayfayı çalıştırın.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Filtreleme ve sıralama için QueryExtender denetimi kullanma

`QueryExtender` Denetimi filtreleme ve sıralama işaretlemede belirtmek için bir yol sağlar. Sözdizimi, kullanmakta olduğunuz veritabanı yönetim sistemi (DBMS) bağımsızdır. Ayrıca, genellikle bağımsız olarak Entity Framework ile özel durum söz dizimi Gezinti özelliklerini kullanmak için Entity Framework benzersiz olduğundan emin olur.

Öğreticinin bu bölümünde kullanacağınız bir `QueryExtender` filtre ile sipariş verilerini denetimine ve sipariş tarafından alandan biri bir gezinti özelliği olacaktır.

(Kod yerine biçimlendirme otomatik olarak oluşturulan sorgular genişletmek için kullanmayı tercih ederseniz `EntityDataSource` denetim yapabilirsiniz, işleme `QueryCreated` olay. Bu, nasıl `QueryExtender` denetim genişletir `EntityDataSource` denetim sorgular ayrıca.)

Açık *Courses.aspx* sayfasında ve daha önce eklediğiniz biçimlendirme başlığı, dizeleri, arama düğmesini girmek için metin kutusu oluşturmak için aşağıdaki biçimlendirme ekleyin ve bir `EntityDataSource` içinbağlıdenetimi`Courses` varlık kümesi.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Dikkat `EntityDataSource` denetimin `Include` özelliği `Department`. Veritabanındaki `Course` tablo bölüm adını içermiyor; içerdiği bir `DepartmentID` yabancı anahtar sütunu. Kurs verileriyle birlikte bölüm adını almak için veritabanını doğrudan sorgulama, katılmak zorunda `Course` ve `Department` tablolar. Ayarlayarak `Include` özelliğini `Department`, Entity Framework ilgili alma iş yapmalı belirttiğiniz `Department` alır, varlık bir `Course` varlık. `Department` Varlık depolanan ardından `Department` gezinti özelliği `Course` varlık. (Varsayılan olarak, `SchoolEntities` veri modeli tasarımcısı tarafından oluşturulan sınıfı gerekmesi ve o sınıfın şekilde ayarlama, veri kaynağı denetimini bağlı ilgili verileri alır `Include` özellik gerekli değildir. Entity Framework veri almak için çağrıları veritabanına ayrı aksi hale getirir ancak bu ayarın sayfasının performansı artırır `Course` varlıkları ve ilgili için `Department` varlıkları.)

Sonra `EntityDataSource` yeni denetim oluşturmak için aşağıdaki işaretlemeyi ekleyin oluşturulan bir `QueryExtender` olarak bağlı denetimi `EntityDataSource` denetimi.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Öğesi kursları başlıkları eşleşen metin kutusuna girilen değer seçmek istediğinizi belirtir. Metin kutusuna girilen karakterlerinin kaçının, çünkü Karşılaştırılacak yalnızca `SearchType` özellik belirtir `StartsWith`.

`OrderByExpression` Öğeyi belirten bir sonuç kümesi bölüm adı içinde kurs başlığa göre sıralanır. Bölüm adı nasıl belirtildiğine dikkat edin: `Department.Name`. Çünkü arasındaki ilişkiyi `Course` varlık ve `Department` varlıktır bire bir, `Department` gezinti özelliği içeren bir `Department` varlık. (Bu bire çok ilişkisi varsa, özelliği bir koleksiyonu içerir.) Bölüm adını almak için belirtmelisiniz `Name` özelliği `Department` varlık.

Son olarak, ekleme bir `GridView` denetim kursları listesini görüntülemek için:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

İlk sütun bölüm adını görüntüleyen bir şablon alandır. Veri bağlama ifadesi belirtir `Department.Name`gördüğünüz şekilde `QueryExtender` denetimi.

Sayfayı çalıştırın. İlk görüntü departmana göre ve sonra kurs başlığa göre sırayla tüm kursları listesini gösterir.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

"M" girin ve tıklayın **arama** başlıkları "(arama olduğu değil büyük küçük harf duyarlı) m" ile başlayan tüm kursları görmek için.

[![image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>"Gibi" işleci kullanarak verileri filtreleme

Benzer şekilde bir efekti elde edebileceğiniz `QueryExtender` denetimin `StartsWith`, `Contains`, ve `EndsWith` arama türlerini kullanarak bir `Like` işlecinde `EntityDataSource` denetimin `Where` özelliği. Öğreticinin bu bölümünde nasıl kullanılacağını göreceğiniz `Like` işleci için bir öğrenci adına göre aramak için.

Açık *Students.aspx* içinde **kaynak** görünümü. Sonra `GridView` denetlemek, aşağıdaki işaretlemeyi ekleyin:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Bu işaretleme ne daha önce hariç gördüğünüz benzer `Where` özellik değeri. İkinci bölümü `Where` ifade tanımlayan bir alt dize aramayı (`LIKE %FirstMidName% or LIKE %LastName%`) ne olursa olsun metin kutusuna girilen için her iki ilk ve son adlarını arar.

Sayfayı çalıştırın. Varsayılan değer için Başlangıçta tüm Öğrenciler gördüğünüz `StudentName` "%" parametresidir.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

"G" harfini metin kutusuna girin ve tıklayın **arama**. Bir "g" ya da ilk veya son ada sahip Öğrenciler listesini görürsünüz.

[![image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Artık görüntülenen, güncelleştirildi, filtre, sipariş ve ayrı ayrı tablolardaki verileri gruplandırılır. Sonraki öğreticide (ana öğe-ayrıntı senaryoları) ilgili verilerle çalışmaya başlarsınız.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-4.md)
