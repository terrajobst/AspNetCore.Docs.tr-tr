---
title: ASP.NET Core kısmi etiketi yok
author: scottaddie
description: ASP.NET Core kısmi etiket Yardımcısı ve her özniteliklerini yürütmek kısmi bir görünümü işlemeye rol bulur.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 786c333980db89a9a5a60dc70c0bd1998ca159cd
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962600"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>ASP.NET Core kısmi etiketi yok

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Genel Bakış

Kısmi etiket Yardımcısı işleme için kullanılan bir [kısmi Görünüm](xref:mvc/views/partial) Razor sayfaları ve MVC uygulamalarında. Bu BT göz önünde bulundurun:

* ASP.NET Core 2.1 veya sonrası gerektirir.
* İçin bir alternatif [HTML Yardımcısı sözdizimi](xref:mvc/views/partial#referencing-a-partial-view).
* Kısmi görünümü zaman uyumsuz olarak işler.

Kısmi bir görünümü işlemeye yönelik HTML Yardımcısı seçenekleri şunlardır:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Ürün* modeli örnekleri bu belge boyunca kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Kısmi etiketi yardımcı öznitelik envanterini izler.

## <a name="name"></a>name

`name` Özniteliği gereklidir. Adı veya yolu işlenecek kısmi görünümün gösterir. Kısmi görünümün adı sağlandığında [görünüm bulma](xref:mvc/views/overview#view-discovery) işlemi başlatıldı. Bu işlem, özel bir yol sağlandığında atlanır.

Aşağıdaki biçimlendirmede olduğunu gösteren, açık bir yol kullanır *_ProductPartial.cshtml* gelen yükleneceği *paylaşılan* klasör. Kullanarak [için](#for) öznitelik, bir model geçirilir kısmi görünüm için bağlama.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>for

`for` Özniteliği atar bir [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) geçerli modeline göre değerlendirilecek. A `ModelExpression` oluşturur `@Model.` sözdizimi. Örneğin, `for="Product"` yerine kullanılan `for="@Model.Product"`. Bu varsayılan çıkarım davranışı kullanarak kılınmadığı `@` bir satır içi ifade tanımlamak için simge. `for` Özniteliği ile kullanılamaz [modeli](#model) özniteliği.

Aşağıdaki biçimlendirmede yükler *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

Kısmi görünüm ilişkili sayfa modelinin için bağlı `Product` özelliği:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>model

`model` Özniteliği kısmi görünüme iletmek için bir model örneği atar. `model` Özniteliği ile kullanılamaz [için](#for) özniteliği.

Aşağıdaki biçimlendirmede, yeni bir `Product` nesne örneği ve geçirilen `model` özniteliği için bağlama:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>Görünüm veri

`view-data` Özniteliği atar bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüme iletmek için. Aşağıdaki biçimlendirmede tüm ViewData koleksiyon kısmi görünüm için erişilebilir hale getirir:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

Önceki kod `IsNumberReadOnly` anahtar değeri ayarı `true` ve ViewData koleksiyonuna eklenir. Sonuç olarak, `ViewData["IsNumberReadOnly"]` aşağıdaki kısmi görünümü içinde erişilebilir yapılır:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

Bu örnekte, değeri `ViewData["IsNumberReadOnly"]` belirler olup olmadığını *numarası* alan salt okunur olarak görüntülenir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kısmi görünümler](xref:mvc/views/partial)
* [Veri'zayıf yazılmış (ViewData, ViewData özniteliği ve Görünüm Paketi)](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
