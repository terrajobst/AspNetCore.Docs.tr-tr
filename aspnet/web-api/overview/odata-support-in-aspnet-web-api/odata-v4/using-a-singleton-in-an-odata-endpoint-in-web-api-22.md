---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 kullanarak OData v4 Teklide oluşturma | Microsoft Docs
author: rick-anderson
description: Bu konuda, tek bir OData uç Web API'si 2.2 tanımlamak gösterilmiştir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566769"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="d5407-103">Web API 2.2 kullanarak OData v4 Teklide oluşturma</span><span class="sxs-lookup"><span data-stu-id="d5407-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="d5407-104">Zoe Luo tarafından</span><span class="sxs-lookup"><span data-stu-id="d5407-104">by Zoe Luo</span></span>

> <span data-ttu-id="d5407-105">Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5407-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="d5407-106">Ancak OData v4 Singleton ve her ikisi de Webapı 2.2 destekleyen kapsama olmak üzere iki ek seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5407-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="d5407-107">Bu makalede, tek bir OData uç Web API'si 2.2 tanımlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d5407-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="d5407-108">Hangi bir tek ve bunu kullanarak nasıl yardımcı olabileceğini hakkında daha fazla bilgi için bkz: [özel varlığınız tanımlamak için bir singleton kullanarak](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5407-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="d5407-109">Web API'de OData V4 uç noktası oluşturmak için bkz: [bir OData v4 uç nokta kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d5407-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="d5407-110">Aşağıdaki veri modelini kullanarak Web API projesinde tek oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="d5407-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Veri modeli](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="d5407-112">Adlı bir singleton `Umbrella` türüne bağlı olarak tanımlanacak `Company`ve bir varlık adlandırılmış kümesi `Employees` türüne bağlı olarak tanımlanacak `Employee`.</span><span class="sxs-lookup"><span data-stu-id="d5407-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="d5407-113">Bu öğreticide kullanılan çözüm yüklenebilir [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="d5407-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="d5407-114">Veri modelini tanımlama</span><span class="sxs-lookup"><span data-stu-id="d5407-114">Define the data model</span></span>

1. <span data-ttu-id="d5407-115">CLR Türleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d5407-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="d5407-116">CLR türlerine göre EDM modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5407-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="d5407-117">Burada, `builder.Singleton<Company>("Umbrella")` adlı bir singleton oluşturmak için model Oluşturucu söyler `Umbrella` EDM modeli.</span><span class="sxs-lookup"><span data-stu-id="d5407-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="d5407-118">Oluşturulan meta verileri aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="d5407-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="d5407-119">Meta verileri, görebiliriz gezinti özelliği `Company` içinde `Employees` varlık kümesi için singleton bağlı `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="d5407-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="d5407-120">Bağlama tarafından otomatik olarak yapılır `ODataConventionModelBuilder`, bu yana yalnızca `Umbrella` sahip `Company` türü.</span><span class="sxs-lookup"><span data-stu-id="d5407-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="d5407-121">Modelde bir belirsizlik ise kullanabilirsiniz `HasSingletonBinding` açıkça bir tekliye; bir gezinti özelliği bağlamak için `HasSingletonBinding` kullanarak aynı etkiye sahip `Singleton` CLR türü tanımı özniteliğinde:</span><span class="sxs-lookup"><span data-stu-id="d5407-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="d5407-122">Singleton denetleyicisi tanımlayın</span><span class="sxs-lookup"><span data-stu-id="d5407-122">Define the singleton controller</span></span>

<span data-ttu-id="d5407-123">Singleton denetleyicisi devraldığı EntitySet denetleyicisi gibi `ODataController`, ve singleton Denetleyici adı olmalıdır `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="d5407-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="d5407-124">Farklı türde isteklerini işlemek için Eylemler denetleyicide önceden tanımlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5407-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="d5407-125">**Öznitelik yönlendirme** Webapı 2.2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="d5407-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="d5407-126">Örneğin, sorgulama işlemek için bir eylem tanımlamak için `Revenue` gelen `Company` özniteliği yönlendirme, kullanımı aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="d5407-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="d5407-127">Her eylem öznitelik tanımlamak istekli değilse, aşağıdaki eylemleri tanımlamak [OData yönlendirme kuralları](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="d5407-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="d5407-128">Bir anahtar tek sorgulamak için gerekli olmadığından, singleton denetleyicide tanımlanan Eylemler entityset denetleyicide tanımlanan Eylemler biraz farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d5407-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="d5407-129">Başvuru için her tek denetleyici eylem tanımında yöntemi imzaları aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="d5407-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="d5407-130">Temel olarak, tüm hizmet tarafında yapmanız gereken budur.</span><span class="sxs-lookup"><span data-stu-id="d5407-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="d5407-131">[Örnek proje](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) tüm çözüm ve tekli kullanmayı gösterir OData istemci kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="d5407-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="d5407-132">İçindeki adımları izleyerek istemci yerleşik [bir OData v4 istemci uygulaması oluşturma](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="d5407-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="d5407-133">biçimindeki telefon numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="d5407-133">.</span></span> 

<span data-ttu-id="d5407-134">*Bu makalenin özgün içerik için Leo Hu teşekkür ederiz.*</span><span class="sxs-lookup"><span data-stu-id="d5407-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
