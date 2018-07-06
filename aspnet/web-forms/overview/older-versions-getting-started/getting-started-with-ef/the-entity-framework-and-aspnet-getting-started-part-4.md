---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 4. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 5f8b1c15fbfd2d65b603013db3902b42faa40665
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836794"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 4. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>İlgili verileri ile çalışma

Önceki öğreticide kullandığınız `EntityDataSource` grubu verileri filtreleme, sıralama ve denetimi. Bu öğreticide görüntüleyebilir ve ilgili verileri güncelleştirme.

Eğitmenler listesini gösteren Eğitmenler sayfa oluşturacaksınız. Bir eğitmen seçtiğinizde, eğitmenler tarafından verilen derslerimiz listesini görürsünüz. Bir kurs seçtiğinizde, kurs ve kursun kayıtlı öğrencilerin listesini ayrıntılarını görürsünüz. Eğitmen adını, işe alım tarihi ve ofis ataması düzenleyebilirsiniz. Ofis ataması bir gezinti özelliği üzerinden erişen ayrı varlık kümesidir.

Ana veri ayrıntı verilerini kodda veya biçimlendirmede bağlayabilirsiniz. Öğreticinin bu bölümünde, her iki yöntem de kullanmanız gerekir.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>GridView denetimindeki ilgili varlıkları güncelleştirme ve görüntüleme

Adlı yeni bir web sayfası oluşturma *Instructors.aspx* kullanan *Site.Master* ana sayfa ve eklemek için aşağıdaki biçimlendirme `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` Eğitmenler seçer ve güncellemelerini denetimi. `div` Öğe daha sonra sağdaki sütun ekleyebilmeniz soldaki işlemek için biçimlendirme yapılandırır.

Arasında `EntityDataSource` işaretleme ve kapatma `</div>` etiketinde, oluşturan aşağıdaki işaretlemeyi ekleyin bir `GridView` denetimi ve bir `Label` hata iletileri için kullanacağınız denetimi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Bu `GridView` denetimi satır seçimini etkinleştirir, açık gri arka plan rengi ile seçilen satırı vurgular ve belirtir (Bu, daha sonra oluşturacağınız) için işleyiciler `SelectedIndexChanged` ve `Updating` olayları. Ayrıca belirtir `PersonID` için `DataKeyNames` özelliği, böylece daha sonra ekleyeceğiniz başka bir denetime anahtar değeri seçilen sıranın geçirilebilir.

Son sütun içeren bir gezinti özelliği içinde depolanan Eğitmen ofis ataması `Person` varlık ilişkili bir varlıktan geldiğinden. Dikkat `EditItemTemplate` öğesi belirtir `Eval` yerine `Bind`, çünkü `GridView` denetimi doğrudan bağlanamıyor Gezinti özellikleri için bunları güncelleştirmek için. Kod içinde office atamayı güncelleştireceksiniz. Bunu yapmak için bir başvuru gerekir `TextBox` denetimi ve almak ve, kaydetmek `TextBox` denetimin `Init` olay.

Aşağıdaki `GridView` denetimi bir `Label` hata iletileri için kullanılan bir denetim. Denetimin `Visible` özelliği `false`, ve görünüm durumunu Kapalı, etiket yalnızca, kod bir hata yanıtı görünür kolaylaştırır görünür.

Açık *Instructors.aspx.cs* dosyasını açıp aşağıdaki `using` deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Hemen kısmi sınıf adı bildiriminin office atama metin kutusu için bir başvuru tutan bir özel sınıf alan ekleyin.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Eklemek için bir saplama `SelectedIndexChanged` olay işleyici kodu daha sonra kullanmak üzere ekleyeceksiniz. Ayrıca office ataması için bir işleyici eklemek `TextBox` denetimin `Init` olay başvuru depolayabileceğiniz `TextBox` denetimi. Bu başvuru, değer sahip gezinme özelliği ilişkili varlık güncelleştirmek için girilen kullanıcı almak için kullanacaksınız.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Kullanacağınız `GridView` denetimin `Updating` güncelleştirilecek olay `Location` ilişkilendirilen özellik `OfficeAssignment` varlık. Aşağıdaki işleyicisi eklemek `Updating` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Kullanıcı tıkladığında bu kod çalıştırılır **güncelleştirme** içinde bir `GridView` satır. Kodu almak için LINQ to Entities kullanan `OfficeAssignment` geçerli ilişkili varlık `Person` varlığı kullanarak `PersonID` olay bağımsız seçilen satırın.

