---
title: ASP.NET core'da kısmi etiket Yardımcısı
author: scottaddie
description: ASP.NET Core kısmi etiket Yardımcısı ve her biri özniteliklerini play kısmi bir görünümü işlemeye içinde rol keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 2272b2ecdd6f2b0a759356b1f03dd5c495ea1c91
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889109"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="5e5dc-103">ASP.NET core'da kısmi etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="5e5dc-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="5e5dc-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5e5dc-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5e5dc-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e5dc-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="5e5dc-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5e5dc-106">Overview</span></span>

<span data-ttu-id="5e5dc-107">Kısmi etiket Yardımcısı işleme için kullanılan bir [kısmi Görünüm](xref:mvc/views/partial) Razor sayfaları ve MVC uygulamalarında.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="5e5dc-108">BT'nin göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-108">Consider that it:</span></span>

* <span data-ttu-id="5e5dc-109">ASP.NET Core 2.1 veya üzerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="5e5dc-110">Alternatif [HTML Yardımcısı söz dizimi](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="5e5dc-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="5e5dc-111">Kısmi görünüm zaman uyumsuz olarak işler.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="5e5dc-112">Kısmi bir görünümü işlemeye yönelik HTML Yardımcısı seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="5e5dc-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="5e5dc-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="5e5dc-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="5e5dc-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="5e5dc-115">*Ürün* modeli, bu belge boyunca örnekleri kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="5e5dc-116">Kısmi etiket Yardımcısı öznitelikleri envanterini izler.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="5e5dc-117">name</span><span class="sxs-lookup"><span data-stu-id="5e5dc-117">name</span></span>

<span data-ttu-id="5e5dc-118">`name` Özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-118">The `name` attribute is required.</span></span> <span data-ttu-id="5e5dc-119">Bu, ad veya işlenecek kısmi görünümün yolunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="5e5dc-120">Kısmi görünümün adı sağlandığında [görünümü bulma](xref:mvc/views/overview#view-discovery) işlemi başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="5e5dc-121">Bu işlem, açık bir yol sağlandığında atlanır.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="5e5dc-122">Aşağıdaki biçimlendirmede gösteren özel bir yol kullanır *_ProductPartial.cshtml* yüklenmesini olan *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="5e5dc-123">Kullanarak [için](#for) öznitelik, bir model geçirilir kısmi görünüm için bağlama.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="5e5dc-124">for</span><span class="sxs-lookup"><span data-stu-id="5e5dc-124">for</span></span>

<span data-ttu-id="5e5dc-125">`for` Atar özniteliği bir [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) geçerli modeline göre değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="5e5dc-126">A `ModelExpression` çıkarsar `@Model.` söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="5e5dc-127">Örneğin, `for="Product"` yerine kullanılan `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="5e5dc-128">Bu varsayılan kesmesi davranışını kullanarak geçersiz kılındı `@` simge bir satır içi ifadesi tanımlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="5e5dc-129">`for` Özniteliği ile kullanılamaz [modeli](#model) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="5e5dc-130">Aşağıdaki biçimlendirmede yükler *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="5e5dc-131">Kısmi görünüm ilişkili sayfa modeli için bağlı `Product` özelliği:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="5e5dc-132">model</span><span class="sxs-lookup"><span data-stu-id="5e5dc-132">model</span></span>

<span data-ttu-id="5e5dc-133">`model` Özniteliği kısmi görünüme iletmek için bir model örneği atar.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="5e5dc-134">`model` Özniteliği ile kullanılamaz [için](#for) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="5e5dc-135">Aşağıdaki biçimlendirmede yeni `Product` nesne örneği ve geçirilen `model` bağlama için öznitelik:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="5e5dc-136">verileri görüntüle</span><span class="sxs-lookup"><span data-stu-id="5e5dc-136">view-data</span></span>

<span data-ttu-id="5e5dc-137">`view-data` Atar özniteliği bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüme iletmek için.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="5e5dc-138">Aşağıdaki biçimlendirmede ViewData koleksiyonunun tamamını kısmi görünüm için erişilebilir hale getirir:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="5e5dc-139">Önceki kodda, `IsNumberReadOnly` anahtar değeri ayarı `true` ve ViewData koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="5e5dc-140">Sonuç olarak, `ViewData["IsNumberReadOnly"]` aşağıdaki kısmi görünümü içinde erişilebilir hale getirdik:</span><span class="sxs-lookup"><span data-stu-id="5e5dc-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="5e5dc-141">Bu örnekte, değerini `ViewData["IsNumberReadOnly"]` belirler olmadığını *numarası* alanı salt okunur olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="5e5dc-142">Bir HTML Yardımcısı geçiş</span><span class="sxs-lookup"><span data-stu-id="5e5dc-142">Migrate from an HTML Helper</span></span>

<span data-ttu-id="5e5dc-143">Aşağıdaki zaman uyumsuz HTML Yardımcısı örneği göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-143">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="5e5dc-144">Ürünleri koleksiyonunu yinelenir ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-144">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="5e5dc-145">Başına `PartialAsync` yöntemin ilk parametresi, *_ProductPartial.cshtml* kısmi görünüm yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-145">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="5e5dc-146">Örneği `Product` model bağlama için kısmi görünüme iletilir.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-146">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="5e5dc-147">Aynı zaman uyumsuz işleme davranışı aşağıdaki kısmi etiket Yardımcısı başarır `PartialAsync` HTML Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-147">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="5e5dc-148">`model` Öznitelik atanmış bir `Product` bağlama kısmi görünüm için model örneği.</span><span class="sxs-lookup"><span data-stu-id="5e5dc-148">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="5e5dc-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5e5dc-149">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>