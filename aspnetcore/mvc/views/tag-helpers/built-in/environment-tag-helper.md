---
title: ASP.NET Core ortam etiketi yok
author: pkellner
description: Tüm özellikleri dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 05c07b06a4fedac0b0ff39d168807f5e2e6996cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276922"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="dfd22-103">ASP.NET Core ortam etiketi yok</span><span class="sxs-lookup"><span data-stu-id="dfd22-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dfd22-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="dfd22-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="dfd22-105">Ortam etiketi yardımcı koşullu geçerli barındırma ortamına bağlı kapalı içeriği işler.</span><span class="sxs-lookup"><span data-stu-id="dfd22-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="dfd22-106">Tek öznitelik `names` ortamı virgülle ayrılmış listesi olduğu ad, herhangi bir geçerli ortama eşleşen, işlenmek üzere kapalı içerik tetikler.</span><span class="sxs-lookup"><span data-stu-id="dfd22-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="dfd22-107">Ortam etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="dfd22-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="dfd22-108">adlar</span><span class="sxs-lookup"><span data-stu-id="dfd22-108">names</span></span>

<span data-ttu-id="dfd22-109">Tek bir barındırma ortamı ad veya kapalı içerik çizmeye tetiklemek ortam adları barındırma virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="dfd22-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="dfd22-110">Bu değer ASP.NET Core statik özelliğinden döndürülen geçerli değer karşılaştırılır `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="dfd22-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="dfd22-111">Bu değer şunlardan biri değil: **hazırlama**; **Geliştirme** veya **üretim**.</span><span class="sxs-lookup"><span data-stu-id="dfd22-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="dfd22-112">Karşılaştırma durumu yok sayar.</span><span class="sxs-lookup"><span data-stu-id="dfd22-112">The comparison ignores case.</span></span>

<span data-ttu-id="dfd22-113">Geçerli bir örneği `environment` etiketi yardımcı:</span><span class="sxs-lookup"><span data-stu-id="dfd22-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="dfd22-114">eklemek veya çıkartmak öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dfd22-114">include and exclude attributes</span></span>

<span data-ttu-id="dfd22-115">ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="dfd22-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="dfd22-116">Kapsanan veya dışlanan barındırma ortamı adlarına göre kapalı içeriğini işlemek bu öznitelikler denetim.</span><span class="sxs-lookup"><span data-stu-id="dfd22-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="dfd22-117">ASP.NET Core 2.0 ve üzeri dahil</span><span class="sxs-lookup"><span data-stu-id="dfd22-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="dfd22-118">`include` Özelliğine benzer bir davranışını `names` ASP.NET Core 1.0 özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dfd22-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="dfd22-119">ASP.NET Core 2.0 ve üzeri Dışla</span><span class="sxs-lookup"><span data-stu-id="dfd22-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="dfd22-120">Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için kapalı içerik oluşturması.</span><span class="sxs-lookup"><span data-stu-id="dfd22-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="dfd22-121">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dfd22-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
