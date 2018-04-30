---
title: ASP.NET Core kısmi etiketi yok
author: scottaddie
description: ASP.NET Core kısmi etiket Yardımcısı ve her özniteliklerini yürütmek kısmi bir görünümü işlemeye rol bulur.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 670663b963f4207da793afff44d55b85ba58b7f8
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="326f2-103">ASP.NET Core kısmi etiketi yok</span><span class="sxs-lookup"><span data-stu-id="326f2-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="326f2-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="326f2-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="326f2-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="326f2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="326f2-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="326f2-106">Overview</span></span>

<span data-ttu-id="326f2-107">Kısmi etiket Yardımcısı işleme için kullanılan bir [kısmi Görünüm](xref:mvc/views/partial) Razor sayfaları ve MVC uygulamalarında.</span><span class="sxs-lookup"><span data-stu-id="326f2-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="326f2-108">Bu BT göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="326f2-108">Consider that it:</span></span>

* <span data-ttu-id="326f2-109">ASP.NET Core 2.1 veya sonrası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="326f2-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="326f2-110">İçin bir alternatif [HTML Yardımcısı sözdizimi](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="326f2-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="326f2-111">Kısmi görünümü zaman uyumsuz olarak işler.</span><span class="sxs-lookup"><span data-stu-id="326f2-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="326f2-112">Kısmi bir görünümü işlemeye yönelik HTML Yardımcısı seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="326f2-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="326f2-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="326f2-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="326f2-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="326f2-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="326f2-115">*Ürün* modeli örnekleri bu belge boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="326f2-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="326f2-116">Kısmi etiketi yardımcı öznitelik envanterini izler.</span><span class="sxs-lookup"><span data-stu-id="326f2-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="326f2-117">name</span><span class="sxs-lookup"><span data-stu-id="326f2-117">name</span></span>

<span data-ttu-id="326f2-118">`name` Özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="326f2-118">The `name` attribute is required.</span></span> <span data-ttu-id="326f2-119">Adı veya yolu işlenecek kısmi görünümün gösterir.</span><span class="sxs-lookup"><span data-stu-id="326f2-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="326f2-120">Kısmi görünümün adı sağlandığında [görünüm bulma](xref:mvc/views/overview#view-discovery) işlemi başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="326f2-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="326f2-121">Bu işlem, özel bir yol sağlandığında atlanır.</span><span class="sxs-lookup"><span data-stu-id="326f2-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="326f2-122">Aşağıdaki biçimlendirmede olduğunu gösteren, açık bir yol kullanır *_ProductPartial.cshtml* gelen yükleneceği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="326f2-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="326f2-123">Kullanarak [için](#for) öznitelik, bir model geçirilir kısmi görünüm için bağlama.</span><span class="sxs-lookup"><span data-stu-id="326f2-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="326f2-124">for</span><span class="sxs-lookup"><span data-stu-id="326f2-124">for</span></span>

<span data-ttu-id="326f2-125">`for` Özniteliği atar bir [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) geçerli modeline göre değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="326f2-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="326f2-126">A `ModelExpression` oluşturur `@Model.` sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="326f2-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="326f2-127">Örneğin, `for="Product"` yerine kullanılan `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="326f2-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="326f2-128">Bu varsayılan çıkarım davranışı kullanarak kılınmadığı `@` bir satır içi ifade tanımlamak için simge.</span><span class="sxs-lookup"><span data-stu-id="326f2-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="326f2-129">`for` Özniteliği ile kullanılamaz [modeli](#model) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="326f2-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="326f2-130">Aşağıdaki biçimlendirmede yükler *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="326f2-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="326f2-131">Kısmi görünüm ilişkili sayfa modelinin için bağlı `Product` özelliği:</span><span class="sxs-lookup"><span data-stu-id="326f2-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="326f2-132">model</span><span class="sxs-lookup"><span data-stu-id="326f2-132">model</span></span>

<span data-ttu-id="326f2-133">`model` Özniteliği kısmi görünüme iletmek için bir model örneği atar.</span><span class="sxs-lookup"><span data-stu-id="326f2-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="326f2-134">`model` Özniteliği ile kullanılamaz [için](#for) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="326f2-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="326f2-135">Aşağıdaki biçimlendirmede, yeni bir `Product` nesne örneği ve geçirilen `model` özniteliği için bağlama:</span><span class="sxs-lookup"><span data-stu-id="326f2-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="326f2-136">Görünüm veri</span><span class="sxs-lookup"><span data-stu-id="326f2-136">view-data</span></span>

<span data-ttu-id="326f2-137">`view-data` Özniteliği atar bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüme iletmek için.</span><span class="sxs-lookup"><span data-stu-id="326f2-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="326f2-138">Aşağıdaki biçimlendirmede tüm ViewData koleksiyon kısmi görünüm için erişilebilir hale getirir:</span><span class="sxs-lookup"><span data-stu-id="326f2-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="326f2-139">Önceki kod `IsNumberReadOnly` anahtar değeri ayarı `true` ve ViewData koleksiyonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="326f2-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="326f2-140">Sonuç olarak, `ViewData["IsNumberReadOnly"]` aşağıdaki kısmi görünümü içinde erişilebilir yapılır:</span><span class="sxs-lookup"><span data-stu-id="326f2-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="326f2-141">Bu örnekte, değeri `ViewData["IsNumberReadOnly"]` belirler olup olmadığını *numarası* alan salt okunur olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="326f2-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="326f2-142">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="326f2-142">Additional resources</span></span>

* [<span data-ttu-id="326f2-143">Kısmi görünümler</span><span class="sxs-lookup"><span data-stu-id="326f2-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="326f2-144">Zayıf yazılmış verileri (ViewData ve Görünüm Paketi)</span><span class="sxs-lookup"><span data-stu-id="326f2-144">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)