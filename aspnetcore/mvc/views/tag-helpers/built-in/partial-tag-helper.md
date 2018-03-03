---
title: "Kısmi etiket Yardımcısı"
author: scottaddie
description: "ASP.NET Core kısmi etiket Yardımcısı ve her özniteliklerini yürütmek kısmi bir görünümü işlemeye rol bulur."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: e5c71ccb7a355ffe1c24f389ab490d614d333589
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="partial-tag-helper"></a>Kısmi etiket Yardımcısı

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Genel Bakış

Kısmi etiket Yardımcısı işleme için kullanılan bir [kısmi Görünüm](xref:mvc/views/partial) Razor sayfaları ve MVC uygulamalarında. Bu BT göz önünde bulundurun:

* ASP.NET Core 2.1 veya sonrası gerektirir.
* İçin bir alternatif [HTML Yardımcısı sözdizimi](xref:mvc/views/partial#referencing-a-partial-view).

Olduðunu için kısmi bir görünümü işlemeye yönelik HTML Yardımcısı seçenekleri şunlardır:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Ürün* modeli örnekleri bu belge boyunca kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Kısmi etiketi yardımcı öznitelik envanterini izler.

## <a name="name"></a>name

`name` Özniteliği gereklidir. Adı veya yolu işlenecek kısmi görünümün gösterir. Kısmi görünümün adı sağlandığında [görünüm bulma](xref:mvc/views/overview#view-discovery) işlemi başlatıldı. Bu işlem, özel bir yol sağlandığında atlanır.

Aşağıdaki biçimlendirmede olduğunu gösteren, açık bir yol kullanır *_ProductPartial.cshtml* gelen yükleneceği *paylaşılan* klasör. Kullanarak [asp-için](#asp-for) öznitelik, bir model geçirilir kısmi görünüm için bağlama.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="asp-for"></a>ASP-için

`asp-for` Özniteliği atar bir [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) geçerli modeline göre değerlendirilecek. A `ModelExpression` oluşturur `@Model.` sözdizimi. Örneğin, `asp-for="Product"` yerine kullanılan `asp-for="@Model.Product"`. Bu varsayılan çıkarım davranışı kullanarak kılınmadığı `@` bir satır içi ifade tanımlamak için simge.

Aşağıdaki biçimlendirmede yükler *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_AspFor)]

Kısmi görünüm ilişkili sayfa modelinin için bağlı `Product` özelliği:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="view-data"></a>view-data

`view-data` Özniteliği atar bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüme iletmek için. Aşağıdaki biçimlendirmede tüm ViewData koleksiyon kısmi görünüm için erişilebilir hale getirir:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

Önceki kod `IsNumberReadOnly` anahtar değeri ayarı `true` ve ViewData koleksiyonuna eklenir. Sonuç olarak, `ViewData["IsNumberReadOnly"]` aşağıdaki kısmi görünümü içinde erişilebilir yapılır:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

Bu örnekte, değeri `ViewData["IsNumberReadOnly"]` belirler olup olmadığını *numarası* alan salt okunur olarak görüntülenir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kısmi görünümler](xref:mvc/views/partial)
* [Zayıf yazılmış verileri (ViewData ve Görünüm Paketi)](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
