---
title: "Geçirme HTTP işleyicileri ve ASP.NET Core ara yazılım modülleri"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: f104c9116cfaa4a82ac88e4a83b4b6f172eb2aa1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="c9bd0-102">Geçirme HTTP işleyicileri ve ASP.NET Core ara yazılım modülleri</span><span class="sxs-lookup"><span data-stu-id="c9bd0-102">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="c9bd0-103">Tarafından [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="c9bd0-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="c9bd0-104">Bu makalede mevcut ASP.NET geçirmek nasıl gösterilmektedir [HTTP modülleri ve system.webserver işleyicilerini](https://docs.microsoft.com/iis/configuration/system.webserver/) ASP.NET Core için [Ara](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="c9bd0-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="c9bd0-105">Modüller ve tekrar ziyaret işleyicileri</span><span class="sxs-lookup"><span data-stu-id="c9bd0-105">Modules and handlers revisited</span></span>

<span data-ttu-id="c9bd0-106">ASP.NET Core ara yazılımı için devam etmeden önce şimdi ilk HTTP modülleri ve işleyicileri nasıl çalıştığını olduðunu:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Modül işleyicisi](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="c9bd0-108">**İşleyicileri şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-108">**Handlers are:**</span></span>

   * <span data-ttu-id="c9bd0-109">Sınıfları uygulayan [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="c9bd0-109">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="c9bd0-110">Gibi belirli dosya adı veya uzantısı ile isteklerini işlemek için kullanılan *.report*</span><span class="sxs-lookup"><span data-stu-id="c9bd0-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="c9bd0-111">[Yapılandırılmış](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) içinde *Web.config*</span><span class="sxs-lookup"><span data-stu-id="c9bd0-111">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="c9bd0-112">**Modülleri şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-112">**Modules are:**</span></span>

   * <span data-ttu-id="c9bd0-113">Sınıfları uygulayan [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="c9bd0-113">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="c9bd0-114">Her istek için çağrılır</span><span class="sxs-lookup"><span data-stu-id="c9bd0-114">Invoked for every request</span></span>

   * <span data-ttu-id="c9bd0-115">Kısa devre oluşturur (bir isteğin diğer işlemleri durdurmak) için</span><span class="sxs-lookup"><span data-stu-id="c9bd0-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="c9bd0-116">HTTP yanıtı eklemek veya kendi oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="c9bd0-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="c9bd0-117">[Yapılandırılmış](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) içinde *Web.config*</span><span class="sxs-lookup"><span data-stu-id="c9bd0-117">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="c9bd0-118">**Modülleri gelen istekleri işleme sırası tarafından belirlenir:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="c9bd0-119">[Uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx), ASP.NET tarafından tetiklenen serisi olayları olduğu: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), vb. Her modül işleyicisi için bir veya daha fazla olay oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="c9bd0-120">Aynı olay, bunlar yapılandırılmış sipariş için *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="c9bd0-121">Modülleri yanı sıra, yaşam döngüsü olayları için işleyiciler ekleyebilirsiniz, *Global.asax.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="c9bd0-122">Bu işleyiciler, yapılandırılmış modüllerdeki işleyicileri sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="c9bd0-123">İşleyicileri ve ara yazılım modülleri</span><span class="sxs-lookup"><span data-stu-id="c9bd0-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="c9bd0-124">**Ara yazılım HTTP modülleri ve işleyicileri basit şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="c9bd0-125">Modüller, işleyiciler *Global.asax.cs*, *Web.config* (dışında IIS yapılandırmasının) ve uygulama yaşam döngüsü kayboldu</span><span class="sxs-lookup"><span data-stu-id="c9bd0-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="c9bd0-126">Modüller ve işleyicileri rollerini ara yazılım tarafından ele alınmış</span><span class="sxs-lookup"><span data-stu-id="c9bd0-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="c9bd0-127">Ara yazılım, kod kullanarak yapılandırılır yerine içinde *Web.config*</span><span class="sxs-lookup"><span data-stu-id="c9bd0-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="c9bd0-128">[Ardışık Düzen dallanma](../fundamentals/middleware.md#middleware-run-map-use) yalnızca URL'yi de istek üstbilgileri, sorgu dizeleri, vb. göre belirli ara yazılımı için istekleri gönderdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-128">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="c9bd0-129">**Ara yazılım modülleri için çok benzer:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="c9bd0-130">Her istek için ilkesine çağrılır</span><span class="sxs-lookup"><span data-stu-id="c9bd0-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="c9bd0-131">Bir istek tarafından kısa devre oluşturur yapabileceksiniz [isteği için bir sonraki ara yazılım geçirme değil](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="c9bd0-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="c9bd0-132">Kendi HTTP yanıtı oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="c9bd0-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="c9bd0-133">**Ara yazılım ve modülleri farklı bir sırada işlenir:**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="c9bd0-134">Ara yazılım sırasını içinde eklenir istek ardışık düzenine modülleri sırasını çoğunlukla tabanlı çalıştırırken sipariş temel [uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx) olayları</span><span class="sxs-lookup"><span data-stu-id="c9bd0-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="c9bd0-135">Modülleri sırasını istekleri ve yanıtları için aynı olsa da Ara yazılım yanıtlar için istekleri, ters sırasıdır</span><span class="sxs-lookup"><span data-stu-id="c9bd0-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="c9bd0-136">Bkz: [IApplicationBuilder ile Ara yazılım ardışık düzenini oluşturma](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="c9bd0-136">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Ara yazılım](http-modules/_static/middleware.png)

<span data-ttu-id="c9bd0-138">Yukarıdaki resimde kimlik doğrulaması ara yazılım istek nasıl short-circuited unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="c9bd0-139">Ara yazılım Geçirme Modülü kodu</span><span class="sxs-lookup"><span data-stu-id="c9bd0-139">Migrating module code to middleware</span></span>

<span data-ttu-id="c9bd0-140">Var olan bir HTTP Modül aşağıdakine benzer görünecektir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="c9bd0-141">Gösterildiği gibi [ara yazılımı](../fundamentals/middleware.md) sayfasında, bir ASP.NET Core Ara yazılımıdır gösteren bir sınıf bir `Invoke` yöntemi alma bir `HttpContext` ve döndüren bir `Task`.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-141">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="c9bd0-142">Yeni Ara yazılımınızı şuna benzeyecektir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="c9bd0-143">Yukarıdaki ara yazılım şablonu bölümünden alındığı [ara yazılımı yazma](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="c9bd0-143">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="c9bd0-144">*MyMiddlewareExtensions* yardımcı sınıfı, Ara yapılandırmak kolaylaştırır, `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="c9bd0-145">`UseMyMiddleware` Yöntemi ara yazılım sınıfınız istek ardışık düzenine ekler.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="c9bd0-146">Ara yazılım tarafından gerekli hizmetleri Ara oluşturucuda ucunuz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="c9bd0-147">Örneğin kullanıcının yetkili olmayan bir istek modülünüzün sonlandırabilir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="c9bd0-148">Bir ara yazılım değil çağırarak bu işleme `Invoke` ardışık düzende sonraki ara yazılım üzerinde.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="c9bd0-149">Ardışık düzeni üzerinden geri yolu yanıt yaptığında, önceki middlewares hala çağrılacak olduğundan bu tam istek sonlandırmak değil olduğunu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="c9bd0-150">Yeni, Ara, modülün işlevselliği geçirdiğinizde, kodunuzu çünkü derleyin değil bulabilirsiniz `HttpContext` sınıfı ASP.NET Core önemli ölçüde değişti.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c9bd0-151">[Daha sonra](#migrating-to-the-new-httpcontext), yeni ASP.NET Core HttpContext geçirmek nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="c9bd0-152">İstek ardışık düzenini geçirme modülü ekleme</span><span class="sxs-lookup"><span data-stu-id="c9bd0-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="c9bd0-153">HTTP modülleri, istek ardışık düzen kullanarak genellikle eklenir *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="c9bd0-154">Bu dönüştürme [, yeni Ara ekleme](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) için istek ardışık düzeninde, `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-154">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="c9bd0-155">Bir modül olarak işlenen olay Ekle burada yeni Ara yazılımınızı ardışık düzeninde tam nokta bağlıdır (`BeginRequest`, `EndRequest`, vs.) ve kendi sıra modülleri, listesinde *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="c9bd0-156">Daha önce belirtildiği gibi ASP.NET Core hiçbir uygulama yaşam döngüsü yoktur ve modülleri tarafından kullanılan sırayı yanıtları ara yazılım tarafından işlenir sipariş farklıdır.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="c9bd0-157">Bu sipariş Kararınızı daha zor hale getirebilir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="c9bd0-158">Sıralama bir sorun olursa, bağımsız olarak sıralanabilir birden fazla ara yazılım bileşenlerine modülünüzün bölebilir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="c9bd0-159">Ara yazılım geçirme işleyici kodu</span><span class="sxs-lookup"><span data-stu-id="c9bd0-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="c9bd0-160">Bir HTTP işleyicisini şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="c9bd0-161">ASP.NET Core projenizde, bu şuna benzer bir ara yazılımı için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="c9bd0-162">Bu ara yazılım modülleri için karşılık gelen ara çok benzer.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="c9bd0-163">Yalnızca gerçek fark burada hiçbir çağrısı yok `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="c9bd0-164">Olur; böylece işleyici çağırmak için hiçbir sonraki ara yazılım istek ardışık düzen sonunda olduğundan, mantıklıdır.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="c9bd0-165">İstek ardışık düzenini geçirme işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="c9bd0-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="c9bd0-166">Bir HTTP işleyicisini yapılandırma yapılır *Web.config* ve şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="c9bd0-167">Bu istek ardışık düzeninde, yeni işleyici Ara ekleyerek dönüştüremedi, `Startup` sınıfı, ara yazılımı modüllerden dönüştürülen benzer.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="c9bd0-168">Bu yaklaşım, tüm istekleri için yeni bir işleyici Ara yazılımınızı gönderebilir sorundur.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="c9bd0-169">Ancak, yalnızca, Ara ulaşması isteklerini belirli bir uzantıya sahip ister.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="c9bd0-170">HTTP işleyicisi ile sahip olduğunuz aynı işlevselliği verirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="c9bd0-171">Bir çözümdür isteklerini belirli bir uzantıya sahip işlem hattı oluşturmak için kullanarak `MapWhen` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="c9bd0-172">Aynı bunu `Configure` diğer ara yazılımdan eklediğiniz yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="c9bd0-173">`MapWhen`Bu parametreleri alır:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="c9bd0-174">Geçen lambda `HttpContext` ve döndürür `true` istek şube görünmeliyse.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="c9bd0-175">Bu, yalnızca kendi uzantısı, aynı zamanda istek üstbilgileri, sorgu dizesi parametreleri vb. bağlı olarak istekleri dal anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="c9bd0-176">Geçen lambda bir `IApplicationBuilder` ve dal için tüm ara yazılımı ekler.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="c9bd0-177">Başka bir deyişle, ek ara yazılım, işleyici Ara önünde dala ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="c9bd0-178">Dal tüm isteklerinde çağrılacak önce ara yazılım ardışık düzenine eklenen; dal, bunlar üzerinde hiçbir etkisi sahip olur.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="c9bd0-179">Ara yazılım seçenekleri seçenekleri desenini kullanarak yükleme</span><span class="sxs-lookup"><span data-stu-id="c9bd0-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="c9bd0-180">Bazı modüller ve işleyicileri depolanmış yapılandırma seçeneğiniz *Web.config*. Ancak, ASP.NET Core yeni bir yapılandırma modeli yerine kullanılan *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="c9bd0-181">Yeni [yapılandırma sistemi](xref:fundamentals/configuration/index) bunu çözmek için bu seçeneği sunar:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="c9bd0-182">Ara yazılım seçeneklere gösterildiği gibi doğrudan Ekle [sonraki bölümde](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="c9bd0-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="c9bd0-183">Kullanım [seçenekleri düzeni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="c9bd0-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="c9bd0-184">Örneğin, ara yazılım seçenekleri tutmak için bir sınıf oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-184">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="c9bd0-185">Seçenek değerleri depolama</span><span class="sxs-lookup"><span data-stu-id="c9bd0-185">Store the option values</span></span>

    <span data-ttu-id="c9bd0-186">Yapılandırma sistemi seçenek değerleri istediğiniz herhangi bir yere depolamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="c9bd0-187">Ancak, en siteleri kullanım *appsettings.json*, biz bu yaklaşımı benimsemeye:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="c9bd0-188">*MyMiddlewareOptionsSection* bir bölüm adı aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="c9bd0-189">Seçenek sınıfı adı ile aynı olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="c9bd0-190">Seçenek değerleri seçenekleri sınıfıyla ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="c9bd0-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="c9bd0-191">Seçenekleri türü ilişkilendirmek için ASP.NET Core'nın bağımlılık ekleme framework seçenekleri desen kullanır (gibi `MyMiddlewareOptions`) ile bir `MyMiddlewareOptions` , gerçek seçenekleri sahip bir nesne.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="c9bd0-192">Güncelleştirme, `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-192">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="c9bd0-193">Kullanıyorsanız, *appsettings.json*, yapılandırma Oluşturucusu'nda eklemek `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="c9bd0-194">Seçenekler hizmetini yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-194">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="c9bd0-195">Seçeneklerinizi seçenekleri sınıfınız ile ilişkilendirin:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-195">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="c9bd0-196">Ara yazılım Oluşturucu seçeneklere yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="c9bd0-197">Bu, bir denetleyici seçeneklere injecting için benzer.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-197">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="c9bd0-198">[UseMiddleware](#http-modules-usemiddleware) , ara yazılımı ekler genişletme yöntemi `IApplicationBuilder` mvc'deki bağımlılık ekleme.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="c9bd0-199">Bu sınırlı değildir `IOptions` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="c9bd0-200">Ara yazılımınızı gerektiren herhangi bir nesne bu şekilde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="c9bd0-201">Ara yazılım seçenekleri doğrudan ekleme işlemi aracılığıyla yükleniyor</span><span class="sxs-lookup"><span data-stu-id="c9bd0-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="c9bd0-202">Seçenekler düzeni seçenekleri değerler ve bunların tüketicileri arasında Kuplaj gevşek oluşturur avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="c9bd0-203">Bir seçenek sınıfı gerçek seçenekleri değerlerle ilişkilendirdiğiniz sonra başka bir sınıf seçeneklerine bağımlılık ekleme çerçevesi aracılığıyla erişim elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="c9bd0-204">Geçici bir çözüm seçenekleri değerleri geçirmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="c9bd0-205">Farklı seçeneklerle aynı ara yazılım, iki kez kullanmak istiyorsanız, bunu ancak böler.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="c9bd0-206">Farklı roller sağlayan farklı dallarda kullanılan örneğin bir yetkilendirme ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="c9bd0-207">İki farklı seçenekler nesneleri bir seçenek sınıfı ile ilişkilendiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="c9bd0-208">Çözüm seçenekleri nesneleri gerçek seçenekleri değerleri elde etmektir, `Startup` sınıfı ve bu, Ara her örneği için doğrudan geçirin.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="c9bd0-209">İkinci anahtar eklemek *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="c9bd0-209">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="c9bd0-210">İkinci bir set seçenekleri eklemek için *appsettings.json* dosya, yeni bir anahtar benzersiz şekilde tanımlamak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="c9bd0-211">Seçenek değerleri almak ve bunları ara geçirin.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="c9bd0-212">`Use...` , Ara yazılım ardışık düzene ekler) genişletme yöntemi (yerdir seçeneği değerleri geçirmek için bir mantıksal:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="c9bd0-213">Seçenekler parametresi yapılacak Ara yazılımlarını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="c9bd0-214">Bir aşırı yüklemesini sağlayan `Use...` genişletme yöntemi (Seçenekler parametresi alır ve buna ileten `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="c9bd0-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="c9bd0-215">Zaman `UseMiddleware` çağrılır Ara nesne başlattığında parametrelerle, parametreleri, ara yazılım Oluşturucu geçirir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="c9bd0-216">Bu seçenekler nesnesinde nasıl sarmalar Not bir `OptionsWrapper` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="c9bd0-217">Bu uygulayan `IOptions`, ara yazılım Oluşturucu tarafından beklendiği gibi.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="c9bd0-218">Yeni HttpContext geçirme</span><span class="sxs-lookup"><span data-stu-id="c9bd0-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="c9bd0-219">Daha önce gördüğünüzle, `Invoke` , Ara yönteminde türünde bir parametre alan `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="c9bd0-220">`HttpContext`ASP.NET Core önemli ölçüde değişti.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c9bd0-221">Bu bölümde, en sık kullanılan özelliklerini çevirmek gösterilmiştir [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) yeni `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="c9bd0-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="c9bd0-222">HttpContext</span></span>

<span data-ttu-id="c9bd0-223">**HttpContext.Items** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="c9bd0-224">**Benzersiz istek kimliği (System.Web.HttpContext karşılık gelen)**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="c9bd0-225">Her istek için benzersiz bir kimlik verir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="c9bd0-226">Günlüklerinize dahil etmek çok kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-226">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="c9bd0-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="c9bd0-227">HttpContext.Request</span></span>

<span data-ttu-id="c9bd0-228">**HttpContext.Request.HttpMethod** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="c9bd0-229">**HttpContext.Request.QueryString** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="c9bd0-230">**HttpContext.Request.Url** ve **HttpContext.Request.RawUrl** için çevir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="c9bd0-231">**HttpContext.Request.IsSecureConnection** translates to:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="c9bd0-232">**HttpContext.Request.UserHostAddress** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="c9bd0-233">**HttpContext.Request.Cookies** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="c9bd0-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="c9bd0-235">**HttpContext.Request.Headers** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="c9bd0-236">**HttpContext.Request.UserAgent** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="c9bd0-237">**HttpContext.Request.UrlReferrer** translates to:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="c9bd0-238">**HttpContext.Request.ContentType** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="c9bd0-239">**HttpContext.Request.Form** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="c9bd0-240">İçerik alt türü yalnızca ise form değerleri Okuma *x-www-form-urlencoded* veya *form verileri*.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="c9bd0-241">**HttpContext.Request.InputStream** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="c9bd0-242">Bu kod yalnızca bir ardışık düzen sonunda bir işleyici türü Ara kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="c9bd0-243">İstek başına yalnızca bir kez yukarıda gösterildiği gibi ham gövde okuyabilir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="c9bd0-244">İlk okuma boş bir gövdeyi okuyacaksa sonra gövdesi okumaya çalışırken bir ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="c9bd0-245">Bu, bir arabelleğinden yapıldığından form daha önce gösterildiği gibi okuma için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="c9bd0-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="c9bd0-246">HttpContext.Response</span></span>

<span data-ttu-id="c9bd0-247">**HttpContext.Response.Status** ve **HttpContext.Response.StatusDescription** için çevir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="c9bd0-248">**HttpContext.Response.ContentEncoding** ve **HttpContext.Response.ContentType** için çevir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="c9bd0-249">**HttpContext.Response.ContentType** üzerinde kendi ayrıca için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="c9bd0-250">**HttpContext.Response.Output** için çevirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="c9bd0-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="c9bd0-252">Bir dosyayı yedeklemeniz hizmet veren ele alınmıştır [burada](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="c9bd0-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="c9bd0-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="c9bd0-254">Yanıt Üstbilgileri gönderme gerçeğiyle karmaşık her şeyi yanıt gövdesi yazıldıktan sonra ayarlarsanız, bunlar değil gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="c9bd0-255">Sağ yanıt başlatır yazmadan önce çağrılacak bir geri çağırma yöntemini ayarlamak için kullanılan çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="c9bd0-256">Bu en iyi başlangıcında gerçekleştirilir `Invoke` Ara yazılımınızı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="c9bd0-257">Yanıt Üstbilgileri ayarlar bu geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="c9bd0-258">Aşağıdaki kod adlı bir geri çağırma yöntemi ayarlar `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c9bd0-259">`SetHeaders` Geri çağırma yöntemi şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="c9bd0-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c9bd0-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="c9bd0-261">Tanımlama bilgilerini seyahat tarayıcıya bir *Set-Cookie* yanıtı üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="c9bd0-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="c9bd0-262">Sonuç olarak, tanımlama bilgileri gönderme kullanılanlarla aynı geri çağırma yanıt üstbilgileri göndermek için gerektirir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c9bd0-263">`SetCookies` Geri çağırma yöntemi uygulamamız şu şekilde görünecektir:</span><span class="sxs-lookup"><span data-stu-id="c9bd0-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="c9bd0-264">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c9bd0-264">Additional resources</span></span>

* [<span data-ttu-id="c9bd0-265">HTTP işleyicileri ve HTTP modülleri genel bakış</span><span class="sxs-lookup"><span data-stu-id="c9bd0-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="c9bd0-266">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c9bd0-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="c9bd0-267">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="c9bd0-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="c9bd0-268">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="c9bd0-268">Middleware</span></span>](xref:fundamentals/middleware)
