---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, 3. Bölüm: sıralama ve filtreleme | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, kullanmaya başlama Entity Framework 4.0 öğretici serisinin tarafından oluşturulan Contoso University web uygulaması oluşturur. BEN...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 71e26dc9a63131842daf4aa1549d761690b723ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755519"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, 3. Bölüm: sıralama ve filtreleme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinde Contoso University web uygulaması tarafından oluşturulan geliştirir [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticilerde tamamlanmadıysa, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturmuş olduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici serisinin tarafından oluşturulur. Öğreticileri hakkında sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide Entity Framework kullanan bir n katmanlı web uygulamasında depo deseni uygulanan ve `ObjectDataSource` denetimi. Bu öğreticide, sıralama ve filtreleme yapmak ve ana öğe-ayrıntı senaryoları işlemek gösterilir. Aşağıdaki gibi iyileştirmeyi ekleyeceksiniz *Departments.aspx* sayfası:

- Departmanlar adına göre seçmek kullanıcılara izin vermek için metin kutusu.
- Kılavuzda gösterilen her departman kursları listesi.
- Sütun başlıklarını tıklatarak sıralama olanağı.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>GridView Sütun sıralama özelliği ekleme

Açık *Departments.aspx* sayfa ve ekleme bir `SortParameterName="sortExpression"` özniteliğini `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`. (Daha sonra oluşturacağınız bir `GetDepartments` adlı bir parametresi alan yöntemi `sortExpression`.) Açılış etiketinde denetimi için biçimlendirme artık aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Ekleme `AllowSorting="true"` özniteliği öğesinin açılış etiketinde `GridView` denetimi. Açılış etiketinde denetimi için biçimlendirme artık aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

İçinde *Departments.aspx.cs*, çağırarak varsayılan sıralama düzenini ayarlayın `GridView` denetimin `Sort` yönteminden `Page_Load` yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

İş mantığı sınıfı veya depo sınıfını sıralar veya filtreleri kod ekleyebilirsiniz. İş mantığı sınıfı, bunu yaparsanız, veritabanından verilerin alındıktan sonra ile iş mantığı sınıfı çalıştığından sıralama veya filtreleme iş yapılır bir `IEnumerable` deposu tarafından döndürülen nesne. Sıralama ve filtreleme depo sınıfını kodda ekleyin ve bir LINQ ifadesini önce bunu ya da nesne sorgusu için dönüştürülmüş bir `IEnumerable` komutlarınızı geçirilir aracılığıyla veritabanı, genellikle daha verimli bir işlem için nesne. Bu öğreticide, sıralama ve filtreleme işlemi veritabanı tarafından yapılacak neden olan bir yolla uygulayacaksınız — diğer bir deyişle, depodaki.

Sıralama özelliği eklemek için depo arabirimi ve depo sınıfları da iş mantığı sınıfı için yeni bir yöntem eklemeniz gerekir. İçinde *ISchoolRepository.cs* dosya, yeni bir `GetDepartments` gereken yöntemini bir `sortExpression` döndürülen bölümleri sıralamak için kullanılacak parametre:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Sıralanacak sütunu ve sıralama yönünü parametre belirtin.

Yeni bir yönteme için kod ekleme *SchoolRepository.cs* dosyası:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Parametresiz varolan değiştirme `GetDepartments` yeni yöntemini çağırmak için yöntem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Test projesinde aşağıdaki yeni yöntemi ekleyin *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Bu metot üzerinde sıralanmış listesini döndüren bir işleve bağımlı herhangi bir birim test oluşturacağınız döndürmeden önce listeyi sıralamak gerekir. Yöntemi yalnızca bölümlerin Sıralanmamış liste dönebilmeniz testleri gibi Bu öğreticide, oluşturduğunuz gerekmez.

İçinde *SchoolBL.cs* iş mantığı sınıfına aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Bu kod sıralama parametresi depo yönteme geçirir.

Çalıştırma *Departments.aspx* sayfası.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Artık, herhangi bir sütuna göre sıralamak için sütunun başlığına da tıklayabilirsiniz. Sütun zaten sıraladıysanız başlığına tıklayarak sıralama yönünü tersine çevirir.

