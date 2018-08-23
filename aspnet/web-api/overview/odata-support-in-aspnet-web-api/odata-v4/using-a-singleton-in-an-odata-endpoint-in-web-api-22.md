---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 kullanma OData v4 sürümünde Singleton oluşturma | Microsoft Docs
author: rick-anderson
description: Bu konuda, tek bir OData uç noktası, Web API 2.2 tanımlamak gösterilmektedir.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7562a90ae34b216dca2dd3cf541d086585735212
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752732"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="406f6-103">Web API 2.2 kullanma OData v4 sürümünde Singleton oluşturma</span><span class="sxs-lookup"><span data-stu-id="406f6-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="406f6-104">Zoe Luo tarafından</span><span class="sxs-lookup"><span data-stu-id="406f6-104">by Zoe Luo</span></span>

> <span data-ttu-id="406f6-105">Geleneksel olarak, bir varlık kümesi içinde kapsüllenmiş, bir varlığın yalnızca erişilemedi.</span><span class="sxs-lookup"><span data-stu-id="406f6-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="406f6-106">Ancak, OData v4 tekil ve kapsama, ikisi için de Webapı 2.2 destekleyen iki ek seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="406f6-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="406f6-107">Bu makalede, tek bir OData uç noktası, Web API 2.2 tanımlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="406f6-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="406f6-108">Hangi bir tek ve onu kullanarak kazanabileceğinizi hakkında daha fazla bilgi için bkz. [, özel bir varlık tanımlamak için singleton kullanarak](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="406f6-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="406f6-109">Web API'de OData V4 uç nokta oluşturmak için bkz [OData v4 uç noktası kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="406f6-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="406f6-110">Aşağıdaki veri modelini kullanarak Web API projesinde bir singleton oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="406f6-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Veri modeli](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="406f6-112">Adlı bir singleton `Umbrella` türüne göre tanımlanır `Company`ve bir varlık adlandırılmış kümesi `Employees` türüne göre tanımlanır `Employee`.</span><span class="sxs-lookup"><span data-stu-id="406f6-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="406f6-113">Bu öğreticide kullanılan çözüm indirilebileceğini [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="406f6-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="406f6-114">Veri modelini tanımlama</span><span class="sxs-lookup"><span data-stu-id="406f6-114">Define the data model</span></span>

1. <span data-ttu-id="406f6-115">CLR Türleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="406f6-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="406f6-116">CLR türlerine göre EDM modelini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="406f6-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="406f6-117">Burada, `builder.Singleton<Company>("Umbrella")` adlı bir singleton oluşturmak için model Oluşturucu bildirir `Umbrella` EDM modeli.</span><span class="sxs-lookup"><span data-stu-id="406f6-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="406f6-118">Oluşturulan meta veriler aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="406f6-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="406f6-119">Meta verileri görebiliriz gezinme özelliğini `Company` içinde `Employees` varlık kümesi için tekli bağlı `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="406f6-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="406f6-120">Bağlama tarafından otomatik olarak yapılır `ODataConventionModelBuilder`, bu yana yalnızca `Umbrella` sahip `Company` türü.</span><span class="sxs-lookup"><span data-stu-id="406f6-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="406f6-121">Modelde belirsizlik varsa, kullanabileceğiniz `HasSingletonBinding` açıkça bir gezinti özelliği için singleton; bağlamak için `HasSingletonBinding` kullanmakla aynı etkiye sahip `Singleton` CLR tür tanımında öznitelik:</span><span class="sxs-lookup"><span data-stu-id="406f6-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="406f6-122">Singleton denetleyicisi tanımlayın</span><span class="sxs-lookup"><span data-stu-id="406f6-122">Define the singleton controller</span></span>

<span data-ttu-id="406f6-123">Singleton denetleyicisi devraldığı EntitySet denetleyicisi gibi `ODataController`, ve tekil Denetleyici adı olması gereken `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="406f6-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="406f6-124">Farklı türde istekleri işlemek için Eylemler denetleyicide önceden tanımlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="406f6-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="406f6-125">**Öznitelik yönlendirme** Webapı 2.2 içinde varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="406f6-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="406f6-126">Örneğin, sorgulama işlemek için bir eylem tanımlamak için `Revenue` gelen `Company` öznitelik yönlendirme, kullanarak aşağıdaki kullanın:</span><span class="sxs-lookup"><span data-stu-id="406f6-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="406f6-127">Her eylem için öznitelikleri tanımlamak iradeye sahip değilse, aşağıdaki eylemleri tanımlamak [OData yönlendirme kuralları](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="406f6-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="406f6-128">Bir anahtarı bir singleton sorgulamak için gerekli olmadığından, tekil denetleyicide tanımlanan biraz farklı eylemler entityset denetleyicide tanımlanan eylemlerdir.</span><span class="sxs-lookup"><span data-stu-id="406f6-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="406f6-129">Başvuru için her eylem tanımı tekil denetleyicideki için yöntem imzaları aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="406f6-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="406f6-130">Temel olarak, tüm hizmet tarafında yapmanız gereken budur.</span><span class="sxs-lookup"><span data-stu-id="406f6-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="406f6-131">[Kodunuzla](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) tüm çözüm ve tekli kullanmayı gösterir OData istemci kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="406f6-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="406f6-132">İçindeki adımları izleyerek istemci yerleşik [bir OData v4 istemci uygulaması oluşturma](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="406f6-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="406f6-133">biçimindeki telefon numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="406f6-133">.</span></span> 

<span data-ttu-id="406f6-134">*Bu makalede, özgün içerik sayesinde Hu satılmıştır.*</span><span class="sxs-lookup"><span data-stu-id="406f6-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
