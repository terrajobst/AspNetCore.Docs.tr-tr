---
title: ASP.NET Core etiket Yardımcısı bağlantı
author: rick-anderson
ms.author: riande
description: ASP.NET Core link etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML bağlantısı etiketinin genişletme davranışında oynadığı rolü bulur.
ms.custom: mvc
ms.date: 09/24/2019
uid: mvc/views/tag-helpers/builtin-th/link-tag-helper
ms.openlocfilehash: e1e2e58b4ab9087e1f9de5b5c03b587feb88f1b9
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256521"
---
# <a name="link-tag-helper-in-aspnet-core"></a>ASP.NET Core etiket Yardımcısı bağlantı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Bağlantı etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) , birincil veya GERI dönüş CSS dosyasına bir bağlantı oluşturur. Genellikle birincil CSS dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Bağlantı etiketi Yardımcısı, CSS dosyası için CDN ve CDN kullanılabilir olmadığında geri dönüş belirtmenize olanak tanır. Bağlantı etiketi Yardımcısı, CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.

Aşağıdaki Razor biçimlendirmesinde, ASP.NET Core Web `head` uygulaması şablonuyla oluşturulan bir düzen dosyasının öğesi gösterilmektedir:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet)]

Aşağıdaki kod, önceki koddan (geliştirme olmayan bir ortamda) HTML olarak işlenir:

[!code-csharp[](link-tag-helper/sample/HtmlPage1.html)]

Önceki kodda, bağlantı etiketi Yardımcısı `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` öğesi ve istenen *Bootstrap. min. css* dosyasının CDN üzerinde kullanılabilir olduğunu doğrulamak için kullanılan aşağıdaki JavaScript 'i oluşturdu. Bu durumda, CSS dosyası kullanılabilir olduğundan, etiket Yardımcısı CDN CSS dosyası ile `<link />` öğeyi üretti.

## <a name="commonly-used-link-tag-helper-attributes"></a>Yaygın olarak kullanılan bağlantı etiketi Yardımcısı öznitelikleri

Tüm bağlantı etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [bağlantı etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) ' na bakın.

### <a name="href"></a>değerini

Bağlı kaynağın tercih edilen adresi. Adres, her durumda oluşturulan HTML 'ye düşünce olarak iletilir.

### <a name="asp-fallback-href"></a>ASP-geri dönüş-href

Birincil URL 'nin başarısız olması durumunda öğesine geri dönüş için CSS stil sayfasının URL 'SI.

### <a name="asp-fallback-test-class"></a>ASP-geri dönüş-test sınıfı

Geri dönüş testi için kullanılacak stil sayfasında tanımlanan sınıf adı. Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>ASP-Fallback-test-özelliği

Geri dönüş testi için kullanılacak CSS özellik adı. Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>ASP-geri dönüş-test-değeri

Geri dönüş testi için kullanılacak CSS özelliği değeri. Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

### <a name="asp-fallback-test-value"></a>ASP-geri dönüş-test-değeri

Geri dönüş testi için kullanılacak CSS özelliği değeri. Daha fazla bilgi için bkz.<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
