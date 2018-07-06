---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Sorgu dizesi değerlerini kullanarak model bağlama ile verileri filtreleme ve web forms | Microsoft Docs
author: tfitzmac
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: b4615d004a32d00b91635bc321a2d4ea792fddbe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837269"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Sorgu dizesi değerlerini verilere filtre uygulamak, model bağlama ve web forms ile kullanma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Bu öğreticide, sorgu dizesinde bir değer geçirmek ve model bağlama aracılığıyla veri almak için bu değeri gösterilir.
> 
> Bu öğreticide oluşturulan proje geliştirir [önceki](retrieving-data.md) dizisinin bölümleri.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide, gerekir:

1. Bir öğrenci için kayıtlı kursları göstermek için yeni bir sayfa ekleyin
2. Sorgu dizesinde bir değere göre seçilen Öğrenci için kayıtlı kursları Al
3. Bir sorgu dizesi değerini içeren bir köprü kılavuz görünümünden yeni sayfaya ekleyin.

Bu öğreticide, yaptığınız için oldukça benzer adımlarla önceki [öğretici](sorting-paging-and-filtering-data.md) bir açılan liste kullanıcı seçimine göre görüntülenen öğrencilere filtre uygulamak için. Bu öğreticide kullandığınız **denetimi** select yöntemindeki parametre değeri bir denetimden geldiğini belirtmek için özniteliği. Bu öğreticide kullanacaksınız **QueryString** select yöntemindeki parametre değeri, Sorgu dizesinden geldiğini belirtmek için özniteliği.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Bir öğrenci kursları görüntülemek için Yeni Sayfa Ekle

Site.master ana sayfa kullanan yeni bir web formu ekleyin ve sayfayı adlandırın **kursları**.

İçinde **Courses.aspx** Kurslar için seçilen Öğrenci görüntülemek için bir kılavuz görünümüne ekleyin.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Select yöntemi tanımlayın

İçinde **Courses.aspx.cs**, select yöntemi kılavuz görünümünün belirtilen adla ekleyeceksiniz **SelectMethod** özelliği. Bu yöntemde, bir öğrencinin kurslar almak için sorgu tanımlama ve parametre, parametre olarak aynı ada sahip bir sorgu dizesi değerinden geldiğini belirtmek.

İlk olarak, aşağıdaki ekleyin **kullanarak** deyimleri.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Ardından, Courses.aspx.cs için aşağıdaki kodu ekleyin:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Sorgu dizesi öznitelik StudentID adlı bir sorgu dizesi değerini bu yöntem parametresi için otomatik olarak atandığı anlamına gelir.

## <a name="add-hyperlink-with-query-string-value"></a>Sorgu dizesi değeri ile köprü ekleme

Students.aspx ızgara görünümünde, yeni kursları sayfasına bağlayan bir köprü alanına ekler. Köprüyü öğrencinin kimliğine sahip bir sorgu dizesi değerini içerir.

Students.aspx içinde aşağıdaki alan ızgara görünümü sütunları hemen altındaki alanı için toplam kredi ekleyin.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Uygulamayı çalıştırmak ve ızgara görünümü artık kursları bağlantı içerdiğine dikkat edin.

![Köprü ekleme](using-query-string-values-to-retrieve-data/_static/image1.png)

Bağlantılardan birine tıkladığınızda, bu öğrencinin kayıtlı kursları görürsünüz.

![kursları göster](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, bir sorgu dizesi değerini içeren bir bağlantı eklendi. Bu sorgu dizesi değerini select yöntemi parametresinin değeri için kullanılır.

Sonraki [öğretici](adding-business-logic-layer.md), kod ile iş mantığı katmanı ve veri erişim katmanı arka plan kod dosyaları taşınır.

> [!div class="step-by-step"]
> [Önceki](integrating-jquery-ui.md)
> [İleri](adding-business-logic-layer.md)
