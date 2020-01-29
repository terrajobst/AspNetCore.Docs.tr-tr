---
title: ASP.NET Core etiket Yardımcısı bağlantı
author: rick-anderson
ms.author: riande
description: ASP.NET Core link etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML bağlantısı etiketinin genişletme davranışında oynadığı rolü bulur.
ms.custom: mvc
ms.date: 09/24/2019
uid: mvc/views/tag-helpers/builtin-th/link-tag-helper
ms.openlocfilehash: d7514433bee8a138cd7d75bfd15c9798d4fd31a3
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809113"
---
# <a name="link-tag-helper-in-aspnet-core"></a><span data-ttu-id="41188-103">ASP.NET Core etiket Yardımcısı bağlantı</span><span class="sxs-lookup"><span data-stu-id="41188-103">Link Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="41188-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="41188-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="41188-105">[Bağlantı etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) , birincil veya GERI dönüş CSS dosyasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="41188-105">The [Link Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) generates a link to a primary or fall back CSS file.</span></span> <span data-ttu-id="41188-106">Genellikle birincil CSS dosyası [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="41188-106">Typically the primary CSS file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="41188-107">Bağlantı etiketi Yardımcısı, CSS dosyası için CDN ve CDN kullanılabilir olmadığında geri dönüş belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="41188-107">The Link Tag Helper allows you to specify a CDN for the CSS file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="41188-108">Bağlantı etiketi Yardımcısı, CDN 'nin performans avantajlarından yararlanarak yerel barındırma sağlamlığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="41188-108">The Link Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="41188-109">Aşağıdaki Razor biçimlendirmesinde, ASP.NET Core Web uygulaması şablonuyla oluşturulan bir düzen dosyasının `head` öğesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="41188-109">The following Razor markup shows the `head` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet)]

<span data-ttu-id="41188-110">Aşağıdaki kod, önceki koddan (geliştirme olmayan bir ortamda) HTML olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="41188-110">The following is rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage1.html)]

<span data-ttu-id="41188-111">Yukarıdaki kodda, bağlantı etiketi Yardımcısı `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` öğesini ve istenen *Bootstrap. min. css* dosyasını doğrulamak için kullanılan aşağıdaki JavaScript 'ı, CDN üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="41188-111">In the preceding code, the Link Tag Helper generated the `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` element and the following JavaScript which is used to verify the requested *bootstrap.min.css* file is available on the CDN.</span></span> <span data-ttu-id="41188-112">Bu durumda, CSS dosyası kullanılabilir olduğundan, etiket Yardımcısı CDN CSS dosyası ile `<link />` öğeyi üretti.</span><span class="sxs-lookup"><span data-stu-id="41188-112">In this case, the CSS file was available so the Tag Helper generated the `<link />` element with the CDN CSS file.</span></span>

## <a name="commonly-used-link-tag-helper-attributes"></a><span data-ttu-id="41188-113">Yaygın olarak kullanılan bağlantı etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="41188-113">Commonly used Link Tag Helper attributes</span></span>

<span data-ttu-id="41188-114">Tüm bağlantı etiketi Yardımcısı öznitelikleri, özellikleri ve yöntemleri için [bağlantı etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) ' na bakın.</span><span class="sxs-lookup"><span data-stu-id="41188-114">See [Link Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper)  for all the Link Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="41188-115">{1&gt;href&lt;1}</span><span class="sxs-lookup"><span data-stu-id="41188-115">href</span></span>

<span data-ttu-id="41188-116">Bağlı kaynağın tercih edilen adresi.</span><span class="sxs-lookup"><span data-stu-id="41188-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="41188-117">Adres, her durumda oluşturulan HTML 'ye düşünce olarak iletilir.</span><span class="sxs-lookup"><span data-stu-id="41188-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="41188-118">ASP-geri dönüş-href</span><span class="sxs-lookup"><span data-stu-id="41188-118">asp-fallback-href</span></span>

<span data-ttu-id="41188-119">Birincil URL 'nin başarısız olması durumunda öğesine geri dönüş için CSS stil sayfasının URL 'SI.</span><span class="sxs-lookup"><span data-stu-id="41188-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="41188-120">ASP-geri dönüş-test sınıfı</span><span class="sxs-lookup"><span data-stu-id="41188-120">asp-fallback-test-class</span></span>

<span data-ttu-id="41188-121">Geri dönüş testi için kullanılacak stil sayfasında tanımlanan sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="41188-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="41188-122">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="41188-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="41188-123">ASP-Fallback-test-özelliği</span><span class="sxs-lookup"><span data-stu-id="41188-123">asp-fallback-test-property</span></span>

<span data-ttu-id="41188-124">Geri dönüş testi için kullanılacak CSS özellik adı.</span><span class="sxs-lookup"><span data-stu-id="41188-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="41188-125">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="41188-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="41188-126">ASP-geri dönüş-test-değeri</span><span class="sxs-lookup"><span data-stu-id="41188-126">asp-fallback-test-value</span></span>

<span data-ttu-id="41188-127">Geri dönüş testi için kullanılacak CSS özelliği değeri.</span><span class="sxs-lookup"><span data-stu-id="41188-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="41188-128">Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="41188-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41188-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="41188-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
