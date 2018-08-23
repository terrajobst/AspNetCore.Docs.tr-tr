---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma | Microsoft Docs
author: microsoft
description: OData v4 belirtimine göre bir karmaşık tür başka bir karmaşık türden devralabilir. (Bir karmaşık türü bir yapılandırılmış bir anahtar olmadan türdür.) Web API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752351"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="ecc7f-104">ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma</span><span class="sxs-lookup"><span data-stu-id="ecc7f-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="ecc7f-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ecc7f-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ecc7f-106">OData v4 göre [belirtimi](http://www.odata.org/documentation/odata-version-4-0/), karmaşık bir tür, başka bir karmaşık türden devralabilir.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="ecc7f-107">(A *karmaşık* türü olan bir anahtar olmadan yapılandırılmış bir tür.) Web API OData 5.3, karmaşık tür devralma destekler.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="ecc7f-108">Bu konu, bir varlık veri modeli (EDM) derleme ile karmaşık devralma türleri nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="ecc7f-109">Tam kaynak kodunu görmek [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="ecc7f-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ecc7f-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ecc7f-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ecc7f-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="ecc7f-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="ecc7f-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="ecc7f-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="ecc7f-113">Modeli hiyerarşisi</span><span class="sxs-lookup"><span data-stu-id="ecc7f-113">Model Hierarchy</span></span>

<span data-ttu-id="ecc7f-114">Karmaşık Tür devralma göstermek için aşağıdaki sınıf hiyerarşisi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="ecc7f-115">`Shape` bir soyut karmaşık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="ecc7f-116">`Rectangle`, `Triangle`, ve `Circle` öğesinden türetilen karmaşık türler `Shape`, ve `RoundRectangle` türetildiği `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="ecc7f-117">`Window` bir varlık türü olduğu ve içeren bir `Shape` örneği.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="ecc7f-118">Bu tür tanımlama CLR sınıfları şunlardır.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="ecc7f-119">EDM modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="ecc7f-119">Build the EDM Model</span></span>

<span data-ttu-id="ecc7f-120">EDM oluşturmak için kullanabileceğiniz **ODataConventionModelBuilder**, kalıtım ilişkileri CLR türünden çıkarır.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="ecc7f-121">Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="ecc7f-122">Bu, daha fazla kod sürer, ancak EDM üzerinde daha fazla denetim verir.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="ecc7f-123">Bu iki örnek aynı EDM şema oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="ecc7f-124">Meta veri belgesi</span><span class="sxs-lookup"><span data-stu-id="ecc7f-124">Metadata Document</span></span>

<span data-ttu-id="ecc7f-125">Karmaşık Tür devralma gösteren OData meta veri belgesi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="ecc7f-126">Meta veri belgeden görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ecc7f-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="ecc7f-127">`Shape` Karmaşık türü Özet.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="ecc7f-128">`Rectangle`, `Triangle`, Ve `Circle` karmaşık türün temel türünü sahip `Shape`.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="ecc7f-129">`RoundRectangle` Türünde taban türü `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="ecc7f-130">Karmaşık türler atama</span><span class="sxs-lookup"><span data-stu-id="ecc7f-130">Casting Complex Types</span></span>

<span data-ttu-id="ecc7f-131">Karmaşık türlerde atama artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="ecc7f-132">Örneğin, aşağıdaki atamalardan sorgu bir `Shape` için bir `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ecc7f-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="ecc7f-133">Yanıt yükünde şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="ecc7f-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
