---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Web API 2.2 kullanma kapsama OData v4 içinde | Microsoft Docs"
author: rick-anderson
description: "Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir. Ancak OData v4 Singleton ve Con olmak üzere iki ek seçenekler sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="f931e-104">OData v4 içinde kapsama Web API 2.2 kullanma</span><span class="sxs-lookup"><span data-stu-id="f931e-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="f931e-105">tarafından Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="f931e-105">by Jinfu Tan</span></span>

> <span data-ttu-id="f931e-106">Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f931e-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="f931e-107">Ancak OData v4 Singleton ve her ikisi de Webapı 2.2 destekleyen kapsama olmak üzere iki ek seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="f931e-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="f931e-108">Bu konuda, bir kapsama Webapı 2.2 içinde bir OData uç noktasını tanımlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f931e-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="f931e-109">Kapsama hakkında daha fazla bilgi için bkz: [kapsama ile OData v4 yakında](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="f931e-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="f931e-110">Web API'de OData V4 uç noktası oluşturmak için bkz: [bir OData v4 uç nokta kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f931e-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="f931e-111">İlk olarak, bir kapsama etki alanı modeli OData hizmeti bu veri modelini kullanarak oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="f931e-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Veri modeli](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="f931e-113">Bir hesap birçok PaymentInstruments (PI) içerir, ancak bir varlık için bir PI kümesine tanımlarız yok.</span><span class="sxs-lookup"><span data-stu-id="f931e-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="f931e-114">Bunun yerine, yönergelerinin yalnızca bir hesap erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f931e-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="f931e-115">Bu konudan kullanılan çözüm indirebilirsiniz [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="f931e-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="f931e-116">Veri modeli tanımlama</span><span class="sxs-lookup"><span data-stu-id="f931e-116">Defining the data model</span></span>

1. <span data-ttu-id="f931e-117">CLR Türleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="f931e-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="f931e-118">`Contained` Özniteliği kapsama Gezinti özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f931e-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="f931e-119">CLR türlerine göre EDM modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f931e-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="f931e-120">`ODataConventionModelBuilder` Varsa EDM modelini oluşturma işleyecek `Contained` özniteliği, karşılık gelen gezinme özelliğini eklenir.</span><span class="sxs-lookup"><span data-stu-id="f931e-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="f931e-121">Özelliği bir koleksiyon türü ise bir `GetCount(string NameContains)` işlevi de oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="f931e-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="f931e-122">Oluşturulan meta verileri aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="f931e-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="f931e-123">`ContainsTarget` Özniteliği gezinme özelliğinin bir kapsama olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="f931e-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="f931e-124">İçeren varlık kümesi denetleyicisi tanımlayın</span><span class="sxs-lookup"><span data-stu-id="f931e-124">Define the containing entity set controller</span></span>

<span data-ttu-id="f931e-125">Kapsanan varlıklar kendi denetleyicisi gerekmez; eylemi içeren varlık kümesi denetleyicide tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="f931e-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="f931e-126">Bu örnekte, bir AccountsController, ancak hiçbir PaymentInstrumentsController yoktur.</span><span class="sxs-lookup"><span data-stu-id="f931e-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="f931e-127">OData yolu kesimleri 4 veya daha fazla ise, yalnızca Yönlendirme works gibi özniteliği `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` yukarıdaki denetleyicideki.</span><span class="sxs-lookup"><span data-stu-id="f931e-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="f931e-128">Aksi takdirde, hem öznitelik hem de Geleneksel yönlendirme çalışır: Örneğin, `GetPayInPIs(int key)` eşleşen `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="f931e-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="f931e-129">*Bu makalenin özgün içerik için Leo Hu teşekkür ederiz.*</span><span class="sxs-lookup"><span data-stu-id="f931e-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