Kodun ardından değere bağlı olarak aşağıdaki eylemlerden birini alır `InstructorOfficeTextBox` denetimi:

- Metin kutusuna bir değere sahip ve olduğundan hiçbir `OfficeAssignment` güncelleştirmek için varlık tane oluşturur.
- Metin kutusuna bir değere sahip ve var olan bir `OfficeAssignment` varlık güncelleştirdiği `Location` özellik değeri.
- Metin kutusu boş olduğunda ve bir `OfficeAssignment` varlıkta, varlığın siler.

Bundan sonra değişiklikleri veritabanına kaydeder. Bir özel durum oluşursa bir hata iletisi görüntüler.

Sayfayı çalıştırın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Tıklayın **Düzenle** ve metin kutularına tüm alanları değiştirin.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Dahil olmak üzere, bu değerlerden herhangi birini değiştirmek **ofis ataması**. Tıklayın **güncelleştirme** listesinde yansımış değişiklikleri görebilirsiniz.

## <a name="displaying-related-entities-in-a-separate-control"></a>Ayrı bir denetimde ilgili varlıkları görüntüleme

Her Eğitmen ekleyeceğiniz için bir veya daha fazla kursları öğretebiliriz bir `EntityDataSource` denetimi ve bir `GridView` hangi Eğitmen eğitmenlerini seçili ile ilişkili kursları listelemek için Denetim `GridView` denetimi. Bir bölüm başlığı oluşturmak için ve `EntityDataSource` denetleme kurs varlıklar için hata iletisi arasında aşağıdaki işaretlemeyi ekleyin `Label` denetimi ve kapatma `</div>` etiketi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametre değerini içeren `PersonID` olan satır içinde seçili eğitmen, `InstructorsGridView` denetimi. `Where` Özelliği içerir, ilişkili tüm alır yazarak bir komut `Person` varlıklardan bir `Course` varlığın `People` gezinti özelliği ve seçer `Course` varlık yalnızca şu durumlarda bir ilişkili `Person`varlıklarını içeren seçili `PersonID` değeri.

Oluşturulacak `GridView` denetim. hemen ardından aşağıdaki işaretlemeyi ekleyin `CoursesEntityDataSource` denetimi (kapatmadan önce `</div>` etiketi):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Hiçbir Eğitmen seçili ise, hiçbir kursları görüntülenen bir `EmptyDataTemplate` öğesi bulunur.

