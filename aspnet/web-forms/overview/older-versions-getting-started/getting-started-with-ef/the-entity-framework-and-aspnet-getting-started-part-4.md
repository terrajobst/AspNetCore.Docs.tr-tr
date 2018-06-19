---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 4 | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886875"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 4
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>İlgili verileri ile çalışma

Önceki öğreticide kullandığınız `EntityDataSource` denetim filtresi, sıralama ve Grup verileri. Bu öğreticide görüntülemek ve ilgili verileri güncelleştirin.

Eğitmen listesini gösteren Eğitmen sayfası oluşturacaksınız. Bir eğitmen seçtiğinizde, o Eğitmen tarafından öğrettin kurslar listesini görürsünüz. Bir indirmelere seçtiğinizde, Ayrıntılar indirmelere ve öğrenciler indirmelere kayıtlı bir listesi için bkz. Eğitmen adını, işe alma tarihi ve office atama düzenleyebilirsiniz. Office atama bir gezinti özelliği üzerinden erişen ayrı bir varlık kümesidir.

Ana veri ayrıntı verileri işaretleme veya kod bağlayabilirsiniz. Öğreticinin bu bölümünde, her iki yöntem kullanırsınız.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Görüntüleme ve bir GridView denetimindeki ilgili varlıkları güncelleştirme

