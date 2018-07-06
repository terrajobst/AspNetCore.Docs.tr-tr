---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma | Microsoft Docs
author: microsoft
description: OData v4 belirtimine göre bir karmaşık tür başka bir karmaşık türden devralabilir. (Bir karmaşık türü bir yapılandırılmış bir anahtar olmadan türdür.) Web API...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824036"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="2a651-104">ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma</span><span class="sxs-lookup"><span data-stu-id="2a651-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="2a651-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2a651-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2a651-106">OData v4 göre [belirtimi](http://www.odata.org/documentation/odata-version-4-0/), karmaşık bir tür, başka bir karmaşık türden devralabilir.</span><span class="sxs-lookup"><span data-stu-id="2a651-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="2a651-107">(A *karmaşık* türü olan bir anahtar olmadan yapılandırılmış bir tür.) Web API OData 5.3, karmaşık tür devralma destekler.</span><span class="sxs-lookup"><span data-stu-id="2a651-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="2a651-108">Bu konu, bir varlık veri modeli (EDM) derleme ile karmaşık devralma türleri nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="2a651-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="2a651-109">Tam kaynak kodunu görmek [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="2a651-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a651-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2a651-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2a651-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="2a651-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="2a651-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="2a651-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="2a651-113">Modeli hiyerarşisi</span><span class="sxs-lookup"><span data-stu-id="2a651-113">Model Hierarchy</span></span>

<span data-ttu-id="2a651-114">Karmaşık Tür devralma göstermek için aşağıdaki sınıf hiyerarşisi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2a651-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="2a651-115">`Shape` bir soyut karmaşık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="2a651-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="2a651-116">`Rectangle`, `Triangle`, ve `Circle` öğesinden türetilen karmaşık türler `Shape`, ve `RoundRectangle` türetildiği `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="2a651-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="2a651-117">`Window` bir varlık türü olduğu ve içeren bir `Shape` örneği.</span><span class="sxs-lookup"><span data-stu-id="2a651-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="2a651-118">Bu tür tanımlama CLR sınıfları şunlardır.</span><span class="sxs-lookup"><span data-stu-id="2a651-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="2a651-119">EDM modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="2a651-119">Build the EDM Model</span></span>

<span data-ttu-id="2a651-120">EDM oluşturmak için kullanabileceğiniz **ODataConventionModelBuilder**, kalıtım ilişkileri CLR türünden çıkarır.</span><span class="sxs-lookup"><span data-stu-id="2a651-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="2a651-121">Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="2a651-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="2a651-122">Bu, daha fazla kod sürer, ancak EDM üzerinde daha fazla denetim verir.</span><span class="sxs-lookup"><span data-stu-id="2a651-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="2a651-123">Bu iki örnek aynı EDM şema oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2a651-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="2a651-124">Meta veri belgesi</span><span class="sxs-lookup"><span data-stu-id="2a651-124">Metadata Document</span></span>

<span data-ttu-id="2a651-125">Karmaşık Tür devralma gösteren OData meta veri belgesi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2a651-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="2a651-126">Meta veri belgeden görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2a651-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="2a651-127">`Shape` Karmaşık türü Özet.</span><span class="sxs-lookup"><span data-stu-id="2a651-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="2a651-128">`Rectangle`, `Triangle`, Ve `Circle` karmaşık türün temel türünü sahip `Shape`.</span><span class="sxs-lookup"><span data-stu-id="2a651-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="2a651-129">`RoundRectangle` Türünde taban türü `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="2a651-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="2a651-130">Karmaşık türler atama</span><span class="sxs-lookup"><span data-stu-id="2a651-130">Casting Complex Types</span></span>

<span data-ttu-id="2a651-131">Karmaşık türlerde atama artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="2a651-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="2a651-132">Örneğin, aşağıdaki atamalardan sorgu bir `Shape` için bir `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="2a651-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="2a651-133">Yanıt yükünde şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="2a651-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
