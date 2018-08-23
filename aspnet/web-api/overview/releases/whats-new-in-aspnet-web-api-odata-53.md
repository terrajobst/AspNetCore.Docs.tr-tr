---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753883"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="751c7-102">ASP.NET Web API OData 5.3 sürümündeki yenilikler</span><span class="sxs-lookup"><span data-stu-id="751c7-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="751c7-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="751c7-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="751c7-104">Bu konuda, ASP.NET Web API OData 5.3 için Yenilikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="751c7-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="751c7-105">İndir</span><span class="sxs-lookup"><span data-stu-id="751c7-105">Download</span></span>](#download)
- [<span data-ttu-id="751c7-106">Belgeler</span><span class="sxs-lookup"><span data-stu-id="751c7-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="751c7-107">OData çekirdek kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="751c7-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="751c7-108">Yeni Özellikler</span><span class="sxs-lookup"><span data-stu-id="751c7-108">New Features</span></span>](#newf)
- [<span data-ttu-id="751c7-109">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="751c7-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="751c7-110">Hata düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="751c7-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="751c7-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="751c7-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="751c7-112">İndir</span><span class="sxs-lookup"><span data-stu-id="751c7-112">Download</span></span>

<span data-ttu-id="751c7-113">Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="751c7-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="751c7-114">NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:</span><span class="sxs-lookup"><span data-stu-id="751c7-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="751c7-115">Belgeler</span><span class="sxs-lookup"><span data-stu-id="751c7-115">Documentation</span></span>

<span data-ttu-id="751c7-116">Öğreticilerde ve diğer belgelerde ASP.NET Web API OData hakkında bulabilirsiniz [ASP.NET web sitesi](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="751c7-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="751c7-117">OData çekirdek kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="751c7-117">OData Core Libraries</span></span>

<span data-ttu-id="751c7-118">OData v4 için Web API artık ODataLib sürüm 6.5.0 kullanır.</span><span class="sxs-lookup"><span data-stu-id="751c7-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="751c7-119">Yeni özellikleri ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="751c7-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="751c7-120">$ $Levels genişletmek için destek</span><span class="sxs-lookup"><span data-stu-id="751c7-120">Support for $levels in $expand</span></span>

<span data-ttu-id="751c7-121">$Levels kullanabileceğiniz sorgu seçeneği $ sorguları genişletin.</span><span class="sxs-lookup"><span data-stu-id="751c7-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="751c7-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="751c7-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="751c7-123">Bu sorgu için eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="751c7-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="751c7-124">Açık varlık türleri için destek</span><span class="sxs-lookup"><span data-stu-id="751c7-124">Support for Open Entity Types</span></span>

<span data-ttu-id="751c7-125">Bir *açık tür* ek tür tanımında bildirilen herhangi bir özelliği olarak dinamik özellikleri içeren bir stuctured türüdür.</span><span class="sxs-lookup"><span data-stu-id="751c7-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="751c7-126">Açık türler, veri modelleri için esneklik eklemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="751c7-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="751c7-127">Daha fazla bilgi için xxxx bakın.</span><span class="sxs-lookup"><span data-stu-id="751c7-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="751c7-128">Dinamik koleksiyon özelliklerinde açık türler için destek</span><span class="sxs-lookup"><span data-stu-id="751c7-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="751c7-129">Daha önce bir dinamik özellik tek bir değer olması gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="751c7-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="751c7-130">5.3 içinde dinamik özellikler koleksiyonu değerlere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="751c7-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="751c7-131">Örneğin, aşağıdaki JSON yükü olarak `Emails` dinamik bir özelliktir ve dize türü koleksiyonudur:</span><span class="sxs-lookup"><span data-stu-id="751c7-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="751c7-132">Karmaşık türleri için devralma desteği</span><span class="sxs-lookup"><span data-stu-id="751c7-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="751c7-133">Artık karmaşık türler, temel türünden devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="751c7-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="751c7-134">Örneğin, bir OData hizmeti aşağıdaki karmaşık türler tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="751c7-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="751c7-135">Bu örneğin EDM şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="751c7-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="751c7-136">Daha fazla bilgi için [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="751c7-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="751c7-137">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="751c7-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="751c7-138">Bu bölümde, bilinen sorunlar ve ASP.NET Web API OData 5.3 bozucu değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="751c7-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="751c7-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="751c7-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="751c7-140">Sorgu Seçenekleri</span><span class="sxs-lookup"><span data-stu-id="751c7-140">Query Options</span></span>

<span data-ttu-id="751c7-141">Sorun: $levels ile genişletin kullanarak iç içe geçmiş $ bir yanlış genişletme derinliğini, en büyük sonuç =.</span><span class="sxs-lookup"><span data-stu-id="751c7-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="751c7-142">Örneğin, aşağıdaki isteği verilen:</span><span class="sxs-lookup"><span data-stu-id="751c7-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="751c7-143">Varsa `MaxExpansionDepth` 5'tir, bu sorgu 6 bir genişletme derinliğini neden olur.</span><span class="sxs-lookup"><span data-stu-id="751c7-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="751c7-144">Hata düzeltmeleri ve alt özellik güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="751c7-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="751c7-145">Bu sürüm çeşitli hata düzeltmeleri ve küçük özelliği içerir güncelleştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="751c7-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="751c7-146">Tam listesini burada bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="751c7-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="751c7-147">Hata düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="751c7-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="751c7-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="751c7-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="751c7-149">Bu sürümde yaptık bir [hata düzeltmesi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) AllowedFunctions numaralandırmalar bazılarının.</span><span class="sxs-lookup"><span data-stu-id="751c7-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="751c7-150">Bu sürüm, herhangi bir hata düzeltmeleri veya yeni özellikleri içermez.</span><span class="sxs-lookup"><span data-stu-id="751c7-150">This release doesn't have any other bug fixes or new features.</span></span>