Adlı yeni bir web sayfası oluşturun *Instructors.aspx* kullanan *Site.Master* ana sayfa ve aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` Eğitmen seçer ve güncellemelerini denetim. `div` Öğesi sağ tarafta daha sonra bir sütun ekleyeceği sol tarafta işlemek için biçimlendirme yapılandırır.

Arasında `EntityDataSource` biçimlendirme ve kapanış `</div>` etiketi, oluşturan aşağıdaki biçimlendirmeleri eklemek bir `GridView` denetim ve `Label` hata iletileri için kullanacağınız denetimi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Bu `GridView` denetimi satır seçimi sağlar, seçilen satırın açık gri arka plan rengiyle vurgular ve belirtir (daha sonra oluşturacağınız) işleyicileri için `SelectedIndexChanged` ve `Updating` olaylar. Ayrıca belirtir `PersonID` için `DataKeyNames` özelliği, böylece daha sonra eklersiniz başka bir denetim için seçilen satırın anahtar değeri geçirilebilir.

Bir gezinti özelliği içinde depolanan Eğitmen office atama son sütun içeriyor `Person` varlık ilişkili bir varlıktan geldiğinden. Dikkat `EditItemTemplate` öğesi belirttiğinden `Eval` yerine `Bind`, çünkü `GridView` denetim doğrudan bağlanamıyor Gezinti özellikleri için bunları güncelleştirmek için. Kodda office atama güncelleştireceksiniz. Bunu yapmak için bir başvuru gerekir `TextBox` denetimi ve almak ve giriş kaydetmek `TextBox` denetimin `Init` olay.

Aşağıdaki `GridView` denetimi bir `Label` hata iletileri için kullanılan denetim. Denetimin `Visible` özelliği `false`, ve görünüm durumu kapalı, böylece etiketi yalnızca zaman kodu bir hata yanıtı görünür kolaylaştırır görünür.

Açık *Instructors.aspx.cs* dosya ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Özel sınıfı alanına atama metin kutusu için bir başvuru tutmak için hemen kısmi sınıf adı bildirimi ekleyin.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

İçin bir saplama eklemek `SelectedIndexChanged` olay işleyicisi kod için daha sonra eklersiniz. Ayrıca office atama için bir işleyici ekleyin `TextBox` denetimin `Init` olay başvuru depolayabileceğiniz `TextBox` denetim. Bu başvuru gezinti özelliği ile ilişkili varlık güncelleştirmek için girdiğiniz kullanıcı değeri almak için kullanacaksınız.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Kullanacağınız `GridView` denetimin `Updating` güncelleştirmek için olay `Location` ilişkilendirilen özellik `OfficeAssignment` varlık. Aşağıdaki işleyicisi ekleme `Updating` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Kullanıcı tıkladığında bu kodu çalıştırmak **güncelleştirme** içinde bir `GridView` satır. Kod LINQ to Entities almak için kullanır. `OfficeAssignment` geçerli ilişkili varlık `Person` varlık kullanarak `PersonID` seçilen satırın gelen olay bağımsız değişken.

Kod ardından değeri bağlı olarak aşağıdaki eylemlerden birini alır `InstructorOfficeTextBox` denetimi:

- Metin kutusuna bir değer içeriyor ve var olup olmadığını hiç `OfficeAssignment` güncelleştirmek için varlık tane oluşturur.
- Metin kutusuna bir değer içeriyor ve var olan bir `OfficeAssignment` varlık, güncelleştirdiği `Location` özellik değeri.
- Metin kutusu boş ise ve bir `OfficeAssignment` varlık var, varlık siler.

Bundan sonra bu değişiklikleri veritabanına kaydeder. Bir özel durum oluşursa, bir hata iletisi görüntüler.

Sayfayı çalıştırın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Tıklatın **Düzenle** ve tüm alanları metin kutularına değiştirin.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Dahil olmak üzere, bu değerleri değiştirmek **Office atama**. Tıklatın **güncelleştirme** ve listede yansıtılan değişiklikleri görürsünüz.

## <a name="displaying-related-entities-in-a-separate-control"></a>İlgili varlıklar ayrı bir denetiminde görüntüleme

Eklediğiniz böylece her Eğitmen bir veya daha fazla kurslar öğretmek bir `EntityDataSource` denetim ve `GridView` hangi Eğitmen Eğitmen seçili ile ilişkili kursları listelemek için Denetim `GridView` denetim. Bir başlığı oluşturmak için ve `EntityDataSource` denetiminde kurslar varlıklar için hata iletisi arasında aşağıdaki biçimlendirmeleri eklemek `Label` denetimi ve kapanış `</div>` etiketi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametre değeri içeriyor `PersonID` olan satır seçildiğinde, eğitmen, `InstructorsGridView` denetim. `Where` Özelliği, ilişkili tüm alır yazarak bir komut içerir `Person` varlıklardan bir `Course` varlığın `People` gezinti özelliği ve seçer `Course` varlık eksikse ilişkili birini`Person`varlıklarını içeren seçili `PersonID` değeri.

Oluşturmak için `GridView` denetimi. hemen ardından aşağıdaki biçimlendirmeleri eklemek `CoursesEntityDataSource` denetimi (kapatmadan önce `</div>` etiketi):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Hiçbir Eğitmen seçilirse, hiçbir kurslar görüntülenen bir `EmptyDataTemplate` öğesi eklenmiştir.

Sayfayı çalıştırın.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Atanan bir veya daha fazla kurslar sahip bir eğitmen seçin ve indirmelere veya kurslar listesinde görüntülenir. (Not: birden çok kurslar veritabanı şeması olanak tanısa da veritabanı ile sağlanan test verilerde hiçbir Eğitmen gerçekte birden fazla indirmelere vardır. Kurslar veritabanına kendiniz ekleyebileceğiniz kullanarak **Sunucu Gezgini** penceresi veya *CoursesAdd.aspx* sonraki öğreticide ekleyeceksiniz sayfası.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Denetimi yalnızca birkaç indirmelere alanları gösterir. Bir indirmelere tüm ayrıntılarını görüntülemek için kullanacağınız bir `DetailsView` denetimi kullanıcının seçtiği indirmelere için. İçinde *Instructors.aspx*, kapattıktan sonra aşağıdaki biçimlendirme eklemek `</div>` etiketi (Bu biçimlendirme yerleştirdiğiniz emin olun **sonra** kapanış div etiketi, önce onu):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` bağlı denetim `Courses` varlık kümesi. `Where` Özelliği seçer kullanarak bir indirmelere `CourseID` kursları seçilen satırın değerini `GridView` denetim. İşaretleme için bir işleyici belirtir `Selected` başka bir düzeyi hiyerarşisinde daha düşük olan, daha sonra Öğrenci dereceleri görüntülemek için kullanacağınız, olayı.

