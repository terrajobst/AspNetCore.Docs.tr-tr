---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölümü 8 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 78a54fb1b0e4c0ebd5445cca175103471925a065
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölümü 8
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Biçimlendirmek ve verileri doğrulamak için dinamik veri işlevlerini kullanma

Önceki öğreticide saklı yordamlar uygulanmadı. Bu öğretici nasıl dinamik veri işlevlerini aşağıdaki faydaları sağlayabilir gösterir:

- Alanlar, kendi veri türüne göre görüntülemek için otomatik olarak biçimlendirilir.
- Alanları otomatik olarak kendi veri türüne göre doğrulanır.
- Meta veri biçimlendirmeyi ve doğrulama davranışını özelleştirmek için veri modeli ekleyebilirsiniz. Bunu yapmak için yalnızca tek bir yerde biçimlendirmeyi ve doğrulama kuralları ekleyebilirsiniz ve bunlar otomatik olarak her yerde uygulanan zaman dinamik veri denetimleri kullanarak alanlara erişim.

Bunun nasıl çalıştığını görmek için kullandığınız görüntülemek ve varolan alanları düzenlemek için denetimleri değiştireceğiz *Students.aspx* sayfası ve ekleyeceğiz biçimlendirmeyi ve doğrulama meta verileri için ad ve tarih alanlarını `Student` varlık türü.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>DynamicField ve DynamicControl denetimleri kullanma

Açık *Students.aspx* sayfa ve `StudentsGridView` denetimi Değiştir **adı** ve **kayıt tarihi** `TemplateField` aşağıdaki biçimlendirme ile öğeleri:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Bu biçimlendirme kullanan `DynamicControl` yerine denetimleri `TextBox` ve `Label` Öğrenci denetimlerinde şablon alan adı ve kullanır bir `DynamicField` denetimi için kayıt tarihi. Biçim dizeleri belirtilir.

Ekleme bir `ValidationSummary` sonra Denetim `StudentsGridView` denetim.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

İçinde `SearchGridView` denetim değiştirmek için biçimlendirme **adı** ve **kayıt tarihi** sütunları, olarak yaptığınız `StudentsGridView` atlayın dışında kontrol `EditItemTemplate` öğesi. `Columns` Öğesinin `SearchGridView` denetimi şimdi aşağıdaki biçimlendirme içerir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Açık *Students.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Sayfa için bir işleyici ekleyin `Init` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Bu kod dinamik veri biçimlendirme ve bu verilere bağlı denetimler alanları için doğrulama sağlayacak belirtir `Student` varlık. Sayfa çalıştırdığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsanız, bu genellikle çağırmayı unuttunuz anlamına gelir `EnableDynamicData` yönteminde `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Sayfayı çalıştırın.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

İçinde **kayıt tarihi** sütun, özellik türü olduğundan süresi tarihi ile birlikte görüntülenir `DateTime`. Daha sonra düzeltmesi.

Şu an için dinamik veri otomatik olarak temel veri doğrulama sağlar dikkat edin. Örneğin, **Düzenle**, tarih alanını temizleyin, tıklatın **güncelleştirme**, ve değeri veri modelinde boş değer atanabilir olmadığından dinamik veri otomatik olarak bu gerekli bir alan sağlar, bkz. Alan ve bir hata iletisi sonra yıldız sayfasını görüntüler `ValidationSummary` denetimi:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Atlayın `ValidationSummary` denetim fare işaretçisini hata iletisini görmek için yıldız tutabilir çünkü:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dinamik veri de girilen verilerin doğrulama **kayıt tarihi** alandır geçerli bir tarih:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Gördüğünüz gibi genel bir hata iletisi budur. Sonraki bölümde iletileri yanı sıra doğrulama ve biçimlendirme kuralları nasıl özelleştirileceği görürsünüz.

## <a name="adding-metadata-to-the-data-model"></a>Veri modeli için meta veri ekleme

Genellikle, dinamik veri tarafından sağlanan işlevleri özelleştirmek istediğiniz. Örneğin, verilerin nasıl görüntüleneceğini ve hata iletileri içeriği değiştirebilirsiniz. Genellikle ayrıca dinamik veri sağladıkları'den daha fazla işlevsellik sağlamak için veri doğrulama kuralları otomatik olarak veri türlerine göre özelleştirin. Bunu yapmak için varlık türleri için karşılık gelen kısmi sınıflar oluşturun.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje, select **Başvuru Ekle**ve bir başvuru ekleyin `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