## <a name="adding-a-search-box"></a>Bir arama kutusu ekleme

Bu bölümde, arama metin kutusu ekleme, gerekir bağlantısına `ObjectDataSource` denetim parametresini kullanarak denetlemek ve Filtrelemeyi desteklemek üzere iş mantığı sınıfına bir yöntem ekleyin.

Açık *Departments.aspx* sayfasında ve başlık ve ilk arasında aşağıdaki işaretlemeyi ekleyin `ObjectDataSource` denetimi:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

İçinde `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`, aşağıdakileri yapın:

- Ekleme bir `SelectParameters` adlı bir parametre için öğe `nameSearchString` girilen değerin alır `SearchTextBox` denetimi.
- Değişiklik `SelectMethod` öznitelik değerine `GetDepartmentsByName`. (Daha sonra bu yöntem oluşturacaksınız.)

İşaretleme için `ObjectDataSource` denetimi artık aşağıdaki örnekte benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

İçinde *ISchoolRepository.cs*, ekleme bir `GetDepartmentsByName` yönteminin hem de alan `sortExpression` ve `nameSearchString` parametreleri:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

İçinde *SchoolRepository.cs*, aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Bu kod bir `Where` arama dizesini içeren öğelerini seçmek için yöntemi. Arama dizesi boş ise, tüm kayıtları seçilir. Yöntem çağrıları birlikte böyle bir ifade belirttiğinizde unutmayın (`Include`, ardından `OrderBy`, ardından `Where`), `Where` yöntemi her zaman son olmalıdır.

Varolan değiştirme `GetDepartments` gereken yöntemini bir `sortExpression` parametresini kullanarak yeni yöntemi çağırın:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

İçinde *MockSchoolRepository.cs* test projesinde aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

İçinde *SchoolBL.cs*, aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Çalıştırma *Departments.aspx* sayfasında ve seçim mantığını çalıştığından emin olmak için bir arama dizesini girin. Metin kutusunu boş bırakın ve tüm kayıtlar döndürülür emin olmak için bir arama deneyin.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Ayrıntılar sütunu için her bir ızgara satır ekleme

Ardından, tüm kursları kılavuzunun sağ hücrede görüntülenen her departman için görmek istiyorsunuz. Bunu yapmak için iç içe bir kullanacağınız `GridView` denetim ve veri bağlama için verileri `Courses` gezinti özelliği `Department` varlık.

Açık *Departments.aspx* ve için biçimlendirme `GridView` denetimi bir işleyici belirtmek için `RowDataBound` olay. Açılış etiketinde denetimi için biçimlendirme artık aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Yeni bir `TemplateField` öğeden sonra `Administrator` şablonu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Bu işaretleme iç içe bir oluşturur `GridView` kurs sayısını ve başlığını kursları listesini gösteren denetimdir. Databind gerekir çünkü bir veri kaynağı belirtmiyor kod içinde `RowDataBound` işleyici.

Açık *Departments.aspx.cs* ve eklemek için aşağıdaki işleyicisi `RowDataBound` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Bu kod alır `Department` olay bağımsız değişkenleri varlıktan dönüştürür `Courses` gezinme özelliği için bir `List` koleksiyona ve iç içe databinds `GridView` koleksiyonuna.

Açık *SchoolRepository.cs* istekli yükleme için belirtin ve dosya `Courses` çağırarak gezinti özelliği `Include` oluşturduğunuz nesne sorgusu yönteminde `GetDepartmentsByName` yöntemi. `return` Deyiminde `GetDepartmentsByName` yöntemi artık aşağıdaki örnekte benzer.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Sayfayı çalıştırın. Sıralama ve filtreleme, daha önce eklediğiniz özelliği yanı sıra GridView denetiminde artık her departman için iç içe geçmiş kurs ayrıntıları gösterir.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Bu sıralama, filtreleme ve ana öğe-ayrıntı senaryoları giriş tamamlar. Sonraki öğreticide eşzamanlılık nasıl ele alınacağını görürsünüz.

> [!div class="step-by-step"]
> [Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [İleri](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
