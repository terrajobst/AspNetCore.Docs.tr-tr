---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Güncelleştirme, silme ve veri model bağlama ve web forms ile oluşturma | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885851"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Güncelleştirme, silme ve veri model bağlama ve web forms ile oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Bu öğretici, oluşturmak, güncelleştirmek ve verileri model bağlamayla silmek gösterilmektedir. Aşağıdaki özellikler ayarlayacak:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Bu özellikleri karşılık gelen işlemi işleyen yöntem adını alır. Bu yöntem içinde veri ile etkileşmek için mantığı sağlar.
> 
> Bu öğretici ilk oluşturulan proje inşa edilmiştir [bölümü](retrieving-data.md) dizi.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide, gerekir:

1. Dinamik veri şablonları Ekle
2. Model bağlama yöntemlerle güncelleştirme ve silme verilerini etkinleştir
3. Veri doğrulama kuralları uygula - veritabanında yeni bir kayıt oluşturma etkinleştirin

## <a name="add-dynamic-data-templates"></a>Dinamik veri şablonları Ekle

En iyi kullanıcı deneyimi sağlamak ve kod yineleme en aza indirmek için dinamik veri şablonları kullanır. NuGet paketini yükleyerek var olan sitenize önceden derlenmiş dinamik veri şablonları kolayca tümleştirebilirsiniz.

Gelen **NuGet paketlerini Yönet**, yükleme **DynamicDataTemplatesCS**.

![dinamik veri şablonları](updating-deleting-and-creating-data/_static/image1.png)

Projenizi şimdi adlı bir klasör içerir bildirimi **DynamicData**. Bu klasörde, web forms Dinamik denetimleri için otomatik olarak uygulanan şablonları bulabilirsiniz.

![dinamik veri klasörü](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Etkinleştirme güncelleştirme ve silme

Güncelleştirme ve veritabanındaki kayıtları silme sağlama verilerini alma işlemi için çok benzer. İçinde **UpdateMethod** ve **DeleteMethod** özellikleri, o işlemler gerçekleştiren yöntemleri adlarını belirtin. GridView denetimi ile düzenleme otomatik olarak oluşturulmasını belirtin ve düğmeleri silin. Aşağıdaki vurgulanmış kodu GridView kod eklemeler gösterir.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Kullanarak bir arka plan kod dosyasına ekleyin bildirimi **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Ardından, aşağıdaki güncelleştirmeyi ekleyin ve yöntemlerini silin.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** yöntemi web formundan eşleşen veri bağlama değerleri veri öğesi için geçerlidir. Veri öğesi kimliği parametresinin değeri göre alınır.

## <a name="enforce-validation-requirements"></a>Doğrulama gereksinimlerini zorla

Öğrenci sınıfı FirstName, LastName ve yıl özelliklerinde uygulanan doğrulama özniteliklerinin verileri güncelleştirilirken otomatik olarak uygulanır. DynamicField denetimleri doğrulama özniteliklerini temel alarak istemci ve sunucu doğrulayıcıları ekleyin. Ad ve Soyadı her ikisi de özelliklerdir gerekli. Ad uzunluğu 20 karakterden uzun olamaz ve LastName 40 karakteri aşamaz. Yıl AcademicYear numaralandırma için geçerli bir değer olmalıdır.

Kullanıcı doğrulama gereksinimlerini bozup güncelleştirme devam etmez. Hata iletisini görmek için ValidationSummary denetimi GridView üstüne ekleyin. Model bağlama doğrulama hatalarından görüntülenecek ayarlamak **ShowModelStateErrors** özelliğini **doğru**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Web uygulamasını çalıştırmak, güncelleştirme ve kayıtları silin.

![Verileri güncelleştirme](updating-deleting-and-creating-data/_static/image3.png)

Düzenleme modunda yıl özelliğinin değeri otomatik olarak açılır listeden olarak işlendiğinde dikkat edin. Year özelliği bir numaralandırma değeridir ve açılan düzenlemek için listeden bir numaralandırma değeri için dinamik veri şablonu belirtir. Bu şablonu açarak bulabileceğiniz **numaralandırma\_Edit.ascx** dosyasını **DynamicData**/**FieldTemplates** klasör.

Geçerli değerler sağlarsanız, güncelleştirme başarıyla tamamlanır. Doğrulama gereksinimlerini birini ihlal güncelleştirme devam etmez ve kılavuz bir hata iletisi görüntülenir.

![Hata iletisi](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Yeni kayıtlar ekleme

GridView denetiminin içermemesi **InsertMethod** özelliği ve bu nedenle model bağlama ile yeni bir kayıt eklemek için kullanılamaz. InsertMethod özelliğinde bulabilirsiniz **FormView**, **DetailsView**, veya **ListView** kontrol eder. Bu öğreticide, yeni bir kayıt eklemek için FormView denetimi kullanır.

İlk olarak, yeni bir kayıt eklemek için oluşturacağınız yeni sayfaya bir bağlantı ekleyin. ValidationSummary ekleyin:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Yeni bağlantı Öğrenciler sayfasının içeriğinin üstünde görünür.

![Yeni bağlantı](updating-deleting-and-creating-data/_static/image5.png)

Ardından, bir ana sayfa kullanarak yeni bir web formu ekleyin ve adını **AddStudent**. Site.Master ana sayfa olarak seçin.

Kullanarak yeni bir öğrenci eklemek için alanlarını kılacak bir **DynamicEntity** denetim. DynamicEntity denetim ItemType özelliğinde belirtilen sınıfında düzenlenebilir bu özellikleri işler. StudentID özelliği ile işaretlenmiş **[ScaffoldColumn(false)]** değil işlenen şekilde özniteliği. MainContent yer tutucu AddStudent sayfasında, aşağıdaki kodu ekleyin.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Arka plan kod dosyasına (AddStudent.aspx.cs) ekleyin bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Ardından, yeni bir kayıt ve iptal düğmesi olay işleyicisi ekleme belirtmek için aşağıdaki yöntemleri ekleyin.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Tüm değişiklikleri kaydedin.

Web uygulamasını çalıştırın ve yeni Öğrenci oluşturun.

![Yeni Öğrenci ekleme](updating-deleting-and-creating-data/_static/image6.png)

Tıklatın **Ekle** ve yeni Öğrenci oluşturulan dikkat edin.

![Yeni Öğrenci görüntüleme](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, güncelleştirme, silme ve veri oluşturma etkin. Doğrulama kuralları verilerle kullanılırken uygulanır güvence altına.

Sonraki [öğretici](sorting-paging-and-filtering-data.md) bu dizide, disk belleği, verileri sıralama ve filtreleme olanak sağlar.

> [!div class="step-by-step"]
> [Önceki](retrieving-data.md)
> [sonraki](sorting-paging-and-filtering-data.md)
