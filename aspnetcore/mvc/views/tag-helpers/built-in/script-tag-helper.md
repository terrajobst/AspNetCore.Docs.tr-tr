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
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="8472b-103">ASP.NET Core 'de betik etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="8472b-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8472b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8472b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8472b-105">[Betik etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) , birincil veya geri dönüş betik dosyasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8472b-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="8472b-106">Genellikle birincil betik dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="8472b-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="8472b-107">Betik etiketi Yardımcısı, CDN kullanılabilir olmadığında betik dosyası ve geri dönüş için CDN belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8472b-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="8472b-108">Betik etiketi Yardımcısı, bir CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8472b-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="8472b-109">Aşağıdaki Razor biçimlendirmesinde, ASP.NET Core Web `script` uygulaması şablonuyla oluşturulan bir düzen dosyasının öğesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="8472b-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="8472b-110">Aşağıdaki kod, önceki koddan işlenmiş HTML 'ye benzerdir (geliştirme dışı bir ortamda):</span><span class="sxs-lookup"><span data-stu-id="8472b-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="8472b-111">Önceki kodda, komut dosyası etiketi Yardımcısı, için `<script>  (window.jQuery || document.write(` `window.jQuery`test olan ikinci komut dosyası () öğesini oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="8472b-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="8472b-112">`window.jQuery` Bulunmazsa ,`document.write(` çalışır ve bir komut dosyası oluşturur</span><span class="sxs-lookup"><span data-stu-id="8472b-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="8472b-113">Yaygın olarak kullanılan betik etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="8472b-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="8472b-114">Tüm betik etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [komut dosyası etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) ' na bakın.</span><span class="sxs-lookup"><span data-stu-id="8472b-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="8472b-115">değerini</span><span class="sxs-lookup"><span data-stu-id="8472b-115">href</span></span>

<span data-ttu-id="8472b-116">Bağlı kaynağın tercih edilen adresi.</span><span class="sxs-lookup"><span data-stu-id="8472b-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="8472b-117">Adres, her durumda oluşturulan HTML 'ye düşünce olarak iletilir.</span><span class="sxs-lookup"><span data-stu-id="8472b-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="8472b-118">ASP-geri dönüş-href</span><span class="sxs-lookup"><span data-stu-id="8472b-118">asp-fallback-href</span></span>

<span data-ttu-id="8472b-119">Birincil URL 'nin başarısız olması durumunda öğesine geri dönüş için CSS stil sayfasının URL 'SI.</span><span class="sxs-lookup"><span data-stu-id="8472b-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="8472b-120">ASP-geri dönüş-test sınıfı</span><span class="sxs-lookup"><span data-stu-id="8472b-120">asp-fallback-test-class</span></span>

<span data-ttu-id="8472b-121">Geri dönüş testi için kullanılacak stil sayfasında tanımlanan sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="8472b-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="8472b-122">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="8472b-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="8472b-123">ASP-Fallback-test-özelliği</span><span class="sxs-lookup"><span data-stu-id="8472b-123">asp-fallback-test-property</span></span>

<span data-ttu-id="8472b-124">Geri dönüş testi için kullanılacak CSS özellik adı.</span><span class="sxs-lookup"><span data-stu-id="8472b-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="8472b-125">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="8472b-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="8472b-126">ASP-geri dönüş-test-değeri</span><span class="sxs-lookup"><span data-stu-id="8472b-126">asp-fallback-test-value</span></span>

<span data-ttu-id="8472b-127">Geri dönüş testi için kullanılacak CSS özelliği değeri.</span><span class="sxs-lookup"><span data-stu-id="8472b-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="8472b-128">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="8472b-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="8472b-129">ASP-geri dönüş-test-değeri</span><span class="sxs-lookup"><span data-stu-id="8472b-129">asp-fallback-test-value</span></span>

<span data-ttu-id="8472b-130">Geri dönüş testi için kullanılacak CSS özelliği değeri.</span><span class="sxs-lookup"><span data-stu-id="8472b-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="8472b-131">Daha fazla bilgi için bkz.<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span><span class="sxs-lookup"><span data-stu-id="8472b-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="8472b-132">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8472b-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
