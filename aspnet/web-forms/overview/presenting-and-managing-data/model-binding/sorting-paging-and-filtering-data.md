---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sıralama, sayfalama ve model bağlama ve web forms ile verileri filtreleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 624f98cea6030e0b7b022f86c4c1aa37f1db9726
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020961"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sıralama, sayfalama ve model bağlama ve web forms ile verileri filtreleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Bu öğreticide, sıralama, sayfalama ve model bağlama aracılığıyla veri filtreleme nasıl ekleneceğini gösterir.
> 
> Bu öğreticide oluşturduğunuz ilk proje geliştirir [bölümü](retrieving-data.md) dizi.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

1. Verileri sayfalama ve sıralamayı etkinleştir
2. Kullanıcı tarafından bir seçimine göre veri filtreleme etkinleştir

## <a name="add-sorting"></a>Sıralama Ekle

GridView ' ' sıralama etkinleştirmek çok kolaydır. Student.aspx dosyasında ayarlamanız yeterlidir **AllowSorting** için **true** GridView içinde. Ayarlanacak gerekmez bir **SortExpression** DataField otomatik olarak kullanılan her sütun için değer. GridView sorguyu verileri seçilen değere göre sıralama içerecek şekilde değiştirir. Aşağıdaki vurgulanmış kodu sıralamayı etkinleştirmek yapmanız gereken ek gösterir.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Web uygulamasını çalıştırmak ve sıralama Öğrenci kayıtları farklı sütundaki değerleri sınayın.

![Sıralama öğrenciler](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Sayfalama ekleme

Disk belleği etkinleştirme de çok kolaydır. GridView ' ayarlayın **AllowPaging** özelliğini **true** ayarlayıp **PageSize** özelliğini istediğiniz her sayfada görüntülenecek kayıt sayısı. Bu öğreticide, 4'e ayarlayabilirsiniz.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Web uygulamasını çalıştırın ve kayıtları üzerinde birden çok sayfada tek bir sayfada görüntülenen en fazla 4 kayıtlarla ayrılır artık dikkat edin.

![Sayfalama ekleme](sorting-paging-and-filtering-data/_static/image4.png)

Ertelenen sorgu yürütme uygulama verimliliğini artırır. Tüm veri kümesini almak yerine, yalnızca geçerli sayfa için kayıtları almak için sorgu GridView değiştirir.

## <a name="filter-records-by-user-selection"></a>Kayıtları kullanıcı seçime göre filtre uygula

Model bağlama bir model bağlama yöntemin bir parametresinin değeri ayarlamak nasıl atamak sağlayan birkaç öznitelik ekler. Bu öznitelikler bulunan **System.Web.ModelBinding** ad alanı. Bunlara aşağıdakiler dahildir:

- Denetim
- Tanımlama bilgisi
- Form
- Profil
- QueryString
- Routedata öğesi
- Oturum
- Kullanıcı profili
- Görünüm durumu

Bu öğreticide, bir denetimin değeri içinde GridView kayıtları filtrelemek için kullanır. Ekleyeceksiniz **denetimi** sorgu yöntemine daha önce oluşturduğunuz özniteliği. İçinde bir [daha sonra](using-query-string-values-to-retrieve-data.md) öğreticide uygulanacaktır **QueryString** parametre değeri bir sorgu dizesi değerinden geldiğini belirtmek üzere bir parametre özniteliği.

İlk olarak, ValidationSummary bir açılan listeden filtreleme için hangi Öğrenciler gösterilen ekleyin.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Arka plan kod dosyasında, bir değer denetiminden almak için select yöntemi değiştirin ve parametre adı değeri sağlayan denetimin adına ayarlayın.

Eklemelisiniz bir **kullanarak** bildirimi **System.Web.ModelBinding** denetim özniteliği çözmek için ad alanı.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Aşağıdaki kod, aşağı açılan listeden değere göre döndürülen verileri filtrelemek için yeniden çalışılan select yöntemi gösterir. Bu parametre için değer aynı ada sahip bir denetimden gelen bir parametre belirtir. önce bir denetim özniteliği ekleniyor.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Web uygulamasını çalıştırın ve aşağı açılan listeden Öğrenciler listesine filtre uygulamak için farklı değerler seçin.

![Filtre Öğrenciler](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, sıralama ve veri disk belleği etkin. Ayrıca, bir denetimin değeri tarafından verileri filtreleme etkin.

Sonraki [öğretici](integrating-jquery-ui.md) kullanıcı arabirimini dinamik veri şablona JQuery kullanıcı Arabirimi pencere öğesi tümleştirerek ekleyeceksiniz.

> [!div class="step-by-step"]
> [Önceki](updating-deleting-and-creating-data.md)
> [İleri](integrating-jquery-ui.md)
