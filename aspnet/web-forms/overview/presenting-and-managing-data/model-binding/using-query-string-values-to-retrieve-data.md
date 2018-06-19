---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Model bağlama ile verileri filtrelemek ve web formları için sorgu dizesi değerlerini kullanarak | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886829"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Filtre veri için sorgu dizesi değerlerini model bağlama ve web forms ile kullanma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Bu öğretici, sorgu dizesinde bir değer geçirmek ve model bağlama üzerinden veri almak için bu değeri kullanın gösterilmektedir.
> 
> Bu öğreticide oluşturduğunuz proje inşa edilmiştir [önceki](retrieving-data.md) seri bölümlerini.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide, gerekir:

1. Öğrencinin için kayıtlı kurslar göstermek için yeni bir sayfa ekleyin
2. Sorgu dizesindeki bir değere göre seçilen Öğrenci için kayıtlı kurslar alma
3. Sorgu dize değerine sahip bir köprüyü kılavuz görünümünden yeni sayfasına ekleyin.

Bu öğreticideki adımlardan yaptıklarınızın için oldukça benzer önceki içinde [öğretici](sorting-paging-and-filtering-data.md) görüntülenen Öğrenciler açılır listeden kullanıcı seçime göre filtre uygulamak için. Bu öğreticide kullandığınız **denetim** parametre değeri denetimden geldiğini belirtmek için select yöntemi özniteliği. Bu öğreticide kullanacaksınız **QueryString** parametre değeri Sorgu dizesinden geldiğini belirtmek için select yöntemi özniteliği.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Öğrencinin kurslar görüntülemek için yeni bir sayfa ekleyin

Site.master ana sayfayı kullanan yeni bir web formu ekleyin ve sayfanın adını **kurslar**.

İçinde **Courses.aspx** dosya, seçilen Öğrenci için kursları görüntülemek için bir kılavuz görünüm ekleyin.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Select yöntemi tanımlayın

İçinde **Courses.aspx.cs**, kılavuz görünümünün belirtilen adla select yöntemi ekleyecek **SelectMethod** özelliği. Bu yöntemde, öğrencinin kurslar almak için sorgu tanımlayın ve parametre için parametre olarak aynı ada sahip bir sorgu dizesi değerinden geldiğini belirtin.

İlk olarak, aşağıdaki ekleyin **kullanarak** deyimleri.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Ardından, Courses.aspx.cs için aşağıdaki kodu ekleyin:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Sorgu dizesi özniteliği StudentID adlı bir sorgu dizesi değeri otomatik olarak bu yöntem parametresinde atandığı anlamına gelir.

## <a name="add-hyperlink-with-query-string-value"></a>Sorgu dizesi değeri ile köprü ekleme

Students.aspx ızgara görünümünde yeni kurslar sayfanıza bağlanan bir köprü alanı ekleyeceksiniz. Köprü öğrencinin kimliğine sahip bir sorgu dizesi değerini içerir.

Students.aspx içinde aşağıdaki alan hemen altına alan ızgara görünümü sütunları için toplam iadeleri ekleyin.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Uygulamayı çalıştırın ve ızgara görünümü şimdi kurslar bağlantı içerdiğine dikkat edin.

![Köprü ekleme](using-query-string-values-to-retrieve-data/_static/image1.png)

Bağlantılardan birine tıkladığınızda, bu öğrencinin kayıtlı kurslar görürsünüz.

![kursları göster](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, bir sorgu dizesi değerini içeren bir bağlantı eklendi. Select yöntemindeki parametre değeri, sorgu dizesi değeri kullanılır.

Sonraki [öğretici](adding-business-logic-layer.md), kodu bir iş mantığı katmanı ve veri erişim katmanı olarak arka plan kodu dosyalarından taşır.

> [!div class="step-by-step"]
> [Önceki](integrating-jquery-ui.md)
> [sonraki](adding-business-logic-layer.md)
