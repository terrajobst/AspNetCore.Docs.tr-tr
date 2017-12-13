---
title: ASP.NET Core ortam etiketi yok
author: pkellner
description: "Tüm özellikleri dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 2639e4d7494e752462a1a2cb0648042a2d2d06ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="11b37-104">ASP.NET Core ortam etiketi yok</span><span class="sxs-lookup"><span data-stu-id="11b37-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="11b37-105">Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="11b37-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="11b37-106">Ortam etiketi yardımcı koşullu geçerli barındırma ortamına bağlı kapalı içeriği işler.</span><span class="sxs-lookup"><span data-stu-id="11b37-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="11b37-107">Tek öznitelik `names` ortamı virgülle ayrılmış listesi olduğu ad, herhangi bir geçerli ortama eşleşen, işlenmek üzere kapalı içerik tetikler.</span><span class="sxs-lookup"><span data-stu-id="11b37-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="11b37-108">Ortam etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="11b37-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="11b37-109">adlar</span><span class="sxs-lookup"><span data-stu-id="11b37-109">names</span></span>

<span data-ttu-id="11b37-110">Tek bir barındırma ortamı ad veya kapalı içerik çizmeye tetiklemek ortam adları barındırma virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="11b37-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="11b37-111">Bu değer ASP.NET Core statik özelliğinden döndürülen geçerli değer karşılaştırılır `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="11b37-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="11b37-112">Bu değer şunlardan biri değil: **hazırlama**; **Geliştirme** veya **üretim**.</span><span class="sxs-lookup"><span data-stu-id="11b37-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="11b37-113">Karşılaştırma durumu yok sayar.</span><span class="sxs-lookup"><span data-stu-id="11b37-113">The comparison ignores case.</span></span>

<span data-ttu-id="11b37-114">Geçerli bir örneği `environment` etiketi yardımcı:</span><span class="sxs-lookup"><span data-stu-id="11b37-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="11b37-115">eklemek veya çıkartmak öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="11b37-115">include and exclude attributes</span></span>

<span data-ttu-id="11b37-116">ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="11b37-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="11b37-117">Kapsanan veya dışlanan barındırma ortamı adlarına göre kapalı içeriğini işlemek bu öznitelikler denetim.</span><span class="sxs-lookup"><span data-stu-id="11b37-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="11b37-118">ASP.NET Core 2.0 ve üzeri dahil</span><span class="sxs-lookup"><span data-stu-id="11b37-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="11b37-119">`include` Özelliğine benzer bir davranışını `names` ASP.NET Core 1.0 özniteliği.</span><span class="sxs-lookup"><span data-stu-id="11b37-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="11b37-120">ASP.NET Core 2.0 ve üzeri Dışla</span><span class="sxs-lookup"><span data-stu-id="11b37-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="11b37-121">Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için kapalı içerik oluşturması.</span><span class="sxs-lookup"><span data-stu-id="11b37-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="11b37-122">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="11b37-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
