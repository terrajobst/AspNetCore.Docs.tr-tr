---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API ile OData v4 karmaşık tür devralma | Microsoft Docs
author: microsoft
description: OData v4 belirtimine göre başka bir karmaşık türü bir karmaşık tür devralabilirsiniz. (Bir karmaşık türü bir anahtar olmadan yapılandırılmış bir tür olur.) Web API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566832"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="7b943-104">ASP.NET Web API ile OData v4 karmaşık tür devralma</span><span class="sxs-lookup"><span data-stu-id="7b943-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="7b943-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7b943-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7b943-106">OData v4 göre [belirtimi](http://www.odata.org/documentation/odata-version-4-0/), başka bir karmaşık türü bir karmaşık tür devralabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b943-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="7b943-107">(A *karmaşık* türü olan bir anahtar olmadan yapılandırılmış bir tür.) Web API OData 5.3 karmaşık tür devralma destekler.</span><span class="sxs-lookup"><span data-stu-id="7b943-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="7b943-108">Bu konu, bir varlık veri modeli (EDM) karmaşık devralma türleriyle nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7b943-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="7b943-109">Tam kaynak kodunu bkz [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="7b943-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7b943-110">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="7b943-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7b943-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="7b943-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="7b943-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="7b943-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="7b943-113">Modeli hiyerarşisi</span><span class="sxs-lookup"><span data-stu-id="7b943-113">Model Hierarchy</span></span>

<span data-ttu-id="7b943-114">Karmaşık Tür devralma göstermek için aşağıdaki sınıf hiyerarşisi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="7b943-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="7b943-115">`Shape`soyut bir karmaşık tür ' dir.</span><span class="sxs-lookup"><span data-stu-id="7b943-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="7b943-116">`Rectangle`, `Triangle`, ve `Circle` karmaşık türler türetilmiş `Shape`, ve `RoundRectangle` türetilen `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7b943-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="7b943-117">`Window`bir varlık türü olduğu ve içeren bir `Shape` örneği.</span><span class="sxs-lookup"><span data-stu-id="7b943-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="7b943-118">Burada, bu tür tanımlamak CLR sınıflarını bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7b943-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="7b943-119">EDM modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="7b943-119">Build the EDM Model</span></span>

<span data-ttu-id="7b943-120">EDM oluşturmak için kullanabileceğiniz **ODataConventionModelBuilder**, CLR türünden devralma ilişkisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7b943-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="7b943-121">Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="7b943-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="7b943-122">Bu, daha fazla kod alır, ancak EDM üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="7b943-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="7b943-123">Bu iki örnek aynı EDM şeması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7b943-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="7b943-124">Meta veri belgesi</span><span class="sxs-lookup"><span data-stu-id="7b943-124">Metadata Document</span></span>

<span data-ttu-id="7b943-125">Karmaşık Tür devralma gösteren OData meta veri belgesi aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="7b943-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="7b943-126">Meta veri belgeden görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7b943-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="7b943-127">`Shape` Karmaşık türü Özet.</span><span class="sxs-lookup"><span data-stu-id="7b943-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="7b943-128">`Rectangle`, `Triangle`, Ve `Circle` karmaşık türün temel türünü sahip `Shape`.</span><span class="sxs-lookup"><span data-stu-id="7b943-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="7b943-129">`RoundRectangle` Türüne sahip temel türü `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7b943-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="7b943-130">Karmaşık türler atama</span><span class="sxs-lookup"><span data-stu-id="7b943-130">Casting Complex Types</span></span>

<span data-ttu-id="7b943-131">Karmaşık türler üzerinde atama artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="7b943-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="7b943-132">Örneğin, aşağıdaki atamalar sorgu bir `Shape` için bir `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7b943-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="7b943-133">Yanıt yükü şöyledir:</span><span class="sxs-lookup"><span data-stu-id="7b943-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
