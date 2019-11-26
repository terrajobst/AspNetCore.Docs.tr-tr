---
title: ASP.NET Core 'de betik etiketi Yardımcısı
author: rick-anderson
ms.author: riande
description: ASP.NET Core betik etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML komut dosyası etiketinin genişletme davranışında oynadığı rolü bulur.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: c3d9148bd62dcc045873cc3a72884ae458349d70
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317115"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="28a06-103">ASP.NET Core 'de betik etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="28a06-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="28a06-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="28a06-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="28a06-105">[Betik etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) , birincil veya geri dönüş betik dosyasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28a06-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="28a06-106">Genellikle birincil betik dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="28a06-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="28a06-107">Betik etiketi Yardımcısı, CDN kullanılabilir olmadığında betik dosyası ve geri dönüş için CDN belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="28a06-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="28a06-108">Betik etiketi Yardımcısı, bir CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="28a06-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="28a06-109">Aşağıdaki Razor biçimlendirmesinde geri dönüş içeren bir `script` öğesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="28a06-109">The following Razor markup shows a `script` element with a fallback:</span></span>

```HTML
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="28a06-110">Yaygın olarak kullanılan betik etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="28a06-110">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="28a06-111">Tüm betik etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [komut dosyası etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) ' na bakın.</span><span class="sxs-lookup"><span data-stu-id="28a06-111">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="asp-fallback-test"></a><span data-ttu-id="28a06-112">ASP-geri dönüş-test</span><span class="sxs-lookup"><span data-stu-id="28a06-112">asp-fallback-test</span></span>

<span data-ttu-id="28a06-113">Geri dönüş testi için kullanılacak birincil betikte tanımlanan betik yöntemi.</span><span class="sxs-lookup"><span data-stu-id="28a06-113">The script method defined in the primary script to use for the fallback test.</span></span> <span data-ttu-id="28a06-114">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span><span class="sxs-lookup"><span data-stu-id="28a06-114">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span></span>

### <a name="asp-fallback-src"></a><span data-ttu-id="28a06-115">ASP-geri dönüş-src</span><span class="sxs-lookup"><span data-stu-id="28a06-115">asp-fallback-src</span></span>

<span data-ttu-id="28a06-116">Birincil bir hata durumunda öğesine geri dönüş yapılacak bir betik etiketinin URL 'SI.</span><span class="sxs-lookup"><span data-stu-id="28a06-116">The URL of a Script tag to fallback to in the case the primary one fails.</span></span> <span data-ttu-id="28a06-117">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span><span class="sxs-lookup"><span data-stu-id="28a06-117">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28a06-118">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="28a06-118">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