İçinde *DAL* klasörü, yeni bir sınıf dosyası oluşturun, adlandırın *Student.cs*ve şablon kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Bu kod için bir parçalı sınıf oluşturur `Student` varlık. `MetadataType` Bu sınıfa uygulanan öznitelik meta verileri belirtmek için kullandığınız sınıfı tanımlar. Meta veri sınıfının herhangi bir ad olabilir, ancak varlık adı artı "Meta veri" kullanarak yaygın bir uygulamadır.

Meta veri sınıfının özelliklerine uygulanan öznitelikleri biçimlendirme, doğrulama, kuralları ve hata iletileri belirtin. Burada gösterilen öznitelikleri aşağıdaki sonuçları olacaktır:

- `EnrollmentDate`Tarih (olmadan bir saat) olarak görüntüler.
- Daha düşük bir değer uzunluğu ve bir özel hata iletisi sağlanan veya her iki ad alanlarını 25 karakter olmalıdır.
- Her iki ad alanları gereklidir ve bir özel hata iletisi sağlanır.

Çalıştırma *Students.aspx* yeniden sayfasında ve tarihleri kez şimdi görüntülenen bakın:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Bir satır düzenleyin ve ad alanlarının temizlemek deneyin. Tıklatmadan önce bir alanı bırakın hemen alan hataları belirten bir yıldız işareti görünür **güncelleştirme**. Tıkladığınızda **güncelleştirme**, belirttiğiniz hata iletisi metni sayfasını görüntüler.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

25 karakterden daha uzun ad girmeyi deneyin, tıklatın **güncelleştirme**, ve belirttiğiniz hata iletisi metni sayfasını görüntüler.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Bu biçimlendirmeyi ve doğrulama kuralları veri modelinin meta verilerindeki kurdu, kullandığınız sürece otomatik olarak bu alanlara değişikliğe izin verdiğinden ya da görüntüleyen her sayfada uygulanacak kurallar `DynamicControl` veya `DynamicField` kontrol eder. Bu programlama ve sınamayı daha kolay hale getirir, yedekli kod yazmak zorunda miktarını azaltır ve veri biçimlendirmeyi ve doğrulama bir uygulama genelinde tutarlı olmasını sağlar.

## <a name="more-information"></a>Daha Fazla Bilgi

Bu öğretici Entity Framework ile çalışmaya başlama dizi sonlanır. Entity Framework kullanmayı öğrenmenize yardımcı olacak daha fazla kaynak için devam [sonraki Entity Framework öğretici serideki ilk öğreticide](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) veya aşağıdaki siteleri ziyaret edin:

- [Entity Framework ile ilgili SSS](http://www.ef-faq.org/introduction.html)
- [Entity Framework ekip blogu](https://blogs.msdn.com/b/adonet/)
- [MSDN Kitaplığı'nda Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572.aspx)
- [Entity Framework MSDN veri Geliştirici Merkezi](https://msdn.microsoft.com/en-us/data/ef.aspx)
- [MSDN Kitaplığı'nda EntityDataSource Web sunucusu denetimine genel bakış](https://msdn.microsoft.com/en-us/library/cc488502.aspx)
- [EntityDataSource denetim MSDN Kitaplığı'nda API Başvurusu](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN'de Entity Framework forumları](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/)
- [Julie Lerman'ın blogu](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[Önceki](the-entity-framework-and-aspnet-getting-started-part-7.md)
