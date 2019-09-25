---
title: ASP.NET Core 'de betik etiketi Yardımcısı
author: rick-anderson
ms.author: riande
description: ASP.NET Core betik etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML komut dosyası etiketinin genişletme davranışında oynadığı rolü bulur.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256500"
---
# <a name="script-tag-helper-in-aspnet-core"></a>ASP.NET Core 'de betik etiketi Yardımcısı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Betik etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) , birincil veya geri dönüş betik dosyasına bir bağlantı oluşturur. Genellikle birincil betik dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Betik etiketi Yardımcısı, CDN kullanılabilir olmadığında betik dosyası ve geri dönüş için CDN belirtmenize olanak tanır. Betik etiketi Yardımcısı, bir CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.

Aşağıdaki Razor biçimlendirmesinde, ASP.NET Core Web `script` uygulaması şablonuyla oluşturulan bir düzen dosyasının öğesi gösterilmektedir:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

Aşağıdaki kod, önceki koddan işlenmiş HTML 'ye benzerdir (geliştirme dışı bir ortamda):

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

Önceki kodda, komut dosyası etiketi Yardımcısı, için `<script>  (window.jQuery || document.write(` `window.jQuery`test olan ikinci komut dosyası () öğesini oluşturdu. `window.jQuery` Bulunmazsa ,`document.write(` çalışır ve bir komut dosyası oluşturur 

## <a name="commonly-used-script-tag-helper-attributes"></a>Yaygın olarak kullanılan betik etiketi Yardımcısı öznitelikleri

Tüm betik etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [komut dosyası etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) ' na bakın.

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
