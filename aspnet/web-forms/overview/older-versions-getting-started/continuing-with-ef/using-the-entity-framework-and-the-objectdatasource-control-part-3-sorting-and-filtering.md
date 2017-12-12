---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, Bölüm 3: sıralama ve filtreleme | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri ile çalışmaya başlama Entity Framework 4.0 öğretici serisi tarafından oluşturulan Contoso University web uygulaması üzerinde oluşturur. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Entity Framework 4.0 ve ObjectDataSource denetimi kullanarak, Bölüm 3: sıralama ve filtreleme
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Bu öğretici seri tarafından oluşturulan Contoso University web uygulaması üzerinde derlemeler [Entity Framework 4.0 ile çalışmaya başlama](https://asp.net/entity-framework/tutorials#Getting%20Started) öğretici serisi. Önceki öğreticileri tamamlanmadı, Bu öğretici için bir başlangıç noktası olarak yapabilecekleriniz [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) oluşturduğunuz. Ayrıca [uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tam öğretici seri tarafından oluşturulur. Öğreticiler hakkında sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx).


Önceki öğreticide Entity Framework kullanan bir n katmanlı web uygulama havuzu desende uygulanan ve `ObjectDataSource` denetim. Bu öğretici, sıralama ve filtreleme yapabilir ve ana-ayrıntı senaryolarını ele gösterilmektedir. Aşağıdaki geliştirmeleri ekleyeceksiniz *Departments.aspx* sayfa:

- Departmanlar adına göre seçmek yapmalarına izin vermek için bir metin kutusu.
- Kılavuzda gösterilen her bölüm için kurslar listesi.
- Sütun başlıklarını tıklatarak sıralama olanağı.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>GridView sütunları sıralama özelliği ekleme

Açık *Departments.aspx* sayfasında ve ekleme bir `SortParameterName="sortExpression"` özniteliğini `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`. (Daha sonra oluşturacağınız bir `GetDepartments` yönteminin adlı bir parametre alan `sortExpression`.) Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Ekleme `AllowSorting="true"` özniteliği için açılış etiketinde `GridView` denetim. Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

İçinde *Departments.aspx.cs*, varsayılan sıralama düzeni aranarak `GridView` denetimin `Sort` yönteminden `Page_Load` yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

İş mantığı sınıfı veya depo sınıfını sıralar veya filtreler kod ekleyebilirsiniz. İş mantığı sınıfında bunu yaparsanız, verileri veritabanından alındıktan sonra iş mantığı sınıfı ile çalıştığından sıralama veya filtreleme iş yapılır bir `IEnumerable` depo tarafından döndürülen nesne. Sıralama ve filtreleme depo sınıfını kodda ekler ve LINQ ifadesi önce yapın ya da sorgu nesnesi için dönüştürülmüş bir `IEnumerable` nesnesi, komutlarınızı geçirilecektir aracılığıyla, genellikle daha verimli bir işlem için veritabanı. Bu öğreticide, sıralama ve veritabanı tarafından yapılması işleme neden olan bir şekilde filtreleme uygulamadan — diğer bir deyişle, depodaki.

Sıralama özelliği eklemek için depo arabirimi ve depo sınıflarını da iş mantığı sınıfı için yeni bir yöntem eklemeniz gerekir. İçinde *ISchoolRepository.cs* dosya, yeni bir ekleme `GetDepartments` yönteminin alan bir `sortExpression` döndürülen Departmanlar sıralamak için kullanılacak parametre:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Parametresi sıralanacak sütunu ve sıralama yönünü belirtin.

Yeni bir yönteme için kodu ekleyin *SchoolRepository.cs* dosyası:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Parametresiz varolan değiştirmek `GetDepartments` yeni yöntemini çağırmak için yöntemi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Test projesinde aşağıdaki yeni yöntemi ekleyin *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Sıralanmış bir döndürme bu yöntem bağımlı olduğu tüm birim testleri oluşturmak için olmaya ederse döndürmeden önce sıralamak gerekir. Yöntemi yalnızca bölümlerin Sıralanmamış liste dönebilmeniz testleri gibi Bu öğreticide oluşturduğunuz olmaz.

İçinde *SchoolBL.cs* dosya, iş mantığı sınıfına aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Bu kod sıralama parametresi deposu yönteme geçirir.

Çalıştırma *Departments.aspx* sayfası.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Artık herhangi bir sütuna göre sıralamak için sütun başlığını tıklatabilirsiniz. Sütun zaten sıraladıysanız başlığını tıklatarak sıralama yönünü tersine çevirir.

## <a name="adding-a-search-box"></a>Bir arama kutusu ekleme

Bu bölümde, arama metin kutusuna eklemek için bağlantının `ObjectDataSource` denetim parametresini kullanarak denetlemek ve bir yöntem Filtrelemeyi desteklemek için iş mantığı sınıfına ekleyin.

Açık *Departments.aspx* sayfasında ve başlık ve ilk arasında aşağıdaki biçimlendirmeleri eklemek `ObjectDataSource` denetimi:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

İçinde `ObjectDataSource` adlı Denetim `DepartmentsObjectDataSource`, aşağıdakileri yapın:

- Ekleme bir `SelectParameters` öğesi adlı bir parametre için `nameSearchString` girilen değerin alır `SearchTextBox` denetim.
- Değişiklik `SelectMethod` öznitelik değeri için `GetDepartmentsByName`. (Daha sonra bu yöntem oluşturacaksınız.)

İşaretleme için `ObjectDataSource` denetim şimdi aşağıdaki örnekte benzer:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

İçinde *ISchoolRepository.cs*, ekleme bir `GetDepartmentsByName` yönteminin hem de alan `sortExpression` ve `nameSearchString` Parametreler:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

İçinde *SchoolRepository.cs*, aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Bu kodu kullanan bir `Where` yöntemi arama dizesini içeren öğeleri seçin. Arama dizesi boş ise, tüm kayıtlar seçilir. Yöntem çağrıları birlikte şöyle bir deyim belirttiğinizde unutmayın (`Include`, ardından `OrderBy`, ardından `Where`), `Where` yöntemi her zaman son olmalıdır.

Varolan değiştirme `GetDepartments` yönteminin alan bir `sortExpression` yeni yöntemini çağırmak için parametre:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

İçinde *MockSchoolRepository.cs* test projesinde aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

İçinde *SchoolBL.cs*, aşağıdaki yeni yöntemi ekleyin:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Çalıştırma *Departments.aspx* sayfasında ve seçim mantığı çalıştığından emin olmak için bir arama dizesi girin. Metin kutusunu boş bırakın ve tüm kayıtların döndürüldüğünden emin olmak için bir arama deneyin.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Her kılavuz satır için ayrıntıları sütun ekleme

Ardından, tüm kılavuz sağ hücrede görüntülenen her bölüm için kurslar görmek istiyorsunuz. Bunu yapmak için iç içe bir kullanacağınız `GridView` denetimi ve databind verilerden kendisine `Courses` Gezinti özelliğinin `Department` varlık.

Açık *Departments.aspx* ve için biçimlendirme `GridView` denetlemek, bir işleyici belirtmek için `RowDataBound` olay. Denetimin açılış etiketi için biçimlendirme şimdi aşağıdaki örneğe benzer.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Yeni bir ekleme `TemplateField` öğeden sonra `Administrator` şablonu alan:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Bu biçimlendirme iç içe bir oluşturur `GridView` denetim indirmelere numarası ve başlığı kurslar listesini gösterir. Databind gerekir çünkü bir veri kaynağı belirtmiyor kodda içinde `RowDataBound` işleyicisi.

Açık *Departments.aspx.cs* ve aşağıdaki işleyicisi ekleme `RowDataBound` olay:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Bu kod alır `Department` olay bağımsız varlıktan dönüştürür `Courses` gezinti özelliği için bir `List` toplama ve iç içe databinds `GridView` koleksiyonuna.

Açık *SchoolRepository.cs* dosya ve istekli yükleme için belirtmek `Courses` çağırarak gezinti özelliği `Include` oluşturduğunuz sorgu nesnesi yönteminde `GetDepartmentsByName` yöntemi. `return` Deyiminde `GetDepartmentsByName` yöntemi şimdi aşağıdaki örnekte benzer.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Sayfayı çalıştırın. Sıralama ve filtreleme daha önce eklediğiniz yetenek ek olarak, GridView denetiminin şimdi her bölüm için iç içe geçmiş indirmelere ayrıntıları gösterir.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Bu, sıralama, filtreleme ve ana-ayrıntı senaryoları giriş tamamlar. Sonraki öğreticide eşzamanlılık nasıl ele alınacağını görürsünüz.

>[!div class="step-by-step"]
[Önceki](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[sonraki](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
