---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: ASP.NET MVC yürütme işlemini anlama | Microsoft Docs
author: microsoft
description: ASP.NET MVC çerçevesi bir tarayıcı isteğini adım adım nasıl işlediğini öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: d94fcedb6e0ec6134bdbd69729abf1f875fac420
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378277"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="d26e4-103">ASP.NET MVC yürütme işlemini anlama</span><span class="sxs-lookup"><span data-stu-id="d26e4-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="d26e4-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d26e4-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d26e4-105">ASP.NET MVC çerçevesi bir tarayıcı isteğini adım adım nasıl işlediğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d26e4-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="d26e4-106">Bir ASP.NET MVC tabanlı Web uygulama isteklerini ilk geçirmek aracılığıyla **UrlRoutingModule** HTTP modülü nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="d26e4-107">Bu modül, istek ayrıştırır ve yol seçimi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="d26e4-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="d26e4-108">**UrlRoutingModule** nesnesini geçerli istek eşleşen ilk yönlendirme nesne seçer.</span><span class="sxs-lookup"><span data-stu-id="d26e4-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="d26e4-109">(Uygulayan bir sınıf rota nesnedir **RouteBase**, ve genellikle bir örneğini **rota** sınıfı.) Yol yok eşleşiyorsa **UrlRoutingModule** nesne hiçbir şey yapmaz ve normal ASP.NET veya IIS istek işleme dönmesi istek sağlar.</span><span class="sxs-lookup"><span data-stu-id="d26e4-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="d26e4-110">Seçili **rota** nesnesi **UrlRoutingModule** nesnesi alır **IRouteHandler** ile ilişkili nesne **rota**nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="d26e4-111">Genellikle, bir MVC uygulamasında, bu örneği olacaktır **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="d26e4-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="d26e4-112">**IRouteHandler** örneği oluşturan bir **IHttpHandler** geçirir ve nesne **IHttpContext** nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="d26e4-113">Varsayılan olarak, **IHttpHandler** MVC için örnek **MvcHandler** nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="d26e4-114">**MvcHandler** nesne sonra sonuçta isteğini işleyecek denetleyiciyi seçer.</span><span class="sxs-lookup"><span data-stu-id="d26e4-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="d26e4-115">Bir ASP.NET MVC Web uygulaması, IIS 7. 0'çalıştığında, MVC projeleri için hiçbir dosya adı uzantısı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d26e4-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="d26e4-116">Ancak, IIS 6. 0'da, işleyici .mvc dosya adı uzantısı için ASP.NET ISAPI DLL eşlemenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d26e4-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="d26e4-117">ASP.NET MVC çerçevesi için giriş noktaları modülü ve işleyici var.</span><span class="sxs-lookup"><span data-stu-id="d26e4-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="d26e4-118">Bunlar, aşağıdaki eylemleri gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="d26e4-118">They perform the following actions:</span></span>

- <span data-ttu-id="d26e4-119">Bir MVC Web uygulamasında uygun denetleyiciyi seçin.</span><span class="sxs-lookup"><span data-stu-id="d26e4-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="d26e4-120">Belirli bir denetleyici örneği alın.</span><span class="sxs-lookup"><span data-stu-id="d26e4-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="d26e4-121">Denetleyicinin çağrı **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d26e4-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="d26e4-122">Bir MVC Web projesi için yürütme aşamalarını listeler:</span><span class="sxs-lookup"><span data-stu-id="d26e4-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="d26e4-123">Uygulama için ilk isteği alma</span><span class="sxs-lookup"><span data-stu-id="d26e4-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="d26e4-124">Global.asax dosyasındaki **rota** nesneleri eklenir **RouteTable** nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="d26e4-125">Yönlendirme gerçekleştirin</span><span class="sxs-lookup"><span data-stu-id="d26e4-125">Perform routing</span></span> 

    - <span data-ttu-id="d26e4-126">**UrlRoutingModule** modülü kullanır uyan ilk **rota** nesnesine **RouteTable** oluşturmak için koleksiyon **RouteData** ardından oluşturmak için kullandığı bir nesne bir **RequestContext** (**IHttpContext**) nesne.</span><span class="sxs-lookup"><span data-stu-id="d26e4-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="d26e4-127">MVC istek işleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d26e4-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="d26e4-128">**MvcRouteHandler** nesnesi örneği oluşturan **MvcHandler** sınıfı ve geçirir **RequestContext** örneği.</span><span class="sxs-lookup"><span data-stu-id="d26e4-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="d26e4-129">Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="d26e4-129">Create controller</span></span> 

    - <span data-ttu-id="d26e4-130">**MvcHandler** nesne kullandığı **RequestContext** tanımlamak için örnek **IControllerFactory** nesne (genellikle bir örneğini  **DefaultControllerFactory** sınıf) ile denetleyici örneği oluşturulamadı.</span><span class="sxs-lookup"><span data-stu-id="d26e4-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="d26e4-131">Yürütme denetleyicisi - **MvcHandler** örneği çağrıları s denetleyicisi **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d26e4-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="d26e4-132">Eylemini çağır</span><span class="sxs-lookup"><span data-stu-id="d26e4-132">Invoke action</span></span> 

    - <span data-ttu-id="d26e4-133">Çoğu denetleyicileri devralınacak **denetleyicisi** temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d26e4-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="d26e4-134">Bunu yapmak denetleyicileri için **ControllerActionInvoker** denetleyiciyle ilişkili nesne hangi eylem yöntemini çağırmak için denetleyici sınıfının belirler ve ardından bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="d26e4-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="d26e4-135">Yürütme sonucu</span><span class="sxs-lookup"><span data-stu-id="d26e4-135">Execute result</span></span> 

    - <span data-ttu-id="d26e4-136">Tipik bir eylem yöntemi, kullanıcı girişi almak, uygun yanıt verileri hazırlama ve sonucu bir sonuç türü döndürerek yürütebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d26e4-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="d26e4-137">Yürütülebilir yerleşik sonuç türleri şunlardır: **ViewResult** (görünümü işleyen ve en sık kullanılan sonuç türü olan), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, ve **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="d26e4-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
