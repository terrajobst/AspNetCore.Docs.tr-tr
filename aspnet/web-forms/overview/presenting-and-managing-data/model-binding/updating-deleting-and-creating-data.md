---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Güncelleştirme, silme ve verileri model bağlama ve web forms ile oluşturma | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: b6cafe29d1cb46061a8743cbee62a7ffec6be990
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393882"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Güncelleştirme, silme ve verileri model bağlama ve web forms ile oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Bu öğreticide, oluşturmak, güncelleştirmek ve model bağlama ile verileri silmek gösterilir. Aşağıdaki özellikleri ayarlar:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Bu özellikler, karşılık gelen işlemi işleyen yöntem adını alır. Bu yöntem içinde verilerle etkileşim kurmak için mantığı sağlar.
> 
> Bu öğreticide oluşturduğunuz ilk proje geliştirir [bölümü](retrieving-data.md) dizi.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

1. Dinamik veri şablonları ekleme
2. Model bağlama yöntemleri güncelleştirme ve silme veri etkinleştir
3. Veri doğrulama kuralları uygulamak - bir veritabanında yeni bir kayıt oluşturma etkinleştirin

## <a name="add-dynamic-data-templates"></a>Dinamik veri şablonları ekleme

Kod tekrarından en aza indirmek ve en iyi kullanıcı deneyimi sağlamak için dinamik veri şablonları kullanır. Bir NuGet paketini yükleyerek önceden oluşturulmuş dinamik veri şablonları mevcut sitenize kolayca tümleştirebilirsiniz.

Gelen **NuGet paketlerini Yönet**, yükleme **DynamicDataTemplatesCS**.

![dinamik veri şablonları](updating-deleting-and-creating-data/_static/image1.png)

Projeniz artık adlı bir klasör içeriyorsa bildirimi **DynamicData**. Bu klasörde otomatik olarak web formlarınızı dinamik denetimlere uygulanan şablonlar bulabilirsiniz.

![dinamik veri klasörü](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Etkinleştirme güncelleştirme ve silme

Kullanıcıların, güncelleştirme ve silme kayıtlarını veritabanında veri alma işlemine çok benzer. İçinde **UpdateMethod** ve **DeleteMethod** özellikleri, bu işlemleri gerçekleştiren yöntemler adlarını belirtin. Bir GridView denetimi Düzenle otomatik olarak oluşturulmasını belirtin ve Sil düğmeleri. Aşağıdaki vurgulanmış kodu GridView kod eklemeleri gösterir.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Kullanarak bir arka plan kod dosyasında, ekleme deyimini **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Ardından, şu güncelleştirmeyi ekleyin ve yöntemlerini silin.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** yöntemi web formu eşleşen verilere bağlı değerleri veri öğesi için geçerlidir. Veri öğesi dayalı kimliği parametresinin değeri olarak alınır.

## <a name="enforce-validation-requirements"></a>Doğrulama gereksinimlerini zorunlu

Verileri güncelleştirirken, Öğrenci sınıfında FirstName ve LastName yıl özelliklerine uygulanan doğrulama özniteliklerinin otomatik olarak uygulanır. İstemci ve sunucu doğrulayıcıları doğrulama özniteliklerine dayalı DynamicField denetimleri ekleyin. FirstName ve LastName özelliklerinin her ikisi de olduğundan gerekli. FirstName 20 karakterden uzun olamaz ve LastName 40 karakterden uzun olamaz. Yıl AcademicYear sabit listesi için geçerli bir değer olmalıdır.

Kullanıcı doğrulama gereksinimlerinden biri, ihlal edilirse, güncelleştirme devam etmez. Hata iletisini görmek için yukarıdaki GridView bir ValidationSummary denetimi ekleyin. Model bağlama doğrulama hatalarından görüntülenecek kümesi **ShowModelStateErrors** özelliğini **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Web uygulamasını çalıştırmak, güncelleştirme ve kayıtları silin.

![verileri güncelleştirme](updating-deleting-and-creating-data/_static/image3.png)

Düzenleme modunda yıl özelliğinin değeri otomatik olarak açılan bir liste olarak işlendiğinde dikkat edin. Year özelliği bir sabit listesi değeri ve bir açılan listeden düzenlemek için bir sabit listesi değeri için dinamik veri şablonu belirtir. Bu şablon açarak bulabilirsiniz **numaralandırma\_Edit.ascx** dosyası **DynamicData**/**FieldTemplates** klasör.

Geçerli değerler sağlarsanız, güncelleştirme başarıyla tamamlar. Doğrulama gereksinimlerini ihlal güncelleştirme devam etmiyor ve kılavuzun üzerindeki bir hata iletisi görüntülenir.

![hata iletisi](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Yeni kayıtlar ekleme

GridView denetiminde yer almaz **InsertMethod** özelliği ve bu nedenle yeni bir kayıt ile model bağlama eklemek için kullanılamaz. InsertMethod özelliğinde bulabilirsiniz **FormView**, **DetailsView**, veya **ListView** kontrol eder. Bu öğreticide, yeni bir kayıt eklemek için FormView denetimi kullanır.

İlk olarak, yeni bir kayıt eklemek için oluşturacağınız yeni sayfasına bağlantı ekleyin. Bir ValidationSummary ekleyin:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Yeni bağlantı içeriklerin Öğrenciler üstünde görünür.

![Yeni bağlantı](updating-deleting-and-creating-data/_static/image5.png)

Ardından, bir ana sayfa kullanan yeni bir web formu ekleyin ve adlandırın **AddStudent**. Site.Master ana sayfa olarak seçin.

Kullanarak yeni bir öğrenci eklemek için alanlar işleyecek bir **DynamicEntity** denetimi. Düzenlenebilir, Itemtype özelliğinde belirtilen sınıf özelliklerinde DynamicEntity denetimi oluşturur. StudentID özelliği ile işaretlenmiş **[ScaffoldColumn(false)]** değil işlenebilmesi özniteliği. MainContent yer tutucusu AddStudent sayfasının, aşağıdaki kodu ekleyin.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Arka plan kod dosyasında (AddStudent.aspx.cs) ekleme bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Ardından, yeni bir kayıt ve iptal düğmesi için bir olay işleyicisi ekleme belirtmek için aşağıdaki yöntemleri ekleyin.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Tüm değişiklikleri kaydedin.

Web uygulamasını çalıştırın ve yeni bir öğrenci oluşturun.

![Yeni bir öğrenci eklemek](updating-deleting-and-creating-data/_static/image6.png)

Tıklayın **Ekle** ve yeni Öğrenci oluşturulan dikkat edin.

![Yeni Öğrenci görüntüleme](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, güncelleştirme, silme ve verileri oluşturma etkin. Doğrulama kuralları verilerle etkileşim kurulurken uygulanır olmasını sağladı.

Sonraki [öğretici](sorting-paging-and-filtering-data.md) bu dizide, sıralama, sayfalama ve verileri filtreleme sağlayacaktır.

> [!div class="step-by-step"]
> [Önceki](retrieving-data.md)
> [İleri](sorting-paging-and-filtering-data.md)
