---
ms.openlocfilehash: a0c563f4c58350d069aa789c00081cfe999a2190
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327238"
---
`<form method="post">` Öğesi bir [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi Yardımcısı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).

Yapı iskelesi altyapısı Razor işaretlemesi için her bir alan (ID dışında) modelinde aşağıdakine benzer oluşturur:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve `<span asp-validation-for`) doğrulama hataları görüntüleyin. Doğrulama Bu seriyi ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.

[Etiket etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiketi başlığını oluşturur ve `for` özniteliğini `Title` özelliği.

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.
