---
title: ASP.NET core'da ortam etiketi Yardımcısı
author: pkellner
description: Tüm özellikler dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: e2e038fe69da696b67f7aef61795e23dc8512fdf
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856130"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="c3034-103">ASP.NET core'da ortam etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="c3034-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c3034-104">Tarafından [Peter Kellner](https://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c3034-104">By [Peter Kellner](https://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c3034-105">Ortam etiketi Yardımcısı koşullu olarak geçerli göre kapalı içeriğini işler [barındırma ortamı](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c3034-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="c3034-106">Ortam etiketi Yardımcısı'nın tek öznitelik `names`, ortam adlarının virgülle ayrılmış listesidir.</span><span class="sxs-lookup"><span data-stu-id="c3034-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="c3034-107">Sağlanan ortam adları geçerli ortamı hiçbiriyle, ekteki içeriğin işlenir.</span><span class="sxs-lookup"><span data-stu-id="c3034-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="c3034-108">Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="c3034-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="c3034-109">Ortam etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="c3034-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="c3034-110">adlar</span><span class="sxs-lookup"><span data-stu-id="c3034-110">names</span></span>

<span data-ttu-id="c3034-111">`names` Tek bir barındırma ortamı adı veya işleme ekteki içeriğin tetikleyen ortam adları barındırma virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="c3034-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="c3034-112">Tarafından döndürülen değeri, geçerli ortam değerlerini karşılaştırılır [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="c3034-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="c3034-113">Karşılaştırma çalışması yoksayar.</span><span class="sxs-lookup"><span data-stu-id="c3034-113">The comparison ignores case.</span></span>

<span data-ttu-id="c3034-114">Aşağıdaki örnek, bir ortam etiketi Yardımcısı kullanır.</span><span class="sxs-lookup"><span data-stu-id="c3034-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="c3034-115">Barındırma ortamı hazırlık veya üretim olması durumunda içeriğinin işlenip:</span><span class="sxs-lookup"><span data-stu-id="c3034-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="c3034-116">dahil etme ve dışlama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="c3034-116">include and exclude attributes</span></span>

<span data-ttu-id="c3034-117">`include` & `exclude` dahil veya hariç tutulan barındırma ortamı adlarını temel alarak ekteki içeriğin işleme öznitelikleri denetimi.</span><span class="sxs-lookup"><span data-stu-id="c3034-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="c3034-118">include</span><span class="sxs-lookup"><span data-stu-id="c3034-118">include</span></span>

<span data-ttu-id="c3034-119">`include` Özelliği için benzer bir davranış gösteriyor `names` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c3034-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="c3034-120">Listelenen bir ortam `include` öznitelik değeri, uygulamanın barındırma ortamına eşleşmelidir ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) içeriği işlemek için `<environment>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="c3034-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="c3034-121">exclude</span><span class="sxs-lookup"><span data-stu-id="c3034-121">exclude</span></span>

<span data-ttu-id="c3034-122">Tersine `include` özniteliği, içeriğini `<environment>` etiket listede bir ortam barındırma ortamı eşleşmediğinde işlenir `exclude` öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="c3034-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c3034-123">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c3034-123">Additional resources</span></span>

* <xref:fundamentals/environments>
