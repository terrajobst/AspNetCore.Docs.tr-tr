---
title: ASP.NET Core 'de betik etiketi Yardımcısı
author: rick-anderson
ms.author: riande
description: ASP.NET Core betik etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML komut dosyası etiketinin genişletme davranışında oynadığı rolü bulur.
ms.custom: mvc
ms.date: 12/02/2019
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: a037abb6a454e6d06305e7d7f6ecad0c2a0ca717
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659840"
---
# <a name="script-tag-helper-in-aspnet-core"></a>ASP.NET Core 'de betik etiketi Yardımcısı

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

[Betik etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) , birincil veya geri dönüş betik dosyasına bir bağlantı oluşturur. Genellikle birincil betik dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Betik etiketi Yardımcısı, CDN kullanılabilir olmadığında betik dosyası ve geri dönüş için CDN belirtmenize olanak tanır. Betik etiketi Yardımcısı, bir CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.

Aşağıdaki Razor biçimlendirmesinde geri dönüş içeren bir `script` öğesi gösterilmektedir:

```html
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

CDN betiğini yüklemeyi ertelemek için `<script>` öğenin [erteleme](https://developer.mozilla.org/docs/Web/HTML/Element/script) özniteliğini kullanmayın. Komut dosyası etiketi Yardımcısı, [ASP-Fallback-test](#asp-fallback-test) ifadesini hemen yürüten JavaScript 'i işler. CDN betiği yükleme ertelenir ise ifade başarısız olur.

## <a name="commonly-used-script-tag-helper-attributes"></a>Yaygın olarak kullanılan betik etiketi Yardımcısı öznitelikleri

Tüm betik etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [komut dosyası etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) ' na bakın.

### <a name="asp-fallback-test"></a>ASP-geri dönüş-test

Geri dönüş testi için kullanılacak birincil betikte tanımlanan betik yöntemi. Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.

### <a name="asp-fallback-src"></a>ASP-geri dönüş-src

Birincil bir hata durumunda öğesine geri dönüş yapılacak bir betik etiketinin URL 'SI. Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
