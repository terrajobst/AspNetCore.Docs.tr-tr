---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sıralama, disk belleği ve model bağlama ve web forms ile veri filtreleme | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885412"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sıralama, disk belleği ve model bağlama ve web forms ile verileri filtreleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Bu öğretici, sıralama, disk belleği ve model bağlama aracılığıyla verileri filtreleme nasıl ekleneceğini gösterir.
> 
> Bu öğretici ilk oluşturulan proje inşa edilmiştir [bölümü](retrieving-data.md) dizi.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide, gerekir:

1. Sıralama ve verilerin disk belleği etkinleştir
2. Kullanıcı tarafından seçime dayalı veri filtreleme etkinleştir

## <a name="add-sorting"></a>Sıralama Ekle

GridView sıralama etkinleştirme çok kolaydır. Student.aspx dosyasında sadece ayarlanan **AllowSorting** için **true** GridView içinde. Ayarlanacak gerekmez bir **SortExpression** DataField otomatik olarak kullanılan her sütun için değer. GridView sorguyu seçili değeriyle verileri sıralama içerecek şekilde değiştirir. Aşağıda vurgulanan kod sıralamayı etkinleştirmek yapmanız gereken ek gösterir.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Web uygulamasını çalıştırın ve sıralama Öğrenci kayıtlarını farklı sütunlardaki değerleri sınayın.

![Sıralama öğrenciler](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>disk belleği ekleme

Disk belleği etkinleştirme ayrıca çok kolaydır. GridView, ayarlamak **AllowPaging** özelliğine **true** ve **PageSize** istediğiniz her bir sayfasında görüntülenecek kayıt sayısını özelliğine. Bu öğreticide, 4'e ayarlayabilirsiniz.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Web uygulamasını çalıştırmak ve kayıtları tek bir sayfada görüntülenen en fazla 4 kayıtlarla birden çok sayfa ayrılır artık dikkat edin.

![disk belleği ekleme](sorting-paging-and-filtering-data/_static/image4.png)

Ertelenmiş sorgu yürütme uygulama verimliliğini artırır. GridView, tüm veri kümesinin alınıyor yerine, yalnızca geçerli sayfa için kayıtları almaya yönelik sorgu değiştirir.

## <a name="filter-records-by-user-selection"></a>Kullanıcı seçime göre filtre kayıtları

Model bağlama bir model bağlama yönteminde bir parametre için değer ayarlamak nasıl tanımlamanızı sağlayan birkaç öznitelik ekler. Bu öznitelikler bulunan **System.Web.ModelBinding** ad alanı. Bunlara aşağıdakiler dahildir:

- Denetim
- Tanımlama bilgisi
- Form
- Profil
- QueryString
- Routedata öğesi
- Oturum
- UserProfile
- Görünüm durumu

Bu öğretici kapsamında, hangi kayıtların GridView görüntüleneceğini filtrelemek için bir denetimin değeri kullanır. Ekleyeceksiniz **denetim** özniteliğine daha önce oluşturduğunuz sorgu yöntemi. İçinde bir [daha sonra](using-query-string-values-to-retrieve-data.md) öğretici, uygulanacaktır **QueryString** öznitelik parametre değeri bir sorgu dizesi değerinden geldiğini belirtmek için bir parametre.

İlk olarak, bir açılır listeyi filtrelemek için hangi Öğrenciler gösterilen ValidationSummary ekleyin.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Arka plan kod dosyasına denetimden bir değer almasını select yöntemini değiştirin ve parametresinin adı değeri sağlayan denetimi adına ayarlayın.

Eklemelisiniz bir **kullanarak** bildirimi **System.Web.ModelBinding** denetim özniteliği çözümlemek için ad alanı.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Aşağıdaki kod döndürülen verileri aşağı açılan listeden değere göre filtre uygulamak için yeniden çalışılan seçme yöntemini gösterir. Bir parametre değeri bu parametre için aynı ada sahip bir denetiminden geldiğini belirten önce bir denetim özniteliği ekleniyor.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Web uygulamasını çalıştırın ve açılır listeden Öğrenciler listesini filtrelemek için farklı değerler seçin.

![Filtre Öğrenciler](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, sıralama ve verilerin sayfalama etkinleştirilir. Ayrıca verileri denetim değeriyle filtreleme etkin.

Sonraki [öğretici](integrating-jquery-ui.md) dinamik veri şablonuna JQuery UI pencere öğesi tümleştirerek UI ekleyeceksiniz.

> [!div class="step-by-step"]
> [Önceki](updating-deleting-and-creating-data.md)
> [sonraki](integrating-jquery-ui.md)
