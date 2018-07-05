---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 8. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374058"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 8. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Biçimlendirmek ve verileri doğrulamak için dinamik veri işlevini kullanma

Önceki öğreticide saklı yordamlar uygulanır. Bu öğreticide nasıl dinamik veri işlevselliğini aşağıdaki faydaları sağlayabilir gösterilecek:

- Alanlar, kendi veri türüne göre görüntülemek için otomatik olarak biçimlendirilir.
- Alanlar, kendi veri türüne göre otomatik olarak doğrulanır.
- Meta veri biçimlendirme ve doğrulama davranışını özelleştirmek için veri modeline ekleyebilirsiniz. Bunu, biçimlendirme ve doğrulama kurallarını yalnızca tek bir yerde ekleyebilirsiniz ve otomatik olarak her yerde uygulandıkları kullanarak dinamik veri denetimlerine alanları erişin.

Bunun nasıl çalıştığını görmek için kullandığınız görüntülemek ve varolan alanları düzenlemek için denetimleri değiştireceksiniz *Students.aspx* sayfası, ekleyin ve biçimlendirme ve doğrulama meta verileri için ad ve tarih alanlarını `Student` varlık türü.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>DynamicField ve DynamicControl denetimleri kullanma

Açık *Students.aspx* sayfası ve `StudentsGridView` denetimi Değiştir **adı** ve **kayıt tarihi** `TemplateField` aşağıdaki işaretlemeyle öğeleri:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Bu işaretleme kullanır `DynamicControl` yerine denetimleri `TextBox` ve `Label` Öğrenci denetimlerinde şablon alan adı ve kullandığı bir `DynamicField` denetimi için kayıt tarihi. Belirtilen biçim dizesi yok.

Ekleme bir `ValidationSummary` sonra Denetim `StudentsGridView` denetimi.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

İçinde `SearchGridView` denetimi değiştirmek için biçimlendirme **adı** ve **kayıt tarihi** sütunlar halinde, yaptığınız `StudentsGridView` atlamak dışındaki denetlemek `EditItemTemplate` öğesi. `Columns` Öğesinin `SearchGridView` denetimi artık aşağıdaki biçimlendirme içerir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Açık *Students.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Sayfa için bir işleyici eklemek `Init` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Bu kod, dinamik veri biçimlendirme ve bu verilere bağlı denetimler alanları için doğrulama sağlayacaktır belirtir `Student` varlık. Sayfa çalıştırdığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsanız, bu genellikle çağrılacak Unutulan anlamına gelir `EnableDynamicData` yönteminde `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Sayfayı çalıştırın.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

İçinde **kayıt tarihi** sütun, özellik türü olduğundan saati tarih ile birlikte görüntülenir `DateTime`. Daha sonra değiştireceğiz.

Şimdilik, dinamik verileri otomatik olarak temel veri doğrulama sağlayan dikkat edin. Örneğin, **Düzenle**, tarih alanı temizleyin, tıklayın **güncelleştirme**, değeri veri modelinde null yapılabilir olmadığından dinamik verileri otomatik olarak bu gerekli bir alan aklınızdan görürsünüz. Alan ve bir hata iletisi sonra yıldız sayfası görüntüler `ValidationSummary` denetimi:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Atlarsanız `ValidationSummary` denetim fare işaretçisi hata iletisini görmek için yıldız işareti tutabilen çünkü:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dinamik veri de girilen verilerin doğrulama **kayıt tarihi** geçerli bir tarih alanıdır:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Gördüğünüz gibi genel bir hata iletisi budur. Sonraki bölümde, doğrulama ve biçimlendirme kuralları yanı sıra iletileri özelleştirmek nasıl öğreneceksiniz.

## <a name="adding-metadata-to-the-data-model"></a>Meta verileri veri modeline ekleme

Genellikle, dinamik veri tarafından sağlanan işlevselliği özelleştirmek istediğiniz. Örneğin, verilerin nasıl görüntüleneceğini ve hata iletileri içeriğini değişebilir. Genellikle de dinamik veri sağlanan korumanın değerinden daha fazla işlevsellik sağlamak için veri doğrulama kuralları otomatik olarak veri türlerine göre özelleştirin. Bunu yapmak için varlık türlerine karşılık gelen kısmi sınıflar oluşturun.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje, select **Başvuru Ekle**, bir başvuru ekleyin `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

İçinde *DAL* klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *Student.cs*, şablonu kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Bu kod için bir parçalı sınıf oluşturur `Student` varlık. `MetadataType` Bu kısmi sınıfa uygulanan öznitelik meta verileri belirtmek için kullanmakta olduğunuz sınıfı tanımlar. Meta veri sınıfının herhangi bir ad olabilir, ancak varlık adının yanı sıra "Metadata" kullanarak yaygın bir uygulamadır.

Biçimlendirme, doğrulama, kuralları ve hata iletileri için meta veri sınıfının özelliklerinde uygulanan öznitelikleri belirtin. Burada gösterilen öznitelikler aşağıdaki sonuçları olacaktır:

- `EnrollmentDate` bir tarih (olmadan, bir saat) olarak görüntülenir.
- Küçük uzunluğu ve özel hata iletisi sağlanan veya her iki ad alanlarını 25 karakter olmalıdır.
- Her iki ad alanları gereklidir ve bir özel hata iletisi sağlandı.

Çalıştırma *Students.aspx* yeniden sayfasında ve tarihleri, zamanları artık görüntülendiğini görürsünüz:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Bir satırı düzenleyin ve ad alanlarındaki değerleri temizlemek deneyin. Tıklatmadan önce bir alan bırakın hemen sonra alan hataları belirten yıldız görünür **güncelleştirme**. Tıkladığınızda **güncelleştirme**, sayfasında belirtilen hata iletisi metni görüntülenir.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

25 karakterden uzun adlara girmeyi deneyin, tıklayın **güncelleştirme**, ve sayfasında belirtilen hata iletisi metni görüntülenir.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Bu biçimlendirme ve doğrulama kurallarını veri modelinin meta verilerindeki ayarladığınız, kullandığınız sürece kuralları otomatik olarak her sayfada görüntüler veya bu alanlara değişikliklere izin uygulanacak `DynamicControl` veya `DynamicField` kontrol eder. Bu programlama ve sınamayı daha kolay hale getirir, yazmanız gereken gereksiz kod miktarını azaltır ve veri biçimlendirme ve doğrulama bir uygulamanın tamamında tutarlı olmasını güvence altına alır.

## <a name="more-information"></a>Daha fazla bilgi

Bu öğretici serisinde, Entity Framework ile çalışmaya başlama hakkında burada sona eriyor. Entity Framework kullanmayı öğrenmenize yardımcı olacak daha fazla kaynak için devam [sonraki Entity Framework öğretici serisinin ilk öğreticide](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) veya aşağıdaki siteleri ziyaret edin:

- [Entity Framework ile ilgili SSS](http://www.ef-faq.org/introduction.html)
- [Entity Framework ekip blogu](https://blogs.msdn.com/b/adonet/)
- [MSDN Kitaplığı'nda varlık çerçevesi](https://msdn.microsoft.com/library/bb399572.aspx)
- [Varlık çerçevesi veri MSDN Geliştirici Merkezi](https://msdn.microsoft.com/data/ef.aspx)
- [MSDN Kitaplığı'nda EntityDataSource Web sunucusu denetimine genel bakış](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource denetim MSDN Kitaplığı'nda API Başvurusu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework MSDN Forumlarında](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman'ın blogu](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-7.md)
