---
ms.openlocfilehash: 45fb8a8c0d6e98b002789a6fb7b2b0fce893ad63
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705571"
---
`<form method="post">` Öğesi bir [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).

Yapı iskelesi altyapısı Razor işaretlemesi için her bir alan (ID dışında) modelinde aşağıdakine benzer oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hataları görüntüleyin. Doğrulama Bu seriyi ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiketi başlığını oluşturur ve `for` özniteliğini `Title` özelliği.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.
