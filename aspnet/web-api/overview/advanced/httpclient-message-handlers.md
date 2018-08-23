---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API'de HttpClient ileti işleyicileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754014"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="e2faf-102">ASP.NET Web API'de HttpClient ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e2faf-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e2faf-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e2faf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e2faf-104">A *ileti işleyicisi* HTTP isteği aldığında ve bir HTTP yanıtı döndüren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e2faf-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="e2faf-105">Genellikle, bir dizi ileti işleyicileri zincirleme birlikte.</span><span class="sxs-lookup"><span data-stu-id="e2faf-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="e2faf-106">İlk işleyici HTTP isteği aldığında, işlem yapar ve bir sonraki işleyici istek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e2faf-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="e2faf-107">Belirli bir noktada yanıt oluşturulur ve zincirinde döner.</span><span class="sxs-lookup"><span data-stu-id="e2faf-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="e2faf-108">Bu düzen adı verilen bir *temsilci olarak görevlendirme* işleyici.</span><span class="sxs-lookup"><span data-stu-id="e2faf-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="e2faf-109">İstemci tarafında **HttpClient** sınıfını isteklerini işlemek için bir ileti işleyicisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e2faf-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="e2faf-110">Varsayılan işleyici **HttpClientHandler**, ağ üzerinden isteği gönderir ve sunucudan yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="e2faf-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="e2faf-111">Özel ileti işleyicileri istemci ardışık düzende ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e2faf-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="e2faf-112">ASP.NET Web API, sunucu tarafında ileti işleyicileri de kullanır.</span><span class="sxs-lookup"><span data-stu-id="e2faf-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="e2faf-113">Daha fazla bilgi için [HTTP ileti işleyicileri](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="e2faf-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="e2faf-114">Özel ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e2faf-114">Custom Message Handlers</span></span>

<span data-ttu-id="e2faf-115">Özel ileti işleyicisi yazmak için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e2faf-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="e2faf-116">Aşağıda, yöntem imzası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e2faf-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="e2faf-117">Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e2faf-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="e2faf-118">Tipik bir uygulaması şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="e2faf-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="e2faf-119">İşlem istek iletisi.</span><span class="sxs-lookup"><span data-stu-id="e2faf-119">Process the request message.</span></span>
2. <span data-ttu-id="e2faf-120">Çağrı `base.SendAsync` iç işleyici için istek gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2faf-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="e2faf-121">İç işleyici, bir yanıt iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="e2faf-121">The inner handler returns a response message.</span></span> <span data-ttu-id="e2faf-122">(Bu adım, zaman uyumsuz.)</span><span class="sxs-lookup"><span data-stu-id="e2faf-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="e2faf-123">Yanıta işlemek ve çağırana döndürmesi.</span><span class="sxs-lookup"><span data-stu-id="e2faf-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="e2faf-124">Aşağıdaki örnek, bir giden istek için özel bir başlık ekler bir ileti işleyicisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="e2faf-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="e2faf-125">Çağrı `base.SendAsync` zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="e2faf-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="e2faf-126">İşleyici, bu çağrıdan sonra herhangi bir iş varsa, kullanmak **await** yöntemi tamamlandıktan sonra yürütülmesine devam etmek için anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="e2faf-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="e2faf-127">Aşağıdaki örnek, hata kodları günlükleri bir işleyici gösterir.</span><span class="sxs-lookup"><span data-stu-id="e2faf-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="e2faf-128">Günlük çok ilginç değil, ancak örnek işleyici içinde yanıt alın nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="e2faf-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="e2faf-129">İstemci işlem hattına ileti işleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="e2faf-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="e2faf-130">Özel işleyicileri eklemek için **HttpClient**, kullanın **HttpClientFactory.Create** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e2faf-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="e2faf-131">İleti işleyicileri içine geçirdiğiniz sırayla çağrılır **Oluştur** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e2faf-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="e2faf-132">Yanıt iletisi işleyicileri nedeniyle, diğer yönde hareket eder.</span><span class="sxs-lookup"><span data-stu-id="e2faf-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="e2faf-133">Diğer bir deyişle, son ilk yanıt iletisi işleyicidir.</span><span class="sxs-lookup"><span data-stu-id="e2faf-133">That is, the last handler is the first to get the response message.</span></span>
