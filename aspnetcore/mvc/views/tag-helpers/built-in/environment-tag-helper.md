---
title: ASP.NET Core ortam etiketi yok
author: pkellner
description: "Tüm özellikleri dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="78ff9-103">ASP.NET Core ortam etiketi yok</span><span class="sxs-lookup"><span data-stu-id="78ff9-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="78ff9-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="78ff9-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="78ff9-105">Ortam etiketi yardımcı koşullu geçerli barındırma ortamına bağlı kapalı içeriği işler.</span><span class="sxs-lookup"><span data-stu-id="78ff9-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="78ff9-106">Tek öznitelik `names` ortamı virgülle ayrılmış listesi olduğu ad, herhangi bir geçerli ortama eşleşen, işlenmek üzere kapalı içerik tetikler.</span><span class="sxs-lookup"><span data-stu-id="78ff9-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="78ff9-107">Ortam etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="78ff9-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="78ff9-108">adlar</span><span class="sxs-lookup"><span data-stu-id="78ff9-108">names</span></span>

<span data-ttu-id="78ff9-109">Tek bir barındırma ortamı ad veya kapalı içerik çizmeye tetiklemek ortam adları barındırma virgülle ayrılmış listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="78ff9-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="78ff9-110">Bu değer ASP.NET Core statik özelliğinden döndürülen geçerli değer karşılaştırılır `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="78ff9-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="78ff9-111">Bu değer şunlardan biri değil: **hazırlama**; **Geliştirme** veya **üretim**.</span><span class="sxs-lookup"><span data-stu-id="78ff9-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="78ff9-112">Karşılaştırma durumu yok sayar.</span><span class="sxs-lookup"><span data-stu-id="78ff9-112">The comparison ignores case.</span></span>

<span data-ttu-id="78ff9-113">Geçerli bir örneği `environment` etiketi yardımcı:</span><span class="sxs-lookup"><span data-stu-id="78ff9-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="78ff9-114">eklemek veya çıkartmak öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="78ff9-114">include and exclude attributes</span></span>

<span data-ttu-id="78ff9-115">ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="78ff9-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="78ff9-116">Kapsanan veya dışlanan barındırma ortamı adlarına göre kapalı içeriğini işlemek bu öznitelikler denetim.</span><span class="sxs-lookup"><span data-stu-id="78ff9-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="78ff9-117">ASP.NET Core 2.0 ve üzeri dahil</span><span class="sxs-lookup"><span data-stu-id="78ff9-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="78ff9-118">`include` Özelliğine benzer bir davranışını `names` ASP.NET Core 1.0 özniteliği.</span><span class="sxs-lookup"><span data-stu-id="78ff9-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="78ff9-119">ASP.NET Core 2.0 ve üzeri Dışla</span><span class="sxs-lookup"><span data-stu-id="78ff9-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="78ff9-120">Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için kapalı içerik oluşturması.</span><span class="sxs-lookup"><span data-stu-id="78ff9-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="78ff9-121">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="78ff9-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
