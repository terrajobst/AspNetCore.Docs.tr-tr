---
title: ASP.NET core'da etiket Yardımcısı bileşenleri
author: scottaddie
description: Etiket Yardımcısı bileşenler nelerdir ve ASP.NET Core nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: b5b3abea6492cfaa7d6acd0e54073a8db12eb2a5
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034758"
---
# <a name="tag-helper-components-in-aspnet-core"></a><span data-ttu-id="5d951-103">ASP.NET core'da etiket Yardımcısı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="5d951-103">Tag Helper Components in ASP.NET Core</span></span>

<span data-ttu-id="5d951-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span><span class="sxs-lookup"><span data-stu-id="5d951-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span></span>

<span data-ttu-id="5d951-105">Bir etiket Yardımcısı koşullu olarak değiştirmek veya HTML öğelerinin sunucu tarafı koddan eklemek izin veren bir etiket Yardımcısı bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="5d951-105">A Tag Helper Component is a Tag Helper that allows you to conditionally modify or add HTML elements from server-side code.</span></span> <span data-ttu-id="5d951-106">Bu özellik, ASP.NET Core 2.0 veya sonraki sürümlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5d951-106">This feature is available in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="5d951-107">ASP.NET Core, iki yerleşik etiket Yardımcısı bileşenleri içerir: `head` ve `body`.</span><span class="sxs-lookup"><span data-stu-id="5d951-107">ASP.NET Core includes two built-in Tag Helper Components: `head` and `body`.</span></span> <span data-ttu-id="5d951-108">Bulunan <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> ad alanı ve MVC ve Razor sayfaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5d951-108">They're located in the <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> namespace and can be used in both MVC and Razor Pages.</span></span> <span data-ttu-id="5d951-109">Etiket Yardımcısı bileşenleri yoksa uygulamayı kayıt gerektiren *_viewımports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5d951-109">Tag Helper Components don't require registration with the app in *_ViewImports.cshtml*.</span></span>

<span data-ttu-id="5d951-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5d951-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="use-cases"></a><span data-ttu-id="5d951-111">Uygulama alanları</span><span class="sxs-lookup"><span data-stu-id="5d951-111">Use cases</span></span>

<span data-ttu-id="5d951-112">Etiket Yardımcısı bileşenlerinin iki yaygın kullanım örnekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5d951-112">Two common use cases of Tag Helper Components include:</span></span>