İçinde *Instructors.aspx.cs*, oluşturmak için aşağıdaki saplama `CourseDetailsEntityDataSource_Selected` yöntemi. (Bu saplama daha sonra öğreticide doldurduğunuz; sayfa derlemek ve çalıştırmak için şu an için ihtiyacınız.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Sayfayı çalıştırın.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Başlangıçta hiçbir indirmelere seçilmediğinden indirmelere ayrıntı yok vardır. Atanmış bir indirmelere sahip bir eğitmen seçin ve ardından bir indirmelere ayrıntıları görmek için seçin.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>EntityDataSource kullanılarak "olay ilgili verileri görüntülemek için seçilen"

Son olarak, tüm kayıtlı Öğrenciler ve seçili indirmelere için kendi dereceleri göstermek istiyorsunuz. Bunu yapmak için kullanacağınız `Selected` olayı `EntityDataSource` denetim bağlı indirmelere `DetailsView`.

İçinde *Instructors.aspx*, sonra aşağıdaki biçimlendirmeleri eklemek `DetailsView` denetimi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Bu biçimlendirme oluşturur bir `ListView` Öğrenciler ve seçili indirmelere için kendi dereceleri listesini görüntüleyen denetim. Databind kodu denetiminde gerekir çünkü hiçbir veri kaynağı belirtilmedi. `EmptyDataTemplate` Öğesi hiçbir indirmelere seçildiğinde görüntülenecek bir ileti sağlar; bu durumda, görüntülemek için hiçbir Öğrenciler vardır. `LayoutTemplate` Öğesi listesini görüntülemek için bir HTML tablosu oluşturur ve `ItemTemplate` görüntülenecek sütunları belirtir. Öğrenci Kimliğini ve Öğrenci düzeyde arasındadır `StudentGrade` varlık ve Öğrenci adı olan `Person` Entity Framework kullanılabilmesini varlık `Person` Gezinti özelliğinin `StudentGrade` varlık.

İçinde *Instructors.aspx.cs*, tamamlanmamış çıkış Değiştir `CourseDetailsEntityDataSource_Selected` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Bu olay için olay bağımsız değişken bir öğe veya hiçbir şey seçtiyseniz sıfır öğe olan koleksiyon, formdaki seçili verileri sağlar, bir `Course` varlık seçilidir. Varsa bir `Course` varlık seçildiğinde, kodu kullanan `First` koleksiyonu tek bir nesne olarak dönüştürmek için yöntem. Ardından alır `StudentGrade` gezinti özelliği varlıklardan bunları bir koleksiyona dönüştürür ve bağlar `GradesListView` koleksiyonuna denetim.

Bu görüntülenecek dereceleri ancak boş veri şablonu iletisinde sayfasına ilk kez görüntülendiğinden emin olmak istiyoruz yeterlidir ve her bir indirmelere seçilmedi. Bunu yapmak için iki yerde çağırmanız aşağıdaki yöntemi oluşturun:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Bu yeni yöntemi çağırın `Page_Load` sayfası görüntülenirse boş veri şablonu ilk zaman görüntülemek için yöntem. Ve ondan çağrısı `InstructorsGridView_SelectedIndexChanged` yöntemi bir eğitmen seçildiğinde bu olay tetiklenir olduğundan, yeni kurslar yani yüklenir kurslara `GridView` denetim ve hiçbiri seçili henüz. İki çağrıları şunlardır:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Sayfayı çalıştırın.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Atanmış bir indirmelere sahip bir eğitmen seçin ve ardından indirmelere seçin.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Şimdi ilgili verilerle çalışmak için birkaç şekilde gördünüz. Aşağıdaki öğreticide, var olan varlıkları arasındaki ilişkileri ekleme konusunda bilgi edineceksiniz ilişkileri kaldırma ve var olan bir varlığa bir ilişkisi olan yeni bir varlık ekleme.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [sonraki](the-entity-framework-and-aspnet-getting-started-part-5.md)
