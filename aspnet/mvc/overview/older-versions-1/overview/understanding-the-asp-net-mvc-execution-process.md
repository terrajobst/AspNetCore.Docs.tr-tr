---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: ASP.NET MVC yürütme işlemi anlama | Microsoft Docs
author: microsoft
description: ASP.NET MVC çerçevesi adım adım bir tarayıcı isteğini nasıl işlediği hakkında bilgi edinin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564525"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="123fa-103">ASP.NET MVC yürütme işlemi anlama</span><span class="sxs-lookup"><span data-stu-id="123fa-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="123fa-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="123fa-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="123fa-105">ASP.NET MVC çerçevesi adım adım bir tarayıcı isteğini nasıl işlediği hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="123fa-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="123fa-106">Bir ASP.NET MVC tabanlı Web uygulama isteklerini ilk geçirmek aracılığıyla **UrlRoutingModule** bir HTTP modülü nesne.</span><span class="sxs-lookup"><span data-stu-id="123fa-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="123fa-107">Bu modül isteği ayrıştırır ve rota seçimi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="123fa-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="123fa-108">**UrlRoutingModule** nesne geçerli istek eşleşen ilk rota nesneyi seçer.</span><span class="sxs-lookup"><span data-stu-id="123fa-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="123fa-109">(Uygulayan bir sınıf bir rota nesnesidir **RouteBase**, ve genellikle bir örneğini **rota** sınıfı.) Yol yok eşleşiyorsa **UrlRoutingModule** nesne hiçbir şey yapmaz ve normal ASP.NET veya IIS istek işleme geri isteği olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="123fa-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="123fa-110">Seçilen **rota** nesnesi **UrlRoutingModule** nesnesi alır **IRouteHandler** ile ilişkili nesne **rota**nesnesi.</span><span class="sxs-lookup"><span data-stu-id="123fa-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="123fa-111">Genellikle, bir MVC uygulamasında bu örneği olacaktır **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="123fa-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="123fa-112">**IRouteHandler** örneği oluşturur bir **IHttpHandler** nesne ve geçirir **IHttpContext** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="123fa-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="123fa-113">Varsayılan olarak, **IHttpHandler** MVC için örnek **MvcHandler** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="123fa-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="123fa-114">**MvcHandler** nesne sonra sonuçta isteğini işleyecek denetleyiciyi seçer.</span><span class="sxs-lookup"><span data-stu-id="123fa-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="123fa-115">IIS 7. 0 ' bir ASP.NET MVC Web uygulaması çalıştığında, MVC projeleri için hiçbir dosya adı uzantısı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="123fa-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="123fa-116">Ancak, IIS 6. 0'da, ASP.NET ISAPI DLL .mvc dosya adı uzantısını eşlemek işleyici gerektirir.</span><span class="sxs-lookup"><span data-stu-id="123fa-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="123fa-117">Modül ve işleyici girişi ASP.NET MVC çerçevesi için noktalarıdır.</span><span class="sxs-lookup"><span data-stu-id="123fa-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="123fa-118">Bunlar, aşağıdaki eylemleri gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="123fa-118">They perform the following actions:</span></span>

- <span data-ttu-id="123fa-119">Uygun denetleyicisi bir MVC Web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="123fa-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="123fa-120">Belirli bir denetleyici örneği edinin.</span><span class="sxs-lookup"><span data-stu-id="123fa-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="123fa-121">Denetleyicinin çağrısı **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="123fa-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="123fa-122">Yürütme aşamaları bir MVC Web projesi için listeler:</span><span class="sxs-lookup"><span data-stu-id="123fa-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="123fa-123">Uygulama için ilk isteği alma</span><span class="sxs-lookup"><span data-stu-id="123fa-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="123fa-124">Global.asax dosyasındaki **rota** nesneleri eklenir **RouteTable** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="123fa-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="123fa-125">Yönlendirme gerçekleştirin</span><span class="sxs-lookup"><span data-stu-id="123fa-125">Perform routing</span></span> 

    - <span data-ttu-id="123fa-126">**UrlRoutingModule** modülünü kullanan ilk eşleşen **rota** nesnesinde **RouteTable** oluşturmak için koleksiyon **RouteData** ardından oluşturmak için kullandığı nesne bir **RequestContext** (**IHttpContext**) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="123fa-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="123fa-127">MVC istek işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="123fa-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="123fa-128">**MvcRouteHandler** nesnesinin bir örneğini oluşturur **MvcHandler** sınıfı ve geçirir **RequestContext** örneği.</span><span class="sxs-lookup"><span data-stu-id="123fa-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="123fa-129">Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="123fa-129">Create controller</span></span> 

    - <span data-ttu-id="123fa-130">**MvcHandler** nesne kullanır **RequestContext** belirlemek için örnek **IControllerFactory** nesne (genellikle bir örneği  **DefaultControllerFactory** sınıfı) ile denetleyici örneği oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="123fa-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="123fa-131">Execute denetleyicisi - **MvcHandler** örneği çağırır s denetleyicisi **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="123fa-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="123fa-132">Eylem çağırma</span><span class="sxs-lookup"><span data-stu-id="123fa-132">Invoke action</span></span> 

    - <span data-ttu-id="123fa-133">Çoğu denetleyicileri devralınmalıdır **denetleyicisi** temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="123fa-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="123fa-134">Bunu yapmak denetleyicileri için **ControllerActionInvoker** Denetleyici ile ilişkili olan nesne hangi eylemini yöntemi çağırmak için denetleyici sınıfının belirler ve bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="123fa-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="123fa-135">Sonuç yürütme</span><span class="sxs-lookup"><span data-stu-id="123fa-135">Execute result</span></span> 

    - <span data-ttu-id="123fa-136">Tipik eylem yöntemi, kullanıcı girişi almak, uygun yanıtı verileri hazırlamak ve sonucu bir sonuç türü döndürerek sonra yürütün.</span><span class="sxs-lookup"><span data-stu-id="123fa-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="123fa-137">Yürütülebilir yerleşik sonuç türleri şunlardır: **ViewResult** (hangi görünümü işleyen ve en sık kullanılan sonuç türü değil), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, ve **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="123fa-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
