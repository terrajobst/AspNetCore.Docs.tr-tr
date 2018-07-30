---
title: ASP.NET core'da ortam etiketi Yardımcısı
author: pkellner
description: Tüm özellikler dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342230"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="dec57-103">ASP.NET core'da ortam etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="dec57-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dec57-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="dec57-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="dec57-105">Ortam etiketi Yardımcısı koşullu olarak geçerli barındırma ortamına bağlı kapalı içeriğini işler.</span><span class="sxs-lookup"><span data-stu-id="dec57-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="dec57-106">Bunun tek öznitelik `names` ortam virgülle ayrılmış listesidir ad, tüm geçerli ortama eşleşen, işlenecek ekteki içeriğin tetikler.</span><span class="sxs-lookup"><span data-stu-id="dec57-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="dec57-107">Ortam etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dec57-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="dec57-108">adlar</span><span class="sxs-lookup"><span data-stu-id="dec57-108">names</span></span>

<span data-ttu-id="dec57-109">Tek bir barındırma ortamı adı veya işleme ekteki içeriğin tetikleyen ortam adları barındırma virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="dec57-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="dec57-110">Bu değerler ASP.NET Core statik özelliğinden döndürülen değerle karşılaştırılır `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="dec57-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="dec57-111">Bu değer aşağıdakilerden biridir: **hazırlama**; **Geliştirme** veya **üretim**.</span><span class="sxs-lookup"><span data-stu-id="dec57-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="dec57-112">Karşılaştırma çalışması yoksayar.</span><span class="sxs-lookup"><span data-stu-id="dec57-112">The comparison ignores case.</span></span>

<span data-ttu-id="dec57-113">Geçerli bir örneği `environment` etiket Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="dec57-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="dec57-114">dahil etme ve dışlama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dec57-114">include and exclude attributes</span></span>

<span data-ttu-id="dec57-115">ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="dec57-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="dec57-116">Bu öznitelikler dahil veya hariç tutulan barındırma ortamı adlarını temel alarak ekteki içeriğin oluşturma denetimi.</span><span class="sxs-lookup"><span data-stu-id="dec57-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="dec57-117">ASP.NET Core 2.0 ve üzeri dahil</span><span class="sxs-lookup"><span data-stu-id="dec57-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="dec57-118">`include` Özelliğine sahiptir, benzer bir davranış `names` ASP.NET Core 1.0 özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dec57-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="dec57-119">ASP.NET Core 2.0 ve sonraki sürümleri hariç tut</span><span class="sxs-lookup"><span data-stu-id="dec57-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="dec57-120">Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için ekteki içeriğin işleme.</span><span class="sxs-lookup"><span data-stu-id="dec57-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="dec57-121">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dec57-121">Additional resources</span></span>

* <xref:fundamentals/environments>