1. [<span data-ttu-id="5d951-113">Ekleme bir `<link>` içine `<head>`.</span><span class="sxs-lookup"><span data-stu-id="5d951-113">Injecting a `<link>` into the `<head>`.</span></span>](#inject-into-html-head-element)
1. [<span data-ttu-id="5d951-114">Ekleme bir `<script>` içine `<body>`.</span><span class="sxs-lookup"><span data-stu-id="5d951-114">Injecting a `<script>` into the `<body>`.</span></span>](#inject-into-html-body-element)

<span data-ttu-id="5d951-115">Bunlar aşağıdaki bölümlerde açıklanmaktadır durumlarda kullanın.</span><span class="sxs-lookup"><span data-stu-id="5d951-115">The following sections describe these use cases.</span></span>

### <a name="inject-into-html-head-element"></a><span data-ttu-id="5d951-116">HTML baş öğesinin içine ekleme</span><span class="sxs-lookup"><span data-stu-id="5d951-116">Inject into HTML head element</span></span>

<span data-ttu-id="5d951-117">HTML içinde `<head>` öğesi, CSS dosyaları HTML ile sık alınan `<link>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5d951-117">Inside the HTML `<head>` element, CSS files are commonly imported with the HTML `<link>` element.</span></span> <span data-ttu-id="5d951-118">Aşağıdaki kod eklediği bir `<link>` öğesine `<head>` öğesini kullanarak `head` etiket Yardımcısı bileşeni:</span><span class="sxs-lookup"><span data-stu-id="5d951-118">The following code injects a `<link>` element into the `<head>` element using the `head` Tag Helper Component:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

<span data-ttu-id="5d951-119">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="5d951-119">In the preceding code:</span></span>

* <span data-ttu-id="5d951-120">`AddressStyleTagHelperComponent` uygulayan <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span><span class="sxs-lookup"><span data-stu-id="5d951-120">`AddressStyleTagHelperComponent` implements <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span></span> <span data-ttu-id="5d951-121">Özet:</span><span class="sxs-lookup"><span data-stu-id="5d951-121">The abstraction:</span></span>
  * <span data-ttu-id="5d951-122">Başlatma ile sınıfının sağlayan bir <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span><span class="sxs-lookup"><span data-stu-id="5d951-122">Allows initialization of the class with a <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span></span>
  * <span data-ttu-id="5d951-123">Etiket Yardımcısı bileşenleri, ekleyin veya HTML öğeleri değiştirmek için kullanılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5d951-123">Enables the use of Tag Helper Components to add or modify HTML elements.</span></span>
* <span data-ttu-id="5d951-124"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Özelliği bileşenleri işlenir sırasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5d951-124">The <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> property defines the order in which the Components are rendered.</span></span> <span data-ttu-id="5d951-125">`Order` Etiket Yardımcısı bileşenlerin bir uygulamada birden fazla kullanımları olduğunda gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5d951-125">`Order` is necessary when there are multiple usages of Tag Helper Components in an app.</span></span>
* <span data-ttu-id="5d951-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> yürütme bağlamı'nın karşılaştırır <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> özellik değerini `head`.</span><span class="sxs-lookup"><span data-stu-id="5d951-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> compares the execution context's <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> property value to `head`.</span></span> <span data-ttu-id="5d951-127">Karşılaştırma true olarak içeriğini değerlendirilirse `_style` alan HTML'e eklenen `<head>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5d951-127">If the comparison evaluates to true, the content of the `_style` field is injected into the HTML `<head>` element.</span></span>

### <a name="inject-into-html-body-element"></a><span data-ttu-id="5d951-128">HTML gövde öğesi ekleme</span><span class="sxs-lookup"><span data-stu-id="5d951-128">Inject into HTML body element</span></span>

<span data-ttu-id="5d951-129">`body` Etiket Yardımcısı bileşeni ekleme bir `<script>` öğesine `<body>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5d951-129">The `body` Tag Helper Component can inject a `<script>` element into the `<body>` element.</span></span> <span data-ttu-id="5d951-130">Aşağıdaki kod, bu tekniği gösterir:</span><span class="sxs-lookup"><span data-stu-id="5d951-130">The following code demonstrates this technique:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

<span data-ttu-id="5d951-131">Ayrı bir HTML dosyası depolamak için kullanılan `<script>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5d951-131">A separate HTML file is used to store the `<script>` element.</span></span> <span data-ttu-id="5d951-132">HTML dosyası daha temiz ve daha sürdürülebilir kod yapar.</span><span class="sxs-lookup"><span data-stu-id="5d951-132">The HTML file makes the code cleaner and more maintainable.</span></span> <span data-ttu-id="5d951-133">Yukarıdaki kod içeriğini okur *TagHelpers/Templates/AddressToolTipScript.html* ve etiket Yardımcısı çıkışıyla ekler.</span><span class="sxs-lookup"><span data-stu-id="5d951-133">The preceding code reads the contents of *TagHelpers/Templates/AddressToolTipScript.html* and appends it with the Tag Helper output.</span></span> <span data-ttu-id="5d951-134">*AddressToolTipScript.html* dosyası aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="5d951-134">The *AddressToolTipScript.html* file includes the following markup:</span></span>

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

<span data-ttu-id="5d951-135">Yukarıdaki kod bağlayan bir [Bootstrap araç ipucu pencere öğesi](https://getbootstrap.com/docs/3.3/javascript/#tooltips) herhangi `<address>` içeren öğesi bir `printable` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5d951-135">The preceding code binds a [Bootstrap tooltip widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) to any `<address>` element that includes a `printable` attribute.</span></span> <span data-ttu-id="5d951-136">Fare işaretçisi öğenin bekletildiğinde etkili görülebilir.</span><span class="sxs-lookup"><span data-stu-id="5d951-136">The effect is visible when a mouse pointer hovers over the element.</span></span>

## <a name="register-a-component"></a><span data-ttu-id="5d951-137">Bir bileşeni kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-137">Register a Component</span></span>

<span data-ttu-id="5d951-138">Bir etiket Yardımcısı bileşeni uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5d951-138">A Tag Helper Component must be added to the app's Tag Helper Components collection.</span></span> <span data-ttu-id="5d951-139">Koleksiyona eklemenin üç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="5d951-139">There are three ways to add to the collection:</span></span>

* [<span data-ttu-id="5d951-140">ASP.NET core'da etiket Yardımcısı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="5d951-140">Tag Helper Components in ASP.NET Core</span></span>](#tag-helper-components-in-aspnet-core)
  * [<span data-ttu-id="5d951-141">Kullanım örnekleri</span><span class="sxs-lookup"><span data-stu-id="5d951-141">Use cases</span></span>](#use-cases)
    * [<span data-ttu-id="5d951-142">HTML baş öğesinin içine ekleme</span><span class="sxs-lookup"><span data-stu-id="5d951-142">Inject into HTML head element</span></span>](#inject-into-html-head-element)
    * [<span data-ttu-id="5d951-143">HTML gövde öğesi ekleme</span><span class="sxs-lookup"><span data-stu-id="5d951-143">Inject into HTML body element</span></span>](#inject-into-html-body-element)
  * [<span data-ttu-id="5d951-144">Bir bileşeni kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-144">Register a Component</span></span>](#register-a-component)
    * [<span data-ttu-id="5d951-145">Kapsayıcı Hizmetleri üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-145">Registration via services container</span></span>](#registration-via-services-container)
    * [<span data-ttu-id="5d951-146">Razor dosya üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-146">Registration via Razor file</span></span>](#registration-via-razor-file)
    * [<span data-ttu-id="5d951-147">Sayfa modeli veya denetleyicisi üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-147">Registration via Page Model or controller</span></span>](#registration-via-page-model-or-controller)
  * [<span data-ttu-id="5d951-148">Bir bileşen oluşturun</span><span class="sxs-lookup"><span data-stu-id="5d951-148">Create a Component</span></span>](#create-a-component)
  * [<span data-ttu-id="5d951-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5d951-149">Additional resources</span></span>](#additional-resources)

### <a name="registration-via-services-container"></a><span data-ttu-id="5d951-150">Kapsayıcı Hizmetleri üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-150">Registration via services container</span></span>

<span data-ttu-id="5d951-151">Etiket Yardımcısı bileşen sınıfı ile yönetilmiyorsa <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) sistem.</span><span class="sxs-lookup"><span data-stu-id="5d951-151">If the Tag Helper Component class isn't managed with <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, it must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) system.</span></span> <span data-ttu-id="5d951-152">Aşağıdaki `Startup.ConfigureServices` kod kayıtları `AddressStyleTagHelperComponent` ve `AddressScriptTagHelperComponent` ile sınıfları bir [geçici ömrü](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span><span class="sxs-lookup"><span data-stu-id="5d951-152">The following `Startup.ConfigureServices` code registers the `AddressStyleTagHelperComponent` and `AddressScriptTagHelperComponent` classes with a [transient lifetime](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a><span data-ttu-id="5d951-153">Razor dosya üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-153">Registration via Razor file</span></span>

<span data-ttu-id="5d951-154">Etiket Yardımcısı bileşen DI, kayıtlı değilse bir Razor sayfaları sayfası ya da bir MVC görünümü kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5d951-154">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page or an MVC view.</span></span> <span data-ttu-id="5d951-155">Bu teknik, eklenen biçimlendirme ve Razor dosyasından bileşen yürütme sırasını denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5d951-155">This technique is used for controlling the injected markup and the component execution order from a Razor file.</span></span>

<span data-ttu-id="5d951-156">`ITagHelperComponentManager` Etiket Yardımcısı bileşenleri ekleyip bunları uygulamadan kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5d951-156">`ITagHelperComponentManager` is used to add Tag Helper Components or remove them from the app.</span></span> <span data-ttu-id="5d951-157">Aşağıdaki kod bu teknikte gösterir `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="5d951-157">The following code demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

<span data-ttu-id="5d951-158">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="5d951-158">In the preceding code:</span></span>

* <span data-ttu-id="5d951-159">`@inject` Yönergenin sağladığı örneğini `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="5d951-159">The `@inject` directive provides an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="5d951-160">Örnek adlı bir değişkene atanır `manager` aşağı yönde Razor dosyasına erişim için.</span><span class="sxs-lookup"><span data-stu-id="5d951-160">The instance is assigned to a variable named `manager` for access downstream in the Razor file.</span></span>
* <span data-ttu-id="5d951-161">Örneği `AddressTagHelperComponent` uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="5d951-161">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

<span data-ttu-id="5d951-162">`AddressTagHelperComponent` kabul eden bir oluşturucu içerecek şekilde değişiklik `markup` ve `order` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="5d951-162">`AddressTagHelperComponent` is modified to accommodate a constructor that accepts the `markup` and `order` parameters:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

<span data-ttu-id="5d951-163">Sağlanan `markup` parametresi kullanılır `ProcessAsync` gibi:</span><span class="sxs-lookup"><span data-stu-id="5d951-163">The provided `markup` parameter is used in `ProcessAsync` as follows:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a><span data-ttu-id="5d951-164">Sayfa modeli veya denetleyicisi üzerinden kayıt</span><span class="sxs-lookup"><span data-stu-id="5d951-164">Registration via Page Model or controller</span></span>

<span data-ttu-id="5d951-165">Etiket Yardımcısı bileşen DI, kayıtlı değilse, Razor sayfaları sayfa modeli veya bir MVC denetleyicisi kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5d951-165">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page model or an MVC controller.</span></span> <span data-ttu-id="5d951-166">Bu teknik, C# Razor dosyaları mantığından ayırmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="5d951-166">This technique is useful for separating C# logic from Razor files.</span></span>

<span data-ttu-id="5d951-167">Oluşturucu ekleme örneği erişmek için kullanılan `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="5d951-167">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="5d951-168">Etiket Yardımcısı bileşen örneğinin etiket Yardımcısı bileşenleri koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="5d951-168">The Tag Helper Component is added to the instance's Tag Helper Components collection.</span></span> <span data-ttu-id="5d951-169">Bu teknikte aşağıdaki Razor sayfaları sayfa modeli gösterir `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="5d951-169">The following Razor Pages page model demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

<span data-ttu-id="5d951-170">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="5d951-170">In the preceding code:</span></span>

* <span data-ttu-id="5d951-171">Oluşturucu ekleme örneği erişmek için kullanılan `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="5d951-171">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span>
* <span data-ttu-id="5d951-172">Örneği `AddressTagHelperComponent` uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="5d951-172">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

## <a name="create-a-component"></a><span data-ttu-id="5d951-173">Bir bileşen oluşturun</span><span class="sxs-lookup"><span data-stu-id="5d951-173">Create a Component</span></span>

<span data-ttu-id="5d951-174">Bir özel etiket Yardımcısı bileşeni oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="5d951-174">To create a custom Tag Helper Component:</span></span>

* <span data-ttu-id="5d951-175">Türetilen bir ortak sınıf oluşturun <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span><span class="sxs-lookup"><span data-stu-id="5d951-175">Create a public class deriving from <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span></span>
* <span data-ttu-id="5d951-176">Geçerli bir [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) öznitelik sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5d951-176">Apply an [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) attribute to the class.</span></span> <span data-ttu-id="5d951-177">Hedef HTML öğesinin adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="5d951-177">Specify the name of the target HTML element.</span></span>
* <span data-ttu-id="5d951-178">*İsteğe bağlı*: Geçerli bir [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) türün görüntülenmesine IntelliSense içinde Sınıf özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5d951-178">*Optional*: Apply an [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) attribute to the class to suppress the type's display in IntelliSense.</span></span>

<span data-ttu-id="5d951-179">Aşağıdaki kod bir özel etiket Yardımcısı bileşen hedefleyen oluşturur `<address>` HTML öğesi:</span><span class="sxs-lookup"><span data-stu-id="5d951-179">The following code creates a custom Tag Helper Component that targets the `<address>` HTML element:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

<span data-ttu-id="5d951-180">Özel kullanmanın `address` gibi HTML biçimlendirmesi eklemesine etiket Yardımcısı bileşeni:</span><span class="sxs-lookup"><span data-stu-id="5d951-180">Use the custom `address` Tag Helper Component to inject HTML markup as follows:</span></span>

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

<span data-ttu-id="5d951-181">Önceki `ProcessAsync` yöntemi için sağlanan HTML eklediği <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> eşleşen içine `<address>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5d951-181">The preceding `ProcessAsync` method injects the HTML provided to <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> into the matching `<address>` element.</span></span> <span data-ttu-id="5d951-182">Ekleme gerçekleşir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="5d951-182">The injection occurs when:</span></span>

* <span data-ttu-id="5d951-183">Yürütme bağlamı'nın `TagName` özellik değeri eşittir `address`.</span><span class="sxs-lookup"><span data-stu-id="5d951-183">The execution context's `TagName` property value equals `address`.</span></span>
* <span data-ttu-id="5d951-184">Buna karşılık gelen `<address>` öğeye sahip bir `printable` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5d951-184">The corresponding `<address>` element has a `printable` attribute.</span></span>

<span data-ttu-id="5d951-185">Örneğin, `if` ifadesi, aşağıdaki işleme sırasında true olarak değerlendirilir `<address>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="5d951-185">For example, the `if` statement evaluates to true when processing the following `<address>` element:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a><span data-ttu-id="5d951-186">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5d951-186">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
