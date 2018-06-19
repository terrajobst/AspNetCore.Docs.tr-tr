---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API'de HttpClient ileti işleyicileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566343"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="62013-102">ASP.NET Web API'de HttpClient ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="62013-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="62013-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="62013-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="62013-104">A *ileti işleyicisi* bir HTTP isteğini alır ve bir HTTP yanıtı döndüren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="62013-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="62013-105">Genellikle, bir dizi ileti işleyicileri zincirleme birlikte.</span><span class="sxs-lookup"><span data-stu-id="62013-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="62013-106">İlk işleyici bir HTTP isteği aldığında, bir işlem yapar ve sonraki işleyicisi isteği verir.</span><span class="sxs-lookup"><span data-stu-id="62013-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="62013-107">Belirli bir noktada yanıtı oluşturulur ve tekrar zincirinde geçer.</span><span class="sxs-lookup"><span data-stu-id="62013-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="62013-108">Bu desen adlı bir *temsilci* işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="62013-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="62013-109">İstemci tarafında **HttpClient** sınıfı isteklerini işlemek için bir ileti işleyicisini kullanır.</span><span class="sxs-lookup"><span data-stu-id="62013-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="62013-110">Varsayılan işleyici **HttpClientHandler**, ağ üzerinden isteği gönderir ve sunucudan yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="62013-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="62013-111">Özel ileti işleyicileri istemci ardışık düzenine ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="62013-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="62013-112">ASP.NET Web API, sunucu tarafında ileti işleyicileri de kullanır.</span><span class="sxs-lookup"><span data-stu-id="62013-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="62013-113">Daha fazla bilgi için bkz: [HTTP ileti işleyicileri](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="62013-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="62013-114">Özel ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="62013-114">Custom Message Handlers</span></span>

<span data-ttu-id="62013-115">Özel ileti işleyicisi yazma için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62013-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="62013-116">Yöntem imzası şöyledir:</span><span class="sxs-lookup"><span data-stu-id="62013-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="62013-117">Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="62013-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="62013-118">Tipik bir uygulama şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="62013-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="62013-119">İşlem istek iletisi.</span><span class="sxs-lookup"><span data-stu-id="62013-119">Process the request message.</span></span>
2. <span data-ttu-id="62013-120">Çağrı `base.SendAsync` iç işleyici isteği göndermek için.</span><span class="sxs-lookup"><span data-stu-id="62013-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="62013-121">İç işleyici bir yanıt iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="62013-121">The inner handler returns a response message.</span></span> <span data-ttu-id="62013-122">(Bu adım, zaman uyumsuz bağlıdır.)</span><span class="sxs-lookup"><span data-stu-id="62013-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="62013-123">Yanıtı işlemek ve çağırana döndürür.</span><span class="sxs-lookup"><span data-stu-id="62013-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="62013-124">Aşağıdaki örnek, bir özel üst bilgi giden isteğe ekler bir ileti işleyicisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="62013-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="62013-125">Çağrı `base.SendAsync` zaman uyumsuz olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="62013-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="62013-126">İşleyici bu çağrısından sonra herhangi bir iş varsa, kullanmak **await** yöntemi tamamlandıktan sonra yürütme devam etmek için anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="62013-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="62013-127">Aşağıdaki örnek hata kodları günlüklerini bir işleyici gösterir.</span><span class="sxs-lookup"><span data-stu-id="62013-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="62013-128">Günlük çok ilginç değildir, ancak örnek işleyici içinde yanıt alın gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="62013-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="62013-129">İstemci ardışık düzene ileti işleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="62013-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="62013-130">Özel işleyicileri eklemek için **HttpClient**, kullanın **HttpClientFactory.Create** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="62013-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="62013-131">İleti işleyicileri bunlara geçirdiğiniz sırayla çağrılır **oluşturma** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62013-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="62013-132">Yanıt iletisi işleyicileri iç içe geçmiş olduğundan, diğer yönde hareket eder.</span><span class="sxs-lookup"><span data-stu-id="62013-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="62013-133">Diğer bir deyişle, son yanıt iletisi ilk işleyicidir.</span><span class="sxs-lookup"><span data-stu-id="62013-133">That is, the last handler is the first to get the response message.</span></span>