Sayfayı çalıştırın.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Bir veya daha fazla kursları atanmış olan bir eğitmen seçin ve listede Kurs veya kurslar. (Not: birden çok kursları veritabanı şeması olanak tanısa da veritabanı ile sağlanan test verilerini hiçbir Eğitmen gerçekten birden fazla kurs vardır. Kursları veritabanına kendiniz ekleyebileceğiniz kullanarak **Sunucu Gezgini** penceresi veya *CoursesAdd.aspx* sayfasında, bir sonraki öğreticide ekleyeceksiniz.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Denetimi yalnızca birkaç kurs alanları gösterir. Bir kurs için tüm ayrıntılarını görüntülemek için kullanacağınız bir `DetailsView` kullanıcının seçtiği kursa denetimi. İçinde *Instructors.aspx*, kapattıktan sonra aşağıdaki işaretlemeyi ekleyin `</div>` etiketi (Bu işaretleme yerleştirdiğiniz emin **sonra** kapanış div etiketi, daha önce):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` bağlı denetim `Courses` varlık kümesi. `Where` Özelliği seçen bir kurs kullanarak `CourseID` kursları seçilen satırın değerini `GridView` denetimi. İşaretleme için bir işleyici belirtir `Selected` başka bir düzey hiyerarşide daha düşük olan Öğrenci derece görüntülemek için daha sonra kullanacağınız, olay.

İçinde *Instructors.aspx.cs*, oluşturmak için aşağıdaki saplama `CourseDetailsEntityDataSource_Selected` yöntemi. (Öğreticinin ilerleyen bölümlerinde bu saplama, doldururlar; sayfa derler ve çalışır böylece şu an için ihtiyacınız.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Sayfayı çalıştırın.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Başlangıçta hiç kurs seçilmediğinden kurs ayrıntı yok vardır. Bir kurs atanmış olan bir eğitmen seçin ve ardından ayrıntılarını görmek için bir kurs seçin.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>EntityDataSource kullanarak "ilgili verileri görüntülemek için olay seçili"

Son olarak, tüm kayıtlı Öğrenciler ve bunların derece seçili kursa göstermek istiyorsunuz. Bunu yapmak için kullanacağınız `Selected` olayı `EntityDataSource` denetimine bağlı kursa `DetailsView`.

İçinde *Instructors.aspx*, sonra aşağıdaki işaretlemeyi ekleyin `DetailsView` denetimi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Bu biçimlendirme oluşturur bir `ListView` Öğrenciler ve bunların derece seçili kursa listesini görüntüleyen denetim. Kod denetiminde veri bağlama gerekir çünkü veri kaynağı belirtildi. `EmptyDataTemplate` Öğesi hiçbir kurs seçili olduğunda görüntülenecek bir ileti sağlar; bu durumda, görüntülenecek Öğrenci vardır. `LayoutTemplate` Öğesi listesini görüntülemek için bir HTML tablosu oluşturur ve `ItemTemplate` gösterilecek sütunları belirtir. Öğrenci Kimliği ve Öğrenci sınıf arasındadır `StudentGrade` varlık ve Öğrenci adı geldiği `Person` Entity Framework kullanılabilmesini varlık `Person` gezinti özelliği `StudentGrade` varlık.

İçinde *Instructors.aspx.cs*, saplama çıkış değiştirin `CourseDetailsEntityDataSource_Selected` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Seçili verileri sıfır öğe hiçbir şey seçili değilse veya bir öğe olan bir koleksiyon biçiminde bu olay için olay bağımsız değişkenini sağlar, bir `Course` varlığı seçilidir. Varsa bir `Course` varlık seçildiğinde, kod `First` koleksiyonu tek bir nesneye dönüştürmek için yöntemi. Ardından alır `StudentGrade` gezinti özelliği varlıklardan bunları bir koleksiyona dönüştürür ve bağlar `GradesListView` koleksiyona denetimi.

Bu derece görüntülemek için ancak boş veri şablonun iletinin sayfasına ilk kez görüntülendiğini emin olmanız yeterlidir ve her bir kurs seçilmedi. Bunu yapmak için iki yerden ararız aşağıdaki yöntemi oluşturun:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Bu yeni yöntemi çağırın `Page_Load` sayfası görüntülenirse boş veri şablonu ilk zaman görüntülemek için yöntemi. Ve ondan çağrı `InstructorsGridView_SelectedIndexChanged` yöntemini eğitmenin seçildiğinde bu olay tetiklenir çünkü yeni Kursların yani yüklenir kurslara `GridView` denetimi ve hiçbiri seçili henüz. İki çağrıları şunlardır:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Sayfayı çalıştırın.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Bir kurs atanmış olan bir eğitmen seçip ardından kursu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Şimdi, ilgili verilerle çalışmak için birkaç yolu gördünüz. Aşağıdaki öğreticide, var olan varlıkları arasındaki ilişkileri ekleme öğreneceksiniz ilişkileri kaldırma ve var olan bir varlığa yönelik bir ilişkisi olan yeni bir varlık ekleme.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-5.md)
